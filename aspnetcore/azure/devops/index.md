---
title: DevOps s využitím ASP.NET Core a Azure
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 53667831f5e33107178a947f23d957ff22e8c1a0
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340092"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="b6d1c-103">DevOps s využitím ASP.NET Core a Azure</span><span class="sxs-lookup"><span data-stu-id="b6d1c-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="b6d1c-104">[![Titulní obrázek](./media/cover-large.png)](https://aka.ms/devopsbook)</span><span class="sxs-lookup"><span data-stu-id="b6d1c-104">[![Cover Image](./media/cover-large.png)](https://aka.ms/devopsbook)</span></span>

<span data-ttu-id="b6d1c-105">Podle [kamera Soper](https://twitter.com/camsoper) a [Scott Addie](https://twitter.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b6d1c-105">By [Cam Soper](https://twitter.com/camsoper) and [Scott Addie](https://twitter.com/scottaddie)</span></span>

<span data-ttu-id="b6d1c-106">Tento průvodce je k dispozici jako [PDF e kniha ke stažení](https://aka.ms/devopsbook).</span><span class="sxs-lookup"><span data-stu-id="b6d1c-106">This guide is available as a [downloadable PDF e-book](https://aka.ms/devopsbook).</span></span>

## <a name="welcome"></a><span data-ttu-id="b6d1c-107">Vítej</span><span class="sxs-lookup"><span data-stu-id="b6d1c-107">Welcome</span></span> 

<span data-ttu-id="b6d1c-108">Vítejte v Průvodci životního cyklu vývoje Azure pro .NET</span><span class="sxs-lookup"><span data-stu-id="b6d1c-108">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="b6d1c-109">Tato příručka představuje základní koncepty vytváření životního cyklu vývoje kolem Azure s využitím .NET nástrojů a procesů.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-109">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="b6d1c-110">Po dokončení tohoto průvodce, budete těžit z výhod až po zralé sadu nástrojů DevOps.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-110">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="b6d1c-111">Kdo tento průvodce je na</span><span class="sxs-lookup"><span data-stu-id="b6d1c-111">Who this guide is for</span></span>

<span data-ttu-id="b6d1c-112">Měli byste být vývojáři ASP.NET Core (úroveň 200 300).</span><span class="sxs-lookup"><span data-stu-id="b6d1c-112">You should be an experienced ASP.NET Core developer (200-300 level).</span></span> <span data-ttu-id="b6d1c-113">Nemusíte vědět nic o Azure, jak tom dozvíme v tomto úvodu.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-113">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="b6d1c-114">Tento průvodce může být také užitečné pro DevOps techniky, kteří se zaměřují na operace než vývoje.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-114">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="b6d1c-115">Tato příručka cílí na vývojáře Windows.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-115">This guide targets Windows developers.</span></span> <span data-ttu-id="b6d1c-116">Systémy Linux a macOS se ale plně podporují pomocí .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-116">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="b6d1c-117">Přizpůsobit tento průvodce pro Linux nebo macOS, podívejte se na pro popisky pro Linux nebo macOS rozdíly.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-117">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="b6d1c-118">Co tato příručka nepopisuje</span><span class="sxs-lookup"><span data-stu-id="b6d1c-118">What this guide doesn't cover</span></span>

<span data-ttu-id="b6d1c-119">Tato příručka se zaměřuje na začátku do konce průběžné nasazování prostředí pro vývojáře na platformě .NET.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-119">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="b6d1c-120">Není vyčerpávající příručka k vše, co Azure, a to není zaměřit výrazně na rozhraní .NET API pro služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-120">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="b6d1c-121">Důraz je po průběžnou integraci, nasazení, monitorování a ladění.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-121">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="b6d1c-122">Na konci Průvodce nabízí doporučení pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-122">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="b6d1c-123">Součástí návrhy jsou služeb platformy Azure, které jsou užitečné pro vývojáře v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-123">Included in the suggestions are Azure platform services that are useful to ASP.NET Core developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="b6d1c-124">Co je v této příručce</span><span class="sxs-lookup"><span data-stu-id="b6d1c-124">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="b6d1c-125">Nástroje a soubory ke stažení</span><span class="sxs-lookup"><span data-stu-id="b6d1c-125">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="b6d1c-126">Zjistěte, kde získat nástroje používané v tomto průvodci.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-126">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="b6d1c-127">Nasazení do App Service</span><span class="sxs-lookup"><span data-stu-id="b6d1c-127">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="b6d1c-128">Přečtěte si různé metody pro nasazení aplikace ASP.NET Core do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-128">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="b6d1c-129">Průběžná integrace a nasazování</span><span class="sxs-lookup"><span data-stu-id="b6d1c-129">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="b6d1c-130">Sestavení začátku do konce průběžnou integraci a nasazení řešení pro aplikace ASP.NET Core s využitím Githubu, služby Azure DevOps a Azure.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-130">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, Azure DevOps Services, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="b6d1c-131">Monitorování a ladění</span><span class="sxs-lookup"><span data-stu-id="b6d1c-131">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="b6d1c-132">Pomocí nástrojů Azure můžete sledovat, řešení potíží a ladění aplikací.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-132">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="b6d1c-133">Další postup</span><span class="sxs-lookup"><span data-stu-id="b6d1c-133">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="b6d1c-134">Další postupy výuky pro vývojáře v ASP.NET Core seznámení s Azure.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-134">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="additional-introductory-reading"></a><span data-ttu-id="b6d1c-135">Další úvodní články</span><span class="sxs-lookup"><span data-stu-id="b6d1c-135">Additional introductory reading</span></span>

<span data-ttu-id="b6d1c-136">Pokud je toto první vystavení ke cloud computingu, tyto články vysvětlují základní informace.</span><span class="sxs-lookup"><span data-stu-id="b6d1c-136">If this is your first exposure to cloud computing, these articles explain the basics.</span></span>

* [<span data-ttu-id="b6d1c-137">Co je Cloud Computing?</span><span class="sxs-lookup"><span data-stu-id="b6d1c-137">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="b6d1c-138">Příklady Cloud computingu</span><span class="sxs-lookup"><span data-stu-id="b6d1c-138">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="b6d1c-139">Co je IaaS?</span><span class="sxs-lookup"><span data-stu-id="b6d1c-139">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="b6d1c-140">Co je PaaS?</span><span class="sxs-lookup"><span data-stu-id="b6d1c-140">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
