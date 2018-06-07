---
title: Používala ověřování souborů cookie bez ASP.NET Core Identity
author: rick-anderson
description: Vysvětlení pomocí souboru cookie ověřování bez ASP.NET Core Identity
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 82f826bbc2bb19339851d5e25c539ea2c03acfb3
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819107"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="2069f-103">Používala ověřování souborů cookie bez ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="2069f-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="2069f-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2069f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2069f-105">Protože jste viděli v dřívější ověřování témata, [ASP.NET Core Identity](xref:security/authentication/identity) je poskytovatel dokončení, plně vybavený ověřování pro vytváření a údržbu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2069f-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="2069f-106">Můžete však použít logika vlastní ověřování na základě souboru cookie ověřování v některých případech.</span><span class="sxs-lookup"><span data-stu-id="2069f-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="2069f-107">Ověřování na základě souborů cookie slouží jako zprostředkovatel ověřování samostatné bez ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="2069f-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="2069f-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2069f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2069f-109">Pro demonstrační účely v ukázkové aplikace je uživatelský účet pro hypotetické uživatele Marie Rodriguez pevně zakódované do aplikace.</span><span class="sxs-lookup"><span data-stu-id="2069f-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="2069f-110">Použití uživatelského jména e-mailu "maria.rodriguez@contoso.com" a žádné heslo pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2069f-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="2069f-111">Ověření uživatele v `AuthenticateUser` metoda v *Pages/Account/Login.cshtml.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="2069f-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="2069f-112">V příkladu reálného uživatele by ověřovány proti databázi.</span><span class="sxs-lookup"><span data-stu-id="2069f-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="2069f-113">Informace o migraci na základě souboru cookie ověřování z ASP.NET Core 1.x k 2.0, najdete v části [migrovat ověřování a identita na technologii ASP.NET 2.0 základní téma (ověřování na základě souborů Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="2069f-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="2069f-114">Pomocí ASP.NET Core Identity naleznete v části [Úvod k identitě](xref:security/authentication/identity) tématu.</span><span class="sxs-lookup"><span data-stu-id="2069f-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="2069f-115">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="2069f-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2069f-116">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2069f-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2069f-117">Pokud aplikace nemá používat [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), v souboru projektu pro vytvoření odkaz na balíček [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíčku (verze 2.1.0 nebo později).</span><span class="sxs-lookup"><span data-stu-id="2069f-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="2069f-118">V `ConfigureServices` metoda, vytvořit službu Middleware ověřování s `AddAuthentication` a `AddCookie` metody:</span><span class="sxs-lookup"><span data-stu-id="2069f-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="2069f-119">`AuthenticationScheme` Předaný `AddAuthentication` nastaví výchozí schéma ověřování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2069f-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="2069f-120">`AuthenticationScheme` je užitečné, když je spuštěno více instancí ověřování souborů cookie a vy chcete [autorizaci s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="2069f-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="2069f-121">Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma.</span><span class="sxs-lookup"><span data-stu-id="2069f-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="2069f-122">Můžete zadat libovolnou hodnotu řetězce, která rozlišuje schéma.</span><span class="sxs-lookup"><span data-stu-id="2069f-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="2069f-123">V `Configure` metoda, použijte `UseAuthentication` metoda k vyvolání Middleware ověřování, který nastaví `HttpContext.User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2069f-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="2069f-124">Volání `UseAuthentication` metoda před voláním `UseMvcWithDefaultRoute` nebo `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="2069f-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="2069f-125">**Možnosti AddCookie**</span><span class="sxs-lookup"><span data-stu-id="2069f-125">**AddCookie Options**</span></span>

<span data-ttu-id="2069f-126">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) třída se používá ke konfiguraci možností zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="2069f-127">Možnost</span><span class="sxs-lookup"><span data-stu-id="2069f-127">Option</span></span> | <span data-ttu-id="2069f-128">Popis</span><span class="sxs-lookup"><span data-stu-id="2069f-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="2069f-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="2069f-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="2069f-130">Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) při aktivaci pomocí `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="2069f-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="2069f-131">Výchozí hodnota je `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="2069f-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="2069f-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="2069f-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="2069f-133">Vystavitel, který má používat pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost deklarace identity, vytvořených službou souboru cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="2069f-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="2069f-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="2069f-135">Název domény, kde je soubor cookie zpracovat.</span><span class="sxs-lookup"><span data-stu-id="2069f-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="2069f-136">Ve výchozím nastavení to je název hostitele žádosti.</span><span class="sxs-lookup"><span data-stu-id="2069f-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="2069f-137">Prohlížeč odesílá pouze soubor cookie v požadavcích na odpovídající název hostitele.</span><span class="sxs-lookup"><span data-stu-id="2069f-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="2069f-138">Můžete upravit tak, aby soubory cookie, které jsou k dispozici na každém hostiteli ve vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="2069f-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="2069f-139">Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2069f-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="2069f-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="2069f-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="2069f-141">Získá nebo nastaví životnost souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="2069f-142">V současné době to možnost Ne ops a bude v ASP.NET Core 2.1 + zastaralé.</span><span class="sxs-lookup"><span data-stu-id="2069f-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="2069f-143">Použití `ExpireTimeSpan` nastavit vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="2069f-144">Další informace najdete v tématu [vysvětlení chování CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="2069f-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="2069f-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="2069f-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="2069f-146">Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="2069f-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="2069f-147">Změnou této hodnoty `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může zobrazit v aplikaci krádež souborů cookie musí mít vaše aplikace [skriptování (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2069f-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="2069f-148">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="2069f-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="2069f-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="2069f-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="2069f-150">Nastaví název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="2069f-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="2069f-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="2069f-152">Použít k izolování aplikací běžících na stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="2069f-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="2069f-153">Pokud máte aplikace spuštěné v `/app1` a chcete omezit soubory cookie pro tuto aplikaci, nastavte `CookiePath` vlastnost `/app1`.</span><span class="sxs-lookup"><span data-stu-id="2069f-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="2069f-154">Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním.</span><span class="sxs-lookup"><span data-stu-id="2069f-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="2069f-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="2069f-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="2069f-156">Určuje, zda má prohlížeč umožnit soubor cookie na připojit ke stejné lokalitě požadavky jenom (`SameSiteMode.Strict`) nebo požadavků webů pomocí bezpečné metod HTTP a žádostí stejné lokalitě (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="2069f-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="2069f-157">Pokud nastavíte hodnotu `SameSiteMode.None`, není nastaven hodnotu hlavičky souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="2069f-158">Všimněte si, že [Middleware zásady souborů Cookie](#cookie-policy-middleware) přepsat hodnotu, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="2069f-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="2069f-159">Pro podporu ověřování OAuth, výchozí hodnota je `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="2069f-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="2069f-160">Další informace najdete v tématu [poškozený z důvodu zásad SameSite souboru cookie ověřování OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="2069f-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="2069f-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="2069f-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="2069f-162">Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="2069f-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="2069f-163">Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="2069f-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="2069f-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="2069f-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="2069f-165">Nastaví `DataProtectionProvider` sloužící k vytvoření výchozích `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="2069f-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="2069f-166">Pokud `TicketDataFormat` je vlastnost nastavena, `DataProtectionProvider` možnost nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="2069f-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="2069f-167">Pokud není zadaná, se používá výchozí zprostředkovatele ochrany data aplikace.</span><span class="sxs-lookup"><span data-stu-id="2069f-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="2069f-168">Události</span><span class="sxs-lookup"><span data-stu-id="2069f-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="2069f-169">Obslužná rutina volá metody zprostředkovatele, která poskytnou aplikaci kontrolu v určitých bodech zpracování.</span><span class="sxs-lookup"><span data-stu-id="2069f-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="2069f-170">Pokud `Events` není zadáno, je zadaný výchozí instance, který nic neprovádí, pokud jsou volány metody.</span><span class="sxs-lookup"><span data-stu-id="2069f-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="2069f-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="2069f-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="2069f-172">Použít jako typ služby k získání `Events` instanci místo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2069f-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="2069f-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="2069f-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="2069f-174">`TimeSpan` Po kterém vyprší platnost lístku ověřování, který je uložený v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="2069f-175">`ExpireTimeSpan` se přidá do aktuální čas vytvoření dobu vypršení platnosti lístku.</span><span class="sxs-lookup"><span data-stu-id="2069f-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="2069f-176">`ExpiredTimeSpan` Hodnotu vždy přejde do šifrované AuthTicket ověřit server.</span><span class="sxs-lookup"><span data-stu-id="2069f-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="2069f-177">Může také přejít do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) záhlaví, ale jenom v případě `IsPersistent` nastavena.</span><span class="sxs-lookup"><span data-stu-id="2069f-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="2069f-178">Chcete-li nastavit `IsPersistent` k `true`, nakonfigurujte [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) předaný `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2069f-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="2069f-179">Výchozí hodnota `ExpireTimeSpan` je 14 dnů.</span><span class="sxs-lookup"><span data-stu-id="2069f-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="2069f-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="2069f-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="2069f-181">Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) při aktivaci pomocí `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="2069f-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="2069f-182">Aktuální adresa URL, která generovala kód 401 je přidána do `LoginPath` jako parametr řetězce dotazu s názvem podle `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="2069f-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="2069f-183">Jednou požadavek na `LoginPath` udělí novou identitu přihlášení, `ReturnUrlParameter` hodnota se používá k přesměrování prohlížeče zpět na adresu URL, která způsobila původní kód neautorizovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="2069f-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="2069f-184">Výchozí hodnota je `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="2069f-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="2069f-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="2069f-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="2069f-186">Pokud `LogoutPath` je poskytnuta obslužnou rutinu, pak přesměruje požadavek na tuto cestu na základě hodnoty z `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="2069f-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="2069f-187">Výchozí hodnota je `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="2069f-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="2069f-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="2069f-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="2069f-189">Určuje název parametru řetězce dotazu, který se připojí obslužnou rutinou pro odpovědi 302 Found (adresa URL přesměrování).</span><span class="sxs-lookup"><span data-stu-id="2069f-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="2069f-190">`ReturnUrlParameter` se používá, pokud dorazí požadavek na `LoginPath` nebo `LogoutPath` po provedení akce přihlašovací nebo odhlašovací vrátit v prohlížeči na původní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2069f-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="2069f-191">Výchozí hodnota je `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="2069f-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="2069f-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="2069f-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="2069f-193">Volitelný kontejner používá k ukládání identit napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="2069f-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="2069f-194">Pokud se používá, jenom identifikátor relace je odeslat klientovi.</span><span class="sxs-lookup"><span data-stu-id="2069f-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="2069f-195">`SessionStore` slouží k zmírnit potenciální potíže s velkými identitami.</span><span class="sxs-lookup"><span data-stu-id="2069f-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="2069f-196">parametr slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="2069f-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="2069f-197">Příznak, který udává, pokud se nový soubor cookie s času aktualizované vypršení platnosti musí být vydávány dynamicky.</span><span class="sxs-lookup"><span data-stu-id="2069f-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="2069f-198">To se může stát při všechny žádosti, kde je aktuální doba vypršení platnosti souboru cookie více než 50 % vypršela.</span><span class="sxs-lookup"><span data-stu-id="2069f-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="2069f-199">Nové datum vypršení platnosti je dál přesunout na aktuální datum a `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="2069f-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="2069f-200">[Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) lze nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2069f-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="2069f-201">Čas vypršení platnosti absolutní lze vylepšit zabezpečení vaší aplikace omezení množství času, který je platný soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="2069f-202">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="2069f-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="2069f-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="2069f-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="2069f-204">`TicketDataFormat` Se používá k ochraně a zrušení identity a dalších vlastností, které jsou uložené v hodnotě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="2069f-205">Pokud není zadaná, `TicketDataFormat` je vytvořený pomocí [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="2069f-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="2069f-206">Ověření</span><span class="sxs-lookup"><span data-stu-id="2069f-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="2069f-207">Metoda, která kontroluje, zda jsou platné možnosti.</span><span class="sxs-lookup"><span data-stu-id="2069f-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="2069f-208">Nastavit `CookieAuthenticationOptions` v konfiguraci služby pro ověřování v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="2069f-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2069f-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2069f-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2069f-210">Soubor cookie používá ASP.NET Core 1.x [middleware](xref:fundamentals/middleware/index) který serializuje hlavní název uživatele do šifrovaného souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="2069f-211">Na následné žádosti, se ověří souboru cookie a znovu vytvořit a přiřadit k objektu zabezpečení `HttpContext.User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2069f-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="2069f-212">Nainstalujte [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="2069f-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="2069f-213">Tento balíček obsahuje middlewaru souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="2069f-214">Použití `UseCookieAuthentication` metoda v `Configure` metoda v vaší *Startup.cs* soubor před `UseMvc` nebo `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="2069f-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="2069f-215">**Možnosti CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="2069f-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="2069f-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) třída se používá ke konfiguraci možností zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="2069f-217">Možnost</span><span class="sxs-lookup"><span data-stu-id="2069f-217">Option</span></span> | <span data-ttu-id="2069f-218">Popis</span><span class="sxs-lookup"><span data-stu-id="2069f-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="2069f-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="2069f-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="2069f-220">Nastaví schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-220">Sets the authentication scheme.</span></span> <span data-ttu-id="2069f-221">`AuthenticationScheme` je užitečné, když je spuštěno více instancí ověřování a vy chcete [autorizaci s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="2069f-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="2069f-222">Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma.</span><span class="sxs-lookup"><span data-stu-id="2069f-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="2069f-223">Můžete zadat libovolnou hodnotu řetězce, která rozlišuje schéma.</span><span class="sxs-lookup"><span data-stu-id="2069f-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="2069f-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="2069f-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="2069f-225">Nastaví hodnotu, znamená, že ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.</span><span class="sxs-lookup"><span data-stu-id="2069f-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="2069f-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="2069f-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="2069f-227">V případě hodnoty true zpracovává middleware ověřování automatické problémy.</span><span class="sxs-lookup"><span data-stu-id="2069f-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="2069f-228">Pokud hodnotu false, middleware ověřování jenom mění odpovědi, když explicitně uvedené `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="2069f-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="2069f-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="2069f-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="2069f-230">Vystavitel, který má používat pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost deklarace identity, vytvořené middleware ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="2069f-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="2069f-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="2069f-232">Název domény, kde je soubor cookie zpracovat.</span><span class="sxs-lookup"><span data-stu-id="2069f-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="2069f-233">Ve výchozím nastavení to je název hostitele žádosti.</span><span class="sxs-lookup"><span data-stu-id="2069f-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="2069f-234">V prohlížeči slouží pouze soubor cookie na odpovídající název hostitele.</span><span class="sxs-lookup"><span data-stu-id="2069f-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="2069f-235">Můžete upravit tak, aby soubory cookie, které jsou k dispozici na každém hostiteli ve vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="2069f-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="2069f-236">Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2069f-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="2069f-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="2069f-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="2069f-238">Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="2069f-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="2069f-239">Změnou této hodnoty `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může zobrazit v aplikaci krádež souborů cookie musí mít vaše aplikace [skriptování (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2069f-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="2069f-240">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="2069f-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="2069f-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="2069f-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="2069f-242">Použít k izolování aplikací běžících na stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="2069f-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="2069f-243">Pokud máte aplikace spuštěné v `/app1` a chcete omezit soubory cookie pro tuto aplikaci, nastavte `CookiePath` vlastnost `/app1`.</span><span class="sxs-lookup"><span data-stu-id="2069f-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="2069f-244">Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním.</span><span class="sxs-lookup"><span data-stu-id="2069f-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="2069f-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="2069f-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="2069f-246">Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="2069f-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="2069f-247">Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="2069f-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="2069f-248">Popis</span><span class="sxs-lookup"><span data-stu-id="2069f-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="2069f-249">Další informace o typu ověřování, který je k dispozici na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2069f-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="2069f-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="2069f-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="2069f-251">`TimeSpan` Po kterém vyprší platnost lístku ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="2069f-252">Přidá na aktuální čas vytvoření dobu vypršení platnosti lístku.</span><span class="sxs-lookup"><span data-stu-id="2069f-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="2069f-253">Použít `ExpireTimeSpan`, je nutné nastavit `IsPersistent` k `true` v `AuthenticationProperties` předaný `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2069f-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="2069f-254">Výchozí hodnota je 14 dnů.</span><span class="sxs-lookup"><span data-stu-id="2069f-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="2069f-255">parametr slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="2069f-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="2069f-256">Příznak, který udává, zda datum vypršení platnosti souboru cookie resetuje při více než polovinu `ExpireTimeSpan` uplynutí intervalu.</span><span class="sxs-lookup"><span data-stu-id="2069f-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="2069f-257">Nový čas exipiration se dál přesune na aktuální datum a `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="2069f-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="2069f-258">[Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) lze nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2069f-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="2069f-259">Čas vypršení platnosti absolutní lze vylepšit zabezpečení vaší aplikace omezení množství času, který je platný soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="2069f-260">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="2069f-260">The default value is `true`.</span></span> |

<span data-ttu-id="2069f-261">Nastavit `CookieAuthenticationOptions` pro Middleware ověřování souborů Cookie v `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="2069f-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="2069f-262">Middlewaru souboru cookie zásad.</span><span class="sxs-lookup"><span data-stu-id="2069f-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="2069f-263">[Middleware zásady souborů cookie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) povoluje možnosti zásad souborů cookie v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2069f-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="2069f-264">Přidání middleware aplikace zpracování kanálu je pořadí citlivé; ovlivňuje pouze komponenty registrované po ho v kanálu.</span><span class="sxs-lookup"><span data-stu-id="2069f-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="2069f-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) poskytované middlewaru souboru Cookie zásady umožňují řídit globální vlastnosti souboru cookie zpracování a háku do obslužné rutiny zpracování souboru cookie, když jsou soubory cookie připojí nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="2069f-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="2069f-266">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2069f-266">Property</span></span> | <span data-ttu-id="2069f-267">Popis</span><span class="sxs-lookup"><span data-stu-id="2069f-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="2069f-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="2069f-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="2069f-269">Ovlivňuje, jestli HttpOnly musí být soubory cookie, který je příznak označující, pokud soubor cookie musí být přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="2069f-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="2069f-270">Výchozí hodnota je `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="2069f-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="2069f-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="2069f-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="2069f-272">Ovlivňuje stejné lokalitě atribut souboru cookie (viz níže).</span><span class="sxs-lookup"><span data-stu-id="2069f-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="2069f-273">Výchozí hodnota je `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="2069f-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="2069f-274">Tato možnost je k dispozici pro jádro ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="2069f-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="2069f-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="2069f-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="2069f-276">Voláno, když se připojí do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="2069f-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="2069f-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="2069f-278">Volá se při odstranění souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="2069f-279">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2069f-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="2069f-280">Ovlivňuje, jestli soubory cookie musí být zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="2069f-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="2069f-281">Výchozí hodnota je `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="2069f-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="2069f-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + pouze)</span><span class="sxs-lookup"><span data-stu-id="2069f-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="2069f-283">Výchozí hodnota `MinimumSameSitePolicy` hodnota je `SameSiteMode.Lax` pro povolení ověřování OAuth2.</span><span class="sxs-lookup"><span data-stu-id="2069f-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="2069f-284">Výhradně vynutit zásady stejné lokalitě `SameSiteMode.Strict`, nastavte `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="2069f-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="2069f-285">I když toto nastavení dělí OAuth2 a jiná schémata ověřování cross-origin, zvyšuje úroveň zabezpečení souboru cookie pro ostatní typy aplikací, které nejsou závisí na zpracování požadavků mezi zdroji.</span><span class="sxs-lookup"><span data-stu-id="2069f-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="2069f-286">Nastavení zásad middlewaru souboru Cookie pro `MinimumSameSitePolicy` může mít vliv na vaše nastavení `Cookie.SameSite` v `CookieAuthenticationOptions` nastavení podle níže uvedené tabulky.</span><span class="sxs-lookup"><span data-stu-id="2069f-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="2069f-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="2069f-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="2069f-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="2069f-288">Cookie.SameSite</span></span> | <span data-ttu-id="2069f-289">Výsledná nastavení Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="2069f-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="2069f-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2069f-290">SameSiteMode.None</span></span>     | <span data-ttu-id="2069f-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2069f-291">SameSiteMode.None</span></span><br><span data-ttu-id="2069f-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="2069f-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="2069f-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2069f-294">SameSiteMode.None</span></span><br><span data-ttu-id="2069f-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="2069f-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="2069f-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="2069f-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2069f-298">SameSiteMode.None</span></span><br><span data-ttu-id="2069f-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="2069f-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="2069f-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="2069f-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="2069f-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="2069f-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="2069f-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="2069f-305">SameSiteMode.None</span></span><br><span data-ttu-id="2069f-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="2069f-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="2069f-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="2069f-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="2069f-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="2069f-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="2069f-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="2069f-311">Vytvoření souboru cookie pro ověřování</span><span class="sxs-lookup"><span data-stu-id="2069f-311">Create an authentication cookie</span></span>

<span data-ttu-id="2069f-312">Pokud chcete vytvořit soubor cookie, která uchovává informace o uživateli, je nutné vytvořit [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="2069f-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="2069f-313">Informace o uživateli se serializovat a uloží do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="2069f-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2069f-314">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2069f-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2069f-315">Vytvoření [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) s jakékoli požadované [deklarace identity](/dotnet/api/system.security.claims.claim)s a volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) k přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="2069f-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2069f-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2069f-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2069f-317">Volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) k přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="2069f-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="2069f-318">`SignInAsync` Vytvoří šifrovaného souboru cookie a přidává ji k aktuální odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2069f-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="2069f-319">Pokud neurčíte `AuthenticationScheme`, se používá výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="2069f-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="2069f-320">V pozadí, je šifrování, které používá ASP.NET Core [ochrany dat](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systému.</span><span class="sxs-lookup"><span data-stu-id="2069f-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="2069f-321">Pokud jste hostování aplikace na více počítačů, Vyrovnávání zatížení napříč aplikací nebo pomocí webové farmy, pak musíte [konfigurovat ochranu dat](xref:security/data-protection/configuration/overview) pro stejný klíč prstenec a identifikátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="2069f-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="2069f-322">Odhlásit se</span><span class="sxs-lookup"><span data-stu-id="2069f-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2069f-323">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2069f-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2069f-324">Odhlásit se aktuálního uživatele a odstranit jejich souboru cookie, volání [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="2069f-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2069f-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2069f-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2069f-326">Odhlásit se aktuálního uživatele a odstranit jejich souboru cookie, volání [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="2069f-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="2069f-327">Pokud nepoužíváte `CookieAuthenticationDefaults.AuthenticationScheme` (nebo "soubory cookie.) jako schéma (například" ContosoCookie"), zadejte schéma, které jste použili při konfiguraci zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="2069f-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="2069f-328">Jinak je použit výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="2069f-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="2069f-329">Reagovat na změny back-end</span><span class="sxs-lookup"><span data-stu-id="2069f-329">React to back-end changes</span></span>

<span data-ttu-id="2069f-330">Po vytvoření souboru cookie bude jediným zdrojem identity.</span><span class="sxs-lookup"><span data-stu-id="2069f-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="2069f-331">I v případě, že zakážete uživatele v systémech back-end, nemá žádné informace o tento soubor cookie ověřování systému a uživatel zůstane přihlášený tak dlouho, dokud jejich soubor cookie je neplatný.</span><span class="sxs-lookup"><span data-stu-id="2069f-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="2069f-332">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) událost v ASP.NET Core 2.x nebo [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metoda v ASP.NET Core 1.x lze použít pro zachycení a přepsat ověření souboru cookie identity.</span><span class="sxs-lookup"><span data-stu-id="2069f-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="2069f-333">Tento přístup snižuje riziko odvolané uživatele, kteří používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2069f-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="2069f-334">Jeden ze způsobů ověření souboru cookie je založena na udržování přehledu o při změně databáze uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2069f-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="2069f-335">Pokud databáze nebyla změněna od uživatele soubor cookie vydala, není nutné opakované ověření uživatele, pokud jejich souborů cookie je stále platné.</span><span class="sxs-lookup"><span data-stu-id="2069f-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="2069f-336">K implementaci databázi, která je implementovaná v tomto scénáři `IUserRepository` v tomto příkladu ukládá `LastChanged` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2069f-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="2069f-337">Při aktualizaci všechny uživatele v databázi, `LastChanged` hodnota nastavena na aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="2069f-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="2069f-338">Chcete-li zrušit platnost souboru cookie, pokud změny databáze v závislosti na `LastChanged` hodnotu, vytvořte soubor cookie s `LastChanged` deklarace identity, který obsahuje aktuální `LastChanged` hodnotu z databáze:</span><span class="sxs-lookup"><span data-stu-id="2069f-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2069f-339">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2069f-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2069f-340">K přepsání pro implementaci `ValidatePrincipal` událostí, zápisu a metoda podpisem následující třídy, které pocházejí z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="2069f-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="2069f-341">Příklad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2069f-341">An example looks like the following:</span></span>

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

<span data-ttu-id="2069f-342">Registrace instance události při registraci služby souborů cookie v `ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="2069f-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="2069f-343">Zadejte registraci vymezené služby pro vaše `CustomCookieAuthenticationEvents` třídy:</span><span class="sxs-lookup"><span data-stu-id="2069f-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2069f-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2069f-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2069f-345">K přepsání pro implementaci `ValidateAsync` událostí, zápisu a metoda podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="2069f-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="2069f-346">ASP.NET Core Identity implementuje tato kontrola v rámci jeho [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="2069f-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="2069f-347">Příklad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2069f-347">An example looks like the following:</span></span>

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

<span data-ttu-id="2069f-348">Během konfigurace ověřování souborů cookie v registraci události `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="2069f-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="2069f-349">Představte si situaci, ve kterém se aktualizuje uživatelské jméno &mdash; rozhodnutí, které nemají vliv na zabezpečení žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2069f-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="2069f-350">Pokud chcete aktualizovat nedestruktivně hlavní název uživatele, zavolejte `context.ReplacePrincipal` a nastavte `context.ShouldRenew` vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="2069f-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="2069f-351">Přístup v rámci popsaného se aktivuje u každého požadavku.</span><span class="sxs-lookup"><span data-stu-id="2069f-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="2069f-352">To může způsobit snížení výkonu velký pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2069f-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="2069f-353">Trvalé soubory cookie</span><span class="sxs-lookup"><span data-stu-id="2069f-353">Persistent cookies</span></span>

<span data-ttu-id="2069f-354">Můžete chtít soubor cookie k uchování v rámci relací prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2069f-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="2069f-355">Tato trvalosti by měl povolit pouze s souhlas explicitní uživatele s "Zapamatovat uživatele" zaškrtávací políčko je na přihlášení nebo podobné mechanismus.</span><span class="sxs-lookup"><span data-stu-id="2069f-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="2069f-356">Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který odolává prostřednictvím prohlížeče uzavření.</span><span class="sxs-lookup"><span data-stu-id="2069f-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="2069f-357">Jsou dodržení nastavení klouzavé vypršení platnosti předtím nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="2069f-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="2069f-358">Pokud při zavření prohlížeče vyprší platnost souboru cookie, prohlížeč vymaže soubor cookie po jeho restartování.</span><span class="sxs-lookup"><span data-stu-id="2069f-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2069f-359">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2069f-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="2069f-360">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) třída se nachází v `Microsoft.AspNetCore.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2069f-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2069f-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2069f-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="2069f-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) třída se nachází v `Microsoft.AspNetCore.Http.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2069f-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="2069f-363">Platnost absolutní souborů cookie</span><span class="sxs-lookup"><span data-stu-id="2069f-363">Absolute cookie expiration</span></span>

<span data-ttu-id="2069f-364">Můžete nastavit čas vypršení platnosti absolutní s `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="2069f-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="2069f-365">Musíte taky nastavit `IsPersistent`, jinak hodnota `ExpiresUtc` je ignorován a k vytvoření souboru cookie jedné relace.</span><span class="sxs-lookup"><span data-stu-id="2069f-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="2069f-366">Když `ExpiresUtc` je nastavený na `SignInAsync`, přepíše hodnotu `ExpireTimeSpan` možnost `CookieAuthenticationOptions`, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="2069f-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="2069f-367">Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který trvá po dobu 20 minut.</span><span class="sxs-lookup"><span data-stu-id="2069f-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="2069f-368">To bude ignorovat nastavení klouzavé vypršení platnosti předtím nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="2069f-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2069f-369">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2069f-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2069f-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2069f-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="additional-resources"></a><span data-ttu-id="2069f-371">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2069f-371">Additional resources</span></span>

* [<span data-ttu-id="2069f-372">Změny auth 2.0 nebo migrace oznámení</span><span class="sxs-lookup"><span data-stu-id="2069f-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="2069f-373">Omezení identity schématem</span><span class="sxs-lookup"><span data-stu-id="2069f-373">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="2069f-374">Autorizace na základě deklarace identity</span><span class="sxs-lookup"><span data-stu-id="2069f-374">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="2069f-375">Kontroly rolí na základě zásad</span><span class="sxs-lookup"><span data-stu-id="2069f-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
