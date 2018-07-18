---
title: Přehled zabezpečení ASP.NET Core
author: tdykstra
description: Další informace o ověřování, autorizaci a zabezpečení základy v ASP.NET Core.
ms.author: tdykstra
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: ed64594c85d555d8417903947fc3ce927dc04cec
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095759"
---
# <a name="overview-of-aspnet-core-security"></a>Přehled zabezpečení ASP.NET Core

ASP.NET Core umožňuje vývojářům snadno konfigurovat a spravovat zabezpečení pro své aplikace. ASP.NET Core obsahuje funkce pro správu ověřování, autorizaci, ochranu dat, vynucování SSL, tajných kódů aplikace, ochrana proti padělání požadavků ochrany a správy CORS. Tyto funkce zabezpečení umožňují vytvářet robustní ještě zabezpečení aplikací ASP.NET Core.

## <a name="aspnet-core-security-features"></a>Funkce zabezpečení ASP.NET Core

ASP.NET Core nabízí mnoho nástrojů a knihoven k zabezpečení vašich aplikací, včetně předdefinovaných poskytovatelů identit ale můžete použít 3. stran identit služeb jako je Facebook, Twitter a LinkedIn. ASP.NET Core můžete snadno spravovat tajné kódy aplikace, které představují způsob, jak uložit a použít důvěrné informace, aniž byste museli ho zpřístupnit v kódu.

## <a name="authentication-vs-authorization"></a>Ověřování vs. Autorizace

Ověřování je proces, ve kterém uživatel zadá přihlašovací údaje, které jsou pak porovnána s uloženými v operačním systému, databáze, aplikace nebo prostředku. Pokud se shodují, úspěšně ověřit uživatele a pak můžete provádět akce, které mají autorizaci pro, během autorizace. Povolení se vztahuje k procesu, která určuje, co uživatel může dělat.

Dalším způsobem, jak si můžete představit ověřování je vzít v úvahu jako způsob, jak při autorizaci akce, které může uživatel provádět, do které objekty do tohoto místa (server, databáze nebo aplikace) zadejte mezeru, například server, databáze, aplikace nebo prostředku.

## <a name="common-vulnerabilities-in-software"></a>Běžné chyby v softwaru

ASP.NET Core a EF obsahují funkce, které vám pomůžou zabezpečit vaše aplikace a zabránit porušení zabezpečení. Následující seznam odkazů přejdete k dokumentaci s podrobnostmi o techniky, aby nejběžnějších ohrožení zabezpečení ve službě web apps:

* [Útoky skriptování napříč weby](xref:security/cross-site-scripting)
* [Útoky prostřednictvím injektáže SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Proti padělání požadavků mezi weby (CSRF)](xref:security/anti-request-forgery)
* [Útoky na otevřeném přesměrování](xref:security/preventing-open-redirects)

Existují další chyby zabezpečení, které byste měli vědět. Další informace najdete v části v tomto dokumentu na *dokumentace k ASP.NET je zabezpečení*.

## <a name="aspnet-security-documentation"></a>Dokumentace ke službě Security technologie ASP.NET

*   [Ověřování](xref:security/authentication/index)
    *   [Úvod do systému Identity](xref:security/authentication/identity)
    *   [Povolení ověřování přes Facebook, Google a další externí zprostředkovatele](xref:security/authentication/social/index)
    *   [Povolení ověření přes WS-Federation](xref:security/authentication/ws-federation)
    * [Konfigurace ověřování systému Windows](xref:security/authentication/windowsauth)
    *   [Potvrzení účtu a obnovení hesla](xref:security/authentication/accconfirm)
    *   [Dvoufaktorové ověřování přes SMS](xref:security/authentication/2fa)
    *   [Používala ověřování souborů cookie bez systému Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrace Azure AD do webové aplikace ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Volání rozhraní API pro ASP.NET Core Web z aplikace WPF pomocí Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Volání webového rozhraní API webové aplikace ASP.NET Core pomocí Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Webovou aplikaci ASP.NET Core s Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Zabezpečení aplikací ASP.NET serverem IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorizace](xref:security/authorization/index)
    *   [Úvod](xref:security/authorization/introduction)
    *   [Vytvoření aplikace s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data)
    *   [Jednoduchá autorizace](xref:security/authorization/simple)
    *   [Ověřování založené na rolích](xref:security/authorization/roles)
    *   [Autorizace na základě deklarace identity](xref:security/authorization/claims)
    *   [Autorizace na základě zásad](xref:security/authorization/policies)
    *   [Injektáž závislostí v obslužných rutinách požadavků](xref:security/authorization/dependencyinjection)
    *   [Autorizace na základě prostředků](xref:security/authorization/resourcebased)
    *   [Autorizace na základě zobrazení](xref:security/authorization/views)
    *   [Omezení identity schématem](xref:security/authorization/limitingidentitybyscheme)
*   [Ochrana dat](xref:security/data-protection/index)
    *   [Úvod do ochrany dat](xref:security/data-protection/introduction)
    *   [Začínáme s rozhraními API na ochranu dat](xref:security/data-protection/using-data-protection)
    *   [Rozhraní API příjemců](xref:security/data-protection/consumer-apis/index)
        *   [Přehled rozhraní API příjemců](xref:security/data-protection/consumer-apis/overview)
        *   [Účelové řetězce](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Hierarchie účelů a víceklientská architektura](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Určení hodnoty hash hesel](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Omezení životnosti chráněných datových částí](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Zrušení ochrany datových částí s odvolanými klíči](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Konfigurace](xref:security/data-protection/configuration/index)
        *   [Konfigurace ochrany dat](xref:security/data-protection/configuration/overview)
        *   [Výchozí nastavení](xref:security/data-protection/configuration/default-settings)
        *   [Zásady pro celý počítač](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Používající DI scénáře](xref:security/data-protection/configuration/non-di-scenarios)
    *   [Rozšiřující rozhraní API](xref:security/data-protection/extensibility/index)
        *   [Rozšiřitelnost základní kryptografie](xref:security/data-protection/extensibility/core-crypto)
        *   [Rozšiřitelnost správy klíčů](xref:security/data-protection/extensibility/key-management)
        *   [Ostatní rozhraní API](xref:security/data-protection/extensibility/misc-apis)
    *   [Implementace](xref:security/data-protection/implementation/index)
        *   [Podrobnosti ověřeného šifrování](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Odvozování podklíčů a ověřené šifrování](xref:security/data-protection/implementation/subkeyderivation)
        *   [Kontextová záhlaví](xref:security/data-protection/implementation/context-headers)
        *   [Správa klíčů](xref:security/data-protection/implementation/key-management)
        *   [Zprostředkovatelé úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers)
        *   [Šifrování klíčů v klidovém stavu](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Neměnnost klíče a nastavení](xref:security/data-protection/implementation/key-immutability)
        *   [Formát ukládání klíčů](xref:security/data-protection/implementation/key-storage-format)
        *   [Zprostředkovatelé dočasné ochrany dat](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Kompatibilita](xref:security/data-protection/compatibility/index)
        *   [Nahrazení <machineKey> v ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Vytvoření aplikace s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data)
*   [Bezpečné ukládání tajných kódů aplikace při vývoji](xref:security/app-secrets)
*   [Zprostředkovatel konfigurace služby Azure Key Vault](xref:security/key-vault-configuration)
*   [Vynucení SSL](xref:security/enforcing-ssl)
*   [Ochrana proti padělání požadavků](xref:security/anti-request-forgery)
*   [Prevence útoků založených na otevřeném přesměrování](xref:security/preventing-open-redirects)
*   [Obrana proti skriptování mezi weby](xref:security/cross-site-scripting)
*   [Povolení žádostí nepůvodního zdroje (CORS)](xref:security/cors)
*   [Sdílení souborů cookie mezi aplikacemi](xref:security/cookie-sharing)
