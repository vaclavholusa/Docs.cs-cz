---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevence útoků proti padělání (CSRF) žádosti více webů v ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje útok proti padělání (CSRF) podvržení žádosti a implementovat opatření proti CSRF v ASP.NET Web API.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: cd7d978190d28a028285746781a380d9bb5f91d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752852"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Prevence útoků proti padělání (CSRF) žádosti více webů v ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Padělání žádosti mezi weby (CSRF) je útok, kde škodlivý web odešle požadavek na zranitelné lokality, kde uživatel je momentálně přihlášený

Tady je příklad útok CSRF:

1. Přihlášení uživatele do `www.example.com` ověřování pomocí formulářů.
2. Server ověřuje uživatele. Odpověď ze serveru obsahuje soubor cookie ověřování.
3. Bez odhlášení, uživatel navštíví škodlivý web. Tento škodlivý web obsahuje následující formulář HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Všimněte si, že akce formuláře zveřejní ohrožených site nechcete škodlivé weby. Toto je část "webů" CSRF.
4. Uživatel klikne na tlačítko Odeslat. Prohlížeč zahrnuje ověřovacího souboru cookie s požadavkem.
5. Požadavek běží na serveru s kontext ověřování uživatele a dělat cokoli, ověřený uživatel smí provádět.

Přestože tento příklad vyžaduje, aby uživatel kliknout na tlačítko formuláře, stejně jako můžete snadno spouštět skript, který se odešle formulář automaticky může škodlivé stránky. Kromě toho pomocí protokolu SSL nebrání útok CSRF, protože škodlivý web můžete poslat žádost o "https://".

Útokům CSRF jsou obvykle možné proti webové stránky, které používají soubory cookie pro ověřování, protože prohlížeče odesílají všechny relevantní soubory cookie na cílový webový server. Útokům CSRF však nejsou omezené na využívání soubory cookie. Například jsou Basic a Digest ověřování také zranitelné. Po přihlášení uživatele pomocí ověřování základní nebo Digest. prohlížeč automaticky odesílá přihlašovací údaje, dokud se relace neukončí.

## <a name="anti-forgery-tokens"></a>Tokeny proti zfalšování

Aby se zabránilo útokům CSRF, ASP.NET MVC používá tokeny proti zfalšování, také nazývané *požádat o tokeny ověření*.

1. Si klient vyžádá stránku HTML, který obsahuje formulář.
2. Server obsahuje dva tokeny v odpovědi. Jeden token se odešle jako soubor cookie. Druhá je umístěn v skryté pole formuláře. Tokeny jsou generovány náhodně tak, že nežádoucí osoba nelze uhodnout hodnoty.
3. Když klient odešle formulář, kterou musí odesílat oba tokeny zpět na server. Klient odešle token souboru cookie jako soubor cookie a odešle token formuláře uvnitř data formuláře. (Klientský prohlížeč to provede automaticky při odeslání formuláře.)
4. Pokud žádost neobsahuje oba tokeny, server nepovoluje požadavku.

Tady je příklad z formuláře HTML s tokenem skryté formuláře:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokenů proti padělání pracovat, protože škodlivý stránku nelze číst tokeny uživatele z důvodu zásadami stejného původu. ([Zásadami stejného původu](http://www.w3.org/Security/wiki/Same_Origin_Policy) zabránit dokumenty hostovaný na dvou různých lokalit v přístupu k obsahu druhé strany. V předchozím příkladu se zlými úmysly stránky mohou odesílat žádosti example.com, takže ho nejde přečíst odpověď.)

Chcete-li zabránit útokům CSRF, pomocí tokenů proti padělání pomocí libovolného protokolu pro ověřování, kde tiše prohlížeč odesílá pověření po přihlášení uživatele. To zahrnuje protokoly pro ověřování na základě souboru cookie, jako je například ověřování pomocí formulářů, jakož i protokoly, jako jsou ověřování Basic a Digest.

Je potřeba tokenů proti padělání pro jakékoli nonsafe metody (POST, PUT, DELETE). Také se ujistěte, že bezpečné metody (GET, HEAD) nemají žádné vedlejší účinky. Kromě toho pokud povolíte podporu napříč doménami, jako je například CORS a JSONP, pak i bezpečné metody, například GET jsou potenciálně zranitelná vůči útokům CSRF, mu umožní načíst potenciálně citlivá data.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokeny proti zfalšování v architektuře ASP.NET MVC

Chcete-li přidat tokeny proti zfalšování na stránku Razor, použijte **HtmlHelper.AntiForgeryToken** pomocnou metodu:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Tato metoda přidá skryté pole formuláře a také nastaví token souboru cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-útokům CSRF a AJAX

Token formuláře může být problém pro odesílání požadavků AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML. Jedním z řešení je odesílat tokeny ve vlastní hlavičku HTTP. Následující kód používá syntaxi Razor ke generování tokenů a pak přidá tokenů pro požadavek AJAX. Tokeny jsou generovány na serveru voláním **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Při zpracování požadavku extrahujte tokeny v hlavičce požadavku. Zavolejte **AntiForgery.Validate** metodu za účelem ověření tokenů. **Ověřit** metoda vyvolá výjimku, pokud tokeny nejsou platné.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
