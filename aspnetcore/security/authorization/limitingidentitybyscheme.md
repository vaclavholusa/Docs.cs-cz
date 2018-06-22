---
title: Autorizaci s konkrétní schéma v ASP.NET Core
author: rick-anderson
description: Tento článek vysvětluje, jak omezit identity pro konkrétní schéma při práci s více metod ověřování.
ms.author: riande
ms.date: 10/12/2017
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 231c664006ee7ff91f471aa8d16c1fd18dcbabb1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278197"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autorizaci s konkrétní schéma v ASP.NET Core

V některých případech, například jednostránkové aplikace (SPA) je běžné použít více metod ověřování. Aplikace může například použít ověřování založené na soubor cookie k přihlášení a ověřování nosiče JWT pro JavaScript požadavky. V některých případech aplikace může mít více instancí obslužnou rutinu ověřování. Například dvě souboru cookie obslužné rutiny, kde jeden obsahuje základní identity a jedna se vytvoří při použití služby Multi-Factor authentication (MFA) byla spuštěna. MFA mohou být vyvolány, protože uživatel si vyžádal, operace, která vyžaduje další bezpečnostní.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Schéma ověřování má název, když ověřovací služby se konfiguruje během ověřování. Příklad:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

V předchozí kód byly přidány dvě obslužné rutiny ověřování: jeden pro soubory cookie a jeden pro nosiče.

>[!NOTE]
>Určení výchozího schématu výsledkem `HttpContext.User` nastavenou na danou identitu. Pokud není toto chování žádoucí, zakažte ho vyvoláním bez parametrů formu `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Při ověřování middlewares byly nastaveny při ověřování, jsou pojmenované schémat ověřování. Příklad:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

V předchozí kód byly přidány dva middlewares ověřování: jeden pro soubory cookie a jeden pro nosiče.

>[!NOTE]
>Určení výchozího schématu výsledkem `HttpContext.User` nastavenou na danou identitu. Pokud není toto chování žádoucí, ji zakázat nastavením `AuthenticationOptions.AutomaticAuthenticate` vlastnost `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Výběr schéma s atributem autorizovat

V místě autorizaci aplikace udává obslužné rutiny, který se má použít. Vyberte obslužná rutina, pomocí kterého aplikace předáním čárkami oddělený seznam schémat ověřování k ověření `[Authorize]`. `[Authorize]` Atribut určuje schéma ověřování nebo schémata používat bez ohledu na to, zda je nakonfigurován výchozí. Příklad:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

V předchozím příkladu soubor cookie i nosiče obslužné rutiny spustit a možnost vytvořit a připojit identity pro aktuálního uživatele. Zadáním pouze jeden schéma odpovídající obslužná rutina se spustí.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

V předchozí kód spustí jenom obslužná rutina s schéma "Nosiče". Všechny identity na základě souborů cookie se ignorují.

## <a name="selecting-the-scheme-with-policies"></a>Výběr schéma pomocí zásad

Pokud chcete zadat požadované schémata v [zásad](xref:security/authorization/policies), můžete nastavit `AuthenticationSchemes` kolekce při přidávání vaše zásady:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

V předchozím příkladu "Over18" zásady lze spustit pouze s identitou vytvořené obslužná rutina "Nosiče". Použijte zásady podle nastavení `[Authorize]` atributu `Policy` vlastnost:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
