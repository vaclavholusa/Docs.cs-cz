---
uid: web-api/overview/advanced/http-cookies
title: Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368402"
---
<a name="http-cookies-in-aspnet-web-api"></a>Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Toto téma popisuje, jak odesílat a přijímat soubory cookie protokolu HTTP v rozhraní Web API.

## <a name="background-on-http-cookies"></a>Na pozadí na soubory cookie protokolu HTTP

Tato část poskytuje stručný přehled o tom, jak jsou implementované soubory cookie na úrovni protokolu HTTP. Podrobné informace, [RFC 6265](http://tools.ietf.org/html/rfc6265).

Soubor cookie je část dat, která odesílá na server v odpovědi HTTP. Klient (volitelně) uloží soubor cookie a vrátí na subsequet požadavky. Díky tomu klienta a serveru, sdílení stavu. Nastavení souboru cookie, server obsahuje hlavičku Set-Cookie v odpovědi. Formát souboru cookie je pár název hodnota pomocí volitelných atributů. Příklad:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Tady je příklad s atributy:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Pokud chcete vrátit do souboru cookie na serveru, klienta obsahuje hlavičku souboru Cookie novějším žádostem.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Odpověď HTTP může obsahovat více záhlaví Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Klient odešle několik souborů cookie pomocí jednoho hlavičky souboru Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Rozsahu a doby trvání soubor cookie se řídí následujícími atributy v hlavičce Set-Cookie:

- **Domény**: instruuje klienta, které domény souboru cookie přijímat. Pokud tato doména "example.com", klient vrátí soubor cookie pro každou poddoménu example.com. Pokud není zadán, doména je zdrojový server.
- **Cesta**: omezuje na zadané cestě v rámci domény souboru cookie. Pokud není zadán, je použít cesty identifikátoru URI požadavku.
- **Vypršení platnosti**: nastaví datum vypršení platnosti souboru cookie. Klient odstraní soubor cookie, když jeho platnost vyprší.
- **Max-Age**: Nastaví maximální stáří souboru cookie. Klient odstraní soubor cookie, když dosáhl maximální stáří.

Pokud mají oba `Expires` a `Max-Age` jsou nastaveny, `Max-Age` přednost. Pokud je nastavena ani jedno, klient odstraní soubor cookie po skončení aktuální relaci. (Přesné význam "relace" se určuje podle uživatelského agenta).

Nezapomínejte, že klienti mohou ignorovat soubory cookie. Uživatel může například zakažte soubory cookie z důvodů ochrany osobních údajů. Klienti soubory cookie odstranit předtím, než vyprší jejich platnost, nebo omezit počet uložených souborů cookie. Z důvodu ochrany osobních údajů klientů často odmítnout "třetích stran" soubory cookie, kde doména se neshoduje s původním serveru. Stručně řečeno server neměli spoléhat na návrat, které nastaví soubory cookie.

## <a name="cookies-in-web-api"></a>Soubory cookie ve webovém rozhraní API

Pokud chcete přidat do souboru cookie do odpovědi HTTP, vytvořte **CookieHeaderValue** instanci, která představuje soubor cookie. Zavolejte **AddCookies** rozšiřující metoda, která je definována v **System.Net.Http. HttpResponseHeadersExtensions** třídy, chcete-li přidat soubor cookie.

Například následující kód přidá soubor cookie v rámci akce kontroleru:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Všimněte si, že **AddCookies** přijímá pole **CookieHeaderValue** instancí.

Chcete-li extrahovat soubory cookie z požadavku klienta, zavolejte **GetCookies** metody:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** obsahuje kolekci **CookieState** instancí. Každý **CookieState** představuje jeden soubor cookie. Získat pomocí metody indexeru **CookieState** podle názvu, jak je znázorněno.

## <a name="structured-cookie-data"></a>Soubor Cookie strukturovaná Data

Většina prohlížečů omezení počtu souborů cookie, které se budou ukládat&#8212;celkový počet i číslo na doménu. Proto může být užitečné zavést strukturovaná data do jednoho souboru cookie, namísto nastavení více souborů cookie.

> [!NOTE]
> RFC 6265 nedefinuje strukturu dat souboru cookie.


Použití **CookieHeaderValue** třídy, můžete předat seznam dvojic název hodnota pro data souborů cookie. Tyto páry název hodnota jsou zakódovány jako data kódovaná adresou URL formuláře v hlavičce Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Předchozí kód vytvoří následující hlavičkou Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** třída poskytuje metodu indexer načíst dílčí hodnoty ze souboru cookie ve zprávě požadavku:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Příklad: Nastavení a načtení souborů cookie v obslužné rutiny zpráv

Předchozí příklady nám ukázaly, jak používat soubory cookie z v rámci kontroleru webového rozhraní API. Další možností je použít [obslužné rutiny zpráv](http-message-handlers.md). Obslužné rutiny zpráv jsou vyvolány dříve v kanálu než řadiče. Obslužné rutiny zpráv můžete číst soubory cookie z požadavku, než požadavek dosáhne kontroleru nebo po kontroleru generuje odpovědi přidat soubory cookie v odpovědi.

![](http-cookies/_static/image2.png)

Následující kód ukazuje obslužné rutiny zpráv pro vytvoření ID relace. ID relace se ukládají do souboru cookie. Obslužná rutina ověří požadavek pro soubor cookie relace. Pokud žádost neobsahuje soubor cookie, obslužná rutina generuje ID nové relace. V obou případech se obslužná rutina uloží ID relace v **HttpRequestMessage.Properties** kontejner objektů a dat. Také přidá soubor cookie relace do odpovědi HTTP.

Tato implementace nelze ověřit, že ID relace, od klienta byl ve skutečnosti vydán serverem. Nepoužívejte ho jako formu ověřování! Bod v příkladu je zobrazení správy souboru cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Kontroler můžete získat ID relace, od **HttpRequestMessage.Properties** kontejner objektů a dat.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
