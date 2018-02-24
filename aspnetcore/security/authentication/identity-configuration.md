---
title: Nakonfigurujte identitu ASP.NET Core
author: AdrienTorris
description: "Výchozí hodnoty ASP.NET Core Identity a zjistěte, jak nakonfigurovat vlastnosti Identity, které chcete použít vlastní hodnoty."
manager: wpickett
ms.author: scaddie
ms.date: 02/21/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: c6f67240c4bfa5ddc1c3aad6c6270ed07349bc72
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="configure-identity"></a>Konfigurace Identity

Jádro ASP.NET Identity použije výchozí konfiguraci pro nastavení, jako například zásady hesel, uzamčení čas a nastavení souborů cookie. Tato nastavení mohou být přepsána nastaveními v aplikace `Startup` třídy.

## <a name="identity-options"></a>Možnosti identity

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) třída reprezentuje možnosti, které můžete použít ke konfiguraci systém identit.

### <a name="claims-identity"></a>Deklarace Identity

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Určuje [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) s vlastnostmi uvedené v tabulce.

| Vlastnost | Popis | Výchozí |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Získá nebo nastaví typ deklarace použitý pro deklarace rolí. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Získá nebo nastaví typ deklarace použitý pro deklarace identity razítko zabezpečení. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Získá nebo nastaví typ deklarace použitý pro deklarace identity identifikátor uživatele. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Získá nebo nastaví typ deklarace použitý pro deklarace identity názvu uživatele. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Uzamčení

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Určuje [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) s vlastnostmi uvedené v tabulce.

| Vlastnost | Popis | Výchozí |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Určuje, pokud nového uživatele lze uzamknout. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Množství času, uživatel je uzamčen když dojde k uzamčení. | 5 minut |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Počet neúspěšných pokusů o přístup uživatele uzamčení, pokud je povoleno uzamčení. | 5 |

### <a name="password"></a>Heslo

Ve výchozím nastavení Identity vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak. Hesla musí být nejméně šest znaků. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) lze změnit v `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Jádro ASP.NET 2.0, které jsou přidány [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) vlastnost. Možnosti, jinak jsou stejné jako ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Určuje [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) s vlastnostmi uvedené v tabulce.

| Vlastnost | Popis | Výchozí |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Vyžaduje číslo mezi 0-9 v hesle. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimální délka hesla. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Vztahuje se pouze na technologii ASP.NET Core 2.0 nebo novější.<br><br> Vyžaduje počet jedinečných znaků v hesle. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Vyžaduje malé písmeno v hesle. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Vyžaduje jiný než alfanumerický znak v hesle. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Vyžaduje velké písmeno v hesle. | `true` |

### <a name="sign-in"></a>Přihlášení

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Určuje [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) s vlastnostmi uvedené v tabulce.

| Vlastnost | Popis | Výchozí |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Vyžaduje potvrzen e-mail pro přihlášení. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Vyžaduje bylo potvrzeno telefonní číslo pro přihlášení. | `false` |

### <a name="tokens"></a>Tokeny

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Určuje [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) s vlastnostmi uvedené v tabulce.

| Vlastnost | Popis |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Získá nebo nastaví `AuthenticatorTokenProvider` použitý k přihlášení pomocí ověřovací data dvoufaktorové ověření. |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Získá nebo nastaví `ChangeEmailTokenProvider` používají ke generování tokenů použít v e-mailu změnu potvrzení e-mailů. |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Získá nebo nastaví `ChangePhoneNumberTokenProvider` používají ke generování tokenů při změně telefonních čísel. |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Získá nebo nastaví zprostředkovatele tokenu používají ke generování tokenů použít v e-mailech potvrzení účtu. |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Získá nebo nastaví [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) používají ke generování tokenů použít v e-mailech resetování hesla. |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Použitý k vytvoření [zprostředkovatele tokenu uživatele](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) klíčem použít jako název poskytovatele. |

### <a name="user"></a>Uživatel

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Určuje [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) s vlastnostmi uvedené v tabulce.

| Vlastnost | Popis | Výchozí |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Povolený počet znaků v uživatelské jméno. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Vyžaduje, aby měl jedinečnou e-mailovou každý uživatel. | `false` |

## <a name="cookie-settings"></a>Nastavení souborů cookie

Konfigurovat soubor cookie aplikace v `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) má následující vlastnosti:

| Vlastnost | Popis |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | Obslužná rutina informuje, že by se měl změnit odchozí *403 Zakázáno* stavový kód do *přesměrováním 302* na zadané cestě.<br><br>Výchozí hodnota je `/Account/AccessDenied`. |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | Platí jenom pro ASP.NET Core 1.x.<br><br> Logický název pro konkrétní ověřování schématu. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | Platí jenom pro ASP.NET Core 1.x.<br><br> V případě hodnoty true ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | Platí jenom pro ASP.NET Core 1.x.<br><br> V případě hodnoty true zpracovává middleware ověřování automatické problémy. Pokud hodnotu false, middleware ověřování jenom mění odpovědi, když explicitně uvedené `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | Získá nebo nastaví vystavitele, který se má použít pro deklarace identity, které jsou vytvořené (zděděno z [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Domény, ke které soubor cookie přidružit. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | Získá nebo nastaví životnost souboru cookie. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Určuje, zda je soubor cookie přístupný pro skript na straně klienta.<br><br>Výchozí hodnota je `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Název souboru cookie.<br><br>Výchozí hodnota je `.AspNetCore.Cookies`. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Cesta k souboru cookie. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | `SameSite` Atribut souboru cookie.<br><br>Výchozí hodnota je [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) konfigurace.<br><br>Výchozí hodnota je [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy). |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | Platí jenom pro ASP.NET Core 1.x.<br><br> Název domény, kde je soubor cookie zpracovat. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | Platí jenom pro ASP.NET Core 1.x.<br><br> Příznak, který udává, pokud je soubor cookie musí být přístupný jenom pro servery.<br><br>Výchozí hodnota je `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | Platí jenom pro ASP.NET Core 1.x.<br><br> Použít k izolování aplikací běžících na stejný název hostitele. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | Platí jenom pro ASP.NET Core 1.x.<br><br> Příznak, který udává, pokud je třeba vytvořit soubor cookie omezit na HTTPS (`CookieSecurePolicy.Always`), HTTP nebo HTTPS (`CookieSecurePolicy.None`), nebo stejný protokol jako požadavku (`CookieSecurePolicy.SameAsRequest`).<br><br>Výchozí hodnota je `CookieSecurePolicy.SameAsRequest`. |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | Komponenta, kterou používá k získání souborů cookie z požadavku nebo k jejich nastavení u odpovědi. | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | Pokud nastavíte, zprostředkovatele používají [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) ochrany dat. |
| [Popis](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | Platí jenom pro ASP.NET Core 1.x.<br><br> Další informace o typu ověřování, který je k dispozici na aplikaci. |
| [Události](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | Obslužná rutina volá metody zprostředkovatele, které poskytnou aplikaci kontrolu v určitých bodech, kde dochází k zpracování. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | Pokud nastavíte, službu zadejte získat `Events` instanci místo vlastnost (zděděno z [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | Ovládací prvky, kolik času uložené zůstane soubor cookie platný od okamžiku, které se vytvoří lístek ověřování.<br><br>Výchozí hodnota je 14 dnů. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | Pokud uživatel není autorizovaný, budou přesměrováni na tuto cestu k přihlášení.<br><br>Výchozí hodnota je `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | Při přihlášení uživatele přesměrován se k této cestě.<br><br>Výchozí hodnota je `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | Určuje název parametru řetězce dotazu, který je middlewarem připojen při *401 – Neověřeno* kód stavu se změní na *přesměrováním 302* na přihlašovací cestě.<br><br>Výchozí hodnota je `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | Volitelný kontejner, do kterého chcete ukládat identita u různých požadavků. |
| [parametr slidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | V případě hodnoty true je vydána nový soubor cookie s novou čas vypršení platnosti, pokud je aktuální soubor cookie vyčerpal více než polovinu životnosti.<br><br>Výchozí hodnota je `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | `TicketDataFormat` Se používá k ochraně a zrušení identity a dalších vlastností, které jsou uloženy v hodnotě souboru cookie. |
