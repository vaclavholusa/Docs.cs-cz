---
title: Konfigurace ASP.NET Core Identity
author: AdrienTorris
description: ASP.NET Core Identity výchozí hodnoty a zjistěte, jak nakonfigurovat vlastnosti Identity použít vlastní hodnoty.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: c597eacbb21ed0968e6195f7b6dcb46d37ba80a5
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870914"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="0c298-103">Konfigurace ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="0c298-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="0c298-104">ASP.NET Core Identity používá výchozí hodnoty pro nastavení jako zásady hesel, uzamčení a konfigurace souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="0c298-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="0c298-105">Tato nastavení mohou být přepsána nastaveními v `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="0c298-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="0c298-106">Možnosti identity</span><span class="sxs-lookup"><span data-stu-id="0c298-106">Identity options</span></span>

<span data-ttu-id="0c298-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) třída reprezentuje možnosti, které lze použít ke konfiguraci systému identit.</span><span class="sxs-lookup"><span data-stu-id="0c298-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="0c298-108">`IdentityOptions` musí být nastavena **po** volání `AddIdentity` nebo `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="0c298-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="0c298-109">Deklarace Identity</span><span class="sxs-lookup"><span data-stu-id="0c298-109">Claims Identity</span></span>

<span data-ttu-id="0c298-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Určuje, [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) s vlastnostmi, které jsou uvedeny v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c298-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="0c298-111">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c298-111">Property</span></span> | <span data-ttu-id="0c298-112">Popis</span><span class="sxs-lookup"><span data-stu-id="0c298-112">Description</span></span> | <span data-ttu-id="0c298-113">Výchozí</span><span class="sxs-lookup"><span data-stu-id="0c298-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0c298-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="0c298-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="0c298-115">Získá nebo nastaví typ deklarace použitý pro deklarace role.</span><span class="sxs-lookup"><span data-stu-id="0c298-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="0c298-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="0c298-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="0c298-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="0c298-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="0c298-118">Získá nebo nastaví typ deklarace použitý pro deklarace identity razítko zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0c298-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="0c298-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="0c298-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="0c298-120">Získá nebo nastaví typ deklarace použitý pro deklarace identity identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c298-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="0c298-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="0c298-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="0c298-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="0c298-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="0c298-123">Získá nebo nastaví typ deklarace použitý pro deklarace identity uživatele název.</span><span class="sxs-lookup"><span data-stu-id="0c298-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="0c298-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="0c298-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="0c298-125">Uzamčení</span><span class="sxs-lookup"><span data-stu-id="0c298-125">Lockout</span></span>

<span data-ttu-id="0c298-126">Uzamčení se nastavuje v [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metody:</span><span class="sxs-lookup"><span data-stu-id="0c298-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="0c298-127">Předchozí kód je založený na `Login` Identity šablony.</span><span class="sxs-lookup"><span data-stu-id="0c298-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="0c298-128">Možnosti uzamčení se nastavují v `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0c298-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="0c298-129">Předchozí kód nastaví [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) s výchozími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0c298-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="0c298-130">Po provedení úspěšného ověření resetuje počet pokusů o neúspěšných přístupů a resetuje na hodiny.</span><span class="sxs-lookup"><span data-stu-id="0c298-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="0c298-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Určuje, [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) s vlastnostmi, které jsou uvedené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c298-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0c298-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c298-132">Property</span></span> | <span data-ttu-id="0c298-133">Popis</span><span class="sxs-lookup"><span data-stu-id="0c298-133">Description</span></span> | <span data-ttu-id="0c298-134">Výchozí</span><span class="sxs-lookup"><span data-stu-id="0c298-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0c298-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="0c298-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="0c298-136">Určuje, pokud nového uživatele lze uzamknout.</span><span class="sxs-lookup"><span data-stu-id="0c298-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="0c298-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="0c298-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="0c298-138">Množství času uživatele je uzamčen když dojde k uzamčení.</span><span class="sxs-lookup"><span data-stu-id="0c298-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="0c298-139">5 minut</span><span class="sxs-lookup"><span data-stu-id="0c298-139">5 minutes</span></span> |
| [<span data-ttu-id="0c298-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="0c298-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="0c298-141">Počet neúspěšných pokusů o přístup dokud uživatel je uzamčen, pokud je povoleno uzamčení.</span><span class="sxs-lookup"><span data-stu-id="0c298-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="0c298-142">5</span><span class="sxs-lookup"><span data-stu-id="0c298-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="0c298-143">Heslo</span><span class="sxs-lookup"><span data-stu-id="0c298-143">Password</span></span>

<span data-ttu-id="0c298-144">Ve výchozím nastavení identita vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak.</span><span class="sxs-lookup"><span data-stu-id="0c298-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="0c298-145">Hesla musí být aspoň šest znaků dlouhá.</span><span class="sxs-lookup"><span data-stu-id="0c298-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="0c298-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) je možné nastavit v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0c298-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="0c298-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Určuje, [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) s vlastnostmi, které jsou uvedené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c298-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0c298-148">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c298-148">Property</span></span> | <span data-ttu-id="0c298-149">Popis</span><span class="sxs-lookup"><span data-stu-id="0c298-149">Description</span></span> | <span data-ttu-id="0c298-150">Výchozí</span><span class="sxs-lookup"><span data-stu-id="0c298-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0c298-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="0c298-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="0c298-152">Vyžaduje číslo v rozmezí 0 – 9 v hesle.</span><span class="sxs-lookup"><span data-stu-id="0c298-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="0c298-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="0c298-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="0c298-154">Minimální délka hesla.</span><span class="sxs-lookup"><span data-stu-id="0c298-154">The minimum length of the password.</span></span> | <span data-ttu-id="0c298-155">6</span><span class="sxs-lookup"><span data-stu-id="0c298-155">6</span></span> |

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0c298-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Platí jenom pro ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0c298-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="0c298-157">Vyžaduje počet jedinečných znaků v hesle.</span><span class="sxs-lookup"><span data-stu-id="0c298-157">Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="0c298-158">| 1 |</span><span class="sxs-lookup"><span data-stu-id="0c298-158">| 1 |</span></span>

::: moniker-end

<span data-ttu-id="0c298-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Vyžaduje malé písmeno v hesle.</span><span class="sxs-lookup"><span data-stu-id="0c298-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requires a lowercase character in the password.</span></span><span data-ttu-id="0c298-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Vyžaduje nealfanumerických znaků v hesle.</span><span class="sxs-lookup"><span data-stu-id="0c298-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requires a non-alphanumeric character in the password.</span></span><span data-ttu-id="0c298-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Vyžaduje velké písmeno heslo.</span><span class="sxs-lookup"><span data-stu-id="0c298-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requires an uppercase character in the password.</span></span> | `true` |

### <a name="sign-in"></a><span data-ttu-id="0c298-162">přihlášení</span><span class="sxs-lookup"><span data-stu-id="0c298-162">Sign-in</span></span>

<span data-ttu-id="0c298-163">Následující kód nastaví `SignIn` nastavení (výchozí hodnoty):</span><span class="sxs-lookup"><span data-stu-id="0c298-163">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="0c298-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Určuje, [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) s vlastnostmi, které jsou uvedené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c298-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0c298-165">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c298-165">Property</span></span> | <span data-ttu-id="0c298-166">Popis</span><span class="sxs-lookup"><span data-stu-id="0c298-166">Description</span></span> | <span data-ttu-id="0c298-167">Výchozí</span><span class="sxs-lookup"><span data-stu-id="0c298-167">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0c298-168">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="0c298-168">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="0c298-169">Vyžaduje potvrzeno e-mailu pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0c298-169">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="0c298-170">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="0c298-170">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="0c298-171">Vyžaduje potvrzeno telefonní číslo pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0c298-171">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="0c298-172">Tokeny</span><span class="sxs-lookup"><span data-stu-id="0c298-172">Tokens</span></span>

<span data-ttu-id="0c298-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Určuje, [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) s vlastnostmi, které jsou uvedené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c298-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="0c298-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c298-174">Property</span></span>                                                         |                                                                                      <span data-ttu-id="0c298-175">Popis</span><span class="sxs-lookup"><span data-stu-id="0c298-175">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="0c298-176">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0c298-176">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="0c298-177">Získá nebo nastaví `AuthenticatorTokenProvider` použitý k ověření dvojúrovňového přihlášení pomocí ověřovací data.</span><span class="sxs-lookup"><span data-stu-id="0c298-177">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="0c298-178">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0c298-178">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="0c298-179">Získá nebo nastaví `ChangeEmailTokenProvider` sloužící ke generování tokenů, které používá e-mailu změnit potvrzení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="0c298-179">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="0c298-180">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0c298-180">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="0c298-181">Získá nebo nastaví `ChangePhoneNumberTokenProvider` sloužící ke generování tokenů při změně telefonního čísla.</span><span class="sxs-lookup"><span data-stu-id="0c298-181">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="0c298-182">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0c298-182">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="0c298-183">Získá nebo nastaví poskytovatele tokenu, kterého používá ke generování tokenů použít účet potvrzení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="0c298-183">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="0c298-184">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0c298-184">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="0c298-185">Získá nebo nastaví [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) sloužící ke generování tokenů, které používá e-mailů resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="0c298-185">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="0c298-186">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="0c298-186">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="0c298-187">Slouží k vytvoření [Poskytovatel tokenu uživatele](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) s klíč použít jako název zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="0c298-187">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="0c298-188">Uživatel</span><span class="sxs-lookup"><span data-stu-id="0c298-188">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="0c298-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Určuje, [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) s vlastnostmi, které jsou uvedené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c298-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0c298-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0c298-190">Property</span></span> | <span data-ttu-id="0c298-191">Popis</span><span class="sxs-lookup"><span data-stu-id="0c298-191">Description</span></span> | <span data-ttu-id="0c298-192">Výchozí</span><span class="sxs-lookup"><span data-stu-id="0c298-192">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0c298-193">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="0c298-193">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="0c298-194">Povolené znaky uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="0c298-194">Allowed characters in the username.</span></span> | <span data-ttu-id="0c298-195">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="0c298-195">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="0c298-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="0c298-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="0c298-197">0123456789</span><span class="sxs-lookup"><span data-stu-id="0c298-197">0123456789</span></span><br><span data-ttu-id="0c298-198">-._@+</span><span class="sxs-lookup"><span data-stu-id="0c298-198">-._@+</span></span> |
| [<span data-ttu-id="0c298-199">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="0c298-199">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="0c298-200">Vyžaduje každý uživatel měl jedinečnou e-mailovou.</span><span class="sxs-lookup"><span data-stu-id="0c298-200">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="0c298-201">Nastavení souborů cookie</span><span class="sxs-lookup"><span data-stu-id="0c298-201">Cookie settings</span></span>

<span data-ttu-id="0c298-202">Konfigurovat soubor cookie aplikace v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0c298-202">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0c298-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) musí být volána **po** volání `AddIdentity` nebo `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="0c298-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="0c298-204">Další informace najdete v tématu [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="0c298-204">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>