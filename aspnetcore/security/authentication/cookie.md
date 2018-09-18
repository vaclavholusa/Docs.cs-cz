---
title: Ověřování souborem cookie bez ASP.NET Core Identity
author: rick-anderson
description: Vysvětlení, používání ověřování souborem cookie bez ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 8045a1bf27853ff5f03166e7cf10d89e2ad38fd1
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011833"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="92f47-103">Ověřování souborem cookie bez ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="92f47-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="92f47-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="92f47-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="92f47-105">Jak už víte, v předchozích tématech ověřování [ASP.NET Core Identity](xref:security/authentication/identity) je ověřování dokončeno, plně vybavené zprostředkovatele pro vytváření a správa přihlášení.</span><span class="sxs-lookup"><span data-stu-id="92f47-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="92f47-106">Můžete ale použít vlastní logiku ověřování vlastní soubor cookie s ověřováním pomocí čas od času.</span><span class="sxs-lookup"><span data-stu-id="92f47-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="92f47-107">Ověřování na základě souborů cookie slouží jako zprostředkovatel ověřování samostatné bez ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="92f47-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="92f47-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92f47-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="92f47-109">Pro demonstrační účely v ukázkové aplikaci je uživatelský účet pro hypotetické uživatele Marie Rodriguez pevně zakódované do aplikace.</span><span class="sxs-lookup"><span data-stu-id="92f47-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="92f47-110">Použití uživatelského jména e-mailu "maria.rodriguez@contoso.com" a jakékoli heslo k přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="92f47-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="92f47-111">Ověření uživatele v `AuthenticateUser` metodu *Pages/Account/Login.cshtml.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="92f47-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="92f47-112">V reálný příklad uživatel by ověřovány proti databázi.</span><span class="sxs-lookup"><span data-stu-id="92f47-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="92f47-113">Informace o migraci na základě souboru cookie ověřování ASP.NET Core 1.x do 2.0, naleznete v tématu [migrovat ověřování a identita na téma ASP.NET Core 2.0 (ověřování na základě souborů Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="92f47-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="92f47-114">Použití ASP.NET Core Identity, najdete v článku [Úvod do Identity](xref:security/authentication/identity) tématu.</span><span class="sxs-lookup"><span data-stu-id="92f47-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="92f47-115">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="92f47-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f47-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f47-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="92f47-117">Pokud se aplikace nepoužívá [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app), vytvořit odkaz na balíček v souboru projektu [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíčku (verze 2.1.0 nebo později).</span><span class="sxs-lookup"><span data-stu-id="92f47-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="92f47-118">V `ConfigureServices` metody vytvoření Middleware ověřování služby s `AddAuthentication` a `AddCookie` metody:</span><span class="sxs-lookup"><span data-stu-id="92f47-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="92f47-119">`AuthenticationScheme` Předaný `AddAuthentication` nastaví výchozí schéma ověřování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92f47-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="92f47-120">`AuthenticationScheme` je užitečné, pokud existuje více instancí ověřování souborů cookie a vy chcete [autorizovat s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="92f47-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="92f47-121">Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma.</span><span class="sxs-lookup"><span data-stu-id="92f47-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="92f47-122">Můžete zadat libovolnou hodnotu řetězce, která odlišuje schéma.</span><span class="sxs-lookup"><span data-stu-id="92f47-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="92f47-123">Schéma ověřování aplikace se liší od schématu ověřování souborů cookie aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92f47-123">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="92f47-124">Pokud není k dispozici schéma ověřování souborů cookie do <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, používá [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (soubory cookie.").</span><span class="sxs-lookup"><span data-stu-id="92f47-124">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="92f47-125">V `Configure` metody, použijte `UseAuthentication` metoda k vyvolání ověřovací Middleware, který nastaví `HttpContext.User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="92f47-125">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="92f47-126">Volání `UseAuthentication` před voláním metody `UseMvcWithDefaultRoute` nebo `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="92f47-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="92f47-127">**Možnosti AddCookie**</span><span class="sxs-lookup"><span data-stu-id="92f47-127">**AddCookie Options**</span></span>

<span data-ttu-id="92f47-128">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) třída se používá ke konfiguraci možností zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-128">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="92f47-129">Možnost</span><span class="sxs-lookup"><span data-stu-id="92f47-129">Option</span></span> | <span data-ttu-id="92f47-130">Popis</span><span class="sxs-lookup"><span data-stu-id="92f47-130">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="92f47-131">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="92f47-131">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="92f47-132">Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) aktivovaného `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="92f47-132">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="92f47-133">Výchozí hodnota je `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="92f47-133">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="92f47-134">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="92f47-134">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="92f47-135">Vystavitel, který má použít pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost na všechny deklarace vytvořené službou ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-135">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="92f47-136">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="92f47-136">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="92f47-137">Název domény, kde je soubor cookie obsluhuje.</span><span class="sxs-lookup"><span data-stu-id="92f47-137">The domain name where the cookie is served.</span></span> <span data-ttu-id="92f47-138">Ve výchozím nastavení toto je název hostitele žádosti.</span><span class="sxs-lookup"><span data-stu-id="92f47-138">By default, this is the host name of the request.</span></span> <span data-ttu-id="92f47-139">Prohlížeč odesílá pouze soubor cookie v požadavcích na odpovídající název hostitele.</span><span class="sxs-lookup"><span data-stu-id="92f47-139">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="92f47-140">Můžete chtít upravit tak, aby soubory cookie, které jsou k dispozici pro všechny hostitele ve vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="92f47-140">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="92f47-141">Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="92f47-141">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="92f47-142">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="92f47-142">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="92f47-143">Získá nebo nastaví životnost do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-143">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="92f47-144">V současné době to možnost operace a bude zastaralé v ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="92f47-144">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="92f47-145">Použití `ExpireTimeSpan` možnost nastavit vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-145">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="92f47-146">Další informace najdete v tématu [vysvětlení chování CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="92f47-146">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="92f47-147">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="92f47-147">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="92f47-148">Příznak označující, pokud by měl být soubor cookie přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="92f47-148">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="92f47-149">Změna této hodnoty na `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může otevřít aplikaci tak, aby krádež souborů cookie by měl mít vaše aplikace [skriptování napříč weby (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="92f47-149">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="92f47-150">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="92f47-150">The default value is `true`.</span></span> |
| [<span data-ttu-id="92f47-151">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="92f47-151">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="92f47-152">Nastaví název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-152">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="92f47-153">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="92f47-153">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="92f47-154">Použít k izolaci aplikací spuštěných na stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="92f47-154">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="92f47-155">Pokud máte aplikaci spuštěnou na `/app1` a chcete omezit souborů cookie do této aplikace, nastavte `CookiePath` vlastnost `/app1`.</span><span class="sxs-lookup"><span data-stu-id="92f47-155">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="92f47-156">Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním.</span><span class="sxs-lookup"><span data-stu-id="92f47-156">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="92f47-157">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="92f47-157">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="92f47-158">Určuje, zda v prohlížeči by měl povolit soubory cookie bude připojený na stejný web požadavky jenom (`SameSiteMode.Strict`) nebo pomocí bezpečných metod HTTP a požadavky na stejný web požadavky mezi weby (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="92f47-158">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="92f47-159">Pokud je nastavena na `SameSiteMode.None`, není-li nastavena hodnota hlavičky souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-159">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="92f47-160">Všimněte si, že [middlewaru souboru Cookie. zásady](#cookie-policy-middleware) mohou přepsat hodnotu, kterou zadáte.</span><span class="sxs-lookup"><span data-stu-id="92f47-160">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="92f47-161">Pro podporu ověřování OAuth, výchozí hodnota je `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="92f47-161">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="92f47-162">Další informace najdete v tématu [nefunkční kvůli zásadám SameSite souboru cookie ověřování OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="92f47-162">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="92f47-163">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="92f47-163">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="92f47-164">Příznak označující, pokud by měl být soubor cookie vytvořený omezené na protokol HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavek (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="92f47-164">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="92f47-165">Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="92f47-165">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="92f47-166">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="92f47-166">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="92f47-167">Nastaví `DataProtectionProvider` , který slouží k vytvoření výchozích `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="92f47-167">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="92f47-168">Pokud `TicketDataFormat` je vlastnost nastavena, `DataProtectionProvider` není použita možnost.</span><span class="sxs-lookup"><span data-stu-id="92f47-168">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="92f47-169">Pokud se nezadá, výchozí zprostředkovatel ochrany dat aplikace se používá.</span><span class="sxs-lookup"><span data-stu-id="92f47-169">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="92f47-170">Události</span><span class="sxs-lookup"><span data-stu-id="92f47-170">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="92f47-171">Obslužná rutina volá metody zprostředkovatele, který poskytují kontrolu aplikace v určitých bodech, zpracování.</span><span class="sxs-lookup"><span data-stu-id="92f47-171">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="92f47-172">Pokud `Events` nejsou zadaná, je použita výchozí instance, který nemá žádný účinek, pokud jsou volány metody.</span><span class="sxs-lookup"><span data-stu-id="92f47-172">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="92f47-173">EventsType</span><span class="sxs-lookup"><span data-stu-id="92f47-173">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="92f47-174">Použít jako typ služby k získání `Events` instanci místo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="92f47-174">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="92f47-175">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="92f47-175">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="92f47-176">`TimeSpan` Po kterém vyprší platnost lístku ověřování uložený v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-176">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="92f47-177">`ExpireTimeSpan` je přidán do aktuální čas při vytváření dobu vypršení platnosti-the-ticket.</span><span class="sxs-lookup"><span data-stu-id="92f47-177">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="92f47-178">`ExpiredTimeSpan` Hodnota vždy přejde do šifrovaného AuthTicket ověřit server.</span><span class="sxs-lookup"><span data-stu-id="92f47-178">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="92f47-179">Může taky přejít do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) záhlaví, ale pouze v případě `IsPersistent` nastavena.</span><span class="sxs-lookup"><span data-stu-id="92f47-179">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="92f47-180">Nastavit `IsPersistent` k `true`, nakonfigurujte [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) předán `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="92f47-180">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="92f47-181">Výchozí hodnota `ExpireTimeSpan` je 14 dní.</span><span class="sxs-lookup"><span data-stu-id="92f47-181">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="92f47-182">LoginPath</span><span class="sxs-lookup"><span data-stu-id="92f47-182">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="92f47-183">Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) aktivovaného `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="92f47-183">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="92f47-184">Aktuální adresa URL, které vygenerovalo 401 je přidán do `LoginPath` jako parametru řetězce dotazu s názvem podle `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="92f47-184">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="92f47-185">Jakmile požadavek na `LoginPath` udělí novou identitu přihlášení, `ReturnUrlParameter` hodnota slouží k přesměrování prohlížeče zpět na adresu URL, která způsobila původní kód neautorizovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="92f47-185">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="92f47-186">Výchozí hodnota je `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="92f47-186">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="92f47-187">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="92f47-187">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="92f47-188">Pokud `LogoutPath` je k dispozici pro obslužnou rutinu, pak přesměruje požadavek na tuto cestu podle hodnoty `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="92f47-188">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="92f47-189">Výchozí hodnota je `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="92f47-189">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="92f47-190">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="92f47-190">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="92f47-191">Určuje název parametru řetězce dotazu, který se připojí obslužnou rutinou pro odpovědi 302 Found (adresa URL přesměrování).</span><span class="sxs-lookup"><span data-stu-id="92f47-191">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="92f47-192">`ReturnUrlParameter` se používá, když dorazí požadavek na `LoginPath` nebo `LogoutPath` do prohlížeče po provedení akce přihlašovací nebo odhlašovací vraťte k původní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="92f47-192">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="92f47-193">Výchozí hodnota je `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="92f47-193">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="92f47-194">SessionStore</span><span class="sxs-lookup"><span data-stu-id="92f47-194">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="92f47-195">Volitelný kontejner používá k ukládání identit napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="92f47-195">An optional container used to store identity across requests.</span></span> <span data-ttu-id="92f47-196">Když se použije, jenom identifikátor relace je odeslat klientovi.</span><span class="sxs-lookup"><span data-stu-id="92f47-196">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="92f47-197">`SessionStore` je možné zmírnit potenciální potíže s velkými identitami.</span><span class="sxs-lookup"><span data-stu-id="92f47-197">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="92f47-198">parametr slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="92f47-198">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="92f47-199">Příznak označující, pokud se nový soubor cookie s čas vypršení platnosti aktualizace by měla být vydána dynamicky.</span><span class="sxs-lookup"><span data-stu-id="92f47-199">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="92f47-200">To může nastat na všechny požadavky, pokud aktuální období vypršení platnosti souboru cookie je více než 50 % vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="92f47-200">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="92f47-201">Nové datum vypršení platnosti bude přesunut dopředu na aktuální datum plus `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="92f47-201">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="92f47-202">[Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) můžete nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="92f47-202">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="92f47-203">Čas absolutní vypršení platnosti můžete zlepšit zabezpečení vaší aplikace tím, že omezíte množství času, který je platný soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-203">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="92f47-204">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="92f47-204">The default value is `true`.</span></span> |
| [<span data-ttu-id="92f47-205">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="92f47-205">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="92f47-206">`TicketDataFormat` Se používá k ochraně a zrušení této ochrany identity a dalších vlastností, které jsou uloženy v hodnotě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-206">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="92f47-207">Pokud se nezadá, `TicketDataFormat` je vytvořený pomocí [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="92f47-207">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="92f47-208">Ověření</span><span class="sxs-lookup"><span data-stu-id="92f47-208">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="92f47-209">Metoda, která kontroluje, že možnosti jsou platné.</span><span class="sxs-lookup"><span data-stu-id="92f47-209">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="92f47-210">Nastavte `CookieAuthenticationOptions` v konfiguraci služby pro ověřování v `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="92f47-210">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f47-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f47-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="92f47-212">Soubor cookie používá ASP.NET Core 1.x [middleware](xref:fundamentals/middleware/index) hlavní název uživatele, který serializuje do zašifrovaného souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-212">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="92f47-213">Na následné žádosti soubor cookie se ověří a znovu vytvořit a přiřadit k objektu zabezpečení `HttpContext.User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="92f47-213">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="92f47-214">Nainstalujte [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíčku NuGet ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="92f47-214">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="92f47-215">Tento balíček obsahuje middlewaru souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-215">This package contains the cookie middleware.</span></span>

<span data-ttu-id="92f47-216">Použití `UseCookieAuthentication` metoda ve `Configure` metoda ve vaší *Startup.cs* souboru před `UseMvc` nebo `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="92f47-216">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="92f47-217">**Možnosti CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="92f47-217">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="92f47-218">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) třída se používá ke konfiguraci možností zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-218">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="92f47-219">Možnost</span><span class="sxs-lookup"><span data-stu-id="92f47-219">Option</span></span> | <span data-ttu-id="92f47-220">Popis</span><span class="sxs-lookup"><span data-stu-id="92f47-220">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="92f47-221">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="92f47-221">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="92f47-222">Nastaví schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-222">Sets the authentication scheme.</span></span> <span data-ttu-id="92f47-223">`AuthenticationScheme` je užitečné, pokud existuje více instancí ověřování a vy chcete [autorizovat s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="92f47-223">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="92f47-224">Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma.</span><span class="sxs-lookup"><span data-stu-id="92f47-224">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="92f47-225">Můžete zadat libovolnou hodnotu řetězce, která odlišuje schéma.</span><span class="sxs-lookup"><span data-stu-id="92f47-225">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="92f47-226">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="92f47-226">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="92f47-227">Nastaví hodnotu označující, že ověřování souborů cookie by měl spustit u každého požadavku a pokusí se ověřit a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořená.</span><span class="sxs-lookup"><span data-stu-id="92f47-227">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="92f47-228">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="92f47-228">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="92f47-229">Při hodnotě true se zpracovává middleware ověřování automatické výzvy.</span><span class="sxs-lookup"><span data-stu-id="92f47-229">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="92f47-230">Pokud false, middleware ověřování pouze změní odpovědi, když explicitně `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="92f47-230">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="92f47-231">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="92f47-231">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="92f47-232">Vystavitel, který má použít pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost na všechny deklarace vytvořené middleware ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-232">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="92f47-233">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="92f47-233">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="92f47-234">Název domény, kde je soubor cookie obsluhuje.</span><span class="sxs-lookup"><span data-stu-id="92f47-234">The domain name where the cookie is served.</span></span> <span data-ttu-id="92f47-235">Ve výchozím nastavení toto je název hostitele žádosti.</span><span class="sxs-lookup"><span data-stu-id="92f47-235">By default, this is the host name of the request.</span></span> <span data-ttu-id="92f47-236">Prohlížeč slouží pouze souboru cookie, který má odpovídající název hostitele.</span><span class="sxs-lookup"><span data-stu-id="92f47-236">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="92f47-237">Můžete chtít upravit tak, aby soubory cookie, které jsou k dispozici pro všechny hostitele ve vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="92f47-237">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="92f47-238">Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="92f47-238">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="92f47-239">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="92f47-239">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="92f47-240">Příznak označující, pokud by měl být soubor cookie přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="92f47-240">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="92f47-241">Změna této hodnoty na `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může otevřít aplikaci tak, aby krádež souborů cookie by měl mít vaše aplikace [skriptování napříč weby (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="92f47-241">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="92f47-242">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="92f47-242">The default value is `true`.</span></span> |
| [<span data-ttu-id="92f47-243">CookiePath</span><span class="sxs-lookup"><span data-stu-id="92f47-243">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="92f47-244">Použít k izolaci aplikací spuštěných na stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="92f47-244">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="92f47-245">Pokud máte aplikaci spuštěnou na `/app1` a chcete omezit souborů cookie do této aplikace, nastavte `CookiePath` vlastnost `/app1`.</span><span class="sxs-lookup"><span data-stu-id="92f47-245">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="92f47-246">Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním.</span><span class="sxs-lookup"><span data-stu-id="92f47-246">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="92f47-247">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="92f47-247">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="92f47-248">Příznak označující, pokud by měl být soubor cookie vytvořený omezené na protokol HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavek (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="92f47-248">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="92f47-249">Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="92f47-249">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="92f47-250">Popis</span><span class="sxs-lookup"><span data-stu-id="92f47-250">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="92f47-251">Další informace o typu ověřování, který je k dispozici pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92f47-251">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="92f47-252">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="92f47-252">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="92f47-253">`TimeSpan` Po kterém vyprší platnost lístku ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-253">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="92f47-254">Přidá se do aktuálního času vytvoření dobu vypršení platnosti-the-ticket.</span><span class="sxs-lookup"><span data-stu-id="92f47-254">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="92f47-255">Použití `ExpireTimeSpan`, je nutné nastavit `IsPersistent` k `true` v `AuthenticationProperties` předán `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="92f47-255">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="92f47-256">Výchozí hodnota je 14 dní.</span><span class="sxs-lookup"><span data-stu-id="92f47-256">The default value is 14 days.</span></span> |
| [<span data-ttu-id="92f47-257">parametr slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="92f47-257">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="92f47-258">Příznak označující, zda datum vypršení platnosti souboru cookie resetuje při více než polovina `ExpireTimeSpan` uplynutí intervalu.</span><span class="sxs-lookup"><span data-stu-id="92f47-258">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="92f47-259">Nový čas exipiration bude přesunut dopředu na aktuální datum plus `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="92f47-259">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="92f47-260">[Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) můžete nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="92f47-260">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="92f47-261">Čas absolutní vypršení platnosti můžete zlepšit zabezpečení vaší aplikace tím, že omezíte množství času, který je platný soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-261">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="92f47-262">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="92f47-262">The default value is `true`.</span></span> |

<span data-ttu-id="92f47-263">Nastavte `CookieAuthenticationOptions` pro Middleware ověřování souborů Cookie v `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="92f47-263">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="92f47-264">Zásady middlewaru souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-264">Cookie Policy Middleware</span></span>

<span data-ttu-id="92f47-265">[Zásady middlewaru souboru cookie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) umožňuje možnosti zásad souborů cookie v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92f47-265">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="92f47-266">Přidání middleware do kanálu zpracování aplikace je v pořadí citlivé; ovlivňuje pouze komponenty zaregistrovaný v kanálu po něm.</span><span class="sxs-lookup"><span data-stu-id="92f47-266">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="92f47-267">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) poskytnuté middlewaru souboru Cookie. zásady umožňují určit globální vlastnosti zpracování souboru cookie a hook do obslužné rutiny zpracování souboru cookie, kdy jsou soubory cookie připojí nebo odstraní.</span><span class="sxs-lookup"><span data-stu-id="92f47-267">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="92f47-268">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="92f47-268">Property</span></span> | <span data-ttu-id="92f47-269">Popis</span><span class="sxs-lookup"><span data-stu-id="92f47-269">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="92f47-270">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="92f47-270">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="92f47-271">Ovlivňuje, jestli HttpOnly musí být soubory cookie, který je příznak označující, pokud by měl být soubor cookie přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="92f47-271">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="92f47-272">Výchozí hodnota je `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="92f47-272">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="92f47-273">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="92f47-273">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="92f47-274">Má vliv na stejný web atribut souboru cookie (viz níže).</span><span class="sxs-lookup"><span data-stu-id="92f47-274">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="92f47-275">Výchozí hodnota je `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="92f47-275">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="92f47-276">Tato možnost je dostupná pro ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="92f47-276">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="92f47-277">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="92f47-277">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="92f47-278">Volá se, když je přidán do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-278">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="92f47-279">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="92f47-279">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="92f47-280">Volá se, když se odstraní soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-280">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="92f47-281">zabezpečení</span><span class="sxs-lookup"><span data-stu-id="92f47-281">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="92f47-282">Ovlivňuje, jestli soubory cookie musí být zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="92f47-282">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="92f47-283">Výchozí hodnota je `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="92f47-283">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="92f47-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + jenom)</span><span class="sxs-lookup"><span data-stu-id="92f47-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="92f47-285">Výchozí hodnota `MinimumSameSitePolicy` hodnotu `SameSiteMode.Lax` tak, aby povolovala ověřování OAuth2.</span><span class="sxs-lookup"><span data-stu-id="92f47-285">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="92f47-286">Přísně vynutit zásady stejný web `SameSiteMode.Strict`, nastavte `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="92f47-286">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="92f47-287">I když toto nastavení přeruší OAuth2 nebo jiná schémata ověřování mezi zdroji, zvyšuje úroveň zabezpečení souboru cookie pro ostatní typy aplikací, které se nemusíte spoléhat na zpracování žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="92f47-287">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="92f47-288">Nastavení zásad middlewaru souboru Cookie pro `MinimumSameSitePolicy` může mít vliv na vaše nastavení `Cookie.SameSite` v `CookieAuthenticationOptions` nastavení podle níže uvedených matice.</span><span class="sxs-lookup"><span data-stu-id="92f47-288">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="92f47-289">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="92f47-289">MinimumSameSitePolicy</span></span> | <span data-ttu-id="92f47-290">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="92f47-290">Cookie.SameSite</span></span> | <span data-ttu-id="92f47-291">Výsledná nastavení Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="92f47-291">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="92f47-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="92f47-292">SameSiteMode.None</span></span>     | <span data-ttu-id="92f47-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="92f47-293">SameSiteMode.None</span></span><br><span data-ttu-id="92f47-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="92f47-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="92f47-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="92f47-296">SameSiteMode.None</span></span><br><span data-ttu-id="92f47-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="92f47-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="92f47-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-299">SameSiteMode.Lax</span></span>      | <span data-ttu-id="92f47-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="92f47-300">SameSiteMode.None</span></span><br><span data-ttu-id="92f47-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="92f47-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="92f47-303">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-303">SameSiteMode.Lax</span></span><br><span data-ttu-id="92f47-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="92f47-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-305">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="92f47-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-306">SameSiteMode.Strict</span></span>   | <span data-ttu-id="92f47-307">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="92f47-307">SameSiteMode.None</span></span><br><span data-ttu-id="92f47-308">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="92f47-308">SameSiteMode.Lax</span></span><br><span data-ttu-id="92f47-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-309">SameSiteMode.Strict</span></span> | <span data-ttu-id="92f47-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-310">SameSiteMode.Strict</span></span><br><span data-ttu-id="92f47-311">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-311">SameSiteMode.Strict</span></span><br><span data-ttu-id="92f47-312">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="92f47-312">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="92f47-313">Vytvoření souboru cookie pro ověřování</span><span class="sxs-lookup"><span data-stu-id="92f47-313">Create an authentication cookie</span></span>

<span data-ttu-id="92f47-314">Pokud chcete vytvořit soubor cookie, která uchovává informace o uživateli, je nutné vytvořit [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="92f47-314">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="92f47-315">Informace o uživateli je serializován a uložená v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="92f47-315">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f47-316">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f47-316">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="92f47-317">Vytvoření [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) s jakékoli požadované [deklarace identity](/dotnet/api/system.security.claims.claim)s a volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) k přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="92f47-317">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f47-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f47-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="92f47-319">Volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) k přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="92f47-319">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="92f47-320">`SignInAsync` Vytvoří do zašifrovaného souboru cookie a přidá jej do aktuální odpovědi.</span><span class="sxs-lookup"><span data-stu-id="92f47-320">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="92f47-321">Pokud nezadáte `AuthenticationScheme`, se používá výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="92f47-321">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="92f47-322">Pod pokličkou, je šifrování, které používá ASP.NET Core [ochranu dat](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systému.</span><span class="sxs-lookup"><span data-stu-id="92f47-322">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="92f47-323">Pokud se hostují aplikaci na více počítačů, Vyrovnávání zatížení napříč aplikacemi nebo pomocí webové farmy, je nutné [Konfigurace ochrany dat](xref:security/data-protection/configuration/overview) používat stejné aktualizační kanál klíč a identifikátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="92f47-323">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="92f47-324">Odhlásit se</span><span class="sxs-lookup"><span data-stu-id="92f47-324">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f47-325">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f47-325">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="92f47-326">Chcete-li odhlásit aktuálního uživatele a odstranit jejich souborů cookie, zavolejte [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="92f47-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f47-327">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f47-327">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="92f47-328">Chcete-li odhlásit aktuálního uživatele a odstranit jejich souborů cookie, zavolejte [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="92f47-328">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="92f47-329">Pokud nepoužíváte `CookieAuthenticationDefaults.AuthenticationScheme` (nebo "Soubory cookie") jako schéma (například "ContosoCookie"), zadejte schéma, které jste použili při konfiguraci zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="92f47-329">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="92f47-330">V opačném případě se používá výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="92f47-330">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="92f47-331">Reagovat na změny back-end</span><span class="sxs-lookup"><span data-stu-id="92f47-331">React to back-end changes</span></span>

<span data-ttu-id="92f47-332">Po vytvoření souboru cookie, bude jediný zdroj identity.</span><span class="sxs-lookup"><span data-stu-id="92f47-332">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="92f47-333">I v případě, že zakázání uživatele v back endových systémů, systém ověřování souborů cookie je vůbec nezná to a uživatel zůstane přihlášený jako jejich soubor cookie je neplatný.</span><span class="sxs-lookup"><span data-stu-id="92f47-333">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="92f47-334">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) událost v ASP.NET Core 2.x nebo [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metoda v ASP.NET Core 1.x lze zachytit a přepsat ověření souboru cookie identity.</span><span class="sxs-lookup"><span data-stu-id="92f47-334">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="92f47-335">Tento přístup snižuje riziko odvolané uživatele, kteří používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92f47-335">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="92f47-336">Jedním z přístupů k ověření souboru cookie je založená na udržování přehledu o při změně uživatelskou databázi.</span><span class="sxs-lookup"><span data-stu-id="92f47-336">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="92f47-337">Pokud databáze nebylo změněno od uživatele soubor cookie vydala, není nutné opakované ověření uživatele, pokud jejich souborů cookie je stále platný.</span><span class="sxs-lookup"><span data-stu-id="92f47-337">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="92f47-338">Databáze, které je implementované v tomto scénáři implementovat `IUserRepository` v tomto příkladu ukládá `LastChanged` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="92f47-338">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="92f47-339">Když se aktualizuje každý uživatel v databázi, `LastChanged` je hodnota nastavena na aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="92f47-339">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="92f47-340">Pokud chcete platnost souboru cookie, pokud změny databáze v závislosti na `LastChanged` hodnoty, vytvořte soubor cookie s `LastChanged` deklarace obsahující aktuální `LastChanged` hodnota z databáze:</span><span class="sxs-lookup"><span data-stu-id="92f47-340">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f47-341">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f47-341">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="92f47-342">K implementaci přepsání `ValidatePrincipal` události, napište metodu s následující signaturou ve třídě, který je odvozen z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="92f47-342">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="92f47-343">Příklad bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="92f47-343">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="92f47-344">Zaregistrovat instanci události během registrace služby souborů cookie v `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="92f47-344">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="92f47-345">Registraci vymezené služby pro vaše `CustomCookieAuthenticationEvents` třídy:</span><span class="sxs-lookup"><span data-stu-id="92f47-345">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f47-346">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f47-346">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="92f47-347">K implementaci přepsání `ValidateAsync` události, napište metodu s následující signaturou:</span><span class="sxs-lookup"><span data-stu-id="92f47-347">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="92f47-348">ASP.NET Core Identity implementuje tato kontrola v rámci jeho [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="92f47-348">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="92f47-349">Příklad bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="92f47-349">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="92f47-350">Zaregistrujte události během konfigurace ověřování souborů cookie v `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="92f47-350">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="92f47-351">Představte si situaci, ve kterém se aktualizuje uživatelské jméno &mdash; rozhodnutí, která nemá vliv na zabezpečení žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="92f47-351">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="92f47-352">Pokud chcete aktualizovat nedestruktivně hlavní název uživatele, zavolejte `context.ReplacePrincipal` a nastavit `context.ShouldRenew` vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="92f47-352">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="92f47-353">Tento přístup je zde popsáno, se aktivuje u každého požadavku.</span><span class="sxs-lookup"><span data-stu-id="92f47-353">The approach described here is triggered on every request.</span></span> <span data-ttu-id="92f47-354">To může způsobit snížení výkonu velké aplikace.</span><span class="sxs-lookup"><span data-stu-id="92f47-354">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="92f47-355">Trvalé soubory cookie</span><span class="sxs-lookup"><span data-stu-id="92f47-355">Persistent cookies</span></span>

<span data-ttu-id="92f47-356">Můžete chtít soubor cookie zachovat v rámci relací prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="92f47-356">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="92f47-357">Tato trvalost má být povoleno pouze explicitní uživatelské souhlas s "Zapamatovat" zaškrtávací políčko na přihlašovací nebo mechanismus podobný.</span><span class="sxs-lookup"><span data-stu-id="92f47-357">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="92f47-358">Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který odolává prostřednictvím prohlížeče UZÁVĚRECH.</span><span class="sxs-lookup"><span data-stu-id="92f47-358">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="92f47-359">Klouzavé vypršení platnosti nastavení jste dříve nakonfigurovali jsou zachované.</span><span class="sxs-lookup"><span data-stu-id="92f47-359">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="92f47-360">Pokud se při zavření prohlížeče vyprší platnost souboru cookie, vymaže prohlížeči soubor cookie po jeho restartování.</span><span class="sxs-lookup"><span data-stu-id="92f47-360">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f47-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f47-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="92f47-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) třídy se nachází v `Microsoft.AspNetCore.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="92f47-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f47-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f47-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="92f47-364">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) třídy se nachází v `Microsoft.AspNetCore.Http.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="92f47-364">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="92f47-365">Soubor cookie absolutní vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="92f47-365">Absolute cookie expiration</span></span>

<span data-ttu-id="92f47-366">Můžete nastavit dobu vypršení platnosti absolutní s `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="92f47-366">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="92f47-367">Musíte taky nastavit `IsPersistent`; v opačném případě `ExpiresUtc` je ignorována a vytvoří se soubor cookie jedné relace.</span><span class="sxs-lookup"><span data-stu-id="92f47-367">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="92f47-368">Když `ExpiresUtc` je nastavena na `SignInAsync`, přepíše hodnotu `ExpireTimeSpan` možnost `CookieAuthenticationOptions`, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="92f47-368">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="92f47-369">Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který má platnost po dobu 20 minut.</span><span class="sxs-lookup"><span data-stu-id="92f47-369">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="92f47-370">To ignoruje klouzavé vypršení platnosti nastavení předtím nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="92f47-370">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f47-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f47-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f47-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f47-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="additional-resources"></a><span data-ttu-id="92f47-373">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="92f47-373">Additional resources</span></span>

* [<span data-ttu-id="92f47-374">Ověřování 2.0 změny / migrace oznámení</span><span class="sxs-lookup"><span data-stu-id="92f47-374">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="92f47-375">Role na základě zásad kontroly</span><span class="sxs-lookup"><span data-stu-id="92f47-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
