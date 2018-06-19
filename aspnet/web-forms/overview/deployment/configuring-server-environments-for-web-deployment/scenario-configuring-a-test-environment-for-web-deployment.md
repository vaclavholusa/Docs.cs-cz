---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scénář: Konfigurace testovacího prostředí pro nasazení webu | Microsoft Docs'
author: jrjlee
description: Toto téma popisuje scénář nasazení typické web pro vývojáře nebo testovací prostředí a popisuje úlohy, které potřebujete k dokončení, aby bylo možné nastavit serveru...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879852"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="d1956-103">Scénář: Konfigurace testovacího prostředí pro nasazení webu</span><span class="sxs-lookup"><span data-stu-id="d1956-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="d1956-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d1956-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d1956-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="d1956-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d1956-106">Toto téma popisuje scénář nasazení typické web pro vývojáře nebo testovací prostředí a popisuje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.</span><span class="sxs-lookup"><span data-stu-id="d1956-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="d1956-107">Když vývojáři pracovat na webové aplikace, budou se často poskytnut přístup k prostředí serveru, který můžete použít k testování změny svých aplikací v reálných nastavení.</span><span class="sxs-lookup"><span data-stu-id="d1956-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="d1956-108">Tento druh vývojovém nebo testovacím prostředí obvykle má tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d1956-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="d1956-109">Prostředí se skládá z jedné databáze serveru jednom webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="d1956-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="d1956-110">Vývojáři obvykle oprávnění správce na serverech, aby mohly nakonfigurovat prostředí tak, aby požadavky aplikací.</span><span class="sxs-lookup"><span data-stu-id="d1956-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="d1956-111">Změny aplikace jsou nasazeny na základě časté, tak v prostředí musí podporovat jeden krok nebo automatizované nasazení.</span><span class="sxs-lookup"><span data-stu-id="d1956-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="d1956-112">Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink je vývojář společnosti Fabrikam, Inc. Matt pracuje na řešení obraťte se na správce a pravidelně je potřeba nasadit změny v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1956-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="d1956-113">Matt je správce na serveru webových testů a testovací databázový server.</span><span class="sxs-lookup"><span data-stu-id="d1956-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="d1956-114">Na začátku Matt musí být schopný nasadit řešení přímo do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1956-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="d1956-115">Jako pracovní připojí postupuje a další vývojáři týmu, obraťte se na správce, řešení je nakonfigurován pro nepřetržitou integraci (CI) v Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="d1956-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="d1956-116">Vždy, když vývojář zkontroluje v obsahu, Team Build by měl sestavte řešení, spustit všechny testy jednotek a automaticky nasadit řešení do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1956-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="d1956-117">Přehled řešení</span><span class="sxs-lookup"><span data-stu-id="d1956-117">Solution Overview</span></span>

<span data-ttu-id="d1956-118">Testovací prostředí musí podporovat krokování nebo automatizované nasazení ze vzdáleného počítače, takže máte na výběr dva hlavní přístupy.</span><span class="sxs-lookup"><span data-stu-id="d1956-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="d1956-119">Můžeš:</span><span class="sxs-lookup"><span data-stu-id="d1956-119">You can:</span></span>

- <span data-ttu-id="d1956-120">Konfigurace serveru webových testů pro podporu nasazení pomocí agenta služba pro nasazení webu ("vzdáleného agenta").</span><span class="sxs-lookup"><span data-stu-id="d1956-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="d1956-121">Konfigurace serveru webových testů pro podporu nasazení pomocí obslužná rutina nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="d1956-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="d1956-122">Můžete také použít [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temp agent").</span><span class="sxs-lookup"><span data-stu-id="d1956-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="d1956-123">Toto je podobná přístup vzdáleného agenta z hlediska požadavky a omezení.</span><span class="sxs-lookup"><span data-stu-id="d1956-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="d1956-124">V takovém případě vývojáři mít oprávnění správce na cílovém serveru a testovací prostředí není předmětem omezení přísných bezpečnostních, tak, aby logické volbou konfigurace serveru webového testu pro podporu nasazení pomocí vzdáleného agenta.</span><span class="sxs-lookup"><span data-stu-id="d1956-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="d1956-125">To je méně složitých a vyžaduje menší počáteční konfiguraci než přístup obslužné rutiny nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="d1956-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="d1956-126">Také budete muset konfigurovat databázový server pro podporu nasazení a vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="d1956-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="d1956-127">Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:</span><span class="sxs-lookup"><span data-stu-id="d1956-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="d1956-128">[Konfigurace webového serveru pro nasazení webu publikování (vzdáleného agenta)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="d1956-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="d1956-129">Toto téma popisuje, jak sestavit webový server, který podporuje nasazení webu publikování, pomocí agenta vzdáleného přístupu od čisté sestavení Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="d1956-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="d1956-130">[Konfigurovat databázový Server pro publikování nasazení webu](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d1956-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="d1956-131">Toto téma popisuje postup konfigurace databázový server pro podporu nasazení od výchozí instalaci systému SQL Server 2008 R2 a vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="d1956-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="d1956-132">Další čtení</span><span class="sxs-lookup"><span data-stu-id="d1956-132">Further Reading</span></span>

<span data-ttu-id="d1956-133">Pokyny týkající se konfigurace typické pracovní prostředí najdete v tématu [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="d1956-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="d1956-134">Pokyny týkající se konfigurace typické provozním prostředí najdete v tématu [scénář: Konfigurace produkčním prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="d1956-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1956-135">[Předchozí](choosing-the-right-approach-to-web-deployment.md)
> [další](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="d1956-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
