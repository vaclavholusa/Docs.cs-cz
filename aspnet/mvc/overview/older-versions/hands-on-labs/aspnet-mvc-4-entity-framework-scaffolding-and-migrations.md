---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace | Microsoft Docs
author: rick-anderson
description: Pokud se seznámíte s metody kontroleru architektury ASP.NET MVC 4, nebo byly dokončeny &quot;pomocné rutiny, formulářů a ověřování&quot; praktické cvičení, byste měli vědět...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 42a12ee39223a06054382dbe9b4784196a706216
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306842"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="e2b62-103">ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace</span><span class="sxs-lookup"><span data-stu-id="e2b62-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="e2b62-104">Podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e2b62-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e2b62-105">Stažení webové táborech cvičení Kit</span><span class="sxs-lookup"><span data-stu-id="e2b62-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e2b62-106">Pokud se seznámíte s metody kontroleru architektury ASP.NET MVC 4, nebo byly dokončeny &quot;pomocné rutiny, formulářů a ověřování&quot; praktických testovací prostředí, je třeba si uvědomit, že řadu logiku pro vytvoření, aktualizovat, seznamu a odebrat jakékoli data entity se opakuje mezi aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="e2b62-107">Nechcete zmínili, že pokud váš model má několik tříd k manipulaci, bude pravděpodobně tráví mnoho času zápis POST a GET metody akce pro každou operaci entity, jakož i každé z zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e2b62-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="e2b62-108">V tomto testovacím prostředí se dozvíte, jak pomocí generování uživatelského rozhraní ASP.NET MVC 4 automaticky generovat účaří CRUD vaší aplikace (vytvoření, čtení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="e2b62-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="e2b62-109">Spouštění z jednoduchého modelu třídy a, bez nutnosti napsat jediný řádek kódu, bude vytvoření kontroler, který bude obsahovat všechny operace CRUD a také všechny nezbytné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e2b62-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="e2b62-110">Po vytváření a spouštění jednoduchým řešením, je nutné databázi aplikace vygenerována, společně s logiku MVC a zobrazení pro manipulaci s daty.</span><span class="sxs-lookup"><span data-stu-id="e2b62-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="e2b62-111">Kromě toho se dozvíte, jak je snadné použití Entity Framework migrace k provedení aktualizací modelu v celé vaší celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2b62-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="e2b62-112">Entity Framework migrace vám umožní změnit databázi po změně modelu pomocí jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="e2b62-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="e2b62-113">Pomocí všech těchto pamatovat bude možné vytvářet a udržovat efektivněji, webových aplikací s využitím nejnovějších funkcí technologie ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e2b62-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="e2b62-114">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e2b62-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e2b62-115">Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="e2b62-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e2b62-116">Cíle</span><span class="sxs-lookup"><span data-stu-id="e2b62-116">Objectives</span></span>

<span data-ttu-id="e2b62-117">V tomto testovacím prostředí Hands-On se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="e2b62-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="e2b62-118">Pomocí ASP.NET generování uživatelského rozhraní pro operace CRUD v seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="e2b62-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="e2b62-119">Změňte model databáze pomocí migrace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2b62-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e2b62-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e2b62-120">Prerequisites</span></span>

<span data-ttu-id="e2b62-121">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="e2b62-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e2b62-122">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="e2b62-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e2b62-123">Instalace</span><span class="sxs-lookup"><span data-stu-id="e2b62-123">Setup</span></span>

<span data-ttu-id="e2b62-124">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="e2b62-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="e2b62-125">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2b62-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e2b62-126">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="e2b62-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e2b62-127">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2b62-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e2b62-128">Cvičení</span><span class="sxs-lookup"><span data-stu-id="e2b62-128">Exercises</span></span>

<span data-ttu-id="e2b62-129">Následujícím cvičení tvoří toto Hands-On testovací prostředí:</span><span class="sxs-lookup"><span data-stu-id="e2b62-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="e2b62-130">Pomocí generování uživatelského rozhraní ASP.NET MVC 4 migrace Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e2b62-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="e2b62-131">Tento postup je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení výkonu.</span><span class="sxs-lookup"><span data-stu-id="e2b62-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="e2b62-132">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení výkonu.</span><span class="sxs-lookup"><span data-stu-id="e2b62-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="e2b62-133">Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**</span><span class="sxs-lookup"><span data-stu-id="e2b62-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="e2b62-134">Cvičení 1: Pomocí generování uživatelského rozhraní ASP.NET MVC 4 migrace Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e2b62-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="e2b62-135">Generování uživatelského rozhraní aplikace ASP.NET MVC poskytuje rychlý způsob, jak vygenerovat operace CRUD a standardizovaným způsobem, vytváření nezbytné logiky, která umožňuje aplikaci používat v databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="e2b62-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="e2b62-136">V tomto cvičení se dozvíte, jak generování uživatelského rozhraní ASP.NET MVC 4 s kódem nejprve vytvořit pomocí metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="e2b62-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="e2b62-137">Potom se dozvíte, jak se bude aktualizovat váš model provádění změn v databázi pomocí Entity Framework migrace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="e2b62-138">Úloha 1 – Vytvoření nové rozhraní ASP.NET MVC 4 projekt pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="e2b62-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="e2b62-139">Pokud už otevřený, spusťte **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="e2b62-140">Vyberte **souboru | Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-140">Select **File | New Project**.</span></span> <span data-ttu-id="e2b62-141">V nový projekt dialogové okno, v části **Visual C# | Webové** vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e2b62-142">Pojmenujte projekt **MVC4andEFMigrations** a nastavte jeho umístění na **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e2b62-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="e2b62-143">Nastavte **název řešení** k **začít** a ujistěte se, **vytvořit adresář pro řešení** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="e2b62-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="e2b62-144">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-144">Click **OK**.</span></span>

    <span data-ttu-id="e2b62-145">![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "architektury ASP.NET MVC 4 projektu dialogové okno Nový")</span><span class="sxs-lookup"><span data-stu-id="e2b62-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="e2b62-146">*ASP.NET MVC 4 projektu dialogové okno Nový*</span><span class="sxs-lookup"><span data-stu-id="e2b62-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="e2b62-147">V **nový ASP.NET MVC 4 projekt** dialogovém okně vyberte pole **Internetové aplikace** šablony a ujistěte se, že **Razor** je vybraný **zobrazovací modul**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="e2b62-148">Klikněte na tlačítko **OK** a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="e2b62-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="e2b62-149">![Nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nové aplikace ASP.NET MVC 4 Internetu")</span><span class="sxs-lookup"><span data-stu-id="e2b62-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="e2b62-150">*Nové aplikace ASP.NET MVC 4 Internetu*</span><span class="sxs-lookup"><span data-stu-id="e2b62-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="e2b62-151">V Průzkumníku řešení klikněte pravým tlačítkem na **modely** a vyberte **přidat | Třída** vytvořit jednoduchou třídu osoba (objektů POCO).</span><span class="sxs-lookup"><span data-stu-id="e2b62-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="e2b62-152">Pojmenujte ji **osoba** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="e2b62-153">Otevřete třídu osoby a vložte následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e2b62-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="e2b62-154">(Code fragment kódu - *architektury ASP.NET MVC 4 a Entity Framework migrace - Ex1 osoba vlastnosti*)</span><span class="sxs-lookup"><span data-stu-id="e2b62-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="e2b62-155">Klikněte na tlačítko **sestavení | Vytvoření řešení** uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="e2b62-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="e2b62-156">![Vytváření aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "vytváření aplikace")</span><span class="sxs-lookup"><span data-stu-id="e2b62-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="e2b62-157">*Vytváření aplikace*</span><span class="sxs-lookup"><span data-stu-id="e2b62-157">*Building the Application*</span></span>
7. <span data-ttu-id="e2b62-158">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat | Řadič**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="e2b62-159">Název kontroleru *PersonController* a dokončete **možnosti generování uživatelského rozhraní** s následujícími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="e2b62-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="e2b62-160">V **šablony** rozevíracího seznamu, vyberte **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s využitím nástroje Entity Framework** možnost.</span><span class="sxs-lookup"><span data-stu-id="e2b62-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="e2b62-161">V **třída modelu** rozevíracího seznamu, vyberte **osoba** třídy.</span><span class="sxs-lookup"><span data-stu-id="e2b62-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="e2b62-162">V **třída kontextu dat** seznamu, vyberte  **&lt;nový kontext data... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="e2b62-163">Vyberte libovolný název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="e2b62-164">V **zobrazení** rozevíracího seznamu, ujistěte se, že **Razor** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="e2b62-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="e2b62-165">![Přidání řadiče osoba s generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "přidání řadiče osoba s generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="e2b62-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="e2b62-166">*Přidání řadiče osoba s generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="e2b62-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="e2b62-167">Klikněte na tlačítko **přidat** k vytvoření nového řadiče pro osoby s generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e2b62-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="e2b62-168">Byly generovány akce kontroleru, jakož i zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e2b62-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="e2b62-169">![Po vytvoření kontroleru osoba pomocí generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po vytvoření kontroleru osoba pomocí generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="e2b62-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="e2b62-170">*Po vytvoření kontroleru osoba pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="e2b62-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="e2b62-171">Otevřete **PersonController** třídy.</span><span class="sxs-lookup"><span data-stu-id="e2b62-171">Open **PersonController** class.</span></span> <span data-ttu-id="e2b62-172">Všimněte si, že úplné metody akce CRUD byly vytvořeny automaticky.</span><span class="sxs-lookup"><span data-stu-id="e2b62-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="e2b62-173">![Uvnitř řadičem osoba](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "uvnitř osoba řadiče")</span><span class="sxs-lookup"><span data-stu-id="e2b62-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="e2b62-174">*Uvnitř řadičem osoba*</span><span class="sxs-lookup"><span data-stu-id="e2b62-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="e2b62-175">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e2b62-175">Task 2- Running the application</span></span>

<span data-ttu-id="e2b62-176">V tomto okamžiku není ještě vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="e2b62-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="e2b62-177">V této úloze se spustit aplikaci poprvé a testování operací CRUD.</span><span class="sxs-lookup"><span data-stu-id="e2b62-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="e2b62-178">Databáze bude vytvořena na za chodu s Code First.</span><span class="sxs-lookup"><span data-stu-id="e2b62-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="e2b62-179">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e2b62-180">V prohlížeči přidat **/Person** na adresu URL, otevře se stránka osoby.</span><span class="sxs-lookup"><span data-stu-id="e2b62-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="e2b62-181">![Prvním spuštění aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prvním spuštění aplikace")</span><span class="sxs-lookup"><span data-stu-id="e2b62-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="e2b62-182">*Aplikace: prvním spuštění*</span><span class="sxs-lookup"><span data-stu-id="e2b62-182">*Application: first run*</span></span>
3. <span data-ttu-id="e2b62-183">Nyní se následující stránky osoby a testování operací CRUD.</span><span class="sxs-lookup"><span data-stu-id="e2b62-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="e2b62-184">Klikněte na tlačítko **vytvořit nový** chcete přidat nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e2b62-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="e2b62-185">Zadejte jméno a příjmení a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="e2b62-186">![Přidání nové osobě](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "přidání nové osobě")</span><span class="sxs-lookup"><span data-stu-id="e2b62-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="e2b62-187">*Přidání nové osobě*</span><span class="sxs-lookup"><span data-stu-id="e2b62-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="e2b62-188">V seznamu osoby můžete odstranit, upravit nebo přidat položky.</span><span class="sxs-lookup"><span data-stu-id="e2b62-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="e2b62-189">![seznam osob](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "seznam osob")</span><span class="sxs-lookup"><span data-stu-id="e2b62-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="e2b62-190">*Seznam osob*</span><span class="sxs-lookup"><span data-stu-id="e2b62-190">*Person list*</span></span>
    3. <span data-ttu-id="e2b62-191">Klikněte na tlačítko **podrobnosti** otevřete její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e2b62-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="e2b62-192">![Podrobnosti uživatele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "podrobnosti uživatele")</span><span class="sxs-lookup"><span data-stu-id="e2b62-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="e2b62-193">*Podrobnosti uživatele*</span><span class="sxs-lookup"><span data-stu-id="e2b62-193">*Person's details*</span></span>
4. <span data-ttu-id="e2b62-194">Zavřete prohlížeč a vraťte se k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2b62-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="e2b62-195">Všimněte si, že jste vytvořili celou CRUD pro entitu osoby v celé vaší aplikaci - z modelu, který má zobrazení – bez nutnosti napsat jediný řádek kódu!</span><span class="sxs-lookup"><span data-stu-id="e2b62-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="e2b62-196">Úloha 3Aktualizace databázi pomocí Entity Framework migrace</span><span class="sxs-lookup"><span data-stu-id="e2b62-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="e2b62-197">V této úloze zaktualizuje databázi pomocí Entity Framework migrace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="e2b62-198">Zjistíte, jak je snadné a změňte model projevily změny v databázích pomocí funkce migrace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2b62-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="e2b62-199">Otevřete konzolu Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e2b62-199">Open the Package Manager Console.</span></span> <span data-ttu-id="e2b62-200">Vyberte **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="e2b62-201">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e2b62-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="e2b62-202">POMOCÍ PMC</span><span class="sxs-lookup"><span data-stu-id="e2b62-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="e2b62-203">![Povolení migrace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "povolení migrace")</span><span class="sxs-lookup"><span data-stu-id="e2b62-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="e2b62-204">*Povolení migrace*</span><span class="sxs-lookup"><span data-stu-id="e2b62-204">*Enabling migrations*</span></span>

    <span data-ttu-id="e2b62-205">Příkaz Enable-migraci vytvoří **migrace** složky, která obsahuje skriptu k chybě při inicializaci databáze.</span><span class="sxs-lookup"><span data-stu-id="e2b62-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="e2b62-206">![Složku migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "složku Migrations")</span><span class="sxs-lookup"><span data-stu-id="e2b62-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="e2b62-207">*Složku migrations*</span><span class="sxs-lookup"><span data-stu-id="e2b62-207">*Migrations folder*</span></span>
3. <span data-ttu-id="e2b62-208">Otevřete **Configuration.cs** soubor ve složce migrace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="e2b62-209">Vyhledejte konstruktoru třídy a změňte **AutomaticMigrationsEnabled** hodnotu *true*.</span><span class="sxs-lookup"><span data-stu-id="e2b62-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="e2b62-210">Otevřete osoba třídu a přidejte atribut pro křestní jméno osoby.</span><span class="sxs-lookup"><span data-stu-id="e2b62-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="e2b62-211">Pomocí tohoto nového atributu změníte model.</span><span class="sxs-lookup"><span data-stu-id="e2b62-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="e2b62-212">Vyberte **sestavení | Vytvoření řešení** v nabídce pro sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="e2b62-213">![Vytváření aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "vytváření aplikace")</span><span class="sxs-lookup"><span data-stu-id="e2b62-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="e2b62-214">*Vytváření aplikace*</span><span class="sxs-lookup"><span data-stu-id="e2b62-214">*Building the application*</span></span>
6. <span data-ttu-id="e2b62-215">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e2b62-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="e2b62-216">POMOCÍ PMC</span><span class="sxs-lookup"><span data-stu-id="e2b62-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="e2b62-217">Tento příkaz bude hledat změny v datových objektů, a potom přidá potřebné příkazy k úpravám databáze odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e2b62-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="e2b62-218">![Přidání křestní jméno](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "přidání křestní jméno")</span><span class="sxs-lookup"><span data-stu-id="e2b62-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="e2b62-219">*Přidání křestní jméno*</span><span class="sxs-lookup"><span data-stu-id="e2b62-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="e2b62-220">(Volitelné) Spuštěním následujícího příkazu vygenerovat skript SQL s rozdílové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="e2b62-221">To vám umožní aktualizovat databázi ručně (v takovém případě není nutné), nebo použít změny v ostatních databázích:</span><span class="sxs-lookup"><span data-stu-id="e2b62-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="e2b62-222">POMOCÍ PMC</span><span class="sxs-lookup"><span data-stu-id="e2b62-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="e2b62-223">![Generování skriptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generování skriptu SQL")</span><span class="sxs-lookup"><span data-stu-id="e2b62-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="e2b62-224">*Generování skriptu SQL*</span><span class="sxs-lookup"><span data-stu-id="e2b62-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="e2b62-225">![Skript SQL aktualizace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizace skript SQL")</span><span class="sxs-lookup"><span data-stu-id="e2b62-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="e2b62-226">*Skript SQL aktualizace*</span><span class="sxs-lookup"><span data-stu-id="e2b62-226">*SQL Script update*</span></span>
8. <span data-ttu-id="e2b62-227">V konzole Správce balíčků zadejte následující příkaz k aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="e2b62-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="e2b62-228">POMOCÍ PMC</span><span class="sxs-lookup"><span data-stu-id="e2b62-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="e2b62-229">![Aktualizace databáze](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizace databáze")</span><span class="sxs-lookup"><span data-stu-id="e2b62-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="e2b62-230">*Aktualizace databáze*</span><span class="sxs-lookup"><span data-stu-id="e2b62-230">*Updating the Database*</span></span>

    <span data-ttu-id="e2b62-231">Se přidá **MiddleName** sloupec v **osoby** tabulky tak, aby odpovídala aktuální definice **osoba** třídy.</span><span class="sxs-lookup"><span data-stu-id="e2b62-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="e2b62-232">Po aktualizaci databáze, klikněte pravým tlačítkem na složku řadiče a vyberte **přidat | Řadič** pro přidání osoba řadiče znovu (dokončení se stejnými hodnotami).</span><span class="sxs-lookup"><span data-stu-id="e2b62-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="e2b62-233">Tím dojde k aktualizaci existující metody a zobrazení přidání nového atributu.</span><span class="sxs-lookup"><span data-stu-id="e2b62-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="e2b62-234">![Přidání řadiče aktualizace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "přidání řadiče aktualizace")</span><span class="sxs-lookup"><span data-stu-id="e2b62-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="e2b62-235">*Aktualizace kontroleru*</span><span class="sxs-lookup"><span data-stu-id="e2b62-235">*Updating the controller*</span></span>
10. <span data-ttu-id="e2b62-236">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-236">Click **Add**.</span></span> <span data-ttu-id="e2b62-237">Potom vyberte hodnoty **přepsat PersonController.cs** a **přepsat přidružené zobrazení** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Přidání přepsat řadiče](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="e2b62-239">*Aktualizace kontroleru*</span><span class="sxs-lookup"><span data-stu-id="e2b62-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="e2b62-240">Task4 - spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e2b62-240">Task4- Running the application</span></span>

1. <span data-ttu-id="e2b62-241">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b62-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e2b62-242">Otevřete **/Person**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-242">Open **/Person**.</span></span> <span data-ttu-id="e2b62-243">Všimněte si, že byla data zachována, při sloupci Křestní jméno byla přidána.</span><span class="sxs-lookup"><span data-stu-id="e2b62-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="e2b62-244">![Křestní jméno přidat](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "přidat křestní jméno")</span><span class="sxs-lookup"><span data-stu-id="e2b62-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="e2b62-245">*Křestní jméno přidat*</span><span class="sxs-lookup"><span data-stu-id="e2b62-245">*Middle Name added*</span></span>
3. <span data-ttu-id="e2b62-246">Pokud kliknete na tlačítko **upravit**, nebudete moct přidat křestní jméno aktuální osobě.</span><span class="sxs-lookup"><span data-stu-id="e2b62-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="e2b62-247">![Křestní jméno edice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edice křestní jméno")</span><span class="sxs-lookup"><span data-stu-id="e2b62-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e2b62-248">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e2b62-248">Summary</span></span>

<span data-ttu-id="e2b62-249">V tomto testovacím prostředí praktických jste se naučili jednoduché kroky k vytvoření operace CRUD s ASP.NET MVC 4 generování pomocí libovolné třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="e2b62-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="e2b62-250">Pak jste zjistili, jak provést aktualizaci koncová ve vaší aplikaci - z databáze k zobrazení - pomocí migrace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2b62-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e2b62-251">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="e2b62-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e2b62-252">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e2b62-253">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e2b62-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e2b62-254">Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e2b62-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e2b62-255">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2b62-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e2b62-256">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-256">Click on **Install Now**.</span></span> <span data-ttu-id="e2b62-257">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="e2b62-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e2b62-258">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="e2b62-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e2b62-259">![Nainstalovat Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e2b62-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e2b62-260">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e2b62-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e2b62-261">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e2b62-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="e2b62-263">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="e2b62-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e2b62-264">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="e2b62-264">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="e2b62-266">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="e2b62-266">*Installation progress*</span></span>
6. <span data-ttu-id="e2b62-267">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-267">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="e2b62-269">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="e2b62-269">*Installation completed*</span></span>
7. <span data-ttu-id="e2b62-270">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="e2b62-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e2b62-271">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="e2b62-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="e2b62-273">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="e2b62-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="e2b62-274">Příloha B: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="e2b62-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="e2b62-275">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="e2b62-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e2b62-276">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e2b62-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e2b62-277">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="e2b62-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e2b62-278">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="e2b62-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e2b62-279">***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***</span><span class="sxs-lookup"><span data-stu-id="e2b62-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e2b62-280">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="e2b62-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e2b62-281">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="e2b62-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e2b62-282">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="e2b62-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e2b62-283">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="e2b62-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e2b62-284">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="e2b62-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e2b62-285">![Začněte psát název fragmentu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="e2b62-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e2b62-286">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="e2b62-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="e2b62-287">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="e2b62-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e2b62-288">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="e2b62-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e2b62-289">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="e2b62-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e2b62-290">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="e2b62-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e2b62-291">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e2b62-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e2b62-292">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="e2b62-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e2b62-293">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="e2b62-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e2b62-294">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="e2b62-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e2b62-295">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="e2b62-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e2b62-296">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="e2b62-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e2b62-297">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="e2b62-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e2b62-298">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="e2b62-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
