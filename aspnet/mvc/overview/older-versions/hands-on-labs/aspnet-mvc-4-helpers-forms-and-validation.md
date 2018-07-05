---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 – Pomocníci, formuláře a ověřování | Dokumentace Microsoftu
author: rick-anderson
description: V ASP.NET MVC 4 modely a Data Access praktického testovacího prostředí které byly načítání a zobrazení dat z databáze. Ve tohoto praktického testovacího prostředí, které přidáte...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376734"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="b9bbb-104">ASP.NET MVC 4 – Pomocníci, formuláře a ověřování</span><span class="sxs-lookup"><span data-stu-id="b9bbb-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="b9bbb-105">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b9bbb-106">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="b9bbb-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b9bbb-107">V **ASP.NET MVC 4 modely a přístup k datům** praktického testovacího prostředí, aby byla načtení a zobrazení dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="b9bbb-108">Ve tohoto praktického testovacího prostředí, které přidáte **Music Store** aplikace možnost upravit tato data.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="b9bbb-109">Pomocí tohoto cíle v úvahu nejprve vytvoříte kontroler, který bude podporovat vytvoření, čtení, aktualizace a odstranění (CRUD) akce alb.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="b9bbb-110">Vygeneruje šablonu zobrazení Index s využitím ASP.NET MVC generování uživatelského rozhraní funkce k zobrazení vlastností alb v tabulku HTML.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="b9bbb-111">K vylepšení tohoto zobrazení, které přidáte vlastní pomocné rutiny HTML, který se zkrátí dlouhé popisy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="b9bbb-112">Později přidáte, upravit a vytvořit zobrazení, které vám umožní změnit alb v databázi, pomocí elementů formuláře jako rozevírací seznamy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="b9bbb-113">A konečně vám umožní uživatelům odstranit alba a také vám zabrání jejich zadávání chybná data ověřením svůj vstup.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="b9bbb-114">Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="b9bbb-115">Pokud jste ještě nepoužívali **ASP.NET MVC** před, doporučujeme si projít **základy ASP.NET MVC** praktického testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="b9bbb-116">Tato laboratoř vás provede vylepšení a nových funkcí popsaných dříve použitím menší změny na ukázkovou webovou aplikaci ve zdrojové složce k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="b9bbb-117">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b9bbb-118">Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Pomocníci, formuláře a ověřování](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b9bbb-119">Cíle</span><span class="sxs-lookup"><span data-stu-id="b9bbb-119">Objectives</span></span>

<span data-ttu-id="b9bbb-120">V tomto praktického testovacího prostředí se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b9bbb-121">Vytvoření kontroleru podporovat operace CRUD</span><span class="sxs-lookup"><span data-stu-id="b9bbb-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="b9bbb-122">Generovat zobrazení o Index pro zobrazení vlastností entity do tabulky HTML</span><span class="sxs-lookup"><span data-stu-id="b9bbb-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="b9bbb-123">Přidat vlastní pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="b9bbb-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="b9bbb-124">Vytvoření a přizpůsobení zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="b9bbb-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="b9bbb-125">Rozlišovat mezi metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání</span><span class="sxs-lookup"><span data-stu-id="b9bbb-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="b9bbb-126">Přidání a přizpůsobení vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="b9bbb-126">Add and customize a Create View</span></span>
- <span data-ttu-id="b9bbb-127">Zpracování odstranění entity</span><span class="sxs-lookup"><span data-stu-id="b9bbb-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="b9bbb-128">Ověření vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="b9bbb-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b9bbb-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b9bbb-129">Prerequisites</span></span>

<span data-ttu-id="b9bbb-130">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b9bbb-131">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b9bbb-132">Instalace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-132">Setup</span></span>

<span data-ttu-id="b9bbb-133">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="b9bbb-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="b9bbb-134">Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b9bbb-135">K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b9bbb-136">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b9bbb-137">Cvičení</span><span class="sxs-lookup"><span data-stu-id="b9bbb-137">Exercises</span></span>

<span data-ttu-id="b9bbb-138">Následující praktická cvičení tvoří tohoto praktického testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="b9bbb-139">Vytvoření kontroleru Store Manager a jeho zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="b9bbb-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="b9bbb-140">Přidání pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="b9bbb-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="b9bbb-141">Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="b9bbb-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="b9bbb-142">Přidání zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="b9bbb-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="b9bbb-143">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="b9bbb-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="b9bbb-144">Přidání ověření</span><span class="sxs-lookup"><span data-stu-id="b9bbb-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="b9bbb-145">Pomocí Nerušivého jQuery na straně klienta</span><span class="sxs-lookup"><span data-stu-id="b9bbb-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="b9bbb-146">Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b9bbb-147">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b9bbb-148">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="b9bbb-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="b9bbb-149">Cvičení 1: Vytvoření kontroleru Store Manager a jeho zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="b9bbb-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="b9bbb-150">V tomto cvičení se dozvíte, jak vytvořit nový kontroler podporuje operace CRUD, přizpůsobte její metody akce indexu se seznam alb z databáze a nakonec generování šablony služby Index zobrazení využití výhod generování uživatelského rozhraní ASP.NET MVC funkce k zobrazení vlastností alb v tabulku HTML.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="b9bbb-151">Úloha 1 – vytváření StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="b9bbb-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="b9bbb-152">V této úloze vytvoříte nový kontroler volá **StoreManagerController** pro podporu operací CRUD.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="b9bbb-153">Otevřít **začít** řešení nachází v **zdroj/Ex1-CreatingTheStoreManagerController/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="b9bbb-154">Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-155">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-156">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-157">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-158">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-159">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-160">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-161">Přidáte nový kontroler.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-161">Add a new controller.</span></span> <span data-ttu-id="b9bbb-162">Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složku v Průzkumníku řešení vyberte **přidat** a pak **řadič** příkazu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="b9bbb-163">Změnit **řadič** **název** k **StoreManagerController** a ujistěte se, že možnost **kontroler MVC s akcemi čtení/zápisu-prázdný**zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="b9bbb-164">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-164">Click **Add**.</span></span>

    <span data-ttu-id="b9bbb-165">![Dialogové okno Přidat kontroler](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "dialogového okna Přidat kontroler")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="b9bbb-166">*Přidat Dialog Kontroleru*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="b9bbb-167">Nová třída Kontroleru je generována.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-167">A new Controller class is generated.</span></span> <span data-ttu-id="b9bbb-168">Vzhledem k tomu, který jste určili pro přidání akce pro čtení a zápis, metody zástupných procedur pro ty, běžné akce CRUD se vytvoří s komentáře TODO vyplněna, s výzvou k zahrnout konkrétní logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="b9bbb-169">Úloha 2 – přizpůsobení StoreManager indexu</span><span class="sxs-lookup"><span data-stu-id="b9bbb-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="b9bbb-170">V této úloze budete přizpůsobovat StoreManager Index metody akce k vrácení se seznamem alb zobrazení z databáze.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="b9bbb-171">Ve třídě StoreManagerController přidejte následující *pomocí* direktivy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="b9bbb-172">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex1 pomocí MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="b9bbb-173">Přidáním pole do **StoreManagerController** pro uložení instance **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="b9bbb-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="b9bbb-174">(Fragment - kódu *ASP.NET MVC 4 – Pomocníci, formuláře a ověřování – Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="b9bbb-175">Implementace StoreManagerController indexu akce, která vrátí zobrazení se seznamem alb.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="b9bbb-176">Akce logice Kontroleru bude velmi podobný indexu akce StoreController napsané dříve.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="b9bbb-177">Zprostředkovatel LINQ slouží pro načtení všech alb, včetně informací o žánr a interpret pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="b9bbb-178">(Fragment - kódu *ASP.NET MVC 4 – Pomocníci, formuláře a ověřování – Ex1 StoreManagerController Index*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="b9bbb-179">Úloha 3 – vytvoření indexu zobrazení</span><span class="sxs-lookup"><span data-stu-id="b9bbb-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="b9bbb-180">V této úloze vytvoříte šablonu zobrazení Index k zobrazení seznamu vrácených alb **StoreManager** Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="b9bbb-181">Před vytvořením nové šablony zobrazení se má sestavit projekt tak, aby **Přidat Dialog zobrazení** ví o **alba** třídu použít.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="b9bbb-182">Vyberte **sestavení | Sestavení MvcMusicStore** k sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="b9bbb-183">Klepněte pravým tlačítkem myši **Index** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="b9bbb-184">Tím se otevře **přidat zobrazení** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="b9bbb-185">![Přidání zobrazení](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "přidat zobrazení")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="b9bbb-186">*Přidání zobrazení z v rámci Index – metoda*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="b9bbb-187">V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **Index**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="b9bbb-188">Vyberte **vytvoření zobrazení se silnými typy** a vyberte možnost **alba (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="b9bbb-189">Vyberte **seznamu** z **šablona Scaffold** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b9bbb-190">Nechte **zobrazovací modul** k **Razor** a v ostatních polích pomocí jejich výchozí hodnoty a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b9bbb-191">![Přidání zobrazení index se o](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "přidáte zobrazení indexu")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="b9bbb-192">*Přidání zobrazení Index se o*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="b9bbb-193">Úloha 4 – přizpůsobení vygenerované uživatelské rozhraní zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="b9bbb-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="b9bbb-194">V tomto úkolu budou upraveny jednoduché zobrazení šablony vytvořené s funkcí generování uživatelského rozhraní ASP.NET MVC na něm zobrazit pole, která chcete.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="b9bbb-195">**Generování uživatelského rozhraní** podpory v rámci technologie ASP.NET MVC vygeneruje šablonu jednoduché zobrazení, která se zobrazí seznam všech polí v modelu alba.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="b9bbb-196">**Generování uživatelského rozhraní** poskytuje rychlý způsob, jak začít pracovat v zobrazení se silnými typy: namísto nutnosti psát zobrazit šablonu ručně, generování uživatelského rozhraní rychle generuje výchozí šablonu a pak můžete vygenerovaný kód upravit.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="b9bbb-197">Revize kódu vytvoří.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-197">Review the code created.</span></span> <span data-ttu-id="b9bbb-198">Vygenerovaný seznam polí bude součástí následující tabulky HTML, který **generování uživatelského rozhraní** používá pro zobrazení tabulková data.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="b9bbb-199">Nahradit **&lt;tabulky&gt;** kód následujícím kódem, chcete-li zobrazit pouze **žánr**, **interpreta**, **alba**, a **cena** pole.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="b9bbb-200">Tím se odstraní **AlbumId** a **alba obrázky URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="b9bbb-201">Navíc změní GenreId a ArtistId sloupce pro zobrazení jejich vlastností propojené třídy **Artist.Name** a **Genre.Name**a odebere **podrobnosti** odkaz.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="b9bbb-202">Změna v následujících popisech.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b9bbb-203">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="b9bbb-204">V této úloze budete testovat, který **StoreManager** **Index** zobrazit šablonu zobrazí seznam alb podle návrhu v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="b9bbb-205">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-206">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-206">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-207">Změňte adresu URL na **/StoreManager** k ověření, že se zobrazí seznam alb, zobrazení jejich **Title**, **interpreta** a **žánr**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="b9bbb-208">![Procházení seznamu alb](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "procházení seznamu alb")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="b9bbb-209">*Procházení seznamu alb*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="b9bbb-210">Cvičení 2: Přidání pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="b9bbb-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="b9bbb-211">StoreManager indexovou stránku má jedním potenciálním problémem: název a interpret vlastnosti mohou být dostatečně dlouhá, aby throw vypnout formátování tabulky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="b9bbb-212">V tomto cvičení se dozvíte, jak přidat vlastní pomocné rutiny HTML došlo ke zkrácení tento text.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="b9bbb-213">Na následujícím obrázku vidíte, jak formát se mění z důvodu délka textu při použití malé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="b9bbb-214">![Procházení seznamu alb s nikoli zkráceno text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "procházení seznamu alb s nikoli zkráceno text")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="b9bbb-215">*Procházení seznamu alb s nikoli zkráceno text*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="b9bbb-216">Úloha 1 – rozšíření pomocné rutiny HTML</span><span class="sxs-lookup"><span data-stu-id="b9bbb-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="b9bbb-217">V této úloze přidá novou metodu **Truncate** k **HTML** objekt zveřejní v zobrazeních ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="b9bbb-218">K tomuto účelu můžete implementovat **– metoda rozšíření** do vestavěné **System.Web.Mvc.HtmlHelper** třídy poskytované rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="b9bbb-219">Další informace o tom **rozšiřující metody**, navštivte prosím článku na webu msdn.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="b9bbb-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="b9bbb-221">Otevřít **začít** řešení nachází v **zdroj/Ex2-AddingAnHTMLHelper/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="b9bbb-222">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9bbb-223">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-224">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-225">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-226">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-227">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-228">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-229">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-230">Otevřete zobrazení indexu StoreManager společnosti.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="b9bbb-231">Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak bude **StoreManager** a otevřete **Index.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="b9bbb-232">Přidejte následující kód níže <strong>@model</strong> směrnice pro definování <strong>Truncate</strong> Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="b9bbb-233">Úloha 2 – zkracování textu na stránce</span><span class="sxs-lookup"><span data-stu-id="b9bbb-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="b9bbb-234">V této úloze budete používat **Truncate** metoda se text zkrátí podle v šabloně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="b9bbb-235">Otevřete zobrazení indexu StoreManager společnosti.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="b9bbb-236">Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak bude **StoreManager** a otevřete **Index.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="b9bbb-237">Nahraďte na čárách znázorňujících **interpret** a alba **Title**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="b9bbb-238">Provedete to nahrazením následující řádky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b9bbb-239">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="b9bbb-240">V této úloze budete testovat, který **StoreManager** **Index** zobrazit šablonu zkrátí nadpis alba a interpret.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="b9bbb-241">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-242">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-242">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-243">Změňte adresu URL na **/StoreManager** k ověření tohoto dlouho texty v **Title** a **interpreta** sloupce se zkrátí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="b9bbb-244">![Oříznutá názvy a umělci názvy](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "zkrácen názvy a umělci názvy")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="b9bbb-245">*Zkrácené názvy a názvy interpret*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="b9bbb-246">Cvičení 3: Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="b9bbb-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="b9bbb-247">V tomto cvičení se dozvíte, jak vytvořit formulář umožňuje správcům úložiště upravit alba.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="b9bbb-248">Bude procházet **/StoreManager/Edit/id** adresy URL (**id** je jedinečné id alba pro úpravy), díky čemuž volání rozhraní HTTP GET na server.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="b9bbb-249">Metoda akce Kontroleru upravit bude načítat příslušné alba z databáze, vytvořit **StoreManagerViewModel** objektu k zapouzdření (spolu s seznam vašim Animátorům a žánry) a pak ji předal zobrazit šablonu pro vykreslení stránky HTML zpět uživateli.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="b9bbb-250">Na této stránce **&lt;formuláře&gt;** element s textových polí a rozevírací seznamy pro úpravy vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="b9bbb-251">Jakmile uživatel aktualizuje hodnoty formuláře alba a klikne na tlačítko **Uložit** tlačítko, se změny odesílají prostřednictvím HTTP POST zpětné volání **/StoreManager/Edit/id**. I když adresa URL zůstane stejná jako v posledním volání, ASP.NET MVC identifikuje, který této doby je POST protokolu HTTP a proto spustí jinou metodu akce úpravy (jeden dekorován **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="b9bbb-252">Úloha 1 – implementace metody GET protokolu HTTP úpravy akce</span><span class="sxs-lookup"><span data-stu-id="b9bbb-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="b9bbb-253">V této úloze budete implementovat verze HTTP GET metody akce úpravy k načtení příslušné alba z databáze, a také seznam všech žánry a umělci.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="b9bbb-254">To se zabalit tato data do **StoreManagerViewModel** definované v posledním kroku, který se potom předá do šablony zobrazení k vykreslení odpovědi s objekty.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="b9bbb-255">Otevřít **začít** řešení nachází v **zdroj/EX3.-CreatingTheEditView/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="b9bbb-256">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9bbb-257">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-258">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-259">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-260">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-261">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-262">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-263">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-264">Otevřít **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="b9bbb-265">Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="b9bbb-266">Nahraďte **upravit HTTP GET** metodu akce pomocí následující kód k načtení příslušné **alba** také **žánry** a **umělci**seznamy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="b9bbb-267">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - EX3. StoreManagerController HTTP GET upravit akci*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-268">Používáte **System.Web.Mvc** **SelectList** vašim Animátorům a žánry místo **System.Collections.Generic** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="b9bbb-269">**SelectList** je čistší způsob naplnění HTML a spravovat věci, jako je aktuální výběr.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="b9bbb-270">Vytváření instancí a novější k nastavení těchto objektů ViewModel v akce kontroleru způsobí, že scénář formuláře úprav čisticího modulu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="b9bbb-271">Úloha 2 – Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="b9bbb-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="b9bbb-272">V této úloze vytvoříte šablonu zobrazení pro úpravy, které se později zobrazí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="b9bbb-273">Vytvoření zobrazení pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-273">Create the Edit View.</span></span> <span data-ttu-id="b9bbb-274">Chcete-li to provést, klepněte pravým tlačítkem myši **upravit** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="b9bbb-275">V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **upravit**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="b9bbb-276">Zkontrolujte **vytvoření zobrazení se silnými typy** zaškrtávací políčko a vyberte **alba (MvcMusicStore.Models)** z **zobrazení dat třídy** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="b9bbb-277">Vyberte **upravit** z **šablona Scaffold** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b9bbb-278">Ostatní pole nechte výchozí hodnoty a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b9bbb-279">![Přidání zobrazení pro úpravy](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "přidání zobrazení pro úpravy")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="b9bbb-280">*Přidání zobrazení pro úpravy*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b9bbb-281">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="b9bbb-282">V této úloze budete testovat, který **StoreManager** **upravit** zobrazení stránky zobrazí hodnoty vlastností pro album předán jako parametr.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="b9bbb-283">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-284">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-284">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-285">Změňte adresu URL na **/StoreManager/Edit/1** k ověření, že se zobrazují hodnoty vlastností pro alba předán.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="b9bbb-286">![Procházení zobrazení pro úpravy alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "procházení alba zobrazení pro úpravy")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="b9bbb-287">*Procházení alba zobrazení pro úpravy*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="b9bbb-288">Úloha 4 – implementace rozevírací seznamy pro šablony alba editoru</span><span class="sxs-lookup"><span data-stu-id="b9bbb-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="b9bbb-289">V této úloze přidáte rozevírací seznamy k šabloně zobrazení vytvořili v minulé úloze tak, aby si uživatel může vybrat ze seznamu umělců nebo žánrů.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="b9bbb-290">Nahradit vše **alba** fieldset kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-291">**Html.DropDownList** pomocné rutiny byla přidána k vykreslení rozevírací seznamy pro výběr umělců nebo žánrů.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="b9bbb-292">Parametry předané do **Html.DropDownList** jsou:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="b9bbb-293">Název pole formuláře (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="b9bbb-294">**SelectList** hodnot pro rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b9bbb-295">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="b9bbb-296">V této úloze budete testovat, který **StoreManager** **upravit** zobrazení stránky zobrazí rozevírací seznamy místo interpreta a rozšířením podle tematických ID textová pole.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="b9bbb-297">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-298">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-298">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-299">Změňte adresu URL na **/StoreManager/Edit/1** k ověření, zobrazuje rozevírací seznamy místo interpreta a rozšířením podle tematických ID textová pole.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="b9bbb-300">![Procházení alba zobrazení pro úpravy s rozevírací nabídky](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "procházení alba zobrazení pro úpravy s rozevírací nabídky")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="b9bbb-301">*Zobrazení pro úpravy procházení alb, tentokrát s rozevírací seznamy*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="b9bbb-302">Krok 6 – implementace upravit HTTP POST metody akce</span><span class="sxs-lookup"><span data-stu-id="b9bbb-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="b9bbb-303">Teď, když zobrazení pro úpravy pro zobrazí podle očekávání, budete muset implementovat metodu HTTP POST upravit akci Uložit změny provedené v alba.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="b9bbb-304">Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b9bbb-305">Otevřít **StoreManagerController** z **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="b9bbb-306">Nahraďte **upravit HTTP POST** kódu metody akce s tímto (Všimněte si, že je metoda, která se musí nahradit přetížené verze, která přijímá dva parametry):</span><span class="sxs-lookup"><span data-stu-id="b9bbb-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="b9bbb-307">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - EX3. StoreManagerController HTTP POST upravit akci*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-308">Tato metoda se provede, když uživatel klikne **Uložit** tlačítko zobrazení a provádí HTTP-POST hodnot formulář zpět na server pro uchování v databázi.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="b9bbb-309">Dekoratér **[HttpPost]** označuje, že metoda by měla sloužit pro tyto scénáře HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="b9bbb-310">Tato metoda přebírá **alba** objektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-310">The method takes an **Album** object.</span></span> <span data-ttu-id="b9bbb-311">ASP.NET MVC automaticky vytvoří objekt alba z zaúčtované &lt;formuláře&gt; hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="b9bbb-312">Metoda provede tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="b9bbb-313">Pokud je model platný:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="b9bbb-314">Aktualizujte položku alba v kontextu označte ji jako změněný objekt.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="b9bbb-315">Uložte změny a přesměrování na zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="b9bbb-316">Pokud model není platný, naplní objekt ViewBag s **GenreId** a **ArtistId**, vrátí zobrazení s přijatého objektu alba umožňující uživateli provést všechny požadované aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="b9bbb-317">Úloha 7: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="b9bbb-318">V této úloze budete testovat, který **StoreManager upravit** aktualizovaná data alba zobrazit stránku ve skutečnosti uloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="b9bbb-319">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-320">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-320">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-321">Změňte adresu URL na **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="b9bbb-322">Změnit název alba **zatížení** a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="b9bbb-323">Ověřte, že ve skutečnosti změnit název společnosti v seznamu alb.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="b9bbb-324">![Aktualizuje se alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizace alba")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="b9bbb-325">*Aktualizuje se alba*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="b9bbb-326">Cvičení 4: Přidání vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="b9bbb-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="b9bbb-327">Teď, když **StoreManagerController** podporuje **upravit** možnost, v tomto cvičení se dozvíte, jak přidat příkaz Create View šablonu, která umožní ukládat správci přidat nové alb do aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="b9bbb-328">Stejně jako s funkcí upravit implementujete scénář vytvořit pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="b9bbb-329">Jedna metoda akce se zobrazí prázdný formulář po Správci úložiště nejprve přejděte **/StoreManager/vytvořit** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="b9bbb-330">Druhá metoda akce zpracuje scénář, kde klikne na tlačítko Správce úložiště **Uložit** tlačítko ve formuláři a odešle hodnoty zpět **/StoreManager/vytvořit** adresy URL jako POST protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="b9bbb-331">Úloha 1 – implementace metody GET protokolu HTTP vytvořit akci</span><span class="sxs-lookup"><span data-stu-id="b9bbb-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="b9bbb-332">V této úloze budete implementovat verze HTTP GET metody akce vytvořit seznam všech žánry a umělci získat, zabalit tato data do **StoreManagerViewModel** objekt, který se potom předá do zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="b9bbb-333">Otevřít **začít** řešení nachází v **zdroj/Ex4-AddingACreateView/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="b9bbb-334">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9bbb-335">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-336">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-337">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-338">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-339">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-340">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-341">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-342">Otevřít **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b9bbb-343">Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="b9bbb-344">Nahradit **vytvořit** akce metoda kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="b9bbb-345">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování – akce vytvoření Ex4 StoreManagerController HTTP GET*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="b9bbb-346">Úloha 2 – Přidání zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="b9bbb-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="b9bbb-347">V této úloze přidá šablony vytvořit zobrazení, které se zobrazí nový formulář alba (prázdné).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="b9bbb-348">Klepněte pravým tlačítkem myši **vytvořit** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="b9bbb-349">Tím se otevře dialogové okno Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="b9bbb-350">V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="b9bbb-351">Vyberte **vytvoření zobrazení se silnými typy** možnost a vyberte **alba (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu a **vytvořit** z **šablona Scaffold** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b9bbb-352">Ostatní pole nechte výchozí hodnoty a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b9bbb-353">![Přidání zobrazení pro vytváření](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "přidání-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="b9bbb-354">*Přidání zobrazení pro vytváření*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="b9bbb-355">Aktualizace **GenreId** a **ArtistId** pole pomocí rozevíracího seznamu, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b9bbb-356">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="b9bbb-357">V této úloze budete testovat, který **StoreManager** **vytvořit** prázdný formulář alba zobrazí stránku zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="b9bbb-358">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-359">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-359">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-360">Změňte adresu URL na **/StoreManager/vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="b9bbb-361">Zkontrolujte, jestli prázdný formulář zobrazuje pro vyplnění nové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="b9bbb-362">![Vytvořit zobrazení s prázdný formulář](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "vytvořit zobrazení s prázdný formulář")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="b9bbb-363">*Vytvořit zobrazení s prázdný formulář*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="b9bbb-364">Úloha 4 – implementace HTTP POST vytvořit metody akce</span><span class="sxs-lookup"><span data-stu-id="b9bbb-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="b9bbb-365">V této úloze budete implementovat verze HTTP POST metody akce Create, která se vyvolá, když uživatel klikne **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="b9bbb-366">Metoda by měla uložit nové album v databázi.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="b9bbb-367">Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b9bbb-368">Otevřít **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b9bbb-369">Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="b9bbb-370">Nahraďte **HTTP POST vytvořit** akce metoda kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="b9bbb-371">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex4 StoreManagerController HTTP - POST akce vytvoření*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-372">Akce vytvoření je hodně podobné předchozí metoda akce upravit, ale namísto objektu nastavení, jako jsou změny, se přidá se do kontextu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b9bbb-373">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="b9bbb-374">V této úloze budete testovat, který **StoreManager vytvořit** stránka zobrazení vám umožní vytvářet nové alba a pak přesměruje na zobrazení StoreManager Index.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="b9bbb-375">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-376">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-376">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-377">Změňte adresu URL na **/StoreManager/vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="b9bbb-378">Vyplnit všechna pole formuláře s daty pro nové Album jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="b9bbb-379">![Vytváření alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "vytváření alba")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="b9bbb-380">*Vytváření alba*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-380">*Creating an Album*</span></span>
3. <span data-ttu-id="b9bbb-381">Ověřte, že získání přesměrován na StoreManager Index zobrazení, které obsahuje nové Album právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="b9bbb-382">![Vytvoří nové Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nové Album vytvořili")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="b9bbb-383">*Vytvoření nové Album*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="b9bbb-384">Cvičení 5: Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="b9bbb-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="b9bbb-385">Možnost odstranit alb ještě není naimplementovaný.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="b9bbb-386">To je, co toto cvičení bude o.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-386">This is what this exercise will be about.</span></span> <span data-ttu-id="b9bbb-387">Podobně jako předtím implementujete scénář odstranění pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="b9bbb-388">Jedna metoda akce se zobrazí potvrzení formuláře</span><span class="sxs-lookup"><span data-stu-id="b9bbb-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="b9bbb-389">Druhá metoda akce bude zpracovávat odeslání formuláře</span><span class="sxs-lookup"><span data-stu-id="b9bbb-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="b9bbb-390">Úloha 1 – implementace metody GET protokolu HTTP Delete akce</span><span class="sxs-lookup"><span data-stu-id="b9bbb-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="b9bbb-391">V této úloze budete implementovat verze HTTP GET metody akce Delete se načíst informace o alba.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="b9bbb-392">Otevřít **začít** řešení nachází v **zdroj/Ex5-HandlingDeletion/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="b9bbb-393">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9bbb-394">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-395">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-396">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-397">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-398">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-399">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-400">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-401">Otevřít **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b9bbb-402">Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="b9bbb-403">Odstranění akce kontroleru je přesně stejný jako předchozí akce kontroleru Store podrobnosti: se dotázal **alba** objekt z databáze pomocí **id** součástí adresy URL a vrátí odpovídající **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="b9bbb-404">Chcete-li to provést, nahraďte HTTP GET **odstranit** akce metoda kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="b9bbb-405">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex5 zpracování odstranění – odstranit HTTP GET – akce*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="b9bbb-406">Klepněte pravým tlačítkem myši **odstranit** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="b9bbb-407">Tím se otevře dialogové okno Přidat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="b9bbb-408">V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="b9bbb-409">Vyberte **vytvoření zobrazení se silnými typy** možnost a vyberte **alba (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="b9bbb-410">Vyberte **odstranit** z **šablona Scaffold** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="b9bbb-411">Ostatní pole nechte výchozí hodnoty a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="b9bbb-412">![Přidání zobrazení odstranit](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "přidání odstranit zobrazení")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="b9bbb-413">*Přidání zobrazení Delete*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="b9bbb-414">Odstranit šablonu zobrazí všechna pole z modelu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="b9bbb-415">Se zobrazí pouze alba název.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-415">You will show only the album's title.</span></span> <span data-ttu-id="b9bbb-416">Provedete to tak, nahradí obsah zobrazení s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b9bbb-417">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="b9bbb-418">V této úloze budete testovat, který **StoreManager** **odstranit** zobrazení stránky zobrazí formulář potvrzení odstranění.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="b9bbb-419">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-420">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-420">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-421">Změňte adresu URL na **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="b9bbb-422">Vyberte jeden alba odstranit tak, že kliknete na **odstranit** a ověřte, že se nahraje nové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="b9bbb-423">![Odstraňuje se alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "odstranění alba")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="b9bbb-424">*Odstraňuje se alba*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="b9bbb-425">Úloha 3 – implementace metody akce Delete HTTP POST</span><span class="sxs-lookup"><span data-stu-id="b9bbb-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="b9bbb-426">V této úloze budete implementovat verze HTTP POST metody akce odstranění, který bude vyvolán při kliknutí **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="b9bbb-427">Metoda by měla odstranit album v databázi.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="b9bbb-428">Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b9bbb-429">Otevřít **StoreManagerController** třídy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="b9bbb-430">Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="b9bbb-431">Nahraďte **HTTP POST odstranit** akce metoda kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="b9bbb-432">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex5 zpracování odstranění – odstranit HTTP POST – akce*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b9bbb-433">Úloha 4 – spouštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="b9bbb-434">V této úloze budete testovat, který **StoreManager odstranit** stránka zobrazení můžete odstranit alba a pak přesměruje na zobrazení indexu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="b9bbb-435">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-436">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-436">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-437">Změňte adresu URL na **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="b9bbb-438">Vyberte jeden alba odstranit tak, že kliknete na **odstranit.**</span><span class="sxs-lookup"><span data-stu-id="b9bbb-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="b9bbb-439">Potvrďte odstranění kliknutím **odstranit** tlačítka:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="b9bbb-440">![Odstraňuje se alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "odstranění alba")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="b9bbb-441">*Odstraňuje se alba*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="b9bbb-442">Ověřte, že byl alba odstranit, protože se nezobrazují v **Index** stránky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="b9bbb-443">Cvičení 6: Přidání ověření</span><span class="sxs-lookup"><span data-stu-id="b9bbb-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="b9bbb-444">Vytvoření a úprava formuláře, které máte v současné době neprovádějte jakéhokoli druhu ověřování.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="b9bbb-445">Pokud uživatel ponechá povinné pole je prázdné nebo písmena typ v poli pro cenu, první chyby, které dostanete se z databáze.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="b9bbb-446">Můžete přidat ověřování do aplikace tak, že přidáte anotacemi dat do vaší třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="b9bbb-447">Anotací dat při povolení popisující pravidla, která chcete použít pro vlastnosti modelu a ASP.NET MVC se postará o vynucení a zobrazení odpovídající zprávu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="b9bbb-448">Úloha 1 – přidání anotací dat</span><span class="sxs-lookup"><span data-stu-id="b9bbb-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="b9bbb-449">V této úloze přidáte anotacemi dat alba model, který bude na stránce vytvořit a upravit zobrazení ověřovacích zpráv v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="b9bbb-450">Pro jednoduchou třídu modelu, přidávání poznámek Data právě probíhá tak, že přidáte **pomocí** příkaz pro **System.ComponentModel.DataAnnotation**, pak umístit **[povinné]** atribut na odpovídající vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="b9bbb-451">V následujícím příkladu s žádným **název** vlastnost povinné pole. v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="b9bbb-452">Toto je o něco složitější v případech, jako je tato aplikace, ve kterém se vygeneruje modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="b9bbb-453">Pokud jste přidali anotacemi dat přímo do třídy modelu, byste při aktualizaci modelu z databáze nástroje přepsány.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="b9bbb-454">Místo toho můžete provést pomocí metadat částečné třídy, které budou existovat pro uložení poznámky a jsou spojeny s modelem třídy pomocí **[MetadataType]** atribut.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="b9bbb-455">Otevřít **začít** řešení nachází v **zdroj/Ex6-AddingValidation/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="b9bbb-456">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9bbb-457">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-458">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-459">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-460">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-461">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-462">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-463">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-464">Otevřít **Album.cs** z **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="b9bbb-465">Nahraďte **Album.cs** obsahu se zvýrazněný kód tak, aby vypadal jako následující:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9bbb-466">Na řádku **[DisplayFormat(ConvertEmptyStringToNull=false)]** označuje, že prázdné řetězce z modelu nebudou převedeny na hodnotu null při aktualizaci ve zdroji dat datové pole.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="b9bbb-467">Toto nastavení zabrání výjimku, pokud předtím, než Data anotace ověřuje pole Entity Framework přiřadí hodnoty null do modelu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="b9bbb-468">(Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - částečné třídy metadat Ex6 alba*)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-469">To **alba** má částečné třídy **MetadataType** atribut, který odkazuje na **AlbumMetaData** třídy pro datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="b9bbb-470">Zde je několik příkladů dat anotace atributy, které používáte opatřit poznámkami alba modelu:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="b9bbb-471">Nepožadováno – označuje, že vlastnost je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="b9bbb-472">DisplayName – definuje text bude použit na pole formuláře a ověřovacích zpráv</span><span class="sxs-lookup"><span data-stu-id="b9bbb-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="b9bbb-473">DisplayFormat – určuje způsob zobrazení a ve formátu datová pole.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="b9bbb-474">StringLength – definuje maximální délku pole řetězce</span><span class="sxs-lookup"><span data-stu-id="b9bbb-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="b9bbb-475">V rozsahu - poskytuje maximální a minimální hodnoty pro číselné pole</span><span class="sxs-lookup"><span data-stu-id="b9bbb-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="b9bbb-476">ScaffoldColumn – umožňuje skrýt pole z editoru formuláře</span><span class="sxs-lookup"><span data-stu-id="b9bbb-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b9bbb-477">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b9bbb-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="b9bbb-478">V této úloze budete testovat, vytvoření a úprava stránky ověření pole, pomocí zobrazované názvy zvolili v minulé úloze.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="b9bbb-479">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b9bbb-480">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-480">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-481">Změňte adresu URL na **/StoreManager/vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="b9bbb-482">Ověřte, že zobrazované názvy odpovídají v částečné třídy (jako je **alba obrázky URL** místo **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="b9bbb-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="b9bbb-483">Klikněte na tlačítko **vytvořit**, bez vyplňování formuláře.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="b9bbb-484">Ověřte, že dostanete odpovídajících ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="b9bbb-485">![Ověření pole na stránce vytvořit](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "ověření pole na stránce vytvořit")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="b9bbb-486">*Ověřená polí na stránce vytvořit*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="b9bbb-487">Můžete ověřit, že stejné dochází u **upravit** stránky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="b9bbb-488">Změňte adresu URL na **/StoreManager/Edit/1** a ověřte, že zobrazované názvy odpovídají v částečné třídy (jako je **alba obrázky URL** místo **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="b9bbb-489">Prázdný **Title** a **cena** pole a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="b9bbb-490">Ověřte, že dostanete odpovídajících ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-490">Verify that you get the corresponding validation messages.</span></span>

    ![Ověřená polí na stránce Upravit](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="b9bbb-492">*Ověřená polí na stránce Upravit*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="b9bbb-493">Cvičení 7: Pomocí Nerušivého jQuery na straně klienta</span><span class="sxs-lookup"><span data-stu-id="b9bbb-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="b9bbb-494">V tomto cvičení se dozvíte, jak povolit MVC 4 Nerušivý jQuery ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="b9bbb-495">Nerušivý jQuery používá data-ajax předponu JavaScript volat v serveru místo intrusively generování skriptů klienta vložené metody akce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="b9bbb-496">Úloha 1 – spuštění aplikace před povolením Nerušivý jQuery</span><span class="sxs-lookup"><span data-stu-id="b9bbb-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="b9bbb-497">V této úloze budete spouštět aplikace před zahrnutím jQuery k porovnání obou modelů ověřování.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="b9bbb-498">Otevřít **začít** řešení nachází v **zdroj/Ex7-UnobtrusivejQueryValidation/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="b9bbb-499">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9bbb-500">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9bbb-501">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9bbb-502">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9bbb-503">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9bbb-504">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9bbb-505">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9bbb-506">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9bbb-507">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="b9bbb-508">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-508">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-509">Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplňování formuláře a přesvědčte se, že dostanete zprávy ověření na:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="b9bbb-510">![Zakázáno ověřování na straně klienta](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "zakázáno ověřování na straně klienta")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="b9bbb-511">*Zakázáno ověřování na straně klienta*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="b9bbb-512">V prohlížeči otevřete zdrojový kód HTML:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="b9bbb-513">Úloha 2 – povolení ověřování Nerušivý klienta</span><span class="sxs-lookup"><span data-stu-id="b9bbb-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="b9bbb-514">V této úloze povolíte jQuery **ověření nerušivého klienta** z **Web.config** soubor, který je ve výchozím nastavení nastavena na hodnotu false ve všech nových projektech ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="b9bbb-515">Kromě toho můžete přidá že nezbytné skripty odkazy, aby jQuery pracovní Nerušivý ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="b9bbb-516">Otevřít **Web.Config** soubor v kořenu projektu a ujistěte se, že **ClientValidationEnabled** a **UnobtrusiveJavaScriptEnabled** hodnoty klíče jsou nastaveny na **true**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-517">Můžete také povolit ověřování na straně klienta pomocí kódu v Global.asax.cs stejné výsledky:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="b9bbb-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="b9bbb-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="b9bbb-519">Kromě toho můžete přiřadit ClientValidationEnabled atribut do libovolného řadiče mít vlastní chování.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="b9bbb-520">Otevřít **Create.cshtml** na **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="b9bbb-521">Ujistěte se, že následující soubory skriptu, **jquery.validate** a **jquery.validate.unobtrusive**, přes zobrazení odkazuje &quot; **~/bundles/jqueryval** &quot; sady.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b9bbb-522">Tyto knihovny jQuery jsou součástí nové projekty MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="b9bbb-523">Můžete najít další knihovny v **/skripty** složce projektu je.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="b9bbb-524">Aby bylo toto ověření pracovní knihovny, budete muset přidat odkaz na framework knihovnu jQuery.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="b9bbb-525">Vzhledem k tomu, že tento odkaz je již přidána do  **\_Layout.cshtml** souboru, není potřeba ji přidat do tohoto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="b9bbb-526">Úloha 3 – spuštění aplikace pomocí Nerušivého jQuery ověření</span><span class="sxs-lookup"><span data-stu-id="b9bbb-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="b9bbb-527">V této úloze budete testovat, který **StoreManager** vytvořte zobrazení šablony provádí ověřování na straně klienta pomocí knihovny jQuery, když uživatel vytvoří nové album.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="b9bbb-528">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b9bbb-529">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-529">The project starts in the Home page.</span></span> <span data-ttu-id="b9bbb-530">Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplňování formuláře a přesvědčte se, že dostanete zprávy ověření na:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="b9bbb-531">![Ověřování na straně klienta s jQuery povolené](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "ověřování na straně klienta s jQuery povoleno")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="b9bbb-532">*Ověřování na straně klienta s jQuery povoleno*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="b9bbb-533">V prohlížeči otevřete zdrojový kód pro zobrazení pro vytváření:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="b9bbb-534">Pro každý ověřovací pravidlo klienta Nerušivý jQuery přidá atribut s daty – val –*rulename*=&quot;*zpráva*&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="b9bbb-535">Níže je seznam značek tohoto Unobtrusive jQuery vloží do vstupního pole html k provedení ověřování na straně klienta:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="b9bbb-536">Data val</span><span class="sxs-lookup"><span data-stu-id="b9bbb-536">Data-val</span></span>
   > - <span data-ttu-id="b9bbb-537">Číslo datového val</span><span class="sxs-lookup"><span data-stu-id="b9bbb-537">Data-val-number</span></span>
   > - <span data-ttu-id="b9bbb-538">Oblast dat val</span><span class="sxs-lookup"><span data-stu-id="b9bbb-538">Data-val-range</span></span>
   > - <span data-ttu-id="b9bbb-539">Data val rozsah min / Data val range max</span><span class="sxs-lookup"><span data-stu-id="b9bbb-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="b9bbb-540">Vyžadována val data</span><span class="sxs-lookup"><span data-stu-id="b9bbb-540">Data-val-required</span></span>
   > - <span data-ttu-id="b9bbb-541">Délka dat val</span><span class="sxs-lookup"><span data-stu-id="b9bbb-541">Data-val-length</span></span>
   > - <span data-ttu-id="b9bbb-542">Data-val délka max / dat val délka min</span><span class="sxs-lookup"><span data-stu-id="b9bbb-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="b9bbb-543">Všechny datové hodnoty jsou vyplněny hodnotou modelu **dat. Poznámka**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="b9bbb-544">Potom veškerou logiku, která funguje na straně serveru může být spuštěna na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="b9bbb-545">Například cena atribut má následující poznámka data v modelu:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="b9bbb-546">Po použití Nerušivý jQuery, je generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b9bbb-547">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b9bbb-547">Summary</span></span>

<span data-ttu-id="b9bbb-548">Vyplněním tohoto praktického testovacího prostředí jste se naučili, jak povolit uživatelům změnit data uložená v databázi s použitím následující:</span><span class="sxs-lookup"><span data-stu-id="b9bbb-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="b9bbb-549">Akce kontroleru, jako je Index, vytvořit, upravit, odstranit</span><span class="sxs-lookup"><span data-stu-id="b9bbb-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="b9bbb-550">Funkce generování uživatelského rozhraní ASP.NET MVC pro zobrazení vlastností v tabulku HTML</span><span class="sxs-lookup"><span data-stu-id="b9bbb-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="b9bbb-551">Vlastní pomocné rutiny HTML pro zlepšení uživatelského prostředí</span><span class="sxs-lookup"><span data-stu-id="b9bbb-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="b9bbb-552">Metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání</span><span class="sxs-lookup"><span data-stu-id="b9bbb-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="b9bbb-553">Šablona sdíleného editor pro podobné zobrazení šablony, jako je vytvoření a úprava</span><span class="sxs-lookup"><span data-stu-id="b9bbb-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="b9bbb-554">Rozevírací seznamy, jako jsou elementy Form</span><span class="sxs-lookup"><span data-stu-id="b9bbb-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="b9bbb-555">Anotací dat při ověření modelu</span><span class="sxs-lookup"><span data-stu-id="b9bbb-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="b9bbb-556">Ověřování na straně klienta pomocí Nerušivého knihovny jQuery</span><span class="sxs-lookup"><span data-stu-id="b9bbb-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b9bbb-557">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="b9bbb-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b9bbb-558">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b9bbb-559">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b9bbb-560">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b9bbb-561">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b9bbb-562">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-562">Click on **Install Now**.</span></span> <span data-ttu-id="b9bbb-563">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b9bbb-564">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b9bbb-565">![Instalace sady Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b9bbb-566">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b9bbb-567">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="b9bbb-569">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b9bbb-570">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-570">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="b9bbb-572">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-572">*Installation progress*</span></span>
6. <span data-ttu-id="b9bbb-573">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-573">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="b9bbb-575">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-575">*Installation completed*</span></span>
7. <span data-ttu-id="b9bbb-576">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b9bbb-577">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="b9bbb-579">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b9bbb-580">Příloha B: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="b9bbb-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b9bbb-581">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b9bbb-582">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b9bbb-583">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b9bbb-584">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b9bbb-585">***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***</span><span class="sxs-lookup"><span data-stu-id="b9bbb-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b9bbb-586">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b9bbb-587">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b9bbb-588">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b9bbb-589">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="b9bbb-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b9bbb-590">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b9bbb-591">![Začněte psát název fragmentu kódu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b9bbb-592">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="b9bbb-593">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b9bbb-594">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b9bbb-595">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b9bbb-596">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b9bbb-597">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b9bbb-598">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b9bbb-599">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b9bbb-600">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="b9bbb-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b9bbb-601">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b9bbb-602">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b9bbb-603">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="b9bbb-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b9bbb-604">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="b9bbb-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
