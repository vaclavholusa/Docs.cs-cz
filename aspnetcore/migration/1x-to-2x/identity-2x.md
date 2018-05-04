---
title: Migrujte na technologii ASP.NET 2.0 základní ověřování a identita
author: scottaddie
description: Tento článek popisuje nejběžnější kroky pro migraci ASP.NET Core 1.x ověřování a identita na technologii ASP.NET 2.0 jádra.
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 0653906996f9f37d436ebefc6a738d2603788d53
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="0ea03-103">Migrujte na technologii ASP.NET 2.0 základní ověřování a identita</span><span class="sxs-lookup"><span data-stu-id="0ea03-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="0ea03-104">Podle [Scott Addie](https://github.com/scottaddie) a [ani Haovi společnosti](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="0ea03-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="0ea03-105">Jádro ASP.NET 2.0 obsahuje nový model pro ověřování a [Identity](xref:security/authentication/identity) který zjednodušuje konfiguraci pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="0ea03-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="0ea03-106">ASP.NET Core 1.x aplikací, které používají ověřování nebo Identity můžete aktualizovat, a použít nový model jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0ea03-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="0ea03-107">Middleware ověřování a služby</span><span class="sxs-lookup"><span data-stu-id="0ea03-107">Authentication Middleware and services</span></span>
<span data-ttu-id="0ea03-108">V projektech 1.x je nakonfigurované ověřování prostřednictvím middlewaru.</span><span class="sxs-lookup"><span data-stu-id="0ea03-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="0ea03-109">Middleware metoda je volána pro každé schéma ověřování, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="0ea03-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="0ea03-110">Následující příklad 1.x nakonfiguruje ověřování Facebook s identitou v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

<span data-ttu-id="0ea03-111">V 2.0 projektů je nakonfigurované ověřování prostřednictvím služeb.</span><span class="sxs-lookup"><span data-stu-id="0ea03-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="0ea03-112">Každé schéma ověřování je zaregistrován v `ConfigureServices` metodu *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0ea03-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="0ea03-113">`UseIdentity` Se nahradí metoda `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="0ea03-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="0ea03-114">Následující příklad 2.0 nakonfiguruje ověřování Facebook s identitou v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="0ea03-115">`UseAuthentication` Metoda přidá komponenta middlewaru jednotné ověřování, která je zodpovědná za automatické ověření a zpracování žádostí o vzdálené ověření.</span><span class="sxs-lookup"><span data-stu-id="0ea03-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="0ea03-116">Nahradí všechny komponenty middlewaru jednotlivých se součástí jedné, běžné middleware.</span><span class="sxs-lookup"><span data-stu-id="0ea03-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="0ea03-117">Níže jsou uvedeny pokyny k migraci 2.0 pro každé schéma hlavní ověřování.</span><span class="sxs-lookup"><span data-stu-id="0ea03-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="0ea03-118">Ověřování na základě souborů cookie</span><span class="sxs-lookup"><span data-stu-id="0ea03-118">Cookie-based authentication</span></span>
<span data-ttu-id="0ea03-119">Vyberte jednu z následujících dvou možností a proveďte potřebné změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="0ea03-120">Použít soubory cookie s identitou</span><span class="sxs-lookup"><span data-stu-id="0ea03-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="0ea03-121">Nahraďte `UseIdentity` s `UseAuthentication` v `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="0ea03-122">Vyvolání `AddIdentity` metoda v `ConfigureServices` metoda pro přidání služby ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="0ea03-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="0ea03-123">Volitelně můžete vyvolat `ConfigureApplicationCookie` nebo `ConfigureExternalCookie` metoda v `ConfigureServices` metodu za účelem vylepšení nastavení souborů cookie Identity.</span><span class="sxs-lookup"><span data-stu-id="0ea03-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="0ea03-124">Použít soubory cookie bez Identity</span><span class="sxs-lookup"><span data-stu-id="0ea03-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="0ea03-125">Nahraďte `UseCookieAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="0ea03-126">Vyvolání `AddAuthentication` a `AddCookie` metody v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="0ea03-127">Ověřování nosiče JWT</span><span class="sxs-lookup"><span data-stu-id="0ea03-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="0ea03-128">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0ea03-129">Nahraďte `UseJwtBearerAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0ea03-130">Vyvolání `AddJwtBearer` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="0ea03-131">Tento fragment kódu nepoužívá identitu, abyste výchozí schéma musí nastavit předáním `JwtBearerDefaults.AuthenticationScheme` k `AddAuthentication` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ea03-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="0ea03-132">Ověřování OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="0ea03-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="0ea03-133">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="0ea03-134">Nahraďte `UseOpenIdConnectAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0ea03-135">Vyvolání `AddOpenIdConnect` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="0ea03-136">ověřování Facebook</span><span class="sxs-lookup"><span data-stu-id="0ea03-136">Facebook authentication</span></span>
<span data-ttu-id="0ea03-137">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0ea03-138">Nahraďte `UseFacebookAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0ea03-139">Vyvolání `AddFacebook` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="0ea03-140">Ověřování Google</span><span class="sxs-lookup"><span data-stu-id="0ea03-140">Google authentication</span></span>
<span data-ttu-id="0ea03-141">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0ea03-142">Nahraďte `UseGoogleAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0ea03-143">Vyvolání `AddGoogle` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="0ea03-144">Ověřování Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="0ea03-144">Microsoft Account authentication</span></span>
<span data-ttu-id="0ea03-145">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0ea03-146">Nahraďte `UseMicrosoftAccountAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0ea03-147">Vyvolání `AddMicrosoftAccount` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="0ea03-148">Ověřování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="0ea03-148">Twitter authentication</span></span>
<span data-ttu-id="0ea03-149">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0ea03-150">Nahraďte `UseTwitterAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0ea03-151">Vyvolání `AddTwitter` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="0ea03-152">Schémat ověřování výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="0ea03-152">Setting default authentication schemes</span></span>
<span data-ttu-id="0ea03-153">V 1.x `AutomaticAuthenticate` a `AutomaticChallenge` vlastnosti [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) základní třída měla má nastavit na jedno schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="0ea03-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="0ea03-154">Došlo k dispozici žádné dobrý způsob, jak to vynucovat.</span><span class="sxs-lookup"><span data-stu-id="0ea03-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="0ea03-155">V rámci 2.0, byly odebrány tyto dvě vlastnosti jako vlastnosti na jednotlivých `AuthenticationOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="0ea03-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="0ea03-156">Lze nastavit v `AddAuthentication` volání metody v rámci `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="0ea03-157">V předchozím fragmentu kódu, výchozí schéma je nastavený na `CookieAuthenticationDefaults.AuthenticationScheme` (dále jen "soubory cookie.).</span><span class="sxs-lookup"><span data-stu-id="0ea03-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="0ea03-158">Můžete taky použít přetížené verzi `AddAuthentication` metodu a nastavit víc než jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0ea03-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="0ea03-159">V následujícím příkladu přetížené metody, nebo výchozí schéma je nastaveno na `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0ea03-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="0ea03-160">Schéma ověřování případně lze zadat v rámci vaší individuální `[Authorize]` atributy nebo zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="0ea03-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="0ea03-161">Definujte výchozí schéma v 2.0, pokud platí jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="0ea03-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="0ea03-162">Má-li být automaticky přihlášeni uživatel</span><span class="sxs-lookup"><span data-stu-id="0ea03-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="0ea03-163">Můžete použít `[Authorize]` atribut nebo autorizační zásady bez zadání schémata</span><span class="sxs-lookup"><span data-stu-id="0ea03-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="0ea03-164">Výjimkou z tohoto pravidla je `AddIdentity` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ea03-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="0ea03-165">Tato metoda přidá soubory cookie a nastaví výchozí ověřit a challenge schémata pro soubor cookie aplikace `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0ea03-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="0ea03-166">Kromě toho nastaví výchozí přihlašovací schéma externí soubor cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="0ea03-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="0ea03-167">Použití rozšíření HttpContext ověřování</span><span class="sxs-lookup"><span data-stu-id="0ea03-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="0ea03-168">`IAuthenticationManager` Rozhraní je hlavní vstupní bod do systému 1.x ověřování.</span><span class="sxs-lookup"><span data-stu-id="0ea03-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="0ea03-169">Byla nahrazena novou sadu `HttpContext` rozšiřující metody v `Microsoft.AspNetCore.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="0ea03-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="0ea03-170">Například 1.x projekty odkaz `Authentication` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="0ea03-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="0ea03-171">V projektech 2.0, importovat `Microsoft.AspNetCore.Authentication` obor názvů a odstranit `Authentication` vlastnost odkazy:</span><span class="sxs-lookup"><span data-stu-id="0ea03-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="0ea03-172">Ověřování systému Windows (soubor HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="0ea03-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="0ea03-173">Existují dvě varianty ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="0ea03-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="0ea03-174">Hostitel pouze umožňuje ověřeným uživatelům</span><span class="sxs-lookup"><span data-stu-id="0ea03-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="0ea03-175">Obě hostitele umožňuje anonymní a ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="0ea03-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="0ea03-176">První variace popsané výše je ovlivněna 2.0 změnami.</span><span class="sxs-lookup"><span data-stu-id="0ea03-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="0ea03-177">Druhá popsané výše je ovlivňován 2.0 změny.</span><span class="sxs-lookup"><span data-stu-id="0ea03-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="0ea03-178">Například je může povolovat anonymní uživatelé aplikaci za služby IIS nebo [HTTP.sys](xref:fundamentals/servers/weblistener) vrstvy ale autorizací uživatele na úrovni Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0ea03-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="0ea03-179">V tomto scénáři nastavena výchozí schéma `IISDefaults.AuthenticationScheme` v `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="0ea03-180">Nepodařilo se nastavit výchozí schéma odpovídajícím způsobem brání vyzve z pracovní požadavku authorize.</span><span class="sxs-lookup"><span data-stu-id="0ea03-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="0ea03-181">IdentityCookieOptions instancí</span><span class="sxs-lookup"><span data-stu-id="0ea03-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="0ea03-182">Vedlejším účinkem 2.0 změny je přepínač tak, aby pomocí možnosti místo instancí možnosti souborů cookie s názvem.</span><span class="sxs-lookup"><span data-stu-id="0ea03-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="0ea03-183">Odeberou se přizpůsobit schéma názvy souborů cookie Identity.</span><span class="sxs-lookup"><span data-stu-id="0ea03-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="0ea03-184">Například 1.x projekty použití [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) předat `IdentityCookieOptions` parametr do *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0ea03-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="0ea03-185">Schéma ověřování externí soubor cookie je k němu přistupovat z zadanou instanci:</span><span class="sxs-lookup"><span data-stu-id="0ea03-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="0ea03-186">Vkládání zmíněnými konstruktor stane nepotřebné 2.0 projektů a `_externalCookieScheme` pole můžete odstranit:</span><span class="sxs-lookup"><span data-stu-id="0ea03-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="0ea03-187">`IdentityConstants.ExternalScheme` Konstanta lze přímo:</span><span class="sxs-lookup"><span data-stu-id="0ea03-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="0ea03-188">Přidat IdentityUser objektů POCO navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="0ea03-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="0ea03-189">Navigační vlastnosti Entity Framework (EF) základní základu `IdentityUser` objektů POCO (prostý původní objekt CLR) byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="0ea03-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="0ea03-190">Pokud projekt 1.x používá tyto vlastnosti, ručně přidáte zpět 2.0 projektu:</span><span class="sxs-lookup"><span data-stu-id="0ea03-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="0ea03-191">Při spuštění migrace základní EF nechcete, aby duplicitní cizí klíče, přidejte následující příkaz pro vaše `IdentityDbContext` třída `OnModelCreating` – metoda (po `base.OnModelCreating();` volání):</span><span class="sxs-lookup"><span data-stu-id="0ea03-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="0ea03-192">Nahraďte GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="0ea03-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="0ea03-193">Synchronní metoda `GetExternalAuthenticationSchemes` byla odebrána považuje asynchronní verzi.</span><span class="sxs-lookup"><span data-stu-id="0ea03-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="0ea03-194">projekty 1.x mají následující kód v *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ea03-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="0ea03-195">Tato metoda se zobrazí v *Login.cshtml* příliš:</span><span class="sxs-lookup"><span data-stu-id="0ea03-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="0ea03-196">V projektech 2.0, použijte `GetExternalAuthenticationSchemesAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="0ea03-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="0ea03-197">V *Login.cshtml*, `AuthenticationScheme` vlastnost přistupovat v `foreach` smyčky změny `Name`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="0ea03-198">Změna vlastnosti ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="0ea03-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="0ea03-199">A `ManageLoginsViewModel` objekt se používá v `ManageLogins` akce *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0ea03-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="0ea03-200">V 1.x projekty, objekt je `OtherLogins` vlastnost návratový typ je `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="0ea03-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="0ea03-201">Vyžaduje tento návratový typ importu `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="0ea03-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="0ea03-202">V projektech 2.0, návratový typ se změní na `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="0ea03-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="0ea03-203">Vyžaduje tento nový návratový typ nahrazení `Microsoft.AspNetCore.Http.Authentication` importovat s `Microsoft.AspNetCore.Authentication` importovat.</span><span class="sxs-lookup"><span data-stu-id="0ea03-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="0ea03-204">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0ea03-204">Additional resources</span></span>
<span data-ttu-id="0ea03-205">Diskusní a další podrobnosti najdete v tématu [diskusi k ověřování 2.0](https://github.com/aspnet/Security/issues/1338) problém na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0ea03-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
