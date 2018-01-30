---
title: "Přehled zabezpečení jádra ASP.NET"
author: rachelappel
description: "Další informace o ověřování, autorizaci a zabezpečení základy v ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: e1aaae09fe69e6b65a917785b436f927fac5345d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-security-overview"></a>Přehled zabezpečení ASP.NET Core

ASP.NET Core umožňuje vývojářům snadno konfigurovat a spravovat zabezpečení pro svoje aplikace. ASP.NET Core obsahuje funkce pro správu ověřování, autorizace, ochrany dat, SSL vynucení, tajné klíče aplikace, žádost proti padělání ochrana a správa CORS. Tyto funkce zabezpečení umožňují vytvářet robustní ještě zabezpečení aplikace ASP.NET Core. 

## <a name="aspnet-core-security-features"></a>Funkce ASP.NET Core zabezpečení

ASP.NET Core poskytuje řadu nástrojů a knihovny k zabezpečení aplikací včetně předdefinované zprostředkovatele Identity, ale můžete použít 3. stran identity služby jako je Facebook, Twitter a LinkedIn. Pomocí ASP.NET Core můžete snadno spravovat tajné klíče aplikace, které představují způsob, jak ukládat a používat důvěrné informace bez nutnosti vystavit v kódu. 

## <a name="authentication-vs-authorization"></a>Ověřování vs. Autorizace

Ověřování je proces, ve kterém uživatel poskytuje pověření, které se pak porovnávají s údajů uložených v operační systém, databáze, aplikace nebo prostředku. Pokud se shodují, uživatelé úspěšně ověřit a potom může provést akce, které mají autorizaci, během autorizačního procesu. Povolení odkazuje na proces, který určuje, co uživatel může provádět. 

Dalším způsobem zamyslet nad ověřování je vzít v úvahu jako způsob, jak při autorizaci akce, které můžete uživatel provádět, abyste objektů, které uvnitř toto místo (server, databázi nebo aplikace) zadejte prostor, například server, databáze, aplikace nebo prostředku.

## <a name="common-vulnerabilities-in-software"></a>Běžné chyby zabezpečení v softwaru

ASP.NET Core a EF obsahují funkce, které vám pomohou zabezpečit vaše aplikace a zabránit porušení zabezpečení. Následující seznam odkazů přejdete k dokumentaci s podrobnostmi o techniky předejdete nejběžnějších chyb zabezpečení ve službě web apps:

* [Útoky skriptování mezi weby](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Útok prostřednictvím injektáže SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Padělání požadavku posílaného mezi weby (proti útokům CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Otevřete přesměrování útoky](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Existují další chyby zabezpečení, které byste měli vědět. Další informace najdete v části v tomto dokumentu na *ASP.NET zabezpečení dokumentaci*. 

## <a name="aspnet-security-documentation"></a>Dokumentace k technologii ASP.NET zabezpečení

*   [Ověřování](authentication/index.md)
    *   [Úvod do systému Identity](authentication/identity.md)
    *   [Povolení ověřování přes Facebook, Google a další externí zprostředkovatele](authentication/social/index.md)
    * [Konfigurace ověřování systému Windows](authentication/windowsauth.md)
    *   [Potvrzení účtu a obnovení hesla](authentication/accconfirm.md)
    *   [Dvoufaktorové ověřování přes SMS](authentication/2fa.md) 
    *   [Používala ověřování souborů cookie bez Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrace Azure AD do webové aplikace ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Volání rozhraní API webové jádro ASP.NET z grafického subsystému WPF aplikace pomocí služby Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Volání webového rozhraní API webové aplikace ASP.NET Core pomocí Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Webové aplikace ASP.NET Core s Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Zabezpečení aplikací ASP.NET serverem IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorizace](authorization/index.md)
    *   [Úvod](authorization/introduction.md)
    *   [Vytvoření aplikace s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data)
    *   [Jednoduchá autorizace](authorization/simple.md)
    *   [Ověřování založené na rolích](authorization/roles.md)
    *   [Autorizace na základě deklarace identity](authorization/claims.md)
    *   [Autorizace na základě zásad](authorization/policies.md)
    *   [Injektáž závislostí v obslužných rutinách požadavků](authorization/dependencyinjection.md)
    *   [Autorizace na základě prostředků](authorization/resourcebased.md)
    *   [Autorizace na základě zobrazení](authorization/views.md)
    *   [Omezení identity schématem](authorization/limitingidentitybyscheme.md)
*   [Ochrana dat](data-protection/index.md)
    *   [Úvod do ochrany dat](data-protection/introduction.md)
    *   [Začínáme s rozhraními API na ochranu dat](data-protection/using-data-protection.md)
    *   [Rozhraní API příjemců](data-protection/consumer-apis/index.md)
        *   [Přehled rozhraní API příjemců](data-protection/consumer-apis/overview.md)
        *   [Účelové řetězce](data-protection/consumer-apis/purpose-strings.md)
        *   [Hierarchie účelů a víceklientská architektura](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Použití funkce hash u hesla](data-protection/consumer-apis/password-hashing.md)
        *   [Omezení životnosti chráněných datových částí](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Zrušení ochrany datových částí s odvolanými klíči](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Konfigurace](data-protection/configuration/index.md)
        *   [Konfigurace ochrany dat](data-protection/configuration/overview.md)
        *   [Výchozí nastavení](data-protection/configuration/default-settings.md)
        *   [Zásady pro celý počítač](data-protection/configuration/machine-wide-policy.md)
        *   [Scénáře využívající DI](data-protection/configuration/non-di-scenarios.md)
    *   [Rozšiřující rozhraní API](data-protection/extensibility/index.md)
        *   [Rozšiřitelnost základní kryptografie](data-protection/extensibility/core-crypto.md)
        *   [Rozšiřitelnost správy klíčů](data-protection/extensibility/key-management.md)
        *   [Ostatní rozhraní API](data-protection/extensibility/misc-apis.md)
    *   [Implementace](data-protection/implementation/index.md)
        *   [Podrobnosti ověřeného šifrování](data-protection/implementation/authenticated-encryption-details.md)
        *   [Odvozování podklíčů a ověřené šifrování](data-protection/implementation/subkeyderivation.md)
        *   [Kontextová záhlaví](data-protection/implementation/context-headers.md)
        *   [Správa klíčů](data-protection/implementation/key-management.md)
        *   [Zprostředkovatelé úložiště klíčů](data-protection/implementation/key-storage-providers.md)
        *   [Šifrování klíčů v klidovém stavu](data-protection/implementation/key-encryption-at-rest.md)
        *   [Neměnnost klíče a změna nastavení](data-protection/implementation/key-immutability.md)
        *   [Formát ukládání klíčů](data-protection/implementation/key-storage-format.md)
        *   [Zprostředkovatelé dočasné ochrany dat](data-protection/implementation/key-storage-ephemeral.md)
    *   [Kompatibilita](data-protection/compatibility/index.md)
        *   [Sdílení souborů cookie mezi aplikacemi](data-protection/compatibility/cookie-sharing.md)
        *   [Nahrazení <machineKey> v ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Vytvoření aplikace s uživatelskými daty chráněnými autorizací](xref:security/authorization/secure-data)
*   [Bezpečné úložiště tajných částí aplikace při vývoji](app-secrets.md)
*   [Zprostředkovatel konfigurace služby Azure Key Vault](key-vault-configuration.md)
*   [Vynucení SSL](enforcing-ssl.md)
*   [Ochrana proti padělání požadavků](anti-request-forgery.md)
*   [Prevence útoků založených na otevřeném přesměrování](preventing-open-redirects.md)
*   [Obrana proti skriptování mezi weby](cross-site-scripting.md)
*   [Povolení žádostí nepůvodního zdroje (CORS)](cors.md)
