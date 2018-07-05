---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevence útoků proti padělání (CSRF) žádosti více webů v ASP.NET Web API | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje útok proti padělání (CSRF) podvržení žádosti a implementovat opatření proti CSRF v ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 7dfddf09a1577cfa7a52f58b37533724a8475435
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379871"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a><span data-ttu-id="337b0-103">Prevence útoků proti padělání (CSRF) žádosti více webů v ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="337b0-103">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>
====================
<span data-ttu-id="337b0-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="337b0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="337b0-105">Padělání žádosti mezi weby (CSRF) je útok, kde škodlivý web odešle požadavek na zranitelné lokality, kde uživatel je momentálně přihlášený</span><span class="sxs-lookup"><span data-stu-id="337b0-105">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in</span></span>

<span data-ttu-id="337b0-106">Tady je příklad útok CSRF:</span><span class="sxs-lookup"><span data-stu-id="337b0-106">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="337b0-107">Přihlášení uživatele do `www.example.com` ověřování pomocí formulářů.</span><span class="sxs-lookup"><span data-stu-id="337b0-107">A user logs into `www.example.com` using forms authentication.</span></span>
2. <span data-ttu-id="337b0-108">Server ověřuje uživatele.</span><span class="sxs-lookup"><span data-stu-id="337b0-108">The server authenticates the user.</span></span> <span data-ttu-id="337b0-109">Odpověď ze serveru obsahuje soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="337b0-109">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="337b0-110">Bez odhlášení, uživatel navštíví škodlivý web.</span><span class="sxs-lookup"><span data-stu-id="337b0-110">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="337b0-111">Tento škodlivý web obsahuje následující formulář HTML:</span><span class="sxs-lookup"><span data-stu-id="337b0-111">This malicious site contains the following HTML form:</span></span> 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    <span data-ttu-id="337b0-112">Všimněte si, že akce formuláře zveřejní ohrožených site nechcete škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="337b0-112">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="337b0-113">Toto je část "webů" CSRF.</span><span class="sxs-lookup"><span data-stu-id="337b0-113">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="337b0-114">Uživatel klikne na tlačítko Odeslat.</span><span class="sxs-lookup"><span data-stu-id="337b0-114">The user clicks the submit button.</span></span> <span data-ttu-id="337b0-115">Prohlížeč zahrnuje ověřovacího souboru cookie s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="337b0-115">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="337b0-116">Požadavek běží na serveru s kontext ověřování uživatele a dělat cokoli, ověřený uživatel smí provádět.</span><span class="sxs-lookup"><span data-stu-id="337b0-116">The request runs on the server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="337b0-117">Přestože tento příklad vyžaduje, aby uživatel kliknout na tlačítko formuláře, stejně jako můžete snadno spouštět skript, který se odešle formulář automaticky může škodlivé stránky.</span><span class="sxs-lookup"><span data-stu-id="337b0-117">Although this example requires the user to click the form button, the malicious page could just as easily run a script that submits the form automatically.</span></span> <span data-ttu-id="337b0-118">Kromě toho pomocí protokolu SSL nebrání útok CSRF, protože škodlivý web můžete poslat žádost o "https://".</span><span class="sxs-lookup"><span data-stu-id="337b0-118">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="337b0-119">Útokům CSRF jsou obvykle možné proti webové stránky, které používají soubory cookie pro ověřování, protože prohlížeče odesílají všechny relevantní soubory cookie na cílový webový server.</span><span class="sxs-lookup"><span data-stu-id="337b0-119">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="337b0-120">Útokům CSRF však nejsou omezené na využívání soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="337b0-120">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="337b0-121">Například jsou Basic a Digest ověřování také zranitelné.</span><span class="sxs-lookup"><span data-stu-id="337b0-121">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="337b0-122">Po přihlášení uživatele pomocí ověřování základní nebo Digest.</span><span class="sxs-lookup"><span data-stu-id="337b0-122">After a user logs in with Basic or Digest authentication.</span></span> <span data-ttu-id="337b0-123">prohlížeč automaticky odesílá přihlašovací údaje, dokud se relace neukončí.</span><span class="sxs-lookup"><span data-stu-id="337b0-123">the browser automatically sends the credentials until the session ends.</span></span>

## <a name="anti-forgery-tokens"></a><span data-ttu-id="337b0-124">Tokeny proti zfalšování</span><span class="sxs-lookup"><span data-stu-id="337b0-124">Anti-Forgery Tokens</span></span>

<span data-ttu-id="337b0-125">Aby se zabránilo útokům CSRF, ASP.NET MVC používá tokeny proti zfalšování, také nazývané *požádat o tokeny ověření*.</span><span class="sxs-lookup"><span data-stu-id="337b0-125">To help prevent CSRF attacks, ASP.NET MVC uses anti-forgery tokens, also called *request verification tokens*.</span></span>

1. <span data-ttu-id="337b0-126">Si klient vyžádá stránku HTML, který obsahuje formulář.</span><span class="sxs-lookup"><span data-stu-id="337b0-126">The client requests an HTML page that contains a form.</span></span>
2. <span data-ttu-id="337b0-127">Server obsahuje dva tokeny v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="337b0-127">The server includes two tokens in the response.</span></span> <span data-ttu-id="337b0-128">Jeden token se odešle jako soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="337b0-128">One token is sent as a cookie.</span></span> <span data-ttu-id="337b0-129">Druhá je umístěn v skryté pole formuláře.</span><span class="sxs-lookup"><span data-stu-id="337b0-129">The other is placed in a hidden form field.</span></span> <span data-ttu-id="337b0-130">Tokeny jsou generovány náhodně tak, že nežádoucí osoba nelze uhodnout hodnoty.</span><span class="sxs-lookup"><span data-stu-id="337b0-130">The tokens are generated randomly so that an adversary cannot guess the values.</span></span>
3. <span data-ttu-id="337b0-131">Když klient odešle formulář, kterou musí odesílat oba tokeny zpět na server.</span><span class="sxs-lookup"><span data-stu-id="337b0-131">When the client submits the form, it must send both tokens back to the server.</span></span> <span data-ttu-id="337b0-132">Klient odešle token souboru cookie jako soubor cookie a odešle token formuláře uvnitř data formuláře.</span><span class="sxs-lookup"><span data-stu-id="337b0-132">The client sends the cookie token as a cookie, and it sends the form token inside the form data.</span></span> <span data-ttu-id="337b0-133">(Klientský prohlížeč to provede automaticky při odeslání formuláře.)</span><span class="sxs-lookup"><span data-stu-id="337b0-133">(A browser client automatically does this when the user submits the form.)</span></span>
4. <span data-ttu-id="337b0-134">Pokud žádost neobsahuje oba tokeny, server nepovoluje požadavku.</span><span class="sxs-lookup"><span data-stu-id="337b0-134">If a request does not include both tokens, the server disallows the request.</span></span>

<span data-ttu-id="337b0-135">Tady je příklad z formuláře HTML s tokenem skryté formuláře:</span><span class="sxs-lookup"><span data-stu-id="337b0-135">Here is an example of an HTML form with a hidden form token:</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

<span data-ttu-id="337b0-136">Tokenů proti padělání pracovat, protože škodlivý stránku nelze číst tokeny uživatele z důvodu zásadami stejného původu.</span><span class="sxs-lookup"><span data-stu-id="337b0-136">Anti-forgery tokens work because the malicious page cannot read the user's tokens, due to same-origin policies.</span></span> <span data-ttu-id="337b0-137">([Zásadami stejného původu](http://www.w3.org/Security/wiki/Same_Origin_Policy) zabránit dokumenty hostovaný na dvou různých lokalit v přístupu k obsahu druhé strany.</span><span class="sxs-lookup"><span data-stu-id="337b0-137">([Same-origin policies](http://www.w3.org/Security/wiki/Same_Origin_Policy) prevent documents hosted on two different sites from accessing each other's content.</span></span> <span data-ttu-id="337b0-138">V předchozím příkladu se zlými úmysly stránky mohou odesílat žádosti example.com, takže ho nejde přečíst odpověď.)</span><span class="sxs-lookup"><span data-stu-id="337b0-138">So in the earlier example, the malicious page can send requests to example.com, but it cannot read the response.)</span></span>

<span data-ttu-id="337b0-139">Chcete-li zabránit útokům CSRF, pomocí tokenů proti padělání pomocí libovolného protokolu pro ověřování, kde tiše prohlížeč odesílá pověření po přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="337b0-139">To prevent CSRF attacks, use anti-forgery tokens with any authentication protocol where the browser silently sends credentials after the user logs in.</span></span> <span data-ttu-id="337b0-140">To zahrnuje protokoly pro ověřování na základě souboru cookie, jako je například ověřování pomocí formulářů, jakož i protokoly, jako jsou ověřování Basic a Digest.</span><span class="sxs-lookup"><span data-stu-id="337b0-140">This includes cookie-based authentication protocols, such as forms authentication, as well as protocols such as Basic and Digest authentication.</span></span>

<span data-ttu-id="337b0-141">Je potřeba tokenů proti padělání pro jakékoli nonsafe metody (POST, PUT, DELETE).</span><span class="sxs-lookup"><span data-stu-id="337b0-141">You should require anti-forgery tokens for any nonsafe methods (POST, PUT, DELETE).</span></span> <span data-ttu-id="337b0-142">Také se ujistěte, že bezpečné metody (GET, HEAD) nemají žádné vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="337b0-142">Also, make sure that safe methods (GET, HEAD) do not have any side effects.</span></span> <span data-ttu-id="337b0-143">Kromě toho pokud povolíte podporu napříč doménami, jako je například CORS a JSONP, pak i bezpečné metody, například GET jsou potenciálně zranitelná vůči útokům CSRF, mu umožní načíst potenciálně citlivá data.</span><span class="sxs-lookup"><span data-stu-id="337b0-143">Moreover, if you enable cross-domain support, such as CORS or JSONP, then even safe methods like GET are potentially vulnerable to CSRF attacks, allowing the attacker to read potentially sensitive data.</span></span>

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a><span data-ttu-id="337b0-144">Tokeny proti zfalšování v architektuře ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="337b0-144">Anti-Forgery Tokens in ASP.NET MVC</span></span>

<span data-ttu-id="337b0-145">Chcete-li přidat tokeny proti zfalšování na stránku Razor, použijte **HtmlHelper.AntiForgeryToken** pomocnou metodu:</span><span class="sxs-lookup"><span data-stu-id="337b0-145">To add the anti-forgery tokens to a Razor page, use the **HtmlHelper.AntiForgeryToken** helper method:</span></span>

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

<span data-ttu-id="337b0-146">Tato metoda přidá skryté pole formuláře a také nastaví token souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="337b0-146">This method adds the hidden form field and also sets the cookie token.</span></span>

## <a name="anti-csrf-and-ajax"></a><span data-ttu-id="337b0-147">Anti-útokům CSRF a AJAX</span><span class="sxs-lookup"><span data-stu-id="337b0-147">Anti-CSRF and AJAX</span></span>

<span data-ttu-id="337b0-148">Token formuláře může být problém pro odesílání požadavků AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="337b0-148">The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="337b0-149">Jedním z řešení je odesílat tokeny ve vlastní hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="337b0-149">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="337b0-150">Následující kód používá syntaxi Razor ke generování tokenů a pak přidá tokenů pro požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="337b0-150">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> <span data-ttu-id="337b0-151">Tokeny jsou generovány na serveru voláním **AntiForgery.GetTokens**.</span><span class="sxs-lookup"><span data-stu-id="337b0-151">The tokens are generated at the server by calling **AntiForgery.GetTokens**.</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

<span data-ttu-id="337b0-152">Při zpracování požadavku extrahujte tokeny v hlavičce požadavku.</span><span class="sxs-lookup"><span data-stu-id="337b0-152">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="337b0-153">Zavolejte **AntiForgery.Validate** metodu za účelem ověření tokenů.</span><span class="sxs-lookup"><span data-stu-id="337b0-153">Then call the **AntiForgery.Validate** method to validate the tokens.</span></span> <span data-ttu-id="337b0-154">**Ověřit** metoda vyvolá výjimku, pokud tokeny nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="337b0-154">The **Validate** method throws an exception if the tokens are not valid.</span></span>

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
