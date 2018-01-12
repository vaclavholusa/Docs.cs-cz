---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "Konfigurace vlastnosti nasazení pro cílové prostředí | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak konfigurovat vlastnosti specifické pro prostředí k nasazení řešení obraťte se na správce ukázka do určité cílové prostředí..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-deployment-properties-for-a-target-environment"></a><span data-ttu-id="efd1c-103">Konfigurace vlastnosti nasazení pro cílové prostředí</span><span class="sxs-lookup"><span data-stu-id="efd1c-103">Configuring Deployment Properties for a Target Environment</span></span>
====================
<span data-ttu-id="efd1c-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="efd1c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="efd1c-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="efd1c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="efd1c-106">Toto téma popisuje postup konfigurace vlastností konkrétní prostředí k nasazení řešení obraťte se na správce ukázka do určité cílové prostředí.</span><span class="sxs-lookup"><span data-stu-id="efd1c-106">This topic describes how to configure environment-specific properties in order to deploy the sample Contact Manager solution to a specific target environment.</span></span>


<span data-ttu-id="efd1c-107">Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení & #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.</span><span class="sxs-lookup"><span data-stu-id="efd1c-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="efd1c-108">Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [Principy procesu sestavení](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="efd1c-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="efd1c-109">V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="efd1c-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="efd1c-110">Přehled procesu</span><span class="sxs-lookup"><span data-stu-id="efd1c-110">Process Overview</span></span>

<span data-ttu-id="efd1c-111">Soubor projektu, které budete používat k vytváření a nasazování řešení obraťte se na správce je rozdělit do dvou fyzických souborů:</span><span class="sxs-lookup"><span data-stu-id="efd1c-111">The project file that you'll use to build and deploy the Contact Manager solution is split into two physical files:</span></span>

- <span data-ttu-id="efd1c-112">Ten, který obsahuje universal sestavení pokyny pro nastavení a ( *Publish.proj* souboru).</span><span class="sxs-lookup"><span data-stu-id="efd1c-112">One that contains universal build settings and instructions (the *Publish.proj* file).</span></span>
- <span data-ttu-id="efd1c-113">Ten, který obsahuje konkrétní prostředí nastavení sestavení (*Env Dev.proj*, *Env Stage.proj*a tak dále).</span><span class="sxs-lookup"><span data-stu-id="efd1c-113">One that contains environment-specific build settings (*Env-Dev.proj*, *Env-Stage.proj*, and so on).</span></span>

<span data-ttu-id="efd1c-114">V okamžiku sestavení souboru projektu příslušné konkrétní prostředí sloučeny do univerzální *Publish.proj* souboru a vytváří kompletní sada pokynů sestavení.</span><span class="sxs-lookup"><span data-stu-id="efd1c-114">At build time, the appropriate environment-specific project file is merged into the universal *Publish.proj* file to form a complete set of build instructions.</span></span> <span data-ttu-id="efd1c-115">Vytvořením nebo přizpůsobení souborů projektu specifické pro prostředí s nastavením, které popisují vlastní scénář nasazení můžete nakonfigurovat nasazení do určité cílové prostředí.</span><span class="sxs-lookup"><span data-stu-id="efd1c-115">You can configure deployment to specific destination environments by creating or customizing environment-specific project files with settings that describe your own deployment scenario.</span></span>

<span data-ttu-id="efd1c-116">Velké množství tyto hodnoty jsou určeny konfiguraci prostředí cílového & #x 2014; zejména, jestli je váš cílový webový server nakonfigurované na používání agenta služba pro nasazení webu (vzdáleného agenta) nebo obslužné rutiny nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="efd1c-116">Lots of these values are determined by how your destination environment is configured&#x2014;in particular, whether your target web server is configured to use the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler.</span></span> <span data-ttu-id="efd1c-117">Další informace o těchto přístupů a pokyny k výběru správný přístup pro vaše vlastní prostředí, najdete v části [výběr práva přístup k nasazení webu](choosing-the-right-approach-to-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-117">For more information on these approaches, and for guidance on choosing the right approach for your own environment, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>

<span data-ttu-id="efd1c-118">[Scénáře využití manažera kontaktujte](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) vyžaduje dva soubory projektu konkrétní prostředí:</span><span class="sxs-lookup"><span data-stu-id="efd1c-118">The [Contact Manager scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) requires two environment-specific project files:</span></span>

- <span data-ttu-id="efd1c-119">Nasazení v testovacím prostředí developer (*Env Dev.proj*).</span><span class="sxs-lookup"><span data-stu-id="efd1c-119">Deployment to a developer test environment (*Env-Dev.proj*).</span></span> <span data-ttu-id="efd1c-120">Testovací prostředí vývojáře je nakonfigurován tak, aby přijímal vzdáleného nasazení pomocí vzdáleného agenta, jak je popsáno v [scénář: Konfigurace testovací prostředí pro nasazení webu](scenario-configuring-a-test-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-120">The developer test environment is configured to accept remote deployments using the remote agent, as described in [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="efd1c-121">Tento soubor musí zajistit vzdáleného agenta adresa koncového bodu, jakož i nastavení pro konkrétní umístění jako koncové body služby a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="efd1c-121">This file needs to provide the remote agent endpoint address as well as location-specific settings like connection strings and service endpoints.</span></span>
- <span data-ttu-id="efd1c-122">Nasazení do pracovního prostředí (*Env Stage.proj*).</span><span class="sxs-lookup"><span data-stu-id="efd1c-122">Deployment to a staging environment (*Env-Stage.proj*).</span></span> <span data-ttu-id="efd1c-123">Je pracovní prostředí nakonfigurováno tak, aby přijímal vzdáleného nasazení pomocí webového nasazení obslužná rutina, jak je popsáno v [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-123">The staging environment is configured to accept remote deployments using the Web Deploy Handler, as described in [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="efd1c-124">Tento soubor vyžaduje k poskytování adresa koncového bodu obslužné rutiny nasazení webu, jakož i umístění konkrétní nastavení, jako je připojovací řetězce a koncové body služby.</span><span class="sxs-lookup"><span data-stu-id="efd1c-124">This file needs to provide the Web Deploy Handler endpoint address as well as location-specific settings like connection strings and service endpoints.</span></span>

<span data-ttu-id="efd1c-125">Je důležité si uvědomit, že nebudou mít vliv nastavení, které nakonfigurujete v souboru projektu konkrétní prostředí obsah webu balíček samotné & #x 2014; místo toho řízení nasazení balíčku a jaké hodnoty parametrů jsou uvedeny, kdy balíček je rozbalena.</span><span class="sxs-lookup"><span data-stu-id="efd1c-125">It's important to note that the settings you configure in the environment-specific project file don't affect the contents of the web package itself&#x2014;instead, they control how the package is deployed and what parameter values are provided when the package is extracted.</span></span> <span data-ttu-id="efd1c-126">Balíček webu do provozního prostředí při importu ručně, jak je popsáno v [scénář: Konfigurace produkčním prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md) a [ručně instalaci webových balíčků](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), Nezáleží na nastavení, které jste v souboru projektu konkrétní prostředí použili při vygenerování balíčku.</span><span class="sxs-lookup"><span data-stu-id="efd1c-126">You're importing the web package into the production environment manually, as described in [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md) and [Manually Installing Web Packages](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), so it doesn't matter what settings you used in the environment-specific project file when you generated the package.</span></span> <span data-ttu-id="efd1c-127">Správce Internetové informační služby (IIS) zobrazí výzvu k zadání veškeré parametrizované hodnoty jako koncové body služby a připojovací řetězce při importu balíčku.</span><span class="sxs-lookup"><span data-stu-id="efd1c-127">Internet Information Services (IIS) Manager will prompt you for any parameterized values, like connection strings and service endpoints, when you import the package.</span></span>

<span data-ttu-id="efd1c-128">K nasazení řešení. Obraťte se na správce cílového prostředí, můžete přizpůsobit tento soubor nebo ho použít jako šablonu a vytvořte svůj vlastní soubor.</span><span class="sxs-lookup"><span data-stu-id="efd1c-128">To deploy the Contact Manager solution to your own target environment, you can either customize this file or use it as a template and create your own file.</span></span>

<span data-ttu-id="efd1c-129">**Ke konfiguraci nastavení nasazení konkrétní prostředí pro řešení obraťte se na správce**</span><span class="sxs-lookup"><span data-stu-id="efd1c-129">**To configure environment-specific deployment settings for the Contact Manager solution**</span></span>

1. <span data-ttu-id="efd1c-130">Otevřete řešení ContactManager WCF v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="efd1c-130">Open the ContactManager-WCF solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="efd1c-131">V **Průzkumníku řešení** okno, rozbalte **publikovat** složky, rozbalte **EnvConfig** složku a pak dvojitým kliknutím **Env-Dev.proj**.</span><span class="sxs-lookup"><span data-stu-id="efd1c-131">In the **Solution Explorer** window, expand the **Publish** folder, expand the **EnvConfig** folder, and then double-click **Env-Dev.proj**.</span></span>

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. <span data-ttu-id="efd1c-132">Nahraďte hodnoty vlastností v *Env Dev.proj* soubor s správné hodnoty pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="efd1c-132">Replace the property values in the *Env-Dev.proj* file with the correct values for your own test environment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="efd1c-133">Tabulka, která následuje po tomto postupu poskytuje další informace o každé z těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="efd1c-133">The table that follows this procedure provides more information on each of these properties.</span></span>
4. <span data-ttu-id="efd1c-134">Uložte si práci a pak zavřete *Env Dev.proj* souboru.</span><span class="sxs-lookup"><span data-stu-id="efd1c-134">Save your work, and then close the *Env-Dev.proj* file.</span></span>

## <a name="choosing-the-right-deployment-properties"></a><span data-ttu-id="efd1c-135">Výběr vlastností správné nasazení</span><span class="sxs-lookup"><span data-stu-id="efd1c-135">Choosing the Right Deployment Properties</span></span>

<span data-ttu-id="efd1c-136">Tato tabulka popisuje účel každé vlastnosti v ukázkovém souboru projektu specifické pro prostředí, *Env Dev.proj*a obsahuje některé pokyny k hodnoty by měl poskytovat.</span><span class="sxs-lookup"><span data-stu-id="efd1c-136">This table describes the purpose of each property in the sample environment-specific project file, *Env-Dev.proj*, and provides some guidance on the values you should provide.</span></span>

| <span data-ttu-id="efd1c-137">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="efd1c-137">Property Name</span></span> | <span data-ttu-id="efd1c-138">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="efd1c-138">Details</span></span> |
| --- | --- |
| <span data-ttu-id="efd1c-139">**MSDeployComputerName** název cílový webový server nebo službu koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="efd1c-139">**MSDeployComputerName** The name of the destination web server or service endpoint.</span></span> | <span data-ttu-id="efd1c-140">Pokud nasazujete službu vzdáleného agenta na cílový webový server, můžete zadat název cílového počítače (například **TESTWEB1** nebo **TESTWEB1.fabrikam.net**), nebo můžete zadat vzdálených koncový bod agenta (například `http://TESTWEB1/MSDEPLOYAGENTSERVICE`).</span><span class="sxs-lookup"><span data-stu-id="efd1c-140">If you're deploying to the remote agent service on the destination web server, you can specify the target computer name (for example, **TESTWEB1** or **TESTWEB1.fabrikam.net**), or you can specify the remote agent endpoint (for example, `http://TESTWEB1/MSDEPLOYAGENTSERVICE`).</span></span> <span data-ttu-id="efd1c-141">Nasazení funguje stejným způsobem jako v každém případu.</span><span class="sxs-lookup"><span data-stu-id="efd1c-141">The deployment works the same way in each case.</span></span> <span data-ttu-id="efd1c-142">Pokud nasazujete do obslužné rutiny webu nasadit na cílový webový server, měli byste zadat koncový bod služby a obsahovat název webu služby IIS jako parametr řetězce dotazu (například `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`).</span><span class="sxs-lookup"><span data-stu-id="efd1c-142">If you're deploying to the Web Deploy Handler on the destination web server, you should specify the service endpoint and include the name of the IIS website as a query string parameter (for example, `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`).</span></span> |
| <span data-ttu-id="efd1c-143">**MSDeployAuth** metodu nasazení webu by měl použít k ověření ke vzdálenému počítači.</span><span class="sxs-lookup"><span data-stu-id="efd1c-143">**MSDeployAuth** The method that Web Deploy should use to authenticate to the remote computer.</span></span> | <span data-ttu-id="efd1c-144">Musí být nastavena na **NTLM** nebo **základní**.</span><span class="sxs-lookup"><span data-stu-id="efd1c-144">This should be set to **NTLM** or **Basic**.</span></span> <span data-ttu-id="efd1c-145">Obvykle použijete **NTLM** Pokud nasazujete ve službě vzdáleného agenta a **základní** Pokud nasazujete do obslužné rutiny nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="efd1c-145">Typically, you'll use **NTLM** if you're deploying to the remote agent service and **Basic** if you're deploying to the Web Deploy Handler.</span></span> <span data-ttu-id="efd1c-146">Pokud používáte základní ověřování, budete také muset zadat uživatelské jméno a heslo, které by měla zosobňovat nástroj nasazení webu služby IIS (Web Deploy) Chcete-li provést nasazení.</span><span class="sxs-lookup"><span data-stu-id="efd1c-146">If you use basic authentication, you also need to specify the user name and password that the IIS Web Deployment Tool (Web Deploy) should impersonate in order to perform the deployment.</span></span> <span data-ttu-id="efd1c-147">V tomto příkladu jsou tyto hodnoty zajišťováno prostřednictvím **MSDeployUsername** a **MSDeployPassword** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="efd1c-147">In this example, these values are provided through the **MSDeployUsername** and **MSDeployPassword** properties.</span></span> <span data-ttu-id="efd1c-148">Pokud používáte ověřování NTLM, můžete vynechat tyto vlastnosti nebo zůstat prázdné.</span><span class="sxs-lookup"><span data-stu-id="efd1c-148">If you use NTLM authentication, you can omit these properties or leave them blank.</span></span> |
| <span data-ttu-id="efd1c-149">**MSDeployUsername** Pokud používáte základní ověřování, Web Deploy použije tento účet na vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="efd1c-149">**MSDeployUsername** If you use basic authentication, Web Deploy will use this account on the remote computer.</span></span> | <span data-ttu-id="efd1c-150">To by měl být ve formátu *domény*\*uživatelské jméno * (například **FABRIKAM\matt**).</span><span class="sxs-lookup"><span data-stu-id="efd1c-150">This should take the form *DOMAIN*\*username* (for example, **FABRIKAM\matt**).</span></span> <span data-ttu-id="efd1c-151">Tato hodnota se používá pouze pokud zadáte základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="efd1c-151">This value is only used if you specify basic authentication.</span></span> <span data-ttu-id="efd1c-152">Pokud používáte ověřování NTLM, můžete vynechat vlastnost.</span><span class="sxs-lookup"><span data-stu-id="efd1c-152">If you use NTLM authentication, the property can be omitted.</span></span> <span data-ttu-id="efd1c-153">Pokud je zadána hodnota, se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="efd1c-153">If a value is supplied, it will be ignored.</span></span> |
| <span data-ttu-id="efd1c-154">**MSDeployPassword** Pokud používáte základní ověřování, Web Deploy použije toto heslo na vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="efd1c-154">**MSDeployPassword** If you use basic authentication, Web Deploy will use this password on the remote computer.</span></span> | <span data-ttu-id="efd1c-155">Toto je heslo pro uživatelský účet zadaný v **MSDeployUsername** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="efd1c-155">This is the password for the user account you specified in the **MSDeployUsername** property.</span></span> <span data-ttu-id="efd1c-156">Tato hodnota se používá pouze pokud zadáte základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="efd1c-156">This value is only used if you specify basic authentication.</span></span> <span data-ttu-id="efd1c-157">Pokud používáte ověřování NTLM, můžete vynechat vlastnost.</span><span class="sxs-lookup"><span data-stu-id="efd1c-157">If you use NTLM authentication, the property can be omitted.</span></span> <span data-ttu-id="efd1c-158">Pokud je zadána hodnota, se budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="efd1c-158">If a value is supplied, it will be ignored.</span></span> |
| <span data-ttu-id="efd1c-159">**ContactManagerIisPath** ve službě IIS cestu, na který chcete nasadit aplikace MVC obraťte se na správce.</span><span class="sxs-lookup"><span data-stu-id="efd1c-159">**ContactManagerIisPath** The IIS path on which you want to deploy the Contact Manager MVC application.</span></span> | <span data-ttu-id="efd1c-160">Měl by to být cesta zobrazí se ve Správci služby IIS ve tvaru [*název webu služby IIS*] nebo [*webové**název aplikace*].</span><span class="sxs-lookup"><span data-stu-id="efd1c-160">This should be the path as it appears in IIS Manager, in the form [*IIS website name*]/[*web**application name*].</span></span> <span data-ttu-id="efd1c-161">Mějte na paměti, že musí existovat před nasazením aplikace na web služby IIS.</span><span class="sxs-lookup"><span data-stu-id="efd1c-161">Remember that the IIS website needs to exist before you deploy your application.</span></span> <span data-ttu-id="efd1c-162">Například pokud jste vytvořili webu IIS s názvem DemoSite, budete moci zadat cestu ke službě IIS pro aplikace MVC jako DemoSite nebo ContactManager.</span><span class="sxs-lookup"><span data-stu-id="efd1c-162">For example, if you've created an IIS website named DemoSite, you could specify the IIS path for the MVC application as DemoSite/ContactManager.</span></span> |
| <span data-ttu-id="efd1c-163">**ContactManagerServiceIisPath** ve službě IIS cestu, na který chcete nasadit službu WCF obraťte se na správce.</span><span class="sxs-lookup"><span data-stu-id="efd1c-163">**ContactManagerServiceIisPath** The IIS path on which you want to deploy the Contact Manager WCF service.</span></span> | <span data-ttu-id="efd1c-164">Pokud jste vytvořili s názvem DemoSite webu IIS, můžete například zadat cestu ke službě IIS pro službu WCF jako **DemoSite/ContactManagerService**.</span><span class="sxs-lookup"><span data-stu-id="efd1c-164">For example, if you've created an IIS website named DemoSite, you could specify the IIS path for the WCF service as **DemoSite/ContactManagerService**.</span></span> |
| <span data-ttu-id="efd1c-165">**ContactManagerTargetUrl** adresa URL, na které jsou dostupné služby WCF.</span><span class="sxs-lookup"><span data-stu-id="efd1c-165">**ContactManagerTargetUrl** The URL at which the WCF service can be reached.</span></span> | <span data-ttu-id="efd1c-166">To bude mít tvar [*adresy URL kořenového adresáře webu služby IIS*] nebo [*název aplikace služby*] nebo [*koncový bod služby*].</span><span class="sxs-lookup"><span data-stu-id="efd1c-166">This will take the form [*IIS website root URL*]/[*service application name*]/[*service endpoint*].</span></span> <span data-ttu-id="efd1c-167">Například pokud vytvoříte webu IIS na portu 85 adresa URL by mít tvar `http://localhost:85/ContactManagerService/ContactService.svc`.</span><span class="sxs-lookup"><span data-stu-id="efd1c-167">For example, if you've created an IIS website on port 85, the URL would take the form `http://localhost:85/ContactManagerService/ContactService.svc`.</span></span> <span data-ttu-id="efd1c-168">Mějte na paměti, že aplikace MVC a služby WCF jsou nasazeny na stejný server.</span><span class="sxs-lookup"><span data-stu-id="efd1c-168">Remember that the MVC application and the WCF service are deployed to the same server.</span></span> <span data-ttu-id="efd1c-169">V důsledku toho tato adresa URL přistupuje vždy jen z počítače, na kterém je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="efd1c-169">As a result, this URL is only ever accessed from the machine on which it's installed.</span></span> <span data-ttu-id="efd1c-170">Z toho důvodu je lepší použít místního hostitele nebo IP adresu, nikoli název počítače nebo hlavičku hostitele v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="efd1c-170">Because of this, it's better to use localhost or the IP address, rather than the machine name or a host header, in the URL.</span></span> <span data-ttu-id="efd1c-171">Pokud použijete název počítače nebo hlavičku hostitele, [zpětné smyčky kontrola](https://go.microsoft.com/?linkid=9805131) funkce zabezpečení ve službě IIS může blokovat adresy URL a vrátit **HTTP 401.1 – Neautorizováno** chyby.</span><span class="sxs-lookup"><span data-stu-id="efd1c-171">If you use the machine name or a host header, the [loopback check](https://go.microsoft.com/?linkid=9805131) security feature in IIS may block the URL and return an **HTTP 401.1 - Unauthorized** error.</span></span> |
| <span data-ttu-id="efd1c-172">**CmDatabaseConnectionString** připojovací řetězec pro databázový server.</span><span class="sxs-lookup"><span data-stu-id="efd1c-172">**CmDatabaseConnectionString** The connection string for the database server.</span></span> | <span data-ttu-id="efd1c-173">Připojovací řetězec Určuje obou pověření, které budou používat VSDBCMD kontaktovat server, databáze a vytvořit databázi a přihlašovací údaje, které budou používat fondu aplikací webového serveru a obraťte se na serveru databáze komunikovat s databází.</span><span class="sxs-lookup"><span data-stu-id="efd1c-173">The connection string determines both the credentials that VSDBCMD will use to contact the database server and create the database and the credentials that the web server application pool will use to contact the database server and interact with the database.</span></span> <span data-ttu-id="efd1c-174">V podstatě máte dvě možnosti sem.</span><span class="sxs-lookup"><span data-stu-id="efd1c-174">Essentially you have two choices here.</span></span> <span data-ttu-id="efd1c-175">Můžete zadat **integrované zabezpečení = true**, v takovém případě se používá integrované ověřování systému Windows: **zdroj dat = TESTDB1; Integrated Security = true** v tomto případě databáze se vytvoří pomocí přihlašovací údaje uživatele, který spouští spustitelný soubor VSDBCMD a aplikace bude přístup k databázi pomocí identity účet počítače webového serveru.</span><span class="sxs-lookup"><span data-stu-id="efd1c-175">You can specify **Integrated Security=true**, in which case integrated Windows authentication is used: **Data Source=TESTDB1;Integrated Security=true** In this case, the database will be created using the credentials of the user who runs the VSDBCMD executable, and the application will access the database using the identity of the web server machine account.</span></span> <span data-ttu-id="efd1c-176">Alternativně můžete zadat uživatelské jméno a heslo účtu systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="efd1c-176">Alternatively, you can specify the user name and password of a SQL Server account.</span></span> <span data-ttu-id="efd1c-177">V takovém případě jsou pověření systému SQL Server používá VSDBCMD k vytvoření databáze a také fondu aplikací pracovat s databází: **zdroj dat = TESTDB1; Id uživatele = ASqlUser; Heslo = Pa$ $w0rd** názorné postupy v tomto tématu předpokládat, že budete používat integrované ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="efd1c-177">In this case, the SQL Server credentials are used both by VSDBCMD to create the database and by the application pool to interact with the database: **Data Source=TESTDB1;User Id=ASqlUser; Password=Pa$$w0rd** The walkthroughs in this topic assume that you'll use integrated Windows authentication.</span></span> |
| <span data-ttu-id="efd1c-178">**CmTargetDatabase** název chcete poskytnout databázi vytvoříte na serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="efd1c-178">**CmTargetDatabase** The name you want to give the database you'll create on the database server.</span></span> | <span data-ttu-id="efd1c-179">Hodnota, kterou tady zadáte, se přidá do příkaz VSDBCMD jako parametr.</span><span class="sxs-lookup"><span data-stu-id="efd1c-179">The value you provide here is added to the VSDBCMD command as a parameter.</span></span> <span data-ttu-id="efd1c-180">Slouží také k sestavení úplný připojovací řetězec, který fond aplikací na webovém serveru můžete používat k interakci s databází.</span><span class="sxs-lookup"><span data-stu-id="efd1c-180">It's also used to build a full connection string that the application pool on the web server can use to interact with the database.</span></span> |
  

<span data-ttu-id="efd1c-181">Tyto příklady ukazují, jak můžete nakonfigurovat tyto vlastnosti pro konkrétní nasazení scénáře.</span><span class="sxs-lookup"><span data-stu-id="efd1c-181">These examples show how you might configure these properties for specific deployment scenarios.</span></span>

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a><span data-ttu-id="efd1c-182">Příklad 1 & #x 2014; nasazení pro službu vzdáleného agenta</span><span class="sxs-lookup"><span data-stu-id="efd1c-182">Example 1&#x2014;Deployment to the Remote Agent Service</span></span>

<span data-ttu-id="efd1c-183">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="efd1c-183">In this example:</span></span>

- <span data-ttu-id="efd1c-184">Nasazujete do služby vzdáleného agenta na TESTWEB1.</span><span class="sxs-lookup"><span data-stu-id="efd1c-184">You're deploying to the remote agent service on TESTWEB1.</span></span>
- <span data-ttu-id="efd1c-185">Jste instruující, nasazení webu použít ověřování NTLM.</span><span class="sxs-lookup"><span data-stu-id="efd1c-185">You're instructing Web Deploy to use NTLM authentication.</span></span> <span data-ttu-id="efd1c-186">Nasazení webu se spustí pomocí přihlašovacích údajů, které jste použili k vyvolání Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="efd1c-186">Web Deploy will run using the credentials you used to invoke the Microsoft Build Engine (MSBuild).</span></span>
- <span data-ttu-id="efd1c-187">Integrované ověřování používáte k nasazení **ContactManager** databáze TESTDB1.</span><span class="sxs-lookup"><span data-stu-id="efd1c-187">You're using integrated authentication to deploy the **ContactManager** database to TESTDB1.</span></span> <span data-ttu-id="efd1c-188">Databáze se nasadí pomocí přihlašovacích údajů, které jste použili k vyvolání nástroje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="efd1c-188">The database will be deployed using the credentials you used to invoke MSBuild.</span></span>


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a><span data-ttu-id="efd1c-189">Příklad 2 & #x 2014; nasazení webu nasazení obslužné rutiny Endpoint</span><span class="sxs-lookup"><span data-stu-id="efd1c-189">Example 2&#x2014;Deployment to the Web Deploy Handler Endpoint</span></span>

<span data-ttu-id="efd1c-190">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="efd1c-190">In this example:</span></span>

- <span data-ttu-id="efd1c-191">Nasazujete do koncového bodu služby obslužné rutiny nasazení webu na STAGEWEB1.</span><span class="sxs-lookup"><span data-stu-id="efd1c-191">You're deploying to the Web Deploy Handler service endpoint on STAGEWEB1.</span></span>
- <span data-ttu-id="efd1c-192">Jste instruující, Web Deploy používat základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="efd1c-192">You're instructing Web Deploy to use basic authentication.</span></span>
- <span data-ttu-id="efd1c-193">Určujete, že nasazení webu by měl zosobnit účet FABRIKAM\stagingdeployer na vzdáleném počítači.</span><span class="sxs-lookup"><span data-stu-id="efd1c-193">You're specifying that Web Deploy should impersonate the FABRIKAM\stagingdeployer account on the remote computer.</span></span>
- <span data-ttu-id="efd1c-194">Ověřování systému SQL Server používáte k nasazení **ContactManager** databáze STAGEDB1.</span><span class="sxs-lookup"><span data-stu-id="efd1c-194">You're using SQL Server authentication to deploy the **ContactManager** database to STAGEDB1.</span></span>


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a><span data-ttu-id="efd1c-195">Závěr</span><span class="sxs-lookup"><span data-stu-id="efd1c-195">Conclusion</span></span>

<span data-ttu-id="efd1c-196">Soubory projektu jsou nyní plně konfigurována pro sestavení a nasazení řešení. Obraťte se na správce na jeden nebo více cílové prostředí.</span><span class="sxs-lookup"><span data-stu-id="efd1c-196">At this point, your project files are fully configured to build and deploy the Contact Manager solution to one or more destination environments.</span></span>

<span data-ttu-id="efd1c-197">K používání těchto souborů projektu jako součást procesu nasazení krokování, opakovatelných, je nutné provést *Publish.proj* souborů pomocí nástroje MSBuild a předat umístění souboru projektu konkrétní prostředí jako parametr.</span><span class="sxs-lookup"><span data-stu-id="efd1c-197">To use these project files as part of a single-step, repeatable deployment process, you need to execute the *Publish.proj* file using MSBuild and pass in the location of the environment-specific project file as a parameter.</span></span> <span data-ttu-id="efd1c-198">Můžete provést různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="efd1c-198">You can do this in various ways:</span></span>

- <span data-ttu-id="efd1c-199">Přehled nástroje MSBuild a úvod do vlastních projektů soubory, najdete v části [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-199">For an overview of MSBuild and an introduction to custom project files, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="efd1c-200">Informace o tom, jak pomocí nástroje MSBuild příkazu, který provede soubory vlastní projektu formulovali najdete v tématu [nasazení webových balíčků](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-200">For information on how to formulate an MSBuild command that executes your custom project files, see [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>
- <span data-ttu-id="efd1c-201">Informace o tom, jak začlenit vaší MSBuild příkazy do příkazového řádku pro jeden krok, opakované nasazení najdete v tématu [vytváření a spouštění soubor příkazů nasazení](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-201">For information on how to incorporate your MSBuild commands into a command file for single-step, repeatable deployments, see [Creating and Running a Deployment Command File](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).</span></span>
- <span data-ttu-id="efd1c-202">Informace o tom, jak provést soubory vlastní projektu z nástroje týmové sestavení najdete v tématu [vytváření definici sestavení toto nasazení podporuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="efd1c-202">For information on how to execute your custom project files from Team Build, see [Creating a Build Definition that Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="efd1c-203">Předchozí</span><span class="sxs-lookup"><span data-stu-id="efd1c-203">Previous</span></span>](creating-a-server-farm-with-the-web-farm-framework.md)