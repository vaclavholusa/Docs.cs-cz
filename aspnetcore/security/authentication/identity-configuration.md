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
# <a name="configure-identity"></a>Konfigurace Identity

ASP.NET Core Identity má společné chování v aplikacích jako zásady hesel, uzamčení čas a nastavení souborů cookie, které ve vaší aplikaci můžete snadno přepsat `Startup` třídy.

## <a name="passwords-policy"></a>Zásady hesel

Ve výchozím nastavení Identity vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak. Existují také některé další omezení. Pro zjednodušení omezení hesla, upravte `ConfigureServices` metodu `Startup` třída vaší aplikace.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Jádro ASP.NET 2.0, které jsou přidány `RequiredUniqueChars` vlastnost. Jinak hodnota možnosti jsou stejné z ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`má následující vlastnosti:

| Vlastnost                | Popis                       | Výchozí |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Vyžaduje číslo mezi 0-9 v hesle. | true |
| `RequiredLength`        | Minimální délka hesla. | 6 |
| `RequireNonAlphanumeric`| Vyžaduje jiný než alfanumerický znak v hesle. | true |
| `RequireUppercase`      | Vyžaduje velké písmeno v hesle. | true |
| `RequireLowercase`      | Vyžaduje malé písmeno v hesle. | true |
| `RequiredUniqueChars`   | Vyžaduje počet jedinečných znaků v hesle. | 1 |


## <a name="users-lockout"></a>Uzamčení uživatele

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`má následující vlastnosti:

| Vlastnost                | Popis                       | Výchozí |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | Množství času, uživatel je uzamčen když dojde k uzamčení.  | 5 minut  |
| `MaxFailedAccessAttempts` | Počet neúspěšných pokusů o přístup uživatele uzamčení, pokud je povoleno uzamčení.  | 5 |
| `AllowedForNewUsers` | Určuje, pokud nového uživatele lze uzamknout.  | true |

## <a name="sign-in-settings"></a>Přihlaste se nastavení

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`má následující vlastnosti:

| Vlastnost                | Popis                       | Výchozí |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Vyžaduje potvrzen e-mail pro přihlášení. | false  |
| `RequireConfirmedPhoneNumber` |  Vyžaduje bylo potvrzeno telefonní číslo pro přihlášení. | false  |

## <a name="user-validation-settings"></a>Nastavení ověření uživatele

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`má následující vlastnosti:

| Vlastnost                | Popis                       | Výchozí |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Vyžaduje, aby měl jedinečnou e-mailovou každý uživatel. | false  |
| `AllowedUserNameCharacters`  | Povolený počet znaků v uživatelské jméno. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Nastavení souborů cookie aplikace

Jako zásady hesla, lze změnit všechna nastavení souboru cookie pro aplikace v `Startup` třídy.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

V části `ConfigureServices` v `Startup` třídu, můžete nakonfigurovat soubor cookie aplikace.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`má následující vlastnosti:

| Vlastnost                | Popis                       | Výchozí |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Název souboru cookie.  | . AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | V případě hodnoty true, není soubor cookie přístupný z skripty na straně klienta.  |  true |
| `ExpireTimeSpan`  | Určuje, jak dlouho lístek ověřování uložené v souboru cookie, který zůstane platný od okamžiku, je vytvořena.  | 14 dnů  |
| `LoginPath`  | Pokud uživatel není autorizovaný, bude přesměrován na tuto cestu k přihlášení. | / Účet nebo přihlášení  |
| `LogoutPath`  | Když je přihlášený uživatel, bude přesměrován na této cestě.  | / Účet/odhlášení  |
| `AccessDeniedPath`  | Pokud se uživatel nezdaří kontrolu autorizace, bude přesměrován na této cestě.  |   |
| `SlidingExpiration`  | V případě hodnoty true bude nový čas vypršení platnosti, pokud je aktuální soubor cookie vyčerpal více než polovinu životnosti vydán nový soubor cookie.  | / Účet nebo AccessDenied |
| `ReturnUrlParameter`  | Určuje název parametru řetězce dotazu, který je middlewarem připojen, pokud až po přesměrování 302 na přihlašovací cestě změněn kód stavu 401 Unauthorized status.  |  true |
| `AuthenticationScheme`  | To platí pouze pro ASP.NET Core 1.x. Logický název pro konkrétní ověřování schématu. |  |
| `AutomaticAuthenticate`  | Tento příznak platí pouze pro ASP.NET Core 1.x. V případě hodnoty true ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.  |  |