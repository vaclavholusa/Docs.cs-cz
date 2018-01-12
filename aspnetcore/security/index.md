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
ms.openlocfilehash: f6a1f32c1edd098b0782fd066d8e32f09952a9b7
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-security-overview"></a><span data-ttu-id="e6146-103">Přehled zabezpečení ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6146-103">ASP.NET Core Security Overview</span></span>

<span data-ttu-id="e6146-104">ASP.NET Core umožňuje vývojářům snadno konfigurovat a spravovat zabezpečení pro svoje aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6146-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="e6146-105">ASP.NET Core obsahuje funkce pro správu ověřování, autorizace, ochrany dat, SSL vynucení, tajné klíče aplikace, žádost proti padělání ochrana a správa CORS.</span><span class="sxs-lookup"><span data-stu-id="e6146-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="e6146-106">Tyto funkce zabezpečení umožňují vytvářet robustní ještě zabezpečení aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6146-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span> 

## <a name="aspnet-core-security-features"></a><span data-ttu-id="e6146-107">Funkce ASP.NET Core zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e6146-107">ASP.NET Core security features</span></span>

<span data-ttu-id="e6146-108">ASP.NET Core poskytuje řadu nástrojů a knihovny k zabezpečení aplikací včetně předdefinované zprostředkovatele Identity, ale můžete použít 3. stran identity služby jako je Facebook, Twitter a LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="e6146-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="e6146-109">Pomocí ASP.NET Core můžete snadno spravovat tajné klíče aplikace, které představují způsob, jak ukládat a používat důvěrné informace bez nutnosti vystavit v kódu.</span><span class="sxs-lookup"><span data-stu-id="e6146-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span> 

## <a name="authentication-vs-authorization"></a><span data-ttu-id="e6146-110">Ověřování vs. Autorizace</span><span class="sxs-lookup"><span data-stu-id="e6146-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="e6146-111">Ověřování je proces, ve kterém uživatel poskytuje pověření, které se pak porovnávají s údajů uložených v operační systém, databáze, aplikace nebo prostředku.</span><span class="sxs-lookup"><span data-stu-id="e6146-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="e6146-112">Pokud se shodují, uživatelé úspěšně ověřit a potom může provést akce, které mají oprávnění, během autorizačního procesu.</span><span class="sxs-lookup"><span data-stu-id="e6146-112">If they match, users authenticate successfully, and can then perform actions that they are authorized for, during an authorization process.</span></span> <span data-ttu-id="e6146-113">Povolení odkazuje na proces, který určuje, co uživatel může provádět.</span><span class="sxs-lookup"><span data-stu-id="e6146-113">The authorization refers to the process that determines what a user is allowed to do.</span></span> 

<span data-ttu-id="e6146-114">Dalším způsobem zamyslet nad ověřování je vzít v úvahu jako způsob, jak při autorizaci akce, které můžete uživatel provádět, abyste objektů, které uvnitř toto místo (server, databázi nebo aplikace) zadejte prostor, například server, databáze, aplikace nebo prostředku.</span><span class="sxs-lookup"><span data-stu-id="e6146-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="e6146-115">Běžné chyby zabezpečení v softwaru</span><span class="sxs-lookup"><span data-stu-id="e6146-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="e6146-116">ASP.NET Core a EF obsahují funkce, které vám pomohou zabezpečit vaše aplikace a zabránit porušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e6146-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="e6146-117">Následující seznam odkazů přejdete k dokumentaci s podrobnostmi o techniky předejdete nejběžnějších chyb zabezpečení ve službě web apps:</span><span class="sxs-lookup"><span data-stu-id="e6146-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="e6146-118">Útoky skriptování mezi weby</span><span class="sxs-lookup"><span data-stu-id="e6146-118">Cross-site scripting attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [<span data-ttu-id="e6146-119">Útok prostřednictvím injektáže SQL</span><span class="sxs-lookup"><span data-stu-id="e6146-119">SQL injection attacks</span></span>](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [<span data-ttu-id="e6146-120">Padělání požadavku posílaného mezi weby (proti útokům CSRF)</span><span class="sxs-lookup"><span data-stu-id="e6146-120">Cross-Site Request Forgery (CSRF)</span></span>](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [<span data-ttu-id="e6146-121">Otevřete přesměrování útoky</span><span class="sxs-lookup"><span data-stu-id="e6146-121">Open redirect attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

<span data-ttu-id="e6146-122">Existují další chyby zabezpečení, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="e6146-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="e6146-123">Další informace najdete v části v tomto dokumentu na *ASP.NET zabezpečení dokumentaci*.</span><span class="sxs-lookup"><span data-stu-id="e6146-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span> 

## <a name="aspnet-security-documentation"></a><span data-ttu-id="e6146-124">Dokumentace k technologii ASP.NET zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e6146-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="e6146-125">Ověřování</span><span class="sxs-lookup"><span data-stu-id="e6146-125">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="e6146-126">Úvod do systému Identity</span><span class="sxs-lookup"><span data-stu-id="e6146-126">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="e6146-127">Povolit ověřování pomocí Facebook, Google a dalších externích zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="e6146-127">Enable authentication using Facebook, Google, and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="e6146-128">Konfigurace ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="e6146-128">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="e6146-129">Potvrzení účtu a heslo pro obnovení</span><span class="sxs-lookup"><span data-stu-id="e6146-129">Account confirmation and password recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="e6146-130">Dvoufaktorové ověřování přes SMS</span><span class="sxs-lookup"><span data-stu-id="e6146-130">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="e6146-131">Používala ověřování souborů cookie bez Identity</span><span class="sxs-lookup"><span data-stu-id="e6146-131">Use cookie authentication without Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="e6146-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e6146-132">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="e6146-133">Integrace Azure AD do webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6146-133">Integrate Azure AD into an ASP.NET Core web app</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="e6146-134">Volání rozhraní API webové jádro ASP.NET z grafického subsystému WPF aplikace pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6146-134">Call an ASP.NET Core Web API from a WPF app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="e6146-135">Volání webového rozhraní API ve webové aplikaci ASP.NET Core pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6146-135">Call a Web API in an ASP.NET Core web app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="e6146-136">Webové aplikace ASP.NET Core s Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="e6146-136">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="e6146-137">Zabezpečení aplikací s IdentityServer4 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6146-137">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="e6146-138">Autorizace</span><span class="sxs-lookup"><span data-stu-id="e6146-138">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="e6146-139">Úvod</span><span class="sxs-lookup"><span data-stu-id="e6146-139">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="e6146-140">Vytvoření aplikace s uživatelskými daty chráněnými autorizací</span><span class="sxs-lookup"><span data-stu-id="e6146-140">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="e6146-141">Jednoduché autorizace</span><span class="sxs-lookup"><span data-stu-id="e6146-141">Simple authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="e6146-142">Ověřování na základě rolí</span><span class="sxs-lookup"><span data-stu-id="e6146-142">Role-based authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="e6146-143">Ověření na základě deklarace identity</span><span class="sxs-lookup"><span data-stu-id="e6146-143">Claims-based authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="e6146-144">Na základě zásad autorizace</span><span class="sxs-lookup"><span data-stu-id="e6146-144">Policy-based authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="e6146-145">Vkládání závislostí v obslužné rutiny požadavku</span><span class="sxs-lookup"><span data-stu-id="e6146-145">Dependency injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="e6146-146">Autorizace na základě prostředků</span><span class="sxs-lookup"><span data-stu-id="e6146-146">Resource-based authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="e6146-147">Autorizace na základě zobrazení</span><span class="sxs-lookup"><span data-stu-id="e6146-147">View-based authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="e6146-148">Limit identity podle schématu</span><span class="sxs-lookup"><span data-stu-id="e6146-148">Limit identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="e6146-149">Ochrana dat</span><span class="sxs-lookup"><span data-stu-id="e6146-149">Data protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="e6146-150">Úvod do ochrany dat</span><span class="sxs-lookup"><span data-stu-id="e6146-150">Introduction to data protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="e6146-151">Začínáme s rozhraními API ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="e6146-151">Get started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="e6146-152">Rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="e6146-152">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="e6146-153">Přehled rozhraní API příjemců</span><span class="sxs-lookup"><span data-stu-id="e6146-153">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="e6146-154">Účel řetězce</span><span class="sxs-lookup"><span data-stu-id="e6146-154">Purpose strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="e6146-155">Hierarchie účelů a víceklientská architektura</span><span class="sxs-lookup"><span data-stu-id="e6146-155">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="e6146-156">Hashování hesel</span><span class="sxs-lookup"><span data-stu-id="e6146-156">Password hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="e6146-157">Omezení doby trvání chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="e6146-157">Limit the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="e6146-158">Zrušte ochranu datových částí, které byly odvolány jejíž klíče</span><span class="sxs-lookup"><span data-stu-id="e6146-158">Unprotect payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="e6146-159">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="e6146-159">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="e6146-160">Konfigurovat ochranu dat</span><span class="sxs-lookup"><span data-stu-id="e6146-160">Configure data protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="e6146-161">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="e6146-161">Default settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="e6146-162">Zásady pro celý počítač</span><span class="sxs-lookup"><span data-stu-id="e6146-162">Machine-wide policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="e6146-163">Scénáře využívající DI</span><span class="sxs-lookup"><span data-stu-id="e6146-163">Non DI-aware scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="e6146-164">Rozšiřující rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e6146-164">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="e6146-165">Rozšiřitelnost základní kryptografie</span><span class="sxs-lookup"><span data-stu-id="e6146-165">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="e6146-166">Rozšiřitelnost správy klíčů</span><span class="sxs-lookup"><span data-stu-id="e6146-166">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="e6146-167">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e6146-167">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="e6146-168">Implementace</span><span class="sxs-lookup"><span data-stu-id="e6146-168">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="e6146-169">Podrobnosti ověřeného šifrování</span><span class="sxs-lookup"><span data-stu-id="e6146-169">Authenticated encryption details</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="e6146-170">Odvození podklíčů a ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="e6146-170">Subkey derivation and authenticated encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="e6146-171">Kontextová záhlaví</span><span class="sxs-lookup"><span data-stu-id="e6146-171">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="e6146-172">Správa klíčů</span><span class="sxs-lookup"><span data-stu-id="e6146-172">Key management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="e6146-173">Poskytovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="e6146-173">Key storage providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="e6146-174">Klíče šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="e6146-174">Key encryption at rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="e6146-175">Klíče neměnitelnosti a změna nastavení</span><span class="sxs-lookup"><span data-stu-id="e6146-175">Key immutability and changing settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="e6146-176">Formát úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="e6146-176">Key storage format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="e6146-177">Zprostředkovatelé dočasné ochrany dat</span><span class="sxs-lookup"><span data-stu-id="e6146-177">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="e6146-178">Kompatibilita</span><span class="sxs-lookup"><span data-stu-id="e6146-178">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="e6146-179">Soubory cookie sdílení mezi aplikacemi</span><span class="sxs-lookup"><span data-stu-id="e6146-179">Share cookies between apps</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="e6146-180">Nahraďte <machineKey> technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6146-180">Replace <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="e6146-181">Vytvoření aplikace s uživatelskými daty chráněnými autorizací</span><span class="sxs-lookup"><span data-stu-id="e6146-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="e6146-182">Bezpečné úložiště tajných částí aplikace při vývoji</span><span class="sxs-lookup"><span data-stu-id="e6146-182">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="e6146-183">Zprostředkovatel konfigurace služby Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e6146-183">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="e6146-184">Vynutit SSL</span><span class="sxs-lookup"><span data-stu-id="e6146-184">Enforce SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="e6146-185">Ochrana proti padělání požadavků</span><span class="sxs-lookup"><span data-stu-id="e6146-185">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="e6146-186">Zabránit útokům otevřete přesměrování</span><span class="sxs-lookup"><span data-stu-id="e6146-186">Prevent open redirect attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="e6146-187">Zabránit skriptování mezi servery</span><span class="sxs-lookup"><span data-stu-id="e6146-187">Prevent Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="e6146-188">Povolení žádostí napříč zdroji (CORS)</span><span class="sxs-lookup"><span data-stu-id="e6146-188">Enable Cross-Origin Requests (CORS)</span></span>](cors.md)
