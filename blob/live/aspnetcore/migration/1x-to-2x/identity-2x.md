---
title: "Migrace ověřování a identita na jádro ASP.NET 2.0"
author: scottaddie
description: "Tento článek popisuje nejběžnější kroky pro migraci ASP.NET Core 1.x ověřování a identita na technologii ASP.NET 2.0 jádra."
keywords: "Ověřování ASP.NET Core, Identity,"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 1d8c75a21cd7110b3e414f0c600e9f05cbaeff45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="5ff71-104">Migrace ověřování a identita na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="5ff71-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="5ff71-105">Podle [Scott Addie](https://github.com/scottaddie) a [ani Haovi společnosti](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="5ff71-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="5ff71-106">Jádro ASP.NET 2.0 obsahuje nový model pro ověřování a [Identity](xref:security/authentication/identity) který zjednodušuje konfiguraci pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="5ff71-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="5ff71-107">ASP.NET Core 1.x aplikací, které používají ověřování nebo Identity můžete aktualizovat, a použít nový model jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="5ff71-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="5ff71-108">Middleware ověřování a služby</span><span class="sxs-lookup"><span data-stu-id="5ff71-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="5ff71-109">V projektech 1.x je nakonfigurované ověřování prostřednictvím middlewaru.</span><span class="sxs-lookup"><span data-stu-id="5ff71-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="5ff71-110">Middleware metoda je volána pro každé schéma ověřování, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="5ff71-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="5ff71-111">Následující příklad 1.x nakonfiguruje ověřování Facebook s identitou v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="5ff71-112">V 2.0 projektů je nakonfigurované ověřování prostřednictvím služeb.</span><span class="sxs-lookup"><span data-stu-id="5ff71-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="5ff71-113">Každé schéma ověřování je zaregistrován v `ConfigureServices` metodu *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ff71-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="5ff71-114">`UseIdentity` Se nahradí metoda `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="5ff71-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="5ff71-115">Následující příklad 2.0 nakonfiguruje ověřování Facebook s identitou v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="5ff71-116">`UseAuthentication` Metoda přidá komponenta middlewaru jednotné ověřování, která je zodpovědná za automatické ověření a zpracování žádostí o vzdálené ověření.</span><span class="sxs-lookup"><span data-stu-id="5ff71-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="5ff71-117">Nahradí všechny komponenty middlewaru jednotlivých se součástí jedné, běžné middleware.</span><span class="sxs-lookup"><span data-stu-id="5ff71-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="5ff71-118">Níže jsou uvedeny pokyny k migraci 2.0 pro každé schéma hlavní ověřování.</span><span class="sxs-lookup"><span data-stu-id="5ff71-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="5ff71-119">Ověřování na základě souborů cookie</span><span class="sxs-lookup"><span data-stu-id="5ff71-119">Cookie-based Authentication</span></span>
<span data-ttu-id="5ff71-120">Vyberte jednu z následujících dvou možností a proveďte potřebné změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="5ff71-121">Použít soubory cookie s identitou</span><span class="sxs-lookup"><span data-stu-id="5ff71-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="5ff71-122">Nahraďte `UseIdentity` s `UseAuthentication` v `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="5ff71-123">Vyvolání `AddIdentity` metoda v `ConfigureServices` metoda pro přidání služby ověřování souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="5ff71-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="5ff71-124">Volitelně můžete vyvolat `ConfigureApplicationCookie` nebo `ConfigureExternalCookie` metoda v `ConfigureServices` metodu za účelem vylepšení nastavení souborů cookie Identity.</span><span class="sxs-lookup"><span data-stu-id="5ff71-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="5ff71-125">Použít soubory cookie bez Identity</span><span class="sxs-lookup"><span data-stu-id="5ff71-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="5ff71-126">Nahraďte `UseCookieAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="5ff71-127">Vyvolání `AddAuthentication` a `AddCookie` metody v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="5ff71-128">Ověřování nosiče JWT</span><span class="sxs-lookup"><span data-stu-id="5ff71-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="5ff71-129">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="5ff71-130">Nahraďte `UseJwtBearerAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="5ff71-131">Vyvolání `AddJwtBearer` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="5ff71-132">Tento fragment kódu nepoužívá identitu, abyste výchozí schéma musí nastavit předáním `JwtBearerDefaults.AuthenticationScheme` k `AddAuthentication` metoda.</span><span class="sxs-lookup"><span data-stu-id="5ff71-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="5ff71-133">OpenID Connect ověřování (OIDC)</span><span class="sxs-lookup"><span data-stu-id="5ff71-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="5ff71-134">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="5ff71-135">Nahraďte `UseOpenIdConnectAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="5ff71-136">Vyvolání `AddOpenIdConnect` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="5ff71-137">Ověřování Facebook</span><span class="sxs-lookup"><span data-stu-id="5ff71-137">Facebook Authentication</span></span>
<span data-ttu-id="5ff71-138">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="5ff71-139">Nahraďte `UseFacebookAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="5ff71-140">Vyvolání `AddFacebook` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="5ff71-141">Ověřování Google</span><span class="sxs-lookup"><span data-stu-id="5ff71-141">Google Authentication</span></span>
<span data-ttu-id="5ff71-142">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="5ff71-143">Nahraďte `UseGoogleAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="5ff71-144">Vyvolání `AddGoogle` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="5ff71-145">Ověření účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="5ff71-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="5ff71-146">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="5ff71-147">Nahraďte `UseMicrosoftAccountAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="5ff71-148">Vyvolání `AddMicrosoftAccount` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="5ff71-149">Ověřování služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="5ff71-149">Twitter Authentication</span></span>
<span data-ttu-id="5ff71-150">Proveďte následující změny v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="5ff71-151">Nahraďte `UseTwitterAuthentication` volání metody `Configure` metoda s `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="5ff71-152">Vyvolání `AddTwitter` metoda v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="5ff71-153">Schémat ověřování výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="5ff71-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="5ff71-154">V 1.x `AutomaticAuthenticate` a `AutomaticChallenge` vlastnosti [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) základní třída měla má nastavit na jedno schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="5ff71-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="5ff71-155">Došlo k dispozici žádné dobrý způsob, jak to vynucovat.</span><span class="sxs-lookup"><span data-stu-id="5ff71-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="5ff71-156">V rámci 2.0, byly odebrány tyto dvě vlastnosti jako vlastnosti na jednotlivých `AuthenticationOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="5ff71-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="5ff71-157">Lze nastavit v `AddAuthentication` volání metody v rámci `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="5ff71-158">V předchozím fragmentu kódu, výchozí schéma je nastavený na `CookieAuthenticationDefaults.AuthenticationScheme` (dále jen "soubory cookie.).</span><span class="sxs-lookup"><span data-stu-id="5ff71-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="5ff71-159">Můžete taky použít přetížené verzi `AddAuthentication` metodu a nastavit víc než jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5ff71-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="5ff71-160">V následujícím příkladu přetížené metody, nebo výchozí schéma je nastaveno na `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="5ff71-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="5ff71-161">Schéma ověřování případně lze zadat v rámci vaší individuální `[Authorize]` atributy nebo zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="5ff71-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="5ff71-162">Definujte výchozí schéma v 2.0, pokud platí jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="5ff71-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="5ff71-163">Má-li být automaticky přihlášeni uživatel</span><span class="sxs-lookup"><span data-stu-id="5ff71-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="5ff71-164">Můžete použít `[Authorize]` atribut nebo autorizační zásady bez zadání schémata</span><span class="sxs-lookup"><span data-stu-id="5ff71-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="5ff71-165">Výjimkou z tohoto pravidla je `AddIdentity` metoda.</span><span class="sxs-lookup"><span data-stu-id="5ff71-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="5ff71-166">Tato metoda přidá soubory cookie a nastaví výchozí ověřit a challenge schémata pro soubor cookie aplikace `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="5ff71-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="5ff71-167">Kromě toho nastaví výchozí přihlašovací schéma externí soubor cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="5ff71-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="5ff71-168">Použití rozšíření HttpContext ověřování</span><span class="sxs-lookup"><span data-stu-id="5ff71-168">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="5ff71-169">`IAuthenticationManager` Rozhraní je hlavní vstupní bod do systému 1.x ověřování.</span><span class="sxs-lookup"><span data-stu-id="5ff71-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="5ff71-170">Byla nahrazena novou sadu `HttpContext` rozšiřující metody v `Microsoft.AspNetCore.Authentication` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5ff71-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="5ff71-171">Například 1.x projekty odkaz `Authentication` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="5ff71-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="5ff71-172">V projektech 2.0, importovat `Microsoft.AspNetCore.Authentication` obor názvů a odstranit `Authentication` vlastnost odkazy:</span><span class="sxs-lookup"><span data-stu-id="5ff71-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="5ff71-173">Ověřování systému Windows (soubor HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="5ff71-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="5ff71-174">Existují dvě varianty ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="5ff71-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="5ff71-175">Hostitel pouze umožňuje ověřeným uživatelům</span><span class="sxs-lookup"><span data-stu-id="5ff71-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="5ff71-176">Obě hostitele umožňuje anonymní a ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="5ff71-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="5ff71-177">První variace popsané výše je ovlivněna 2.0 změnami.</span><span class="sxs-lookup"><span data-stu-id="5ff71-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="5ff71-178">Druhá popsané výše je ovlivňován 2.0 změny.</span><span class="sxs-lookup"><span data-stu-id="5ff71-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="5ff71-179">Například je může povolovat anonymní uživatelé aplikaci za služby IIS nebo [HTTP.sys](xref:fundamentals/servers/weblistener) vrstvy ale autorizací uživatele na úrovni Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="5ff71-179">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="5ff71-180">V tomto scénáři nastavena výchozí schéma `IISDefaults.AuthenticationScheme` v `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="5ff71-181">Nepodařilo se nastavit výchozí schéma odpovídajícím způsobem brání vyzve z pracovní požadavku authorize.</span><span class="sxs-lookup"><span data-stu-id="5ff71-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="5ff71-182">IdentityCookieOptions instancí</span><span class="sxs-lookup"><span data-stu-id="5ff71-182">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="5ff71-183">Vedlejším účinkem 2.0 změny je přepínač tak, aby pomocí možnosti místo instancí možnosti souborů cookie s názvem.</span><span class="sxs-lookup"><span data-stu-id="5ff71-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="5ff71-184">Odeberou se přizpůsobit schéma názvy souborů cookie Identity.</span><span class="sxs-lookup"><span data-stu-id="5ff71-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="5ff71-185">Například 1.x projekty použití [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) předat `IdentityCookieOptions` parametr do *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ff71-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="5ff71-186">Schéma ověřování externí soubor cookie je k němu přistupovat z zadanou instanci:</span><span class="sxs-lookup"><span data-stu-id="5ff71-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="5ff71-187">Vkládání zmíněnými konstruktor stane nepotřebné 2.0 projektů a `_externalCookieScheme` pole můžete odstranit:</span><span class="sxs-lookup"><span data-stu-id="5ff71-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="5ff71-188">`IdentityConstants.ExternalScheme` Konstanta lze přímo:</span><span class="sxs-lookup"><span data-stu-id="5ff71-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="5ff71-189">Přidat IdentityUser objektů POCO navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="5ff71-189">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="5ff71-190">Navigační vlastnosti Entity Framework (EF) základní základu `IdentityUser` objektů POCO (prostý původní objekt CLR) byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="5ff71-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="5ff71-191">Pokud projekt 1.x používá tyto vlastnosti, ručně přidáte zpět 2.0 projektu:</span><span class="sxs-lookup"><span data-stu-id="5ff71-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="5ff71-192">Při spuštění migrace základní EF nechcete, aby duplicitní cizí klíče, přidejte následující příkaz pro vaše `IdentityDbContext` třída `OnModelCreating` – metoda (po `base.OnModelCreating();` volání):</span><span class="sxs-lookup"><span data-stu-id="5ff71-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="5ff71-193">Nahraďte GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="5ff71-193">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="5ff71-194">Synchronní metoda `GetExternalAuthenticationSchemes` byla odebrána považuje asynchronní verzi.</span><span class="sxs-lookup"><span data-stu-id="5ff71-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="5ff71-195">projekty 1.x mají následující kód v *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ff71-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="5ff71-196">Tato metoda se zobrazí v *Login.cshtml* příliš:</span><span class="sxs-lookup"><span data-stu-id="5ff71-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="5ff71-197">V projektech 2.0, použijte `GetExternalAuthenticationSchemesAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="5ff71-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="5ff71-198">V *Login.cshtml*, `AuthenticationScheme` vlastnost přistupovat v `foreach` smyčky změny `Name`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="5ff71-199">Změna vlastnosti ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="5ff71-199">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="5ff71-200">A `ManageLoginsViewModel` objekt se používá v `ManageLogins` akce *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ff71-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="5ff71-201">V 1.x projekty, objekt je `OtherLogins` vlastnost návratový typ je `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="5ff71-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="5ff71-202">Vyžaduje tento návratový typ importu `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="5ff71-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="5ff71-203">V projektech 2.0, návratový typ se změní na `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="5ff71-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="5ff71-204">Vyžaduje tento nový návratový typ nahrazení `Microsoft.AspNetCore.Http.Authentication` importovat s `Microsoft.AspNetCore.Authentication` importovat.</span><span class="sxs-lookup"><span data-stu-id="5ff71-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="5ff71-205">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="5ff71-205">Additional Resources</span></span>
<span data-ttu-id="5ff71-206">Diskusní a další podrobnosti najdete v tématu [diskusi k ověřování 2.0](https://github.com/aspnet/Security/issues/1338) problém na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5ff71-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
