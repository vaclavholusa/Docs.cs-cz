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
# <a name="configure-identity"></a>Konfigurace Identity

ASP.NET Core Identity má některé výchozí chování lze přepsat snadno ve vaší aplikaci `Startup` třídy.

## <a name="passwords-policy"></a>Zásady hesel

Ve výchozím nastavení Identity vyžaduje, aby hesla obsahovat velké písmeno, malé písmeno, číslici a jiný než alfanumerický znak. Existují také některé další omezení. Pokud chcete zjednodušit omezení heslo, můžete to udělat `Startup` třída vaší aplikace.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Jádro ASP.NET 2.0, které jsou přidány `RequiredUniqueChars` vlastnost. Jinak hodnota možnosti jsou stejné z ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`má následující vlastnosti:
* `RequireDigit`: Vyžaduje číslo mezi 0-9 v hesle. Výchozí hodnota je true.
* `RequiredLength`: Minimální délku hesla. Výchozí hodnota je 6.
* `RequireNonAlphanumeric`: Vyžaduje jiný než alfanumerický znak v hesle. Výchozí hodnota je true.
* `RequireUppercase`: Vyžaduje velké písmeno v hesle. Výchozí hodnota je true.
* `RequireLowercase`: Vyžaduje malé písmeno v hesle. Výchozí hodnota je true.
* `RequiredUniqueChars`: Vyžaduje počet jedinečných znaků v hesle. Výchozí hodnota je 1.


## <a name="users-lockout"></a>Uzamčení uživatele

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`má následující vlastnosti:
* `DefaultLockoutTimeSpan`: Množství času, uživatel je uzamčen když dojde k uzamčení. Výchozí hodnota je 5 minut.
* `MaxFailedAccessAttempts`: Počet neúspěšných pokusů o přístup uživatele uzamčení, pokud je povoleno uzamčení. Výchozí hodnota je 5.
* `AllowedForNewUsers`: Určuje, pokud nového uživatele lze uzamknout. Výchozí hodnota je true.


## <a name="sign-in-settings"></a>Přihlaste se nastavení

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`má následující vlastnosti:
* `RequireConfirmedEmail`: Vyžaduje potvrzen e-mail pro přihlášení. Výchozí hodnota je false.
* `RequireConfirmedPhoneNumber`: Vyžaduje bylo potvrzeno telefonní číslo pro přihlášení. Výchozí hodnota je false.


## <a name="user-validation-settings"></a>Nastavení ověření uživatele

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`má následující vlastnosti:
* `RequireUniqueEmail`: Vyžaduje, aby měl jedinečnou e-mailovou každý uživatel. Výchozí hodnota je false.
* `AllowedUserNameCharacters`: V uživatelské jméno povolený počet znaků. Výchozí hodnota je abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Nastavení souborů cookie aplikace

Jako zásady hesla, lze změnit všechna nastavení souboru cookie pro aplikace v `Startup` třídy.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

V části `ConfigureServices` v `Startup` třídu, můžete nakonfigurovat soubor cookie aplikace.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`má následující vlastnosti:
* `Cookie.Name`: Název souboru cookie. Výchozí hodnota. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Pokud je nastavena hodnota true, není soubor cookie přístupný z skripty na straně klienta. Výchozí hodnota je true.
* `ExpireTimeSpan`: Určuje, jak dlouho lístek ověřování uložené v souboru cookie, který zůstane platný od okamžiku, je vytvořena. Výchozí hodnota je 14 dnů.
* `LoginPath`: Pokud neoprávněný uživatel, bude přesměrován na tuto cestu k přihlášení. Výchozí hodnota je/Account/přihlášení.
* `LogoutPath`: Pokud je přihlášený uživatel, bude přesměrován na této cestě. Výchozí hodnota je/Account/odhlášení.
* `AccessDeniedPath`: V případě, že se uživateli nepodaří kontrolu autorizace, bude přesměrován na této cestě. Výchozí hodnota je/Account/AccessDenied.
* `SlidingExpiration`: Pokud je nastavena hodnota true, bude vydána nový soubor cookie s novou čas vypršení platnosti, pokud je aktuální soubor cookie vyčerpal více než polovinu životnosti. Výchozí hodnota je true.
* `ReturnUrlParameter`: ReturnUrlParameter Určuje název parametru řetězce dotazu, který je middlewarem připojen, pokud až po přesměrování 302 na přihlašovací cestě změněn kód stavu 401 Unauthorized status.
* `AuthenticationScheme`: Toto je pouze relevantní pro ASP.NET Core 1.x. Logický název pro konkrétní ověřování schématu.
* `AutomaticAuthenticate`: Tento příznak platí pouze pro ASP.NET Core 1.x. V případě hodnoty true ověřování souborů cookie by měl spustit u každého požadavku a pokus o ověření a proveďte rekonstrukci serializovaný objekt zabezpečení, vytvořen.

