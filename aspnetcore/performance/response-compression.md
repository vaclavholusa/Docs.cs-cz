---
title: "Middleware komprese odpovědi pro ASP.NET Core"
author: guardrex
description: "Další informace o odpovědi komprese a jak používat Middleware komprese odpovědi v aplikacích ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: d05256af4e62834b8d43689786a7b8bb3a5e58fb
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Middleware komprese odpovědi pro ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Šířka pásma sítě je omezená prostředků. Rychlost reakce aplikace, je obvykle zmenšení velikosti odpovědi často výrazně zvyšuje. Jedním ze způsobů ke zmenšení velikosti datové části je kompresi odpovědí aplikace.

## <a name="when-to-use-response-compression-middleware"></a>Kdy použít middlewaru odpovědi komprese.
Použijte technologie komprese odpovědi na serveru služby IIS, Apache nebo Nginx. Výkon middleware pravděpodobně nebude odpovídat serverových modulů. [Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) a [Kestrel](xref:fundamentals/servers/kestrel) aktuálně nenabízí podporu předdefinované komprese.

Middleware komprese odpovědi použijte, pokud jste:

* Nelze použít následující technologie komprese na serveru:
  * [Modul pro kompresi dynamického služby IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate modulu](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx komprese a dekomprese](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hostování přímo na:
  * [Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Komprese odpovědi
Obvykle všechny odpovědi komprimované nativně využívat výhod komprese odpovědi. Odpovědi komprimované nativně obvykle zahrnují: šablon stylů CSS, JavaScript, HTML, XML a JSON. Neměli komprimovat nativně komprimované prostředky, jako jsou například soubory PNG. Když zkusíte další komprimovat nativně komprimované odpověď, jakékoli malé další snížení velikosti a přenosu čas pravděpodobně overshadowed o dobu, kterou trvalo zpracování komprese. Nekomprimovat soubory menší než asi 1 150 000 bajtů (v závislosti na obsahu souboru a efektivitu komprese). Režijní náklady komprimace malých souborů může vytvořit komprimovaný soubor větší než nekomprimovaného souboru.

Když klient může zpracovat komprimovaného obsahu, klient musí uvědomit serveru její možnosti `Accept-Encoding` hlavičky v požadavku. Když server odešle komprimovaného obsahu, musí obsahovat informace v `Content-Encoding` záhlaví na tom, jak je zakódován zkomprimovanou odpověď. V následující tabulce jsou uvedeny obsahu kódování označení nepodporuje middleware.

| `Accept-Encoding` hodnoty hlavičky | Podporované middlewaru. | Popis                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | Ne                   | Formát Brotli komprimovaná Data                               |
| `compress`                      | Ne                   | Formát dat "komprimovat" systému UNIX                                 |
| `deflate`                       | Ne                   | "deflate" komprimovaná data uvnitř formát dat "zlib"     |
| `exi`                           | Ne                   | Výměnu efektivní XML W3C                               |
| `gzip`                          | Ano (výchozí)        | Formát souboru GZIP                                            |
| `identity`                      | Ano                  | "Bez kódování" identifikátor: odpověď nesmí být zakódován. |
| `pack200-gzip`                  | Ne                   | Formát přenosu sítě pro Java archivy                   |
| `*`                             | Ano                  | Žádný dostupný obsah kódování není explicitně požadovaný     |

Další informace najdete v tématu [IANA oficiální obsahu kódování seznamu](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Middleware umožňuje přidat další komprese zprostředkovatele pro vlastní `Accept-Encoding` hodnoty hlavičky. Další informace najdete v tématu [Vlastní zprostředkovatelé](#custom-providers) níže.

Middleware je schopný reagovat na hodnotu kvality (qvalue, `q`) vážení při odeslání klient k určení priority schémat komprese. Další informace najdete v tématu [RFC 7231: přijměte kódování](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algoritmy komprese podléhají kompromis mezi rychlostí komprese a efektivitu komprese. *Efektivita* v tomto kontextu odkazuje na velikost výstupu po kompresi. Nejmenší velikost se dosahuje nejvíc *optimální* komprese.

Hlavičky účastnící se požaduje, odesílání, ukládání do mezipaměti a přijímání komprimovaného obsahu jsou popsány v následující tabulce.

| Záhlaví             | Role |
| ------------------ | ---- |
| `Accept-Encoding`  | Odeslaných z klienta na server k označení obsahu, kódování schémata přijatelné pro klienta. |
| `Content-Encoding` | Odeslaných ze serveru do klienta k označení, kódování obsahu v datové části. |
| `Content-Length`   | Když dojde k komprese, `Content-Length` záhlaví je odebrán, od změny obsahu textu při zkomprimovanou odpověď. |
| `Content-MD5`      | Když dojde k komprese, `Content-MD5` záhlaví je odebrán, protože došlo ke změně textu obsahu a hodnota hash je již neplatný. |
| `Content-Type`     | Určuje typ MIME obsahu. Každé odpovědi by měl určovat jeho `Content-Type`. Middleware ověří tuto hodnotu, která určí, pokud odpověď by měla být zkomprimuje. Middleware určuje sadu [výchozí typy MIME](#mime-types) , může být kódován, ale můžete nahradit nebo přidat typy MIME. |
| `Vary`             | Při odeslání serveru s hodnotou `Accept-Encoding` pro klienty a proxy servery, `Vary` Hlavička uvádí, klienta nebo proxy server, který by měl mezipaměti (lišit) na základě odpovědí na základě hodnoty `Accept-Encoding` hlavičky žádosti. Výsledek vrácení obsahu pomocí `Vary: Accept-Encoding` záhlaví je, že oba komprimované a jsou v nekomprimované odpovědi samostatně ukládat do mezipaměti. |

Můžete prozkoumat funkce middlewaru komprese odpovědi s [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). Ukázka předvádí:
* Komprese odpovědí aplikace pomocí gzip a poskytovatelé vlastní komprese.
* Jak přidat typ MIME do seznamu výchozích typů standardu MIME pro kompresi.

## <a name="package"></a>Balíček
Zahrnout middleware projekt, přidejte odkaz na [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) balíček nebo použít [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) balíčku. Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.

## <a name="configuration"></a>Konfigurace
Následující kód ukazuje, jak povolit kompresi gzip výchozí a pro typy MIME výchozí Middleware komprese odpovědi.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), nebo [Postman](https://www.getpostman.com/) nastavit `Accept-Encoding` hlavička požadavku a prostudovali hlavičky odpovědi, velikost a text.

Odeslat žádost o ukázková aplikace bez `Accept-Encoding` záhlaví a zjistíte, že je odpověď na nekomprimované. `Content-Encoding` a `Vary` hlavičky nejsou k dispozici na odpověď.

![Zobrazuje výsledek požadavku bez hlavičky Accept-Encoding okno aplikaci Fiddler. Odpověď není zkomprimuje.](response-compression/_static/request-uncompressed.png)

Odeslat žádost o vzorovou aplikaci s `Accept-Encoding: gzip` záhlaví a zjistíte, že zkomprimovanou odpověď. `Content-Encoding` a `Vary` hlavičky se nacházejí na odpověď.

![Výsledek požadavku s hlavičky Accept-Encoding, a hodnota gzip okno aplikaci Fiddler. Hlavičky měnit a kódování obsahu se přidají do odpovědi. Zkomprimovanou odpověď.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>Zprostředkovatelé
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
Použití `GzipCompressionProvider` kompresi odpovědí s gzip. Toto je výchozí zprostředkovatel komprese-li zadán žádný. Komprese můžete nastavit úroveň s `GzipCompressionProviderOptions`. 

Výchozí zprostředkovatel kompresi gzip nejrychlejší úroveň komprese (`CompressionLevel.Fastest`), který nemusí vracet nejúčinnější komprese. Pokud se požaduje nejúčinnější komprese, můžete nakonfigurovat middleware pro optimální komprese.

| Úroveň komprese                | Popis                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | Komprese co nejrychleji provést i v případě, že výsledný výstup není komprimována optimálně. |
| `CompressionLevel.NoCompression` | Je třeba provést bez komprese.                                                                           |
| `CompressionLevel.Optimal`       | Odpovědi musí být optimálně komprimován, i v případě, že komprese trvá déle.                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>typy MIME
Middleware určuje sadu výchozích typů standardu MIME pro kompresi:
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Můžete nahradit nebo připojit typy MIME s možnosti middlewaru komprese odpovědi. Všimněte si, že zástupný znak standardu MIME typy, jako například `text/*` nejsou podporovány. Ukázková aplikace přidá typ MIME pro `image/svg+xml` komprimuje a slouží ASP.NET Core banner image (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>Vlastní zprostředkovatelé
Můžete vytvořit vlastní komprese implementace s `ICompressionProvider`. `EncodingName` Reprezentuje obsah, který tato kódování `ICompressionProvider` vytváří. Middleware používá tuto informaci k vyberte poskytovatele správy na základě zadané v seznamu `Accept-Encoding` hlavičky žádosti.

Použití ukázkové aplikace, klient odešle žádost s `Accept-Encoding: mycustomcompression` záhlaví. Middleware použije implementace vlastní komprese a vrátí odpověď se `Content-Encoding: mycustomcompression` záhlaví. Klient musí umět dekomprimovat vlastní kódování v pořadí pro implementaci vlastní komprese pracovat.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Odeslat žádost o vzorovou aplikaci s `Accept-Encoding: mycustomcompression` záhlaví a sledovat hlavičky odpovědi. `Vary` a `Content-Encoding` hlavičky se nacházejí na odpověď. Text odpovědi (není vidět) není komprimována ve vzorku. Není k dispozici implementace komprese ve `CustomCompressionProvider` třída vzorku. Ukázka však ukazuje, kde by implementovat algoritmus komprese.

![Výsledek požadavku s hlavičky Accept-Encoding, a hodnota mycustomcompression okno aplikaci Fiddler. Hlavičky měnit a kódování obsahu se přidají do odpovědi.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Komprese s zabezpečený protokol
Komprimované odpovědi prostřednictvím zabezpečeného připojení se dá řídit pomocí `EnableForHttps` možnost, která je ve výchozím nastavení zakázané. Komprese pomocí dynamicky generovaném stránky může způsobit problémy se zabezpečením, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky.

## <a name="adding-the-vary-header"></a>Přidání měnit hlavičky
Při kompresi odpovědí na základě `Accept-Encoding` záhlaví, existují potenciálně více komprimované verze odpovědi a nekomprimované verze. Chcete-li pokyn mezipaměti klienta a serveru proxy, několik verzí existují a by měla být uložena, `Vary` záhlaví se přidá s `Accept-Encoding` hodnotu. V ASP.NET Core 1.x, přidání `Vary` hlavičky odpovědi dosahuje ručně. V ASP.NET Core 2.x, přidá middleware `Vary` záhlaví automaticky, když zkomprimovanou odpověď.

**ASP.NET základní pouze 1.x**

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Middleware problém při za Nginx reverzní proxy server
Pokud je požadavek směrovány přes proxy server pomocí Nginx, `Accept-Encoding` záhlaví se odebere. Middleware zabrání kompresi odpovědi. Další informace najdete v tématu [NGINX: komprese a dekomprese](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Tento problém je sledovanými [zjistěte průchozí komprese Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Práce s dynamické komprese služby IIS
Pokud máte aktivní IIS dynamické komprese modul nakonfigurována na úrovni serveru, který chcete zakázat pro aplikaci, můžete tak učinit s doplňkem k vaší *web.config* souboru. Další informace najdete v tématu [moduly IIS zakázání](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Poradce při potížích
Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), nebo [Postman](https://www.getpostman.com/), které umožňují nastavit `Accept-Encoding` hlavička požadavku a prostudovali hlavičky odpovědi, velikost a text. Middleware komprese odpovědi komprimaci odpovědi, které splňují následující podmínky:
* `Accept-Encoding` Záhlaví nachází s hodnotou `gzip`, `*`, nebo vlastní kódování, který odpovídá komprese vlastní zprostředkovatele, který jste vytvořili. Hodnota nesmí být `identity` nebo obsahovat hodnotu kvality (qvalue, `q`) nastavení 0 (nula).
* Typ MIME (`Content-Type`) musí být nastavená a nakonfigurovaná na typ MIME se musí shodovat `ResponseCompressionOptions`.
* Nesmí zahrnovat požadavek `Content-Range` záhlaví.
* Pokud zabezpečeného protokolu (https) je nakonfigurován v možnosti middlewaru komprese odpovědi, musíte použít požadavek nezabezpečené protokol (http). *Všimněte si nebezpečí [popsané výše](#compression-with-secure-protocol) při povolování zabezpečené komprese obsahu.*

## <a name="additional-resources"></a>Další zdroje
* [Spuštění aplikace](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
* [Mozilla Developer Network: Přijměte kódování](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 části 3.1.2.1: Obsahu Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 oddílu 4.2.3: Kódování Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Verze specifikaci formátu souboru GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
