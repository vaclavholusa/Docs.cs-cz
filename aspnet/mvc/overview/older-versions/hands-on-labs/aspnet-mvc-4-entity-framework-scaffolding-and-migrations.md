---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace | Dokumentace Microsoftu
author: rick-anderson
description: Pokud se seznámíte s metodami kontroleru ASP.NET MVC 4, nebo dokončení &quot;Pomocníci, formuláře a ověřování&quot; praktických cvičení, byste měli vědět...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394202"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="61e63-103">ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace</span><span class="sxs-lookup"><span data-stu-id="61e63-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="61e63-104">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="61e63-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="61e63-105">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="61e63-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="61e63-106">Pokud se seznámíte s metodami kontroleru ASP.NET MVC 4, nebo dokončení &quot;Pomocníci, formuláře a ověřování&quot; praktických cvičení, je třeba si uvědomit, že spoustu logiky k vytvoření, aktualizace, seznamu a odebrat entitu dat se opakuje mezi aplikace.</span><span class="sxs-lookup"><span data-stu-id="61e63-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="61e63-107">Nemluvě, že pokud váš model obsahuje několik tříd k manipulaci s, budete chtít věnovat značný čas zápisu POST a GET metody akce pro každou operaci entity, jakož i každé zobrazení.</span><span class="sxs-lookup"><span data-stu-id="61e63-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="61e63-108">V tomto testovacím prostředí se dozvíte, jak používat generování uživatelského rozhraní ASP.NET MVC 4 k automatickému generování účaří vaší aplikaci CRUD (vytvoření, čtení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="61e63-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="61e63-109">Spouští se od jednoduchého modelu třídy a, aniž byste museli napsat jediný řádek kódu, vytvoříte kontroler, který bude obsahovat všechny operace CRUD, jakož i všechny nezbytné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="61e63-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="61e63-110">Po sestavení a spuštění jednoduché řešení, je nutné databázi aplikace, které jsou vygenerovány spolu s logikou MVC a zobrazení pro manipulaci s daty.</span><span class="sxs-lookup"><span data-stu-id="61e63-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="61e63-111">Kromě toho se dozvíte, jak je snadné použití migrace Entity Framework k provedení aktualizace modelu v rámci celé aplikace.</span><span class="sxs-lookup"><span data-stu-id="61e63-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="61e63-112">Migrace rozhraní Entity Framework umožňují měnit databáze po změně modelu jednoduchých krocích.</span><span class="sxs-lookup"><span data-stu-id="61e63-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="61e63-113">Pomocí všech těchto v úvahu budete moct sestavovat a spravovat webové aplikace efektivněji, využívat nejnovější funkce technologie ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="61e63-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="61e63-114">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="61e63-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="61e63-115">Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="61e63-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="61e63-116">Cíle</span><span class="sxs-lookup"><span data-stu-id="61e63-116">Objectives</span></span>

<span data-ttu-id="61e63-117">V tomto praktického testovacího prostředí se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="61e63-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="61e63-118">Použití ASP.NET generování uživatelského rozhraní pro operace CRUD v zařízení.</span><span class="sxs-lookup"><span data-stu-id="61e63-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="61e63-119">Změna databáze modelu s použitím migrace rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61e63-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="61e63-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61e63-120">Prerequisites</span></span>

<span data-ttu-id="61e63-121">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="61e63-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="61e63-122">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="61e63-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="61e63-123">Instalace</span><span class="sxs-lookup"><span data-stu-id="61e63-123">Setup</span></span>

<span data-ttu-id="61e63-124">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="61e63-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="61e63-125">Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="61e63-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="61e63-126">K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="61e63-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="61e63-127">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="61e63-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="61e63-128">Cvičení</span><span class="sxs-lookup"><span data-stu-id="61e63-128">Exercises</span></span>

<span data-ttu-id="61e63-129">Následující cvičení tvoří tohoto praktického testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="61e63-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="61e63-130">Migrace rozhraní Entity Framework pomocí generování uživatelského rozhraní ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="61e63-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="61e63-131">V tomto cvičení se sadou **End** složku, která obsahuje výsledný řešení byste měli získat po dokončení výkonu.</span><span class="sxs-lookup"><span data-stu-id="61e63-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="61e63-132">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím výkonu.</span><span class="sxs-lookup"><span data-stu-id="61e63-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="61e63-133">Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**</span><span class="sxs-lookup"><span data-stu-id="61e63-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="61e63-134">Cvičení 1: S migrací architektury Entity pomocí generování uživatelského rozhraní ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="61e63-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="61e63-135">Generování uživatelského rozhraní ASP.NET MVC poskytuje rychlý způsob, jak vygenerovat operace CRUD a standardizovaným způsobem, vytváření nezbytnou logiku, která umožňuje aplikacím komunikovat s databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="61e63-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="61e63-136">V tomto cvičení se dozvíte, jak používat generování uživatelského rozhraní ASP.NET MVC 4 s kódem nejprve vytvořit metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="61e63-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="61e63-137">Potom se dozvíte, jak aktualizovat váš model použití změn v databázi s použitím migrace rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61e63-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="61e63-138">Úloha 1 – Vytvoření nové technologie ASP.NET MVC 4 projektu pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="61e63-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="61e63-139">Pokud ještě není otevřený, začněte **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="61e63-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="61e63-140">Vyberte **soubor | Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="61e63-140">Select **File | New Project**.</span></span> <span data-ttu-id="61e63-141">V nový projekt dialogového okna, v části **Visual C# | Web** vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="61e63-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="61e63-142">Pojmenujte projekt tak, aby **MVC4andEFMigrations** a nastavte umístění **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="61e63-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="61e63-143">Nastavte **název řešení** k **začít** a zajištění **vytvořit adresář pro řešení** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="61e63-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="61e63-144">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e63-144">Click **OK**.</span></span>

    <span data-ttu-id="61e63-145">![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nové pole dialogové okno projektu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="61e63-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="61e63-146">*Nové pole dialogové okno projektu ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="61e63-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="61e63-147">V **nového projektu ASP.NET MVC 4** dialogové okno Vyberte **internetovou aplikaci** šablony a ujistěte se, že **Razor** je vybraný **zobrazovací modul**.</span><span class="sxs-lookup"><span data-stu-id="61e63-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="61e63-148">Klikněte na tlačítko **OK** pro vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="61e63-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="61e63-149">![Nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nové aplikace ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="61e63-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="61e63-150">*Nové aplikace ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="61e63-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="61e63-151">V Průzkumníku řešení klikněte pravým tlačítkem na **modely** a vyberte **přidat | Třída** vytvořit jednoduchou třídu osoba (POCO).</span><span class="sxs-lookup"><span data-stu-id="61e63-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="61e63-152">Pojmenujte ji **osoba** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e63-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="61e63-153">Otevřete třídu osoby a vložte následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="61e63-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="61e63-154">(Fragment - kódu *architektury ASP.NET MVC 4 a migrace rozhraní Entity Framework – vlastnosti osoby Ex1*)</span><span class="sxs-lookup"><span data-stu-id="61e63-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="61e63-155">Klikněte na tlačítko **sestavení | Vytvoření řešení** uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="61e63-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="61e63-156">![Sestavení aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "sestavení aplikace")</span><span class="sxs-lookup"><span data-stu-id="61e63-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="61e63-157">*Sestavení aplikace*</span><span class="sxs-lookup"><span data-stu-id="61e63-157">*Building the Application*</span></span>
7. <span data-ttu-id="61e63-158">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat | Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="61e63-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="61e63-159">Název kontroleru *PersonController* a proveďte **možnosti generování uživatelského rozhraní** s použitím následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="61e63-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="61e63-160">V **šablony** rozevíracího seznamu, vyberte **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s využitím rozhraní Entity Framework** možnost.</span><span class="sxs-lookup"><span data-stu-id="61e63-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="61e63-161">V **třída modelu** rozevíracího seznamu, vyberte **osoba** třídy.</span><span class="sxs-lookup"><span data-stu-id="61e63-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="61e63-162">V **třída kontextu dat** seznamu vyberte  **&lt;nový kontext dat... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="61e63-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="61e63-163">Vyberte libovolný název a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e63-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="61e63-164">V **zobrazení** rozevíracího seznamu, ujistěte se, že **Razor** zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="61e63-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="61e63-165">![Přidání kontroleru osoba s generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "přidání kontroleru osoba s generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="61e63-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="61e63-166">*Přidání kontroleru osoba s generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="61e63-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="61e63-167">Klikněte na tlačítko **přidat** můžete vytvořit nový kontroler pro osoby se generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="61e63-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="61e63-168">Nyní jste vygenerovali akce kontroleru, jakož i zobrazení.</span><span class="sxs-lookup"><span data-stu-id="61e63-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="61e63-169">![Po vytvoření kontroleru osoba s generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po vytvoření kontroleru osoba s generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="61e63-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="61e63-170">*Po vytvoření kontroleru osoba s generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="61e63-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="61e63-171">Otevřít **PersonController** třídy.</span><span class="sxs-lookup"><span data-stu-id="61e63-171">Open **PersonController** class.</span></span> <span data-ttu-id="61e63-172">Všimněte si, že úplné metody akcí CRUD byly vytvořeny automaticky.</span><span class="sxs-lookup"><span data-stu-id="61e63-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="61e63-173">![V kontroleru osoba](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "uvnitř osoba kontroleru")</span><span class="sxs-lookup"><span data-stu-id="61e63-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="61e63-174">*V kontroleru osoby*</span><span class="sxs-lookup"><span data-stu-id="61e63-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="61e63-175">Úloha 2 běžící aplikace</span><span class="sxs-lookup"><span data-stu-id="61e63-175">Task 2- Running the application</span></span>

<span data-ttu-id="61e63-176">V tomto okamžiku není nevytvořili databázi.</span><span class="sxs-lookup"><span data-stu-id="61e63-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="61e63-177">V této úloze se spustit aplikaci poprvé a otestujte operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="61e63-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="61e63-178">Databáze se vytvoří v reálném čase s Code First.</span><span class="sxs-lookup"><span data-stu-id="61e63-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="61e63-179">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="61e63-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="61e63-180">V prohlížeči, přidejte **/Person** na adresu URL a otevřete stránku osoby.</span><span class="sxs-lookup"><span data-stu-id="61e63-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="61e63-181">![Při prvním spuštění aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "při prvním spuštění aplikace")</span><span class="sxs-lookup"><span data-stu-id="61e63-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="61e63-182">*Aplikace: prvním spuštění*</span><span class="sxs-lookup"><span data-stu-id="61e63-182">*Application: first run*</span></span>
3. <span data-ttu-id="61e63-183">Nyní bude procházení stránek osoba a otestujte operace CRUD.</span><span class="sxs-lookup"><span data-stu-id="61e63-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="61e63-184">Klikněte na tlačítko **vytvořit nový** chcete přidat nové uživatele.</span><span class="sxs-lookup"><span data-stu-id="61e63-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="61e63-185">Zadejte jméno a příjmení a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="61e63-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="61e63-186">![Přidání nové osobě](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "přidání nové osobě")</span><span class="sxs-lookup"><span data-stu-id="61e63-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="61e63-187">*Přidání nové osobě*</span><span class="sxs-lookup"><span data-stu-id="61e63-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="61e63-188">V seznamu osoby můžete odstranit, upravit nebo přidat položky.</span><span class="sxs-lookup"><span data-stu-id="61e63-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="61e63-189">![seznam osob](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "seznam osob")</span><span class="sxs-lookup"><span data-stu-id="61e63-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="61e63-190">*Seznam osob*</span><span class="sxs-lookup"><span data-stu-id="61e63-190">*Person list*</span></span>
    3. <span data-ttu-id="61e63-191">Klikněte na tlačítko **podrobnosti** zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="61e63-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="61e63-192">![Podrobnosti o vaší osobě](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "podrobnosti uživatele")</span><span class="sxs-lookup"><span data-stu-id="61e63-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="61e63-193">*Podrobnosti uživatele*</span><span class="sxs-lookup"><span data-stu-id="61e63-193">*Person's details*</span></span>
4. <span data-ttu-id="61e63-194">Zavřete prohlížeč a vraťte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="61e63-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="61e63-195">Všimněte si, že vytvoříte celý CRUD entity osoba v rámci aplikace – z modelu, který má zobrazení – aniž byste museli napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="61e63-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="61e63-196">Úloha 3Aktualizace databáze pomocí migrace rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="61e63-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="61e63-197">V této úloze budete aktualizovat databázi pomocí migrace rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61e63-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="61e63-198">Zjistíte, jak snadné je změna modelu a zavedení změn ve vašich databázích pomocí funkce migrace rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61e63-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="61e63-199">Otevřete konzolu Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="61e63-199">Open the Package Manager Console.</span></span> <span data-ttu-id="61e63-200">Vyberte **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="61e63-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="61e63-201">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61e63-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="61e63-202">PMC</span><span class="sxs-lookup"><span data-stu-id="61e63-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="61e63-203">![Povolení migrace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "povolení migrace")</span><span class="sxs-lookup"><span data-stu-id="61e63-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="61e63-204">*Povolení migrace*</span><span class="sxs-lookup"><span data-stu-id="61e63-204">*Enabling migrations*</span></span>

    <span data-ttu-id="61e63-205">Příkaz povolení migrace vytvoří **migrace** složky, která obsahuje skript pro inicializaci databáze.</span><span class="sxs-lookup"><span data-stu-id="61e63-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="61e63-206">![Migrace složky](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "složky migrace")</span><span class="sxs-lookup"><span data-stu-id="61e63-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="61e63-207">*Migrace složky*</span><span class="sxs-lookup"><span data-stu-id="61e63-207">*Migrations folder*</span></span>
3. <span data-ttu-id="61e63-208">Otevřít **Configuration.cs** souboru ve složce migrace.</span><span class="sxs-lookup"><span data-stu-id="61e63-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="61e63-209">Vyhledejte konstruktoru třídy a změňte **AutomaticMigrationsEnabled** hodnota, která se *true*.</span><span class="sxs-lookup"><span data-stu-id="61e63-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="61e63-210">Otevřete třídu osoby a přidejte atribut pro křestní jméno osoby.</span><span class="sxs-lookup"><span data-stu-id="61e63-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="61e63-211">Pomocí tohoto nového atributu změníte model.</span><span class="sxs-lookup"><span data-stu-id="61e63-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="61e63-212">Vyberte **sestavení | Vytvoření řešení** v nabídce k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="61e63-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="61e63-213">![Sestavení aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "sestavení aplikace")</span><span class="sxs-lookup"><span data-stu-id="61e63-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="61e63-214">*Sestavení aplikace*</span><span class="sxs-lookup"><span data-stu-id="61e63-214">*Building the application*</span></span>
6. <span data-ttu-id="61e63-215">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61e63-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="61e63-216">PMC</span><span class="sxs-lookup"><span data-stu-id="61e63-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="61e63-217">Tento příkaz vyhledá změn v datové objekty a pak přidáte nezbytné příkazy k úpravám databáze odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="61e63-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="61e63-218">![Přidání křestní jméno](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "přidání křestní jméno")</span><span class="sxs-lookup"><span data-stu-id="61e63-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="61e63-219">*Přidání křestní jméno*</span><span class="sxs-lookup"><span data-stu-id="61e63-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="61e63-220">(Volitelné) Spuštěním následujícího příkazu vygenerujte skript SQL s rozdílové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="61e63-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="61e63-221">To vám umožní aktualizovat databázi ručně (v tomto případě není nutné), nebo použít změny v jiných databázích:</span><span class="sxs-lookup"><span data-stu-id="61e63-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="61e63-222">PMC</span><span class="sxs-lookup"><span data-stu-id="61e63-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="61e63-223">![Generování skriptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generování skriptu SQL")</span><span class="sxs-lookup"><span data-stu-id="61e63-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="61e63-224">*Generování skriptu SQL*</span><span class="sxs-lookup"><span data-stu-id="61e63-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="61e63-225">![Aktualizace skriptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizace skript SQL")</span><span class="sxs-lookup"><span data-stu-id="61e63-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="61e63-226">*Aktualizace skriptu SQL*</span><span class="sxs-lookup"><span data-stu-id="61e63-226">*SQL Script update*</span></span>
8. <span data-ttu-id="61e63-227">V konzole Správce balíčků zadejte následující příkaz k aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="61e63-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="61e63-228">PMC</span><span class="sxs-lookup"><span data-stu-id="61e63-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="61e63-229">![Aktualizace databáze](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizace databáze")</span><span class="sxs-lookup"><span data-stu-id="61e63-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="61e63-230">*Aktualizace databáze*</span><span class="sxs-lookup"><span data-stu-id="61e63-230">*Updating the Database*</span></span>

    <span data-ttu-id="61e63-231">Tím se přidá **MiddleName** sloupec v **lidé** tabulku tak, aby odpovídala stávající definici **osoba** třídy.</span><span class="sxs-lookup"><span data-stu-id="61e63-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="61e63-232">Jakmile se aktualizuje databázi, klikněte pravým tlačítkem na složku Kontroleru a vyberte **přidat | Kontroler** přidat osoba kontroleru znovu (dokončete pomocí stejných hodnot).</span><span class="sxs-lookup"><span data-stu-id="61e63-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="61e63-233">Tím se aktualizují existující metody a zobrazení přidáte nový atribut.</span><span class="sxs-lookup"><span data-stu-id="61e63-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="61e63-234">![Přidání kontroleru aktualizace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "přidává aktualizace kontroleru")</span><span class="sxs-lookup"><span data-stu-id="61e63-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="61e63-235">*Aktualizace kontroleru*</span><span class="sxs-lookup"><span data-stu-id="61e63-235">*Updating the controller*</span></span>
10. <span data-ttu-id="61e63-236">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="61e63-236">Click **Add**.</span></span> <span data-ttu-id="61e63-237">Vyberte hodnoty **přepsat PersonController.cs** a **přepsat přidružené zobrazení** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e63-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Přidání kontroleru přepsání](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="61e63-239">*Aktualizace kontroleru*</span><span class="sxs-lookup"><span data-stu-id="61e63-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="61e63-240">Task4 – spouštění aplikace</span><span class="sxs-lookup"><span data-stu-id="61e63-240">Task4- Running the application</span></span>

1. <span data-ttu-id="61e63-241">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="61e63-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="61e63-242">Otevřít **/Person**.</span><span class="sxs-lookup"><span data-stu-id="61e63-242">Open **/Person**.</span></span> <span data-ttu-id="61e63-243">Všimněte si, aby se zajistilo uchování dat, zatímco byl přidán sloupec křestní jméno.</span><span class="sxs-lookup"><span data-stu-id="61e63-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="61e63-244">![Přidání druhé křestní jméno](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "přidání druhé křestní jméno")</span><span class="sxs-lookup"><span data-stu-id="61e63-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="61e63-245">*Přidání druhé křestní jméno*</span><span class="sxs-lookup"><span data-stu-id="61e63-245">*Middle Name added*</span></span>
3. <span data-ttu-id="61e63-246">Vyberete-li **upravit**, bude možné přidat křestní jméno aktuálního osobě.</span><span class="sxs-lookup"><span data-stu-id="61e63-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="61e63-247">![Křestní jméno edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edition křestní jméno")</span><span class="sxs-lookup"><span data-stu-id="61e63-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="61e63-248">Souhrn</span><span class="sxs-lookup"><span data-stu-id="61e63-248">Summary</span></span>

<span data-ttu-id="61e63-249">V této praktických cvičení jste se naučili jednoduché kroky k vytvoření operace CRUD s ASP.NET MVC 4 Scaffolding pomocí libovolné třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="61e63-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="61e63-250">Pak jste se naučili provede aktualizaci začátku do konce v aplikaci – z databáze k zobrazení – s použitím migrace rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61e63-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="61e63-251">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="61e63-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="61e63-252">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="61e63-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="61e63-253">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="61e63-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="61e63-254">Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="61e63-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="61e63-255">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="61e63-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="61e63-256">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="61e63-256">Click on **Install Now**.</span></span> <span data-ttu-id="61e63-257">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="61e63-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="61e63-258">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="61e63-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="61e63-259">![Instalace sady Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="61e63-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="61e63-260">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="61e63-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="61e63-261">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="61e63-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="61e63-263">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="61e63-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="61e63-264">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="61e63-264">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="61e63-266">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="61e63-266">*Installation progress*</span></span>
6. <span data-ttu-id="61e63-267">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="61e63-267">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="61e63-269">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="61e63-269">*Installation completed*</span></span>
7. <span data-ttu-id="61e63-270">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="61e63-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="61e63-271">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="61e63-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="61e63-273">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="61e63-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="61e63-274">Příloha B: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="61e63-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="61e63-275">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="61e63-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="61e63-276">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="61e63-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="61e63-277">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="61e63-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="61e63-278">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="61e63-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="61e63-279">***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***</span><span class="sxs-lookup"><span data-stu-id="61e63-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="61e63-280">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="61e63-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="61e63-281">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="61e63-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="61e63-282">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="61e63-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="61e63-283">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="61e63-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="61e63-284">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="61e63-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="61e63-285">![Začněte psát název fragmentu kódu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="61e63-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="61e63-286">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="61e63-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="61e63-287">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="61e63-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="61e63-288">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="61e63-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="61e63-289">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="61e63-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="61e63-290">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="61e63-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="61e63-291">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="61e63-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="61e63-292">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="61e63-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="61e63-293">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="61e63-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="61e63-294">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="61e63-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="61e63-295">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="61e63-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="61e63-296">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="61e63-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="61e63-297">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="61e63-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="61e63-298">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="61e63-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
