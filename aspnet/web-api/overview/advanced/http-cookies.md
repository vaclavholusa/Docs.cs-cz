---
uid: web-api/overview/advanced/http-cookies
title: Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
<a name="http-cookies-in-aspnet-web-api"></a>Soubory cookie protokolu HTTP v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Toto téma popisuje, jak odesílat a přijímat soubory cookie protokolu HTTP v rozhraní Web API.

## <a name="background-on-http-cookies"></a>Pozadí na souborech cookie HTTP

Tato část poskytuje stručný přehled o tom, jak jsou implementované soubory cookie na úrovni protokolu HTTP. Podrobné informace, [RFC 6265](http://tools.ietf.org/html/rfc6265).

Cookie je část dat, který server odešle v odpovědi HTTP. Klient (volitelně) uloží soubor cookie a vrátí ji u subsequet požadavků. To umožňuje klientovi i serveru sdílet stavu. Pokud chcete nastavit soubor cookie, server obsahuje hlavičku Set-Cookie v odpovědi. Formát souboru cookie je pár název hodnota s volitelných atributů. Příklad:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Tady je příklad s atributy:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Pokud chcete vrátit do souboru cookie na serveru, klienta zahrnuje hlavička Cookie v novější požadavky.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Odpověď HTTP může obsahovat více hlavičky Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Klient vrátí více souborů cookie pomocí jednoho hlavička Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Rozsah a platnost souboru cookie jsou řízeny následující atributy v hlavičce Set-Cookie:

- **Domény**: řekne klientovi, které domény by měly dostávat souboru cookie. Pokud doména je "example.com", klient odešle soubor cookie pro každou poddoménu example.com. Pokud není zadaný, doména je zdrojový server.
- **Cesta**: omezuje soubor cookie na zadané cestě v rámci domény. Pokud není zadaný, použije se cestu identifikátoru URI požadavku.
- **Vypršení platnosti**: nastaví datum vypršení platnosti souboru cookie. Klient odstraní soubor cookie, když vyprší její platnost.
- **Max-Age**: Nastaví maximální stáří souboru cookie. Klient odstraní soubor cookie při dosažení maximální stáří.

Pokud oba `Expires` a `Max-Age` jsou nastavené, `Max-Age` přednost. Pokud je nastavena ani jeden z nich, odstraní klienta při ukončení relace. aktuální soubor cookie. (Přesný význam "relace" je určen podle uživatelského agenta).

Ale pozor, aby klienti ignorovat soubory cookie. Uživatel může například zakázat soubory cookie z důvodů ochrany osobních údajů. Klienti lze soubory cookie odstranit, než vyprší jejich platnost, nebo omezit počet uložené soubory cookie. Z důvodu ochrany osobních údajů klientů často odmítnout soubory cookie "třetích stran", kde domény neodpovídá na zdrojový server. Stručně řečeno server neměli spoléhat na získávání soubory cookie, které se nastaví zpět.

## <a name="cookies-in-web-api"></a>Soubory cookie v rozhraní Web API

Chcete-li přidat do souboru cookie do odpovědi HTTP, vytvořte **CookieHeaderValue** instanci, která představuje soubor cookie. Potom zavolejte **AddCookies** metoda rozšíření, která je definována v **System.Net.Http. HttpResponseHeadersExtensions** třídy, přidejte soubor cookie.

Následující kód například přidá soubor cookie v rámci akce kontroleru:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Všimněte si, že **AddCookies** přijímá pole **CookieHeaderValue** instance.

Pokud chcete extrahovat soubory cookie z požadavku klienta, volání **GetCookies** metoda:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** obsahuje kolekci **CookieState** instance. Každý **CookieState** představuje jeden soubor cookie. Pomocí této metody indexer získat **CookieState** podle názvu, jak je vidět.

## <a name="structured-cookie-data"></a>Soubor Cookie strukturovaných dat

Mnoho prohlížečů omezení počtu souborů cookie, které se budou ukládat&#8212;celkový počet i číslo v každé doméně. Proto může být užitečné uvést strukturovaných dat do jednoho souboru cookie, namísto nastavování více souborů cookie.

> [!NOTE]
> RFC 6265 nedefinuje strukturu dat souboru cookie.


Pomocí **CookieHeaderValue** třídu, můžete předat seznam dvojic název hodnota pro data souboru cookie. Tyto páry název hodnota jsou zakódovány jako data kódovaná adresou URL formuláře v hlavičce Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Předchozí kód vytvoří následující hlavičku Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** třída poskytuje metody indexer číst ze souboru cookie ve zprávě požadavku dílčí hodnoty:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Příklad: Nastavení a načtení souborů cookie v obslužné rutiny zpráv

V předchozích příkladech vám ukázal, jak používat soubory cookie v rámci kontroleru webového rozhraní API. Další možností je použít [obslužné rutiny zpráv](http-message-handlers.md). Obslužné rutiny zpráv jsou vyvolány dříve v kanálu než řadičů. Obslužné rutiny zpráv můžou číst soubory cookie z požadavku, než požadavek dosáhne řadičem nebo po řadičem generuje odpovědi přidat do odpovědi soubory cookie.

![](http-cookies/_static/image2.png)

Následující kód ukazuje obslužné rutiny zpráv pro vytvoření ID relace. ID relace je uložený v souboru cookie. Obslužná rutina ověří žádost o souboru cookie relace. Pokud žádost neobsahuje soubor cookie, obslužná rutina generuje ID nové relace. V obou případech obslužná rutina ukládá ID relace v **HttpRequestMessage.Properties** kontejneru objektů a dat. Soubor cookie relace také přidá do odpovědi HTTP.

Tato implementace neověřuje, zda Identifikátor relace z klienta bylo vydáno ve skutečnosti serveru. Nepoužívejte ho jako formu ověřování! Bod v příkladu se má zobrazit správy souboru cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Řadič můžete získat z ID relace **HttpRequestMessage.Properties** kontejneru objektů a dat.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
