---
title: DevOps s využitím ASP.NET Core a Azure
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722607"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="26f32-103">DevOps s využitím ASP.NET Core a Azure</span><span class="sxs-lookup"><span data-stu-id="26f32-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="26f32-104">Vítejte v Průvodci životního cyklu vývoje Azure pro .NET</span><span class="sxs-lookup"><span data-stu-id="26f32-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="26f32-105">Tato příručka představuje základní koncepty vytváření životního cyklu vývoje kolem Azure s využitím .NET nástrojů a procesů.</span><span class="sxs-lookup"><span data-stu-id="26f32-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="26f32-106">Po dokončení tohoto průvodce, budete těžit z výhod až po zralé sadu nástrojů DevOps.</span><span class="sxs-lookup"><span data-stu-id="26f32-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="26f32-107">Kdo tento průvodce je na</span><span class="sxs-lookup"><span data-stu-id="26f32-107">Who this guide is for</span></span>

<span data-ttu-id="26f32-108">Zkušení vývojáři ASP.NET (úroveň 200 300) by měl být.</span><span class="sxs-lookup"><span data-stu-id="26f32-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="26f32-109">Nemusíte vědět nic o Azure, jak tom dozvíme v tomto úvodu.</span><span class="sxs-lookup"><span data-stu-id="26f32-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="26f32-110">Tento průvodce může být také užitečné pro DevOps techniky, kteří se zaměřují na operace než vývoje.</span><span class="sxs-lookup"><span data-stu-id="26f32-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="26f32-111">Tato příručka cílí na vývojáře Windows.</span><span class="sxs-lookup"><span data-stu-id="26f32-111">This guide targets Windows developers.</span></span> <span data-ttu-id="26f32-112">Systémy Linux a macOS se ale plně podporují pomocí .NET Core.</span><span class="sxs-lookup"><span data-stu-id="26f32-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="26f32-113">Přizpůsobit tento průvodce pro Linux nebo macOS, podívejte se na pro popisky pro Linux nebo macOS rozdíly.</span><span class="sxs-lookup"><span data-stu-id="26f32-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="26f32-114">Co tato příručka nepopisuje</span><span class="sxs-lookup"><span data-stu-id="26f32-114">What this guide doesn't cover</span></span>

<span data-ttu-id="26f32-115">Tato příručka se zaměřuje na začátku do konce průběžné nasazování prostředí pro vývojáře na platformě .NET.</span><span class="sxs-lookup"><span data-stu-id="26f32-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="26f32-116">Není vyčerpávající příručka k vše, co Azure, a to není zaměřit výrazně na rozhraní .NET API pro služby Azure.</span><span class="sxs-lookup"><span data-stu-id="26f32-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="26f32-117">Důraz je po průběžnou integraci, nasazení, monitorování a ladění.</span><span class="sxs-lookup"><span data-stu-id="26f32-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="26f32-118">Na konci Průvodce nabízí doporučení pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="26f32-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="26f32-119">Součástí návrhy jsou služeb platformy Azure, které jsou užitečné pro vývojáře využívající technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="26f32-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="26f32-120">Co je v této příručce</span><span class="sxs-lookup"><span data-stu-id="26f32-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="26f32-121">Nástroje a soubory ke stažení</span><span class="sxs-lookup"><span data-stu-id="26f32-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="26f32-122">Zjistěte, kde získat nástroje používané v tomto průvodci.</span><span class="sxs-lookup"><span data-stu-id="26f32-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="26f32-123">Nasazení do služby App Service</span><span class="sxs-lookup"><span data-stu-id="26f32-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="26f32-124">Přečtěte si různé metody pro nasazení aplikace ASP.NET Core do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="26f32-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="26f32-125">Průběžná integrace a nasazování</span><span class="sxs-lookup"><span data-stu-id="26f32-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="26f32-126">Sestavení začátku do konce průběžnou integraci a nasazení řešení pro aplikace ASP.NET Core s využitím Githubu, VSTS a Azure.</span><span class="sxs-lookup"><span data-stu-id="26f32-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="26f32-127">Monitorování a ladění</span><span class="sxs-lookup"><span data-stu-id="26f32-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="26f32-128">Pomocí nástrojů Azure můžete sledovat, řešení potíží a ladění aplikací.</span><span class="sxs-lookup"><span data-stu-id="26f32-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="26f32-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26f32-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="26f32-130">Další postupy výuky pro vývojáře v ASP.NET Core seznámení s Azure.</span><span class="sxs-lookup"><span data-stu-id="26f32-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="26f32-131">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="26f32-131">Acknowledgments</span></span>

<span data-ttu-id="26f32-132">Děkujeme, že jste pro všechny uživatele v komunitě .NET, který uživatel do tohoto průvodce užitečné návrhy!</span><span class="sxs-lookup"><span data-stu-id="26f32-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="26f32-133">Rádi bychom se konkrétně Děkujeme, že následující členy komunity, kteří se podílejí na konečnou kontrolu těchto materiálů:</span><span class="sxs-lookup"><span data-stu-id="26f32-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="26f32-134">SAM Wronski</span><span class="sxs-lookup"><span data-stu-id="26f32-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="26f32-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="26f32-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="26f32-136">Závěr</span><span class="sxs-lookup"><span data-stu-id="26f32-136">Conclusion</span></span>

<span data-ttu-id="26f32-137">Připraví Tento průvodce vám umožní vytvářet životního cyklu vývoje kontinuální integrace postavené na technologii ASP.NET Core a Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="26f32-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="26f32-138">Další čtení</span><span class="sxs-lookup"><span data-stu-id="26f32-138">Additional reading</span></span>

* [<span data-ttu-id="26f32-139">Co je Cloud Computing?</span><span class="sxs-lookup"><span data-stu-id="26f32-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="26f32-140">Příklady Cloud computingu</span><span class="sxs-lookup"><span data-stu-id="26f32-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="26f32-141">Co je IaaS?</span><span class="sxs-lookup"><span data-stu-id="26f32-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="26f32-142">Co je PaaS?</span><span class="sxs-lookup"><span data-stu-id="26f32-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
