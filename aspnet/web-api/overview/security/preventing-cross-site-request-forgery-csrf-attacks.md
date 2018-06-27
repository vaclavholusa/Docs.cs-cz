---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Popisuje útoku (proti útokům CSRF) padělání požadavku posílaného mezi weby a jak provádět opatření proti proti útokům CSRF v rozhraní ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5e7b24c697e0bb37f388341abd89609c76f6b64c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961234"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Proti útokům webů padělání požadavku (CSRF) je útok, kde škodlivé weby odešle požadavek na citlivé lokality, kde je uživatel momentálně přihlášen v

Tady je příklad útoku proti útokům CSRF:

1. Přihlášení uživatele do `www.example.com` ověřování pomocí formulářů.
2. Server ověřuje uživatele. Odpověď ze serveru obsahuje soubor cookie ověřování.
3. Bez protokolování uživatel navštíví škodlivý web. Tento škodlivý lokalita obsahuje následující formuláře HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Všimněte si, že akce formuláře požadavky na server, snadno napadnutelný, aby škodlivé weby. Toto je část "webů" proti útokům CSRF.
4. Uživatel kliknutím na tlačítko pro odeslání. V prohlížeči zahrnuje ověřovacího souboru cookie s požadavkem.
5. Žádost se spustí na serveru s kontext ověřování uživatele a dělat vše, co ověřený uživatel může provádět.

I když tento příklad vyžaduje, aby uživatel klepnutím na tlačítko formuláře, na stránce škodlivého může stejným způsobem jako snadno spustit skript, který automaticky odešle formulář. Kromě toho pomocí protokolu SSL nebrání útoku proti útokům CSRF protože škodlivé weby můžete odeslat požadavek "https://".

Obvykle je možné proti webové stránky, které používají soubory cookie pro ověřování, proti útokům CSRF útokům, protože prohlížeče odesílají všechny relevantní soubory cookie k cílovému webu. Útoky proti útokům CSRF však nejsou omezeny na zneužitím soubory cookie. Ověřování Basic a Digest jsou například také snadno napadnutelný. Po přihlášení uživatele pomocí ověřování Basic nebo ověřování hodnotou hash. v prohlížeči automaticky odesílá přihlašovací údaje, dokud ukončení relace.

## <a name="anti-forgery-tokens"></a>Tokeny proti zfalšování

Aby se zabránilo útokům proti útokům CSRF, ASP.NET MVC používá tokeny proti zfalšování, označované taky jako *žádosti o ověření tokeny*.

1. Klient požádá o stránku HTML, který obsahuje formuláře.
2. Server obsahuje dva tokeny v odpovědi. Jeden token se odešle jako soubor cookie. Druhá je umístěn v skryté pole formuláře. Tokeny jsou generovány náhodně tak, aby nežádoucí osoba nelze snadno uhodnout hodnoty.
3. Když klient odešle formulář, je potřeba poslat oba tokeny zpět na server. Klient odešle token souboru cookie jako soubor cookie a odešle token formuláře uvnitř data formuláře. (Klientský prohlížeč automaticky to provede, když uživatel odešle formulář.)
4. Pokud žádost neobsahuje oba tokeny, zakáže server žádost.

Tady je příklad formuláře HTML s tokenem skrytého:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokeny proti zfalšování fungovat, protože škodlivý stránky nelze přečíst tokeny uživatele z důvodu zásad stejného původu. ([Zásady stejného původu](http://www.w3.org/Security/wiki/Same_Origin_Policy) zabránit dokumenty, které jsou hostované na dvou různých lokalit v přístupu k obsahu druhé strany. Proto v předchozím příkladu škodlivé stránky mohou odesílat žádosti example.com, ale nelze přečíst odpověď.)

Chcete zabránit útokům proti útokům CSRF, použijte tokeny proti zfalšování s libovolný protokol pro ověřování, kde bezobslužně prohlížeč odesílá přihlašovací údaje po přihlášení uživatele. To zahrnuje protokoly ověřování na základě souborů cookie, jako je například ověřování pomocí formulářů, a také protokoly, jako je například ověřování Basic a Digest.

Pro žádné nonsafe metody (POST, PUT, DELETE) byste měli vyžadovat tokeny proti zfalšování. Taky se ujistěte, že bezpečné metody (GET, HEAD) nemají žádné vedlejší účinky. Kromě toho pokud povolíte podporu mezi doménami, například CORS nebo JSONP, pak i bezpečné metody, třeba GET jsou potenciálně bude zranitelný vůči útoku proti útokům CSRF se mu umožní načíst potenciálně citlivá data.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokeny proti zfalšování v architektuře ASP.NET MVC

Přidat tokeny proti zfalšování na stránku Razor, použijte **HtmlHelper.AntiForgeryToken** pomocnou metodu:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Tato metoda přidá skryté pole formuláře a také nastaví token souboru cookie.

## <a name="anti-csrf-and-ajax"></a>Ochrana proti útokům CSRF a AJAX

Tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML. Jedno řešení je odeslat tokenů v vlastní hlavičku HTTP. Následující kód používá syntaxi Razor ke generování tokenů a potom přidá tokenů na požadavek AJAX. Tokeny jsou generovány na serveru voláním **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Při zpracovávání požadavku extrahuje tokeny z hlaviček požadavku. Potom zavolejte **AntiForgery.Validate** metodu za účelem ověření tokenů. **Ověřením** metoda vyvolá výjimku, pokud tokeny nejsou platné.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
