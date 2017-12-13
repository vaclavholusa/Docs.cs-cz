---
title: Nakonfigurujte identitu ASP.NET Core
author: AdrienTorris
description: "Výchozí hodnoty ASP.NET Core Identity a nakonfigurovat různé vlastnosti Identity, které chcete použít vlastní hodnoty."
keywords: "ASP.NET Core, Identity, ověřování, zabezpečení"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="f7b44-104">Konfigurace Identity</span><span class="sxs-lookup"><span data-stu-id="f7b44-104">Configure Identity</span></span>

<span data-ttu-id="f7b44-105">ASP.NET Core Identity má některé výchozí chování lze přepsat snadno ve vaší aplikaci `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="f7b44-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="f7b44-106">Zásady hesel</span><span class="sxs-lookup"><span data-stu-id="f7b44-106">Passwords policy</span></span>

<span data-ttu-id="f7b44-107">Ve výchozím nastavení Identity vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak.</span><span class="sxs-lookup"><span data-stu-id="f7b44-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="f7b44-108">Existují také některé další omezení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-108">There are also some other restrictions.</span></span> <span data-ttu-id="f7b44-109">Pokud chcete zjednodušit omezení heslo, můžete to udělat `Startup` třída vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7b44-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7b44-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="f7b44-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7b44-111">Jádro ASP.NET 2.0, které jsou přidány `RequiredUniqueChars` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f7b44-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="f7b44-112">Jinak hodnota možnosti jsou stejné z ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f7b44-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7b44-113">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="f7b44-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="f7b44-114">`IdentityOptions.Password`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f7b44-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="f7b44-115">`RequireDigit`: Vyžaduje číslo mezi 0-9 v hesle.</span><span class="sxs-lookup"><span data-stu-id="f7b44-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="f7b44-116">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-116">Defaults to true.</span></span>
* <span data-ttu-id="f7b44-117">`RequiredLength`: Minimální délku hesla.</span><span class="sxs-lookup"><span data-stu-id="f7b44-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="f7b44-118">Výchozí hodnota je 6.</span><span class="sxs-lookup"><span data-stu-id="f7b44-118">Defaults to 6.</span></span>
* <span data-ttu-id="f7b44-119">`RequireNonAlphanumeric`: Vyžaduje jiný než alfanumerický znak v hesle.</span><span class="sxs-lookup"><span data-stu-id="f7b44-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="f7b44-120">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-120">Defaults to true.</span></span>
* <span data-ttu-id="f7b44-121">`RequireUppercase`: Vyžaduje velké písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="f7b44-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="f7b44-122">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-122">Defaults to true.</span></span>
* <span data-ttu-id="f7b44-123">`RequireLowercase`: Vyžaduje malé písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="f7b44-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="f7b44-124">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-124">Defaults to true.</span></span>
* <span data-ttu-id="f7b44-125">`RequiredUniqueChars`: Vyžaduje počet jedinečných znaků v hesle.</span><span class="sxs-lookup"><span data-stu-id="f7b44-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="f7b44-126">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="f7b44-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="f7b44-127">Uzamčení uživatele</span><span class="sxs-lookup"><span data-stu-id="f7b44-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="f7b44-128">`IdentityOptions.Lockout`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f7b44-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="f7b44-129">`DefaultLockoutTimeSpan`: Množství času, uživatel je uzamčen když dojde k uzamčení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="f7b44-130">Výchozí hodnota je 5 minut.</span><span class="sxs-lookup"><span data-stu-id="f7b44-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="f7b44-131">`MaxFailedAccessAttempts`: Počet neúspěšných pokusů o přístup uživatele uzamčení, pokud je povoleno uzamčení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="f7b44-132">Výchozí hodnota je 5.</span><span class="sxs-lookup"><span data-stu-id="f7b44-132">Defaults to 5.</span></span>
* <span data-ttu-id="f7b44-133">`AllowedForNewUsers`: Určuje, pokud nového uživatele lze uzamknout. Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="f7b44-134">Přihlaste se nastavení</span><span class="sxs-lookup"><span data-stu-id="f7b44-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="f7b44-135">`IdentityOptions.SignIn`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f7b44-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="f7b44-136">`RequireConfirmedEmail`: Vyžaduje potvrzen e-mail pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="f7b44-137">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="f7b44-137">Defaults to false.</span></span>
* <span data-ttu-id="f7b44-138">`RequireConfirmedPhoneNumber`: Vyžaduje bylo potvrzeno telefonní číslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="f7b44-139">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="f7b44-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="f7b44-140">Nastavení ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="f7b44-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="f7b44-141">`IdentityOptions.User`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f7b44-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="f7b44-142">`RequireUniqueEmail`: Vyžaduje, aby měl jedinečnou e-mailovou každý uživatel.</span><span class="sxs-lookup"><span data-stu-id="f7b44-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="f7b44-143">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="f7b44-143">Defaults to false.</span></span>
* <span data-ttu-id="f7b44-144">`AllowedUserNameCharacters`: V uživatelské jméno povolený počet znaků.</span><span class="sxs-lookup"><span data-stu-id="f7b44-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="f7b44-145">Výchozí hodnota je abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="f7b44-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="f7b44-146">Nastavení souborů cookie aplikace</span><span class="sxs-lookup"><span data-stu-id="f7b44-146">Application's cookie settings</span></span>

<span data-ttu-id="f7b44-147">Jako zásady hesla, lze změnit všechna nastavení souboru cookie pro aplikace v `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="f7b44-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7b44-148">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="f7b44-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7b44-149">V části `ConfigureServices` v `Startup` třídu, můžete nakonfigurovat soubor cookie aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7b44-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7b44-150">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="f7b44-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="f7b44-151">`CookieAuthenticationOptions`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f7b44-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="f7b44-152">`Cookie.Name`: Název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="f7b44-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="f7b44-153">Výchozí hodnota. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="f7b44-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="f7b44-154">`Cookie.HttpOnly`: Pokud je nastavena hodnota true, není soubor cookie přístupný z skripty na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f7b44-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="f7b44-155">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-155">Defaults to true.</span></span>
* <span data-ttu-id="f7b44-156">`ExpireTimeSpan`: Určuje, jak dlouho lístek ověřování uložené v souboru cookie, který zůstane platný od okamžiku, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f7b44-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="f7b44-157">Výchozí hodnota je 14 dnů.</span><span class="sxs-lookup"><span data-stu-id="f7b44-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="f7b44-158">`LoginPath`: Pokud neoprávněný uživatel, bude přesměrován na tuto cestu k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="f7b44-159">Výchozí hodnota je/Account/přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="f7b44-160">`LogoutPath`: Pokud je přihlášený uživatel, bude přesměrován na této cestě.</span><span class="sxs-lookup"><span data-stu-id="f7b44-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="f7b44-161">Výchozí hodnota je/Account/odhlášení.</span><span class="sxs-lookup"><span data-stu-id="f7b44-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="f7b44-162">`AccessDeniedPath`: V případě, že se uživateli nepodaří kontrolu autorizace, bude přesměrován na této cestě.</span><span class="sxs-lookup"><span data-stu-id="f7b44-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="f7b44-163">Výchozí hodnota je/Account/AccessDenied.</span><span class="sxs-lookup"><span data-stu-id="f7b44-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="f7b44-164">`SlidingExpiration`: Pokud je nastavena hodnota true, bude vydána nový soubor cookie s novou čas vypršení platnosti, pokud je aktuální soubor cookie vyčerpal více než polovinu životnosti.</span><span class="sxs-lookup"><span data-stu-id="f7b44-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="f7b44-165">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="f7b44-165">Defaults to true.</span></span>
* <span data-ttu-id="f7b44-166">`ReturnUrlParameter`: ReturnUrlParameter Určuje název parametru řetězce dotazu, který je middlewarem připojen, pokud až po přesměrování 302 na přihlašovací cestě změněn kód stavu 401 Unauthorized status.</span><span class="sxs-lookup"><span data-stu-id="f7b44-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="f7b44-167">`AuthenticationScheme`: Toto je pouze relevantní pro ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f7b44-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="f7b44-168">Logický název pro konkrétní ověřování schématu.</span><span class="sxs-lookup"><span data-stu-id="f7b44-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="f7b44-169">`AutomaticAuthenticate`: Tento příznak platí pouze pro ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f7b44-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="f7b44-170">V případě hodnoty true ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.</span><span class="sxs-lookup"><span data-stu-id="f7b44-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

