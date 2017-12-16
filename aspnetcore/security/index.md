---
title: "Přehled zabezpečení jádra ASP.NET | Microsoft Docs"
author: rachelappel
description: "Další informace o ověřování, autorizaci a zabezpečení základy v ASP.NET Core"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 3f4df08d6cf5d183735ae4b4ec4f07ed60a9623a
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="aspnet-core-security-overview"></a>Přehled zabezpečení ASP.NET Core

ASP.NET Core umožňuje vývojářům snadno konfigurovat a spravovat zabezpečení pro svoje aplikace. ASP.NET Core obsahuje funkce pro správu ověřování, autorizace, ochrany dat, SSL vynucení, tajné klíče aplikace, žádost proti padělání ochrana a správa CORS. Tyto funkce zabezpečení umožňují vytvářet robustní ještě zabezpečení aplikace ASP.NET Core. 

## <a name="aspnet-core-security-features"></a>Funkce ASP.NET Core zabezpečení

ASP.NET Core poskytuje řadu nástrojů a knihovny k zabezpečení aplikací včetně předdefinované zprostředkovatele Identity, ale můžete použít 3. stran identity služby jako je Facebook, Twitter a LinkedIn. Pomocí ASP.NET Core můžete snadno spravovat tajné klíče aplikace, které představují způsob, jak ukládat a používat důvěrné informace bez nutnosti vystavit v kódu. 

## <a name="authentication-vs-authorization"></a>Ověřování vs. Autorizace

Ověřování je proces, ve kterém uživatel poskytuje pověření, které se pak porovnávají s údajů uložených v operační systém, databáze, aplikace nebo prostředku. Pokud se shodují, uživatelé úspěšně ověřit a potom může provést akce, které mají oprávnění, během autorizačního procesu. Povolení odkazuje na proces, který určuje, co uživatel může provádět. 

Dalším způsobem zamyslet nad ověřování je vzít v úvahu jako způsob, jak při autorizaci akce, které můžete uživatel provádět, abyste objektů, které uvnitř toto místo (server, databázi nebo aplikace) zadejte prostor, například server, databáze, aplikace nebo prostředku.

## <a name="common-vulnerabilities-in-software"></a>Běžné chyby zabezpečení v softwaru

ASP.NET Core a EF obsahují funkce, které vám pomohou zabezpečit vaše aplikace a zabránit porušení zabezpečení. Následující seznam odkazů přejdete k dokumentaci s podrobnostmi o techniky předejdete nejběžnějších chyb zabezpečení ve službě web apps:

* [Útoky skriptováním mezi](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Útok prostřednictvím injektáže SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Padělání požadavku posílaného mezi weby (proti útokům CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Otevřete přesměrování útoky](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Existují další chyby zabezpečení, které byste měli vědět. Další informace najdete v části v tomto dokumentu na *ASP.NET zabezpečení dokumentaci*. 

## <a name="aspnet-security-documentation"></a>Dokumentace k technologii ASP.NET zabezpečení

*   [Ověřování](authentication/index.md)
    *   [Úvod do Identity](authentication/identity.md)
    *   [Povolení ověřování pomocí služby Facebook, Google a dalších externích zprostředkovatelů](authentication/social/index.md)
    * [Konfigurovat ověřování systému Windows](authentication/windowsauth.md)
    *   [Potvrzení účtu a heslo pro obnovení](authentication/accconfirm.md)
    *   [Dvoufaktorové ověřování pomocí serveru SMS](authentication/2fa.md) 
    *   [Pomocí souboru Cookie ověřování bez ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrace Azure AD do webové aplikace ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Volání webového rozhraní API ASP.NET Core z grafického subsystému WPF aplikace pomocí služby Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Volání webového rozhraní API ve webové aplikaci ASP.NET Core pomocí služby Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Webové aplikace ASP.NET Core s Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Zabezpečení aplikací s IdentityServer4 ASP.NET Core](https://identityserver4.readthedocs.io)
*   [Autorizace](authorization/index.md)
    *   [Úvod](authorization/introduction.md)
    *   [Vytvoření aplikace s uživatelskými daty chráněn autorizace](xref:security/authorization/secure-data)
    *   [Jednoduché autorizace](authorization/simple.md)
    *   [Autorizace podle rolí](authorization/roles.md)
    *   [Ověření na základě deklarace identity](authorization/claims.md)
    *   [Autorizace uživatele na základě zásad](authorization/policies.md)
    *   [Vkládání závislostí v obslužné rutiny požadavku](authorization/dependencyinjection.md)
    *   [Ověření na základě prostředků](authorization/resourcebased.md)
    *   [Ověření na základě zobrazení](authorization/views.md)
    *   [Omezení identity s schéma](authorization/limitingidentitybyscheme.md)
*   [Ochrana dat](data-protection/index.md)
    *   [Úvod do ochrany dat](data-protection/introduction.md)
    *   [Začínáme s rozhraními API ochrany dat.](data-protection/using-data-protection.md)
    *   [Rozhraní API příjemce](data-protection/consumer-apis/index.md)
        *   [Přehled rozhraní API příjemce](data-protection/consumer-apis/overview.md)
        *   [Účel řetězce](data-protection/consumer-apis/purpose-strings.md)
        *   [Účel hierarchie a víceklientské prostředí](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Hashování hesel](data-protection/consumer-apis/password-hashing.md)
        *   [Omezení délky trvání chráněných datových částí](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Probíhá rušení ochrany datových částí, které byly odvolány jejíž klíče](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Konfigurace](data-protection/configuration/index.md)
        *   [Konfigurace ochrany dat](data-protection/configuration/overview.md)
        *   [Výchozí nastavení](data-protection/configuration/default-settings.md)
        *   [Široké zásady počítače](data-protection/configuration/machine-wide-policy.md)
        *   [Scénáře využívající bez DI](data-protection/configuration/non-di-scenarios.md)
    *   [Rozšiřitelnost rozhraní API](data-protection/extensibility/index.md)
        *   [Rozšiřitelnost kryptografie jádra](data-protection/extensibility/core-crypto.md)
        *   [Rozšíření správy klíčů](data-protection/extensibility/key-management.md)
        *   [Ostatní rozhraní API](data-protection/extensibility/misc-apis.md)
    *   [Implementace](data-protection/implementation/index.md)
        *   [Podrobnosti o ověřené šifrování](data-protection/implementation/authenticated-encryption-details.md)
        *   [Odvození podklíčů a ověřené šifrování](data-protection/implementation/subkeyderivation.md)
        *   [Kontext hlavičky](data-protection/implementation/context-headers.md)
        *   [Správa klíčů](data-protection/implementation/key-management.md)
        *   [Poskytovatelé úložiště klíčů](data-protection/implementation/key-storage-providers.md)
        *   [Klíče šifrování v klidovém stavu](data-protection/implementation/key-encryption-at-rest.md)
        *   [Klíče neměnitelnosti a změna nastavení](data-protection/implementation/key-immutability.md)
        *   [Formát úložiště klíčů](data-protection/implementation/key-storage-format.md)
        *   [Zprostředkovatelé ochrany dočasných dat](data-protection/implementation/key-storage-ephemeral.md)
    *   [Kompatibilita](data-protection/compatibility/index.md)
        *   [Sdílení souborů cookie mezi aplikacemi](data-protection/compatibility/cookie-sharing.md)
        *   [Nahrazení <machineKey> technologie ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Vytvoření aplikace s uživatelskými daty chráněn autorizace](xref:security/authorization/secure-data)
*   [Bezpečné úložiště tajné klíče aplikace během vývoje](app-secrets.md)
*   [Poskytovatel konfigurace služby Azure Key Vault](key-vault-configuration.md)
*   [Vynucování SSL](enforcing-ssl.md)
*   [Žádost o proti padělání](anti-request-forgery.md)
*   [Prevence útoků otevřete přesměrování](preventing-open-redirects.md)
*   [Brání skriptování mezi servery](cross-site-scripting.md)
*   [Povolení žádostí napříč zdroji (CORS)](cors.md)
