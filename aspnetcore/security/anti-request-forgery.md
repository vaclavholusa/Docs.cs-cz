---
title: "Prevence útoků (XSRF/proti útokům CSRF) padělání požadavku posílaného mezi weby v ASP.NET Core"
author: steve-smith
description: "Prevence útoků (XSRF/proti útokům CSRF) padělání požadavku posílaného mezi weby v ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: e076e301004c04b5c516d775353a4b6e50a3f36e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="c4c07-103">Prevence útoků (XSRF/proti útokům CSRF) padělání požadavku posílaného mezi weby v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4c07-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="c4c07-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4c07-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="c4c07-105">Jaké útoku proti padělání zabránit?</span><span class="sxs-lookup"><span data-stu-id="c4c07-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="c4c07-106">Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF, vyslovováno *najdete procházení*) je útok na hostované webové aplikace, které můžete vytvořit web ovlivnit interakce mezi prohlížeče klienta a webová stránka, která důvěřuje Tento prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c4c07-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="c4c07-107">Tyto útoky jsou možné, protože webových prohlížečů odesílat některé typy ověřování tokenů automaticky s každou žádost na web.</span><span class="sxs-lookup"><span data-stu-id="c4c07-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="c4c07-108">Tato forma zneužití je taky říká *jedním kliknutím útoku* nebo jako *relace jízda*, protože je útok trvá výhod uživatel ověřený relace.</span><span class="sxs-lookup"><span data-stu-id="c4c07-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="c4c07-109">Příklad útoku proti útokům CSRF:</span><span class="sxs-lookup"><span data-stu-id="c4c07-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="c4c07-110">Přihlášení uživatele do `www.example.com`, ověřování pomocí formulářů.</span><span class="sxs-lookup"><span data-stu-id="c4c07-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="c4c07-111">Server ověřuje uživatele a vydá odpovědi, která obsahuje soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="c4c07-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="c4c07-112">Uživatel navštíví škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="c4c07-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="c4c07-113">Škodlivé weby obsahuje formuláře HTML, který je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="c4c07-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="c4c07-114">Všimněte si, že akce formuláře požadavky na server, snadno napadnutelný, aby škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="c4c07-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="c4c07-115">Toto je část "webů" proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="c4c07-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="c4c07-116">Uživatel kliknutím na tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="c4c07-116">The user clicks the submit button.</span></span> <span data-ttu-id="c4c07-117">V prohlížeči automaticky zahrne ověřovacího souboru cookie pro požadovanou doménu (citlivé lokality v tomto případě) s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="c4c07-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="c4c07-118">Žádost se spustí na serveru s kontext ověřování uživatele a můžete provádět žádnou akci, která ověřený uživatel může provádět.</span><span class="sxs-lookup"><span data-stu-id="c4c07-118">The request runs on the server with the user's authentication context and can perform any action that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="c4c07-119">Tento příklad vyžaduje, aby uživatel klepnutím na tlačítko formuláře.</span><span class="sxs-lookup"><span data-stu-id="c4c07-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="c4c07-120">Na stránce škodlivého může:</span><span class="sxs-lookup"><span data-stu-id="c4c07-120">The malicious page could:</span></span>

* <span data-ttu-id="c4c07-121">Spusťte skript, který automaticky odešle formulář.</span><span class="sxs-lookup"><span data-stu-id="c4c07-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="c4c07-122">Odešle odeslání formuláře jako požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="c4c07-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="c4c07-123">Skrytá formuláře pomocí šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="c4c07-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="c4c07-124">Použití protokolu SSL nezabrání útoku proti útokům CSRF, může odesílat škodlivé weby `https://` požadavku.</span><span class="sxs-lookup"><span data-stu-id="c4c07-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="c4c07-125">Některými útoky cílové lokality koncové body, které reagují na `GET` požadavků, ve kterých případ značka obrázku lze použít k provedení akce (Tato forma útoku je běžné v lokalitách fórum, které umožňují bitové kopie, ale blokovat JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c4c07-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="c4c07-126">Aplikace, které změní stav s `GET` požadavky se stát terčem před nebezpečnými útoky.</span><span class="sxs-lookup"><span data-stu-id="c4c07-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="c4c07-127">Útoky proti útokům CSRF je možné proti webové stránky, které používají soubory cookie pro ověřování, protože prohlížeče odesílají všechny relevantní soubory cookie k cílovému webu.</span><span class="sxs-lookup"><span data-stu-id="c4c07-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="c4c07-128">Útoky proti útokům CSRF však nejsou omezeny na zneužitím soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="c4c07-129">Ověřování Basic a Digest jsou například také snadno napadnutelný.</span><span class="sxs-lookup"><span data-stu-id="c4c07-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="c4c07-130">Po přihlášení uživatele pomocí ověřování základní nebo Digest, v prohlížeči automaticky odesílá přihlašovací údaje až do ukončení relace.</span><span class="sxs-lookup"><span data-stu-id="c4c07-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="c4c07-131">Poznámka: V tomto kontextu *relace* odkazuje na straně klienta relace, během které je uživatel ověřený.</span><span class="sxs-lookup"><span data-stu-id="c4c07-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="c4c07-132">Je relace na straně serveru, které nejsou nebo [relace middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="c4c07-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="c4c07-133">Uživatelé můžou chránit proti ohrožení zabezpečení proti útokům CSRF pomocí:</span><span class="sxs-lookup"><span data-stu-id="c4c07-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="c4c07-134">Protokolování z webových stránek, při jejich dokončení jejich používání.</span><span class="sxs-lookup"><span data-stu-id="c4c07-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="c4c07-135">Pravidelně vymazání svého prohlížeče soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="c4c07-136">Ohrožení zabezpečení proti útokům CSRF jsou však zásadně problém s webovou aplikaci, ne koncový uživatel.</span><span class="sxs-lookup"><span data-stu-id="c4c07-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="c4c07-137">Jak ASP.NET MVC základní adresa proti útokům CSRF?</span><span class="sxs-lookup"><span data-stu-id="c4c07-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="c4c07-138">ASP.NET Core implementuje pomocí proti request padělání [zásobníku ochrany dat ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="c4c07-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="c4c07-139">Ochrana dat ASP.NET Core musí být nakonfigurováno pro práci v serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="c4c07-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="c4c07-140">V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="c4c07-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="c4c07-141">Konfigurace ochrany dat proti request padělání výchozí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4c07-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="c4c07-142">V technologii ASP.NET 2.0 MVC jádra [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) vloží tokeny proti zfalšování pro prvků formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="c4c07-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="c4c07-143">Například následující kód v souboru nástroje Razor bude automaticky generovat tokeny proti zfalšování:</span><span class="sxs-lookup"><span data-stu-id="c4c07-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="c4c07-144">Automatické generování tokenů proti padělání prvků formuláře HTML se stane, když:</span><span class="sxs-lookup"><span data-stu-id="c4c07-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="c4c07-145">`form` Značka obsahuje `method="post"` atribut a</span><span class="sxs-lookup"><span data-stu-id="c4c07-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="c4c07-146">Atribut akce je prázdný.</span><span class="sxs-lookup"><span data-stu-id="c4c07-146">The action attribute is empty.</span></span> <span data-ttu-id="c4c07-147">( `action=""`) NEBO</span><span class="sxs-lookup"><span data-stu-id="c4c07-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="c4c07-148">Není zadána atributu akce.</span><span class="sxs-lookup"><span data-stu-id="c4c07-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="c4c07-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="c4c07-149">(`<form method="post">`)</span></span>

<span data-ttu-id="c4c07-150">Můžete zakázat automatické generování tokenů proti padělání pro prvků formuláře HTML pomocí:</span><span class="sxs-lookup"><span data-stu-id="c4c07-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="c4c07-151">Výslovně zakazuje `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="c4c07-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="c4c07-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="c4c07-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="c4c07-153">OPT element form mimo značku Pomocníci pomocí pomocné rutiny značka [! výslovný nesouhlas s symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="c4c07-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="c4c07-154">Odeberte `FormTagHelper` ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c4c07-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="c4c07-155">Můžete odebrat `FormTagHelper` ze zobrazení přidáním následující direktivu Razor zobrazení:</span><span class="sxs-lookup"><span data-stu-id="c4c07-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="c4c07-156">[Stránky Razor](xref:mvc/razor-pages/index) jsou automaticky chráněny před XSRF nebo proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="c4c07-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="c4c07-157">Nemáte k zápisu žádný další kód.</span><span class="sxs-lookup"><span data-stu-id="c4c07-157">You don't have to write any additional code.</span></span> <span data-ttu-id="c4c07-158">V tématu [XSRF/proti útokům CSRF a stránky Razor](xref:mvc/razor-pages/index#xsrf) Další informace.</span><span class="sxs-lookup"><span data-stu-id="c4c07-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="c4c07-159">Nejběžnější přístup k obraně proti útokům proti útokům CSRF je token vzor synchronizátor (STP).</span><span class="sxs-lookup"><span data-stu-id="c4c07-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="c4c07-160">STP je technika používá, když uživatel požádá o stránku s dat formuláře.</span><span class="sxs-lookup"><span data-stu-id="c4c07-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="c4c07-161">Server odešle token spojené s identitou aktuálního uživatele do klienta.</span><span class="sxs-lookup"><span data-stu-id="c4c07-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="c4c07-162">Klient odešle zpět token k serveru pro ověření.</span><span class="sxs-lookup"><span data-stu-id="c4c07-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="c4c07-163">Pokud server přijme token, který neodpovídá identitu ověřeného uživatele, požadavek byl odmítnut.</span><span class="sxs-lookup"><span data-stu-id="c4c07-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="c4c07-164">Token je jedinečný a nepředvídatelným.</span><span class="sxs-lookup"><span data-stu-id="c4c07-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="c4c07-165">Token mohou sloužit také zajistit správné pořadí řady požadavků (stránka 2, která předchází stránka 3 předchází zajistit stránka 1).</span><span class="sxs-lookup"><span data-stu-id="c4c07-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="c4c07-166">Všechny formuláře v šablony ASP.NET Core MVC generování antiforgery tokenů.</span><span class="sxs-lookup"><span data-stu-id="c4c07-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="c4c07-167">Následující dva příklady zobrazení logiky generování antiforgery tokenů:</span><span class="sxs-lookup"><span data-stu-id="c4c07-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="c4c07-168">Můžete explicitně přidat antiforgery token ``<form>`` element bez použití značky Pomocníci s pomocné rutiny HTML ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="c4c07-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="c4c07-169">V každém z předchozích případech se přidá ASP.NET Core skryté pole formuláře podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="c4c07-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="c4c07-170">ASP.NET Core obsahuje tři [filtry](xref:mvc/controllers/filters) pro práci s antiforgery tokeny: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, a ``IgnoreAntiforgeryToken``.</span><span class="sxs-lookup"><span data-stu-id="c4c07-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="c4c07-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="c4c07-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="c4c07-172">``ValidateAntiForgeryToken`` Je filtr akce, který lze použít na jednotlivé akci, kontroler, nebo globálně.</span><span class="sxs-lookup"><span data-stu-id="c4c07-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="c4c07-173">Požadavky na akce, které tento filtr použít se zablokuje, pokud požadavek obsahuje platný antiforgery token.</span><span class="sxs-lookup"><span data-stu-id="c4c07-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="c4c07-174">``ValidateAntiForgeryToken`` Atribut vyžaduje, aby token pro požadavky na metody akce upraví, včetně `HTTP GET` požadavky.</span><span class="sxs-lookup"><span data-stu-id="c4c07-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="c4c07-175">Pokud použijete ho široce, můžete přepsat její ``IgnoreAntiforgeryToken`` atribut.</span><span class="sxs-lookup"><span data-stu-id="c4c07-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="c4c07-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="c4c07-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="c4c07-177">Aplikace ASP.NET Core obecně negenerovat antiforgery tokeny pro bezpečné metody HTTP (GET, HEAD, možnosti a trasování).</span><span class="sxs-lookup"><span data-stu-id="c4c07-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="c4c07-178">Místo použití široce ``ValidateAntiForgeryToken`` atribut a pak jeho pomocí přepsání ``IgnoreAntiforgeryToken`` atributy, můžete použít ``AutoValidateAntiforgeryToken`` atribut.</span><span class="sxs-lookup"><span data-stu-id="c4c07-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="c4c07-179">Tento atribut funguje stejně jako na ``ValidateAntiForgeryToken`` atribut, s tím rozdílem, že není třeba tokeny na požadavky vytvořené pomocí následujících metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="c4c07-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="c4c07-180">GET</span><span class="sxs-lookup"><span data-stu-id="c4c07-180">GET</span></span>
* <span data-ttu-id="c4c07-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="c4c07-181">HEAD</span></span>
* <span data-ttu-id="c4c07-182">MOŽNOSTI</span><span class="sxs-lookup"><span data-stu-id="c4c07-182">OPTIONS</span></span>
* <span data-ttu-id="c4c07-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="c4c07-183">TRACE</span></span>

<span data-ttu-id="c4c07-184">Doporučujeme použít ``AutoValidateAntiforgeryToken`` široce pro scénáře – rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c4c07-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="c4c07-185">Tím se zajistí, že jsou ve výchozím nastavení chráněna vaše akce POST.</span><span class="sxs-lookup"><span data-stu-id="c4c07-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="c4c07-186">Alternativou je ignorovat antiforgery tokeny ve výchozím nastavení, pokud ``ValidateAntiForgeryToken`` se použije k metodě jednotlivé akce.</span><span class="sxs-lookup"><span data-stu-id="c4c07-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="c4c07-187">Je pravděpodobnější v tomto scénáři pro metodu POST akce na levé straně nechráněné, a aplikace bude zranitelný vůči útoku proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="c4c07-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="c4c07-188">I anonymní PŘÍSPĚVCÍCH by měli poslat antiforgery token.</span><span class="sxs-lookup"><span data-stu-id="c4c07-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="c4c07-189">Poznámka: Rozhraní API nemáte automatický mechanismus pro odesílání bez souborů cookie součást tokenu; implementace pravděpodobně závisí na implementaci kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="c4c07-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="c4c07-190">Níže jsou uvedeny některé příklady.</span><span class="sxs-lookup"><span data-stu-id="c4c07-190">Some examples are shown below.</span></span>


<span data-ttu-id="c4c07-191">Příklad (třída úroveň):</span><span class="sxs-lookup"><span data-stu-id="c4c07-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="c4c07-192">Příklad (globální):</span><span class="sxs-lookup"><span data-stu-id="c4c07-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="c4c07-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="c4c07-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="c4c07-194">``IgnoreAntiforgeryToken`` Filtru se používá k vyloučení potřeby token antiforgery být pro danou akci (nebo řadič).</span><span class="sxs-lookup"><span data-stu-id="c4c07-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="c4c07-195">Při použití, přepíše tento filtr ``ValidateAntiForgeryToken`` nebo ``AutoValidateAntiforgeryToken`` filtry zadané na vyšší úrovni (globálně nebo na řadič).</span><span class="sxs-lookup"><span data-stu-id="c4c07-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="c4c07-196">JavaScript AJAX a SPA</span><span class="sxs-lookup"><span data-stu-id="c4c07-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="c4c07-197">V tradiční aplikace založené na HTML antiforgery tokeny jsou předána serveru pomocí skrytého pole.</span><span class="sxs-lookup"><span data-stu-id="c4c07-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="c4c07-198">Moderní aplikace založené na jazyce JavaScript a jednostránkové aplikace (SPA) jsou vytvářeny požadavky mnoho prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="c4c07-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="c4c07-199">Tyto požadavky AJAX může používat další metody (například hlavičky požadavku nebo soubory cookie) k odesílání token.</span><span class="sxs-lookup"><span data-stu-id="c4c07-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="c4c07-200">Pokud se soubory cookie používají k ukládání tokeny ověřování a k ověřování žádostí o rozhraní API na serveru, proti útokům CSRF bude na potenciální problém.</span><span class="sxs-lookup"><span data-stu-id="c4c07-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="c4c07-201">Ale pokud místní úložiště se používá k ukládání tokenu, ohrožení zabezpečení proti útokům CSRF může být omezeny, vzhledem k tomu, že hodnoty z místního úložiště nejsou automaticky odesílá na server s každou novou žádost.</span><span class="sxs-lookup"><span data-stu-id="c4c07-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="c4c07-202">Proto používá místní úložiště k uložení antiforgery token do klienta a odeslání tokenu, protože hlavička požadavku je doporučený postup.</span><span class="sxs-lookup"><span data-stu-id="c4c07-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="c4c07-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="c4c07-203">AngularJS</span></span>

<span data-ttu-id="c4c07-204">AngularJS používá vytváření adresu proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="c4c07-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="c4c07-205">Pokud server odešle soubor cookie s názvem ``XSRF-TOKEN``, úhlová ``$http`` služba přidá hodnotu z tento soubor cookie do záhlaví při odesílání požadavku na tento server.</span><span class="sxs-lookup"><span data-stu-id="c4c07-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="c4c07-206">Tento proces je automatické; nemusíte explicitně nastaven záhlaví.</span><span class="sxs-lookup"><span data-stu-id="c4c07-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="c4c07-207">Název hlavičky je ``X-XSRF-TOKEN``.</span><span class="sxs-lookup"><span data-stu-id="c4c07-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="c4c07-208">Server by měl zjistit tuto hlavičku a ověřit jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="c4c07-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="c4c07-209">Pro rozhraní API ASP.NET Core ve spolupráci s touto konvencí:</span><span class="sxs-lookup"><span data-stu-id="c4c07-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="c4c07-210">Konfigurace aplikace k poskytování token v souboru cookie s názvem``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="c4c07-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="c4c07-211">Konfigurovat službu antiforgery hledání hlavičku s názvem``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="c4c07-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="c4c07-212">[Ukázkové zobrazení](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="c4c07-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="c4c07-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="c4c07-213">JavaScript</span></span>

<span data-ttu-id="c4c07-214">Pomocí jazyka JavaScript se zobrazeními, můžete vytvořit token pomocí služby z v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c4c07-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="c4c07-215">Uděláte to tak, Vložit `Microsoft.AspNetCore.Antiforgery.IAntiforgery` služby do zobrazení a volání `GetAndStoreTokens`, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c4c07-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="c4c07-216">Tento přístup se eliminuje potřeba k řešení přímo nastavení souborů cookie ze serveru nebo jejich čtení z klienta.</span><span class="sxs-lookup"><span data-stu-id="c4c07-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="c4c07-217">JavaScript můžete také přístup k tokeny, které jsou součástí soubory cookie a pak použít obsah souboru cookie pro vytvoření záhlaví s hodnotou tokenu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c4c07-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="c4c07-218">Pak za předpokladu, že vytvoříte svůj skript požadavky odeslat token v hlavičce názvem ``X-CSRF-TOKEN``, konfigurovat službu antiforgery hledání ``X-CSRF-TOKEN`` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="c4c07-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="c4c07-219">Následující příklad používá jQuery požadavek AJAX s příslušnou hlavičky:</span><span class="sxs-lookup"><span data-stu-id="c4c07-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="c4c07-220">Konfigurace Antiforgery</span><span class="sxs-lookup"><span data-stu-id="c4c07-220">Configuring Antiforgery</span></span>

<span data-ttu-id="c4c07-221">`IAntiforgery`poskytuje rozhraní API pro konfiguraci antiforgery systému.</span><span class="sxs-lookup"><span data-stu-id="c4c07-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="c4c07-222">Může být požadována v `Configure` metodu `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="c4c07-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="c4c07-223">Následující příklad používá middleware z aplikace domovské stránky k vygenerování tokenu antiforgery a jeho odeslání v odpovědi jako soubor cookie (pomocí výchozí úhlová konvence popsané výše):</span><span class="sxs-lookup"><span data-stu-id="c4c07-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="c4c07-224">Možnosti</span><span class="sxs-lookup"><span data-stu-id="c4c07-224">Options</span></span>

<span data-ttu-id="c4c07-225">Můžete přizpůsobit [antiforgery možnosti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c4c07-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="c4c07-226">Možnost</span><span class="sxs-lookup"><span data-stu-id="c4c07-226">Option</span></span>        | <span data-ttu-id="c4c07-227">Popis</span><span class="sxs-lookup"><span data-stu-id="c4c07-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="c4c07-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="c4c07-228">CookieDomain</span></span>  | <span data-ttu-id="c4c07-229">Doména souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-229">The domain of the cookie.</span></span> <span data-ttu-id="c4c07-230">Použije se výchozí hodnota `null`.</span><span class="sxs-lookup"><span data-stu-id="c4c07-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="c4c07-231">CookieName</span><span class="sxs-lookup"><span data-stu-id="c4c07-231">CookieName</span></span>    | <span data-ttu-id="c4c07-232">Název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-232">The name of the cookie.</span></span> <span data-ttu-id="c4c07-233">Pokud není nastavena, systém bude generovat jedinečný název začíná `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="c4c07-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="c4c07-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="c4c07-234">CookiePath</span></span>    | <span data-ttu-id="c4c07-235">Cesta, nastavte v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="c4c07-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="c4c07-236">FormFieldName</span></span> | <span data-ttu-id="c4c07-237">Název pole Skrytá formuláře antiforgery systém použije k vykreslení antiforgery tokeny v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="c4c07-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="c4c07-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="c4c07-238">HeaderName</span></span>    | <span data-ttu-id="c4c07-239">Název hlavičky, která používá antiforgery systém.</span><span class="sxs-lookup"><span data-stu-id="c4c07-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="c4c07-240">Pokud `null`, systém bude považovat jenom data formuláře.</span><span class="sxs-lookup"><span data-stu-id="c4c07-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="c4c07-241">requireSsl</span><span class="sxs-lookup"><span data-stu-id="c4c07-241">RequireSsl</span></span>    | <span data-ttu-id="c4c07-242">Určuje, zda je požadován protokol SSL antiforgery systémem.</span><span class="sxs-lookup"><span data-stu-id="c4c07-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="c4c07-243">Použije se výchozí hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="c4c07-243">Defaults to `false`.</span></span> <span data-ttu-id="c4c07-244">Pokud `true`, bez SSL požadavky nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c4c07-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="c4c07-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="c4c07-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="c4c07-246">Určuje, jestli se má potlačit generování `X-Frame-Options` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="c4c07-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="c4c07-247">Ve výchozím nastavení je generována záhlaví s hodnotou "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="c4c07-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="c4c07-248">Použije se výchozí hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="c4c07-248">Defaults to `false`.</span></span> |

<span data-ttu-id="c4c07-249">V tématu https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Další informace.</span><span class="sxs-lookup"><span data-stu-id="c4c07-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="c4c07-250">Rozšíření Antiforgery</span><span class="sxs-lookup"><span data-stu-id="c4c07-250">Extending Antiforgery</span></span>

<span data-ttu-id="c4c07-251">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typ mohou vývojáři rozšířit chování systému anti-XSRF odezvy další data v každý token.</span><span class="sxs-lookup"><span data-stu-id="c4c07-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="c4c07-252">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metoda je volána pokaždé, když se vygeneruje token pole a vložené návratovou hodnotu v rámci vygenerovaný token.</span><span class="sxs-lookup"><span data-stu-id="c4c07-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="c4c07-253">Implementátor může vrátit časové razítko, hodnotu nonce nebo jakoukoli jinou hodnotu a pak zavolají [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) ověřit tato data při ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="c4c07-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="c4c07-254">Uživatelské jméno klienta je již vložených do generovaných tokenů, takže není nutné zahrnout tyto informace.</span><span class="sxs-lookup"><span data-stu-id="c4c07-254">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="c4c07-255">Pokud token obsahuje dodatečná data, ale ne `IAntiForgeryAdditionalDataProvider` byl nakonfigurován, nejsou dodatečná data ověřena.</span><span class="sxs-lookup"><span data-stu-id="c4c07-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="c4c07-256">Základy</span><span class="sxs-lookup"><span data-stu-id="c4c07-256">Fundamentals</span></span>

<span data-ttu-id="c4c07-257">Útoky proti útokům CSRF spoléhají na výchozí chování prohlížeče odeslat soubory cookie přidružené k doméně s každou žádost odeslanou do této domény.</span><span class="sxs-lookup"><span data-stu-id="c4c07-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="c4c07-258">Tyto soubory cookie jsou uloženy v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c4c07-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="c4c07-259">Často obsahují soubory cookie relace pro ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4c07-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="c4c07-260">Ověřování na základě souborů cookie je Oblíbené formu ověřování.</span><span class="sxs-lookup"><span data-stu-id="c4c07-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="c4c07-261">Ověřování na základě tokenu systémy mají byla popularita, hlavně pro SPA a v dalších scénářích "inteligentní klient".</span><span class="sxs-lookup"><span data-stu-id="c4c07-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="c4c07-262">Ověřování na základě souborů cookie</span><span class="sxs-lookup"><span data-stu-id="c4c07-262">Cookie-based authentication</span></span>

<span data-ttu-id="c4c07-263">Jakmile uživatel byl ověřen pomocí uživatelského jména a hesla, budou se vystaví token, který slouží k jejich identifikaci a ověření, které byly ověřeny.</span><span class="sxs-lookup"><span data-stu-id="c4c07-263">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="c4c07-264">Token je uloženo jako soubor cookie, který doprovází každý požadavek klienta umožňuje.</span><span class="sxs-lookup"><span data-stu-id="c4c07-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="c4c07-265">Generování a ověřování tento soubor cookie je potřeba middleware ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="c4c07-266">Základní technologie ASP.NET poskytuje soubor cookie [middleware](../fundamentals/middleware.md) který serializuje hlavní název uživatele do šifrovaného souboru cookie a pak na následné žádosti, ověří souboru cookie, znovu vytvoří objekt a přiřadí ji k `User` vlastnost `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c4c07-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="c4c07-267">Pokud soubor cookie se používá, ověřovacího souboru cookie je kontejner pro ověřovací lístek.</span><span class="sxs-lookup"><span data-stu-id="c4c07-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="c4c07-268">Lístek se předá jako hodnota souboru cookie pro ověřování formulářů s každou žádostí a ověřování pomocí formulářů, na serveru, používá k identifikaci ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4c07-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="c4c07-269">Když je uživatel přihlášen do systému, uživatelské relace je vytvořena na straně serveru a je uložen v databázi nebo jiných trvalého úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4c07-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="c4c07-270">Systém vygeneruje klíč relace, která odkazuje na skutečnou relace v úložišti dat a pak se odešle jako soubor cookie straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c4c07-270">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="c4c07-271">Webový server zkontroluje tento klíč relace vždy, když uživatel požádá o prostředek, který vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="c4c07-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="c4c07-272">Systém kontroluje, zda relace uživatele má oprávnění k přístupu k požadovanému zdroji.</span><span class="sxs-lookup"><span data-stu-id="c4c07-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="c4c07-273">Pokud ano, pokračuje v požadavku.</span><span class="sxs-lookup"><span data-stu-id="c4c07-273">If so, the request continues.</span></span> <span data-ttu-id="c4c07-274">Žádost, jinak vrátí jako není autorizovaný.</span><span class="sxs-lookup"><span data-stu-id="c4c07-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="c4c07-275">V tento postup soubory cookie se použijí zpřístupnění aplikace se zdají být stavová, vzhledem k tomu, že je schopna "nezapomeňte", má uživatel ověřený se serverem.</span><span class="sxs-lookup"><span data-stu-id="c4c07-275">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="c4c07-276">Tokeny uživatele</span><span class="sxs-lookup"><span data-stu-id="c4c07-276">User tokens</span></span>

<span data-ttu-id="c4c07-277">Ověřování na základě tokenu není relace uložit na server.</span><span class="sxs-lookup"><span data-stu-id="c4c07-277">Token-based authentication doesn't store session on the server.</span></span> <span data-ttu-id="c4c07-278">Když je uživatel přihlášen, že se vystaví token, (ne antiforgery token).</span><span class="sxs-lookup"><span data-stu-id="c4c07-278">When a user is logged in, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="c4c07-279">Tento token obsahuje data, která je nutné k ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="c4c07-279">This token holds the data that's required to validate the token.</span></span> <span data-ttu-id="c4c07-280">Také obsahuje informace o uživateli ve formě [deklarace identity](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="c4c07-280">It also contains user information in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="c4c07-281">Pokud chce uživatel pro přístup k prostředkům serveru, které vyžadují ověřování, token je odeslána na server s hlavičku další ověřování ve formuláři nosiče {token}.</span><span class="sxs-lookup"><span data-stu-id="c4c07-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="c4c07-282">Díky aplikaci bezstavové vzhledem k tomu, že v každé následné žádosti o token je předán v požadavku pro ověřování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="c4c07-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="c4c07-283">Tento token není *šifrované*; je spíše *kódovaný*.</span><span class="sxs-lookup"><span data-stu-id="c4c07-283">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="c4c07-284">Na straně serveru může dekódovat tokenu pro přístup k nezpracované informace v tokenu.</span><span class="sxs-lookup"><span data-stu-id="c4c07-284">On the server-side, the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="c4c07-285">Odeslat token v následných žádostí, buď uložit v místním úložišti v prohlížeči nebo v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-285">To send the token in subsequent requests, either store it in the browser's local storage or in a cookie.</span></span> <span data-ttu-id="c4c07-286">Nemusíte si dělat starosti o ohrožení zabezpečení XSRF Pokud token je uložený v místním úložišti, ale je problém, pokud je token je uložen v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="c4c07-286">Don't worry about XSRF vulnerability if the token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="c4c07-287">Více aplikací, které jsou hostované v jedné doméně</span><span class="sxs-lookup"><span data-stu-id="c4c07-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="c4c07-288">I když `example1.cloudapp.net` a `example2.cloudapp.net` jsou různých hostitelích, je vztah implicitní vztah důvěryhodnosti mezi hostiteli v rámci `*.cloudapp.net` domény.</span><span class="sxs-lookup"><span data-stu-id="c4c07-288">Although `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="c4c07-289">Tento vztah důvěryhodnosti implicitní umožňuje potenciálně nedůvěryhodní hostitelé ovlivnit vzájemně soubory cookie (stejného původu zásady, které řídí požadavky AJAX nemáte platit pro soubory cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="c4c07-289">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="c4c07-290">Modul runtime ASP.NET Core poskytuje některé zmírnění v tom, že uživatelské jméno se vloží do tokenu pole.</span><span class="sxs-lookup"><span data-stu-id="c4c07-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token.</span></span> <span data-ttu-id="c4c07-291">I když je možné přepsat token relace škodlivý subdomény, nelze vygenerovat token pro daného uživatele platné pole.</span><span class="sxs-lookup"><span data-stu-id="c4c07-291">Even if a malicious subdomain is able to overwrite a session token, it can't generate a valid field token for the user.</span></span> <span data-ttu-id="c4c07-292">Když jsou hostované v takovém prostředí, rutiny předdefinované anti-XSRF stále nelze bránit proti zneužití relace nebo přihlášení proti útokům CSRF útoky.</span><span class="sxs-lookup"><span data-stu-id="c4c07-292">When hosted in such an environment, the built-in anti-XSRF routines still can't defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="c4c07-293">Sdílené hostitelská prostředí jsou vunerable zneužití relace, přihlášení proti útokům CSRF a jiným útokům.</span><span class="sxs-lookup"><span data-stu-id="c4c07-293">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="c4c07-294">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c4c07-294">Additional resources</span></span>

* <span data-ttu-id="c4c07-295">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="c4c07-295">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
