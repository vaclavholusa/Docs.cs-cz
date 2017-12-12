---
title: "Autorizaci s konkrétní schéma - ASP.NET Core"
author: rick-anderson
description: "Tento článek vysvětluje, jak omezit identity pro konkrétní schéma při práci s více metod ověřování."
keywords: "ASP.NET Core, identity, schéma ověřování"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 8c9d068b88263d0c06b11a6b87416fb02885c475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="5feaf-104">Autorizaci s konkrétní schéma</span><span class="sxs-lookup"><span data-stu-id="5feaf-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="5feaf-105">V některých případech, například jednostránkové aplikace (SPA) je běžné použít více metod ověřování.</span><span class="sxs-lookup"><span data-stu-id="5feaf-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="5feaf-106">Aplikace může například použít ověřování založené na soubor cookie k přihlášení a ověřování nosiče JWT pro JavaScript požadavky.</span><span class="sxs-lookup"><span data-stu-id="5feaf-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="5feaf-107">V některých případech aplikace může mít více instancí obslužnou rutinu ověřování.</span><span class="sxs-lookup"><span data-stu-id="5feaf-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="5feaf-108">Například dvě souboru cookie obslužné rutiny, kde jeden obsahuje základní identity a jedna se vytvoří při použití služby Multi-Factor authentication (MFA) byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="5feaf-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="5feaf-109">MFA mohou být vyvolány, protože uživatel si vyžádal, operace, která vyžaduje další bezpečnostní.</span><span class="sxs-lookup"><span data-stu-id="5feaf-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5feaf-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="5feaf-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5feaf-111">Schéma ověřování má název, když ověřovací služby se konfiguruje během ověřování.</span><span class="sxs-lookup"><span data-stu-id="5feaf-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="5feaf-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5feaf-112">For example:</span></span>

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

<span data-ttu-id="5feaf-113">V předchozí kód byly přidány dvě obslužné rutiny ověřování: jeden pro soubory cookie a jeden pro nosiče.</span><span class="sxs-lookup"><span data-stu-id="5feaf-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5feaf-114">Určení výchozího schématu výsledkem `HttpContext.User` nastavenou na danou identitu.</span><span class="sxs-lookup"><span data-stu-id="5feaf-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5feaf-115">Pokud není toto chování žádoucí, zakažte ho vyvoláním bez parametrů formu `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="5feaf-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5feaf-116">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="5feaf-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5feaf-117">Při ověřování middlewares byly nastaveny při ověřování, jsou pojmenované schémat ověřování.</span><span class="sxs-lookup"><span data-stu-id="5feaf-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="5feaf-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5feaf-118">For example:</span></span>

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

<span data-ttu-id="5feaf-119">V předchozí kód byly přidány dva middlewares ověřování: jeden pro soubory cookie a jeden pro nosiče.</span><span class="sxs-lookup"><span data-stu-id="5feaf-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5feaf-120">Určení výchozího schématu výsledkem `HttpContext.User` nastavenou na danou identitu.</span><span class="sxs-lookup"><span data-stu-id="5feaf-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5feaf-121">Pokud není toto chování žádoucí, ji zakázat nastavením `AuthenticationOptions.AutomaticAuthenticate` vlastnost `false`.</span><span class="sxs-lookup"><span data-stu-id="5feaf-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="5feaf-122">Výběr schéma s atributem autorizovat</span><span class="sxs-lookup"><span data-stu-id="5feaf-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="5feaf-123">V místě autorizaci aplikace udává obslužné rutiny, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="5feaf-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="5feaf-124">Vyberte obslužná rutina, pomocí kterého aplikace předáním čárkami oddělený seznam schémat ověřování k ověření `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="5feaf-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="5feaf-125">`[Authorize]` Atribut určuje schéma ověřování nebo schémata používat bez ohledu na to, zda je nakonfigurován výchozí.</span><span class="sxs-lookup"><span data-stu-id="5feaf-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="5feaf-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5feaf-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5feaf-127">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="5feaf-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5feaf-128">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="5feaf-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="5feaf-129">V předchozím příkladu soubor cookie i nosiče obslužné rutiny spustit a možnost vytvořit a připojit identity pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="5feaf-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="5feaf-130">Zadáním pouze jeden schéma odpovídající obslužná rutina se spustí.</span><span class="sxs-lookup"><span data-stu-id="5feaf-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5feaf-131">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="5feaf-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5feaf-132">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="5feaf-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="5feaf-133">V předchozí kód spustí jenom obslužná rutina s schéma "Nosiče".</span><span class="sxs-lookup"><span data-stu-id="5feaf-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="5feaf-134">Všechny identity na základě souborů cookie se ignorují.</span><span class="sxs-lookup"><span data-stu-id="5feaf-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="5feaf-135">Výběr schéma pomocí zásad</span><span class="sxs-lookup"><span data-stu-id="5feaf-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="5feaf-136">Pokud chcete zadat požadované schémata v [zásad](xref:security/authorization/policies), můžete nastavit `AuthenticationSchemes` kolekce při přidávání vaše zásady:</span><span class="sxs-lookup"><span data-stu-id="5feaf-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="5feaf-137">V předchozím příkladu "Over18" zásady lze spustit pouze s identitou vytvořené obslužná rutina "Nosiče".</span><span class="sxs-lookup"><span data-stu-id="5feaf-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="5feaf-138">Použijte zásady podle nastavení `[Authorize]` atributu `Policy` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="5feaf-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
