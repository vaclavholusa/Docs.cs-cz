---
title: Zachování další deklarace identity a tokeny od externích poskytovatelů v ASP.NET Core
author: guardrex
description: Zjistěte, jak vytvořit další deklarace identity a tokeny od externích poskytovatelů.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: dc8b3e32141466a12e4eff0c86d2d4bed689afe5
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206354"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="6d802-103">Zachování další deklarace identity a tokeny od externích poskytovatelů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d802-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="6d802-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6d802-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6d802-105">Aplikace ASP.NET Core můžete vytvořit další deklarace identity a tokenů z externího zprostředkovatele ověřování, jako je Facebook, Google, Microsoft a Twitter.</span><span class="sxs-lookup"><span data-stu-id="6d802-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="6d802-106">Každý poskytovatel odhalí různé informace o uživatelích na své platformě, ale vzor pro příjem a transformaci dat uživatele do dalších deklarací identity je stejný.</span><span class="sxs-lookup"><span data-stu-id="6d802-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="6d802-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([stažení](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6d802-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="6d802-108">Předpoklad</span><span class="sxs-lookup"><span data-stu-id="6d802-108">Prerequisite</span></span>

<span data-ttu-id="6d802-109">Rozhodněte, které externího zprostředkovatele ověřování pro podporu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6d802-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="6d802-110">Pro každého zprostředkovatele aplikace registrovaly a získaly ID klienta a tajný kód klienta.</span><span class="sxs-lookup"><span data-stu-id="6d802-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="6d802-111">Další informace naleznete v tématu <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="6d802-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="6d802-112">[Ukázkovou aplikaci](#sample-app-instructions) používá [zprostředkovatele ověřování Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="6d802-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="6d802-113">Konfigurace poskytovatele ověřování</span><span class="sxs-lookup"><span data-stu-id="6d802-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="6d802-114">Nastavte ID klienta a tajný kód klienta</span><span class="sxs-lookup"><span data-stu-id="6d802-114">Set the client ID and client secret</span></span>

<span data-ttu-id="6d802-115">Zprostředkovatele ověřování OAuth vytváří vztah důvěryhodnosti s aplikací pomocí ID klienta a tajný kód klienta.</span><span class="sxs-lookup"><span data-stu-id="6d802-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="6d802-116">ID klienta a hodnoty tajných kódů klienta jsou vytvořené pro aplikaci zprostředkovatele externího ověřování při registraci aplikace s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="6d802-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="6d802-117">Každý externího poskytovatele, který používá aplikace musí mít nakonfigurovanou nezávisle na sobě ID klienta a tajný kód klienta poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="6d802-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="6d802-118">Další informace najdete v tématech zprostředkovatele externího ověřování, které platí pro váš scénář:</span><span class="sxs-lookup"><span data-stu-id="6d802-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="6d802-119">Ověřování Facebooku</span><span class="sxs-lookup"><span data-stu-id="6d802-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="6d802-120">Ověřování Googlu</span><span class="sxs-lookup"><span data-stu-id="6d802-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="6d802-121">Ověřování Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6d802-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="6d802-122">Ověřování Twitteru</span><span class="sxs-lookup"><span data-stu-id="6d802-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="6d802-123">Další zprostředkovatelé ověřování</span><span class="sxs-lookup"><span data-stu-id="6d802-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="6d802-124">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="6d802-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="6d802-125">Ukázková aplikace nakonfiguruje zprostředkovatele ověřování Google s ID klienta a tajný kód klienta, které jsou k dispozici společností Google:</span><span class="sxs-lookup"><span data-stu-id="6d802-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="6d802-126">Vytvořit rozsah ověřování</span><span class="sxs-lookup"><span data-stu-id="6d802-126">Establish the authentication scope</span></span>

<span data-ttu-id="6d802-127">Zadejte seznam oprávnění k načtení z poskytovatele tak, že zadáte <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="6d802-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="6d802-128">V následující tabulce se zobrazí obory ověřování pro běžné externího poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="6d802-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="6d802-129">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="6d802-129">Provider</span></span>  | <span data-ttu-id="6d802-130">Rozsah</span><span class="sxs-lookup"><span data-stu-id="6d802-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="6d802-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="6d802-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="6d802-132">Google</span><span class="sxs-lookup"><span data-stu-id="6d802-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="6d802-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="6d802-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="6d802-134">Na twitteru</span><span class="sxs-lookup"><span data-stu-id="6d802-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="6d802-135">Ukázková aplikace přidá ke službě Google `plus.login` oboru požadavku Google + přihlášení oprávnění:</span><span class="sxs-lookup"><span data-stu-id="6d802-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="6d802-136">Mapování uživatelských dat klíčů a vytvářet deklarace identity</span><span class="sxs-lookup"><span data-stu-id="6d802-136">Map user data keys and create claims</span></span>

<span data-ttu-id="6d802-137">V možnostech zprostředkovatele, zadejte <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> pro každý klíč externího poskytovatele JSON uživatelské údaje pro identitu aplikace ke čtení na přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6d802-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="6d802-138">Další informace o typy deklarací identity najdete v tématu <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="6d802-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="6d802-139">Vytvoří ukázkovou aplikaci <xref:System.Security.Claims.ClaimTypes.Gender> deklarace identity z `gender` klíče v Google uživatelská data:</span><span class="sxs-lookup"><span data-stu-id="6d802-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="6d802-140">V <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) je přihlášený k aplikaci s <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="6d802-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="6d802-141">Během procesu, přihlašování <xref:Microsoft.AspNetCore.Identity.UserManager`1> můžete ukládat `ApplicationUser` deklarací identity pro uživatele nejsou k dispozici data <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="6d802-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="6d802-142">V ukázkové aplikaci `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) vytváří <xref:System.Security.Claims.ClaimTypes.Gender> deklarací identity pro podepsané v `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="6d802-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="6d802-143">Uložit přístupový token</span><span class="sxs-lookup"><span data-stu-id="6d802-143">Save the access token</span></span>

<span data-ttu-id="6d802-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> definuje, jestli tokeny přístupu a aktualizace by měla být uložena v <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="6d802-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="6d802-145">`SaveTokens` je nastavena na `false` ve výchozím nastavení ke zmenšení velikosti konečné ověřovacího souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="6d802-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="6d802-146">Ukázková aplikace nastaví hodnotu `SaveTokens` k `true` v <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="6d802-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="6d802-147">Když `OnPostConfirmationAsync` spustí, ukládat přístupový token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) z externího poskytovatele v `ApplicationUser`společnosti `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="6d802-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="6d802-148">Ukázková aplikace uloží přístupového tokenu v:</span><span class="sxs-lookup"><span data-stu-id="6d802-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="6d802-149">`OnPostConfirmationAsync` &ndash; Spustí pro nové registrace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d802-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="6d802-150">`OnGetCallbackAsync` &ndash; Spustí, když se dříve zaregistrovaný uživatel přihlásí do aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d802-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="6d802-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6d802-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="6d802-152">Jak přidat další vlastní tokeny</span><span class="sxs-lookup"><span data-stu-id="6d802-152">How to add additional custom tokens</span></span>

<span data-ttu-id="6d802-153">K předvedení jak přidat vlastní token, který je uložený jako součást `SaveTokens`, přidá ukázkovou aplikaci <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> s aktuálním <xref:System.DateTime> pro [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) z `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="6d802-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="6d802-154">Ukázku pokynů aplikace</span><span class="sxs-lookup"><span data-stu-id="6d802-154">Sample app instructions</span></span>

<span data-ttu-id="6d802-155">Ukázková aplikace předvádí, jak:</span><span class="sxs-lookup"><span data-stu-id="6d802-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="6d802-156">Získat pohlaví uživatele z Googlu a ukládat pohlaví deklaraci identity s hodnotou.</span><span class="sxs-lookup"><span data-stu-id="6d802-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="6d802-157">Přístupový token Google Store v uživatele `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="6d802-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="6d802-158">Použití ukázkové aplikace:</span><span class="sxs-lookup"><span data-stu-id="6d802-158">To use the sample app:</span></span>

1. <span data-ttu-id="6d802-159">Aplikace registrovaly a získaly platné ID klienta a tajný kód klienta pro ověřování Google.</span><span class="sxs-lookup"><span data-stu-id="6d802-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="6d802-160">Další informace naleznete v tématu <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="6d802-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="6d802-161">Zadejte ID klienta a tajný kód klienta aplikace v <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> z `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6d802-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="6d802-162">Spusťte aplikaci a požadavku na stránku Moje deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="6d802-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="6d802-163">Když uživatel není přihlášen, aplikace přesměruje do Googlu.</span><span class="sxs-lookup"><span data-stu-id="6d802-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="6d802-164">Přihlásit se přes Google.</span><span class="sxs-lookup"><span data-stu-id="6d802-164">Sign in with Google.</span></span> <span data-ttu-id="6d802-165">Google přesměruje uživatele zpět do aplikace (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="6d802-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="6d802-166">Ověření uživatele a načtení stránky Moje deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="6d802-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="6d802-167">Není k dispozici v rámci deklarace pohlaví **deklarace identity uživatelů** s hodnotou získanou z Google.</span><span class="sxs-lookup"><span data-stu-id="6d802-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="6d802-168">Přístupový token se zobrazí v **vlastnosti ověřování**.</span><span class="sxs-lookup"><span data-stu-id="6d802-168">The access token appears in the **Authentication Properties**.</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
