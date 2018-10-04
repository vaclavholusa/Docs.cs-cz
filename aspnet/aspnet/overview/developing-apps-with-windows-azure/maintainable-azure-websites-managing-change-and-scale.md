---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Praktické cvičení: udržitelné weby Azure: Správa změn a škálování | Dokumentace Microsoftu'
author: rick-anderson
description: V tomto testovacím prostředí zjistěte, jak Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 05181ae1b2d857eea45983d378b28011c1cd755a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578130"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="e916f-103">Praktické cvičení: udržitelné weby Azure: Správa změn a škálování</span><span class="sxs-lookup"><span data-stu-id="e916f-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="e916f-104">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e916f-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e916f-105">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="e916f-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="e916f-106">Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="e916f-107">Ale nebyly provedeny, když vaše aplikace je v provozu, je právě začínáte!</span><span class="sxs-lookup"><span data-stu-id="e916f-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="e916f-108">Budete potřebovat pro zpracování se měnící požadavky, aktualizace databáze, škálování a další.</span><span class="sxs-lookup"><span data-stu-id="e916f-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="e916f-109">Naštěstí služby Azure App Service vám kryje záda, spoustou funkcí, které vám pomůže ochránit vaše weby, které běží plynule.</span><span class="sxs-lookup"><span data-stu-id="e916f-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
>
> <span data-ttu-id="e916f-110">Azure nabízí bezpečného a flexibilního vývoje, možnosti nasazení a škálování webových aplikací libovolné velikosti.</span><span class="sxs-lookup"><span data-stu-id="e916f-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="e916f-111">Pomocí svých stávajících nástrojů k vytváření a nasazování aplikací bez starostí o správu infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e916f-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
>
> <span data-ttu-id="e916f-112">Zřiďte provozní webové aplikace si během několika minut a snadno nasadit obsah vytvořený pomocí svůj oblíbený vývojový nástroj.</span><span class="sxs-lookup"><span data-stu-id="e916f-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="e916f-113">Můžete nasadit existující lokalitu přímo ze správy zdrojových kódů s podporou **Git**, **Githubu**, **Bitbucket**, **TFS**a dokonce i  **DropBox**.</span><span class="sxs-lookup"><span data-stu-id="e916f-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="e916f-114">Nasazení přímo z vašeho oblíbeného prostředí IDE nebo skripty s využitím **PowerShell** ve Windows nebo **rozhraní příkazového řádku** nástroje spuštěná v jakémkoli operačním systému.</span><span class="sxs-lookup"><span data-stu-id="e916f-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="e916f-115">Po nasazení, informujte webů neustále o podporu pro průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="e916f-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
>
> <span data-ttu-id="e916f-116">Azure poskytuje škálovatelné, odolné cloudové úložiště, zálohu a řešení obnovy pro jakákoli data, libovolném objemu.</span><span class="sxs-lookup"><span data-stu-id="e916f-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="e916f-117">Při nasazování aplikací do produkčního prostředí, služeb úložiště, jako například tabulky, objekty BLOB a databází SQL, můžete škálovat aplikaci v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e916f-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
>
> <span data-ttu-id="e916f-118">U databází SQL je potřeba aktualizovat databázi produktivní při nasazování nové verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="e916f-119">K **migrace Entity Framework Code First**, vývoj a nasazení modelu dat zjednodušili jsme se aktualizovat vaše prostředí během několika minut.</span><span class="sxs-lookup"><span data-stu-id="e916f-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="e916f-120">Tato praktická cvičení se dozvíte, dalších tématech, které by mohly nastat při nasazení vaší webové aplikace do produkčního prostředí v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
>
> <span data-ttu-id="e916f-121">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e916f-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
>
> <span data-ttu-id="e916f-122">Další podrobné pokrytí v tomto tématu najdete v článku [vytváření skutečných cloudových aplikací s Azure e kniha](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e916f-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="e916f-123">Přehled</span><span class="sxs-lookup"><span data-stu-id="e916f-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e916f-124">Cíle</span><span class="sxs-lookup"><span data-stu-id="e916f-124">Objectives</span></span>

<span data-ttu-id="e916f-125">V této praktická cvičení se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="e916f-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="e916f-126">Povolení migrace rozhraní Entity Framework s existující model</span><span class="sxs-lookup"><span data-stu-id="e916f-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="e916f-127">Aktualizovat objektový model a databáze pomocí migrace rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e916f-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="e916f-128">Nasazení do Azure App Service pomocí Gitu</span><span class="sxs-lookup"><span data-stu-id="e916f-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="e916f-129">Vrátit zpět na předchozí nasazení pomocí portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="e916f-130">Škálování webové aplikace pomocí služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e916f-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="e916f-131">Konfigurace automatického škálování pro webovou aplikaci pomocí portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="e916f-132">Vytvoření a konfigurace projektu zátěžového testu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e916f-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e916f-133">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e916f-133">Prerequisites</span></span>

<span data-ttu-id="e916f-134">K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:</span><span class="sxs-lookup"><span data-stu-id="e916f-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="e916f-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="e916f-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="e916f-136">Sada Azure SDK for .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="e916f-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="e916f-137">Systém správy verzí GIT</span><span class="sxs-lookup"><span data-stu-id="e916f-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="e916f-138">Předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-138">A Microsoft Azure subscription</span></span>

    - <span data-ttu-id="e916f-139">Zaregistrovat [bezplatnou zkušební verzi](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="e916f-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="e916f-140">Pokud jste Visual Studio Professional, Test Professional, Premium nebo Ultimate s MSDN nebo MSDN Platforms odběratele, aktivovat váš [výhodu MSDN](http://aka.ms/watk-msdn) hned a začít s vývojem a testování v Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="e916f-141">[BizSpark](http://aka.ms/watk-bizspark) členové automaticky obdrží Azure benefit prostřednictvím svého Visual Studia Ultimate s předplatným MSDN</span><span class="sxs-lookup"><span data-stu-id="e916f-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="e916f-142">Členové [programu Microsoft Partner Network](http://aka.ms/watk-mpn) programu Cloud Essentials získáte měsíčně kredity Azure ve výši</span><span class="sxs-lookup"><span data-stu-id="e916f-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e916f-143">Instalace</span><span class="sxs-lookup"><span data-stu-id="e916f-143">Setup</span></span>

<span data-ttu-id="e916f-144">Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="e916f-145">Otevřete Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.</span><span class="sxs-lookup"><span data-stu-id="e916f-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="e916f-146">Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a nainstalujte Visual Studio fragmenty kódu pro toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="e916f-147">Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e916f-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="e916f-148">Ujistěte se, že jste zaškrtli všechny závislosti pro toto testovací prostředí před spuštěním instalace.</span><span class="sxs-lookup"><span data-stu-id="e916f-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="e916f-149">Používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="e916f-149">Using the Code Snippets</span></span>

<span data-ttu-id="e916f-150">V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="e916f-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="e916f-151">Pro usnadnění práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který se dá dostat z v rámci Visual Studio 2013, abyste ho nemuseli znovu přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="e916f-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="e916f-152">Každý cvičení se sadou počáteční řešení nachází v **začít** složky výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="e916f-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="e916f-153">Uvědomte si, že chybí z těchto řešení od fragmenty kódu, které se přidávají během cvičení a nemusí fungovat, dokud nedokončíte výkonu.</span><span class="sxs-lookup"><span data-stu-id="e916f-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="e916f-154">Uvnitř zdrojový kód pro cvičení, můžete také najdete **End** složku, která obsahuje řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení.</span><span class="sxs-lookup"><span data-stu-id="e916f-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="e916f-155">Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.</span><span class="sxs-lookup"><span data-stu-id="e916f-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e916f-156">Cvičení</span><span class="sxs-lookup"><span data-stu-id="e916f-156">Exercises</span></span>

<span data-ttu-id="e916f-157">Toto praktické testovací prostředí obsahuje následující praktická cvičení:</span><span class="sxs-lookup"><span data-stu-id="e916f-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e916f-158">Použití migrace Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e916f-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="e916f-159">Nasazení webové aplikace do přípravného prostředí</span><span class="sxs-lookup"><span data-stu-id="e916f-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="e916f-160">V produkčním prostředí provádí vrácení nasazení zpět</span><span class="sxs-lookup"><span data-stu-id="e916f-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="e916f-161">Škálování s využitím služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e916f-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="e916f-162">[Použití automatického škálování pro Web Apps](#Exercise5) (volitelné pro Visual Studio 2013 Ultimate edition)</span><span class="sxs-lookup"><span data-stu-id="e916f-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="e916f-163">Odhadovaný čas dokončení tohoto testovacího prostředí: **75 minut**</span><span class="sxs-lookup"><span data-stu-id="e916f-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="e916f-164">Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce.</span><span class="sxs-lookup"><span data-stu-id="e916f-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="e916f-165">Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e916f-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="e916f-166">Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce.</span><span class="sxs-lookup"><span data-stu-id="e916f-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="e916f-167">Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="e916f-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="e916f-168">Cvičení 1: Použití migrace Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e916f-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="e916f-169">Při vývoji aplikace může časem měnit datový model.</span><span class="sxs-lookup"><span data-stu-id="e916f-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="e916f-170">Tyto změny mohou ovlivnit existující model v databázi (Pokud vytváříte novou verzi) a je potřeba aktualizovat databázi zabráníte tak chybám.</span><span class="sxs-lookup"><span data-stu-id="e916f-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="e916f-171">Pro zjednodušení sledování tyto změny v modelu, **migrace Entity Framework Code First** automaticky zjišťuje změny porovnání modelu pomocí schématu databáze a generuje konkrétní kód k aktualizaci databáze, vytváří se nová *verze* vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="e916f-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="e916f-172">V tomto cvičení se dozvíte, jak povolit **migrace** pro vaše aplikace a jak můžete snadno zjišťovat a generovat změny k aktualizaci databází.</span><span class="sxs-lookup"><span data-stu-id="e916f-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="e916f-173">Úloha 1 – povolení migrace</span><span class="sxs-lookup"><span data-stu-id="e916f-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="e916f-174">V této úloze bude projít kroky povolení **migrace Entity Framework Code First** k **kvíz Informatik** databáze, změna modelu a vysvětlení, jak se tyto změny projeví v databáze.</span><span class="sxs-lookup"><span data-stu-id="e916f-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="e916f-175">Otevřete Visual Studio a otevřete **GeekQuiz.sln** soubor řešení od **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.</span><span class="sxs-lookup"><span data-stu-id="e916f-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="e916f-176">Sestavte řešení, aby bylo možné stáhnout a nainstalovat **NuGet** balíček závislostí.</span><span class="sxs-lookup"><span data-stu-id="e916f-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="e916f-177">Chcete-li to provést, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **sestavit řešení** nebo stiskněte klávesu **Ctrl + Shift + B**.</span><span class="sxs-lookup"><span data-stu-id="e916f-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="e916f-178">Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e916f-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="e916f-179">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e916f-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="e916f-180">Vytvoří se při počáteční migraci na základě existující modelu.</span><span class="sxs-lookup"><span data-stu-id="e916f-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="e916f-181">![Povolení migrace](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "povolení migrace")</span><span class="sxs-lookup"><span data-stu-id="e916f-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="e916f-182">*Povolení migrace*</span><span class="sxs-lookup"><span data-stu-id="e916f-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-183">Tento příkaz přidá **migrace** složky do projektu se Informatik kvíz, který obsahuje soubor s názvem **Configuration.cs**.</span><span class="sxs-lookup"><span data-stu-id="e916f-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="e916f-184">**Konfigurace** třída umožňuje konfigurovat chování migrace pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="e916f-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="e916f-185">Pomocí migrace povolená, je potřeba aktualizovat **konfigurace** třídy k naplnění databáze s počáteční data, která **kvíz Informatik** vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="e916f-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="e916f-186">V části **migrace**, nahraďte **Configuration.cs** soubor s tím, který se nachází v **Source\Assets** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-187">Protože **migrace** zavolá **počáteční hodnoty** metoda při každé aktualizaci databáze, je třeba mít jistotu, že nejsou duplicitní záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="e916f-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="e916f-188">**AddOrUpdate** metoda se pomůže zabránit duplicitní data.</span><span class="sxs-lookup"><span data-stu-id="e916f-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="e916f-189">Pokud chcete přidat počáteční migraci, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e916f-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-190">Ujistěte se, že neexistuje žádná databáze s názvem &quot;GeekQuizProd&quot; ve vaší instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e916f-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="e916f-191">![Přidání základní schéma migrace](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "přidání základní schéma migrace")</span><span class="sxs-lookup"><span data-stu-id="e916f-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="e916f-192">*Přidání základního schématu migrace*</span><span class="sxs-lookup"><span data-stu-id="e916f-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-193">**Přidejte migraci** bude generování uživatelského rozhraní další migrace na základě změn, které jste provedli pro váš model, od vytvoření posledního migrace.</span><span class="sxs-lookup"><span data-stu-id="e916f-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="e916f-194">V takovém případě je to první migraci projekt, přidá skripty k vytvoření všech tabulek, které jsou definovány v **TriviaContext** třídy.</span><span class="sxs-lookup"><span data-stu-id="e916f-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="e916f-195">Spusťte migraci k aktualizaci databáze spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="e916f-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="e916f-196">Pro tento příkaz můžete zadat **Verbose** příznak, chcete-li zobrazit příkazy SQL, zavádí do cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="e916f-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="e916f-197">![Vytvoření první databáze](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "vytváření počáteční databáze.")</span><span class="sxs-lookup"><span data-stu-id="e916f-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="e916f-198">*Vytvoření první databáze*</span><span class="sxs-lookup"><span data-stu-id="e916f-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-199">**Aktualizace databáze** se použije čekající migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="e916f-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="e916f-200">V takovém případě se vytvoří databázi pomocí připojovacího řetězce definovaný v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="e916f-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="e916f-201">Přejděte na **zobrazení** nabídce a otevřete **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e916f-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="e916f-202">![Otevřít v Průzkumníku objektů systému SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "otevřít v Průzkumníku objektů SQL serveru")</span><span class="sxs-lookup"><span data-stu-id="e916f-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="e916f-203">*Otevřít v Průzkumníku objektů SQL serveru*</span><span class="sxs-lookup"><span data-stu-id="e916f-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="e916f-204">V **Průzkumník objektů systému SQL Server** okna, připojte se k instanci LocalDB kliknutím pravým tlačítkem myši **systému SQL Server** uzlu a vyberete **přidat SQL Server...**  možnost.</span><span class="sxs-lookup"><span data-stu-id="e916f-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="e916f-205">![Přidání Instance serveru SQL](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "přidání Instance serveru SQL")</span><span class="sxs-lookup"><span data-stu-id="e916f-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="e916f-206">*Přidání instance serveru SQL Server do Průzkumník objektů systému SQL Server*</span><span class="sxs-lookup"><span data-stu-id="e916f-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="e916f-207">Nastavte **název serveru** k *(localdb) \v11.0* a nechat **ověřování Windows** jako režim ověřování.</span><span class="sxs-lookup"><span data-stu-id="e916f-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="e916f-208">Klikněte na tlačítko **připojit** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e916f-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="e916f-209">![Připojování na instanci LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "připojení na instanci LocalDB")</span><span class="sxs-lookup"><span data-stu-id="e916f-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="e916f-210">*Připojování na instanci LocalDB*</span><span class="sxs-lookup"><span data-stu-id="e916f-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="e916f-211">Otevřít **GeekQuizProd** databáze a rozbalte **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e916f-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="e916f-212">Jak je vidět **aktualizace databáze** příkaz vygeneruje všechny tabulky, které jsou definovány v **TriviaContext** třídy.</span><span class="sxs-lookup"><span data-stu-id="e916f-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="e916f-213">Vyhledejte **dbo. TriviaQuestions** tabulky a otevřete uzel sloupce.</span><span class="sxs-lookup"><span data-stu-id="e916f-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="e916f-214">V dalším úkolem, přidáte nový sloupec do této tabulky a aktualizujte databáze s využitím **migrace**.</span><span class="sxs-lookup"><span data-stu-id="e916f-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="e916f-215">![Triviální prvek otázky, které sloupce](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "triviální prvek otázky, které sloupce")</span><span class="sxs-lookup"><span data-stu-id="e916f-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="e916f-216">*Triviální prvek otázky, které sloupce*</span><span class="sxs-lookup"><span data-stu-id="e916f-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="e916f-217">Úloha 2 – aktualizace schématu databáze pomocí migrace</span><span class="sxs-lookup"><span data-stu-id="e916f-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="e916f-218">V této úloze budete používat **migrace Entity Framework Code First** zjistí změnu ve vašem modelu a generovat kód potřebná k aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="e916f-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="e916f-219">Budete aktualizovat **TriviaQuestions** entity tak, že do ní přidáte novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e916f-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="e916f-220">Potom spustíte příkazy vytvoří novou migraci chcete zahrnout do nového sloupce v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e916f-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="e916f-221">V **Průzkumníka řešení**, dvakrát klikněte **TriviaQuestion.cs** soubor umístěný uvnitř **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="e916f-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="e916f-222">Přidat novou vlastnost s názvem **pomocný parametr**, jak je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="e916f-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="e916f-223">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e916f-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="e916f-224">Nová migrace se vytvoří, odrážející změnu v hodnotě náš model.</span><span class="sxs-lookup"><span data-stu-id="e916f-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="e916f-225">![Přidejte migraci](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "přidat migrace")</span><span class="sxs-lookup"><span data-stu-id="e916f-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="e916f-226">*Přidat migrace*</span><span class="sxs-lookup"><span data-stu-id="e916f-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-227">Soubor migrace se skládá ze dvou způsobů **nahoru** a **dolů**.</span><span class="sxs-lookup"><span data-stu-id="e916f-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    >
    > - <span data-ttu-id="e916f-228">**Nahoru** metoda se použije k určení, co se změní aktuální verze našich aplikací nutnost používat k databázi.</span><span class="sxs-lookup"><span data-stu-id="e916f-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="e916f-229">**Dolů** používaných k vrácení změn, přidali jsme do **nahoru** metody.</span><span class="sxs-lookup"><span data-stu-id="e916f-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    >
    > <span data-ttu-id="e916f-230">Při migraci databáze zaktualizuje databázi, se aplikace spustí všechny migrace v pořadí, časové razítko a jenom na ty, které nebyly použity od poslední aktualizace ( \_MigrationHistory tabulka uchovává informace o migraci, které byly použity).</span><span class="sxs-lookup"><span data-stu-id="e916f-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="e916f-231">**Nahoru** bude volána metoda všechny migrace a provede změny jsme zadali do databáze.</span><span class="sxs-lookup"><span data-stu-id="e916f-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="e916f-232">Pokud se rozhodneme vrátit k předchozí migrace **dolů** volaná metoda vrátit ke svému změny v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e916f-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="e916f-233">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e916f-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="e916f-234">Výstup **aktualizace databáze** příkaz vygeneruje **Alter Table** příkaz jazyka SQL, chcete-li přidat nový sloupec, **TriviaQuestions** tabulky, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e916f-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="e916f-235">![Přidat příkaz jazyka SQL sloupce generovány](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "přidat generované sloupce příkaz jazyka SQL")</span><span class="sxs-lookup"><span data-stu-id="e916f-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="e916f-236">*Přidat generované sloupce příkaz jazyka SQL*</span><span class="sxs-lookup"><span data-stu-id="e916f-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="e916f-237">V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, že nový **pomocný parametr** je zobrazen sloupec.</span><span class="sxs-lookup"><span data-stu-id="e916f-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="e916f-238">![Zobrazuje nový sloupec pomocný parametr](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "zobrazuje nový sloupec pomocný parametr")</span><span class="sxs-lookup"><span data-stu-id="e916f-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="e916f-239">*Zobrazuje nový sloupec pomocný parametr*</span><span class="sxs-lookup"><span data-stu-id="e916f-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="e916f-240">Zpátky **TriviaQuestion.cs** editoru přidejte **StringLength** omezení, které *pomocný parametr* vlastnost, jak je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="e916f-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="e916f-241">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e916f-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="e916f-242">V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e916f-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="e916f-243">Výstup **aktualizace databáze** příkaz vygeneruje **Alter Table** SQL. doje k aktualizaci *pomocný parametr* typ sloupce **TriviaQuestions** tabulky, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e916f-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="e916f-244">![Příkaz ALTER vygenerovat příkaz SQL sloupec](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter vygenerovat příkaz SQL sloupec")</span><span class="sxs-lookup"><span data-stu-id="e916f-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="e916f-245">*Příkaz ALTER vygenerovat příkaz SQL sloupec*</span><span class="sxs-lookup"><span data-stu-id="e916f-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="e916f-246">V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, že **pomocný parametr** typ sloupce je **nvarchar(150)**.</span><span class="sxs-lookup"><span data-stu-id="e916f-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="e916f-247">![Zobrazení omezení new](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "znázorňující omezení new")</span><span class="sxs-lookup"><span data-stu-id="e916f-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="e916f-248">*Zobrazuje nové omezení*</span><span class="sxs-lookup"><span data-stu-id="e916f-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="e916f-249">Cvičení 2: Nasazení webové aplikace do přípravného prostředí</span><span class="sxs-lookup"><span data-stu-id="e916f-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="e916f-250">**Webové aplikace ve službě Azure App Service** umožňuje provádět fázované publikování.</span><span class="sxs-lookup"><span data-stu-id="e916f-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="e916f-251">Fázované publikování vytváří slot pro každému výchozímu produkčnímu webu fázi webu a umožňuje vám přepínat mezi těmito sloty bez časové prodlevy.</span><span class="sxs-lookup"><span data-stu-id="e916f-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="e916f-252">To je velmi užitečné ověřit změny před uvolněním veřejně, postupně integrovat obsah webu a vrátit zpět, jestliže změny nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="e916f-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="e916f-253">V tomto cvičení nasadíte **kvíz Informatik** aplikaci do přípravného prostředí vaší webové aplikace pomocí správy zdrojového kódu Gitu.</span><span class="sxs-lookup"><span data-stu-id="e916f-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="e916f-254">K tomuto účelu bude vytvoření webové aplikace a zřízení požadovaných součástí na portálu pro správu, konfiguraci **Git** zdrojového kódu ze svého místního počítače do přípravného slotu úložiště a nabízených oznámení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="e916f-255">Budete také aktualizovat vaši produkční databázi s **migrace Code First** jste vytvořili v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="e916f-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="e916f-256">V tomto testovacím prostředí k ověření své činnosti pak spustí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e916f-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="e916f-257">Jakmile budete spokojeni se pracuje podle vašich očekávání, bude podporovat aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="e916f-258">Chcete-li fázované publikování, musí být webové aplikace v **standardní režim**.</span><span class="sxs-lookup"><span data-stu-id="e916f-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="e916f-259">Všimněte si, že další poplatky podle tarifu při změně vaší webové aplikace do standardního režimu.</span><span class="sxs-lookup"><span data-stu-id="e916f-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="e916f-260">Další informace o cenách najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="e916f-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="e916f-261">Úloha 1 – Vytvoření webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e916f-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="e916f-262">V této úloze vytvoříte webovou aplikaci v **služby Azure App Service** z portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="e916f-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="e916f-263">Můžete také nakonfigurovat **SQL Database** uchování dat aplikací a konfigurace místní úložiště Git pro správu zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="e916f-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="e916f-264">Přejděte [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="e916f-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Přihlaste se k portálu pro správu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="e916f-266">*Přihlaste se k portálu pro správu Azure*</span><span class="sxs-lookup"><span data-stu-id="e916f-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="e916f-267">Klikněte na tlačítko **nový** na příkazovém řádku v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e916f-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="e916f-268">![Vytváří se nová webová aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "vytvoření nové webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="e916f-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="e916f-269">*Vytvoření nové webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="e916f-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="e916f-270">Klikněte na tlačítko **Compute**, **webu** a potom **vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="e916f-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="e916f-271">![Vytvoření nové webové aplikace pomocí vytvořit vlastní](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "vytvoření nové webové aplikace s použitím vlastní vytvoření")</span><span class="sxs-lookup"><span data-stu-id="e916f-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="e916f-272">*Vytvoření nové webové aplikace s použitím vlastní vytvoření*</span><span class="sxs-lookup"><span data-stu-id="e916f-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="e916f-273">V **nový - vytvořit vlastní web** dialogového okna zadejte dostupného **adresy URL** (třeba *informatik kvíz*), vyberte umístění, do **oblasti** rozevírací seznam a vyberte **vytvořit novou databázi SQL** v **databáze** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="e916f-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="e916f-274">Nakonec vyberte **publikování ze správy zdrojových kódů** zaškrtávací políčko a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![Přizpůsobení novou webovou aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="e916f-276">*Přizpůsobení novou webovou aplikaci*</span><span class="sxs-lookup"><span data-stu-id="e916f-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="e916f-277">Zadejte následující informace pro nastavení databáze:</span><span class="sxs-lookup"><span data-stu-id="e916f-277">Specify the following information for the database settings:</span></span>

   - <span data-ttu-id="e916f-278">V **název** textové pole, zadejte název databáze (třeba *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="e916f-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
   - <span data-ttu-id="e916f-279">Na serveru **rozevíracího seznamu** seznamu vyberte **nový SQL server database**.</span><span class="sxs-lookup"><span data-stu-id="e916f-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="e916f-280">Alternativně můžete vybrat existující server.</span><span class="sxs-lookup"><span data-stu-id="e916f-280">Alternatively, you can select an existing server.</span></span>
   - <span data-ttu-id="e916f-281">V **uživatelské jméno pro databázi** a **heslo databáze** polí, zadejte uživatelské jméno správce a heslo pro server služby SQL database.</span><span class="sxs-lookup"><span data-stu-id="e916f-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="e916f-282">Pokud zvolíte možnost server jste již vytvořili, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="e916f-282">If you select a server you have already created, you will be prompted for the password.</span></span>

     ![Nastavení databáze](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     <span data-ttu-id="e916f-284">*Nastavení databáze*</span><span class="sxs-lookup"><span data-stu-id="e916f-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="e916f-285">Klikněte na tlačítko **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e916f-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="e916f-286">Vyberte **místního úložiště Git** pro správu zdrojového kódu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-287">Můžete být vyzváni k nasazení pověření (uživatelské jméno a heslo).</span><span class="sxs-lookup"><span data-stu-id="e916f-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Vytvoření úložiště Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="e916f-289">*Vytvoření úložiště Git*</span><span class="sxs-lookup"><span data-stu-id="e916f-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="e916f-290">Počkejte, až se vytvoří nová webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-291">Ve výchozím nastavení, Azure poskytuje domén na *azurewebsites.net* ale také vám dává možnost nastavit vlastní domény pomocí portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="e916f-292">Pokud používáte režimy určité služby Azure App Service však můžete spravovat pouze vlastních domén.</span><span class="sxs-lookup"><span data-stu-id="e916f-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    >
    > <span data-ttu-id="e916f-293">Azure App Service je k dispozici v edicích Free, Shared, Basic, Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="e916f-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="e916f-294">Všechny webové aplikace v režimu Free a Shared spustit v prostředí s více tenanty a kvóty pro využití procesoru, paměti a sítě.</span><span class="sxs-lookup"><span data-stu-id="e916f-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="e916f-295">Maximální počet bezplatných aplikací může lišit podle vašeho plánu.</span><span class="sxs-lookup"><span data-stu-id="e916f-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="e916f-296">Ve standardním režimu zvolte výpočetní prostředky, které aplikace spustit na vyhrazených virtuálních počítačích, které odpovídají na standardní Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="e916f-297">Můžete najít v konfiguraci webové aplikace režim **škálování** nabídky vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    >
    > <span data-ttu-id="e916f-298">![Azure App Service režimy](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "režimy služby Azure App Service")</span><span class="sxs-lookup"><span data-stu-id="e916f-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    >
    > <span data-ttu-id="e916f-299">Pokud používáte **Shared** nebo **standardní** režimu, bude možné spravovat vlastní domény pro webovou aplikaci tak, že přejdete do vaší aplikace **konfigurovat** nabídky a kliknutím na **Správa domén** pod *názvy domén*.</span><span class="sxs-lookup"><span data-stu-id="e916f-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    >
    > <span data-ttu-id="e916f-300">![Spravovat domény](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "spravovat domény")</span><span class="sxs-lookup"><span data-stu-id="e916f-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    >
    > <span data-ttu-id="e916f-301">![Správa vlastních domén](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "spravování vlastních domén")</span><span class="sxs-lookup"><span data-stu-id="e916f-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="e916f-302">Po vytvoření webové aplikace klikněte na odkaz v části **URL** sloupci zkontrolujte, že se nová webová aplikace spuštěná.</span><span class="sxs-lookup"><span data-stu-id="e916f-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![Přejdete do nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="e916f-304">*Přejdete do nové webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="e916f-304">*Browsing to the new web app*</span></span>

    ![Webová aplikace spuštěná](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="e916f-306">*Webová aplikace spuštěná*</span><span class="sxs-lookup"><span data-stu-id="e916f-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="e916f-307">Úloha 2 – Vytvoření produkční databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e916f-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="e916f-308">V této úloze budete používat **migrace Entity Framework Code First** k vytvoření databáze cílení **Azure SQL Database** instanci, kterou jste vytvořili v předchozí úloze.</span><span class="sxs-lookup"><span data-stu-id="e916f-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="e916f-309">Na portálu Management Portal, přejděte do webové aplikace, kterou jste vytvořili v předchozí úloze a přejděte do jeho **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="e916f-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="e916f-310">V **řídicí panel** klikněte na **zobrazit připojovací řetězce** odkaz pod **rychlý přehled** oddílu.</span><span class="sxs-lookup"><span data-stu-id="e916f-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="e916f-311">![Zobrazit připojovací řetězce](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "zobrazit připojovací řetězce")</span><span class="sxs-lookup"><span data-stu-id="e916f-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="e916f-312">*Zobrazit připojovací řetězce*</span><span class="sxs-lookup"><span data-stu-id="e916f-312">*View connection strings*</span></span>
3. <span data-ttu-id="e916f-313">Kopírovat **připojovací řetězec** hodnotu a zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e916f-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="e916f-314">![Připojovací řetězec v portálu pro správu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "připojovací řetězec v portálu pro správu Azure")</span><span class="sxs-lookup"><span data-stu-id="e916f-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="e916f-315">*Připojovací řetězec v portálu pro správu Azure*</span><span class="sxs-lookup"><span data-stu-id="e916f-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="e916f-316">Klikněte na tlačítko **databází SQL** zobrazíte seznam databází SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="e916f-317">![Nabídka SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "nabídce databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="e916f-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="e916f-318">*Nabídka SQL Database*</span><span class="sxs-lookup"><span data-stu-id="e916f-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="e916f-319">Databáze, kterou jste vytvořili v předchozí úloze vyhledejte a klikněte na Server.</span><span class="sxs-lookup"><span data-stu-id="e916f-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="e916f-320">![Server služby SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "serveru služby SQL Database")</span><span class="sxs-lookup"><span data-stu-id="e916f-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="e916f-321">*Server služby SQL Database*</span><span class="sxs-lookup"><span data-stu-id="e916f-321">*SQL Database server*</span></span>
6. <span data-ttu-id="e916f-322">V **rychlý Start** stránka serveru, klikněte na **konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="e916f-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="e916f-323">![Konfigurovat nabídky](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "konfigurovat nabídky")</span><span class="sxs-lookup"><span data-stu-id="e916f-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="e916f-324">*Konfigurace nabídky*</span><span class="sxs-lookup"><span data-stu-id="e916f-324">*Configure menu*</span></span>
7. <span data-ttu-id="e916f-325">V **povolené IP adresy** části, klikněte na **přidat do povolených IP adres** odkaz umožňující vaše IP adresa pro připojení k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="e916f-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="e916f-326">![Povolené IP adresy](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "povolené IP adresy")</span><span class="sxs-lookup"><span data-stu-id="e916f-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="e916f-327">*Povolené IP adresy*</span><span class="sxs-lookup"><span data-stu-id="e916f-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="e916f-328">Klikněte na tlačítko **Uložit** v dolní části stránky k dokončení kroku.</span><span class="sxs-lookup"><span data-stu-id="e916f-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="e916f-329">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e916f-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="e916f-330">V **Konzola správce balíčků**, spusťte následující příkaz nahrazení *[YOUR-CONNECTION-STRING]* zástupného symbolu připojovacím řetězcem, který jste zkopírovali z Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="e916f-331">![Aktualizace databáze cílení na Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aktualizovat databázi cílení na Windows Azure SQL Database")</span><span class="sxs-lookup"><span data-stu-id="e916f-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="e916f-332">*Aktualizace databáze, které cílí na Azure SQL Database*</span><span class="sxs-lookup"><span data-stu-id="e916f-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="e916f-333">Úloha 3 – nasazení kvíz nadšence do přípravného prostředí pomocí Gitu</span><span class="sxs-lookup"><span data-stu-id="e916f-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="e916f-334">V této úloze povolíte fázované publikování ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e916f-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="e916f-335">Potom se pomocí Gitu publikovat aplikace Informatik kvíz přímo ze svého místního počítače do přípravného prostředí vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="e916f-336">Přejděte zpět na portál a klikněte na název webové aplikace v rámci **název** sloupec, který se zobrazí na stránkách správy.</span><span class="sxs-lookup"><span data-stu-id="e916f-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Otevření webové stránky správy aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="e916f-338">*Otevření webové stránky správy aplikace*</span><span class="sxs-lookup"><span data-stu-id="e916f-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="e916f-339">Přejděte **škálování** stránky.</span><span class="sxs-lookup"><span data-stu-id="e916f-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="e916f-340">V části **Obecné** vyberte **standardní** konfigurace a klikněte na **Uložit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e916f-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-341">Spustit všechny webové aplikace v aktuální oblasti a předplatném v **standardní** režimu, ponechejte tuto položku **Vybrat vše** zaškrtnuto políčko **zvolit servery** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e916f-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="e916f-342">Jinak, zrušte **Vybrat vše** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="e916f-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="e916f-343">![Upgrade webové aplikace do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "upgrade webové aplikace na standardní režim")</span><span class="sxs-lookup"><span data-stu-id="e916f-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="e916f-344">*Upgrade webové aplikace na standardní režim*</span><span class="sxs-lookup"><span data-stu-id="e916f-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="e916f-345">Klikněte na tlačítko **Ano** potvrďte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e916f-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="e916f-346">![Potvrzují se změny do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "budete pokračovat se změnou režimu webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="e916f-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="e916f-347">*Potvrzují se změny do standardního režimu*</span><span class="sxs-lookup"><span data-stu-id="e916f-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="e916f-348">Přejděte na **řídicí panel** stránky a klikněte na tlačítko **fázované publikování na Povolit** pod **rychlý přehled** oddílu.</span><span class="sxs-lookup"><span data-stu-id="e916f-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="e916f-349">![Povolení fázované publikování](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "povolení fázované publikování")</span><span class="sxs-lookup"><span data-stu-id="e916f-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="e916f-350">*Povolení fázované publikování*</span><span class="sxs-lookup"><span data-stu-id="e916f-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="e916f-351">Klikněte na tlačítko **Ano** umožňující fázované publikování.</span><span class="sxs-lookup"><span data-stu-id="e916f-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="e916f-352">![Fázované publikování na potvrzení](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "kliknete na Ano, povolit fázované publikování")</span><span class="sxs-lookup"><span data-stu-id="e916f-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="e916f-353">*Potvrzují se fázované publikování*</span><span class="sxs-lookup"><span data-stu-id="e916f-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="e916f-354">V seznamu webových aplikací rozbalte na políčko nalevo od název vaší webové aplikace k zobrazení slot pro fázi webu.</span><span class="sxs-lookup"><span data-stu-id="e916f-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="e916f-355">Obsahuje název vaší webové aplikace, za nímž následuje ***(pracovní)***.</span><span class="sxs-lookup"><span data-stu-id="e916f-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="e916f-356">Klikněte na pracovním webu přejdete na stránku pro správu.</span><span class="sxs-lookup"><span data-stu-id="e916f-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="e916f-357">![Přejdete na pracovní aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "přejdete do pracovní webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="e916f-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="e916f-358">*Přejděte do pracovní aplikace*</span><span class="sxs-lookup"><span data-stu-id="e916f-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="e916f-359">Všimněte si, že tuto stránku správy he vypadá jako jakoukoli jinou webovou aplikaci pro správu stránky.</span><span class="sxs-lookup"><span data-stu-id="e916f-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="e916f-360">Přejděte **nasazení** stránky a zkopírujte **adresa URL pro Git** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e916f-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="e916f-361">Použijete ji později v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="e916f-361">You will use it later in this exercise.</span></span>

    ![Kopírování hodnoty adresy URL pro Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="e916f-363">*Kopírování hodnoty adresy URL pro Git*</span><span class="sxs-lookup"><span data-stu-id="e916f-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="e916f-364">Otevřete novou **Git Bash** konzoly a spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="e916f-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="e916f-365">Aktualizace *[YOUR cesta aplikace]* zástupný symbol s cestou **GeekQuiz** řešení, umístěný ve **Source\Ex1 DeployingWebSiteToStaging\Begin** složky Toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicializace Git a první potvrzení](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="e916f-367">*Inicializace Git a první potvrzení*</span><span class="sxs-lookup"><span data-stu-id="e916f-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="e916f-368">Spusťte následující příkaz tak, aby nabízel webovou aplikaci do vzdáleného **Git** úložiště.</span><span class="sxs-lookup"><span data-stu-id="e916f-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="e916f-369">Zástupný symbol nahraďte adresu URL, které jste získali z portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="e916f-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="e916f-370">Zobrazí se výzva k zadání hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="e916f-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Doručením (push) do Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="e916f-372">*Doručením (push) do Azure*</span><span class="sxs-lookup"><span data-stu-id="e916f-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-373">Při nasazení obsahu na serveru FTP hostitele nebo úložiště GIT webové aplikace musí ověřit pomocí **přihlašovací údaje pro nasazení** , kterou jste vytvořili z webové aplikace **rychlý Start** nebo **řídicího panelu**  správu stránek.</span><span class="sxs-lookup"><span data-stu-id="e916f-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="e916f-374">Pokud si nejste jisti přihlašovací údaje pro nasazení je snadno obnovit pomocí portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="e916f-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="e916f-375">Otevřít webovou aplikaci **řídicí panel** stránky a klikněte na tlačítko **resetovat přihlašovací údaje pro nasazení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="e916f-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="e916f-376">Zadejte nové heslo a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e916f-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="e916f-377">Přihlašovací údaje pro nasazení jsou platná pro použití s všech webových aplikací přidružených k vašemu předplatnému.</span><span class="sxs-lookup"><span data-stu-id="e916f-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="e916f-378">Pokud chcete ověřit, webové aplikace bylo úspěšně vloženo do Azure, přejděte zpět na portál pro správu a klikněte na tlačítko **Websites**.</span><span class="sxs-lookup"><span data-stu-id="e916f-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="e916f-379">Vyhledejte vaši webovou aplikaci a rozbalte položku zobrazíte slot pro fázi webu.</span><span class="sxs-lookup"><span data-stu-id="e916f-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="e916f-380">Klikněte na jeho **název** přejdete na stránku pro správu.</span><span class="sxs-lookup"><span data-stu-id="e916f-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="e916f-381">Klikněte na tlačítko **nasazení** zobrazíte **historie nasazení**.</span><span class="sxs-lookup"><span data-stu-id="e916f-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="e916f-382">Ověřte, zda je **aktivní nasazení** s vaší  *&quot;počáteční potvrzení&quot;*.</span><span class="sxs-lookup"><span data-stu-id="e916f-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![Aktivní nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="e916f-384">*Aktivní nasazení*</span><span class="sxs-lookup"><span data-stu-id="e916f-384">*Active deployment*</span></span>
13. <span data-ttu-id="e916f-385">Nakonec klikněte na tlačítko **Procházet** na příkazovém řádku přejděte do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Procházet webovou aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="e916f-387">*Procházet webovou aplikaci*</span><span class="sxs-lookup"><span data-stu-id="e916f-387">*Browse web app*</span></span>
14. <span data-ttu-id="e916f-388">Pokud aplikace je úspěšně nasazená, zobrazí se přihlašovací stránky dotazníku nadšence.</span><span class="sxs-lookup"><span data-stu-id="e916f-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-389">Adresa URL nasazené aplikace obsahuje název vaší webové aplikace, za nímž následuje *– pracovní*.</span><span class="sxs-lookup"><span data-stu-id="e916f-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![Aplikace běžící v testovacím prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="e916f-391">*Aplikace běžící v testovacím prostředí*</span><span class="sxs-lookup"><span data-stu-id="e916f-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="e916f-392">Pokud chcete prozkoumat aplikace, klikněte na tlačítko **zaregistrovat** k registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e916f-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="e916f-393">Vyplňte podrobnosti účtu tak, že zadáte uživatelské jméno, e-mailovou adresu a heslo.</span><span class="sxs-lookup"><span data-stu-id="e916f-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="e916f-394">V dalším kroku aplikace ukazuje na první otázku, kvíz.</span><span class="sxs-lookup"><span data-stu-id="e916f-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="e916f-395">Odpovězte na několik otázek, abyste měli jistotu, že funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="e916f-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![Aplikace je připravená k použití](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="e916f-397">*Aplikace je připravená k použití*</span><span class="sxs-lookup"><span data-stu-id="e916f-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="e916f-398">Úloha 4 – Podpora webové aplikace do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="e916f-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="e916f-399">Teď, když si ověříte, že webová aplikace pracuje správně v testovacím prostředí, budete chtít přesunout do výroby.</span><span class="sxs-lookup"><span data-stu-id="e916f-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="e916f-400">V této úloze se Prohodit slot pro fázi webu s produkční slot webu.</span><span class="sxs-lookup"><span data-stu-id="e916f-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="e916f-401">Přejděte zpět na portál pro správu a vyberte slot pro fázi webu.</span><span class="sxs-lookup"><span data-stu-id="e916f-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="e916f-402">Klikněte na tlačítko **Prohodit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e916f-402">Click **Swap** in the command bar.</span></span>

    ![Zaměnit do produkčního prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="e916f-404">*Zaměnit do produkčního prostředí*</span><span class="sxs-lookup"><span data-stu-id="e916f-404">*Swap to production*</span></span>
2. <span data-ttu-id="e916f-405">Klikněte na tlačítko **Ano** v potvrzovacím dialogovém okně, aby bylo možné pokračovat v operaci prohození.</span><span class="sxs-lookup"><span data-stu-id="e916f-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="e916f-406">Azure bude okamžitě Prohodit obsah pracoviště s obsahem na pracovním webu.</span><span class="sxs-lookup"><span data-stu-id="e916f-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-407">Některá nastavení z připravenou verzi se automaticky zkopírují do produkční verze (například připojovací řetězec přepsání, mapování obslužných rutin, atd.), ale ostatní nastavení nedojde ke změně (např. koncové body DNS, vazby SSL atd.).</span><span class="sxs-lookup"><span data-stu-id="e916f-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![Potvrzení operace prohození](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="e916f-409">*Potvrzení operace prohození*</span><span class="sxs-lookup"><span data-stu-id="e916f-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="e916f-410">Po dokončení prohození vyberte slot produkčního prostředí a klikněte na tlačítko **Procházet** na panelu příkazů otevřete pracoviště.</span><span class="sxs-lookup"><span data-stu-id="e916f-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="e916f-411">Všimněte si, že adresa URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="e916f-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-412">Vám může být nutné aktualizovat prohlížeč vymazat mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="e916f-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="e916f-413">V aplikaci Internet Explorer, provedete stisknutím kombinace kláves **CTRL + R**.</span><span class="sxs-lookup"><span data-stu-id="e916f-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![Webová aplikace spuštěná v produkčním prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="e916f-415">V **GitBash** konzole, aktualizovat vzdálenou adresu URL pro místní úložiště Git cílit na produkční slot.</span><span class="sxs-lookup"><span data-stu-id="e916f-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="e916f-416">Chcete-li to provést, spusťte následující příkaz zástupné texty nahraďte uživatelské jméno pro nasazení a název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-417">V následujícím cvičení vložíte změny do produkční lokality místo pracovní pouze pro zjednodušení tohoto prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="e916f-418">Ve skutečném scénáři se doporučuje ověřit změny v testovacím prostředí před zvýšením úrovně do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="e916f-419">Cvičení 3: Provedení vrácení nasazení zpět v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="e916f-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="e916f-420">Existují scénáře, ve kterém nemáte přípravný slot provádět horké prohození mezi přípravným a produkčním prostředím, například pokud pracujete s **Free** nebo **Shared** režimu.</span><span class="sxs-lookup"><span data-stu-id="e916f-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="e916f-421">V těchto scénářích měli byste otestovat vaši aplikaci v testovacím prostředí – místně nebo ve vzdálené lokalitě – před nasazením do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="e916f-422">Je však možné, že problém nebyl zjištěn během fáze testování mohou nastat v produkční lokality.</span><span class="sxs-lookup"><span data-stu-id="e916f-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="e916f-423">V takovém případě je důležité mít mechanismus pro snadno přepínat na předchozí a více stabilní verzi aplikace co nejrychleji.</span><span class="sxs-lookup"><span data-stu-id="e916f-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="e916f-424">V **služby Azure App Service**, toto je to možné díky díky průběžné nasazování ze správy zdrojového kódu **znovu nasadit** akce, které jsou k dispozici na portálu management portal.</span><span class="sxs-lookup"><span data-stu-id="e916f-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="e916f-425">Uchovává informace o nasazení přidružená k potvrzení změn do úložiště Azure a nabízí možnost znovu nasadit aplikaci pomocí kteréhokoli z předchozích nasazení, kdykoli.</span><span class="sxs-lookup"><span data-stu-id="e916f-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="e916f-426">V tomto cvičení budete provádět změny kódu v **Informatik kvíz** aplikaci, která záměrně vkládá *chyb*.</span><span class="sxs-lookup"><span data-stu-id="e916f-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="e916f-427">Budete nasazovat aplikaci do produkčního prostředí, abyste viděli chybu a pak bude využít výhod funkce opětovného nasazení se vrátíte do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="e916f-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="e916f-428">Úloha 1 – aktualizace aplikace kvíz Informatik</span><span class="sxs-lookup"><span data-stu-id="e916f-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="e916f-429">V této úloze bude Refaktorovat malou část kódu **TriviaController** třídy k extrakci část logiky, který načte kvíz vybranou možnost z databáze do nové metody.</span><span class="sxs-lookup"><span data-stu-id="e916f-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="e916f-430">Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="e916f-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="e916f-431">V **Průzkumníka řešení**, otevřete **TriviaController.cs** soubor uvnitř **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="e916f-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="e916f-432">Vyhledejte **StoreAsync** metody a vyberte zvýrazněný kód na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e916f-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![Vyberte kód](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="e916f-434">*Vyberte kód*</span><span class="sxs-lookup"><span data-stu-id="e916f-434">*Selecting the code*</span></span>
4. <span data-ttu-id="e916f-435">Klikněte pravým tlačítkem na vybraný kód, rozbalte **Refaktorovat** nabídky a vybereme **extrahovat metodu...** .</span><span class="sxs-lookup"><span data-stu-id="e916f-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![Extrahování kód jako novou metodu](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="e916f-437">*Výběr metody extrahování*</span><span class="sxs-lookup"><span data-stu-id="e916f-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="e916f-438">V **extrahovat metodu** dialogové okno, název nové metody *MatchesOption* a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e916f-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![Zadání názvu – metoda](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="e916f-440">*Zadejte název pro extrahovaný – metoda*</span><span class="sxs-lookup"><span data-stu-id="e916f-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="e916f-441">Vybraný kód je pak extrahován do **MatchesOption** metody.</span><span class="sxs-lookup"><span data-stu-id="e916f-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="e916f-442">Výsledný kód je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="e916f-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="e916f-443">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="e916f-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="e916f-444">Úloha 2 – opětovného nasazení aplikace kvíz Informatik</span><span class="sxs-lookup"><span data-stu-id="e916f-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="e916f-445">Nyní budou nabízená oznámení změn, které jste provedli v předchozí úloze úložiště, které aktivují nové nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="e916f-446">Potom se potíže, problém pomocí **vývojářské nástroje F12** poskytované Internet Exploreru a pak proveďte vrácení zpět na předchozí nasazení z portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="e916f-447">Otevřete novou **Git Bash** konzoly nasaďte aktualizovanou aplikaci do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e916f-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="e916f-448">Spusťte následující příkazy, které odešlete změny do Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="e916f-449">Aktualizace *[YOUR cesta aplikace]* zástupný symbol s cestou **GeekQuiz** řešení.</span><span class="sxs-lookup"><span data-stu-id="e916f-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="e916f-450">Zobrazí se výzva k zadání hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="e916f-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Nahráním refaktorovaný kódu do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="e916f-452">*Nahráním refaktorovaný kódu do Azure*</span><span class="sxs-lookup"><span data-stu-id="e916f-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="e916f-453">Otevřete aplikaci Internet Explorer a přejděte do své webové aplikace (například `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="e916f-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="e916f-454">Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e916f-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="e916f-455">Stisknutím klávesy **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** kartě a klikněte na tlačítko **Přehrát** tlačítko Spustit záznam.</span><span class="sxs-lookup"><span data-stu-id="e916f-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="e916f-456">![Spouštění nahrávání sítě](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "od sítě záznam")</span><span class="sxs-lookup"><span data-stu-id="e916f-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="e916f-457">*Spouští se záznam sítě*</span><span class="sxs-lookup"><span data-stu-id="e916f-457">*Starting network recording*</span></span>
5. <span data-ttu-id="e916f-458">Vyberte všechny možnosti kvíz.</span><span class="sxs-lookup"><span data-stu-id="e916f-458">Select any option of the quiz.</span></span> <span data-ttu-id="e916f-459">Zobrazí se, že se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="e916f-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="e916f-460">V **F12** okno, zobrazuje položku odpovídající požadavek POST HTTP protokolu HTTP **500** výsledek.</span><span class="sxs-lookup"><span data-stu-id="e916f-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 chyb](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="e916f-462">*HTTP 500 chyb*</span><span class="sxs-lookup"><span data-stu-id="e916f-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="e916f-463">Vyberte **konzoly** kartu. Podrobnosti o příčině je zaznamenána chyba.</span><span class="sxs-lookup"><span data-stu-id="e916f-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![Zaznamenané chyby](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="e916f-465">*Zaznamenané chyby*</span><span class="sxs-lookup"><span data-stu-id="e916f-465">*Logged error*</span></span>
8. <span data-ttu-id="e916f-466">Vyhledejte část Podrobnosti o této chybě.</span><span class="sxs-lookup"><span data-stu-id="e916f-466">Locate the details part of the error.</span></span> <span data-ttu-id="e916f-467">Je zřejmé tato chyba je způsobena kódu, refaktoring jste v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="e916f-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="e916f-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="e916f-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="e916f-469">Nezavírejte prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e916f-469">Do not close the browser.</span></span>
10. <span data-ttu-id="e916f-470">V nové instanci prohlížeče, přejděte [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="e916f-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="e916f-471">Vyberte **Websites** a klikněte na webovou aplikaci, kterou jste vytvořili v cvičení 2.</span><span class="sxs-lookup"><span data-stu-id="e916f-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="e916f-472">Přejděte **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="e916f-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="e916f-473">Všimněte si, že provádí všechna potvrzení změn jsou uvedeny v historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="e916f-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![Seznam existujících nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="e916f-475">*Seznam existujících nasazení*</span><span class="sxs-lookup"><span data-stu-id="e916f-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="e916f-476">Vybrat předchozí potvrzení změn a klikněte na tlačítko **znovu nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e916f-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![Znovu se nasazuje předchozí potvrzení změn](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="e916f-478">*Znovu se nasazuje předchozí potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="e916f-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="e916f-479">Po zobrazení výzvy k potvrzení klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e916f-479">When prompted to confirm, click **Yes**.</span></span>

    ![Potvrzují se opětovné nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="e916f-481">Po dokončení nasazení se přepněte zpět do instance prohlížeče s vaší webové aplikace a stisknutím klávesy **CTRL + F5**.</span><span class="sxs-lookup"><span data-stu-id="e916f-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="e916f-482">Klikněte na některou z možností.</span><span class="sxs-lookup"><span data-stu-id="e916f-482">Click any of the options.</span></span> <span data-ttu-id="e916f-483">Překlopit animace bude nyní provést a výsledek (*opravte nesprávné*) se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e916f-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="e916f-484">(Volitelné) Přepněte **Git Bash** konzoly a spusťte následující příkazy se vrátit k předchozí potvrzení změn.</span><span class="sxs-lookup"><span data-stu-id="e916f-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-485">Tyto příkazy vytvoří nové potvrzení, který vrátí zpět všechny změny v úložišti Git, které byly provedeny v chybný potvrzení.</span><span class="sxs-lookup"><span data-stu-id="e916f-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="e916f-486">Azure pak znovu nasadit aplikaci pomocí nové potvrzení.</span><span class="sxs-lookup"><span data-stu-id="e916f-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="e916f-487">Cvičení 4: Škálování s využitím služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e916f-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="e916f-488">**Objekty BLOB** představují nejjednodušší způsob, jak ukládat velké objemy nestrukturovaných textových nebo binárních dat jako jsou videa, zvukové soubory a obrázky.</span><span class="sxs-lookup"><span data-stu-id="e916f-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="e916f-489">Přesunutí statický obsah aplikace do úložiště, umožňuje škálovat aplikaci podle poskytování obrázků nebo dokumentů přímo do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e916f-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="e916f-490">V tomto cvičení se přesune statický obsah aplikace do kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e916f-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="e916f-491">Pak se konfigurace aplikace pro přidání **přepisu adresy URL technologie ASP.NET pravidlo** v **Web.config** přesměrovat obsah do kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e916f-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="e916f-492">Úloha 1 – Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e916f-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="e916f-493">V této úloze se dozvíte, jak vytvořit nový účet úložiště pomocí portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="e916f-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="e916f-494">Přejděte [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="e916f-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="e916f-495">Vyberte **nové | Data Services | Úložiště | Příkaz pro rychlé vytvoření** a začněte vytvářet nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e916f-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="e916f-496">Zadejte jedinečný název pro účet a vyberte **oblasti** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="e916f-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="e916f-497">Klikněte na tlačítko **vytvořit účet úložiště** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e916f-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="e916f-498">![Vytváří se nový účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "vytváření nového účtu úložiště")</span><span class="sxs-lookup"><span data-stu-id="e916f-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="e916f-499">*Vytváří se nový účet úložiště*</span><span class="sxs-lookup"><span data-stu-id="e916f-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="e916f-500">V **úložiště** části, počkejte, dokud stav nového účtu úložiště se změní na *Online* budete pokračovat následujícím krokem.</span><span class="sxs-lookup"><span data-stu-id="e916f-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="e916f-501">![Účet úložiště vytvořený](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "vytvořený účet úložiště")</span><span class="sxs-lookup"><span data-stu-id="e916f-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="e916f-502">*Vytvoření účtu úložiště*</span><span class="sxs-lookup"><span data-stu-id="e916f-502">*Storage Account created*</span></span>
4. <span data-ttu-id="e916f-503">Klikněte na název účtu úložiště a pak klikněte na tlačítko **řídicí panel** odkazu v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e916f-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="e916f-504">**Řídicí panel** stránce vám poskytne informace o stavu účtu a koncové body služby, které lze použít v rámci vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="e916f-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="e916f-505">![Zobrazení řídicího panelu úložiště účtu](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "zobrazení řídicího panelu účtu úložiště")</span><span class="sxs-lookup"><span data-stu-id="e916f-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="e916f-506">*Zobrazení řídicí panel účtu úložiště*</span><span class="sxs-lookup"><span data-stu-id="e916f-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="e916f-507">Klikněte na tlačítko **spravovat přístupové klíče** tlačítko v navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="e916f-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="e916f-508">![Tlačítko Spravovat přístupové klíče](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "tlačítko Spravovat přístupové klíče")</span><span class="sxs-lookup"><span data-stu-id="e916f-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="e916f-509">*Správa přístupových klíčů tlačítko*</span><span class="sxs-lookup"><span data-stu-id="e916f-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="e916f-510">V **spravovat přístupové klíče** dialogové okno, kopie **název účtu úložiště** a **primární přístupový klíč** , kdykoli budete potřebovat v následujícím cvičení.</span><span class="sxs-lookup"><span data-stu-id="e916f-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="e916f-511">Zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e916f-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="e916f-512">![Přístupový klíč dialogového okna Spravovat](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "dialogové okno Spravovat přístupový klíč")</span><span class="sxs-lookup"><span data-stu-id="e916f-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="e916f-513">*Přístupový klíč dialogového okna Spravovat*</span><span class="sxs-lookup"><span data-stu-id="e916f-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="e916f-514">Úloha 2 – nahrání prostředek do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="e916f-515">V této úloze použijete okno Průzkumníka serveru ze sady Visual Studio pro připojení k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e916f-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="e916f-516">Pak vytvoříte kontejner objektů blob a odeslat soubor s logem kvíz nadšence do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e916f-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="e916f-517">Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="e916f-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="e916f-518">V panelu nabídky vyberte **zobrazení** a potom klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="e916f-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="e916f-519">V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **Azure** uzel a vyberte možnost **připojit se k Azure...** . Přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="e916f-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Připojte se k Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="e916f-521">*Připojení k Azure*</span><span class="sxs-lookup"><span data-stu-id="e916f-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="e916f-522">Rozbalte **Azure** uzel, klikněte pravým tlačítkem na **úložiště** a vyberte **připojit externí úložiště...** .</span><span class="sxs-lookup"><span data-stu-id="e916f-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="e916f-523">V **přidat nový účet úložiště** dialogového okna zadejte **název účtu** a **klíč účtu** jste získali v předchozí úloze a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e916f-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![Přidat nový účet úložiště – dialogové okno](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="e916f-525">*Přidat nový účet úložiště – dialogové okno*</span><span class="sxs-lookup"><span data-stu-id="e916f-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="e916f-526">Váš účet úložiště by se měla zobrazit v části **úložiště** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e916f-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="e916f-527">Rozbalte účet úložiště, klikněte pravým tlačítkem na **objekty BLOB** a vyberte **vytvořit kontejner objektů Blob...** .</span><span class="sxs-lookup"><span data-stu-id="e916f-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="e916f-528">![Vytvořit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "vytvořit kontejner objektů Blob")</span><span class="sxs-lookup"><span data-stu-id="e916f-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="e916f-529">*Vytvořit kontejner objektů Blob*</span><span class="sxs-lookup"><span data-stu-id="e916f-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="e916f-530">V **vytvořit kontejner objektů Blob** dialogového okna zadejte název kontejneru objektů blob a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e916f-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="e916f-531">![Dialogové okno vytvořit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "dialogové okno vytvořit kontejner objektů Blob")</span><span class="sxs-lookup"><span data-stu-id="e916f-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="e916f-532">*Vytvoření dialogového okna kontejner objektů Blob*</span><span class="sxs-lookup"><span data-stu-id="e916f-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="e916f-533">Nový kontejner objektů blob měla být přidána do **objekty BLOB** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e916f-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="e916f-534">Změňte přístupová oprávnění v kontejneru zveřejněte kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e916f-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="e916f-535">Chcete-li to provést, klikněte pravým tlačítkem **image** kontejner a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e916f-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="e916f-536">![Image kontejneru vlastnosti](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Image vlastnosti kontejneru")</span><span class="sxs-lookup"><span data-stu-id="e916f-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="e916f-537">*Vlastnosti kontejneru obrázků*</span><span class="sxs-lookup"><span data-stu-id="e916f-537">*Images container properties*</span></span>
9. <span data-ttu-id="e916f-538">V **vlastnosti** okno, nastaveno **veřejné oprávnění ke čtení** k **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="e916f-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="e916f-539">![Změna vlastnosti veřejné oprávnění ke čtení](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "při změně hodnoty vlastnosti veřejné oprávnění ke čtení")</span><span class="sxs-lookup"><span data-stu-id="e916f-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="e916f-540">*Změna vlastnosti veřejné oprávnění ke čtení*</span><span class="sxs-lookup"><span data-stu-id="e916f-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="e916f-541">Po zobrazení výzvy, pokud jste si jistí, kterou chcete změnit vlastnost veřejného přístupu, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e916f-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="e916f-542">![Microsoft Visual Studio upozornění](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "upozornění Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="e916f-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="e916f-543">*Microsoft Visual Studio upozornění*</span><span class="sxs-lookup"><span data-stu-id="e916f-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="e916f-544">V **Průzkumníka serveru**, klikněte pravým tlačítkem **image** kontejner objektů blob a vyberte **kontejner objektů Blob zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e916f-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="e916f-545">![Zobrazit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "zobrazení kontejner objektů Blob")</span><span class="sxs-lookup"><span data-stu-id="e916f-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="e916f-546">*Zobrazit kontejner objektů Blob*</span><span class="sxs-lookup"><span data-stu-id="e916f-546">*View Blob Container*</span></span>
12. <span data-ttu-id="e916f-547">Kontejner imagí by se otevřít v novém okně a legenda s žádné položky, které se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="e916f-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="e916f-548">Klikněte na tlačítko **nahrát** ikonu pro nahrání souboru do kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e916f-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="e916f-549">![Image kontejneru s žádné položky](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Image kontejneru s žádné položky")</span><span class="sxs-lookup"><span data-stu-id="e916f-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="e916f-550">*Image kontejneru s žádné položky*</span><span class="sxs-lookup"><span data-stu-id="e916f-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="e916f-551">V **nahrát objekt Blob** dialogové okno, přejděte na **prostředky** složky testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="e916f-552">Vyberte **logo big.png** souboru a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="e916f-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="e916f-553">Počkejte, až se soubor nahraje.</span><span class="sxs-lookup"><span data-stu-id="e916f-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="e916f-554">Po dokončení nahrávání souboru by měl nacházet na Image kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e916f-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="e916f-555">Klikněte pravým tlačítkem na položku soubor a vyberte **kopírování adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="e916f-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="e916f-556">![Zkopírujte adresu URL objektu blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "zkopírujte adresu URL souboru objektu blob")</span><span class="sxs-lookup"><span data-stu-id="e916f-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="e916f-557">*Zkopírujte adresu URL objektu blob*</span><span class="sxs-lookup"><span data-stu-id="e916f-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="e916f-558">Spusťte aplikaci Internet Explorer a vložte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e916f-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="e916f-559">Na následujícím obrázku by měla být zobrazená v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e916f-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="e916f-560">![Obrázek loga big.png z úložiště objektů Blob Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo big.png image ze služby storage")</span><span class="sxs-lookup"><span data-stu-id="e916f-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="e916f-561">*Logo big.png image z Azure Blob Storage*</span><span class="sxs-lookup"><span data-stu-id="e916f-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="e916f-562">Úloha 3 – aktualizuje se řešení využívají statický obsah z úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="e916f-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="e916f-563">V této úloze nakonfigurujete **GeekQuiz** řešení, které využívají obrázek nahraný do Azure Blob Storage (a nikoli obraz umístěný ve webové aplikaci) tak, že přidáte pravidlo pro přepis adres URL technologie ASP.NET, v **web.config**souboru.</span><span class="sxs-lookup"><span data-stu-id="e916f-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="e916f-564">V sadě Visual Studio, otevřete **Web.config** soubor uvnitř **GeekQuiz** projektu a vyhledejte **&lt;system.webServer&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="e916f-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="e916f-565">Přidejte následující kód pro přidání přepisování adres URL pravidla aktualizace zástupného textu s názvem vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e916f-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="e916f-566">(Fragment - kódu *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="e916f-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="e916f-567">Přepisování adres URL je proces zachycení příchozí webové žádosti a požadavku přesměrování na jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="e916f-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="e916f-568">Adresa URL přepsání pravidel říká přepis adres modul, když se požadavek musí přesměrovat a kde by měl být přesměrován.</span><span class="sxs-lookup"><span data-stu-id="e916f-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="e916f-569">Pravidlo pro přepis adres se skládá ze dvou řetězců: model v požadované adrese URL (obvykle, pomocí regulárních výrazů), a řetězec pro nahrazení vzoru, pokud nalezen.</span><span class="sxs-lookup"><span data-stu-id="e916f-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="e916f-570">Další informace najdete v tématu [přepisování adres URL v technologii ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span><span class="sxs-lookup"><span data-stu-id="e916f-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="e916f-571">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="e916f-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="e916f-572">Otevřete novou **Git Bash** konzoly nasaďte aktualizovanou aplikaci do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e916f-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="e916f-573">Spusťte následující příkazy, které odešlete změny do Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="e916f-574">Aktualizace *[YOUR cesta aplikace]* zástupný symbol s cestou **GeekQuiz** řešení.</span><span class="sxs-lookup"><span data-stu-id="e916f-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="e916f-575">Zobrazí se výzva k zadání hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="e916f-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Nasazení aktualizací do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="e916f-577">*Nasazení aktualizací do Azure*</span><span class="sxs-lookup"><span data-stu-id="e916f-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="e916f-578">Úloha 4 – ověření</span><span class="sxs-lookup"><span data-stu-id="e916f-578">Task 4 – Verification</span></span>

<span data-ttu-id="e916f-579">V této úloze budete používat **aplikace Internet Explorer** a přejděte **Informatik kvíz** aplikace a zkontrolujte, že adresa URL přepsat pravidlo pro Image funguje a můžete se přesměrují na image hostovanou v **objektů Blob v Azure Úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e916f-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="e916f-580">Otevřete aplikaci Internet Explorer a přejděte do své webové aplikace (například `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="e916f-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="e916f-581">Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e916f-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="e916f-582">![Zobrazuje webovou aplikaci Informatik kvíz s imagí](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "zobrazující webovou aplikaci Informatik kvíz s imagí")</span><span class="sxs-lookup"><span data-stu-id="e916f-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="e916f-583">*Zobrazuje webovou aplikaci Informatik kvíz s imagí*</span><span class="sxs-lookup"><span data-stu-id="e916f-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="e916f-584">Stisknutím klávesy **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** kartu a spusťte záznam.</span><span class="sxs-lookup"><span data-stu-id="e916f-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="e916f-585">![Spouštění nahrávání sítě](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "od sítě záznam")</span><span class="sxs-lookup"><span data-stu-id="e916f-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="e916f-586">*Spouští se záznam sítě*</span><span class="sxs-lookup"><span data-stu-id="e916f-586">*Starting network recording*</span></span>
3. <span data-ttu-id="e916f-587">Stisknutím klávesy **CTRL + F5** aktualizujte webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="e916f-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="e916f-588">Po dokončení načítání stránky byste měli vidět požadavku HTTP pro **/img/logo-big.png** adresu URL pomocí protokolu HTTP **301** výsledek (přesměrování) a další žádost o `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adresu URL s protokolem HTTP **200** výsledek.</span><span class="sxs-lookup"><span data-stu-id="e916f-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="e916f-589">![Ověřuje se adresa URL přesměrování](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "zobrazující přesměrování v nástroje pro vývojáře")</span><span class="sxs-lookup"><span data-stu-id="e916f-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="e916f-590">*Ověřuje se adresa URL pro přesměrování*</span><span class="sxs-lookup"><span data-stu-id="e916f-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="e916f-591">Cvičení 5: Použití automatického škálování pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e916f-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="e916f-592">Tento postup je volitelné, protože vyžaduje podporu pro zatížení webové &amp; testování výkonu, která je dostupná jenom pro **Visual Studio 2013 Ultimate Edition**.</span><span class="sxs-lookup"><span data-stu-id="e916f-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="e916f-593">Další informace o konkrétní součásti, které Visual Studio 2013, porovnat verze [tady](https://www.microsoft.com/visualstudio/eng/products/compare).</span><span class="sxs-lookup"><span data-stu-id="e916f-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="e916f-594">**Azure App Service Web Apps** poskytuje funkci automatického škálování pro web apps spouštěná ve **standardní režim**.</span><span class="sxs-lookup"><span data-stu-id="e916f-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="e916f-595">Automatické škálování umožňuje Azure se dá automaticky škálovat počet instancí webové aplikace v závislosti na zatížení.</span><span class="sxs-lookup"><span data-stu-id="e916f-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="e916f-596">Když je povolené automatické škálování, Azure kontroluje každých pět minut procesoru vaší webové aplikace a přidá instancí podle potřeby v daném okamžiku v čase.</span><span class="sxs-lookup"><span data-stu-id="e916f-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="e916f-597">Pokud bude malé využití procesoru, Azure odebere instance každé dvě hodiny zajistit, že není snížený výkon webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="e916f-598">V tomto cvičení jste projít kroky potřebnými ke konfiguraci **automatického škálování** funkce pro **kvíz Informatik** webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="e916f-599">Tato funkce bude ověřte spuštěním zátěžového testu sady Visual Studio pro generování dostatečného zatížení procesoru v aplikaci k aktivaci instance upgrade.</span><span class="sxs-lookup"><span data-stu-id="e916f-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="e916f-600">Úloha 1 – konfigurace automatického škálování na základě metriky využití procesoru</span><span class="sxs-lookup"><span data-stu-id="e916f-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="e916f-601">V této úloze bude používat portál pro správu Azure k povolení této funkce automatického škálování pro webovou aplikaci, kterou jste vytvořili v cvičení 2.</span><span class="sxs-lookup"><span data-stu-id="e916f-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="e916f-602">V [portál pro správu Azure](https://manage.windowsazure.com/)vyberte **Websites** a klikněte na webovou aplikaci, kterou jste vytvořili v cvičení 2.</span><span class="sxs-lookup"><span data-stu-id="e916f-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="e916f-603">Přejděte **škálování** stránky.</span><span class="sxs-lookup"><span data-stu-id="e916f-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="e916f-604">V části **kapacity** vyberte **procesoru** pro **škálování podle metriky** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e916f-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-605">Při horizontálním škálování podle využití procesoru, Azure dynamicky přizpůsobí počet instancí, které aplikace používá, pokud se změní využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="e916f-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="e916f-606">![Výběr škálování podle využití procesoru](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "výběrem procesoru metriky pro automatické škálování")</span><span class="sxs-lookup"><span data-stu-id="e916f-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="e916f-607">*Výběr škálování podle využití procesoru*</span><span class="sxs-lookup"><span data-stu-id="e916f-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="e916f-608">Změnit **cílový procesor** konfiguraci **20**-**40** procent.</span><span class="sxs-lookup"><span data-stu-id="e916f-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-609">Tento rozsah představuje průměrné využití procesoru pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e916f-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="e916f-610">Azure bude přidávat nebo odebírat instance, které chcete zachovat svou webovou aplikaci v tomto rozsahu.</span><span class="sxs-lookup"><span data-stu-id="e916f-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="e916f-611">Zadat minimální a maximální počet instancí používaných pro škálování v **počet instancí** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e916f-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="e916f-612">Azure se nikdy dostanou nad nebo nad rámec tohoto limitu.</span><span class="sxs-lookup"><span data-stu-id="e916f-612">Azure will never go above or beyond that limit.</span></span>
    >
    > <span data-ttu-id="e916f-613">Výchozí hodnota **cílový procesor** hodnoty jsou změněny jen pro účely tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e916f-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="e916f-614">Nakonfigurováním rozsahu procesoru malé hodnotami jsou zvyšuje šance na aktivační událost automatického škálování při moderování zatížení je umístěn na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e916f-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="e916f-615">![Změna cílového procesoru chcete být 20 až 40 procent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "změna cíl CPU, který má být 20 až 40 procent")</span><span class="sxs-lookup"><span data-stu-id="e916f-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="e916f-616">*Změna cílový procesor bude 20 až 40 procent*</span><span class="sxs-lookup"><span data-stu-id="e916f-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="e916f-617">Klikněte na tlačítko **Uložit** na panelu příkazů a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="e916f-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="e916f-618">Úloha 2 – zátěžové testování pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e916f-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="e916f-619">Teď, když **automatického škálování** byl nakonfigurován, musíte vytvořit **načíst projekt testu výkonu webu a** v sadě Visual Studio k vygenerování nějaké zatížení CPU na webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e916f-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="e916f-620">Otevřít **Visual Studio Ultimate 2013** a vyberte **soubor | Nové | Projekt...**  spusťte nové řešení.</span><span class="sxs-lookup"><span data-stu-id="e916f-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="e916f-621">![Vytvoření nového projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "vytvoření nového projektu")</span><span class="sxs-lookup"><span data-stu-id="e916f-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="e916f-622">*Vytvoření nového projektu*</span><span class="sxs-lookup"><span data-stu-id="e916f-622">*Creating a new project*</span></span>
2. <span data-ttu-id="e916f-623">V **nový projekt** dialogu **webový výkon a projekt zátěžového testu** pod **Visual C# | Test** kartu. Ujistěte se, že **rozhraní .NET Framework 4.5** je vybrána, pojmenujte projekt *WebAndLoadTestProject*, zvolte **umístění** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e916f-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="e916f-624">![Vytvoření nového projektu webového a zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "vytvoření nového projektu webového a zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="e916f-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="e916f-625">*Vytvoření nového projektu webového a zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="e916f-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="e916f-626">V **WebTest1.webtest** klikněte pravým tlačítkem na **WebTest1** uzel a klikněte na tlačítko **přidejte žádosti**.</span><span class="sxs-lookup"><span data-stu-id="e916f-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="e916f-627">![Žádost o přidání do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "přidáním požadavku do WebTest1")</span><span class="sxs-lookup"><span data-stu-id="e916f-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="e916f-628">*Žádost o přidání do WebTest1*</span><span class="sxs-lookup"><span data-stu-id="e916f-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="e916f-629">V **vlastnosti** okno nového uzlu žádost o aktualizaci **Url** vlastnost tak, aby odkazoval na adresu URL webové aplikace (například *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="e916f-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="e916f-630">![Změna vlastnosti adresy Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "při změně hodnoty vlastnosti adresy Url")</span><span class="sxs-lookup"><span data-stu-id="e916f-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="e916f-631">*Změna vlastnosti adresy Url*</span><span class="sxs-lookup"><span data-stu-id="e916f-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="e916f-632">V **WebTest1.webtest** okna, klikněte pravým tlačítkem na **WebTest1** a klikněte na tlačítko **přidejte smyčku...** .</span><span class="sxs-lookup"><span data-stu-id="e916f-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="e916f-633">![Přidání smyčky do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "přidání smyčky do WebTest1")</span><span class="sxs-lookup"><span data-stu-id="e916f-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="e916f-634">*Přidání smyčky do WebTest1*</span><span class="sxs-lookup"><span data-stu-id="e916f-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="e916f-635">V **přidat podmíněné pravidlo a položky do cyklu** dialogové okno, vyberte **smyčky For Loop** pravidla a upravit následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e916f-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

   1. <span data-ttu-id="e916f-636">**Ukončovací hodnota:** 1000</span><span class="sxs-lookup"><span data-stu-id="e916f-636">**Terminating value:** 1000</span></span>
   2. <span data-ttu-id="e916f-637">**Název parametru kontextu:** iterátoru</span><span class="sxs-lookup"><span data-stu-id="e916f-637">**Context Parameter Name:** Iterator</span></span>
   3. <span data-ttu-id="e916f-638">**Přírůstková hodnota:** 1</span><span class="sxs-lookup"><span data-stu-id="e916f-638">**Increment Value:** 1</span></span>

      <span data-ttu-id="e916f-639">![Vyberte pravidlo smyčku For a aktualizace vlastností](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "vyberte pravidlo smyčku For a aktualizace vlastností")</span><span class="sxs-lookup"><span data-stu-id="e916f-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

      <span data-ttu-id="e916f-640">*Vyberte pravidlo smyčku For a aktualizace vlastností*</span><span class="sxs-lookup"><span data-stu-id="e916f-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="e916f-641">V části **položky ve smyčce** vyberte žádosti, které jste předtím vytvořili pro první a poslední položka smyčky.</span><span class="sxs-lookup"><span data-stu-id="e916f-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="e916f-642">Klikněte na tlačítko **OK** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e916f-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="e916f-643">![Výběr položek první a poslední smyčky](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "výběr položek první a poslední smyčky")</span><span class="sxs-lookup"><span data-stu-id="e916f-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="e916f-644">*Výběr položek první a poslední smyčky*</span><span class="sxs-lookup"><span data-stu-id="e916f-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="e916f-645">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **WebAndLoadTestProject** projektu, rozbalte položku **přidat** nabídky a vybereme **zátěžového testu...** .</span><span class="sxs-lookup"><span data-stu-id="e916f-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="e916f-646">![Zátěžový Test do projektu přidáte, WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "zátěžový Test do projektu přidáte, WebAndLoadTestProject")</span><span class="sxs-lookup"><span data-stu-id="e916f-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="e916f-647">*Zátěžový Test do projektu přidáte, WebAndLoadTestProject*</span><span class="sxs-lookup"><span data-stu-id="e916f-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="e916f-648">V **Průvodce novým zátěžovým testem** dialogové okno, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="e916f-649">![Průvodce novým zátěžovým testem](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Průvodce novým zátěžovým testem")</span><span class="sxs-lookup"><span data-stu-id="e916f-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="e916f-650">*Průvodce novým zátěžovým testem*</span><span class="sxs-lookup"><span data-stu-id="e916f-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="e916f-651">V **scénář** stránce **nepoužívat čas přemýšlení** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="e916f-652">![Výběr nepoužívat čas přemýšlení](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "výběr nepoužívat čas přemýšlení")</span><span class="sxs-lookup"><span data-stu-id="e916f-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="e916f-653">*Výběr nepoužívat čas přemýšlení*</span><span class="sxs-lookup"><span data-stu-id="e916f-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="e916f-654">V **vzor zatížení** stránky, ujistěte se, že **konstantní zatížení** je vybraná možnost.</span><span class="sxs-lookup"><span data-stu-id="e916f-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="e916f-655">Změnit **počet uživatelů** nastavení **250** uživatele a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="e916f-656">![Změna počtu uživatelů na 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "změnu počtu uživatelů na 250")</span><span class="sxs-lookup"><span data-stu-id="e916f-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="e916f-657">*Změna počtu uživatelů na 250*</span><span class="sxs-lookup"><span data-stu-id="e916f-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="e916f-658">V **testovat Model kombinace** stránce **na základě pořadí sekvenčního testu** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="e916f-659">![Výběr modelu kombinace testů](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Výběr modelu kombinace testů")</span><span class="sxs-lookup"><span data-stu-id="e916f-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="e916f-660">*Výběr modelu kombinace testů*</span><span class="sxs-lookup"><span data-stu-id="e916f-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="e916f-661">V **Model kombinace testů** klikněte na **přidat...**  přidat test do kombinaci.</span><span class="sxs-lookup"><span data-stu-id="e916f-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="e916f-662">![Přidání testu do poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "přidání testu do poměru testů")</span><span class="sxs-lookup"><span data-stu-id="e916f-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="e916f-663">*Přidání testu do poměru testů*</span><span class="sxs-lookup"><span data-stu-id="e916f-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="e916f-664">V **přidat testy** dialogové okno, klikněte dvakrát na **WebTest1** přidat test **vybrané testy** seznamu.</span><span class="sxs-lookup"><span data-stu-id="e916f-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="e916f-665">Klikněte na tlačítko **OK** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e916f-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="e916f-666">![Přidat WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "přidání WebTest1 testu")</span><span class="sxs-lookup"><span data-stu-id="e916f-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="e916f-667">*Přidat WebTest1 test*</span><span class="sxs-lookup"><span data-stu-id="e916f-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="e916f-668">Zpátky **poměru testů** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="e916f-669">![Dokončení stránce poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "dokončení stránce poměru testů")</span><span class="sxs-lookup"><span data-stu-id="e916f-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="e916f-670">*Dokončení stránce poměru testů*</span><span class="sxs-lookup"><span data-stu-id="e916f-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="e916f-671">V **kombinaci sítí** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="e916f-672">![Kliknete na další na stránce kombinaci sítí](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "kliknete na další na stránce poměr sítí")</span><span class="sxs-lookup"><span data-stu-id="e916f-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="e916f-673">*Kliknutím na další stránce poměr sítí*</span><span class="sxs-lookup"><span data-stu-id="e916f-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="e916f-674">V **kombinace prohlížečů** stránce **Internet Explorer 10.0** jako typ prohlížeče a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="e916f-675">![Výběr typu prohlížeče](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "vyberete typ prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="e916f-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="e916f-676">*Vyberte typ prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="e916f-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="e916f-677">V **sady čítačů** klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e916f-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="e916f-678">![Kliknutím na další stránce sady čítačů](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "kliknutím na další stránce sady čítačů")</span><span class="sxs-lookup"><span data-stu-id="e916f-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="e916f-679">*Kliknutím na další stránce sady čítačů*</span><span class="sxs-lookup"><span data-stu-id="e916f-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="e916f-680">V **parametrů běhu** nastavte **trvání zátěžového testu** k **5 minut** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e916f-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="e916f-681">![Nastavení trvání zátěžového testu na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "nastavení trvání zátěžového testu na 5 minut")</span><span class="sxs-lookup"><span data-stu-id="e916f-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="e916f-682">*Nastavení trvání zátěžového testu na 5 minut*</span><span class="sxs-lookup"><span data-stu-id="e916f-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="e916f-683">V **Průzkumníka řešení**, dvakrát klikněte **Local.settings** souboru prozkoumat nastavení testu.</span><span class="sxs-lookup"><span data-stu-id="e916f-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="e916f-684">Ve výchozím nastavení používá Visual Studio místního počítače ke spuštění testů.</span><span class="sxs-lookup"><span data-stu-id="e916f-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-685">Alternativně můžete nakonfigurovat testovacího projektu pro spuštění zátěžových testů v cloudu pomocí **testovací plány Azure**.</span><span class="sxs-lookup"><span data-stu-id="e916f-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Azure Test Plans**.</span></span> <span data-ttu-id="e916f-686">Azure testovací plány poskytuje cloudové zátěžové testování služba, která simuluje zatížení realističtější, jak se vyhnout omezení místní prostředí jako kapacity procesoru, paměti a šířky pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="e916f-686">Azure Test Plans provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory, and network bandwidth.</span></span> <span data-ttu-id="e916f-687">Další informace o používání Azure testovací plány pro spuštění zátěžových testů, naleznete v tématu [scénáře testování zatížení](/azure/devops/test/load-test/overview?view=vsts).</span><span class="sxs-lookup"><span data-stu-id="e916f-687">For more information about using Azure Test Plans to run load tests, see [Load testing scenarios](/azure/devops/test/load-test/overview?view=vsts).</span></span>

    ![Nastavení testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="e916f-689">Úloha 3 – ověření automatického škálování</span><span class="sxs-lookup"><span data-stu-id="e916f-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="e916f-690">Nyní spusťte zátěžový test, který jste vytvořili v předchozí úloze a naleznete v tématu jak webové aplikace chová při zatížení.</span><span class="sxs-lookup"><span data-stu-id="e916f-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="e916f-691">V **Průzkumníka řešení**, dvakrát klikněte na panel **LoadTest1.loadtest** otevřete zátěžový test.</span><span class="sxs-lookup"><span data-stu-id="e916f-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="e916f-692">![Otevírání LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "otevírání LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="e916f-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="e916f-693">*Otevírání LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="e916f-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="e916f-694">V **LoadTest1.loadtest** okna, klikněte na první tlačítko na panelu nástrojů ke spuštění zátěžového testu.</span><span class="sxs-lookup"><span data-stu-id="e916f-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="e916f-695">![Spuštění zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "spuštění zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="e916f-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="e916f-696">*Spuštění zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="e916f-696">*Running the load test*</span></span>
3. <span data-ttu-id="e916f-697">Počkejte na dokončení zátěžového testu.</span><span class="sxs-lookup"><span data-stu-id="e916f-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-698">Zátěžový test simuluje několik uživatelů, kteří současně zasílat požadavky do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="e916f-699">Když test běží, můžete sledovat dostupné čítače, které chcete zjistit všechny chyby, upozornění nebo jiné informace související se zátěžovým testem.</span><span class="sxs-lookup"><span data-stu-id="e916f-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="e916f-700">![Zátěžový test spuštěn](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "čekání, až po dokončení zátěžového testu")</span><span class="sxs-lookup"><span data-stu-id="e916f-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="e916f-701">*Spuštění zátěžového testu*</span><span class="sxs-lookup"><span data-stu-id="e916f-701">*Load test running*</span></span>
4. <span data-ttu-id="e916f-702">Po dokončení testu, vraťte se do portálu pro správu a přejděte **škálování** stránce vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e916f-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="e916f-703">V části **kapacity** oddílu, měli byste vidět v grafu, že se automaticky nasadí novou instanci.</span><span class="sxs-lookup"><span data-stu-id="e916f-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![Automaticky nasadit novou instanci](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="e916f-705">*Automaticky nasadit novou instanci*</span><span class="sxs-lookup"><span data-stu-id="e916f-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e916f-706">Může trvat několik minut, než se změna projeví v grafu (stiskněte **CTRL + F5** pravidelně obnovíte stránku).</span><span class="sxs-lookup"><span data-stu-id="e916f-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="e916f-707">Pokud se nezobrazí žádné změny, zkuste následující:</span><span class="sxs-lookup"><span data-stu-id="e916f-707">If you do not see any changes, you can try the following:</span></span>
    >
    > - <span data-ttu-id="e916f-708">Prodloužit dobu trvání zátěžového testu (například na **10 minut**)</span><span class="sxs-lookup"><span data-stu-id="e916f-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="e916f-709">Omezit maximální a minimální hodnoty **cílový procesor** rozsah v konfiguraci automatického škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e916f-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="e916f-710">Spusťte zátěžový test v cloudu s využitím **testovací plány Azure**.</span><span class="sxs-lookup"><span data-stu-id="e916f-710">Run the load test in the cloud with **Azure Test Plans**.</span></span> <span data-ttu-id="e916f-711">Další informace o [zde](/azure/devops/test/load-test/index?view=vsts)</span><span class="sxs-lookup"><span data-stu-id="e916f-711">More information [here](/azure/devops/test/load-test/index?view=vsts)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e916f-712">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e916f-712">Summary</span></span>

<span data-ttu-id="e916f-713">V této praktické laboratoři jste zjistili, jak nastavit a nasadit aplikaci do produkční webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="e916f-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="e916f-714">Jste spustili pomocí detekce a aktualizaci vašich databází pomocí **migrace Entity Framework Code First**, pak navázat tak, že nasazování nových verzí vašeho webu pomocí **Git** a vrácení zpět k provádění nejnovější stabilní verze vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="e916f-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="e916f-715">Kromě toho jste zjistili, jak škálovat aplikace pomocí úložiště přesunout statického obsahu do kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e916f-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
