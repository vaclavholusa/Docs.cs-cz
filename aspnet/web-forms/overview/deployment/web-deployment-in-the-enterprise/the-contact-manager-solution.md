---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Obraťte se na správce řešení | Microsoft Docs
author: jrjlee
description: Tato série kurzů používá ukázkové řešení&#x2014;řešení obraťte se na správce&#x2014;představují aplikace podnikovém měřítku s realistické leve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883697"
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="36c3a-103">Obraťte se na správce řešení</span><span class="sxs-lookup"><span data-stu-id="36c3a-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="36c3a-104">podle [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="36c3a-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="36c3a-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="36c3a-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="36c3a-106">To [série kurzů](web-deployment-in-the-enterprise.md) používá ukázkové řešení&#x2014;řešení obraťte se na správce&#x2014;představují podnikovém měřítku aplikace s úrovní realistické složitější.</span><span class="sxs-lookup"><span data-stu-id="36c3a-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="36c3a-107">Toto téma představuje řešení obraťte se na správce, popisuje klíčové komponenty řešení a identifikuje problémy při nasazení tento druh aplikace na různé cílové platformy v podnikovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="36c3a-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="36c3a-108">Při práci prostřednictvím v tématech v těchto kurzech můžete jako referenční implementace, které ukazuje, jak můžou řešit konkrétní problémy v podnikové scénáře nasazení řešení obraťte se na správce.</span><span class="sxs-lookup"><span data-stu-id="36c3a-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="36c3a-109">Dalším tématu [nastavení nahoru řešení obraťte se na správce](setting-up-the-contact-manager-solution.md), popisuje postup stažení a spuštění řešení na pracovní stanici developer.</span><span class="sxs-lookup"><span data-stu-id="36c3a-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="36c3a-110">Přehled řešení</span><span class="sxs-lookup"><span data-stu-id="36c3a-110">Solution Overview</span></span>

<span data-ttu-id="36c3a-111">Řešení obraťte se na správce se skládá ze čtyř jednotlivých projektů:</span><span class="sxs-lookup"><span data-stu-id="36c3a-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="36c3a-112">**ContactManager.Mvc**.</span><span class="sxs-lookup"><span data-stu-id="36c3a-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="36c3a-113">Toto je projekt webové aplikace technologie ASP.NET MVC 3, která představuje vstupní bod pro řešení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="36c3a-114">Nabízí některé funkce základní webové aplikace, jako jsou nabízí uživatelům možnost vytvářet a prohlížet kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="36c3a-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="36c3a-115">Aplikace spoléhá na službu Windows Communication Foundation (WCF) ke správě kontakty a databázi služeb aplikace ASP.NET ke správě ověřování a autorizace.</span><span class="sxs-lookup"><span data-stu-id="36c3a-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="36c3a-116">**ContactManager.Database**.</span><span class="sxs-lookup"><span data-stu-id="36c3a-116">**ContactManager.Database**.</span></span> <span data-ttu-id="36c3a-117">Toto je databáze projekt sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36c3a-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="36c3a-118">Projekt definuje schéma pro databázi, aby úložiště kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="36c3a-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="36c3a-119">**ContactManager.Service**.</span><span class="sxs-lookup"><span data-stu-id="36c3a-119">**ContactManager.Service**.</span></span> <span data-ttu-id="36c3a-120">Toto je projekt webové služby WCF.</span><span class="sxs-lookup"><span data-stu-id="36c3a-120">This is a WCF web service project.</span></span> <span data-ttu-id="36c3a-121">Zpřístupňuje služby WCF, koncový bod, který umožňuje volajícím provádět vytvořit, získat, aktualizovat a odstranit operace na **ContactManager** databáze.</span><span class="sxs-lookup"><span data-stu-id="36c3a-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="36c3a-122">Služba závisí na **ContactManager** databáze a **ContactManager.Common.dll** sestavení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="36c3a-123">**ContactManager.Common**.</span><span class="sxs-lookup"><span data-stu-id="36c3a-123">**ContactManager.Common**.</span></span> <span data-ttu-id="36c3a-124">Toto je projektu knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="36c3a-124">This is a class library project.</span></span> <span data-ttu-id="36c3a-125">Služby WCF spoléhá na typy definované v tomto sestavení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="36c3a-126">Řešení zahrnuje také řešení složku s názvem publikovat.</span><span class="sxs-lookup"><span data-stu-id="36c3a-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="36c3a-127">Tato položka obsahuje různé soubory vlastních projektů a soubory příkazů, které ukazují, jak můžete řídit a manipulaci s procesem sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="36c3a-128">Tyto jsou podrobněji popsané později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="36c3a-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="36c3a-129">Koncepční úrovni zapadají součástí řešení takto:</span><span class="sxs-lookup"><span data-stu-id="36c3a-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="36c3a-130">Pokud webová aplikace ASP.NET MVC 3 používá poskytovatele členství prostředí ASP.NET, všechny stránky v rámci webové aplikace povolí anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="36c3a-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="36c3a-131">Toto není jasně realistické konfigurace.</span><span class="sxs-lookup"><span data-stu-id="36c3a-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="36c3a-132">Řešení je ale nastavit tímto způsobem, aby bylo snazší pro vás k nasazení a testování řešení bez nutnosti konfigurace uživatelských účtů a rolí.</span><span class="sxs-lookup"><span data-stu-id="36c3a-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="36c3a-133">Nasazení výzvy</span><span class="sxs-lookup"><span data-stu-id="36c3a-133">Deployment Challenges</span></span>

<span data-ttu-id="36c3a-134">Obraťte se na správce řešení znázorňuje několik problémů nasazení, které jsou společné pro velké podnikové scénáře nasazení:</span><span class="sxs-lookup"><span data-stu-id="36c3a-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="36c3a-135">Řešení se skládá z více závislých projektů.</span><span class="sxs-lookup"><span data-stu-id="36c3a-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="36c3a-136">Je třeba nasadit tyto projektů současně.</span><span class="sxs-lookup"><span data-stu-id="36c3a-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="36c3a-137">Připojovací řetězce a koncové body služby je třeba aktualizovat pro každé prostředí a v mnoha případech nebude tyto informace k dispozici pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="36c3a-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="36c3a-138">Při nasazení **ContactManager** databáze do pracovní a provozní prostředí, je nutné zachovat stávající data na následné nasazení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="36c3a-139">Když nasadíte databáze aplikačních služeb ASP.NET, je třeba nasadit některé konfigurační data, ale vynechejte jakákoliv uživatelská data účtu.</span><span class="sxs-lookup"><span data-stu-id="36c3a-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="36c3a-140">Projekty zahrnují některé soubory a složky, které by neměly být nasazeny.</span><span class="sxs-lookup"><span data-stu-id="36c3a-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="36c3a-141">Je potřeba vyloučit tyto soubory a složky z procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="36c3a-142">Řešení musí podporovat automatického nasazení ze serveru Team Foundation Server (TFS) sestavení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="36c3a-143">Závěr</span><span class="sxs-lookup"><span data-stu-id="36c3a-143">Conclusion</span></span>

<span data-ttu-id="36c3a-144">Toto téma poskytuje přehled řešení obraťte se na správce a identifikovat některé z problémů vyplývajících nasazení, které jsou společné pro velké podnikové scénáře nasazení.</span><span class="sxs-lookup"><span data-stu-id="36c3a-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="36c3a-145">Zbývající témata v tomto kurzu popisují některé techniky, které můžete použít ke splnění tyto problémy.</span><span class="sxs-lookup"><span data-stu-id="36c3a-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="36c3a-146">Dalším tématu [nastavení nahoru řešení obraťte se na správce](setting-up-the-contact-manager-solution.md), popisuje postup stažení a spuštění řešení na pracovní stanici developer.</span><span class="sxs-lookup"><span data-stu-id="36c3a-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36c3a-147">[Předchozí](web-deployment-in-the-enterprise.md)
> [další](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="36c3a-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
