---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scénář: Konfigurace testovacího prostředí pro nasazení webu | Dokumentace Microsoftu'
author: jrjlee
description: Toto téma popisuje běžné webové scénáře nasazení pro vývojáře nebo testovací prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení incidentech...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ba77c944378245ed82d1cee92b668e31acc51a81
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822367"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="87c11-103">Scénář: Konfigurace testovacího prostředí pro nasazení webu</span><span class="sxs-lookup"><span data-stu-id="87c11-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="87c11-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="87c11-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="87c11-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="87c11-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="87c11-106">Toto téma popisuje běžné webové scénář nasazení pro vývojáře nebo testovací prostředí a vysvětluje úlohy, které potřebujete k dokončení pro nastavení prostředí podobné.</span><span class="sxs-lookup"><span data-stu-id="87c11-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="87c11-107">Když vývojáři pracují na webové aplikace, jsou často budete poskytnut přístup k prostředí serveru, který můžete použít k testování změny do svých aplikací v reálné nastavení.</span><span class="sxs-lookup"><span data-stu-id="87c11-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="87c11-108">Tento druh vývojové nebo testovací prostředí obvykle má tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="87c11-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="87c11-109">Prostředí se skládá z jediného webového serveru a jednoho databázového serveru.</span><span class="sxs-lookup"><span data-stu-id="87c11-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="87c11-110">Vývojáři obvykle oprávnění správce na serverech, aby mohly nakonfigurujte prostředí tak, aby požadavkům jejich aplikací.</span><span class="sxs-lookup"><span data-stu-id="87c11-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="87c11-111">Změny aplikací se nasazují na základě časté, takže prostředí musí podporovat krokování nebo automatizovat nasazení.</span><span class="sxs-lookup"><span data-stu-id="87c11-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="87c11-112">Například v našem [kurz scénář](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink je pro vývojáře společnosti Fabrikam, Inc. Matt pracuje na řešení Správce kontaktů a pravidelně je potřeba nasadit změny do testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="87c11-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="87c11-113">Matt je správcem na webovém serveru testu a testovací databázový server.</span><span class="sxs-lookup"><span data-stu-id="87c11-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="87c11-114">Matt na začátku musí být možné přímo nasadit řešení na testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="87c11-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="87c11-115">Jako práce postupuje a další vývojáři připojit k týmu, kontaktujte správce řešení je nakonfigurovaný pro průběžnou integraci (CI) v Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="87c11-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="87c11-116">Pokaždé, když vývojář vrátí obsah, Team Build by měl sestavte řešení, spuštění všech testů jednotek a automaticky nasadit řešení na testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="87c11-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="87c11-117">Přehled řešení</span><span class="sxs-lookup"><span data-stu-id="87c11-117">Solution Overview</span></span>

<span data-ttu-id="87c11-118">Testovací prostředí musí podporovat krokování nebo automatizovat nasazení ze vzdáleného počítače, takže máte na výběr dva hlavní přístupy.</span><span class="sxs-lookup"><span data-stu-id="87c11-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="87c11-119">Můžeš:</span><span class="sxs-lookup"><span data-stu-id="87c11-119">You can:</span></span>

- <span data-ttu-id="87c11-120">Konfigurace serveru webových testů pro podporu nasazení pomocí Služba agenta nasazení webu ("vzdáleného agenta").</span><span class="sxs-lookup"><span data-stu-id="87c11-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="87c11-121">Konfigurace serveru webových testů pro podporu nasazení pomocí obslužné rutiny pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="87c11-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="87c11-122">Můžete také použít [nasazení webu na vyžádání](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temp agent").</span><span class="sxs-lookup"><span data-stu-id="87c11-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="87c11-123">Toto je podobný přístup vzdáleného agenta z hlediska požadavky a omezení.</span><span class="sxs-lookup"><span data-stu-id="87c11-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="87c11-124">V tomto případě vývojáři oprávnění správce na cílových serverech a testovacího prostředí není předmětem pravidla striktní bezpečnostní omezení, tedy logickou volbou konfigurace serveru webového testu pro podporu nasazení pomocí vzdáleného agenta.</span><span class="sxs-lookup"><span data-stu-id="87c11-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="87c11-125">To není tak složitý a vyžaduje menší počáteční nastavení, než obslužná rutina nasazení webového přístupu.</span><span class="sxs-lookup"><span data-stu-id="87c11-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="87c11-126">Také budete muset konfigurovat databázový server pro podporu vzdáleného přístupu a nasazení.</span><span class="sxs-lookup"><span data-stu-id="87c11-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="87c11-127">Tato témata poskytují všechny informace, které potřebujete k dokončení těchto úloh:</span><span class="sxs-lookup"><span data-stu-id="87c11-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="87c11-128">[Konfigurace webového serveru pro nasazení webu (vzdálený Agent) publikování](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="87c11-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="87c11-129">Toto téma popisuje, jak vytvořit web server, který podporuje nasazení webu publikování, pomocí agenta vzdáleného přístupu od čisté sestavení systému Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="87c11-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="87c11-130">[Konfigurovat databázový Server pro publikování nasazeného webu](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="87c11-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="87c11-131">Toto téma popisuje, jak konfigurovat databázový server pro podporu vzdáleného přístupu a nasazení, od výchozí instalaci systému SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="87c11-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="87c11-132">Další čtení</span><span class="sxs-lookup"><span data-stu-id="87c11-132">Further Reading</span></span>

<span data-ttu-id="87c11-133">Pokyny k nastavení typické přípravné prostředí, najdete v části [scénář: Konfigurace pracovní prostředí pro nasazení webu](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="87c11-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="87c11-134">Pokyny ke konfiguraci typickém produkčním prostředí najdete v tématu [scénář: Konfigurace provozního prostředí pro nasazení webu](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="87c11-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87c11-135">[Předchozí](choosing-the-right-approach-to-web-deployment.md)
> [další](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="87c11-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
