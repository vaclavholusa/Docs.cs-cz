---
title: Přehled zabezpečení jádra ASP.NET
author: rachelappel
description: Další informace o ověřování, autorizaci a zabezpečení základy v ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: da3829b2d5ae5db1861c7423da5afc7acbee6697
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="overview-of-aspnet-core-security"></a>Přehled zabezpečení jádra ASP.NET

ASP.NET Core umožňuje vývojářům snadno konfigurovat a spravovat zabezpečení pro svoje aplikace. ASP.NET Core obsahuje funkce pro správu ověřování, autorizace, ochrany dat, SSL vynucení, tajné klíče aplikace, žádost proti padělání ochrana a správa CORS. Tyto funkce zabezpečení umožňují vytvářet robustní ještě zabezpečení aplikace ASP.NET Core.

## <a name="aspnet-core-security-features"></a>Funkce ASP.NET Core zabezpečení

ASP.NET Core poskytuje řadu nástrojů a knihovny k zabezpečení aplikací včetně předdefinované zprostředkovatele Identity, ale můžete použít 3. stran identity služby jako je Facebook, Twitter a LinkedIn. Pomocí ASP.NET Core můžete snadno spravovat tajné klíče aplikace, které představují způsob, jak ukládat a používat důvěrné informace bez nutnosti vystavit v kódu.

## <a name="authentication-vs-authorization"></a>Ověřování vs. Autorizace

Ověřování je proces, ve kterém uživatel poskytuje pověření, které se pak porovnávají s údajů uložených v operační systém, databáze, aplikace nebo prostředku. Pokud se shodují, uživatelé úspěšně ověřit a potom může provést akce, které mají autorizaci, během autorizačního procesu. Povolení odkazuje na proces, který určuje, co uživatel může provádět.

Dalším způsobem zamyslet nad ověřování je vzít v úvahu jako způsob, jak při autorizaci akce, které můžete uživatel provádět, abyste objektů, které uvnitř toto místo (server, databázi nebo aplikace) zadejte prostor, například server, databáze, aplikace nebo prostředku.

## <a name="common-vulnerabilities-in-software"></a>Běžné chyby zabezpečení v softwaru

ASP.NET Core a EF obsahují funkce, které vám pomohou zabezpečit vaše aplikace a zabránit porušení zabezpečení. Následující seznam odkazů přejdete k dokumentaci s podrobnostmi o techniky předejdete nejběžnějších chyb zabezpečení ve službě web apps:

* [Útoky skriptování mezi weby](xref:security/cross-site-scripting)
* [Útok prostřednictvím injektáže SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Padělání požadavku posílaného mezi weby (proti útokům CSRF)](xref:security/anti-request-forgery)
* [Otevřete přesměrování útoky](xref:security/preventing-open-redirects)

Existují další chyby zabezpečení, které byste měli vědět. Další informace najdete v části v tomto dokumentu na *ASP.NET zabezpečení dokumentaci*.

## <a name="aspnet-security-documentation"></a>Dokumentace k technologii ASP.NET zabezpečení

*   [Ověřování](xref:security/authentication/index)
    *   [Úvod do systému Identity](xref:security/authentication/identity)
    *   [Povolení ověřování přes Facebook, Google a další externí zprostředkovatele](xref:security/authentication/social/index)
    *   [Povolení ověřování pomocí protokolu WS-Federation](xref:security/authentication/ws-federation)
    * [Konfigurace ověřování systému Windows](xref:security/authentication/windowsauth)
    *   [Potvrzení účtu a obnovení hesla](xref:security/authentication/accconfirm)
    *   [Dvoufaktorové ověřování přes SMS](xref:security/authentication/2fa)
    *   [Používala ověřování souborů cookie bez Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrace Azure AD do webové aplikace ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Volání rozhraní API webové jádro ASP.NET z grafického subsystému WPF aplikace pomocí služby Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Volání webového rozhraní API webové aplikace ASP.NET Core pomocí Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Webové aplikace ASP.NET Core s Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
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
        *   [Hodnota hash hesla](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Omezení životnosti chráněných datových částí](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Zrušení ochrany datových částí s odvolanými klíči](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Konfigurace](xref:security/data-protection/configuration/index)
        *   [Konfigurace ochrany dat](xref:security/data-protection/configuration/overview)
        *   [Výchozí nastavení](xref:security/data-protection/configuration/default-settings)
        *   [Zásady pro celý počítač](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Scénáře využívající DI](xref:security/data-protection/configuration/non-di-scenarios)
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
        *   [Klíče neměnitelnosti a nastavení](xref:security/data-protection/implementation/key-immutability)
        *   [Formát ukládání klíčů](xref:security/data-protection/implementation/key-storage-format)
        *   [Zprostředkovatelé dočasné ochrany dat](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Kompatibilita](xref:security/data-protection/compatibility/index)
        *   [Nahrazení <machineKey> v ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Vytvoření aplikace s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data)
*   [Bezpečné úložiště tajné klíče aplikace v vývoj](xref:security/app-secrets)
*   [Zprostředkovatel konfigurace služby Azure Key Vault](xref:security/key-vault-configuration)
*   [Vynucení SSL](xref:security/enforcing-ssl)
*   [Ochrana proti padělání požadavků](xref:security/anti-request-forgery)
*   [Prevence útoků založených na otevřeném přesměrování](xref:security/preventing-open-redirects)
*   [Obrana proti skriptování mezi weby](xref:security/cross-site-scripting)
*   [Povolení žádostí nepůvodního zdroje (CORS)](xref:security/cors)
*   [Sdílení souborů cookie mezi aplikacemi](xref:security/cookie-sharing)
