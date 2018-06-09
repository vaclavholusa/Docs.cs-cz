---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Pomocné rutiny ASP.NET MVC 4, formulářů a ověřování | Microsoft Docs
author: rick-anderson
description: V technologii ASP.NET MVC 4 modely a Data laboratoř Hands-on přístupu máte byla načítání a zobrazení dat z databáze. V tomto testovacím prostředí Hands-on přidáte...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30878175"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="ec7f5-104">Pomocné rutiny ASP.NET MVC 4, formulářů a ověřování</span><span class="sxs-lookup"><span data-stu-id="ec7f5-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="ec7f5-105">Podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="ec7f5-106">Stažení webové táborech cvičení Kit</span><span class="sxs-lookup"><span data-stu-id="ec7f5-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="ec7f5-107">V **ASP.NET MVC 4 modely a přístup k datům** Hands-on testovací prostředí, jste načítání a zobrazení dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="ec7f5-108">V tomto testovacím prostředí Hands-on přidáte **Hudba úložiště** aplikace možnost upravovat data.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="ec7f5-109">S Pamatujte, že cílem nejprve vytvoříte řadič, která bude podporovat vytvoření, čtení, aktualizace a odstranění (CRUD) akce alb.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="ec7f5-110">Vygeneruje šablonu zobrazení indexu využívat výhod funkce generování uživatelského rozhraní ASP.NET MVC zobrazíte vlastnosti alb do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="ec7f5-111">K vylepšení tohoto zobrazení, přidáte vlastní pomocné rutiny HTML, který bude zkrátit dlouhý popis.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="ec7f5-112">Bude později, přidat, upravit a vytvořit zobrazení, které vám umožní změnit alb v databázi, pomocí elementů formuláře jako rozevíracích seznamů.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="ec7f5-113">Nakonec vám umožní uživatelům odstranění alba a také můžete zabránit tomu, zadávání chybná data tím, že ověří jejich vstup.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="ec7f5-114">Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="ec7f5-115">Pokud jste nepoužili **ASP.NET MVC** před, doporučujeme si projít **ASP.NET MVC Základy** Hands-on testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="ec7f5-116">Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="ec7f5-117">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="ec7f5-118">Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 pomocné rutiny, formulářů a ověřování](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="ec7f5-119">Cíle</span><span class="sxs-lookup"><span data-stu-id="ec7f5-119">Objectives</span></span>

<span data-ttu-id="ec7f5-120">V tomto testovacím prostředí Hands-On se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="ec7f5-121">Vytvoření kontroleru podporovat operace CRUD</span><span class="sxs-lookup"><span data-stu-id="ec7f5-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="ec7f5-122">Generovat zobrazení Index pro zobrazení vlastností entity do tabulky HTML</span><span class="sxs-lookup"><span data-stu-id="ec7f5-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="ec7f5-123">Přidat vlastní pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="ec7f5-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="ec7f5-124">Vytvářet a přizpůsobovat zobrazení o upravit</span><span class="sxs-lookup"><span data-stu-id="ec7f5-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="ec7f5-125">Rozlišit mezi metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání</span><span class="sxs-lookup"><span data-stu-id="ec7f5-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="ec7f5-126">Přidání a přizpůsobení vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="ec7f5-126">Add and customize a Create View</span></span>
- <span data-ttu-id="ec7f5-127">Zpracování odstranění entity</span><span class="sxs-lookup"><span data-stu-id="ec7f5-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="ec7f5-128">Ověření vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="ec7f5-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ec7f5-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec7f5-129">Prerequisites</span></span>

<span data-ttu-id="ec7f5-130">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="ec7f5-131">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ec7f5-132">Instalace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-132">Setup</span></span>

<span data-ttu-id="ec7f5-133">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="ec7f5-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="ec7f5-134">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="ec7f5-135">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="ec7f5-136">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ec7f5-137">Cvičení</span><span class="sxs-lookup"><span data-stu-id="ec7f5-137">Exercises</span></span>

<span data-ttu-id="ec7f5-138">Následující cvičení tvoří toto Hands-On testovací prostředí:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="ec7f5-139">Vytváření řadič úložiště Manager a jeho zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="ec7f5-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="ec7f5-140">Přidání pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="ec7f5-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="ec7f5-141">Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="ec7f5-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="ec7f5-142">Přidání zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="ec7f5-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="ec7f5-143">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="ec7f5-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="ec7f5-144">Přidání ověření</span><span class="sxs-lookup"><span data-stu-id="ec7f5-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="ec7f5-145">Pomocí Nerušivý jQuery na straně klienta</span><span class="sxs-lookup"><span data-stu-id="ec7f5-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="ec7f5-146">Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="ec7f5-147">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="ec7f5-148">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="ec7f5-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="ec7f5-149">Cvičení 1: Vytvoření řadič úložiště Manager a jeho zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="ec7f5-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="ec7f5-150">V tomto cvičení se dozvíte, jak vytvořit nový řadič pro podporu operací CRUD, přizpůsobit její metody akce Index k zobrazení seznamu alb z databáze a nakonec generování šablonu zobrazení indexu využívat výhod generování uživatelského rozhraní ASP.NET MVC Funkce zobrazíte vlastnosti alb do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="ec7f5-151">Úloha 1 – Vytvoření StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="ec7f5-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="ec7f5-152">V této úloze se vytvoří nový řadič názvem **StoreManagerController** pro podporu operací CRUD.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="ec7f5-153">Otevřete **začít** řešení nacházející se v **zdroj/Ex1-CreatingTheStoreManagerController/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="ec7f5-154">Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-155">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-156">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-157">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-158">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-159">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-160">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-161">Přidání nového řadiče.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-161">Add a new controller.</span></span> <span data-ttu-id="ec7f5-162">Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složky v Průzkumníku řešení, vyberte **přidat** a potom **řadič** příkaz.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="ec7f5-163">Změna **řadič** **název** k **StoreManagerController** a zajistěte, aby možnost **kontroler MVC s akcemi čtení/zápisu-prázdný**je vybrána.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="ec7f5-164">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-164">Click **Add**.</span></span>

    <span data-ttu-id="ec7f5-165">![Dialogové okno Přidat řadič](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "dialogové okno Přidat kontroler")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="ec7f5-166">*Dialogové okno řadiče přidání*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="ec7f5-167">Generuje se nové třídy Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-167">A new Controller class is generated.</span></span> <span data-ttu-id="ec7f5-168">Vzhledem k tomu, že uvedené přidat akce pro čtení a zápis, se zakázaným inzerováním metody pro ty, jsou vytvořeny běžné akce CRUD s komentáře TODO vyplněno, výzvy zahrnout určitou logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="ec7f5-169">Úloha 2 – přizpůsobení StoreManager indexu</span><span class="sxs-lookup"><span data-stu-id="ec7f5-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="ec7f5-170">V této úloze budete přizpůsobovat StoreManager Index metody akce lze vrátit zobrazení se seznamem alb z databáze.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="ec7f5-171">Ve třídě StoreManagerController přidejte následující *pomocí* direktivy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="ec7f5-172">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – Ex1 pomocí MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="ec7f5-173">Přidat pole do **StoreManagerController** pro uložení instanci **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="ec7f5-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="ec7f5-174">(Code fragment kódu - *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování – Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="ec7f5-175">Implementujte StoreManagerController indexu akce lze vrátit zobrazení se seznamem alb.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="ec7f5-176">Logice Kontroleru akce bude velmi podobná akce indexu StoreController zapsána dříve.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="ec7f5-177">Umožňuje načíst všechny alb, včetně informací o Genre a umělcem pro zobrazení LINQ.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="ec7f5-178">(Code fragment kódu - *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování – Ex1 StoreManagerController Index*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="ec7f5-179">Úloha 3 – vytvoření zobrazení pro Index</span><span class="sxs-lookup"><span data-stu-id="ec7f5-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="ec7f5-180">V této úloze, vytvoříte šablony zobrazení Index pro zobrazení seznamu alb vrácený **StoreManager** řadiče.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="ec7f5-181">Před vytvořením nové šablony zobrazení, měli byste vytvořit projekt tak, aby **Přidat Dialog zobrazení** zná **Album** třídu se má použít.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="ec7f5-182">Vyberte **sestavení | Sestavení MvcMusicStore** a tím projekt sestavit.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="ec7f5-183">Klepněte pravým tlačítkem myši **Index** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="ec7f5-184">Tím se otevře **přidat zobrazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="ec7f5-185">![Přidání zobrazení](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "přidat zobrazení")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="ec7f5-186">*Přidání zobrazení v aplikaci Index – metoda*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="ec7f5-187">V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **Index**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="ec7f5-188">Vyberte **vytvořit zobrazení silného typu** a vyberte možnost **Album (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="ec7f5-189">Vyberte **seznamu** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="ec7f5-190">Ponechte **zobrazovací modul** k **Razor** a v ostatních polích s jejich výchozí hodnoty a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="ec7f5-191">![Přidání zobrazení index se o](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "přidání zobrazení indexu")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="ec7f5-192">*Přidání zobrazení indexu*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="ec7f5-193">Úloha 4 – přizpůsobení vygenerovaného uživatelského rozhraní zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="ec7f5-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="ec7f5-194">V této úloze se upraví jednoduché zobrazení šablony vytvořené pomocí funkce generování uživatelského rozhraní ASP.NET MVC jej pole, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="ec7f5-195">**Generování uživatelského rozhraní** podpory v rámci rozhraní ASP.NET MVC vygeneruje šablonu jednoduché zobrazení, která uvádí všechna pole v modelu alb.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="ec7f5-196">**Generování uživatelského rozhraní** poskytuje rychlý způsob, jak začít pracovat na zobrazení se silnými typy: místo nutnosti psaní zobrazit šablonu ručně, generování uživatelského rozhraní rychle generuje výchozí šablonu a upravte generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="ec7f5-197">Zkontrolujte kód vytvořený.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-197">Review the code created.</span></span> <span data-ttu-id="ec7f5-198">Vygenerovaný seznam polí bude součástí následující tabulky HTML, který **generování uživatelského rozhraní** používá pro zobrazení tabulková data.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="ec7f5-199">Nahraďte **&lt;tabulky&gt;** kódu pomocí následující kód k zobrazení pouze **Genre**, **umělcem**, **název alba**, a **cena** pole.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="ec7f5-200">To odstraní **AlbumId** a **alb obrázky URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="ec7f5-201">Také změní GenreId a ArtistId sloupce k zobrazení jejich vlastnosti propojené třída **Artist.Name** a **Genre.Name**a odebere **podrobnosti** odkaz.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="ec7f5-202">Změňte v následujících popisech.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="ec7f5-203">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="ec7f5-204">V této úloze budete testovat, **StoreManager** **Index** zobrazit šablonu zobrazí seznam alb podle návrhu předchozí kroky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="ec7f5-205">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-206">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-206">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-207">Změňte adresu URL na **/StoreManager** k ověření, že se zobrazí seznam alb, zobrazení jejich **název**, **umělcem** a **Genre**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="ec7f5-208">![Procházení seznamu alb](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "procházení seznam alb")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="ec7f5-209">*Procházení seznamu alb*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="ec7f5-210">Cvičení 2: Přidání pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="ec7f5-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="ec7f5-211">StoreManager indexovou stránku má jeden potenciální potíže: umělcem název a název vlastnosti může být dostatečně dlouhé, aby throw vypnout formátování tabulky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="ec7f5-212">V tomto cvičení se dozvíte, jak přidat vlastní pomocné rutiny HTML zkrátit tento text.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="ec7f5-213">Na následujícím obrázku vidíte, jak formát se mění z důvodu délka textu při použití na velikost malých prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="ec7f5-214">![Procházení seznamu alb s nezkrátila text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "procházení seznam alb s nezkrátila textu")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="ec7f5-215">*Procházení seznamu alb s nezkrátila textu*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="ec7f5-216">Úloha 1 – rozšíření pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="ec7f5-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="ec7f5-217">V této úloze budete přidávat nové metody **Truncate** k **HTML** objekt vystavený v zobrazení ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="ec7f5-218">K tomu budete implementovat **metoda rozšíření** do vestavěné **System.Web.Mvc.HtmlHelper** třída poskytuje rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="ec7f5-219">Další informace o **rozšiřující metody**, navštivte tohoto článku na webu msdn.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="ec7f5-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="ec7f5-221">Otevřete **začít** řešení nacházející se v **zdroj/Ex2-AddingAnHTMLHelper/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="ec7f5-222">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ec7f5-223">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-224">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-225">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-226">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-227">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-228">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-229">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-230">Otevřete zobrazení StoreManager na Index.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="ec7f5-231">Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak se **StoreManager** a otevřete **Index.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="ec7f5-232">Přidejte následující kód níže <strong>@model</strong> – direktiva k definování <strong>Truncate</strong> metodu helper.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="ec7f5-233">Úloha 2 - zkracování textu na stránce</span><span class="sxs-lookup"><span data-stu-id="ec7f5-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="ec7f5-234">V této úloze budete používat **Truncate** metoda zkrátit text v šabloně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="ec7f5-235">Otevřete zobrazení StoreManager na Index.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="ec7f5-236">Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak se **StoreManager** a otevřete **Index.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="ec7f5-237">Nahradit řádky, které ukazují **umělcem název** a alba **název**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="ec7f5-238">Chcete-li to provést, nahraďte následující řádky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="ec7f5-239">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="ec7f5-240">V této úloze budete testovat, **StoreManager** **Index** zobrazit šablonu zkrátí titul alba a umělcem jméno.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="ec7f5-241">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-242">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-242">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-243">Změňte adresu URL na **/StoreManager** k ověření tohoto dlouho texty v **název** a **umělcem** sloupec se zkrátí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="ec7f5-244">![Zkrátí názvy produktů a umělci](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "zkrácen názvy produktů a umělci")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="ec7f5-245">*Zkrácený názvy a umělcem názvy*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="ec7f5-246">Cvičení 3: Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="ec7f5-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="ec7f5-247">V tomto cvičení se dozvíte, jak vytvořit formulář umožňuje správcům úložiště upravit Album.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="ec7f5-248">Bude procházení **/StoreManager/Edit/id** adresa URL (**id** probíhá jedinečné id alba, chcete-li upravit), díky čemuž volání GET protokolu HTTP na server.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="ec7f5-249">Bude metoda akce Kontroleru upravit načtení příslušné Album z databáze, vytvořte **StoreManagerViewModel** objekt, který chcete zapouzdření (spolu s seznam umělci a žánry) a předejte ji do šablonu zobrazení vykreslení stránky HTML zpět na uživatele.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="ec7f5-250">Tato stránka bude obsahovat **&lt;formuláře&gt;** element s textových polí a rozevírací seznamy pro úpravy vlastnosti alba.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="ec7f5-251">Jakmile je uživatel aktualizace hodnot formuláře alba a klikne **Uložit** tlačítko, změny se odešlou přes HTTP POST zpětné volání pro **/StoreManager/Edit/id**. I když adresa URL zůstává stejná jako v posledním volání, ASP.NET MVC identifikuje, že tuto chvíli je HTTP POST a proto provede jinou metodu akce úpravy (jeden označených pomocí **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="ec7f5-252">Úloha 1 – implementace metody akce HTTP GET úpravy</span><span class="sxs-lookup"><span data-stu-id="ec7f5-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="ec7f5-253">V této úloze budete implementovat verze HTTP GET metody akce úpravy k načtení příslušné Album z databáze, a také seznam všech žánry a umělci.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="ec7f5-254">Tato data se bude balíček do **StoreManagerViewModel** objekt definovaný v posledním kroku, který se potom předá šablonu zobrazení k vykreslení odpovědi s.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="ec7f5-255">Otevřete **začít** řešení nacházející se v **zdroj/EX3.-CreatingTheEditView/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="ec7f5-256">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ec7f5-257">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-258">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-259">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-260">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-261">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-262">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-263">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-264">Otevřete **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="ec7f5-265">Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="ec7f5-266">Nahraďte **HTTP GET upravit** metoda akce s následující kód k načtení příslušné **Album** společně s **žánry** a **umělci**uvádí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="ec7f5-267">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – EX3. StoreManagerController HTTP GET upravit akce*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-268">Používáte **System.Web.Mvc** **SelectList** umělci a žánry místo **System.Collections.Generic** seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="ec7f5-269">**SelectList** je čisticí způsob, jak naplnit rozevírací seznamy HTML a spravovat takové věci, jako aktuální výběr.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="ec7f5-270">Vytváření instancí a novější nastavením tyto objekty ViewModel v akce kontroleru bude scénář formuláře upravit čisticí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="ec7f5-271">Úloha 2 – Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="ec7f5-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="ec7f5-272">V této úloze vytvoříte šablonu upravit zobrazení, které se zobrazí později vlastnosti alba.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="ec7f5-273">Vytvoření zobrazení pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-273">Create the Edit View.</span></span> <span data-ttu-id="ec7f5-274">Chcete-li to provést, klikněte pravým tlačítkem uvnitř **upravit** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="ec7f5-275">V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **upravit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="ec7f5-276">Zkontrolujte **vytvořit zobrazení silného typu** zaškrtávací políčko a vyberte **Album (MvcMusicStore.Models)** z **zobrazit třída dat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="ec7f5-277">Vyberte **upravit** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="ec7f5-278">V ostatních polích nechte výchozí hodnoty a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="ec7f5-279">![Přidání zobrazení](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "přidávání zobrazení")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="ec7f5-280">*Přidání zobrazení*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="ec7f5-281">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="ec7f5-282">V této úloze budete testovat, **StoreManager** **upravit** stránka zobrazení zobrazí vlastnosti hodnoty alba předán jako parametr.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="ec7f5-283">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-284">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-284">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-285">Změňte adresu URL na **/StoreManager/Edit/1** k ověření, že se zobrazují vlastnosti hodnoty alba předán.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="ec7f5-286">![Procházení zobrazení alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "procházení alba upravit zobrazení")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="ec7f5-287">*Procházení alba upravit zobrazení*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="ec7f5-288">Úloha 4 – implementace rozevírací seznamy v šabloně Album editoru</span><span class="sxs-lookup"><span data-stu-id="ec7f5-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="ec7f5-289">V této úloze přidáte rozevírací seznamy do zobrazení šablony vytvořené v posledním úkolem tak, aby si uživatel může vybrat ze seznamu umělci a žánry.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="ec7f5-290">Nahraďte všechny **Album** fieldset kódu pomocí následující:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-291">**Html.DropDownList** pomocná byla přidána k vykreslení rozevírací seznamy pro výběr umělci a žánry.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="ec7f5-292">Parametry předaný **Html.DropDownList** jsou:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="ec7f5-293">Název pole formuláře (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="ec7f5-294">**SelectList** hodnot pro rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="ec7f5-295">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="ec7f5-296">V této úloze budete testovat, **StoreManager** **upravit** stránka zobrazení zobrazí rozevírací seznamy místo umělcem a Genre ID textových polí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="ec7f5-297">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-298">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-298">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-299">Změňte adresu URL na **/StoreManager/Edit/1** a ověřte, že ho rozevírací seznamy místo umělcem a Genre ID textových polí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="ec7f5-300">![Procházení alba upravit zobrazení s rozevírací nabídky](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "procházení Album upravit zobrazení s rozevírací nabídky")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="ec7f5-301">*Procházení Album zobrazení, tentokrát pomocí rozevíracích seznamů*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="ec7f5-302">Úloha 6 – implementace metody akce HTTP POST upravit</span><span class="sxs-lookup"><span data-stu-id="ec7f5-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="ec7f5-303">Teď, když upravit zobrazení zobrazí podle očekávání, potřebujete implementovat metodu akce HTTP POST upravit uložit změny provedené album.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="ec7f5-304">Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="ec7f5-305">Otevřete **StoreManagerController** z **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="ec7f5-306">Nahraďte **HTTP POST upravit** kódu metoda akce s následující (Všimněte si, že je metoda, která se musí nahradit přetížené verze, která přijímá dva parametry):</span><span class="sxs-lookup"><span data-stu-id="ec7f5-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="ec7f5-307">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – EX3. StoreManagerController HTTP POST upravit akce*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-308">Tato metoda se provede, když uživatel klikne **Uložit** tlačítko zobrazení a provede POST protokolu HTTP-hodnot formuláře zpět na server aby je uchoval v databázi.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="ec7f5-309">Dekoratéra **[HttpPost]** označuje, že metoda by měla použít pro tyto scénáře HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="ec7f5-310">Tato metoda přebírá **Album** objektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-310">The method takes an **Album** object.</span></span> <span data-ttu-id="ec7f5-311">ASP.NET MVC automaticky vytvoří objekt Album z odeslaných &lt;formuláře&gt; hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="ec7f5-312">Metoda provede tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="ec7f5-313">Pokud je model platný:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="ec7f5-314">Aktualizujte položku album v kontextu označit jako upravený objekt.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="ec7f5-315">Uložte změny a přesměruje na zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="ec7f5-316">Pokud model není platný, naplní objekt ViewBag s **GenreId** a **ArtistId**, pak vrátí zobrazení s přijatého objektu Album uživatelům provádět všechny požadované aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="ec7f5-317">Úloha 7 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="ec7f5-318">V této úloze budete testovat, **StoreManager úpravy** stránka zobrazení aktualizovaných dat alb ve skutečnosti uloží v databázi.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="ec7f5-319">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-320">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-320">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-321">Změňte adresu URL na **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="ec7f5-322">Změna názvu alba k **zatížení** a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="ec7f5-323">Ověřte, že název alba je ve skutečnosti změnila v seznamu alb.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="ec7f5-324">![Aktualizace album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizace album")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="ec7f5-325">*Aktualizace Album*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="ec7f5-326">Cvičení 4: Přidání vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="ec7f5-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="ec7f5-327">Teď, když **StoreManagerController** podporuje **upravit** možnost, v tomto cvičení se dozvíte, jak přidat šablonu vytvořit zobrazení umožníte ukládání správci přidat nové alb k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="ec7f5-328">Stejně jako s funkcemi upravit implementujete scénář vytvořit pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="ec7f5-329">Jedna metoda akce se zobrazí formuláře prázdný, když správci úložiště nejprve navštíví **/StoreManager/vytvořit** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="ec7f5-330">Druhé metody akce zpracuje scénář, kde správce obchodu klikne **Uložit** tlačítko ve formuláři a odešle hodnoty zpět do **/StoreManager/vytvořit** adresu URL jako HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="ec7f5-331">Úloha 1 – implementace metody GET protokolu HTTP vytvořit akce</span><span class="sxs-lookup"><span data-stu-id="ec7f5-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="ec7f5-332">V této úloze budete implementovat HTTP GET verzi vytvořit metody akce k načtení seznamu všech žánry a umělci, tato data zabalit do **StoreManagerViewModel** objekt, který bude předána do zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="ec7f5-333">Otevřete **začít** řešení nacházející se v **zdroj/Ex4-AddingACreateView/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="ec7f5-334">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ec7f5-335">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-336">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-337">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-338">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-339">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-340">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-341">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-342">Otevřete **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="ec7f5-343">Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="ec7f5-344">Nahraďte **vytvořit** kódu metoda akce následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="ec7f5-345">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování - Ex4 StoreManagerController HTTP GET vytvořit akce*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="ec7f5-346">Úloha 2 – Přidání zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="ec7f5-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="ec7f5-347">V této úloze přidá šablony vytvořit zobrazení, která se zobrazí nový formulář Album (prázdný).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="ec7f5-348">Klepněte pravým tlačítkem myši **vytvořit** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="ec7f5-349">Tím se otevře dialogové okno Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="ec7f5-350">V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="ec7f5-351">Vyberte **vytvořit zobrazení silného typu** možnost a vyberte **Album (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu a **vytvořit** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="ec7f5-352">V ostatních polích nechte výchozí hodnoty a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="ec7f5-353">![Přidání zobrazení vytvořit](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "přidání-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="ec7f5-354">*Přidání zobrazení pro vytváření*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="ec7f5-355">Aktualizace **GenreId** a **ArtistId** pole k použití rozevíracího seznamu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="ec7f5-356">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="ec7f5-357">V této úloze budete testovat, **StoreManager** **vytvořit** stránka zobrazení zobrazí prázdné formuláře alb.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="ec7f5-358">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-359">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-359">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-360">Změňte adresu URL na **/StoreManager/vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="ec7f5-361">Ověřte, zda formuláře prázdný zobrazí pro naplnění nové vlastnosti alb.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="ec7f5-362">![Vytvoření zobrazení s formuláře prázdný](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "vytvořit zobrazení s prázdnou formuláře")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="ec7f5-363">*Vytvoření zobrazení pomocí prázdného formuláře*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="ec7f5-364">Úloha 4 – implementace HTTP POST vytvořit metody akce</span><span class="sxs-lookup"><span data-stu-id="ec7f5-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="ec7f5-365">V této úloze budete implementovat HTTP POST verzi vytvořit metody akce, která bude volána, když uživatel klikne **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="ec7f5-366">Metoda měli uložit nové album v databázi.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="ec7f5-367">Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="ec7f5-368">Otevřete **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="ec7f5-369">Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="ec7f5-370">Nahraďte **HTTP POST vytvořit** kódu metoda akce následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="ec7f5-371">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – Ex4 StoreManagerController HTTP POST vytvořením akce*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-372">Akce vytvoření je poměrně podobná předchozích úprav metody akce, ale místo objekt nastavení, jako jsou změny, se přidá do kontextu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="ec7f5-373">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="ec7f5-374">V této úloze budete testovat, **StoreManager vytvořit** stránka zobrazení umožňuje vytvářet nové Album a pak přesměruje na zobrazení StoreManager Index.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="ec7f5-375">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-376">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-376">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-377">Změňte adresu URL na **/StoreManager/vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="ec7f5-378">Pro nové Album, jako na následujícím obrázku vyplníte všechna pole formuláře s daty:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="ec7f5-379">![Vytváření alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "vytváření alba")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="ec7f5-380">*Vytváření alba*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-380">*Creating an Album*</span></span>
3. <span data-ttu-id="ec7f5-381">Ověřte, že budete přesměrováni na StoreManager Index zobrazení, které obsahuje nové Album právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="ec7f5-382">![Vytvořit nové Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nové Album vytvořen")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="ec7f5-383">*Vytvořit nové Album*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="ec7f5-384">Cvičení 5: Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="ec7f5-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="ec7f5-385">Umožňuje odstranit alb není dosud implementována.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="ec7f5-386">Je to, co toto cvičení bude o.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-386">This is what this exercise will be about.</span></span> <span data-ttu-id="ec7f5-387">Jako před, budete implementovat scénáře odstranit pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="ec7f5-388">Jedna metoda akce se zobrazí potvrzení formuláře</span><span class="sxs-lookup"><span data-stu-id="ec7f5-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="ec7f5-389">Druhé metody akce, bude zpracovávat odeslání formuláře</span><span class="sxs-lookup"><span data-stu-id="ec7f5-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="ec7f5-390">Úloha 1 – implementace metody akce Delete HTTP GET</span><span class="sxs-lookup"><span data-stu-id="ec7f5-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="ec7f5-391">V této úloze budete implementovat verze HTTP GET metody akce odstranění, načtení informací o alba.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="ec7f5-392">Otevřete **začít** řešení nacházející se v **zdroj/Ex5-HandlingDeletion/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="ec7f5-393">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ec7f5-394">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-395">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-396">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-397">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-398">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-399">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-400">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-401">Otevřete **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="ec7f5-402">Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="ec7f5-403">Akce kontroleru odstranění je přesně stejný jako předchozí akce kontroleru podrobnosti úložiště: odešle dotaz **album** objekt z databáze pomocí **id** součástí adresy URL a vrátí odpovídající **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="ec7f5-404">Chcete-li to provést, nahraďte HTTP-GET **odstranit** kódu metoda akce s následující:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="ec7f5-405">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování - Ex5 zpracování odstranění – odstranit HTTP GET – akce*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="ec7f5-406">Klepněte pravým tlačítkem myši **odstranit** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="ec7f5-407">Tím se otevře dialogové okno Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="ec7f5-408">V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="ec7f5-409">Vyberte **vytvořit zobrazení silného typu** možnost a vyberte **Album (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="ec7f5-410">Vyberte **odstranit** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="ec7f5-411">V ostatních polích nechte výchozí hodnoty a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="ec7f5-412">![Přidání zobrazení odstranění](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "přidání odstranit zobrazení")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="ec7f5-413">*Přidání zobrazení odstranění*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="ec7f5-414">Odstranit šablonu zobrazí všechna pole z modelu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="ec7f5-415">Se zobrazí pouze alba název.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-415">You will show only the album's title.</span></span> <span data-ttu-id="ec7f5-416">Chcete-li to provést, nahradí obsah zobrazení s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="ec7f5-417">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="ec7f5-418">V této úloze budete testovat, **StoreManager** **odstranit** stránka zobrazení zobrazí potvrzení odstranění formuláře.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="ec7f5-419">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-420">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-420">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-421">Změňte adresu URL na **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="ec7f5-422">Vyberte jeden album odstranit kliknutím **odstranit** a ověřte, zda je nahrán nové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="ec7f5-423">![Odstraňování Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "odstraňování Album")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="ec7f5-424">*Odstraňování Album*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="ec7f5-425">Úloha 3 – implementace metody akce Delete HTTP POST</span><span class="sxs-lookup"><span data-stu-id="ec7f5-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="ec7f5-426">V této úloze budete implementovat verze HTTP POST metodě akce odstranění, která bude volána, když uživatel klikne **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="ec7f5-427">Metoda, měli byste odstranit album v databázi.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="ec7f5-428">Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="ec7f5-429">Otevřete **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="ec7f5-430">Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="ec7f5-431">Nahraďte **HTTP POST odstranit** kódu metoda akce následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="ec7f5-432">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – Ex5 zpracování odstranění – odstranit HTTP POST – akce*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="ec7f5-433">Úloha 4 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="ec7f5-434">V této úloze budete testovat, **StoreManager odstranění** stránka zobrazení umožňuje odstranění alba a pak přesměruje na zobrazení StoreManager Index.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="ec7f5-435">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-436">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-436">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-437">Změňte adresu URL na **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="ec7f5-438">Vyberte jeden album odstranit kliknutím **odstranit.**</span><span class="sxs-lookup"><span data-stu-id="ec7f5-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="ec7f5-439">Potvrďte odstranění kliknutím **odstranit** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="ec7f5-440">![Odstraňování Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "odstraňování Album")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="ec7f5-441">*Odstraňování Album*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="ec7f5-442">Ověřte, že alba byl odstranit, protože v nezobrazí **Index** stránky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="ec7f5-443">Cvičení 6: Přidání ověření</span><span class="sxs-lookup"><span data-stu-id="ec7f5-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="ec7f5-444">Vytvořit a upravit formuláře, které mají zavedené v současné době neprovádět jakýkoli druh ověřování.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="ec7f5-445">Pokud uživatel odejde povinné pole, které jsou prázdné nebo písmena typ pole cena, první chyba, kterou budete mít bude z databáze.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="ec7f5-446">Přidáním datových poznámek na třídu modelu, můžete přidat ověřování do aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="ec7f5-447">Povolit datových poznámek popisující pravidla, která chcete použít pro vaše vlastnosti modelu a ASP.NET MVC se postará o vynucení a zobrazení odpovídající zprávu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="ec7f5-448">Úloha 1 – Přidání datových poznámek</span><span class="sxs-lookup"><span data-stu-id="ec7f5-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="ec7f5-449">V této úloze přidáte, že datových poznámek Album modelu, který bude stránka vytvořit a upravit zobrazení ověřovacích zpráv v případě nutnosti.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="ec7f5-450">Pro třídu modelu jednoduché přidávání datové poznámky se právě zpracovává přidáním **pomocí** příkaz pro **System.ComponentModel.DataAnnotation**, pak umístění **[požadované]** atribut u příslušných vlastností.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="ec7f5-451">V následujícím příkladu by zkontrolujte **název** vlastnost povinné pole v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="ec7f5-452">Toto je trochu složitější v případech, jako je tato aplikace, kde se vygeneruje datového modelu Entity.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="ec7f5-453">Pokud jste přidali datových poznámek přímo do třídy modelu, pokud aktualizovat model z databáze se budou přepsána.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="ec7f5-454">Místo toho můžete provést pomocí metadat částečné třídy, které budou pro uložení poznámky existují a jsou přidruženy k modelu třídy pomocí **[MetadataType]** atribut.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="ec7f5-455">Otevřete **začít** řešení nacházející se v **zdroj/Ex6-AddingValidation/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="ec7f5-456">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ec7f5-457">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-458">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-459">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-460">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-461">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-462">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-463">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-464">Otevřete **Album.cs** z **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="ec7f5-465">Nahraďte **Album.cs** obsahu se zvýrazněný kód tak, aby vypadal jako následující:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec7f5-466">Na řádku **[DisplayFormat(ConvertEmptyStringToNull=false)]** označuje, že prázdné řetězce z modelu nebudou převedeny na hodnotu null při aktualizaci datové pole v datovém zdroji.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="ec7f5-467">Toto nastavení zabrání výjimku, pokud rozhraní Entity Framework před datové poznámky ověří pole přiřadí hodnoty null do modelu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="ec7f5-468">(Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování - třídu metadat Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-469">To **Album** má třídu **MetadataType** atribut, který odkazuje na **AlbumMetaData** třídu pro datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="ec7f5-470">Toto jsou některé z datové poznámky atributy, které použijete k přidání poznámek ke Album modelu:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="ec7f5-471">Požadováno – označuje, že vlastnost je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="ec7f5-472">DisplayName – definuje text, který se použije na pole formuláře a ověřovacích zpráv</span><span class="sxs-lookup"><span data-stu-id="ec7f5-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="ec7f5-473">DisplayFormat - Určuje, jak zobrazit a formátovány datová pole.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="ec7f5-474">StringLength - definuje maximální délku pole řetězce</span><span class="sxs-lookup"><span data-stu-id="ec7f5-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="ec7f5-475">V rozsahu – poskytuje maximální a minimální hodnotu pro číselné pole</span><span class="sxs-lookup"><span data-stu-id="ec7f5-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="ec7f5-476">ScaffoldColumn – umožňuje skrytí pole z editoru formulářů</span><span class="sxs-lookup"><span data-stu-id="ec7f5-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="ec7f5-477">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="ec7f5-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="ec7f5-478">V této úloze budete testovat, se stránky vytvořit a upravit ověření pole, použijte názvy zobrazení vybrali v posledním úkolem.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="ec7f5-479">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ec7f5-480">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-480">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-481">Změňte adresu URL na **/StoreManager/vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="ec7f5-482">Ověřte, že zobrazované názvy odpovídají těm, které jsou v třídu (jako je **alb obrázky URL** místo **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="ec7f5-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="ec7f5-483">Klikněte na tlačítko **vytvořit**, bez vyplnění formuláře.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="ec7f5-484">Ověřte, že dostanete odpovídající ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="ec7f5-485">![Ověření pole na stránce vytvořit](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "ověření pole na stránce vytvořit")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="ec7f5-486">*Ověřené polí na stránce vytvořit*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="ec7f5-487">Můžete ověřit, že stejné vyskytuje v **upravit** stránky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="ec7f5-488">Změňte adresu URL na **/StoreManager/Edit/1** a ověřte, že zobrazované názvy odpovídají těm, které jsou v třídu (jako je **alb obrázky URL** místo **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="ec7f5-489">Prázdný **název** a **cena** pole a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="ec7f5-490">Ověřte, že dostanete odpovídající ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-490">Verify that you get the corresponding validation messages.</span></span>

    ![Ověřené polí na stránce Upravit](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="ec7f5-492">*Ověřené polí na stránce Upravit*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="ec7f5-493">Cvičení 7: Pomocí Nerušivý jQuery na straně klienta</span><span class="sxs-lookup"><span data-stu-id="ec7f5-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="ec7f5-494">V tomto cvičení se dozvíte, jak povolit MVC 4 Nerušivý jQuery ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="ec7f5-495">Nerušivý jQuery používá data-ajax předponu JavaScript volat metody akce na serveru místo intrusively generování vložené skripty klienta.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="ec7f5-496">Úloha 1 – spuštění aplikace před povolením Nerušivý jQuery</span><span class="sxs-lookup"><span data-stu-id="ec7f5-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="ec7f5-497">V této úloze budete spouštět aplikaci před včetně jQuery k porovnání obou modelů ověření.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="ec7f5-498">Otevřete **začít** řešení nacházející se v **zdroj/Ex7-UnobtrusivejQueryValidation/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="ec7f5-499">Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ec7f5-500">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ec7f5-501">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ec7f5-502">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ec7f5-503">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ec7f5-504">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ec7f5-505">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ec7f5-506">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ec7f5-507">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="ec7f5-508">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-508">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-509">Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplnění formuláře a přesvědčte se, abyste měli ověřovacích zpráv:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="ec7f5-510">![Ověření klienta zakázaná](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "zakázáno ověření klienta")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="ec7f5-511">*Ověření klienta zakázána*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="ec7f5-512">V prohlížeči otevřete zdrojový kód HTML:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="ec7f5-513">Úloha 2 – povolení ověření Nerušivého klienta</span><span class="sxs-lookup"><span data-stu-id="ec7f5-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="ec7f5-514">V této úloze povolíte jQuery **ověření nerušivého klienta** z **Web.config** souboru, který je ve výchozím nastavení nastavena na hodnotu false na všechny nové projekty ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="ec7f5-515">Kromě toho budete přidávat, že nezbytné skriptů odkazy aby jQuery pracovní Nerušivý ověření klienta.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="ec7f5-516">Otevřete **Web.Config** souborů v kořenu projektu a ujistěte se, že **ClientValidationEnabled** a **UnobtrusiveJavaScriptEnabled** klíče hodnoty jsou nastaveny na **true**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-517">Kód v Global.asax.cs na stejné výsledky můžete také povolit ověření klienta:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="ec7f5-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="ec7f5-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="ec7f5-519">Kromě toho můžete přiřadit atribut ClientValidationEnabled do libovolného řadiče do mají vlastní chování.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="ec7f5-520">Otevřete **Create.cshtml** v **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="ec7f5-521">Zajistěte, aby následující soubory skriptu, **jquery.validate** a **jquery.validate.unobtrusive**, se odkazuje v zobrazení přes &quot; **~/bundles/jqueryval** &quot; sady.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="ec7f5-522">Tyto knihovny jQuery jsou součástí nové projekty MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="ec7f5-523">Můžete najít další knihovny v **/skriptů** složky můžete projektu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="ec7f5-524">Aby bylo možné toto ověření knihovny práci, budete muset přidat odkaz na knihovnu jQuery framework.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="ec7f5-525">Vzhledem k tomu, že tento odkaz je již přidán do  **\_Layout.cshtml** souboru, není nutné přidat do tohoto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="ec7f5-526">Úloha 3 – spuštění aplikace pomocí Nerušivý jQuery ověření</span><span class="sxs-lookup"><span data-stu-id="ec7f5-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="ec7f5-527">V této úloze budete testovat, **StoreManager** vytvořit zobrazení šablony provádí ověřování na straně klienta pomocí knihovny jQuery, když uživatel vytváří nové album.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="ec7f5-528">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="ec7f5-529">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-529">The project starts in the Home page.</span></span> <span data-ttu-id="ec7f5-530">Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplnění formuláře a přesvědčte se, abyste měli ověřovacích zpráv:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="ec7f5-531">![Ověření klienta s jQuery povoleno](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "ověření klienta s jQuery povoleno")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="ec7f5-532">*Ověření klienta s jQuery povoleno*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="ec7f5-533">V prohlížeči otevřete zdrojový kód pro vytvoření zobrazení:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="ec7f5-534">Pro každý ověřovací pravidlo klienta Nerušivý jQuery přidá atribut data-val -*rulename*=&quot;*zpráva*&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="ec7f5-535">Níže je seznam značek této Unobtrusive jQuery vloží do vstupní pole html k provedení ověření klienta:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="ec7f5-536">Val dat</span><span class="sxs-lookup"><span data-stu-id="ec7f5-536">Data-val</span></span>
   > - <span data-ttu-id="ec7f5-537">Číslo datového val</span><span class="sxs-lookup"><span data-stu-id="ec7f5-537">Data-val-number</span></span>
   > - <span data-ttu-id="ec7f5-538">Oblast dat val</span><span class="sxs-lookup"><span data-stu-id="ec7f5-538">Data-val-range</span></span>
   > - <span data-ttu-id="ec7f5-539">Data-val rozsah min / Data-val rozsah max</span><span class="sxs-lookup"><span data-stu-id="ec7f5-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="ec7f5-540">Potřeba val dat</span><span class="sxs-lookup"><span data-stu-id="ec7f5-540">Data-val-required</span></span>
   > - <span data-ttu-id="ec7f5-541">Délka dat val</span><span class="sxs-lookup"><span data-stu-id="ec7f5-541">Data-val-length</span></span>
   > - <span data-ttu-id="ec7f5-542">Data-val délka max / Data-val délka min</span><span class="sxs-lookup"><span data-stu-id="ec7f5-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="ec7f5-543">Všechny hodnoty data jsou vyplněny modelu **datové poznámky**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="ec7f5-544">Potom všechny logiky, která pracuje na straně serveru může být spuštěna na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="ec7f5-545">Například cena atribut má následující datové poznámky v modelu:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="ec7f5-546">Po použití Nerušivý jQuery, je generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ec7f5-547">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ec7f5-547">Summary</span></span>

<span data-ttu-id="ec7f5-548">Provedením tohoto testovacího prostředí Hands-On jste se naučili, jak povolit uživatelům změnit data uložená v databázi s použitím následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="ec7f5-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="ec7f5-549">Akce kontroleru jako Index, vytvořit, upravit, odstranit</span><span class="sxs-lookup"><span data-stu-id="ec7f5-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="ec7f5-550">Funkce generování uživatelského rozhraní ASP.NET MVC pro zobrazení vlastností do tabulky HTML</span><span class="sxs-lookup"><span data-stu-id="ec7f5-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="ec7f5-551">Vlastní pomocné rutiny HTML ke zlepšení uživatelského prostředí</span><span class="sxs-lookup"><span data-stu-id="ec7f5-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="ec7f5-552">Metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání</span><span class="sxs-lookup"><span data-stu-id="ec7f5-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="ec7f5-553">Šablonu sdílené editor pro podobné zobrazení šablon, například vytvoření a úpravy</span><span class="sxs-lookup"><span data-stu-id="ec7f5-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="ec7f5-554">Elementy Form jako rozevírací seznamy</span><span class="sxs-lookup"><span data-stu-id="ec7f5-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="ec7f5-555">Anotací dat při ověřování modelu</span><span class="sxs-lookup"><span data-stu-id="ec7f5-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="ec7f5-556">Ověřování na straně klienta pomocí Nerušivý knihovny jQuery</span><span class="sxs-lookup"><span data-stu-id="ec7f5-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="ec7f5-557">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="ec7f5-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="ec7f5-558">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="ec7f5-559">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="ec7f5-560">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="ec7f5-561">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="ec7f5-562">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-562">Click on **Install Now**.</span></span> <span data-ttu-id="ec7f5-563">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="ec7f5-564">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="ec7f5-565">![Nainstalovat Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="ec7f5-566">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="ec7f5-567">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="ec7f5-569">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="ec7f5-570">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-570">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="ec7f5-572">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-572">*Installation progress*</span></span>
6. <span data-ttu-id="ec7f5-573">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-573">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="ec7f5-575">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-575">*Installation completed*</span></span>
7. <span data-ttu-id="ec7f5-576">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="ec7f5-577">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="ec7f5-579">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="ec7f5-580">Příloha B: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="ec7f5-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="ec7f5-581">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="ec7f5-582">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="ec7f5-583">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="ec7f5-584">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="ec7f5-585">***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***</span><span class="sxs-lookup"><span data-stu-id="ec7f5-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="ec7f5-586">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="ec7f5-587">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="ec7f5-588">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="ec7f5-589">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="ec7f5-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="ec7f5-590">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="ec7f5-591">![Začněte psát název fragmentu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="ec7f5-592">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="ec7f5-593">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="ec7f5-594">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="ec7f5-595">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="ec7f5-596">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="ec7f5-597">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="ec7f5-598">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="ec7f5-599">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="ec7f5-600">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="ec7f5-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="ec7f5-601">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="ec7f5-602">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="ec7f5-603">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="ec7f5-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="ec7f5-604">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="ec7f5-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
