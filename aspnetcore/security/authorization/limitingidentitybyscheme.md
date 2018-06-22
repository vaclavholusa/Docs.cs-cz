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
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="52484-103">Autorizaci s konkrétní schéma v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52484-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="52484-104">V některých případech, například jednostránkové aplikace (SPA) je běžné použít více metod ověřování.</span><span class="sxs-lookup"><span data-stu-id="52484-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="52484-105">Aplikace může například použít ověřování založené na soubor cookie k přihlášení a ověřování nosiče JWT pro JavaScript požadavky.</span><span class="sxs-lookup"><span data-stu-id="52484-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="52484-106">V některých případech aplikace může mít více instancí obslužnou rutinu ověřování.</span><span class="sxs-lookup"><span data-stu-id="52484-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="52484-107">Například dvě souboru cookie obslužné rutiny, kde jeden obsahuje základní identity a jedna se vytvoří při použití služby Multi-Factor authentication (MFA) byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="52484-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="52484-108">MFA mohou být vyvolány, protože uživatel si vyžádal, operace, která vyžaduje další bezpečnostní.</span><span class="sxs-lookup"><span data-stu-id="52484-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52484-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="52484-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="52484-110">Schéma ověřování má název, když ověřovací služby se konfiguruje během ověřování.</span><span class="sxs-lookup"><span data-stu-id="52484-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="52484-111">Příklad:</span><span class="sxs-lookup"><span data-stu-id="52484-111">For example:</span></span>

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

<span data-ttu-id="52484-112">V předchozí kód byly přidány dvě obslužné rutiny ověřování: jeden pro soubory cookie a jeden pro nosiče.</span><span class="sxs-lookup"><span data-stu-id="52484-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="52484-113">Určení výchozího schématu výsledkem `HttpContext.User` nastavenou na danou identitu.</span><span class="sxs-lookup"><span data-stu-id="52484-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="52484-114">Pokud není toto chování žádoucí, zakažte ho vyvoláním bez parametrů formu `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="52484-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52484-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52484-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="52484-116">Při ověřování middlewares byly nastaveny při ověřování, jsou pojmenované schémat ověřování.</span><span class="sxs-lookup"><span data-stu-id="52484-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="52484-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="52484-117">For example:</span></span>

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

<span data-ttu-id="52484-118">V předchozí kód byly přidány dva middlewares ověřování: jeden pro soubory cookie a jeden pro nosiče.</span><span class="sxs-lookup"><span data-stu-id="52484-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="52484-119">Určení výchozího schématu výsledkem `HttpContext.User` nastavenou na danou identitu.</span><span class="sxs-lookup"><span data-stu-id="52484-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="52484-120">Pokud není toto chování žádoucí, ji zakázat nastavením `AuthenticationOptions.AutomaticAuthenticate` vlastnost `false`.</span><span class="sxs-lookup"><span data-stu-id="52484-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="52484-121">Výběr schéma s atributem autorizovat</span><span class="sxs-lookup"><span data-stu-id="52484-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="52484-122">V místě autorizaci aplikace udává obslužné rutiny, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="52484-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="52484-123">Vyberte obslužná rutina, pomocí kterého aplikace předáním čárkami oddělený seznam schémat ověřování k ověření `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="52484-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="52484-124">`[Authorize]` Atribut určuje schéma ověřování nebo schémata používat bez ohledu na to, zda je nakonfigurován výchozí.</span><span class="sxs-lookup"><span data-stu-id="52484-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="52484-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="52484-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52484-126">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="52484-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52484-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52484-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="52484-128">V předchozím příkladu soubor cookie i nosiče obslužné rutiny spustit a možnost vytvořit a připojit identity pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="52484-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="52484-129">Zadáním pouze jeden schéma odpovídající obslužná rutina se spustí.</span><span class="sxs-lookup"><span data-stu-id="52484-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52484-130">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="52484-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52484-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52484-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="52484-132">V předchozí kód spustí jenom obslužná rutina s schéma "Nosiče".</span><span class="sxs-lookup"><span data-stu-id="52484-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="52484-133">Všechny identity na základě souborů cookie se ignorují.</span><span class="sxs-lookup"><span data-stu-id="52484-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="52484-134">Výběr schéma pomocí zásad</span><span class="sxs-lookup"><span data-stu-id="52484-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="52484-135">Pokud chcete zadat požadované schémata v [zásad](xref:security/authorization/policies), můžete nastavit `AuthenticationSchemes` kolekce při přidávání vaše zásady:</span><span class="sxs-lookup"><span data-stu-id="52484-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="52484-136">V předchozím příkladu "Over18" zásady lze spustit pouze s identitou vytvořené obslužná rutina "Nosiče".</span><span class="sxs-lookup"><span data-stu-id="52484-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="52484-137">Použijte zásady podle nastavení `[Authorize]` atributu `Policy` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="52484-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
