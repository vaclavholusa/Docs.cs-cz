---
title: Nakonfigurujte identitu ASP.NET Core
author: AdrienTorris
description: "Výchozí hodnoty ASP.NET Core Identity a nakonfigurovat různé vlastnosti Identity, které chcete použít vlastní hodnoty."
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: d3a13d1cef3417522460b44c52c1361c3e9d1162
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="configure-identity"></a><span data-ttu-id="9584a-103">Konfigurace Identity</span><span class="sxs-lookup"><span data-stu-id="9584a-103">Configure Identity</span></span>

<span data-ttu-id="9584a-104">ASP.NET Core Identity má společné chování v aplikacích jako zásady hesel, uzamčení čas a nastavení souborů cookie, které ve vaší aplikaci můžete snadno přepsat `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="9584a-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="9584a-105">Zásady hesel</span><span class="sxs-lookup"><span data-stu-id="9584a-105">Passwords policy</span></span>

<span data-ttu-id="9584a-106">Ve výchozím nastavení Identity vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak.</span><span class="sxs-lookup"><span data-stu-id="9584a-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="9584a-107">Existují také některé další omezení.</span><span class="sxs-lookup"><span data-stu-id="9584a-107">There are also some other restrictions.</span></span> <span data-ttu-id="9584a-108">Pro zjednodušení omezení hesla, upravte `ConfigureServices` metodu `Startup` třída vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9584a-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9584a-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="9584a-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9584a-110">Jádro ASP.NET 2.0, které jsou přidány `RequiredUniqueChars` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9584a-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="9584a-111">Jinak hodnota možnosti jsou stejné z ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9584a-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9584a-112">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="9584a-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="9584a-113">`IdentityOptions.Password`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9584a-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="9584a-114">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9584a-114">Property</span></span>                | <span data-ttu-id="9584a-115">Popis</span><span class="sxs-lookup"><span data-stu-id="9584a-115">Description</span></span>                       | <span data-ttu-id="9584a-116">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9584a-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="9584a-117">Vyžaduje číslo mezi 0-9 v hesle.</span><span class="sxs-lookup"><span data-stu-id="9584a-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="9584a-118">true</span><span class="sxs-lookup"><span data-stu-id="9584a-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="9584a-119">Minimální délka hesla.</span><span class="sxs-lookup"><span data-stu-id="9584a-119">The minimum length of the password.</span></span> | <span data-ttu-id="9584a-120">6</span><span class="sxs-lookup"><span data-stu-id="9584a-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="9584a-121">Vyžaduje jiný než alfanumerický znak v hesle.</span><span class="sxs-lookup"><span data-stu-id="9584a-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="9584a-122">true</span><span class="sxs-lookup"><span data-stu-id="9584a-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="9584a-123">Vyžaduje velké písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="9584a-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="9584a-124">true</span><span class="sxs-lookup"><span data-stu-id="9584a-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="9584a-125">Vyžaduje malé písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="9584a-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="9584a-126">true</span><span class="sxs-lookup"><span data-stu-id="9584a-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="9584a-127">Vyžaduje počet jedinečných znaků v hesle.</span><span class="sxs-lookup"><span data-stu-id="9584a-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="9584a-128">1</span><span class="sxs-lookup"><span data-stu-id="9584a-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="9584a-129">Uzamčení uživatele</span><span class="sxs-lookup"><span data-stu-id="9584a-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="9584a-130">`IdentityOptions.Lockout`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9584a-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="9584a-131">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9584a-131">Property</span></span>                | <span data-ttu-id="9584a-132">Popis</span><span class="sxs-lookup"><span data-stu-id="9584a-132">Description</span></span>                       | <span data-ttu-id="9584a-133">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9584a-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="9584a-134">Množství času, uživatel je uzamčen když dojde k uzamčení.</span><span class="sxs-lookup"><span data-stu-id="9584a-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="9584a-135">5 minut</span><span class="sxs-lookup"><span data-stu-id="9584a-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="9584a-136">Počet neúspěšných pokusů o přístup uživatele uzamčení, pokud je povoleno uzamčení.</span><span class="sxs-lookup"><span data-stu-id="9584a-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="9584a-137">5</span><span class="sxs-lookup"><span data-stu-id="9584a-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="9584a-138">Určuje, pokud nového uživatele lze uzamknout.</span><span class="sxs-lookup"><span data-stu-id="9584a-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="9584a-139">true</span><span class="sxs-lookup"><span data-stu-id="9584a-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="9584a-140">Přihlaste se nastavení</span><span class="sxs-lookup"><span data-stu-id="9584a-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="9584a-141">`IdentityOptions.SignIn`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9584a-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="9584a-142">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9584a-142">Property</span></span>                | <span data-ttu-id="9584a-143">Popis</span><span class="sxs-lookup"><span data-stu-id="9584a-143">Description</span></span>                       | <span data-ttu-id="9584a-144">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9584a-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="9584a-145">Vyžaduje potvrzen e-mail pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9584a-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="9584a-146">false</span><span class="sxs-lookup"><span data-stu-id="9584a-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="9584a-147">Vyžaduje bylo potvrzeno telefonní číslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9584a-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="9584a-148">false</span><span class="sxs-lookup"><span data-stu-id="9584a-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="9584a-149">Nastavení ověření uživatele</span><span class="sxs-lookup"><span data-stu-id="9584a-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="9584a-150">`IdentityOptions.User`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9584a-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="9584a-151">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9584a-151">Property</span></span>                | <span data-ttu-id="9584a-152">Popis</span><span class="sxs-lookup"><span data-stu-id="9584a-152">Description</span></span>                       | <span data-ttu-id="9584a-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9584a-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="9584a-154">Vyžaduje, aby měl jedinečnou e-mailovou každý uživatel.</span><span class="sxs-lookup"><span data-stu-id="9584a-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="9584a-155">false</span><span class="sxs-lookup"><span data-stu-id="9584a-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="9584a-156">Povolený počet znaků v uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9584a-156">Allowed characters in the username.</span></span> | <span data-ttu-id="9584a-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="9584a-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="9584a-158">Nastavení souborů cookie aplikace</span><span class="sxs-lookup"><span data-stu-id="9584a-158">Application's cookie settings</span></span>

<span data-ttu-id="9584a-159">Jako zásady hesla, lze změnit všechna nastavení souboru cookie pro aplikace v `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="9584a-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9584a-160">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="9584a-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9584a-161">V části `ConfigureServices` v `Startup` třídu, můžete nakonfigurovat soubor cookie aplikace.</span><span class="sxs-lookup"><span data-stu-id="9584a-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9584a-162">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="9584a-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="9584a-163">`CookieAuthenticationOptions`má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9584a-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="9584a-164">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9584a-164">Property</span></span>                | <span data-ttu-id="9584a-165">Popis</span><span class="sxs-lookup"><span data-stu-id="9584a-165">Description</span></span>                       | <span data-ttu-id="9584a-166">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9584a-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="9584a-167">Název souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="9584a-167">The name of the cookie.</span></span>  | <span data-ttu-id="9584a-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="9584a-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="9584a-169">V případě hodnoty true, není soubor cookie přístupný z skripty na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9584a-169">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="9584a-170">true</span><span class="sxs-lookup"><span data-stu-id="9584a-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="9584a-171">Určuje, jak dlouho lístek ověřování uložené v souboru cookie, který zůstane platný od okamžiku, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="9584a-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="9584a-172">14 dnů</span><span class="sxs-lookup"><span data-stu-id="9584a-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="9584a-173">Pokud uživatel není autorizovaný, bude přesměrován na tuto cestu k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9584a-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="9584a-174">/ Účet nebo přihlášení</span><span class="sxs-lookup"><span data-stu-id="9584a-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="9584a-175">Když je přihlášený uživatel, bude přesměrován na této cestě.</span><span class="sxs-lookup"><span data-stu-id="9584a-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="9584a-176">/ Účet/odhlášení</span><span class="sxs-lookup"><span data-stu-id="9584a-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="9584a-177">Pokud se uživatel nezdaří kontrolu autorizace, bude přesměrován na této cestě.</span><span class="sxs-lookup"><span data-stu-id="9584a-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="9584a-178">V případě hodnoty true bude nový čas vypršení platnosti, pokud je aktuální soubor cookie vyčerpal více než polovinu životnosti vydán nový soubor cookie.</span><span class="sxs-lookup"><span data-stu-id="9584a-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="9584a-179">/ Účet nebo AccessDenied</span><span class="sxs-lookup"><span data-stu-id="9584a-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="9584a-180">Určuje název parametru řetězce dotazu, který je middlewarem připojen, pokud až po přesměrování 302 na přihlašovací cestě změněn kód stavu 401 Unauthorized status.</span><span class="sxs-lookup"><span data-stu-id="9584a-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="9584a-181">true</span><span class="sxs-lookup"><span data-stu-id="9584a-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="9584a-182">To platí pouze pro ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9584a-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="9584a-183">Logický název pro konkrétní ověřování schématu.</span><span class="sxs-lookup"><span data-stu-id="9584a-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="9584a-184">Tento příznak platí pouze pro ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9584a-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="9584a-185">V případě hodnoty true ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.</span><span class="sxs-lookup"><span data-stu-id="9584a-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
