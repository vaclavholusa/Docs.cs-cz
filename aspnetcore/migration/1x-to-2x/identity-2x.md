---
title: Migrujte na technologii ASP.NET 2.0 základní ověřování a identita
author: scottaddie
description: Tento článek popisuje nejběžnější kroky pro migraci ASP.NET Core 1.x ověřování a identita na technologii ASP.NET 2.0 jádra.
ms.author: scaddie
ms.date: 10/26/2017
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 0485b1bdf8be550de35a49803a24666c026b3d9b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276416"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrujte na technologii ASP.NET 2.0 základní ověřování a identita

Podle [Scott Addie](https://github.com/scottaddie) a [ani Haovi společnosti](https://github.com/HaoK)

Jádro ASP.NET 2.0 obsahuje nový model pro ověřování a [Identity](xref:security/authentication/identity) který zjednodušuje konfiguraci pomocí služby. ASP.NET Core 1.x aplikací, které používají ověřování nebo Identity můžete aktualizovat, a použít nový model jak je uvedeno níže.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Middleware ověřování a služby
V projektech 1.x je nakonfigurované ověřování prostřednictvím middlewaru. Middleware metoda je volána pro každé schéma ověřování, které chcete podporovat.

Následující příklad 1.x nakonfiguruje ověřování Facebook s identitou v *Startup.cs*:

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

V 2.0 projektů je nakonfigurované ověřování prostřednictvím služeb. Každé schéma ověřování je zaregistrován v `ConfigureServices` metodu *Startup.cs*. `UseIdentity` Se nahradí metoda `UseAuthentication`.

Následující příklad 2.0 nakonfiguruje ověřování Facebook s identitou v *Startup.cs*:

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

`UseAuthentication` Metoda přidá komponenta middlewaru jednotné ověřování, která je zodpovědná za automatické ověření a zpracování žádostí o vzdálené ověření. Nahradí všechny komponenty middlewaru jednotlivých se součástí jedné, běžné middleware.

Níže jsou uvedeny pokyny k migraci 2.0 pro každé schéma hlavní ověřování.

### <a name="cookie-based-authentication"></a>Ověřování na základě souborů cookie
Vyberte jednu z následujících dvou možností a proveďte potřebné změny v *Startup.cs*:

1. Použít soubory cookie s identitou
    - Nahraďte `UseIdentity` s `UseAuthentication` v `Configure` metoda:

        ```csharp
        app.UseAuthentication();
        ```

    - Vyvolání `AddIdentity` metoda v `ConfigureServices` metoda pro přidání služby ověřování souborů cookie.
    - Volitelně můžete vyvolat `ConfigureApplicationCookie` nebo `ConfigureExternalCookie` metoda v `ConfigureServices` metodu za účelem vylepšení nastavení souborů cookie Identity.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Použít soubory cookie bez Identity
    - Nahraďte `UseCookieAuthentication` volání metody `Configure` metoda s `UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - Vyvolání `AddAuthentication` a `AddCookie` metody v `ConfigureServices` metoda:

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

### <a name="jwt-bearer-authentication"></a>Ověřování nosiče JWT
Proveďte následující změny v *Startup.cs*:
- Nahraďte `UseJwtBearerAuthentication` volání metody `Configure` metoda s `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Vyvolání `AddJwtBearer` metoda v `ConfigureServices` metoda:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Tento fragment kódu nepoužívá identitu, abyste výchozí schéma musí nastavit předáním `JwtBearerDefaults.AuthenticationScheme` k `AddAuthentication` metoda.

### <a name="openid-connect-oidc-authentication"></a>Ověřování OpenID Connect (OIDC)
Proveďte následující změny v *Startup.cs*:

- Nahraďte `UseOpenIdConnectAuthentication` volání metody `Configure` metoda s `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Vyvolání `AddOpenIdConnect` metoda v `ConfigureServices` metoda:

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

### <a name="facebook-authentication"></a>ověřování Facebook
Proveďte následující změny v *Startup.cs*:
- Nahraďte `UseFacebookAuthentication` volání metody `Configure` metoda s `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Vyvolání `AddFacebook` metoda v `ConfigureServices` metoda:
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Ověřování Google
Proveďte následující změny v *Startup.cs*:
- Nahraďte `UseGoogleAuthentication` volání metody `Configure` metoda s `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Vyvolání `AddGoogle` metoda v `ConfigureServices` metoda:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Ověřování Microsoft Account
Proveďte následující změny v *Startup.cs*:
- Nahraďte `UseMicrosoftAccountAuthentication` volání metody `Configure` metoda s `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Vyvolání `AddMicrosoftAccount` metoda v `ConfigureServices` metoda:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Ověřování služby Twitter.
Proveďte následující změny v *Startup.cs*:
- Nahraďte `UseTwitterAuthentication` volání metody `Configure` metoda s `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Vyvolání `AddTwitter` metoda v `ConfigureServices` metoda:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Schémat ověřování výchozí nastavení
V 1.x `AutomaticAuthenticate` a `AutomaticChallenge` vlastnosti [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) základní třída měla má nastavit na jedno schéma ověřování. Došlo k dispozici žádné dobrý způsob, jak to vynucovat.

V rámci 2.0, byly odebrány tyto dvě vlastnosti jako vlastnosti na jednotlivých `AuthenticationOptions` instance. Lze nastavit v `AddAuthentication` volání metody v rámci `ConfigureServices` metodu *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

V předchozím fragmentu kódu, výchozí schéma je nastavený na `CookieAuthenticationDefaults.AuthenticationScheme` (dále jen "soubory cookie.).

Můžete taky použít přetížené verzi `AddAuthentication` metodu a nastavit víc než jednu vlastnost. V následujícím příkladu přetížené metody, nebo výchozí schéma je nastaveno na `CookieAuthenticationDefaults.AuthenticationScheme`. Schéma ověřování případně lze zadat v rámci vaší individuální `[Authorize]` atributy nebo zásad autorizace.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Definujte výchozí schéma v 2.0, pokud platí jedna z následujících podmínek:
- Má-li být automaticky přihlášeni uživatel
- Můžete použít `[Authorize]` atribut nebo autorizační zásady bez zadání schémata

Výjimkou z tohoto pravidla je `AddIdentity` metoda. Tato metoda přidá soubory cookie a nastaví výchozí ověřit a challenge schémata pro soubor cookie aplikace `IdentityConstants.ApplicationScheme`. Kromě toho nastaví výchozí přihlašovací schéma externí soubor cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Použití rozšíření HttpContext ověřování
`IAuthenticationManager` Rozhraní je hlavní vstupní bod do systému 1.x ověřování. Byla nahrazena novou sadu `HttpContext` rozšiřující metody v `Microsoft.AspNetCore.Authentication` oboru názvů.

Například 1.x projekty odkaz `Authentication` vlastnost:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

V projektech 2.0, importovat `Microsoft.AspNetCore.Authentication` obor názvů a odstranit `Authentication` vlastnost odkazy:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Ověřování systému Windows (soubor HTTP.sys / IISIntegration)
Existují dvě varianty ověřování systému Windows:
1. Hostitel pouze umožňuje ověřeným uživatelům
2. Obě hostitele umožňuje anonymní a ověření uživatelé

První variace popsané výše je ovlivněna 2.0 změnami.

Druhá popsané výše je ovlivňován 2.0 změny. Například je může povolovat anonymní uživatelé aplikaci za služby IIS nebo [HTTP.sys](xref:fundamentals/servers/weblistener) vrstvy ale autorizací uživatele na úrovni Kontroleru. V tomto scénáři nastavena výchozí schéma `IISDefaults.AuthenticationScheme` v `ConfigureServices` metodu *Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Nepodařilo se nastavit výchozí schéma odpovídajícím způsobem brání vyzve z pracovní požadavku authorize.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions instancí
Vedlejším účinkem 2.0 změny je přepínač tak, aby pomocí možnosti místo instancí možnosti souborů cookie s názvem. Odeberou se přizpůsobit schéma názvy souborů cookie Identity.

Například 1.x projekty použití [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) předat `IdentityCookieOptions` parametr do *AccountController.cs*. Schéma ověřování externí soubor cookie je k němu přistupovat z zadanou instanci:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Vkládání zmíněnými konstruktor stane nepotřebné 2.0 projektů a `_externalCookieScheme` pole můžete odstranit:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` Konstanta lze přímo:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Přidat IdentityUser objektů POCO navigační vlastnosti
Navigační vlastnosti Entity Framework (EF) základní základu `IdentityUser` objektů POCO (prostý původní objekt CLR) byly odebrány. Pokud projekt 1.x používá tyto vlastnosti, ručně přidáte zpět 2.0 projektu:

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

Při spuštění migrace základní EF nechcete, aby duplicitní cizí klíče, přidejte následující příkaz pro vaše `IdentityDbContext` třída `OnModelCreating` – metoda (po `base.OnModelCreating();` volání):

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

## <a name="replace-getexternalauthenticationschemes"></a>Nahraďte GetExternalAuthenticationSchemes
Synchronní metoda `GetExternalAuthenticationSchemes` byla odebrána považuje asynchronní verzi. projekty 1.x mají následující kód v *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Tato metoda se zobrazí v *Login.cshtml* příliš:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

V projektech 2.0, použijte `GetExternalAuthenticationSchemesAsync` metoda:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

V *Login.cshtml*, `AuthenticationScheme` vlastnost přistupovat v `foreach` smyčky změny `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Změna vlastnosti ManageLoginsViewModel
A `ManageLoginsViewModel` objekt se používá v `ManageLogins` akce *ManageController.cs*. V 1.x projekty, objekt je `OtherLogins` vlastnost návratový typ je `IList<AuthenticationDescription>`. Vyžaduje tento návratový typ importu `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

V projektech 2.0, návratový typ se změní na `IList<AuthenticationScheme>`. Vyžaduje tento nový návratový typ nahrazení `Microsoft.AspNetCore.Http.Authentication` importovat s `Microsoft.AspNetCore.Authentication` importovat.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Další zdroje
Diskusní a další podrobnosti najdete v tématu [diskusi k ověřování 2.0](https://github.com/aspnet/Security/issues/1338) problém na Githubu.
