---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Načítání a zobrazení dat s model vazby a webových formulářů | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889092"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="d0cf9-104">Načítání a zobrazování dat pomocí vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="d0cf9-104">Retrieving and displaying data with model binding and web forms</span></span>
====================
<span data-ttu-id="d0cf9-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d0cf9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d0cf9-106">Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d0cf9-107">Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="d0cf9-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d0cf9-108">Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d0cf9-109">Vzor vazby modelu funguje s technologií pro přístup k žádné data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-109">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="d0cf9-110">V tomto kurzu budete používat rozhraní Entity Framework, ale můžete použít technologie přístup data, která je pro vás nejvíce dobře známé.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-110">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="d0cf9-111">Z ovládacího prvku serveru vázané na data, jako je například ovládacího prvku GridView ListView, DetailsView nebo FormView zadejte názvy metod, které chcete použít pro výběr, aktualizaci, odstranění a vytvoření data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-111">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="d0cf9-112">V tomto kurzu bude zadejte hodnotu pro metody SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-112">In this tutorial, you will specify a value for the SelectMethod.</span></span>
> 
> <span data-ttu-id="d0cf9-113">V rámci této metody poskytují logiku pro načítání dat.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-113">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="d0cf9-114">V dalším kurzu nastavíte hodnoty pro metodu UpdateMethod, metoda DeleteMethod a metodu InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-114">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
> 
> <span data-ttu-id="d0cf9-115">Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-115">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d0cf9-116">Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-116">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d0cf9-117">Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-117">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="d0cf9-118">V tomto kurzu spusťte aplikaci v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-118">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="d0cf9-119">Můžete provést také aplikace dostupné přes Internet pomocí nasazení do hostujícího zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-119">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="d0cf9-120">Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve</span><span class="sxs-lookup"><span data-stu-id="d0cf9-120">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="d0cf9-121">[Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="d0cf9-121">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="d0cf9-122">Informace o tom, jak nasadit webový projekt sady Visual Studio do Azure App Service Web Apps naleznete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) řady.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-122">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="d0cf9-123">Tento kurz také ukazuje, jak nasadit databázi SQL serveru do Azure SQL Database pomocí migrace Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d0cf9-124">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="d0cf9-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d0cf9-125">Microsoft Visual Studio 2013 nebo Microsoft Visual Studio Express 2013 pro Web</span><span class="sxs-lookup"><span data-stu-id="d0cf9-125">Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web</span></span>
>   
> 
> <span data-ttu-id="d0cf9-126">V tomto kurzu funguje taky s Visual Studio 2012, ale bude určité rozdíly v šabloně uživatelské rozhraní a projekt.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-126">This tutorial also works with Visual Studio 2012 but there will be some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d0cf9-127">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="d0cf9-127">What you'll build</span></span>

<span data-ttu-id="d0cf9-128">V tomto kurzu budete:</span><span class="sxs-lookup"><span data-stu-id="d0cf9-128">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d0cf9-129">Vytvoření datových objektů, které odráží vysoké školy s studenty zaregistrované v rámci přípravy</span><span class="sxs-lookup"><span data-stu-id="d0cf9-129">Build data objects that reflect a university with students enrolled in courses</span></span>
2. <span data-ttu-id="d0cf9-130">Vytvoření databázových tabulek z objektů</span><span class="sxs-lookup"><span data-stu-id="d0cf9-130">Build database tables from the objects</span></span>
3. <span data-ttu-id="d0cf9-131">Naplnění databáze testovací data</span><span class="sxs-lookup"><span data-stu-id="d0cf9-131">Populate the database with test data</span></span>
4. <span data-ttu-id="d0cf9-132">Zobrazení dat v webového formuláře</span><span class="sxs-lookup"><span data-stu-id="d0cf9-132">Display data in a web form</span></span>

## <a name="set-up-project"></a><span data-ttu-id="d0cf9-133">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="d0cf9-133">Set up project</span></span>

<span data-ttu-id="d0cf9-134">V sadě Visual Studio 2013, vytvořte novou **webové aplikace ASP.NET** názvem **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-134">In Visual Studio 2013, create a new **ASP.NET Web Application** called **ContosoUniversityModelBinding**.</span></span>

![Vytvoření projektu](retrieving-data/_static/image2.png)

<span data-ttu-id="d0cf9-136">Vyberte šablonu, webových formulářů a nechte výchozí možnosti.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-136">Select the Web Forms template, and leave the other default options.</span></span> <span data-ttu-id="d0cf9-137">Klikněte na tlačítko OK nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-137">Click OK to setup the project.</span></span>

![Vyberte webové formuláře](retrieving-data/_static/image3.png)

<span data-ttu-id="d0cf9-139">Bude nutné mít několik malé změny přizpůsobení vzhledu webu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-139">First, you will make a couple of small changes to customize the appearance of the site.</span></span> <span data-ttu-id="d0cf9-140">Otevřete **Site.Master** soubor a změňte název zahrnout Contoso univerzity místo Moje aplikace technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-140">Open the **Site.Master** file, and change the title to include Contoso University instead of My ASP.NET Application.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

<span data-ttu-id="d0cf9-141">Změňte text záhlaví z **název aplikace** k **Contoso univerzity**.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-141">Then, change the header text from **Application name** to **Contoso University**.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

<span data-ttu-id="d0cf9-142">Také v Site.Master, můžete změňte navigační odkazy, které se zobrazují v hlavičce tak, aby odrážela stránek, které jsou relevantní pro tuto lokalitu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-142">Also in Site.Master, change the navigation links that appear in the header to reflect the pages that are relevant to this site.</span></span> <span data-ttu-id="d0cf9-143">Nebudete potřebovat buď **o** stránky nebo **kontaktujte** stránky tak může tyto odkazy odebrat.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-143">You will not need either the **About** page or the **Contact** page so those links can be removed.</span></span> <span data-ttu-id="d0cf9-144">Místo toho je nutné odkaz na stránku s názvem **studenty**.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-144">Instead, you will need a link to a page called **Students**.</span></span> <span data-ttu-id="d0cf9-145">Tato stránka ještě nebyl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-145">This page has not been created yet.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

<span data-ttu-id="d0cf9-146">Uložte a zavřete Site.Master.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-146">Save and close Site.Master.</span></span>

<span data-ttu-id="d0cf9-147">Teď vytvoříte webového formuláře pro zobrazení dat student.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-147">Now, you'll create the web form for displaying student data.</span></span> <span data-ttu-id="d0cf9-148">Klikněte pravým tlačítkem na projekt, a **přidat** **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-148">Right-click your project, and **Add** a **New Item**.</span></span> <span data-ttu-id="d0cf9-149">Vyberte **webové formuláře se stránkou předlohy** šablony a pojmenujte ji **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-149">Select the **Web Form with Master Page** template, and name it **Students.aspx**.</span></span>

![Vytvoření stránky](retrieving-data/_static/image5.png)

<span data-ttu-id="d0cf9-151">Vyberte **Site.Master** jako stránky předlohy pro nový webový formulář.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-151">Select **Site.Master** as the master page for the new web form.</span></span>

## <a name="create-the-data-models-and-database"></a><span data-ttu-id="d0cf9-152">Vytvoření datové modely a databáze</span><span class="sxs-lookup"><span data-stu-id="d0cf9-152">Create the data models and database</span></span>

<span data-ttu-id="d0cf9-153">Migrace Code First použije k vytvoření objektů a odpovídající databázových tabulek.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-153">You will use Code First Migrations to create objects and the corresponding database tables.</span></span> <span data-ttu-id="d0cf9-154">Tyto tabulky se uloží informace o studenty a jejich kurzy.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-154">These tables will store information about the students and their courses.</span></span>

<span data-ttu-id="d0cf9-155">Ve složce modely, přidejte novou třídu s názvem **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-155">In the Models folder, add a new class named **UniversityModels.cs**.</span></span>

![Vytvoření třídy modelu](retrieving-data/_static/image7.png)

<span data-ttu-id="d0cf9-157">V tomto souboru Definujte třídy SchoolContext, studenty, registraci a během takto:</span><span class="sxs-lookup"><span data-stu-id="d0cf9-157">In this file, define the SchoolContext, Student, Enrollment, and Course classes as follows:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

<span data-ttu-id="d0cf9-158">Třída SchoolContext je odvozena od DbContext, která spravuje připojení k databázi a změny v datech.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-158">The SchoolContext class derives from DbContext, which manages the database connection and changes in the data.</span></span>

<span data-ttu-id="d0cf9-159">Ve třídě studenty, Všimněte si, atributy, které se aplikovaly **FirstName**, **LastName**, a **roku** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-159">In the Student class, notice the attributes that have been applied to the **FirstName**, **LastName**, and **Year** properties.</span></span> <span data-ttu-id="d0cf9-160">Tyto atributy se použije pro ověření dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-160">These attributes will be used for data validation in this tutorial.</span></span> <span data-ttu-id="d0cf9-161">Můžete zjednodušit kód pro tento ukázkový projekt, byly s atributy ověření dat označeny pouze tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-161">To simplify the code for this demonstration project, only these properties were marked with data-validation attributes.</span></span> <span data-ttu-id="d0cf9-162">V skutečné projektu platit na všechny vlastnosti, které je třeba ověřená data, například vlastnosti ve třídách registraci a během atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-162">In a real project, you would apply validation attributes to all properties that need validated data, such as properties in the Enrollment and Course classes.</span></span>

<span data-ttu-id="d0cf9-163">Uložte UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-163">Save UniversityModels.cs.</span></span>

<span data-ttu-id="d0cf9-164">Nástroje pro migrace Code First budete používat k nastavení databáze, založený na těchto třídách.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-164">You will use the tools for Code First Migrations to set up a database based on these classes.</span></span>

<span data-ttu-id="d0cf9-165">V **Konzola správce balíčků**, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="d0cf9-165">In **Package Manager Console**, run the command:</span></span>  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

<span data-ttu-id="d0cf9-166">Po úspěšném dokončení příkazu obdržíte zprávu s oznámením povoleným migrace</span><span class="sxs-lookup"><span data-stu-id="d0cf9-166">If the command completes successfully you will receive a message stating migrations have been enabled,</span></span>

![Povolit migrace](retrieving-data/_static/image8.png)

<span data-ttu-id="d0cf9-168">Všimněte si, že nový soubor s názvem **Configuration.cs** byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-168">Notice that a new file named **Configuration.cs** has been created.</span></span> <span data-ttu-id="d0cf9-169">V sadě Visual Studio je tento soubor otevřít automaticky po jejím vytvoření.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-169">In Visual Studio, this file is automatically opened after it is created.</span></span> <span data-ttu-id="d0cf9-170">Obsahuje třídu konfigurace **počáteční hodnoty** metodu, která umožňuje k předběžnému naplnění tabulky databáze s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-170">The Configuration class contains a **Seed** method which enables you to pre-populate the database tables with test data.</span></span>

<span data-ttu-id="d0cf9-171">Přidejte následující kód do metody počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-171">Add the following code to the Seed method.</span></span> <span data-ttu-id="d0cf9-172">Budete muset přidat **pomocí** údajů **ContosoUniversityModelBinding.Models** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-172">You'll need to add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

<span data-ttu-id="d0cf9-173">Uložte Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-173">Save Configuration.cs.</span></span>

<span data-ttu-id="d0cf9-174">V konzole Správce balíčků, spusťte příkaz `add-migration initial`.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-174">In the Package Manager Console, run the command `add-migration initial`.</span></span>

<span data-ttu-id="d0cf9-175">Spusťte příkaz `update-database`.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-175">Then, run the command `update-database`.</span></span>

<span data-ttu-id="d0cf9-176">Pokud se zobrazí výjimku při spuštění tohoto příkazu, je možné, že mají různé hodnoty pro StudentID a CourseID z hodnot v metodě počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-176">If you receive an exception when running this command, it is possible that the values for StudentID and CourseID have varied from the values in the Seed method.</span></span> <span data-ttu-id="d0cf9-177">Otevřete tyto tabulky v databázi a vyhledat existující hodnoty pro StudentID a CourseID.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-177">Open those tables in the database and find existing values for StudentID and CourseID.</span></span> <span data-ttu-id="d0cf9-178">Přidejte tyto hodnoty v kódu pro synchronizace replik indexů v tabulce registrace.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-178">Add those values in the code for seeding the Enrollments table.</span></span>

<span data-ttu-id="d0cf9-179">Soubor databáze byl přidán, ale je aktuálně skrytá v projektu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-179">The database file has been added, but it is currently hidden in the project.</span></span> <span data-ttu-id="d0cf9-180">Klikněte na tlačítko **zobrazit všechny soubory** zobrazit soubor.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-180">Click **Show All Files** to display the file.</span></span>

![Zobrazit všechny soubory](retrieving-data/_static/image10.png)

<span data-ttu-id="d0cf9-182">Všimněte si souboru .mdf se teď zobrazí v aplikaci\_složku Data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-182">Notice the .mdf file now appears in the App\_Data folder.</span></span>

![soubor databáze](retrieving-data/_static/image12.png)

<span data-ttu-id="d0cf9-184">Poklikejte na soubor MDF a otevřete Průzkumníka serveru.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-184">Double-click the .mdf file and open the Server Explorer.</span></span> <span data-ttu-id="d0cf9-185">V tabulkách teď existují a jsou naplněný daty.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-185">The tables now exist and are populated with data.</span></span>

![tabulky databáze](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a><span data-ttu-id="d0cf9-187">Zobrazit data z studenty a související tabulky</span><span class="sxs-lookup"><span data-stu-id="d0cf9-187">Display data from Students and related tables</span></span>

<span data-ttu-id="d0cf9-188">S daty v databázi nyní jste připraveni k načtení dat a jejich zobrazení na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-188">With data in the database, you are now ready to retrieve that data and display it in a web page.</span></span> <span data-ttu-id="d0cf9-189">Budete používat **GridView** ovládací prvek pro zobrazení dat ve sloupci a řádky.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-189">You will use a **GridView** control to display the data in columns and rows.</span></span>

<span data-ttu-id="d0cf9-190">Otevřete Students.aspx a vyhledejte **MainContent** zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-190">Open Students.aspx, and locate the **MainContent** placeholder.</span></span> <span data-ttu-id="d0cf9-191">V rámci zástupného symbolu, přidejte **GridView** ovládací prvek, který obsahuje následující kód.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-191">Within that placeholder, add a **GridView** control that includes the following code.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

<span data-ttu-id="d0cf9-192">Existuje několik důležité koncepty v Všimněte tímto kódem značek.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-192">There are a couple of important concepts in this markup code for you to notice.</span></span> <span data-ttu-id="d0cf9-193">První, Všimněte si, že je zadána hodnota pro **metody SelectMethod** vlastnost v elementu GridView.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-193">First, notice that a value is set for the **SelectMethod** property in the GridView element.</span></span> <span data-ttu-id="d0cf9-194">Tato hodnota určuje název metody, která se používá k načítání dat pro GridView.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-194">This value specifies the name of the method that is used for retrieving data for the GridView.</span></span> <span data-ttu-id="d0cf9-195">Tato metoda vytvoří v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-195">You will create this method in the next step.</span></span> <span data-ttu-id="d0cf9-196">Druhý, Všimněte si, že **ItemType** je nastavena na třídě studenty, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-196">Second, notice that the **ItemType** property is set to the Student class that you created earlier.</span></span> <span data-ttu-id="d0cf9-197">Nastavením této hodnoty lze odkazovat k vlastnostem této třídy v kódu značek.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-197">By setting this value, you can refer to properties of that class in the markup code.</span></span> <span data-ttu-id="d0cf9-198">Například třída Student obsahuje kolekci s názvem registrace.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-198">For example, the Student class contains a collection named Enrollments.</span></span> <span data-ttu-id="d0cf9-199">Můžete použít **Item.Enrollments** načíst tuto kolekci a pak použijte syntaxi LINQ načtení součet zaregistrovaná kredity pro každý student.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-199">You can use **Item.Enrollments** to retrieve that collection and then use LINQ syntax to retrieve the sum of enrolled credits for each student.</span></span>

<span data-ttu-id="d0cf9-200">V souboru kódu na pozadí, budete muset přidat metodu, která je určená pro **metody SelectMethod** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-200">In the code-behind file, you need to add the method that is specified for the **SelectMethod** value.</span></span> <span data-ttu-id="d0cf9-201">Otevřete **Students.aspx.cs**a přidejte **pomocí** pro příkazy **ContosoUniversityModelBinding.Models** a **System.Data.Entity**obory názvů.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-201">Open **Students.aspx.cs**, and add **using** statements for the **ContosoUniversityModelBinding.Models** and **System.Data.Entity** namespaces.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

<span data-ttu-id="d0cf9-202">Pak přidejte následující metodu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-202">Then, add the following method.</span></span> <span data-ttu-id="d0cf9-203">Všimněte si, že název tato metoda odpovídá názvu, který jste zadali pro metody SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-203">Notice that the name of this method matches the name you provided for SelectMethod.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

<span data-ttu-id="d0cf9-204">**Zahrnout** klauzule zvyšuje výkon tohoto dotazu, ale není nezbytné pro dotaz pro práci.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-204">The **Include** clause improves the performance of this query but is not essential for the query to work.</span></span> <span data-ttu-id="d0cf9-205">Bez klauzuli zahrnout by načíst data pomocí opožděného načítání, který zahrnuje odeslání samostatné dotaz do databáze pokaždé, když související s načíst data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-205">Without the Include clause, the data would be retrieved using lazy loading, which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="d0cf9-206">Tím, že poskytuje klauzuli zahrnout, data se načítají pomocí přes načítání, což znamená, všechna související data se načítají prostřednictvím jednoho dotazu databáze.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-206">By providing the Include clause, the data is retrieved using eager loading, which means all of the related data is retrieved through a single query of the database.</span></span> <span data-ttu-id="d0cf9-207">V případech, kde je většina souvisejících dat nesmí být může být použit, přes načítání méně efektivní, protože načítat další data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-207">In cases where most of the related data is not be used, eager loading can be less efficient because more data is retrieved.</span></span> <span data-ttu-id="d0cf9-208">Nicméně v takovém případě přes načítání poskytuje nejlepší výkon vzhledem k tomu, že se pro každý záznam zobrazí související data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-208">However, in this case, eager loading provides the best performance because the related data is displayed for each record.</span></span>

<span data-ttu-id="d0cf9-209">Pro další informace o výkonu zvážení při načítání související data, najdete v části s názvem **Lazy Eager a explicitní načítání souvisejících dat** v [čtení související Data pomocí rozhraní Entity Framework v ASP Aplikace .NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-209">For more information about performance consideration when loading related data, see the section titled **Lazy, Eager, and Explicit Loading of Related Data** in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) topic.</span></span>

<span data-ttu-id="d0cf9-210">Ve výchozím nastavení data jsou seřazená podle hodnot vlastnosti označen jako klíč.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-210">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="d0cf9-211">Můžete přidat klauzuli OrderBy chcete zadat jinou hodnotu pro řazení.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-211">You can add an OrderBy clause to specify a different value for sorting.</span></span> <span data-ttu-id="d0cf9-212">V tomto příkladu se používá výchozí vlastnost StudentID pro řazení.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-212">In this example, the default StudentID property is used for sorting.</span></span> <span data-ttu-id="d0cf9-213">V [řazení, stránkování a filtrování dat](sorting-paging-and-filtering-data.md) tématu, vám umožní uživateli vybrat sloupec pro řazení.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-213">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) topic, you will enable the user to select a column for sorting.</span></span>

<span data-ttu-id="d0cf9-214">Spuštění webové aplikace a přejděte na stránku studenty.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-214">Run your web application, and navigate to the Students page.</span></span> <span data-ttu-id="d0cf9-215">Na stránce studenti, kteří se zobrazí následující informace student.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-215">The Students page displays the following student information.</span></span>

![Zobrazit data](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="d0cf9-217">Automatické generování metody vazby modelu</span><span class="sxs-lookup"><span data-stu-id="d0cf9-217">Automatic generation of model binding methods</span></span>

<span data-ttu-id="d0cf9-218">Při práci přes tento kurz řady, můžete jednoduše zkopírovat kód z tohoto kurzu do projektu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-218">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="d0cf9-219">Jednou z nevýhod tohoto přístupu je však, nemusí se dozvěděli o funkce poskytované sadě Visual Studio k automatickému generování kódu pro metody vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-219">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="d0cf9-220">Při práci na vašich vlastních projektů, automatické generování kódu můžete ušetřit čas a získat představu o tom, jak implementovat operaci.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-220">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="d0cf9-221">Tato část popisuje funkci generování automatické kódu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-221">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="d0cf9-222">Tato část je pouze informativní a neobsahuje žádný kód, který je nutné implementovat ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-222">This section is only informational and does not contain any code you need to implement in your project.</span></span>

<span data-ttu-id="d0cf9-223">Při nastavení hodnoty pro **metody SelectMethod**, **metody UpdateMethod**, **metodu InsertMethod**, nebo **metody DeleteMethod** vlastností v kódu značek můžete vybrat **vytvoření nové metody** možnost.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-223">When setting a value for the **SelectMethod**, **UpdateMethod**, **InsertMethod**, or **DeleteMethod** properties in the markup code, you can select the **Create New Method** option.</span></span>

![Vytvoření nové metody](retrieving-data/_static/image18.png)

<span data-ttu-id="d0cf9-225">Visual Studio nejen vytvoří metodu v kódu s správný podpis, ale také generuje kód implementace a pomůže vám při provádění této operace.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-225">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to assist you with performing the operation.</span></span> <span data-ttu-id="d0cf9-226">Pokud je nejdřív nastavit **ItemType** vlastnost před použitím funkci generování automatické kód generovaný kód použije tento typ pro operace.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-226">If you first set the **ItemType** property before using the automatic code generation feature, the generated code will use that type for the operations.</span></span> <span data-ttu-id="d0cf9-227">Například při nastavení této vlastnosti metody UpdateMethod, následující kód se automaticky vygeneroval:</span><span class="sxs-lookup"><span data-stu-id="d0cf9-227">For example, when setting the UpdateMethod property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="d0cf9-228">Výše uvedený kód znovu, není nutné přidat do projektu.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-228">Again, the above code does not need to be added to your project.</span></span> <span data-ttu-id="d0cf9-229">V dalším kurzu budete implementovat metody pro aktualizaci, odstranění a přidání nová data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-229">In the next tutorial, you will implement methods for updating, deleting and adding new data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d0cf9-230">Závěr</span><span class="sxs-lookup"><span data-stu-id="d0cf9-230">Conclusion</span></span>

<span data-ttu-id="d0cf9-231">V tomto kurzu jste vytvořili třídy modelu dat a generuje databázi z těchto tříd.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-231">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="d0cf9-232">Tabulky databáze, které se naplní se testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-232">You filled the database tables with test data.</span></span> <span data-ttu-id="d0cf9-233">Použít vazby modelu k načtení dat z databáze a pak se zobrazují data v GridView.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-233">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="d0cf9-234">V dalším [kurzu](updating-deleting-and-creating-data.md) této série povolíte aktualizaci, odstranění a vytvoření data.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-234">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you will enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d0cf9-235">Next</span><span class="sxs-lookup"><span data-stu-id="d0cf9-235">Next</span></span>](updating-deleting-and-creating-data.md)
