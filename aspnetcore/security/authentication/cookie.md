---
title: "Pomocí souboru Cookie ověřování bez ASP.NET Core Identity"
author: rick-anderson
description: "Vysvětlení pomocí souboru cookie ověřování bez ASP.NET Core Identity"
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 26921eb6af6629d821e57112a47b40146cb027f6
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="920a6-103">Pomocí souboru Cookie ověřování bez ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="920a6-103">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="920a6-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="920a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="920a6-105">Protože jste viděli v dřívější ověřování témata, [ASP.NET Core Identity](xref:security/authentication/identity) je poskytovatel dokončení, plně vybavený ověřování pro vytváření a údržbu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="920a6-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="920a6-106">Můžete však použít logika vlastní ověřování na základě souboru cookie ověřování v některých případech.</span><span class="sxs-lookup"><span data-stu-id="920a6-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="920a6-107">Ověřování na základě souborů cookie slouží jako zprostředkovatel ověřování samostatné bez ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="920a6-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="920a6-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="920a6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="920a6-109">Informace o migraci na základě souboru cookie ověřování z ASP.NET Core 1.x k 2.0, najdete v části [migrace ověřování a identita na technologii ASP.NET 2.0 základní téma (ověřování na základě souborů Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="920a6-109">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="920a6-110">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="920a6-110">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="920a6-111">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="920a6-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="920a6-112">Pokud nepoužíváte [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instalace verze 2.0 + [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="920a6-112">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="920a6-113">V `ConfigureServices` metoda, vytvořit službu Middleware ověřování s `AddAuthentication` a `AddCookie` metody:</span><span class="sxs-lookup"><span data-stu-id="920a6-113">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="920a6-114">`AuthenticationScheme`Předaný `AddAuthentication` nastaví výchozí schéma ověřování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="920a6-114">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="920a6-115">`AuthenticationScheme`je užitečné, když je spuštěno více instancí ověřování souborů cookie a vy chcete [autorizaci s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="920a6-115">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="920a6-116">Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma.</span><span class="sxs-lookup"><span data-stu-id="920a6-116">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="920a6-117">Můžete zadat libovolnou hodnotu řetězce, která rozlišuje schéma.</span><span class="sxs-lookup"><span data-stu-id="920a6-117">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="920a6-118">V `Configure` metoda, použijte `UseAuthentication` metoda k vyvolání Middleware ověřování, který nastaví `HttpContext.User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="920a6-118">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="920a6-119">Volání `UseAuthentication` metoda před voláním `UseMvcWithDefaultRoute` nebo `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="920a6-119">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="920a6-120">**Možnosti AddCookie**</span><span class="sxs-lookup"><span data-stu-id="920a6-120">**AddCookie Options**</span></span>

<span data-ttu-id="920a6-121">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) třída se používá ke konfiguraci možností zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-121">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="920a6-122">Možnost</span><span class="sxs-lookup"><span data-stu-id="920a6-122">Option</span></span> | <span data-ttu-id="920a6-123">Popis</span><span class="sxs-lookup"><span data-stu-id="920a6-123">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="920a6-124">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="920a6-124">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="920a6-125">Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) při aktivaci pomocí `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="920a6-125">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="920a6-126">Výchozí hodnota je `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="920a6-126">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="920a6-127">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="920a6-127">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="920a6-128">Vystavitel, který má používat pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost deklarace identity, vytvořených službou souboru cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-128">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="920a6-129">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="920a6-129">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="920a6-130">Název domény, kde je soubor cookie zpracovat.</span><span class="sxs-lookup"><span data-stu-id="920a6-130">The domain name where the cookie is served.</span></span> <span data-ttu-id="920a6-131">Ve výchozím nastavení to je název hostitele žádosti.</span><span class="sxs-lookup"><span data-stu-id="920a6-131">By default, this is the host name of the request.</span></span> <span data-ttu-id="920a6-132">Prohlížeč odesílá pouze soubor cookie v požadavcích na odpovídající název hostitele.</span><span class="sxs-lookup"><span data-stu-id="920a6-132">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="920a6-133">Můžete upravit tak, aby soubory cookie, které jsou k dispozici na každém hostiteli ve vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="920a6-133">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="920a6-134">Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="920a6-134">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="920a6-135">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="920a6-135">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="920a6-136">Získá nebo nastaví životnost souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-136">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="920a6-137">V současné době to možnost Ne ops a bude v ASP.NET Core 2.1 + zastaralé.</span><span class="sxs-lookup"><span data-stu-id="920a6-137">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="920a6-138">Použití `ExpireTimeSpan` nastavit vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-138">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="920a6-139">Další informace najdete v tématu [vysvětlení chování CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="920a6-139">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="920a6-140">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="920a6-140">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="920a6-141">Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="920a6-141">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="920a6-142">Změnou této hodnoty `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může zobrazit v aplikaci krádež souborů cookie musí mít vaše aplikace [skriptování (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="920a6-142">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="920a6-143">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="920a6-143">The default value is `true`.</span></span> |
| [<span data-ttu-id="920a6-144">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="920a6-144">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="920a6-145">Nastaví název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-145">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="920a6-146">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="920a6-146">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="920a6-147">Použít k izolování aplikací běžících na stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="920a6-147">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="920a6-148">Pokud máte aplikace spuštěné v `/app1` a chcete omezit soubory cookie pro tuto aplikaci, nastavte `CookiePath` vlastnost `/app1`.</span><span class="sxs-lookup"><span data-stu-id="920a6-148">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="920a6-149">Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním.</span><span class="sxs-lookup"><span data-stu-id="920a6-149">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="920a6-150">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="920a6-150">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="920a6-151">Určuje, zda má prohlížeč umožnit soubor cookie na připojit ke stejné lokalitě požadavky jenom (`SameSiteMode.Strict`) nebo požadavků webů pomocí bezpečné metod HTTP a žádostí stejné lokalitě (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="920a6-151">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="920a6-152">Pokud nastavíte hodnotu `SameSiteMode.None`, není nastaven hodnotu hlavičky souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-152">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="920a6-153">Všimněte si, že [Middleware zásady souborů Cookie](#cookie-policy-middleware) přepsat hodnotu, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="920a6-153">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="920a6-154">Pro podporu ověřování OAuth, výchozí hodnota je `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="920a6-154">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="920a6-155">Další informace najdete v tématu [poškozený z důvodu zásad SameSite souboru cookie ověřování OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="920a6-155">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="920a6-156">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="920a6-156">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="920a6-157">Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="920a6-157">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="920a6-158">Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="920a6-158">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="920a6-159">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="920a6-159">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="920a6-160">Nastaví `DataProtectionProvider` sloužící k vytvoření výchozích `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="920a6-160">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="920a6-161">Pokud `TicketDataFormat` je vlastnost nastavena, `DataProtectionProvider` možnost nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="920a6-161">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="920a6-162">Pokud není zadaná, se používá výchozí zprostředkovatele ochrany data aplikace.</span><span class="sxs-lookup"><span data-stu-id="920a6-162">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="920a6-163">Události</span><span class="sxs-lookup"><span data-stu-id="920a6-163">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="920a6-164">Obslužná rutina volá metody zprostředkovatele, která poskytnou aplikaci kontrolu v určitých bodech zpracování.</span><span class="sxs-lookup"><span data-stu-id="920a6-164">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="920a6-165">Pokud `Events` není zadáno, je zadaný výchozí instance, který nic neprovádí, pokud jsou volány metody.</span><span class="sxs-lookup"><span data-stu-id="920a6-165">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="920a6-166">EventsType</span><span class="sxs-lookup"><span data-stu-id="920a6-166">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="920a6-167">Použít jako typ služby k získání `Events` instanci místo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="920a6-167">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="920a6-168">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="920a6-168">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="920a6-169">`TimeSpan` Po kterém vyprší platnost lístku ověřování, který je uložený v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-169">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="920a6-170">`ExpireTimeSpan`se přidá do aktuální čas vytvoření dobu vypršení platnosti lístku.</span><span class="sxs-lookup"><span data-stu-id="920a6-170">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="920a6-171">`ExpiredTimeSpan` Hodnotu vždy přejde do šifrované AuthTicket ověřit server.</span><span class="sxs-lookup"><span data-stu-id="920a6-171">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="920a6-172">Může také přejít do [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) záhlaví, ale jenom v případě `IsPersistent` nastavena.</span><span class="sxs-lookup"><span data-stu-id="920a6-172">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="920a6-173">Chcete-li nastavit `IsPersistent` k `true`, nakonfigurujte [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) předaný `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="920a6-173">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="920a6-174">Výchozí hodnota `ExpireTimeSpan` je 14 dnů.</span><span class="sxs-lookup"><span data-stu-id="920a6-174">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="920a6-175">LoginPath</span><span class="sxs-lookup"><span data-stu-id="920a6-175">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="920a6-176">Určuje cestu k poskytování s 302 Found (adresa URL přesměrování) při aktivaci pomocí `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="920a6-176">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="920a6-177">Aktuální adresa URL, která generovala kód 401 je přidána do `LoginPath` jako parametr řetězce dotazu s názvem podle `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="920a6-177">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="920a6-178">Jednou požadavek na `LoginPath` udělí novou identitu přihlášení, `ReturnUrlParameter` hodnota se používá k přesměrování prohlížeče zpět na adresu URL, která způsobila původní kód neautorizovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="920a6-178">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="920a6-179">Výchozí hodnota je `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="920a6-179">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="920a6-180">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="920a6-180">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="920a6-181">Pokud `LogoutPath` je poskytnuta obslužnou rutinu, pak přesměruje požadavek na tuto cestu na základě hodnoty z `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="920a6-181">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="920a6-182">Výchozí hodnota je `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="920a6-182">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="920a6-183">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="920a6-183">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="920a6-184">Určuje název parametru řetězce dotazu, který se připojí obslužnou rutinou pro odpovědi 302 Found (adresa URL přesměrování).</span><span class="sxs-lookup"><span data-stu-id="920a6-184">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="920a6-185">`ReturnUrlParameter`se používá, pokud dorazí požadavek na `LoginPath` nebo `LogoutPath` po provedení akce přihlašovací nebo odhlašovací vrátit v prohlížeči na původní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="920a6-185">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="920a6-186">Výchozí hodnota je `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="920a6-186">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="920a6-187">SessionStore</span><span class="sxs-lookup"><span data-stu-id="920a6-187">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="920a6-188">Volitelný kontejner používá k ukládání identit napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="920a6-188">An optional container used to store identity across requests.</span></span> <span data-ttu-id="920a6-189">Pokud se používá, jenom identifikátor relace je odeslat klientovi.</span><span class="sxs-lookup"><span data-stu-id="920a6-189">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="920a6-190">`SessionStore`slouží k zmírnit potenciální potíže s velkými identitami.</span><span class="sxs-lookup"><span data-stu-id="920a6-190">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="920a6-191">Parametr SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="920a6-191">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="920a6-192">Příznak, který udává, pokud se nový soubor cookie s času aktualizované vypršení platnosti musí být vydávány dynamicky.</span><span class="sxs-lookup"><span data-stu-id="920a6-192">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="920a6-193">To se může stát při všechny žádosti, kde je aktuální doba vypršení platnosti souboru cookie více než 50 % vypršela.</span><span class="sxs-lookup"><span data-stu-id="920a6-193">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="920a6-194">Nové datum vypršení platnosti je dál přesunout na aktuální datum a `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="920a6-194">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="920a6-195">[Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) lze nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="920a6-195">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="920a6-196">Čas vypršení platnosti absolutní lze vylepšit zabezpečení vaší aplikace omezení množství času, který je platný soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-196">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="920a6-197">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="920a6-197">The default value is `true`.</span></span> |
| [<span data-ttu-id="920a6-198">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="920a6-198">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="920a6-199">`TicketDataFormat` Se používá k ochraně a zrušení identity a dalších vlastností, které jsou uložené v hodnotě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-199">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="920a6-200">Pokud není zadaná, `TicketDataFormat` je vytvořený pomocí [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="920a6-200">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="920a6-201">Ověření</span><span class="sxs-lookup"><span data-stu-id="920a6-201">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="920a6-202">Metoda, která kontroluje, zda jsou platné možnosti.</span><span class="sxs-lookup"><span data-stu-id="920a6-202">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="920a6-203">Nastavit `CookieAuthenticationOptions` v konfiguraci služby pro ověřování v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="920a6-203">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="920a6-204">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="920a6-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="920a6-205">Soubor cookie používá ASP.NET Core 1.x [middleware](xref:fundamentals/middleware) který serializuje hlavní název uživatele do šifrovaného souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-205">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="920a6-206">Na následné žádosti, se ověří souboru cookie a znovu vytvořit a přiřadit k objektu zabezpečení `HttpContext.User` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="920a6-206">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="920a6-207">Nainstalujte [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="920a6-207">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="920a6-208">Tento balíček obsahuje middlewaru souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-208">This package contains the cookie middleware.</span></span>

<span data-ttu-id="920a6-209">Použití `UseCookieAuthentication` metoda v `Configure` metoda v vaší *Startup.cs* soubor před `UseMvc` nebo `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="920a6-209">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="920a6-210">**Možnosti CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="920a6-210">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="920a6-211">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) třída se používá ke konfiguraci možností zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-211">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="920a6-212">Možnost</span><span class="sxs-lookup"><span data-stu-id="920a6-212">Option</span></span> | <span data-ttu-id="920a6-213">Popis</span><span class="sxs-lookup"><span data-stu-id="920a6-213">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="920a6-214">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="920a6-214">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="920a6-215">Nastaví schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-215">Sets the authentication scheme.</span></span> <span data-ttu-id="920a6-216">`AuthenticationScheme`je užitečné, když je spuštěno více instancí ověřování a vy chcete [autorizaci s konkrétní schéma](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="920a6-216">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="920a6-217">Nastavení `AuthenticationScheme` k `CookieAuthenticationDefaults.AuthenticationScheme` poskytuje hodnotu "Soubory cookie" pro schéma.</span><span class="sxs-lookup"><span data-stu-id="920a6-217">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="920a6-218">Můžete zadat libovolnou hodnotu řetězce, která rozlišuje schéma.</span><span class="sxs-lookup"><span data-stu-id="920a6-218">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="920a6-219">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="920a6-219">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="920a6-220">Nastaví hodnotu, znamená, že ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.</span><span class="sxs-lookup"><span data-stu-id="920a6-220">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="920a6-221">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="920a6-221">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="920a6-222">V případě hodnoty true zpracovává middleware ověřování automatické problémy.</span><span class="sxs-lookup"><span data-stu-id="920a6-222">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="920a6-223">Pokud hodnotu false, middleware ověřování jenom mění odpovědi, když explicitně uvedené `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="920a6-223">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="920a6-224">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="920a6-224">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="920a6-225">Vystavitel, který má používat pro [vystavitele](/dotnet/api/system.security.claims.claim.issuer) vlastnost deklarace identity, vytvořené middleware ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-225">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="920a6-226">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="920a6-226">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="920a6-227">Název domény, kde je soubor cookie zpracovat.</span><span class="sxs-lookup"><span data-stu-id="920a6-227">The domain name where the cookie is served.</span></span> <span data-ttu-id="920a6-228">Ve výchozím nastavení to je název hostitele žádosti.</span><span class="sxs-lookup"><span data-stu-id="920a6-228">By default, this is the host name of the request.</span></span> <span data-ttu-id="920a6-229">V prohlížeči slouží pouze soubor cookie na odpovídající název hostitele.</span><span class="sxs-lookup"><span data-stu-id="920a6-229">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="920a6-230">Můžete upravit tak, aby soubory cookie, které jsou k dispozici na každém hostiteli ve vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="920a6-230">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="920a6-231">Například nastavení domény souboru cookie na `.contoso.com` ji zpřístupní k `contoso.com`, `www.contoso.com`, a `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="920a6-231">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="920a6-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="920a6-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="920a6-233">Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="920a6-233">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="920a6-234">Změnou této hodnoty `false` povoluje skripty na straně klienta pro přístup k souboru cookie a může zobrazit v aplikaci krádež souborů cookie musí mít vaše aplikace [skriptování (XSS)](xref:security/cross-site-scripting) ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="920a6-234">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="920a6-235">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="920a6-235">The default value is `true`.</span></span> |
| [<span data-ttu-id="920a6-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="920a6-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="920a6-237">Použít k izolování aplikací běžících na stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="920a6-237">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="920a6-238">Pokud máte aplikace spuštěné v `/app1` a chcete omezit soubory cookie pro tuto aplikaci, nastavte `CookiePath` vlastnost `/app1`.</span><span class="sxs-lookup"><span data-stu-id="920a6-238">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="920a6-239">Díky tomu souboru cookie, který je k dispozici pouze u požadavků na `/app1` a všechny aplikace pod ním.</span><span class="sxs-lookup"><span data-stu-id="920a6-239">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="920a6-240">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="920a6-240">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="920a6-241">Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="920a6-241">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="920a6-242">Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="920a6-242">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="920a6-243">Popis</span><span class="sxs-lookup"><span data-stu-id="920a6-243">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="920a6-244">Další informace o typu ověřování, který je k dispozici na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="920a6-244">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="920a6-245">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="920a6-245">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="920a6-246">`TimeSpan` Po kterém vyprší platnost lístku ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-246">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="920a6-247">Přidá na aktuální čas vytvoření dobu vypršení platnosti lístku.</span><span class="sxs-lookup"><span data-stu-id="920a6-247">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="920a6-248">Použít `ExpireTimeSpan`, je nutné nastavit `IsPersistent` k `true` v `AuthenticationProperties` předaný `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="920a6-248">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="920a6-249">Výchozí hodnota je 14 dnů.</span><span class="sxs-lookup"><span data-stu-id="920a6-249">The default value is 14 days.</span></span> |
| [<span data-ttu-id="920a6-250">Parametr SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="920a6-250">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="920a6-251">Příznak, který udává, zda datum vypršení platnosti souboru cookie resetuje při více než polovinu `ExpireTimeSpan` uplynutí intervalu.</span><span class="sxs-lookup"><span data-stu-id="920a6-251">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="920a6-252">Nový čas exipiration se dál přesune na aktuální datum a `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="920a6-252">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="920a6-253">[Čas vypršení platnosti souboru cookie absolutní](xref:security/authentication/cookie#absolute-cookie-expiration) lze nastavit pomocí `AuthenticationProperties` třídy při volání metody `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="920a6-253">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="920a6-254">Čas vypršení platnosti absolutní lze vylepšit zabezpečení vaší aplikace omezení množství času, který je platný soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-254">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="920a6-255">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="920a6-255">The default value is `true`.</span></span> |

<span data-ttu-id="920a6-256">Nastavit `CookieAuthenticationOptions` pro Middleware ověřování souborů Cookie v `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="920a6-256">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="920a6-257">Middlewaru souboru cookie zásad.</span><span class="sxs-lookup"><span data-stu-id="920a6-257">Cookie Policy Middleware</span></span>

<span data-ttu-id="920a6-258">[Middleware zásady souborů cookie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) povoluje možnosti zásad souborů cookie v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="920a6-258">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="920a6-259">Přidání middleware aplikace zpracování kanálu je pořadí citlivé; ovlivňuje pouze komponenty registrované po ho v kanálu.</span><span class="sxs-lookup"><span data-stu-id="920a6-259">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="920a6-260">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) poskytované middlewaru souboru Cookie zásady umožňují řídit globální vlastnosti souboru cookie zpracování a háku do obslužné rutiny zpracování souboru cookie, když jsou soubory cookie připojí nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="920a6-260">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="920a6-261">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="920a6-261">Property</span></span> | <span data-ttu-id="920a6-262">Popis</span><span class="sxs-lookup"><span data-stu-id="920a6-262">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="920a6-263">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="920a6-263">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="920a6-264">Ovlivňuje, jestli HttpOnly musí být soubory cookie, který je příznak označující, pokud soubor cookie musí být přístupný jenom pro servery.</span><span class="sxs-lookup"><span data-stu-id="920a6-264">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="920a6-265">Výchozí hodnota je `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="920a6-265">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="920a6-266">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="920a6-266">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="920a6-267">Ovlivňuje stejné lokalitě atribut souboru cookie (viz níže).</span><span class="sxs-lookup"><span data-stu-id="920a6-267">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="920a6-268">Výchozí hodnota je `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="920a6-268">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="920a6-269">Tato možnost je k dispozici pro jádro ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="920a6-269">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="920a6-270">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="920a6-270">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="920a6-271">Voláno, když se připojí do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-271">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="920a6-272">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="920a6-272">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="920a6-273">Volá se při odstranění souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-273">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="920a6-274">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="920a6-274">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="920a6-275">Ovlivňuje, jestli soubory cookie musí být zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="920a6-275">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="920a6-276">Výchozí hodnota je `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="920a6-276">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="920a6-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + pouze)</span><span class="sxs-lookup"><span data-stu-id="920a6-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="920a6-278">Výchozí hodnota `MinimumSameSitePolicy` hodnota je `SameSiteMode.Lax` pro povolení ověřování OAuth2.</span><span class="sxs-lookup"><span data-stu-id="920a6-278">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="920a6-279">Výhradně vynutit zásady stejné lokalitě `SameSiteMode.Strict`, nastavte `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="920a6-279">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="920a6-280">I když toto nastavení dělí OAuth2 a jiná schémata ověřování cross-origin, zvyšuje úroveň zabezpečení souboru cookie pro ostatní typy aplikací, které nejsou závisí na zpracování požadavků mezi zdroji.</span><span class="sxs-lookup"><span data-stu-id="920a6-280">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="920a6-281">Nastavení zásad middlewaru souboru Cookie pro `MinimumSameSitePolicy` může mít vliv na vaše nastavení `Cookie.SameSite` v `CookieAuthenticationOptions` nastavení podle níže uvedené tabulky.</span><span class="sxs-lookup"><span data-stu-id="920a6-281">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="920a6-282">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="920a6-282">MinimumSameSitePolicy</span></span> | <span data-ttu-id="920a6-283">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="920a6-283">Cookie.SameSite</span></span> | <span data-ttu-id="920a6-284">Výsledná nastavení Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="920a6-284">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="920a6-285">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="920a6-285">SameSiteMode.None</span></span>     | <span data-ttu-id="920a6-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="920a6-286">SameSiteMode.None</span></span><br><span data-ttu-id="920a6-287">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-287">SameSiteMode.Lax</span></span><br><span data-ttu-id="920a6-288">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-288">SameSiteMode.Strict</span></span> | <span data-ttu-id="920a6-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="920a6-289">SameSiteMode.None</span></span><br><span data-ttu-id="920a6-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="920a6-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-291">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="920a6-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-292">SameSiteMode.Lax</span></span>      | <span data-ttu-id="920a6-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="920a6-293">SameSiteMode.None</span></span><br><span data-ttu-id="920a6-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="920a6-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="920a6-296">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-296">SameSiteMode.Lax</span></span><br><span data-ttu-id="920a6-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="920a6-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="920a6-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-299">SameSiteMode.Strict</span></span>   | <span data-ttu-id="920a6-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="920a6-300">SameSiteMode.None</span></span><br><span data-ttu-id="920a6-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="920a6-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="920a6-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="920a6-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-303">SameSiteMode.Strict</span></span><br><span data-ttu-id="920a6-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="920a6-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="920a6-305">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="920a6-306">Vytvoření souboru cookie pro ověřování</span><span class="sxs-lookup"><span data-stu-id="920a6-306">Creating an authentication cookie</span></span>

<span data-ttu-id="920a6-307">Pokud chcete vytvořit soubor cookie, která uchovává informace o uživateli, je nutné vytvořit [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="920a6-307">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="920a6-308">Informace o uživateli se serializovat a uloží do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="920a6-308">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="920a6-309">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="920a6-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="920a6-310">Volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) k přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="920a6-310">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="920a6-311">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="920a6-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="920a6-312">Volání [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) k přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="920a6-312">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="920a6-313">`SignInAsync`Vytvoří šifrovaného souboru cookie a přidává ji k aktuální odpovědi.</span><span class="sxs-lookup"><span data-stu-id="920a6-313">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="920a6-314">Pokud neurčíte `AuthenticationScheme`, se používá výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="920a6-314">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="920a6-315">V pozadí, je šifrování, které používá ASP.NET Core [ochrany dat](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) systému.</span><span class="sxs-lookup"><span data-stu-id="920a6-315">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="920a6-316">Pokud jste hostování aplikace na více počítačů, Vyrovnávání zatížení napříč aplikací nebo pomocí webové farmy, pak musíte [konfigurovat ochranu dat](xref:security/data-protection/configuration/overview) pro stejný klíč prstenec a identifikátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="920a6-316">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="920a6-317">Odhlásit</span><span class="sxs-lookup"><span data-stu-id="920a6-317">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="920a6-318">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="920a6-318">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="920a6-319">Odhlásit se aktuálního uživatele a odstranit jejich souboru cookie, volání [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="920a6-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="920a6-320">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="920a6-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="920a6-321">Odhlásit se aktuálního uživatele a odstranit jejich souboru cookie, volání [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="920a6-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="920a6-322">Pokud nepoužíváte `CookieAuthenticationDefaults.AuthenticationScheme` (nebo "soubory cookie.) jako schéma (například" ContosoCookie"), zadejte schéma, které jste použili při konfiguraci zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="920a6-322">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="920a6-323">Jinak je použit výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="920a6-323">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="920a6-324">Reaguje na změny back-end</span><span class="sxs-lookup"><span data-stu-id="920a6-324">Reacting to back-end changes</span></span>

<span data-ttu-id="920a6-325">Po vytvoření souboru cookie bude jediným zdrojem identity.</span><span class="sxs-lookup"><span data-stu-id="920a6-325">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="920a6-326">I v případě, že zakážete uživatele v systémech back-end, nemá žádné informace o tento soubor cookie ověřování systému a uživatel zůstane přihlášený tak dlouho, dokud jejich soubor cookie je neplatný.</span><span class="sxs-lookup"><span data-stu-id="920a6-326">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="920a6-327">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) událost v ASP.NET Core 2.x nebo [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metoda v ASP.NET Core 1.x lze použít pro zachycení a přepsat ověření souboru cookie identity.</span><span class="sxs-lookup"><span data-stu-id="920a6-327">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="920a6-328">Tento přístup snižuje riziko odvolané uživatele, kteří používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="920a6-328">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="920a6-329">Jeden ze způsobů ověření souboru cookie je založena na udržování přehledu o při změně databáze uživatelů.</span><span class="sxs-lookup"><span data-stu-id="920a6-329">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="920a6-330">Pokud databáze nebyla změněna od uživatele soubor cookie vydala, není nutné opakované ověření uživatele, pokud jejich souborů cookie je stále platné.</span><span class="sxs-lookup"><span data-stu-id="920a6-330">If the database hasn't been changed since the user's cookie was issued, there is no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="920a6-331">K implementaci databázi, která je implementovaná v tomto scénáři `IUserRepository` v tomto příkladu ukládá `LastChanged` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="920a6-331">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="920a6-332">Při aktualizaci všechny uživatele v databázi, `LastChanged` hodnota nastavena na aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="920a6-332">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="920a6-333">Chcete-li zrušit platnost souboru cookie, pokud změny databáze v závislosti na `LastChanged` hodnotu, vytvořte soubor cookie s `LastChanged` deklarace identity, který obsahuje aktuální `LastChanged` hodnotu z databáze:</span><span class="sxs-lookup"><span data-stu-id="920a6-333">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="920a6-334">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="920a6-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="920a6-335">K přepsání pro implementaci `ValidatePrincipal` událostí, zápisu a metoda podpisem následující třídy, které pocházejí z [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="920a6-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="920a6-336">Příklad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="920a6-336">An example looks like the following:</span></span>

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

<span data-ttu-id="920a6-337">Registrace instance události při registraci služby souborů cookie v `ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="920a6-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="920a6-338">Zadejte registraci vymezené služby pro vaše `CustomCookieAuthenticationEvents` třídy:</span><span class="sxs-lookup"><span data-stu-id="920a6-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="920a6-339">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="920a6-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="920a6-340">K přepsání pro implementaci `ValidateAsync` událostí, zápisu a metoda podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="920a6-340">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="920a6-341">ASP.NET Core Identity implementuje tato kontrola v rámci jeho [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="920a6-341">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="920a6-342">Příklad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="920a6-342">An example looks like the following:</span></span>

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

<span data-ttu-id="920a6-343">Během konfigurace ověřování souborů cookie v registraci události `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="920a6-343">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="920a6-344">Představte si situaci, ve kterém se aktualizuje uživatelské jméno &mdash; rozhodnutí, které nemají vliv na zabezpečení žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="920a6-344">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="920a6-345">Pokud chcete aktualizovat nedestruktivně hlavní název uživatele, zavolejte `context.ReplacePrincipal` a nastavte `context.ShouldRenew` vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="920a6-345">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="920a6-346">Přístup v rámci popsaného se aktivuje u každého požadavku.</span><span class="sxs-lookup"><span data-stu-id="920a6-346">The approach described here is triggered on every request.</span></span> <span data-ttu-id="920a6-347">To může způsobit snížení výkonu velký pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="920a6-347">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="920a6-348">Trvalé soubory cookie</span><span class="sxs-lookup"><span data-stu-id="920a6-348">Persistent cookies</span></span>

<span data-ttu-id="920a6-349">Můžete chtít soubor cookie k uchování v rámci relací prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="920a6-349">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="920a6-350">Tato trvalosti by měl povolit pouze s souhlas explicitní uživatele s "Zapamatovat uživatele" zaškrtávací políčko je na přihlášení nebo podobné mechanismus.</span><span class="sxs-lookup"><span data-stu-id="920a6-350">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="920a6-351">Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který odolává prostřednictvím prohlížeče uzavření.</span><span class="sxs-lookup"><span data-stu-id="920a6-351">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="920a6-352">Jsou dodržení nastavení klouzavé vypršení platnosti předtím nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="920a6-352">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="920a6-353">Pokud při zavření prohlížeče vyprší platnost souboru cookie, prohlížeč vymaže soubor cookie po jeho restartování.</span><span class="sxs-lookup"><span data-stu-id="920a6-353">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="920a6-354">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="920a6-354">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="920a6-355">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) třída se nachází v `Microsoft.AspNetCore.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="920a6-355">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="920a6-356">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="920a6-356">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="920a6-357">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) třída se nachází v `Microsoft.AspNetCore.Http.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="920a6-357">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="920a6-358">Platnost absolutní souborů cookie</span><span class="sxs-lookup"><span data-stu-id="920a6-358">Absolute cookie expiration</span></span>

<span data-ttu-id="920a6-359">Můžete nastavit čas vypršení platnosti absolutní s `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="920a6-359">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="920a6-360">Musíte taky nastavit `IsPersistent`, jinak hodnota `ExpiresUtc` je ignorován a k vytvoření souboru cookie jedné relace.</span><span class="sxs-lookup"><span data-stu-id="920a6-360">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="920a6-361">Když `ExpiresUtc` je nastavený na `SignInAsync`, přepíše hodnotu `ExpireTimeSpan` možnost `CookieAuthenticationOptions`, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="920a6-361">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="920a6-362">Následující fragment kódu vytvoří identitu a odpovídající soubor cookie, který trvá po dobu 20 minut.</span><span class="sxs-lookup"><span data-stu-id="920a6-362">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="920a6-363">To bude ignorovat nastavení klouzavé vypršení platnosti předtím nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="920a6-363">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="920a6-364">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="920a6-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="920a6-365">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="920a6-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="see-also"></a><span data-ttu-id="920a6-366">Viz také</span><span class="sxs-lookup"><span data-stu-id="920a6-366">See also</span></span>

* [<span data-ttu-id="920a6-367">Změny auth 2.0 nebo migrace oznámení</span><span class="sxs-lookup"><span data-stu-id="920a6-367">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="920a6-368">Omezení identity schématem</span><span class="sxs-lookup"><span data-stu-id="920a6-368">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
