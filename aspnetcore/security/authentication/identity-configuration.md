---
title: Nakonfigurujte identitu ASP.NET Core
author: AdrienTorris
description: "Výchozí hodnoty ASP.NET Core Identity a nakonfigurovat různé vlastnosti Identity, které chcete použít vlastní hodnoty."
keywords: "ASP.NET Core, Identity, ověřování, zabezpečení"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="10a3f-104">Konfigurace Identity</span><span class="sxs-lookup"><span data-stu-id="10a3f-104">Configure Identity</span></span>

<span data-ttu-id="10a3f-105">ASP.NET Core Identity má společné chování v aplikacích jako zásady hesel, uzamčení čas a nastavení souborů cookie, které ve vaší aplikaci můžete snadno přepsat `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="10a3f-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="10a3f-106">Zásady hesel</span><span class="sxs-lookup"><span data-stu-id="10a3f-106">Passwords policy</span></span>

<span data-ttu-id="10a3f-107">Ve výchozím nastavení Identity vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak.</span><span class="sxs-lookup"><span data-stu-id="10a3f-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="10a3f-108">Existují také některé další omezení.</span><span class="sxs-lookup"><span data-stu-id="10a3f-108">There are also some other restrictions.</span></span> <span data-ttu-id="10a3f-109">Pro zjednodušení omezení hesla, upravte `ConfigureServices` metodu `Startup` třída vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="10a3f-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10a3f-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="10a3f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10a3f-111">Jádro ASP.NET 2.0, které jsou přidány `RequiredUniqueChars` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="10a3f-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="10a3f-112">Jinak hodnota možnosti jsou stejné z ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="10a3f-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10a3f-113">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="10a3f-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="10a3f-114">`IdentityOptions.Password`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="10a3f-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="10a3f-115">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="10a3f-115">Property</span></span>                | <span data-ttu-id="10a3f-116">Popis</span><span class="sxs-lookup"><span data-stu-id="10a3f-116">Description</span></span>                       | <span data-ttu-id="10a3f-117">Výchozí</span><span class="sxs-lookup"><span data-stu-id="10a3f-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="10a3f-118">Vyžaduje číslo mezi 0-9 v hesle.</span><span class="sxs-lookup"><span data-stu-id="10a3f-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="10a3f-119">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="10a3f-120">Minimální délka hesla.</span><span class="sxs-lookup"><span data-stu-id="10a3f-120">The minimum length of the password.</span></span> | <span data-ttu-id="10a3f-121">6</span><span class="sxs-lookup"><span data-stu-id="10a3f-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="10a3f-122">Vyžaduje jiný než alfanumerický znak v hesle.</span><span class="sxs-lookup"><span data-stu-id="10a3f-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="10a3f-123">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="10a3f-124">Vyžaduje velké písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="10a3f-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="10a3f-125">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="10a3f-126">Vyžaduje malé písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="10a3f-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="10a3f-127">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="10a3f-128">Vyžaduje počet jedinečných znaků v hesle.</span><span class="sxs-lookup"><span data-stu-id="10a3f-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="10a3f-129">1</span><span class="sxs-lookup"><span data-stu-id="10a3f-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="10a3f-130">Uzamčení uživatele</span><span class="sxs-lookup"><span data-stu-id="10a3f-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="10a3f-131">`IdentityOptions.Lockout`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="10a3f-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="10a3f-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="10a3f-132">Property</span></span>                | <span data-ttu-id="10a3f-133">Popis</span><span class="sxs-lookup"><span data-stu-id="10a3f-133">Description</span></span>                       | <span data-ttu-id="10a3f-134">Výchozí</span><span class="sxs-lookup"><span data-stu-id="10a3f-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="10a3f-135">Množství času, uživatel je uzamčen když dojde k uzamčení.</span><span class="sxs-lookup"><span data-stu-id="10a3f-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="10a3f-136">5 minut</span><span class="sxs-lookup"><span data-stu-id="10a3f-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="10a3f-137">Počet neúspěšných pokusů o přístup uživatele uzamčení, pokud je povoleno uzamčení.</span><span class="sxs-lookup"><span data-stu-id="10a3f-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="10a3f-138">5</span><span class="sxs-lookup"><span data-stu-id="10a3f-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="10a3f-139">Určuje, pokud nového uživatele lze uzamknout.</span><span class="sxs-lookup"><span data-stu-id="10a3f-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="10a3f-140">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="10a3f-141">Přihlaste se nastavení</span><span class="sxs-lookup"><span data-stu-id="10a3f-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="10a3f-142">`IdentityOptions.SignIn`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="10a3f-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="10a3f-143">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="10a3f-143">Property</span></span>                | <span data-ttu-id="10a3f-144">Popis</span><span class="sxs-lookup"><span data-stu-id="10a3f-144">Description</span></span>                       | <span data-ttu-id="10a3f-145">Výchozí</span><span class="sxs-lookup"><span data-stu-id="10a3f-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="10a3f-146">Vyžaduje potvrzen e-mail pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="10a3f-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="10a3f-147">false</span><span class="sxs-lookup"><span data-stu-id="10a3f-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="10a3f-148">Vyžaduje bylo potvrzeno telefonní číslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="10a3f-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="10a3f-149">false</span><span class="sxs-lookup"><span data-stu-id="10a3f-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="10a3f-150">Nastavení ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="10a3f-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="10a3f-151">`IdentityOptions.User`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="10a3f-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="10a3f-152">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="10a3f-152">Property</span></span>                | <span data-ttu-id="10a3f-153">Popis</span><span class="sxs-lookup"><span data-stu-id="10a3f-153">Description</span></span>                       | <span data-ttu-id="10a3f-154">Výchozí</span><span class="sxs-lookup"><span data-stu-id="10a3f-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="10a3f-155">Vyžaduje, aby měl jedinečnou e-mailovou každý uživatel.</span><span class="sxs-lookup"><span data-stu-id="10a3f-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="10a3f-156">false</span><span class="sxs-lookup"><span data-stu-id="10a3f-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="10a3f-157">Povolený počet znaků v uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="10a3f-157">Allowed characters in the username.</span></span> | <span data-ttu-id="10a3f-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="10a3f-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="10a3f-159">Nastavení souborů cookie aplikace</span><span class="sxs-lookup"><span data-stu-id="10a3f-159">Application's cookie settings</span></span>

<span data-ttu-id="10a3f-160">Jako zásady hesla, lze změnit všechna nastavení souboru cookie pro aplikace v `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="10a3f-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10a3f-161">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="10a3f-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10a3f-162">V části `ConfigureServices` v `Startup` třídu, můžete nakonfigurovat soubor cookie aplikace.</span><span class="sxs-lookup"><span data-stu-id="10a3f-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10a3f-163">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="10a3f-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="10a3f-164">`CookieAuthenticationOptions`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="10a3f-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="10a3f-165">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="10a3f-165">Property</span></span>                | <span data-ttu-id="10a3f-166">Popis</span><span class="sxs-lookup"><span data-stu-id="10a3f-166">Description</span></span>                       | <span data-ttu-id="10a3f-167">Výchozí</span><span class="sxs-lookup"><span data-stu-id="10a3f-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="10a3f-168">Název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="10a3f-168">The name of the cookie.</span></span>  | <span data-ttu-id="10a3f-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="10a3f-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="10a3f-170">V případě hodnoty true, není soubor cookie přístupný z skripty na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="10a3f-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="10a3f-171">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="10a3f-172">Určuje, jak dlouho lístek ověřování uložené v souboru cookie, který zůstane platný od okamžiku, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="10a3f-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="10a3f-173">14 dnů</span><span class="sxs-lookup"><span data-stu-id="10a3f-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="10a3f-174">Pokud uživatel není autorizovaný, bude přesměrován na tuto cestu k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="10a3f-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="10a3f-175">/ Účet nebo přihlášení</span><span class="sxs-lookup"><span data-stu-id="10a3f-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="10a3f-176">Když je přihlášený uživatel, bude přesměrován na této cestě.</span><span class="sxs-lookup"><span data-stu-id="10a3f-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="10a3f-177">/ Účet/odhlášení</span><span class="sxs-lookup"><span data-stu-id="10a3f-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="10a3f-178">Pokud se uživatel nezdaří kontrolu autorizace, bude přesměrován na této cestě.</span><span class="sxs-lookup"><span data-stu-id="10a3f-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="10a3f-179">V případě hodnoty true bude nový čas vypršení platnosti, pokud je aktuální soubor cookie vyčerpal více než polovinu životnosti vydán nový soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="10a3f-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="10a3f-180">/ Účet nebo AccessDenied</span><span class="sxs-lookup"><span data-stu-id="10a3f-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="10a3f-181">Určuje název parametru řetězce dotazu, který je middlewarem připojen, pokud až po přesměrování 302 na přihlašovací cestě změněn kód stavu 401 Unauthorized status.</span><span class="sxs-lookup"><span data-stu-id="10a3f-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="10a3f-182">true</span><span class="sxs-lookup"><span data-stu-id="10a3f-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="10a3f-183">To platí pouze pro ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="10a3f-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="10a3f-184">Logický název pro konkrétní ověřování schématu.</span><span class="sxs-lookup"><span data-stu-id="10a3f-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="10a3f-185">Tento příznak platí pouze pro ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="10a3f-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="10a3f-186">V případě hodnoty true ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.</span><span class="sxs-lookup"><span data-stu-id="10a3f-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |