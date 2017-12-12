---
title: "Ukládání odpovědí do mezipaměti v ASP.NET Core"
author: rick-anderson
description: "Další informace o použití odpověď do mezipaměti pro nižší nároky na šířku pásma a zvýšit výkon aplikací ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 104cfb2eab706a2ec6278b4d1c461f70b0af5df1
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="response-caching-in-aspnet-core"></a>Ukládání odpovědí do mezipaměti v ASP.NET Core

Podle [Jan Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), a [Luke Latham](https://github.com/guardrex)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukládání odpovědí do mezipaměti snižuje počet požadavků, které provede klient nebo server proxy webový server. Ukládání odpovědí do mezipaměti také snižuje množství práce provádí webového serveru pro generování odpovědi. Ukládání odpovědí do mezipaměti řídí hlavičky, které určují, jakým způsobem chcete klienta, proxy server a middleware do mezipaměti odpovědi.

Webový server může ukládat do mezipaměti odpovědi při přidání [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Ukládání do mezipaměti založené na protokolu HTTP odpovědi

[Ukládání do mezipaměti HTTP 1.1 specifikace](https://tools.ietf.org/html/rfc7234) popisuje chování mezipaměti Internetu. Primární záhlaví HTTP, který se používá pro ukládání do mezipaměti je [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), který slouží k určení mezipaměti *direktivy*. Direktivy řídí chování ukládání do mezipaměti jako požadavky zajistěte jejich představ od klientů na servery, a jako odpověď zajistěte jejich představ ze serverů zpět klientům. Požadavky a odpovědi pohyb proxy servery a proxy servery musí odpovídat také specifikace ukládání do mezipaměti HTTP 1.1.

Běžné `Cache-Control` direktivy jsou uvedeny v následující tabulce.

| – Direktiva                                                       | Akce |
| --------------------------------------------------------------- | ------ |
| [veřejné](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Mezipaměť může ukládat odpověď. |
| [privátní](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Odpověď nesmí být uloženy ve sdílené mezipaměti. Privátní mezipaměti může uložit a opakovaně používat odpovědi. |
| [Maximální stáří](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Klient nebude přijímat odpovědi, jejichž stáří je větší než zadaný počet sekund. Příklady: `max-age=60` (60 sekund), `max-age=2592000` (1 měsíc) |
| [Ne mezipaměti](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **U požadavků**: mezipaměti nesmí používat uložené odpovědi ke zpracování požadavku. Poznámka: Na zdrojový server znovu generuje odpovědi pro klienta a middleware aktualizuje odpověď uložená v mezipaměti.<br><br>**V odpovědi**: odpověď nesmí se používat pro následné žádosti bez ověřování na původním serveru. |
| [Ne – úložiště](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **U požadavků**: žádost nesmí uložena mezipaměti.<br><br>**V odpovědi**: mezipaměti nesmí ukládat libovolná součást odpovědi. |

V následující tabulce jsou uvedeny další mezipaměti hlavičky, které hrají roli při ukládání do mezipaměti.

| Záhlaví                                                     | Funkce |
| ---------------------------------------------------------- | -------- |
| [Stáří](https://tools.ietf.org/html/rfc7234#section-5.1)     | Odhad množství času v sekundách, protože odpověď byla vygenerována nebo úspěšně ověřen na původním serveru. |
| [Vypršení platnosti](https://tools.ietf.org/html/rfc7234#section-5.3) | Datum a čas, po jejímž uplynutí je považován za odpověď zastaralých. |
| [Direktiva pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Pro zpětnou kompatibilitu s HTTP/1.0 ukládá do mezipaměti pro nastavení existuje `no-cache` chování. Pokud `Cache-Control` záhlaví nachází, `Pragma` záhlaví je ignorována. |
| [Lišit](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Určuje, že odpovědi v mezipaměti nesmí být odeslána, pokud všechny služby `Vary` záhlaví pole shodují v původní žádost odpověď uložená v mezipaměti a nový požadavek. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Ukládání do mezipaměti ohledech založené na protokolu HTTP žádosti direktivy Cache-Control

[Ukládání do mezipaměti HTTP 1.1 specifikace pro hlavičku Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) vyžaduje mezipaměť pro respektovat platná `Cache-Control` hlavička odeslaná klientem. Klienta můžete provádět požadavky s `no-cache` hodnota hlavičky a force serveru pro generování novou odpověď pro každý požadavek.

Vždy ctít zásady klienta `Cache-Control` hlavičky požadavku dává smysl, pokud uvažujete o cílem HTTP ukládání do mezipaměti. Podle specifikace oficiální ukládání do mezipaměti slouží ke snížení režie latenci a sítě uspokojení požadavků v síti klientů, proxy servery a servery. Není to nutně způsob, jak řídit zatížení na původním serveru.

Neexistuje žádný aktuální vývojáři řídit toto chování ukládání do mezipaměti při použití [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware) protože middleware dodržuje oficiální specifikace ukládání do mezipaměti. [Budoucí vylepšení middleware](https://github.com/aspnet/ResponseCaching/issues/96) povolí přístup konfigurace middlewaru ignorovat požadavku `Cache-Control` záhlaví při rozhodování, která bude sloužit odpovědi v mezipaměti. To nabídne možnost lepší řízení zatížení na serveru při použití middleware.

## <a name="other-caching-technology-in-aspnet-core"></a>Další ukládání do mezipaměti technologie ASP.NET Core

### <a name="in-memory-caching"></a>Ukládání do mezipaměti v paměti

Ukládání do mezipaměti v paměti používá server paměti k ukládání data uložená v mezipaměti. Tento typ ukládání do mezipaměti je vhodný pro jeden nebo více serverů pomocí *trvalé relace*. Trvalé relace znamená, že žádosti z klienta jsou vždy směrovány na stejný server pro zpracování.

Další informace najdete v tématu [Úvod k ukládání do mezipaměti v paměti v ASP.NET Core](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Distribuované mezipaměti

Distribuované mezipaměti využívat k ukládání dat v paměti, když aplikace hostovaná v cloudu nebo server farmy. Mezipaměť je sdílet mezi servery, které zpracovávají požadavky. Klient může odeslat svoji žádost, kterou provádí služba jakýkoli server ve skupině a data uložená v mezipaměti klienta je k dispozici. ASP.NET Core nabízí systému SQL Server a Redis distribuované mezipaměti.

Další informace najdete v tématu [práce s distribuované mezipaměti](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Pomocník značky mezipaměti

Pomocník značky mezipaměti můžete mezipaměti obsahu ze zobrazení MVC nebo stránky Razor. Pomocník značky mezipaměti používá k ukládání dat ukládání do mezipaměti v paměti.

Další informace najdete v tématu [Pomocník značky mezipaměti ASP.NET MVC základní](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Pomocník značky distribuované mezipaměti

Do mezipaměti můžete ukládat obsah ze zobrazení MVC nebo stránky Razor v distribuované cloudu nebo webové farmy scénáře Pomocník značky distribuované mezipaměti. Pomocník distribuované mezipaměti značku používá k ukládání dat systému SQL Server nebo Redis.

Další informace najdete v tématu [distribuované mezipaměti značky pomocná](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Atribut ResponseCache

`ResponseCacheAttribute` Určuje parametry, které jsou nezbytné pro nastavení odpovídající hlavičky v ukládání odpovědí do mezipaměti. V tématu [ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) popis parametry.

> [!WARNING]
> Zakážete ukládání do mezipaměti pro obsah, který obsahuje informace pro klienty ověřené. Ukládání do mezipaměti by měla povoleno pouze pro obsah, který nemění na základě identity uživatele nebo zda je uživatel přihlášen.

`VaryByQueryKeys string[]`(vyžaduje ASP.NET Core 1.1 a novější): Pokud nastavíte, Middleware ukládání do mezipaměti odpovědi se liší uložené odpovědi podle hodnoty daný seznam klíče dotazu. Middleware ukládání do mezipaměti odpovědi musí být povoleno nastavení `VaryByQueryKeys` vlastnost; jinak, je vyvolána výjimka za běhu. Neexistuje žádné odpovídající záhlaví HTTP pro `VaryByQueryKeys` vlastnost. Tato vlastnost je funkce protokolu HTTP zpracovávaných Middlewarem ukládání do mezipaměti odpovědi. Pro middleware k obsluze odpovědi v mezipaměti řetězec dotazu a hodnotu řetězce dotazu musí odpovídat na předchozí požadavek. Představte si třeba pořadí požadavků a výsledky zobrazené v následující tabulce.

| Požadavek                          | Výsledek                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Vrácená serverem     |
| `http://example.com?key1=value1` | Vrácená z middlewaru. |
| `http://example.com?key1=value2` | Vrácená serverem     |

První požadavek je vrácená serverem a uložené v mezipaměti v middlewaru. Druhá žádost se vrátí middlewarem, protože řetězec dotazu odpovídá předchozí požadavek. Třetí žádost není v mezipaměti middleware, protože hodnotu řetězce dotazu se neshoduje se na předchozí požadavek. 

`ResponseCacheAttribute` Slouží ke konfiguraci a vytvoření (prostřednictvím `IFilterFactory`) `ResponseCacheFilter`. `ResponseCacheFilter` Provede práci aktualizace příslušné hlavičky protokolu HTTP a funkce odpovědi. Filtr:

* Odebere všechny existující hlavičky pro `Vary`, `Cache-Control`, a `Pragma`. 
* Zapíše se příslušné hlavičky na základě vlastností nastavit `ResponseCacheAttribute`. 
* Aktualizace odpověď do mezipaměti funkce protokolu HTTP, pokud `VaryByQueryKeys` nastavena.

### <a name="vary"></a>lišit

Tuto hlavičku je zapsat, pouze když `VaryByHeader` je nastavena. Je nastaven na hodnotu `Vary` hodnotu vlastnosti. Následující ukázkové používá `VaryByHeader` vlastnost:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Můžete zobrazit hlavičky odpovědi pomocí nástrojů v prohlížeči na síti. Následující obrázek ukazuje F12 Edge výstup na **sítě** při `About2` se aktualizují metodu akce:

![Při volání metody akce About2 okraj F12 výstup na kartě sítě](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore a Location.None

`NoStore`přepíše většina jiných vlastností. Pokud je tato vlastnost nastavená na `true`, `Cache-Control` záhlaví je nastaven na `no-store`. Pokud `Location` je nastaven na `None`:

* `Cache-Control`je nastavena na `no-store,no-cache`.
* `Pragma`je nastavena na `no-cache`.

Pokud `NoStore` je `false` a `Location` je `None`, `Cache-Control` a `Pragma` jsou nastaveny na `no-cache`.

Obvykle nastavíte `NoStore` k `true` na chybové stránky. Příklad:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Výsledkem je následující hlavičky:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Umístění a doba trvání

Chcete-li povolit ukládání do mezipaměti, `Duration` musí být nastavena na kladnou hodnotu a `Location` musí být buď `Any` (výchozí) nebo `Client`. V takovém případě `Cache-Control` záhlaví je nastaven na hodnotu umístění, za nímž následuje `max-age` odpovědi.

> [!NOTE]
> `Location`na možnosti `Any` a `Client` převede na `Cache-Control` hodnoty hlavičky `public` a `private`, v uvedeném pořadí. Jak je uvedeno dříve, nastavení `Location` k `None` nastaví obě `Cache-Control` a `Pragma` záhlaví `no-cache`.

Níže je příkladem zobrazujícím hlavičky produkovaný nastavení `Duration` a ponechat výchozí `Location` hodnotu:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Tímto se vytvoří následující hlavičky:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profily mezipaměti

Místo duplikování `ResponseCache` nastavení na mnoha atributů akce řadič, mezipaměť profily se dají konfigurovat jako možnosti při nastavování MVC v `ConfigureServices` metoda v `Startup`. Hodnoty zjištěné v profilu odkazované mezipaměti jsou použity jako výchozí pomocí `ResponseCache` atribut a jsou přepsaná podle libovolných vlastností zadaná pro atribut.

Nastavení profilu mezipaměti:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Odkazování na profil mezipaměti:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache` Atribut lze použít jak pro akce (metody) a řadiče (třídy). Atributy na úrovni metody přepsat nastavení zadané v atributy na úrovni třídy.

V předchozím příkladu atribut úrovni třídy určuje dobu trvání 30 sekund, zatímco metoda úrovni atribut odkazuje profil mezipaměti s určitou dobou trvání nastavena na 60 sekund.

Výsledný hlavičky:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Další zdroje

* [Ukládání do mezipaměti v protokolu HTTP z specifikace](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Ukládání do mezipaměti v paměti](xref:performance/caching/memory)
* [Práce s distribuované mezipaměti](xref:performance/caching/distributed)
* [Detekovat změny s tokeny změn](xref:fundamentals/primitives/change-tokens)
* [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware)
* [Pomocník značky mezipaměti](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocník značky distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
