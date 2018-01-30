---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "Předá na testovacím: udržovatelný weby Azure: Správa změn a škálování | Microsoft Docs"
author: rick-anderson
description: "V tomto testovacím prostředí zjistěte, jak Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 4bce02b2c592ff04e0dbce78d18004c69268e4fd
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="4f5ba-103">Předá na testovacím: udržovatelný weby Azure: Správa změn a škálování</span><span class="sxs-lookup"><span data-stu-id="4f5ba-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="4f5ba-104">podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="4f5ba-105">Stažení webové táborech cvičení Kit</span><span class="sxs-lookup"><span data-stu-id="4f5ba-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="4f5ba-106">Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="4f5ba-107">Ale nejsou uděláte, když je aplikace za provozu, budete právě začínat!</span><span class="sxs-lookup"><span data-stu-id="4f5ba-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="4f5ba-108">Budete potřebovat pro zpracování Změna požadavky, aktualizace databáze, škálování a další.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="4f5ba-109">Naštěstí Azure App Service se můžete zahrnutých s dostatkem funkce vám pomůže ochránit vaše lokality plynulý chod.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="4f5ba-110">Azure nabízí zabezpečené a flexibilní vývoj, nasazení a škálování možnosti pro všechny velikosti webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="4f5ba-111">Využívejte existující nástroje k vytváření a nasazování aplikací bez starostí o správu infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="4f5ba-112">Poskytnout produkční webové aplikace se v minutách snadno nasazení obsahu, které jsou vytvořené pomocí Oblíbené vývojový nástroj.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="4f5ba-113">Můžete nasadit přímo od správy zdrojového kódu se podpora pro existující lokalitu **Git**, **Githubu**, **Bitbucket**, **TFS**a to i v  **DropBox**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="4f5ba-114">Nasazení přímo z vašeho oblíbeného rozhraní IDE nebo skripty s použitím **prostředí PowerShell** v systému Windows nebo **rozhraní příkazového řádku** nástroje systémem žádné operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="4f5ba-115">Po nasazení zachovat vaše lokality s podporou pro průběžné nasazování neustále aktuální.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="4f5ba-116">Azure poskytuje škálovatelné, odolné cloudového úložiště, zálohování a obnovení řešení pro všechna data, velké nebo malé.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="4f5ba-117">Při nasazování aplikací pro produkční prostředí, služby úložiště, jako jsou tabulky, objekty BLOB a databází SQL, můžete škálovat aplikaci v cloudu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="4f5ba-118">U databází SQL je důležité k zachování aktualizovaného stavu databáze produktivitu při nasazování nové verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="4f5ba-119">Poděkování **migrace Entity Framework Code First**, vývoj a nasazení datový model je jednodušší aktualizovat vaše prostředí v minutách.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="4f5ba-120">Toto praktické cvičení se zobrazí v různých tématech, ke kterým může dojít při nasazování webové aplikace do produkčního prostředí v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="4f5ba-121">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="4f5ba-122">Další podrobné pokrytí tohoto tématu, najdete v článku [vytváření reálných cloudových aplikací pomocí Azure elektronická kniha](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="4f5ba-123">Přehled</span><span class="sxs-lookup"><span data-stu-id="4f5ba-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="4f5ba-124">Cíle</span><span class="sxs-lookup"><span data-stu-id="4f5ba-124">Objectives</span></span>

<span data-ttu-id="4f5ba-125">V tomto testovacím prostředí praktických se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="4f5ba-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="4f5ba-126">Povolit Entity Framework migrace pomocí existujícího modelu</span><span class="sxs-lookup"><span data-stu-id="4f5ba-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="4f5ba-127">Aktualizace modelu objektu a odpovídajícím způsobem používat Entity Framework migrace databáze</span><span class="sxs-lookup"><span data-stu-id="4f5ba-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="4f5ba-128">Nasazení do Azure App Service pomocí Git</span><span class="sxs-lookup"><span data-stu-id="4f5ba-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="4f5ba-129">Vrácení zpět na předchozí nasazení pomocí portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="4f5ba-130">Škálování webové aplikace pomocí Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4f5ba-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="4f5ba-131">Konfigurace automatického škálování pro webovou aplikaci pomocí portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="4f5ba-132">Vytvoření a konfigurace testovacího projektu zatížení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ba-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="4f5ba-133">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f5ba-133">Prerequisites</span></span>

<span data-ttu-id="4f5ba-134">Pro dokončení této praktické cvičení je vyžadován následující text:</span><span class="sxs-lookup"><span data-stu-id="4f5ba-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="4f5ba-135">[Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/) nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="4f5ba-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="4f5ba-136">Azure SDK pro .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="4f5ba-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="4f5ba-137">Systém správy verzí GIT</span><span class="sxs-lookup"><span data-stu-id="4f5ba-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="4f5ba-138">Předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-138">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="4f5ba-139">Zaregistrujte si [bezplatné zkušební verze](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="4f5ba-140">Pokud jste Visual Studio Professional, Test Professional, Premium nebo Ultimate s předplatitele MSDN nebo MSDN platformy, aktivovat vaší [výhody webu MSDN](http://aka.ms/watk-msdn) nyní zahájíte vývoj a testování v Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="4f5ba-141">[BizSpark](http://aka.ms/watk-bizspark) členové automaticky obdrží Azure benefit prostřednictvím jejich Visual Studio Ultimate s předplatné MSDN</span><span class="sxs-lookup"><span data-stu-id="4f5ba-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="4f5ba-142">Členové [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials na program přijímat měsíční kredity Azure zdarma.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="4f5ba-143">Instalace</span><span class="sxs-lookup"><span data-stu-id="4f5ba-143">Setup</span></span>

<span data-ttu-id="4f5ba-144">Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="4f5ba-145">Otevřete Průzkumníka Windows a přejděte do testovacího prostředí na **zdroj** složky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="4f5ba-146">Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="4f5ba-147">Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="4f5ba-148">Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="4f5ba-149">Používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="4f5ba-149">Using the Code Snippets</span></span>

<span data-ttu-id="4f5ba-150">V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="4f5ba-151">Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="4f5ba-152">Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="4f5ba-153">Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="4f5ba-154">Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="4f5ba-155">Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="4f5ba-156">Cvičení</span><span class="sxs-lookup"><span data-stu-id="4f5ba-156">Exercises</span></span>

<span data-ttu-id="4f5ba-157">Toto praktické cvičení zahrnuje následující cvičení:</span><span class="sxs-lookup"><span data-stu-id="4f5ba-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="4f5ba-158">Pomocí migrace Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4f5ba-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="4f5ba-159">Nasazení webové aplikace do pracovní</span><span class="sxs-lookup"><span data-stu-id="4f5ba-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="4f5ba-160">Provedení vrácení nasazení v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="4f5ba-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="4f5ba-161">Škálování používat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="4f5ba-162">[Použití automatického škálování pro webové aplikace](#Exercise5) (volitelné pro Visual Studio 2013 Ultimate edition)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="4f5ba-163">Odhadovaný čas dokončení tohoto testovacího prostředí: **75 minut**</span><span class="sxs-lookup"><span data-stu-id="4f5ba-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="4f5ba-164">Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="4f5ba-165">Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="4f5ba-166">Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="4f5ba-167">Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="4f5ba-168">Cvičení 1: Pomocí migrace Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4f5ba-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="4f5ba-169">Při vývoji aplikace, můžou časem změnit datový model.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="4f5ba-170">Tyto změny mohou ovlivnit existující model v databázi (Pokud vytváříte novou verzi) a je důležité zajistit, aby vaše databáze aktuální zabráníte tak chybám.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="4f5ba-171">Pro zjednodušení sledování tyto změny v modelu, **migrace Entity Framework Code First** automaticky zjišťuje změny porovnání modelu pomocí schématu databáze a generuje konkrétní kód aktualizovat vaši databázi vytváření nových *verze* vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="4f5ba-172">Toto cvičení se dozvíte, jak povolit **migrace** pro vaše aplikace a jak můžete snadno zjistit a generovat změny k aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="4f5ba-173">Úloha 1 – povolení migrace</span><span class="sxs-lookup"><span data-stu-id="4f5ba-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="4f5ba-174">V této úloze bude projít kroky povolení **migrace Entity Framework Code First** k **Geek kvízu** databáze, změna modelu a pochopení, jak tyto změny se projeví v databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="4f5ba-175">Otevřete Visual Studio a otevřete **GeekQuiz.sln** soubor řešení z **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="4f5ba-176">Sestavte řešení Chcete-li stáhnout a nainstalovat **NuGet** balíček závislosti.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="4f5ba-177">Chcete-li to provést, klikněte pravým tlačítkem na řešení a klikněte na **sestavit řešení** nebo stiskněte klávesu **Ctrl + Shift + B**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="4f5ba-178">Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="4f5ba-179">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="4f5ba-180">Vytvoří se při počáteční migraci na základě existující modelu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="4f5ba-181">![Povolení migrace](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "povolení migrace")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="4f5ba-182">*Povolení migrace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-183">Tento příkaz přidá **migrace** složky pro Geek kvízu projekt, který obsahuje soubor s názvem **Configuration.cs**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="4f5ba-184">**Konfigurace** třída umožňuje konfigurovat chování migrace pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="4f5ba-185">Pomocí migrace povolena, je potřeba aktualizovat **konfigurace** třídu k naplnění databáze počáteční data, **Geek kvízu** vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="4f5ba-186">V části **migrace**, nahraďte **Configuration.cs** soubor se nachází v **Source\Assets** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-187">Vzhledem k tomu **migrace** zavolá **počáteční hodnoty** metoda při každé aktualizaci databáze, je potřeba mít jistotu, že nejsou duplicitní záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="4f5ba-188">**AddOrUpdate** metoda pomáhají zabránit duplicitní data.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="4f5ba-189">Chcete-li přidat počáteční migrace, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-190">Ujistěte se, že neexistuje žádná databáze s názvem &quot;GeekQuizProd&quot; ve vaší instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="4f5ba-191">![Přidání základní schématu migrace](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "přidání základní schématu migrace")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="4f5ba-192">*Přidání základní schématu migrace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-193">**Přidat migrace** bude vygenerovat další migraci na základě změn, které jste udělali modelu od vytvoření posledního migrace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="4f5ba-194">V takovém případě je to první migrace projektu, přidá skripty k vytvoření všechny tabulky definované v **TriviaContext** třídy.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="4f5ba-195">Spusťte migrace k aktualizaci databáze spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="4f5ba-196">Pro tento příkaz zadejte **podrobné** zobrazíte příkazy SQL aplikované na cílovou databázi.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="4f5ba-197">![Vytváření počáteční databáze](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "vytváření počáteční databáze")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="4f5ba-198">*Vytvoření počáteční databáze*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-199">**Update-Database** použije všechny nevyřízené migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="4f5ba-200">V takovém případě vytvoří databázi pomocí připojovacího řetězce definované v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="4f5ba-201">Přejděte na **zobrazení** nabídky a otevřete **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="4f5ba-202">![Otevřít v Průzkumníku objektů systému SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "otevřít v Průzkumníku objektů systému SQL Server")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="4f5ba-203">*Otevřít v Průzkumníku objektů systému SQL Server*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="4f5ba-204">V **Průzkumník objektů systému SQL Server** okně připojení k vaší instanci LocalDB kliknutím pravým tlačítkem myši **systému SQL Server** uzlu a výběrem **přidat SQL Server...**  možnost.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="4f5ba-205">![Přidání Instance systému SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "přidání Instance systému SQL Server")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="4f5ba-206">*Přidání instance systému SQL Server do Průzkumník objektů systému SQL Server*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="4f5ba-207">Nastavte **název serveru** k *(localdb) \v11.0* a nechte **ověřování systému Windows** jako vaše režim ověřování.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="4f5ba-208">Klikněte na tlačítko **Connect** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="4f5ba-209">![Připojení k LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "připojení na instanci LocalDB")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="4f5ba-210">*Připojení k instanci LocalDB*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="4f5ba-211">Otevřete **GeekQuizProd** databáze a rozbalte **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="4f5ba-212">Jak je vidět **Update-Database** příkaz vygeneruje všechny tabulky definované v **TriviaContext** – třída.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="4f5ba-213">Vyhledejte **dbo. TriviaQuestions** tabulky a otevřete uzel sloupce.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="4f5ba-214">V dalším úkolem přidáte nový sloupec do této tabulky a aktualizovat databázi pomocí **migrace**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="4f5ba-215">![Trivia otázky, které sloupce](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia otázky, které sloupce")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="4f5ba-216">*Trivia otázky, které sloupce*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="4f5ba-217">Úloha 2 – aktualizace schématu databáze pomocí migrace</span><span class="sxs-lookup"><span data-stu-id="4f5ba-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="4f5ba-218">V této úloze budete používat **migrace Entity Framework Code First** zjistí změnu v modelu a generovat kód nezbytné k aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="4f5ba-219">Aktualizujte **TriviaQuestions** entity přidáním novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="4f5ba-220">Potom budete spouštět příkazy k vytvoření nové migrace zahrnout nového sloupce v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="4f5ba-221">V **Průzkumníku řešení**, dvakrát klikněte **TriviaQuestion.cs** soubor umístěný uvnitř **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="4f5ba-222">Přidat novou vlastnost s názvem **pomocný parametr**, jak ukazuje následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="4f5ba-223">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="4f5ba-224">Nové migrace bude vytvořen odrážející změny v našich modelu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="4f5ba-225">![Přidat migrace](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "přidat migrace")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="4f5ba-226">*Přidat migrace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-227">Soubor migrace se skládá ze dvou metod **až** a **dolů**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="4f5ba-228">**Až** metoda se použije k určení, jaké změny aktuální verzi naše aplikace potřeba použít k databázi.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="4f5ba-229">**Dolů** se používá k změny jsme přidali do **až** metoda.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="4f5ba-230">Při migraci databáze aktualizuje databázi, se aplikace spustí všechny migrace v pořadí časové razítko a pouze ty, které nebyly použity od poslední aktualizace ( \_MigrationHistory tabulky uchovává informace o migrace, které byly použity).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="4f5ba-231">**Až** bude volána metoda všechny migrace a provede změny, které jsme určili do databáze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="4f5ba-232">Pokud jsme rozhodne se vrátíte k předchozí migrace **dolů** metoda bude volána k vrátit změny v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="4f5ba-233">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="4f5ba-234">Výstup **Update-Database** příkaz generované **příkaz Alter Table** příkaz jazyka SQL, které chcete přidat nový sloupec na **TriviaQuestions** tabulky, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="4f5ba-235">![Přidejte příkaz SQL sloupec generované](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "přidejte příkaz SQL sloupec vygenerována")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="4f5ba-236">*Přidejte příkaz SQL sloupec vygenerována*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="4f5ba-237">V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, že nové **pomocný parametr** je zobrazen sloupec.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="4f5ba-238">![Zobrazení nového sloupce pomocný parametr](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "zobrazující nový sloupec pomocný parametr")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="4f5ba-239">*Zobrazení nového sloupce pomocný parametr*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="4f5ba-240">Zpět v **TriviaQuestion.cs** editoru přidejte **StringLength** omezení, které *pomocný parametr* vlastnost, jak je znázorněno v následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="4f5ba-241">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="4f5ba-242">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="4f5ba-243">Výstup **Update-Database** příkaz generované **příkaz Alter Table** příkazu SQL k aktualizaci *pomocný parametr* typ sloupce pro **TriviaQuestions** tabulky, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="4f5ba-244">![Příkaz ALTER vygenerovat příkaz SQL sloupec](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter vygenerovat příkaz SQL sloupce")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="4f5ba-245">*Příkaz ALTER vygenerovat příkaz SQL sloupce*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="4f5ba-246">V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, zda **pomocný parametr** typ sloupce je **nvarchar(150)**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="4f5ba-247">![Zobrazuje nové omezení](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "zobrazující nové omezení")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="4f5ba-248">*Zobrazuje nové omezení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="4f5ba-249">Cvičení 2: Nasazení webové aplikace do pracovní</span><span class="sxs-lookup"><span data-stu-id="4f5ba-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="4f5ba-250">**Webové aplikace v Azure App Service** umožňuje provádět dvoufázové instalace publikování.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="4f5ba-251">Dvoufázové instalace publikování vytvoří přípravný slot lokality pro každou lokalitu výchozí produkční a umožňuje Prohodit tyto sloty bez výpadek.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="4f5ba-252">To je opravdu užitečné ověřit změny před uvolněním veřejné, postupně integrovat obsah webu a vrátit zpět, jestliže změny nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="4f5ba-253">V tomto cvičení budete nasazovat **Geek kvízu** aplikace pracovní prostředí vaší webové aplikace pomocí Git zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="4f5ba-254">K tomuto účelu bude vytvoření webové aplikace a zřídit požadované součásti v portálu pro správu, konfiguraci **Git** úložiště a nabízenými aplikace zdrojový kód z místního počítače pro přípravný slot.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="4f5ba-255">Je také aktualizovat vaši produkční databázi s **migrace Code First** jste vytvořili v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="4f5ba-256">Aplikace pak provede v tomto prostředí test ověřit jeho funkci.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="4f5ba-257">Jakmile budete spokojeni to funguje podle vašim požadavkům, bude podporovat aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="4f5ba-258">Chcete-li povolit publikování v dvoufázové instalace, musí být webové aplikace v **standardní režim**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="4f5ba-259">Všimněte si, že další poplatky budou vám být účtovány Pokud změníte vaší webové aplikace do standardního režimu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="4f5ba-260">Další informace o cenách najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="4f5ba-261">Úloha 1 – Vytvoření webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4f5ba-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="4f5ba-262">V této úloze, vytvoříte webovou aplikaci v **Azure App Service** z portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="4f5ba-263">Můžete také nakonfigurovat **SQL Database** zachovat data aplikací a konfigurace místní úložiště Git pro řízení zdrojů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="4f5ba-264">Přejděte na [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Přihlaste se k portálu správy Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="4f5ba-266">*Přihlaste se k portálu správy Azure*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="4f5ba-267">Klikněte na tlačítko **nový** na panelu příkazů v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="4f5ba-268">![Vytvoření nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "vytvoření nové webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="4f5ba-269">*Vytvoření nové webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="4f5ba-270">Klikněte na tlačítko **výpočetní**, **webu** a potom **vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="4f5ba-271">![Vytvoření nové webové aplikace pomocí vytvořit vlastní](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "vytvoření nové webové aplikace pomocí vytvořit vlastní")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="4f5ba-272">*Vytvoření nové webové aplikace pomocí vytvořit vlastní*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="4f5ba-273">V **webu nový - vytvořit vlastní** dialogové okno Zadejte dostupný **URL** (například *geek kvízu*), vyberte umístění, do **oblast** rozevírací seznam a vyberte **vytvořit novou databázi SQL** v **databáze** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="4f5ba-274">Nakonec vyberte **publikování od správy zdrojového kódu** zaškrtávací políčko a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![Přizpůsobení nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="4f5ba-276">*Přizpůsobení nové webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="4f5ba-277">Zadejte následující informace pro nastavení databáze:</span><span class="sxs-lookup"><span data-stu-id="4f5ba-277">Specify the following information for the database settings:</span></span>

    - <span data-ttu-id="4f5ba-278">V **název** textové pole, zadejte název databáze (například *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
    - <span data-ttu-id="4f5ba-279">Na serveru **rozevíracího seznamu** seznamu, vyberte **nové SQL databázový server**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="4f5ba-280">Alternativně můžete vybrat existující server.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-280">Alternatively, you can select an existing server.</span></span>
    - <span data-ttu-id="4f5ba-281">V **uživatelské jméno** a **heslo k databázi** polí zadejte uživatelské jméno správce a heslo pro server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="4f5ba-282">Pokud zvolíte možnost server již jste vytvořili, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-282">If you select a server you have already created, you will be prompted for the password.</span></span>

    ![Zadání nastavení databáze](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    <span data-ttu-id="4f5ba-284">*Zadání nastavení databáze*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="4f5ba-285">Klikněte na tlačítko **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="4f5ba-286">Vyberte **úložiště místní Git** pro zdrojového kódu použít a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-287">Můžete být vyzváni k nasazení pověření (uživatelské jméno a heslo).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Vytvoření úložiště Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="4f5ba-289">*Vytvoření úložiště Git*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="4f5ba-290">Počkejte, dokud se vytvoří nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-291">Ve výchozím nastavení, Azure poskytuje domén v *azurewebsites.net* , ale také vám dává možnost nastavit vlastní domény pomocí portálu správy Azure.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="4f5ba-292">Pokud používáte určité režimy služby Azure App Service však můžete spravovat pouze vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="4f5ba-293">Aplikační služba Azure je k dispozici v edicích Free, sdílené, Basic, Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="4f5ba-294">Všechny webové aplikace v režimu volné a sdílené spustit v prostředí s více klienty a mít kvóty pro využití procesoru, paměti a sítě.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="4f5ba-295">Maximální počet bezplatných aplikací se může lišit s plánu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="4f5ba-296">Ve standardním režimu vyberte, které aplikace spustit ve vyhrazených virtuálních počítačů, které odpovídají na standardní Azure výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="4f5ba-297">Můžete najít v konfiguraci webové aplikace režimu **škálování** nabídky vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="4f5ba-298">![Aplikační služba Azure režimy](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "režimy služby Azure App Service")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="4f5ba-299">Pokud používáte **sdílené** nebo **standardní** režimu, bude možné spravovat vlastní domény pro vaši webovou aplikaci tak, že přejdete do vaší aplikace **konfigurace** nabídky a kliknutím na **Správa domén** pod *názvy domén*.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="4f5ba-300">![Spravovat domény](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "spravovat domény")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="4f5ba-301">![Spravovat vlastní domény](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "správě vlastních domén")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="4f5ba-302">Po vytvoření webové aplikace klikněte na odkaz v části **URL** sloupec zkontrolujte, zda je spuštěna nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![Procházení do nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="4f5ba-304">*Procházení do nové webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-304">*Browsing to the new web app*</span></span>

    ![Webová aplikace spuštěná](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="4f5ba-306">*Webová aplikace spuštěná*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="4f5ba-307">Úloha 2 – Vytvoření provozní databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4f5ba-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="4f5ba-308">V této úloze budete používat **migrace Entity Framework Code First** k vytvoření databáze cílení **Azure SQL Database** instanci jste vytvořili v předchozí úloze.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="4f5ba-309">V portálu pro správu, přejděte na webovou aplikaci, kterou jste vytvořili v předchozí úloze a přejděte do jeho **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="4f5ba-310">V **řídicí panel** klikněte na tlačítko **zobrazit připojovací řetězce** v části **rychlého přehledu** části.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="4f5ba-311">![Zobrazit připojovací řetězce](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "zobrazit připojovací řetězce")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="4f5ba-312">*Zobrazit připojovací řetězce*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-312">*View connection strings*</span></span>
3. <span data-ttu-id="4f5ba-313">Kopírování **připojovací řetězec** hodnotu a zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="4f5ba-314">![Připojovací řetězec v portál pro správu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "připojovací řetězec v portál pro správu Azure")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="4f5ba-315">*Připojovací řetězec v portál pro správu Azure*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="4f5ba-316">Klikněte na tlačítko **databází SQL** zobrazíte seznam databází SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="4f5ba-317">![Databáze SQL nabídky](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "nabídky databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="4f5ba-318">*Databáze SQL nabídky*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="4f5ba-319">Vyhledejte databázi, kterou jste vytvořili v předchozí úloze a klepněte na serveru.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="4f5ba-320">![Databáze SQL serveru](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "databáze SQL serveru")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="4f5ba-321">*Databáze SQL serveru*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-321">*SQL Database server*</span></span>
6. <span data-ttu-id="4f5ba-322">V **rychlý Start** stránka serveru, klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="4f5ba-323">![Konfigurace nabídky](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "konfigurovat nabídky")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="4f5ba-324">*Konfigurace nabídky*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-324">*Configure menu*</span></span>
7. <span data-ttu-id="4f5ba-325">V **povolené IP adresy** části, klikněte na **přidat do povolené IP adresy** odkaz na povolit IP pro připojení k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="4f5ba-326">![Povolené IP adresy](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "povolené IP adresy")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="4f5ba-327">*Povolené IP adresy*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="4f5ba-328">Klikněte na tlačítko **Uložit** v dolní části stránky k dokončení kroku.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="4f5ba-329">Přepněte zpátky na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="4f5ba-330">V **Konzola správce balíčků**, spustit následující příkaz nahrazení *[YOUR PŘIPOJOVACÍHO ŘETĚZCE]* zástupného symbolu připojovacím řetězcem, který jste zkopírovali z Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="4f5ba-331">![Aktualizace databáze cílení na Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aktualizace databáze cílení na Windows Azure SQL Database")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="4f5ba-332">*Aktualizace databáze cílení na Azure SQL Database*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="4f5ba-333">Úloha 3 – nasazení kvízu Geek do přípravy pomocí Git</span><span class="sxs-lookup"><span data-stu-id="4f5ba-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="4f5ba-334">V této úloze povolíte dvoufázové instalace publikování ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="4f5ba-335">Poté použije Git pro publikování aplikace Geek kvízu přímo z místního počítače v pracovní prostředí vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="4f5ba-336">Přejděte zpět na portál a klikněte na název webové aplikace v rámci **název** sloupec pro zobrazení stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Otevírání webových stránek správy aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="4f5ba-338">*Otevírání webových stránek správy aplikace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="4f5ba-339">Přejděte na **škálování** stránky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="4f5ba-340">V části **Obecné** vyberte **standardní** pro konfiguraci a klikněte na **Uložit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-341">Lze spustit všechny webové aplikace v aktuální oblasti a předplatné v **standardní** režimu, ponechejte **Vybrat vše** v zaškrtnuto políčko **vyberte lokality** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="4f5ba-342">Jinak, zrušte **Vybrat vše** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="4f5ba-343">![Upgrade webové aplikace do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "upgrade webové aplikace do standardního režimu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="4f5ba-344">*Upgrade webové aplikace do standardního režimu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="4f5ba-345">Klikněte na tlačítko **Ano** potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="4f5ba-346">![Potvrzení změn do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "pokračováním Změna režimu webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="4f5ba-347">*Potvrzení změn do standardního režimu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="4f5ba-348">Přejděte na **řídicí panel** a klikněte na tlačítko **povolit dvoufázové instalace publikování** pod **rychlého přehledu** části.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="4f5ba-349">![Povolení publikování dvoufázové instalace](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "povolení připravený publikování")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="4f5ba-350">*Povolení publikování dvoufázové instalace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="4f5ba-351">Klikněte na tlačítko **Ano** povolit dvoufázové instalace publikování.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="4f5ba-352">![Potvrzení připravený publikování](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "kliknutím na tlačítko Ano, chcete-li povolit dvoufázové instalace publikování")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="4f5ba-353">*Potvrzení publikování dvoufázové instalace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="4f5ba-354">V seznamu webové aplikace rozbalte značku nalevo od název vaší webové aplikace zobrazíte přípravný slot lokality.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="4f5ba-355">Obsahuje název vaší webové aplikace, za nímž následuje ***(pracovní)***.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="4f5ba-356">Klepněte na pracovní síť, přejděte na stránku správy.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="4f5ba-357">![Navigace na pracovní webovou aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "přejdete na pracovní webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="4f5ba-358">*Přejdete na pracovní aplikace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="4f5ba-359">Všimněte si, že této stránce správy he vypadá stránce management jakékoli jiné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="4f5ba-360">Přejděte na **nasazení** stránky a zkopírujte **adresy URL pro Git** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="4f5ba-361">Použije ho později v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-361">You will use it later in this exercise.</span></span>

    ![Kopírování hodnota adresy URL pro Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="4f5ba-363">*Kopírování hodnota adresy URL pro Git*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="4f5ba-364">Otevřete nový **Git Bash** konzolu a spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="4f5ba-365">Aktualizace *[YOUR cesta aplikace]* zástupný symbol cestu k **GeekQuiz** řešení, které jsou umístěné v **Source\Ex1 DeployingWebSiteToStaging\Begin** složky Toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicializace Git a první potvrzení změn](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="4f5ba-367">*Inicializace Git a první potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="4f5ba-368">Spusťte následující příkaz pro uložení vaší webové aplikace do vzdálených **Git** úložiště.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="4f5ba-369">Nahraďte zástupný symbol s adresou URL, které jste získali z portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="4f5ba-370">Zobrazí se výzva k zadání hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Když zavedete do služby Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="4f5ba-372">*Vkládání do Azure*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-373">Při nasazení obsahu FTP hostitele nebo úložiště GIT webové aplikace, je třeba ověřit, pomocí **přihlašovací údaje nasazení** vytvořený z webové aplikace **rychlý Start** nebo **řídicí panel**  správu stránek.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="4f5ba-374">Pokud si nejste jisti, přihlašovací údaje nasazení je snadno obnovit pomocí portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="4f5ba-375">Otevřete web app **řídicí panel** a klikněte na tlačítko **resetovat přihlašovací údaje nasazení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="4f5ba-376">Zadejte nové heslo a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="4f5ba-377">Přihlašovací údaje nasazení jsou platné pro použití s všechny webové aplikace, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="4f5ba-378">Chcete-li ověřit, webové aplikace byla úspěšně nabídnutých do Azure, přejděte zpět na portálu pro správu a klikněte na tlačítko **weby**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="4f5ba-379">Vyhledejte vaší webové aplikace a rozbalte položku zobrazíte přípravný slot lokality.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="4f5ba-380">Klikněte na jeho **název** přejděte na stránku správy.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="4f5ba-381">Klikněte na tlačítko **nasazení** zobrazíte **historii nasazení**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="4f5ba-382">Ověřte, zda je **aktivní nasazení** s vaší  *&quot;počáteční potvrdit&quot;*.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![Aktivní nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="4f5ba-384">*Aktivní nasazení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-384">*Active deployment*</span></span>
13. <span data-ttu-id="4f5ba-385">Nakonec klikněte na **Procházet** na panelu příkazů na Přejít do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Procházet webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="4f5ba-387">*Procházet webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-387">*Browse web app*</span></span>
14. <span data-ttu-id="4f5ba-388">Pokud je aplikace nasazena úspěšně, zobrazí se stránka Geek kvízu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-389">Adresa URL nasazené aplikace obsahuje název vaší webové aplikace, za nímž následuje *-pracovní*.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![Aplikace běžící v testovacím prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="4f5ba-391">*Aplikace běžící v testovacím prostředí*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="4f5ba-392">Pokud chcete prozkoumat aplikace, klikněte na tlačítko **zaregistrovat** k registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="4f5ba-393">Podrobnosti o účtu dokončete tak, že zadáte uživatelské jméno, e-mailovou adresu a heslo.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="4f5ba-394">V dalším kroku aplikaci ukazuje na první otázku kvízu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="4f5ba-395">Odpovězte na několik otázek a ujistěte se, že funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![Aplikace připravené k použití](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="4f5ba-397">*Aplikace připravené k použití*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="4f5ba-398">Úloha 4 – povýšení webové aplikace do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="4f5ba-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="4f5ba-399">Teď, když si ověříte, že webová aplikace funguje správně v testovacím prostředí, budete chtít přesunout do výroby.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="4f5ba-400">V této úloze bude Prohodit přípravný slot lokality s lokality produkční slot.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="4f5ba-401">Přejděte zpět na portálu pro správu a vyberte přípravný slot lokality.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="4f5ba-402">Klikněte na tlačítko **odkládacího souboru** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-402">Click **Swap** in the command bar.</span></span>

    ![Do ostrého provozu](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="4f5ba-404">*Do ostrého provozu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-404">*Swap to production*</span></span>
2. <span data-ttu-id="4f5ba-405">Klikněte na tlačítko **Ano** v potvrzovacím dialogovém okně, chcete-li pokračovat v provádění této operace prohození.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="4f5ba-406">Azure bude okamžitě odkládacího souboru obsahu pracoviště s obsahem webu pracovní.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-407">Některá nastavení z dvoufázové instalace verze se automaticky zkopírují na produkční verzi (například připojovací řetězec přepsání, mapování obslužných rutin, atd.), ale ostatní nastavení nedojde ke změně (např. DNS koncových bodů, vazby SSL, atd.).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![Potvrzení operace prohození](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="4f5ba-409">*Potvrzení operace prohození*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="4f5ba-410">Po dokončení prohození vyberte produkční slot a klikněte na **Procházet** na panelu příkazů otevřete pracoviště.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="4f5ba-411">Všimněte si adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-412">Možná budete muset aktualizujte webový prohlížeč a vymažte mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="4f5ba-413">V aplikaci Internet Explorer, můžete k tomu stisknutím **CTRL + R**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![Webová aplikace spuštěná v produkčním prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="4f5ba-415">V **Git Bash** konzole, aktualizovat vzdálenou adresu URL pro místní úložiště Git pro produkční slot.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="4f5ba-416">To provedete pomocí následujícího příkazu nahraďte zástupné symboly vaše nasazení uživatelské jméno a název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-417">V následujícím cvičení bude doručte změny do pracoviště místo pracovní jenom pro jednoduchost testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="4f5ba-418">Ve scénáři reálného doporučujeme ověřit změny v testovacím prostředí před zvýšením úrovně do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="4f5ba-419">Cvičení 3: Provedení vrácení nasazení v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="4f5ba-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="4f5ba-420">Existují scénáře, kde jste přípravný slot na Zaměnit aktivní přípravné nebo produkční prostředí, například při práci s **volné** nebo **sdílené** režimu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="4f5ba-421">V těchto scénářích měli byste otestovat svoji aplikaci v testovacím prostředí – místně nebo ve vzdálené lokalitě – před nasazením do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="4f5ba-422">Je však možné, že problém nebyl zjištěn během fáze testování mohou nastat v produkční lokality.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="4f5ba-423">V takovém případě je důležité mít mechanismus snadno přepnout na předchozí a další stabilní verzi aplikace co nejrychleji.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="4f5ba-424">V **Azure App Service**, průběžné nasazování od správy zdrojového kódu umožňuje toto možné díky **znovu nasaďte** akce, které jsou k dispozici v portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="4f5ba-425">Azure uchovává informace o nasazení přiřazená k potvrzení nabídnutých do úložiště a poskytuje možnost znovu nasaďte aplikaci pomocí některé z předchozích nasazení, kdykoli.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="4f5ba-426">V tomto cvičení budete provádět změny kódu v **Geek kvízu** aplikace, která záměrně vloží *chyb*.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="4f5ba-427">Nasadíte aplikace do produkčního prostředí, abyste viděli chybu a pak bude využívat funkci znovu ho zaveďte se vrátíte do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="4f5ba-428">Úloha 1 – aktualizace aplikace Geek kvízu s časovým limitem</span><span class="sxs-lookup"><span data-stu-id="4f5ba-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="4f5ba-429">V této úloze bude Refaktorovat malou část kódu **TriviaController** třídy extrahovat část logiky, která načte možnost vybrané kvízu z databáze do nové metody.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="4f5ba-430">Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení z předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="4f5ba-431">V **Průzkumníku řešení**, otevřete **TriviaController.cs** souboru uvnitř **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="4f5ba-432">Vyhledejte **StoreAsync** metoda a vyberte zvýrazněnou kód na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![Výběr kód](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="4f5ba-434">*Výběr kód*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-434">*Selecting the code*</span></span>
4. <span data-ttu-id="4f5ba-435">Klikněte pravým tlačítkem na vybraný úsek kódu, rozbalte **Refaktorovat** nabídku a vyberte **extrahovat metodu...** .</span><span class="sxs-lookup"><span data-stu-id="4f5ba-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![Extrahování kód jako nové metody](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="4f5ba-437">*Výběr metody extrakce*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="4f5ba-438">V **extrahovat metodu** dialogové okno, název nové metody *MatchesOption* a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![Zadání názvu – metoda](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="4f5ba-440">*Zadejte název pro metodu extrahované*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="4f5ba-441">Vybraný úsek kódu je pak extrahován do **MatchesOption** metoda.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="4f5ba-442">Následující fragment kódu ukazuje výsledný kód.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="4f5ba-443">Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="4f5ba-444">Úloha 2 – opětovného nasazení aplikace Geek kvízu s časovým limitem</span><span class="sxs-lookup"><span data-stu-id="4f5ba-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="4f5ba-445">Nyní jste předá změny, které jste provedli v předchozí úloze úložiště, které aktivují nového nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="4f5ba-446">Potom jste se potíže, k problém pomocí **nástroje pro vývoj F12** poskytované Internet Explorer a potom proveďte vrácení zpět na předchozí nasazení z portálu správy Azure.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="4f5ba-447">Otevřete nový **Git Bash** konzolu k nasazení aktualizované aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="4f5ba-448">Spuštěním následujících příkazů pro uložení změn do Azure.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="4f5ba-449">Aktualizace *[YOUR cesta aplikace]* zástupný symbol cestu k **GeekQuiz** řešení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="4f5ba-450">Zobrazí se výzva k zadání hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Vkládání kódu rozdělili do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="4f5ba-452">*Vkládání kódu rozdělili do Azure*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="4f5ba-453">Otevřete Internet Explorer a přejděte na webovou aplikaci (například `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="4f5ba-454">Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="4f5ba-455">Stiskněte klávesu **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** a klikněte **přehrání** tlačítko spusťte záznam.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="4f5ba-456">![Spouštění sítě záznam](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "od sítě záznam")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="4f5ba-457">*Výchozí záznam sítě*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-457">*Starting network recording*</span></span>
5. <span data-ttu-id="4f5ba-458">Vyberte všechny možnost kvízu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-458">Select any option of the quiz.</span></span> <span data-ttu-id="4f5ba-459">Zobrazí se, že se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="4f5ba-460">V **F12** okně zobrazuje položku odpovídající požadavek POST HTTP protokolu HTTP **500** výsledek.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 Chyba](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="4f5ba-462">*HTTP 500 error*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="4f5ba-463">Vyberte **konzoly** kartě. Podrobnosti o příčině je zaznamenána chyba.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![Chyba protokolu](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="4f5ba-465">*Chyba protokolu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-465">*Logged error*</span></span>
8. <span data-ttu-id="4f5ba-466">Vyhledejte část Podrobnosti o chybě.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-466">Locate the details part of the error.</span></span> <span data-ttu-id="4f5ba-467">Je zřejmé tato chyba je způsobená kódu refaktoringu je potvrzené v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="4f5ba-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="4f5ba-469">Nezavírejte okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-469">Do not close the browser.</span></span>
10. <span data-ttu-id="4f5ba-470">V nové instanci prohlížeče, přejděte na [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="4f5ba-471">Vyberte **weby** a klikněte na webovou aplikaci, které jste vytvořili v cvičení 2.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="4f5ba-472">Přejděte na **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="4f5ba-473">Všimněte si, že všechny potvrzení provést jsou uvedeny v historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![Seznam existující nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="4f5ba-475">*Seznam existující nasazení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="4f5ba-476">Vyberte předchozí potvrzení a klikněte na **znovu nasaďte** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![Opětovné nasazení předchozí potvrzení](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="4f5ba-478">*Opětovné nasazení předchozí potvrzení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="4f5ba-479">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-479">When prompted to confirm, click **Yes**.</span></span>

    ![Potvrzení opakované nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="4f5ba-481">Po dokončení nasazení přepnout zpět do instance prohlížeče s webovou aplikaci a stiskněte klávesu **kombinaci kláves CTRL + F5**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="4f5ba-482">Klikněte na některou z možností.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-482">Click any of the options.</span></span> <span data-ttu-id="4f5ba-483">Flip animace bude trvat teď místní a výsledek (*opravte nesprávné*) se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="4f5ba-484">(Volitelné) Přepnout **Git Bash** konzolu a spusťte následující příkazy se vrátit k předchozí potvrzení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-485">Tyto příkazy vytvořit nové potvrzení změn, které vrátí zpět všechny změny v úložišti Git, které byly provedeny v chybný potvrzení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="4f5ba-486">Azure bude poté znovu nasaďte aplikace pomocí nového potvrzení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="4f5ba-487">Cvičení 4: Škálování používat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="4f5ba-488">**Objekty BLOB** jsou nejjednodušší způsob, jak ukládat velké objemy nestrukturovaných textu nebo binárních dat, jako je například video, zvuk a obrázků.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="4f5ba-489">Přesunutí statický obsah aplikace do úložiště, pomáhá škálování aplikace pomocí poskytování obrázků nebo dokumentů přímo do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="4f5ba-490">V tomto cvičení se přesune statický obsah vaší aplikace na kontejner objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="4f5ba-491">Pak budete konfigurovat aplikace pro přidání **adresy URL technologie ASP.NET, přepište pravidlo** v **Web.config** přesměrovat obsah do kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="4f5ba-492">Úloha 1 – Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="4f5ba-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="4f5ba-493">V této úloze se dozvíte, jak vytvořit nový účet úložiště pomocí portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="4f5ba-494">Přejděte na [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="4f5ba-495">Vyberte **nové | Datové služby | Úložiště | Funkce rychle vytvořit** chcete začít vytvářet nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="4f5ba-496">Zadejte jedinečný název pro účet a vyberte **oblast** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="4f5ba-497">Klikněte na tlačítko **vytvořit účet úložiště** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="4f5ba-498">![Vytvoření nového účtu úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "vytvoření nového účtu úložiště")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="4f5ba-499">*Vytvoření nového účtu úložiště*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="4f5ba-500">V **úložiště** část, počkejte, dokud stav nového účtu úložiště se změní na *Online* než budete pokračovat v následujícím kroku.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="4f5ba-501">![Vytvořit účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "vytvořený účet úložiště")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="4f5ba-502">*Vytvořit účet úložiště*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-502">*Storage Account created*</span></span>
4. <span data-ttu-id="4f5ba-503">Klikněte na název účtu úložiště a pak klikněte na **řídicí panel** odkaz v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="4f5ba-504">**Řídicí panel** stránka poskytuje informace o stavu účet a koncové body služby, které lze použít v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="4f5ba-505">![Zobrazení řídicího panelu účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "zobrazení řídicího panelu účtu úložiště")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="4f5ba-506">*Zobrazení řídicího panelu účtu úložiště*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="4f5ba-507">Klikněte **spravovat přístupové klíče** tlačítka na navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="4f5ba-508">![Správa přístupových klíčů tlačítko](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "tlačítko Spravovat přístupové klíče")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="4f5ba-509">*Správa přístupových klíčů tlačítko*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="4f5ba-510">V **spravovat přístupové klíče** dialogové okno, kopie **název účtu úložiště** a **primární přístupový klíč** jako v následujícím cvičení budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="4f5ba-511">Zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="4f5ba-512">![Dialogové okno Spravovat přístupový klíč](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "dialogové okno Spravovat přístupový klíč")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="4f5ba-513">*Přístupový klíč dialogové okno Spravovat*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="4f5ba-514">Úloha 2 – nahrání prostředek do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4f5ba-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="4f5ba-515">V této úloze budete používat v okně Průzkumníka serveru Visual Studio pro připojení k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="4f5ba-516">Potom vytvořte kontejner objektů blob a nahrát soubor s logem kvízu Geek do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="4f5ba-517">Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení z předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="4f5ba-518">V řádku nabídek vyberte **zobrazení** a pak klikněte na **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="4f5ba-519">V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **Azure** uzel a vyberte možnost **připojit k Azure...** . Přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Připojení k systému Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="4f5ba-521">*Připojit k Azure*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="4f5ba-522">Rozbalte **Azure** uzel, klikněte pravým tlačítkem na **úložiště** a vyberte **připojit externí úložiště...** .</span><span class="sxs-lookup"><span data-stu-id="4f5ba-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="4f5ba-523">V **přidat nový účet úložiště** dialogovém okně zadejte **název účtu** a **klíč účtu** jste získali v předchozí úloze a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![Přidat dialogové okno Nový účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="4f5ba-525">*Přidat dialogové okno Nový účet úložiště*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="4f5ba-526">V dialogovém okně účtu úložiště **úložiště** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="4f5ba-527">Rozbalte účtu úložiště, klikněte pravým tlačítkem na **objekty BLOB** a vyberte **vytvořit kontejner objektů Blob...** .</span><span class="sxs-lookup"><span data-stu-id="4f5ba-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="4f5ba-528">![Vytvoření kontejneru objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "vytvořit kontejner objektů Blob")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="4f5ba-529">*Vytvoření kontejneru objektů Blob*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="4f5ba-530">V **vytvořit kontejner objektů Blob** dialogovém okně zadejte název pro kontejner objektů blob a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="4f5ba-531">![Dialogové okno vytvořit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "dialogové okno vytvořit kontejner objektů Blob")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="4f5ba-532">*Kontejner objektů Blob dialogové okno vytvořit*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="4f5ba-533">Nový kontejner objektu blob musí být přidaní do **objekty BLOB** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="4f5ba-534">Změňte přístupová oprávnění v kontejneru zveřejnit kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="4f5ba-535">Chcete-li to provést, klikněte pravým tlačítkem **bitové kopie** kontejneru a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="4f5ba-536">![vlastnosti kontejneru image](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Image vlastnosti kontejneru")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="4f5ba-537">*Vlastnosti kontejneru bitové kopie*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-537">*Images container properties*</span></span>
9. <span data-ttu-id="4f5ba-538">V **vlastnosti** nastavte **veřejný přístup pro čtení** k **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="4f5ba-539">![Změna vlastnosti veřejný přístup pro čtení](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Změna vlastnosti veřejný přístup pro čtení")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="4f5ba-540">*Změna vlastnosti veřejný přístup pro čtení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="4f5ba-541">Po zobrazení výzvy, pokud jste si jistí, kterou chcete změnit vlastnost veřejný přístup, klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="4f5ba-542">![Microsoft Visual Studio upozornění](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "upozornění Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="4f5ba-543">*Microsoft Visual Studio upozornění*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="4f5ba-544">V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **bitové kopie** kontejner objektů blob a vyberte **kontejner objektů Blob zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="4f5ba-545">![Zobrazit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "zobrazení kontejneru objektů Blob")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="4f5ba-546">*Kontejner objektů Blob zobrazení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-546">*View Blob Container*</span></span>
12. <span data-ttu-id="4f5ba-547">Kontejner imagí by měla otevřít v novém okně a by se měly zobrazit Legenda s žádné položky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="4f5ba-548">Klikněte **nahrát** ikonu pro nahrání souboru do kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="4f5ba-549">![Kontejner imagí s žádné položky](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "kontejner Imagí s žádné položky")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="4f5ba-550">*Kontejner imagí s žádné položky*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="4f5ba-551">V **nahrát objekt Blob** dialogové okno pole, přejděte **prostředky** složky prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="4f5ba-552">Vyberte **logo big.png** souboru a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="4f5ba-553">Počkejte, až se soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="4f5ba-554">Po dokončení nahrávání souboru by měl být uvedený v kontejneru bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="4f5ba-555">Pravým tlačítkem na položku soubor a vyberte **kopie URL**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="4f5ba-556">![Zkopírujte adresu URL objektů blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "zkopírujte adresu URL souboru objektů blob")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="4f5ba-557">*Zkopírujte adresu URL objektů blob*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="4f5ba-558">Otevřete Internet Explorer a vložte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="4f5ba-559">Na následujícím obrázku se mají v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="4f5ba-560">![Obrázek loga big.png z úložiště objektů Blob Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo big.png bitovou kopii z úložiště")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="4f5ba-561">*Obrázek loga big.png z Azure Blob Storage*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="4f5ba-562">Úloha 3 – aktualizace řešení využívají statický obsah z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4f5ba-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="4f5ba-563">V této úloze nakonfigurujete **GeekQuiz** řešení využívat image nahrané do Azure Blob Storage (namísto bitovou kopii umístěné ve webové aplikaci) tak, že přidáte pravidlo přepsání adresy URL technologie ASP.NET v **web.config**souboru.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="4f5ba-564">V sadě Visual Studio, otevřete **Web.config** souboru uvnitř **GeekQuiz** projektu a najděte  **&lt;system.webServer&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="4f5ba-565">Přidejte následující kód do přidat adresu URL přepisování pravidlo, aktualizace zástupného textu s názvem svého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="4f5ba-566">(Code fragment kódu - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="4f5ba-567">Přepisování adres URL je proces zachycení příchozí webové žádosti a požadavek přesměrování na jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="4f5ba-568">Přepisování pravidel adres URL informuje třeba přepisovat modul, když potřebuje žádost o přesměrování a kde by měl být přesměrován.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="4f5ba-569">Třeba přepisovat pravidlo se skládá ze dvou řetězců: vzor hledání v požadovanou adresu URL (obvykle s použitím regulárních výrazů), a řetězec, který má nahradit vzor, pokud nalezen.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="4f5ba-570">Další informace najdete v tématu [přepisování adres URL technologie ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="4f5ba-571">Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="4f5ba-572">Otevřete nový **Git Bash** konzolu k nasazení aktualizované aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="4f5ba-573">Spuštěním následujících příkazů pro uložení změn do Azure.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="4f5ba-574">Aktualizace *[YOUR cesta aplikace]* zástupný symbol cestu k **GeekQuiz** řešení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="4f5ba-575">Zobrazí se výzva k zadání hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Nasazení aktualizací do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="4f5ba-577">*Nasazení aktualizací do Azure*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="4f5ba-578">Úloha 4 – ověření</span><span class="sxs-lookup"><span data-stu-id="4f5ba-578">Task 4 – Verification</span></span>

<span data-ttu-id="4f5ba-579">V této úloze budete používat **Internet Explorer** a přejděte **Geek kvízu** aplikaci a zkontrolujte, že adresa URL přepsat pravidlo pro funguje bitové kopie a zprávy jsou přesměrovány na bitovou kopii hostované na **objektů Blob v Azure Úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="4f5ba-580">Otevřete Internet Explorer a přejděte na webovou aplikaci (například `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="4f5ba-581">Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="4f5ba-582">![Zobrazuje webovou aplikaci Geek kvízu s bitovou kopii](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "zobrazující webovou aplikaci Geek kvízu s bitovou kopii")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="4f5ba-583">*Zobrazuje webovou aplikaci Geek kvízu s obrázkem*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="4f5ba-584">Stiskněte klávesu **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** kartě a spusťte záznam.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="4f5ba-585">![Spouštění sítě záznam](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "od sítě záznam")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="4f5ba-586">*Výchozí záznam sítě*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-586">*Starting network recording*</span></span>
3. <span data-ttu-id="4f5ba-587">Stiskněte klávesu **kombinaci kláves CTRL + F5** aktualizovat webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="4f5ba-588">Po dokončení načítání stránky, měli byste vidět požadavku HTTP pro **/img/logo-big.png** adresu URL pomocí protokolu HTTP **301** výsledek (přesměrování) a jiným požadavkem na `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adresu URL s HTTP **200** výsledek.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="4f5ba-589">![Ověřování adresy URL přesměrování](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "zobrazující přesměrování v nástrojích pro vývojáře")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="4f5ba-590">*Ověřování adresy URL přesměrování*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="4f5ba-591">Cvičení 5: Pomocí automatického škálování pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="4f5ba-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="4f5ba-592">Tento postup je volitelná, protože vyžaduje podporu pro zatížení webové &amp; testování výkonu, která je dostupná jenom pro **Visual Studio 2013 Ultimate Edition**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="4f5ba-593">Další informace o konkrétní funkce sady Visual Studio 2013, porovnat verze [zde](https://www.microsoft.com/visualstudio/eng/products/compare).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="4f5ba-594">**Azure App Service Web Apps** poskytuje funkci automatického škálování pro webové aplikace běžící **standardní režim**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="4f5ba-595">Škálování umožňuje Azure automaticky škálovat počet instancí vaší webové aplikace v závislosti na zatížení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="4f5ba-596">Pokud je povoleno automatické škálování, Azure kontroluje procesoru vaší webové aplikace každých pět minut a přidá instancí podle potřeby v tomto bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="4f5ba-597">Pokud je nízké využití procesoru, Azure instance budou odstraněny, každé dvě hodiny zajistit, že není ke snížení výkonu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="4f5ba-598">V tomto cvičení jste projít kroky nutné ke konfiguraci **škálování** funkcí pro **Geek kvízu** webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="4f5ba-599">Tato funkce ověříte spuštěním zátěžového testu sady Visual Studio k vygenerování dostatek zatížení procesoru v aplikaci k aktivaci instance upgrade.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="4f5ba-600">Úloha 1 – konfigurace automatického škálování podle metriky využití procesoru</span><span class="sxs-lookup"><span data-stu-id="4f5ba-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="4f5ba-601">V této úloze budete používat portál pro správu Azure k povolení této funkce automatického škálování pro webovou aplikaci, kterou jste vytvořili v cvičení 2.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="4f5ba-602">V [portál pro správu Azure](https://manage.windowsazure.com/), vyberte **weby** a klikněte na webovou aplikaci, které jste vytvořili v cvičení 2.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="4f5ba-603">Přejděte na **škálování** stránky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="4f5ba-604">V části **kapacity** vyberte **procesoru** pro **škálování podle metriky** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-605">Při zvětšení velikosti podle využití procesoru, Azure dynamicky upraví počet instancí, které aplikace používá, pokud se změní využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="4f5ba-606">![Výběr škálování podle využití procesoru](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "výběrem metriky využití procesoru pro automatické škálování")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="4f5ba-607">*Výběr škálování podle využití procesoru*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="4f5ba-608">Změna **cílový procesor** konfigurace **20**-**40** procent.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-609">Tento rozsah představuje průměrné využití procesoru pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="4f5ba-610">Azure přidá nebo odebere instance, které chcete zachovat vaší webové aplikace v tomto rozsahu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="4f5ba-611">Minimální a maximální počet instancí použitý pro škálování je zadána v **počet instancí** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="4f5ba-612">Azure nikdy přejde nad nebo překračuje tento limit.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-612">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="4f5ba-613">Výchozí hodnota **cílový procesor** hodnoty jsou změnit jenom pro účely tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="4f5ba-614">Konfigurace rozsahu procesoru malé hodnoty, jsou zvýšit pravděpodobnost k aktivační události škálování střední zatížení při umístění na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="4f5ba-615">![Změna cílové procesoru být až 20, 40 procent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "změna cíl procesoru, který má být až 20, 40 procent")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="4f5ba-616">*Změna cílové procesoru být až 20, 40 procent*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="4f5ba-617">Klikněte na tlačítko **Uložit** na panelu příkazů a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="4f5ba-618">Úloha 2 – zatížení testování v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5ba-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="4f5ba-619">Teď, když **škálování** byl nakonfigurován, vytvoříte **výkonu webu a spouštění testovacího projektu** v sadě Visual Studio k vygenerování některých zatížení procesoru na vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="4f5ba-620">Otevřete **Visual Studio Ultimate 2013** a vyberte **souboru | Nové | Projekt...**  zahájíte nové řešení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="4f5ba-621">![Vytvoření nového projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "vytvoření nového projektu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="4f5ba-622">*Vytvoření nového projektu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-622">*Creating a new project*</span></span>
2. <span data-ttu-id="4f5ba-623">V **nový projekt** dialogové okno, vyberte **výkonu webu a zatížení testovacího projektu** pod **Visual C# | Test** kartě. Zajistěte, aby **rozhraní .NET Framework 4.5** je vybrána, název projektu *WebAndLoadTestProject*, vyberte **umístění** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="4f5ba-624">![Vytvoření nového projektu webu a zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "vytvoření nového projektu webu a zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="4f5ba-625">*Vytvoření nového projektu webu a zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="4f5ba-626">V **WebTest1.webtest** klikněte pravým tlačítkem myši **WebTest1** uzel a klikněte na tlačítko **přidat požadavku**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="4f5ba-627">![Žádost o přidání do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "žádost o přidání do WebTest1")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="4f5ba-628">*Žádost o přidání do WebTest1*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="4f5ba-629">V **vlastnosti** okno uzlu novou žádost o aktualizaci **Url** vlastnost tak, aby odkazoval na adresu URL webové aplikace (například  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="4f5ba-630">![Změna vlastnosti adresy Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Změna vlastnosti adresy Url")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="4f5ba-631">*Změna vlastnosti adresy Url*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="4f5ba-632">V **WebTest1.webtest** okna, klikněte pravým tlačítkem na **WebTest1** a klikněte na tlačítko **přidání smyčky...** .</span><span class="sxs-lookup"><span data-stu-id="4f5ba-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="4f5ba-633">![Přidání smyčky do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "přidání smyčky do WebTest1")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="4f5ba-634">*Přidání smyčky do WebTest1*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="4f5ba-635">V **přidat podmíněné pravidlo a položek smyčky** dialogové okno, vyberte **cyklu For** pravidla a upravit následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

    1. <span data-ttu-id="4f5ba-636">**Hodnota se ukončuje:** 1000</span><span class="sxs-lookup"><span data-stu-id="4f5ba-636">**Terminating value:** 1000</span></span>
    2. <span data-ttu-id="4f5ba-637">**Název parametru kontextu:** iterátorů</span><span class="sxs-lookup"><span data-stu-id="4f5ba-637">**Context Parameter Name:** Iterator</span></span>
    3. <span data-ttu-id="4f5ba-638">**Hodnota přírůstku:** 1</span><span class="sxs-lookup"><span data-stu-id="4f5ba-638">**Increment Value:** 1</span></span>

    <span data-ttu-id="4f5ba-639">![Vyberte toto pravidlo pro smyčky a aktualizace vlastností](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "vyberte toto pravidlo pro smyčky a aktualizace vlastností")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

    <span data-ttu-id="4f5ba-640">*Vyberte toto pravidlo pro smyčky a aktualizace vlastností*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="4f5ba-641">V části **položky ve smyčce** vyberte žádosti, které jste předtím vytvořili jako první a poslední položku smyčky.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="4f5ba-642">Klikněte na tlačítko **OK** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="4f5ba-643">![Výběr položek první a poslední smyčky](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "výběr položek první a poslední smyčky")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="4f5ba-644">*Výběr položek první a poslední smyčky*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="4f5ba-645">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **WebAndLoadTestProject** projektu, rozbalte **přidat** nabídku a vyberte **zátěžový Test...** .</span><span class="sxs-lookup"><span data-stu-id="4f5ba-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="4f5ba-646">![Přidání zátěžového testu do projektu WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "přidání do projektu WebAndLoadTestProject zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="4f5ba-647">*Přidání do projektu WebAndLoadTestProject zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="4f5ba-648">V **nového Průvodce zátěžovým testem** dialogové okno, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="4f5ba-649">![Nového Průvodce zátěžovým testem](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "nového Průvodce zátěžovým testem")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="4f5ba-650">*Nového Průvodce zátěžovým testem*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="4f5ba-651">V **scénář** vyberte **nepoužívejte časů přemýšlení** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="4f5ba-652">![Zvolíte nepoužívat časů přemýšlení](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "zvolíte nepoužívat časů přemýšlení")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="4f5ba-653">*Výběr nepoužívat časů přemýšlení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="4f5ba-654">V **vzor zatížení** stránky, ujistěte se, že **konstantní zatížení** je vybraná možnost.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="4f5ba-655">Změna **počet uživatelů** nastavení **250** uživatele a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="4f5ba-656">![Změna počtu uživatelů na 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Změna počtu uživatelů na 250")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="4f5ba-657">*Změna počtu uživatelů na 250*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="4f5ba-658">V **Model kombinace testů** vyberte **podle pořadí sekvenční test** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="4f5ba-659">![Výběr model kombinace testů](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "výběr model kombinace testů")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="4f5ba-660">*Výběr model kombinace testů*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="4f5ba-661">V **Model kombinace testů** klikněte na tlačítko **přidat...**  přidat test s různými.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="4f5ba-662">![Přidání testu do poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "přidávání testu do poměru testů")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="4f5ba-663">*Přidání testu do poměru testů*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="4f5ba-664">V **přidat testy** dialogové okno, dvakrát klikněte na **WebTest1** přidat test **vybrané testy** seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="4f5ba-665">Klikněte na tlačítko **OK** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="4f5ba-666">![Přidání WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "přidání WebTest1 testu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="4f5ba-667">*Přidání WebTest1 testu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="4f5ba-668">Zpět v **poměru testů** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="4f5ba-669">![Dokončení stránce poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "dokončení stránce poměru testů")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="4f5ba-670">*Dokončení stránce poměru testů*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="4f5ba-671">V **kombinace sítí** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="4f5ba-672">![Kliknutím na další na stránce poměr sítí](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "kliknutím další na stránce poměr sítí")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="4f5ba-673">*Kliknutím na tlačítko Další na stránce poměr sítí*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="4f5ba-674">V **prohlížeče kombinaci** vyberte **Internet Explorer 10.0** jako typ prohlížeče a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="4f5ba-675">![Výběr typu prohlížeče](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "výběr typu prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="4f5ba-676">*Výběr typu prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="4f5ba-677">V **sad čítačů** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="4f5ba-678">![Kliknutím na tlačítko Další na stránce sad čítačů](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "kliknutím na tlačítko Další na stránce sady čítačů")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="4f5ba-679">*Kliknutím na tlačítko Další na stránce sady čítačů*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="4f5ba-680">V **spustit nastavení** nastavte **doba trvání testu zatížení** k **5 minut** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="4f5ba-681">![Nastavení Doba trvání testu zatížení na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "nastavení Doba trvání testu zatížení na 5 minut")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="4f5ba-682">*Nastavení Doba trvání testu zatížení na 5 minut*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="4f5ba-683">V **Průzkumníku řešení**, dvakrát klikněte **Local.settings** souboru prozkoumat nastavení testu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="4f5ba-684">Ve výchozím nastavení používá Visual Studio místního počítače ke spuštění testů.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-685">Alternativně můžete nakonfigurovat projekt test spouštění zátěžových testů v cloudu pomocí **Visual Studio Online (VSO)**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="4f5ba-686">VSO poskytuje cloudové zátěžové testování služba, která simuluje realističtější zatížení, zabraňující omezení místní prostředí jako kapacity procesoru, dostupné paměti a šířky pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-686">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="4f5ba-687">Další informace o spouštění zátěžových testů pomocí VSO najdete v tématu [v tomto článku](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-687">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![Nastavení testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="4f5ba-689">Úloha 3 – ověření škálování</span><span class="sxs-lookup"><span data-stu-id="4f5ba-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="4f5ba-690">Nyní se provést zátěžový test, který jste vytvořili v předchozí úloze a najdete v části chování vaší webové aplikace zatížení.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="4f5ba-691">V **Průzkumníku řešení**, dvakrát klikněte na **LoadTest1.loadtest** otevřete zátěžový test.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="4f5ba-692">![Otevírání LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "otevírání LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="4f5ba-693">*Opening LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="4f5ba-694">V **LoadTest1.loadtest** okně klikněte na první tlačítko panelu nástrojů ke spuštění zátěžového testu.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="4f5ba-695">![Spuštění zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "spuštění zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="4f5ba-696">*Spuštění zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-696">*Running the load test*</span></span>
3. <span data-ttu-id="4f5ba-697">Počkejte na dokončení zátěžový test.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-698">Zátěžový test simuluje více uživatelů, kteří souběžně odesílat žádosti do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="4f5ba-699">Po spuštění testu, můžete monitorovat dostupné čítače zjistit případné chyby, upozornění a další informace související s vaší zátěžový test, spustit.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="4f5ba-700">![Zátěžový test systémem](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "čeká, až do dokončení zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="4f5ba-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="4f5ba-701">*Spuštění zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-701">*Load test running*</span></span>
4. <span data-ttu-id="4f5ba-702">Po dokončení testu, přejděte zpět na portálu pro správu a přejděte do **škálování** stránku vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="4f5ba-703">V části **kapacity** část, měli byste vidět v grafu, byla automaticky nasadit novou instanci.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![Nová instance automatické nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="4f5ba-705">*Nová instance automatické nasazení*</span><span class="sxs-lookup"><span data-stu-id="4f5ba-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="4f5ba-706">Může trvat několik minut, než změny zobrazit v grafu (stiskněte **kombinaci kláves CTRL + F5** pravidelně obnovíte stránku).</span><span class="sxs-lookup"><span data-stu-id="4f5ba-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="4f5ba-707">Pokud nevidíte všechny změny, můžete vyzkoušet tyto věci:</span><span class="sxs-lookup"><span data-stu-id="4f5ba-707">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="4f5ba-708">Prodloužit dobu trvání testu zatížení (například k **10 minut**)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="4f5ba-709">Omezit maximální a minimální hodnoty **cílový procesor** rozsah v konfiguraci automatického škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="4f5ba-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="4f5ba-710">Spuštění zátěžového testu v cloudu s **Visual Studio Online**.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-710">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="4f5ba-711">Další informace [sem](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="4f5ba-711">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="4f5ba-712">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4f5ba-712">Summary</span></span>

<span data-ttu-id="4f5ba-713">V tomto testovacím prostředí praktických jste zjistili, jak nastavit a nasadit aplikaci na produkční webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="4f5ba-714">Spuštění zjišťování a aktualizace vašich databází pomocí **migrace Entity Framework Code First**, pak dál nasazením nové verze vaší lokality pomocí **Git** a provádění vracení zpět, abyste nejnovější stabilní verze vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="4f5ba-715">Kromě toho jste zjistili, jak škálování aplikace pomocí úložiště přesunout statický obsah do kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="4f5ba-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
