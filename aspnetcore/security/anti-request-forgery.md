---
title: Zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky v ASP.NET Core
author: steve-smith
description: Zjistit, jak zabránit útoky na webové aplikace, kde můžete škodlivou webovou stránku ovlivnit interakce mezi prohlížeče klienta a aplikace.
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
uid: security/anti-request-forgery
ms.openlocfilehash: a00bd4ff4b265a19766e54e6ad6b97b870df56c5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279594"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="a94cb-103">Zabránit webů požadavku padělání (XSRF/proti útokům CSRF) před útoky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a94cb-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="a94cb-104">Podle [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a94cb-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a94cb-105">Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF, vyslovováno *najdete procházení*) je útok na hostované webové aplikace, které škodlivý webové aplikace mohou mít vliv na interakce mezi prohlížeče klienta a webové aplikace, která důvěřuje, prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="a94cb-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="a94cb-106">Tyto útoky nejsou možné, protože webových prohlížečů odesílat některé typy ověřování tokenů automaticky s každou žádost na web.</span><span class="sxs-lookup"><span data-stu-id="a94cb-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="a94cb-107">Tato forma zneužití je také označován jako *jedním kliknutím útoku* nebo *relace jízda* protože útoku využívá uživatel je ověřen dříve relace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="a94cb-108">Příklad útoku proti útokům CSRF:</span><span class="sxs-lookup"><span data-stu-id="a94cb-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="a94cb-109">Uživatel se přihlásí do `www.good-banking-site.com` ověřování pomocí formulářů.</span><span class="sxs-lookup"><span data-stu-id="a94cb-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="a94cb-110">Server ověřuje uživatele a vydá odpovědi, která obsahuje soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="a94cb-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="a94cb-111">Web není bude zranitelný vůči útoku, protože důvěřovat jakékoli požadavky, které obdrží s platnou ověřovacího souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="a94cb-112">Uživatel navštíví škodlivé weby `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="a94cb-113">Škodlivé weby, `www.bad-crook-site.com`, obsahuje formuláře HTML, který je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="a94cb-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="a94cb-114">Všimněte si, že formuláře `action` příspěvky k větší zranitelnosti lokality, nikoli k škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="a94cb-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="a94cb-115">Toto je část "webů" proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="a94cb-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="a94cb-116">Uživatel vybere tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="a94cb-116">The user selects the submit button.</span></span> <span data-ttu-id="a94cb-117">Prohlížeč provede požadavek a automaticky zahrne ověřovacího souboru cookie pro požadovanou doménu `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="a94cb-118">Se spouští požadavek `www.good-banking-site.com` server s kontext ověřování uživatele a mohou provádět žádnou akci, kterou ověřený uživatel může provádět.</span><span class="sxs-lookup"><span data-stu-id="a94cb-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="a94cb-119">Kromě tohoto scénáře, které si uživatel vybere tlačítko pro odeslání formuláře může škodlivé weby:</span><span class="sxs-lookup"><span data-stu-id="a94cb-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="a94cb-120">Spusťte skript, který automaticky odešle formulář.</span><span class="sxs-lookup"><span data-stu-id="a94cb-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="a94cb-121">Odeslání formuláře odešlete jako požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="a94cb-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="a94cb-122">Skryjte formuláři pomocí šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="a94cb-122">Hide the form using CSS.</span></span>

<span data-ttu-id="a94cb-123">Tyto alternativní scénáře není zapotřebí žádné akce nebo vstup od uživatele než původně návštěvou škodlivé weby.</span><span class="sxs-lookup"><span data-stu-id="a94cb-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="a94cb-124">Pomocí protokolu HTTPS nezabrání útoku proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="a94cb-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="a94cb-125">Může odesílat škodlivé weby `https://www.good-banking-site.com/` stejným způsobem jako ho můžete odeslat požadavek nezabezpečené požadavku.</span><span class="sxs-lookup"><span data-stu-id="a94cb-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="a94cb-126">Některými útoky cílové koncové body, které reagují na požadavky GET, v takovém případě značka obrázku lze provést akci.</span><span class="sxs-lookup"><span data-stu-id="a94cb-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="a94cb-127">Tato forma útoku je běžné v lokalitách fórum, které umožňují bitové kopie, ale blokovat JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a94cb-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="a94cb-128">Aplikace, které změní stav pro požadavky GET, kde jsou změny proměnné nebo prostředky, jsou ohroženy útoky se zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="a94cb-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="a94cb-129">**Jsou nezabezpečené požadavky GET, které změní stav. Osvědčeným postupem je nikdy změny stavu na požadavek GET.**</span><span class="sxs-lookup"><span data-stu-id="a94cb-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="a94cb-130">Útoky proti útokům CSRF jsou možné u webových aplikací, které používají soubory cookie pro ověřování, protože:</span><span class="sxs-lookup"><span data-stu-id="a94cb-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="a94cb-131">Prohlížeče ukládají soubory cookie vydané webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="a94cb-132">Uložené soubory cookie obsahují soubory cookie relace pro ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="a94cb-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="a94cb-133">Prohlížeče odeslat všechny soubory cookie přiřazené doméně do webové aplikace každou žádost bez ohledu na to, jak byl vygenerován požadavek na aplikaci v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a94cb-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="a94cb-134">Nicméně nejsou proti útokům CSRF útoky omezené na zneužitím soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="a94cb-135">Ověřování Basic a Digest jsou například také snadno napadnutelný.</span><span class="sxs-lookup"><span data-stu-id="a94cb-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="a94cb-136">Jakmile se uživatel přihlásí pomocí ověřování základní nebo Digest, v prohlížeči automaticky odesílá přihlašovací údaje dokud relace&dagger; ukončí.</span><span class="sxs-lookup"><span data-stu-id="a94cb-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="a94cb-137">&dagger;V tomto kontextu *relace* odkazuje na straně klienta relace, během které je uživatel ověřený.</span><span class="sxs-lookup"><span data-stu-id="a94cb-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="a94cb-138">Je relace na straně serveru, které nejsou nebo [ASP.NET Core relace Middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="a94cb-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="a94cb-139">Uživatelé mohou chránit proti ohrožení zabezpečení proti útokům CSRF provedením opatření:</span><span class="sxs-lookup"><span data-stu-id="a94cb-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="a94cb-140">Přihlášení z webové aplikace po dokončení jejich používání.</span><span class="sxs-lookup"><span data-stu-id="a94cb-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="a94cb-141">Vymazat prohlížeče soubory cookie pravidelně.</span><span class="sxs-lookup"><span data-stu-id="a94cb-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="a94cb-142">Ohrožení zabezpečení proti útokům CSRF jsou však zásadně problém s webovou aplikaci, ne koncový uživatel.</span><span class="sxs-lookup"><span data-stu-id="a94cb-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="a94cb-143">Základy ověřování</span><span class="sxs-lookup"><span data-stu-id="a94cb-143">Authentication fundamentals</span></span>

<span data-ttu-id="a94cb-144">Ověřování na základě souborů cookie je Oblíbené formu ověřování.</span><span class="sxs-lookup"><span data-stu-id="a94cb-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="a94cb-145">Systémy ověřování na základě tokenu se popularita, hlavně pro jednostránkové aplikace (SPA).</span><span class="sxs-lookup"><span data-stu-id="a94cb-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="a94cb-146">Ověřování na základě souborů cookie</span><span class="sxs-lookup"><span data-stu-id="a94cb-146">Cookie-based authentication</span></span>

<span data-ttu-id="a94cb-147">Při ověření uživatele pomocí uživatelského jména a hesla, že se vystaví token, obsahující lístek ověřování, který slouží k ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="a94cb-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="a94cb-148">Token je uloženo jako soubor cookie, který doprovází každý požadavek klienta umožňuje.</span><span class="sxs-lookup"><span data-stu-id="a94cb-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="a94cb-149">Generování a ověřování tento soubor cookie provádí Middleware ověřování souborů Cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="a94cb-150">[Middleware](xref:fundamentals/middleware/index) serializuje hlavní název uživatele do šifrovaného souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="a94cb-151">Na následné žádosti, middleware ověří, že soubor cookie, znovu vytvoří objekt a přiřadí objekt k [uživatele](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) vlastnost [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="a94cb-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="a94cb-152">Ověřování na základě tokenu</span><span class="sxs-lookup"><span data-stu-id="a94cb-152">Token-based authentication</span></span>

<span data-ttu-id="a94cb-153">Pokud je uživatel ověřen, budou se vystaví token, (ne antiforgery token).</span><span class="sxs-lookup"><span data-stu-id="a94cb-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="a94cb-154">Token obsahuje uživatelské informace ve formě [deklarace identity](/dotnet/framework/security/claims-based-identity-model) nebo odkaz na token, který odkazuje na stav uživatele v aplikaci udržuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="a94cb-155">Když se uživatel pokusí o přístup k prostředku, které vyžadují ověřování, je token odesílají do aplikace s hlavičku další ověřování ve formuláři tokenu nosiče.</span><span class="sxs-lookup"><span data-stu-id="a94cb-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="a94cb-156">Díky tomu bezstavové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-156">This makes the app stateless.</span></span> <span data-ttu-id="a94cb-157">V každé následné žádosti o token je předán v požadavku pro ověřování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="a94cb-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="a94cb-158">Tento token není *šifrované*; má *kódovaný*.</span><span class="sxs-lookup"><span data-stu-id="a94cb-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="a94cb-159">Na serveru je token dekódovat pro přístup k jeho informace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="a94cb-160">Odeslat token na následné žádosti, ukládat do místního úložiště prohlížeče token.</span><span class="sxs-lookup"><span data-stu-id="a94cb-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="a94cb-161">Nemusíte dělat starosti o ohrožení zabezpečení proti útokům CSRF, pokud token je uložený v místním úložišti v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a94cb-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="a94cb-162">Proti útokům CSRF není vážný, pokud token je uložen v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="a94cb-163">Více aplikací, které jsou hostované v jedné doméně</span><span class="sxs-lookup"><span data-stu-id="a94cb-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="a94cb-164">Sdílené hostitelská prostředí jsou citlivé na zneužití relace přihlášení proti útokům CSRF a jiným útokům.</span><span class="sxs-lookup"><span data-stu-id="a94cb-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="a94cb-165">I když `example1.contoso.net` a `example2.contoso.net` jsou různých hostitelích, je vztah implicitní vztah důvěryhodnosti mezi hostiteli v rámci `*.contoso.net` domény.</span><span class="sxs-lookup"><span data-stu-id="a94cb-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="a94cb-166">Tento vztah důvěryhodnosti implicitní umožňuje potenciálně nedůvěryhodní hostitelé ovlivnit vzájemně soubory cookie (stejného původu zásady, které řídí požadavky AJAX nemáte platit pro soubory cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="a94cb-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="a94cb-167">Není sdílením domén se dá zabránit útokům, které využívají důvěryhodné soubory cookie mezi aplikacemi, které jsou hostované ve stejné doméně.</span><span class="sxs-lookup"><span data-stu-id="a94cb-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="a94cb-168">Při každé aplikace je hostované na vlastní domény, neexistuje žádný vztah důvěry implicitní souboru cookie zneužít.</span><span class="sxs-lookup"><span data-stu-id="a94cb-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="a94cb-169">Konfigurace antiforgery ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a94cb-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="a94cb-170">ASP.NET Core implementuje antiforgery pomocí [ochranu dat ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="a94cb-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a94cb-171">Zásobník ochrany dat musí být nakonfigurováno pro práci v serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="a94cb-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="a94cb-172">V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="a94cb-173">V technologii ASP.NET Core 2.0 nebo novější [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) vloží antiforgery tokeny do elementů formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="a94cb-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="a94cb-174">Následující kód v souboru nástroje Razor automaticky vygeneruje antiforgery tokenů:</span><span class="sxs-lookup"><span data-stu-id="a94cb-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="a94cb-175">Wijsova, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generuje tokeny antiforgery ve výchozím nastavení, pokud není formuláře metoda GET.</span><span class="sxs-lookup"><span data-stu-id="a94cb-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="a94cb-176">Automatické generování antiforgery tokeny pro prvků formuláře HTML se stane, když `<form>` značka obsahuje `method="post"` platí atribut a z následujících:</span><span class="sxs-lookup"><span data-stu-id="a94cb-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="a94cb-177">Atribut akce je prázdná (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="a94cb-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="a94cb-178">Není zadaný atribut akce (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="a94cb-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="a94cb-179">Je možné zakázat automatické generování antiforgery tokeny pro prvků formuláře HTML:</span><span class="sxs-lookup"><span data-stu-id="a94cb-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="a94cb-180">Explicitně zakázat antiforgery tokeny se `asp-antiforgery` atribut:</span><span class="sxs-lookup"><span data-stu-id="a94cb-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="a94cb-181">Form element je vyjádřit výslovný souhlas mimo značku Pomocníci pomocí pomocné rutiny značky [! výslovný nesouhlas s symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="a94cb-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="a94cb-182">Odeberte `FormTagHelper` ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a94cb-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="a94cb-183">`FormTagHelper` Můžete odstranit ze zobrazení přidáním následující direktiva do zobrazení Razor:</span><span class="sxs-lookup"><span data-stu-id="a94cb-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="a94cb-184">[Stránky Razor](xref:razor-pages/index) jsou automaticky chráněny před XSRF nebo proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="a94cb-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="a94cb-185">Další informace najdete v tématu [XSRF/proti útokům CSRF a stránky Razor](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="a94cb-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="a94cb-186">Nejběžnější přístupem k obraně proti útokům proti útokům CSRF je použití *tokenu vzor synchronizátor* (STP).</span><span class="sxs-lookup"><span data-stu-id="a94cb-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="a94cb-187">STP se používá, když uživatel požádá o stránku s dat formuláře:</span><span class="sxs-lookup"><span data-stu-id="a94cb-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="a94cb-188">Server odešle token spojené s identitou aktuálního uživatele do klienta.</span><span class="sxs-lookup"><span data-stu-id="a94cb-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="a94cb-189">Klient odešle zpět token k serveru pro ověření.</span><span class="sxs-lookup"><span data-stu-id="a94cb-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="a94cb-190">Pokud server přijme token, který neodpovídá identitu ověřeného uživatele, požadavek byl odmítnut.</span><span class="sxs-lookup"><span data-stu-id="a94cb-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="a94cb-191">Token je jedinečný a nepředvídatelným.</span><span class="sxs-lookup"><span data-stu-id="a94cb-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="a94cb-192">Token lze také zajistit správné pořadí řady požadavků (například zajistíte pořadí požadavek: stránka 1 &ndash; stránka 2 &ndash; stránka 3).</span><span class="sxs-lookup"><span data-stu-id="a94cb-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="a94cb-193">Všechny formuláře v ASP.NET MVC jádra a stránky Razor šablony generování antiforgery tokenů.</span><span class="sxs-lookup"><span data-stu-id="a94cb-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="a94cb-194">Následující pár příkladů, zobrazení generování antiforgery tokenů:</span><span class="sxs-lookup"><span data-stu-id="a94cb-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="a94cb-195">Explicitně přidat antiforgery token `<form>` element bez použití značky Pomocníci s pomocné rutiny HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="a94cb-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="a94cb-196">V každém z předchozích případech se přidá ASP.NET Core skryté pole formuláře podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="a94cb-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="a94cb-197">ASP.NET Core obsahuje tři [filtry](xref:mvc/controllers/filters) pro práci s antiforgery tokenů:</span><span class="sxs-lookup"><span data-stu-id="a94cb-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="a94cb-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="a94cb-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="a94cb-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a94cb-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="a94cb-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a94cb-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="a94cb-201">Antiforgery možnosti</span><span class="sxs-lookup"><span data-stu-id="a94cb-201">Antiforgery options</span></span>

<span data-ttu-id="a94cb-202">Přizpůsobení [antiforgery možnosti](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a94cb-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="a94cb-203">Možnost</span><span class="sxs-lookup"><span data-stu-id="a94cb-203">Option</span></span> | <span data-ttu-id="a94cb-204">Popis</span><span class="sxs-lookup"><span data-stu-id="a94cb-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="a94cb-205">Soubor cookie</span><span class="sxs-lookup"><span data-stu-id="a94cb-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="a94cb-206">Určuje nastavení používaná k vytvoření antiforgery soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="a94cb-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="a94cb-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="a94cb-208">Doména souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-208">The domain of the cookie.</span></span> <span data-ttu-id="a94cb-209">Použije se výchozí hodnota `null`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-209">Defaults to `null`.</span></span> <span data-ttu-id="a94cb-210">Tato vlastnost je zastaralá a bude v budoucí verzi odebrána.</span><span class="sxs-lookup"><span data-stu-id="a94cb-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a94cb-211">Doporučená alternativa je Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="a94cb-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="a94cb-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="a94cb-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="a94cb-213">Název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-213">The name of the cookie.</span></span> <span data-ttu-id="a94cb-214">Pokud není nastavena, systém generuje jedinečný název začíná [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="a94cb-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="a94cb-215">Tato vlastnost je zastaralá a bude v budoucí verzi odebrána.</span><span class="sxs-lookup"><span data-stu-id="a94cb-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a94cb-216">Doporučená alternativa je Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="a94cb-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="a94cb-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="a94cb-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="a94cb-218">Cesta, nastavte v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="a94cb-218">The path set on the cookie.</span></span> <span data-ttu-id="a94cb-219">Tato vlastnost je zastaralá a bude v budoucí verzi odebrána.</span><span class="sxs-lookup"><span data-stu-id="a94cb-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a94cb-220">Doporučená alternativa je Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="a94cb-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="a94cb-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="a94cb-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="a94cb-222">Název pole Skrytá formuláře antiforgery systém použije k vykreslení antiforgery tokeny v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="a94cb-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="a94cb-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="a94cb-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="a94cb-224">Název hlavičky, která používá antiforgery systém.</span><span class="sxs-lookup"><span data-stu-id="a94cb-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="a94cb-225">Pokud `null`, je za pouze data formuláře.</span><span class="sxs-lookup"><span data-stu-id="a94cb-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="a94cb-226">requireSsl</span><span class="sxs-lookup"><span data-stu-id="a94cb-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="a94cb-227">Určuje, zda je požadován protokol SSL antiforgery systémem.</span><span class="sxs-lookup"><span data-stu-id="a94cb-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="a94cb-228">Pokud `true`, požadavky bez SSL selžou.</span><span class="sxs-lookup"><span data-stu-id="a94cb-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="a94cb-229">Použije se výchozí hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-229">Defaults to `false`.</span></span> <span data-ttu-id="a94cb-230">Tato vlastnost je zastaralá a bude v budoucí verzi odebrána.</span><span class="sxs-lookup"><span data-stu-id="a94cb-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a94cb-231">Doporučená alternativa je nastavit Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="a94cb-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="a94cb-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="a94cb-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="a94cb-233">Určuje, jestli se má potlačit generování `X-Frame-Options` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a94cb-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="a94cb-234">Ve výchozím nastavení je generována záhlaví s hodnotou "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="a94cb-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="a94cb-235">Použije se výchozí hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-235">Defaults to `false`.</span></span> |

<span data-ttu-id="a94cb-236">Další informace najdete v tématu [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="a94cb-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="a94cb-237">Konfigurace funkcí antiforgery s IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="a94cb-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="a94cb-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) poskytuje rozhraní API pro konfiguraci antiforgery funkcí.</span><span class="sxs-lookup"><span data-stu-id="a94cb-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="a94cb-239">`IAntiforgery` mohou být vyžádány v `Configure` metodu `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="a94cb-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="a94cb-240">Následující příklad používá middleware z aplikace domovské stránky k vygenerování tokenu antiforgery a jeho odeslání v odpovědi jako soubor cookie (pomocí výchozí úhlová konvence popsané dál v tomto tématu):</span><span class="sxs-lookup"><span data-stu-id="a94cb-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="a94cb-241">Požadovat antiforgery ověření</span><span class="sxs-lookup"><span data-stu-id="a94cb-241">Require antiforgery validation</span></span>

<span data-ttu-id="a94cb-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) je filtr akce, který lze použít na jednotlivé akci, kontroler, nebo globálně.</span><span class="sxs-lookup"><span data-stu-id="a94cb-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="a94cb-243">Požadavky na akce, které tento filtr použít jsou zablokovány, pokud požadavek obsahuje platný antiforgery token.</span><span class="sxs-lookup"><span data-stu-id="a94cb-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="a94cb-244">`ValidateAntiForgeryToken` Atribut vyžaduje, aby token pro požadavky na metody akce upraví, včetně požadavky HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="a94cb-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="a94cb-245">Pokud `ValidateAntiForgeryToken` atribut se používá v rámci řadičů aplikace, je možné přepsat pomocí `IgnoreAntiforgeryToken` atribut.</span><span class="sxs-lookup"><span data-stu-id="a94cb-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="a94cb-246">ASP.NET Core nepodporuje přidání antiforgery tokenů pro požadavky GET automaticky.</span><span class="sxs-lookup"><span data-stu-id="a94cb-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="a94cb-247">Automaticky ověření antiforgery tokeny pro pouze unsafe metody HTTP</span><span class="sxs-lookup"><span data-stu-id="a94cb-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="a94cb-248">Aplikace ASP.NET Core negenerovat zprávy o antiforgery tokeny pro bezpečné metody HTTP (GET, HEAD, možnosti a trasování).</span><span class="sxs-lookup"><span data-stu-id="a94cb-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="a94cb-249">Místo použití široce `ValidateAntiForgeryToken` atribut a pak jeho pomocí přepsání `IgnoreAntiforgeryToken` atributy, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atribut je možné použít.</span><span class="sxs-lookup"><span data-stu-id="a94cb-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="a94cb-250">Tento atribut funguje stejně jako na `ValidateAntiForgeryToken` atribut, s tím rozdílem, že není třeba tokeny na požadavky vytvořené pomocí následujících metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="a94cb-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="a94cb-251">GET</span><span class="sxs-lookup"><span data-stu-id="a94cb-251">GET</span></span>
* <span data-ttu-id="a94cb-252">HEAD</span><span class="sxs-lookup"><span data-stu-id="a94cb-252">HEAD</span></span>
* <span data-ttu-id="a94cb-253">MOŽNOSTI</span><span class="sxs-lookup"><span data-stu-id="a94cb-253">OPTIONS</span></span>
* <span data-ttu-id="a94cb-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="a94cb-254">TRACE</span></span>

<span data-ttu-id="a94cb-255">Doporučujeme používat `AutoValidateAntiforgeryToken` široce pro scénáře – rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a94cb-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="a94cb-256">Tím se zajistí, že jsou ve výchozím nastavení chráněna POST akce.</span><span class="sxs-lookup"><span data-stu-id="a94cb-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="a94cb-257">Alternativou je ignorovat antiforgery tokeny ve výchozím nastavení, pokud `ValidateAntiForgeryToken` se použije u jednotlivých metod akcí.</span><span class="sxs-lookup"><span data-stu-id="a94cb-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="a94cb-258">Ho má více pravděpodobně v tomto scénáři pro metodu POST akce pro být ponecháno nechráněné omylem, a aplikace bude zranitelný vůči útoku proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="a94cb-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="a94cb-259">Všechny příspěvky by měli poslat antiforgery token.</span><span class="sxs-lookup"><span data-stu-id="a94cb-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="a94cb-260">Rozhraní API nemají automatický mechanismus pro odesílání bez souborů cookie součást tokenu.</span><span class="sxs-lookup"><span data-stu-id="a94cb-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="a94cb-261">Implementace pravděpodobně závisí na implementaci kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="a94cb-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="a94cb-262">Níže jsou uvedeny některé příklady:</span><span class="sxs-lookup"><span data-stu-id="a94cb-262">Some examples are shown below:</span></span>

<span data-ttu-id="a94cb-263">Příklad úrovni třídy:</span><span class="sxs-lookup"><span data-stu-id="a94cb-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="a94cb-264">Globální příklad:</span><span class="sxs-lookup"><span data-stu-id="a94cb-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="a94cb-265">Přepsání globální nebo antiforgery atributů kontroleru</span><span class="sxs-lookup"><span data-stu-id="a94cb-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="a94cb-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtru se používá k vyloučení potřeby antiforgery token pro danou akci (nebo řadič).</span><span class="sxs-lookup"><span data-stu-id="a94cb-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="a94cb-267">Při použití, přepíše tento filtr `ValidateAntiForgeryToken` a `AutoValidateAntiforgeryToken` filtry zadané na vyšší úrovni (globálně nebo na řadič).</span><span class="sxs-lookup"><span data-stu-id="a94cb-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="a94cb-268">Po ověření obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="a94cb-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="a94cb-269">Tokeny musí aktualizovat po ověření uživatele přesměrováním uživatele na zobrazení nebo stránku stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a94cb-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="a94cb-270">JavaScript AJAX a SPA</span><span class="sxs-lookup"><span data-stu-id="a94cb-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="a94cb-271">V tradiční aplikace založené na HTML antiforgery tokeny jsou předána serveru pomocí skrytého pole.</span><span class="sxs-lookup"><span data-stu-id="a94cb-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="a94cb-272">Moderní aplikace založené na jazyce JavaScript a SPA mnoho požadavků probíhají prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a94cb-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="a94cb-273">Tyto požadavky AJAX může používat další metody (například hlavičky požadavku nebo soubory cookie) k odesílání token.</span><span class="sxs-lookup"><span data-stu-id="a94cb-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="a94cb-274">Pokud se soubory cookie používají k ukládání tokeny ověřování a k ověřování žádostí o rozhraní API na serveru, proti útokům CSRF je na potenciální problém.</span><span class="sxs-lookup"><span data-stu-id="a94cb-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="a94cb-275">Pokud místní úložiště se používá k ukládání tokenu, může ohrožení zabezpečení proti útokům CSRF omezeny, protože hodnoty z místního úložiště nejsou automaticky odesílá na server s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="a94cb-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="a94cb-276">Proto používá místní úložiště k uložení antiforgery token do klienta a odeslání tokenu, protože hlavička požadavku je doporučený postup.</span><span class="sxs-lookup"><span data-stu-id="a94cb-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="a94cb-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a94cb-277">JavaScript</span></span>

<span data-ttu-id="a94cb-278">Pomocí jazyka JavaScript se zobrazeními, token lze vytvořit pomocí služby z v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a94cb-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="a94cb-279">Vložit [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) služby do zobrazení a volání [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="a94cb-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="a94cb-280">Tento přístup se eliminuje potřeba k řešení přímo nastavení souborů cookie ze serveru nebo jejich čtení z klienta.</span><span class="sxs-lookup"><span data-stu-id="a94cb-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="a94cb-281">V předchozím příkladu používá JavaScript načíst hodnotu skryté pole hlavičky AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="a94cb-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="a94cb-282">JavaScript můžete také přístup k tokeny v souborech cookie a použít k vytvoření záhlaví s hodnotou tokenu souboru cookie obsah.</span><span class="sxs-lookup"><span data-stu-id="a94cb-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="a94cb-283">Za předpokladu, že skript požadavky odeslat token v hlavičce názvem `X-CSRF-TOKEN`, konfigurovat službu antiforgery hledání `X-CSRF-TOKEN` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="a94cb-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="a94cb-284">Následující příklad používá JavaScript aby požadavek AJAX s příslušnou hlavičky:</span><span class="sxs-lookup"><span data-stu-id="a94cb-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="a94cb-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="a94cb-285">AngularJS</span></span>

<span data-ttu-id="a94cb-286">AngularJS používá vytváření adresu proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="a94cb-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="a94cb-287">Pokud server odešle soubor cookie s názvem `XSRF-TOKEN`, AngularJS `$http` služby přidá hodnota souboru cookie záhlaví při odesílání požadavku na server.</span><span class="sxs-lookup"><span data-stu-id="a94cb-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="a94cb-288">Tento proces je automatické.</span><span class="sxs-lookup"><span data-stu-id="a94cb-288">This process is automatic.</span></span> <span data-ttu-id="a94cb-289">Záhlaví nemusí být explicitně nastaveno.</span><span class="sxs-lookup"><span data-stu-id="a94cb-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="a94cb-290">Název hlavičky je `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="a94cb-291">Server by měl zjistit tuto hlavičku a ověřit jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="a94cb-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="a94cb-292">Pro rozhraní API ASP.NET Core ve spolupráci s touto konvencí:</span><span class="sxs-lookup"><span data-stu-id="a94cb-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="a94cb-293">Konfigurace aplikace k poskytování token v souboru cookie s názvem `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="a94cb-294">Konfigurovat službu antiforgery hledání hlavičku s názvem `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="a94cb-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="a94cb-295">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a94cb-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="a94cb-296">Rozšíření antiforgery</span><span class="sxs-lookup"><span data-stu-id="a94cb-296">Extend antiforgery</span></span>

<span data-ttu-id="a94cb-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typ mohou vývojáři rozšířit chování systému anti-proti útokům CSRF odezvy další data v každý token.</span><span class="sxs-lookup"><span data-stu-id="a94cb-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="a94cb-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metoda je volána pokaždé, když se vygeneruje token pole a vložené návratovou hodnotu v rámci vygenerovaný token.</span><span class="sxs-lookup"><span data-stu-id="a94cb-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="a94cb-299">Implementátor může vrátit časové razítko, hodnotu nonce nebo jakoukoli jinou hodnotu a pak zavolají [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) ověřit tato data při ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="a94cb-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="a94cb-300">Uživatelské jméno klienta je již vložených do generovaných tokenů, takže není nutné zahrnout tyto informace.</span><span class="sxs-lookup"><span data-stu-id="a94cb-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="a94cb-301">Pokud token obsahuje dodatečná data, ale ne `IAntiForgeryAdditionalDataProvider` je nakonfigurován, nejsou dodatečná data ověřena.</span><span class="sxs-lookup"><span data-stu-id="a94cb-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a94cb-302">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a94cb-302">Additional resources</span></span>

* <span data-ttu-id="a94cb-303">[Proti útokům CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="a94cb-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
