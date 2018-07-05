---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Zobrazení tabulky databázových dat (VB) | Dokumentace Microsoftu
author: microsoft
description: V tomto kurzu se můžu ukazují dvě metody zobrazení sady záznamů v databázi. Můžu zobrazit dvě metody formátování sadu záznamů databáze v HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f5d9062d901f28b1d64f400e13ed5eb0ca788ab8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362489"
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="34fbd-104">Zobrazení tabulky databázových dat (VB)</span><span class="sxs-lookup"><span data-stu-id="34fbd-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="34fbd-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="34fbd-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="34fbd-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="34fbd-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="34fbd-107">V tomto kurzu se můžu ukazují dvě metody zobrazení sady záznamů v databázi.</span><span class="sxs-lookup"><span data-stu-id="34fbd-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="34fbd-108">Můžu zobrazit dvě metody formátování sadu záznamů databáze v tabulku HTML.</span><span class="sxs-lookup"><span data-stu-id="34fbd-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="34fbd-109">Nejprve mohu zobrazit, jak lze formátovat záznamů databáze přímo v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="34fbd-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="34fbd-110">V dalším kroku můžu ukazují, jak můžete využít výhod částečných zobrazení při formátování záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="34fbd-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="34fbd-111">Cílem tohoto kurzu je vysvětlují, jak můžete zobrazit tabulku HTML databázových dat v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34fbd-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="34fbd-112">Nejprve se dozvíte, jak použít nástroje pro generování uživatelského rozhraní zahrnuté v sadě Visual Studio ke generování zobrazení, které se automaticky zobrazí sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="34fbd-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="34fbd-113">V dalším kroku se dozvíte, jak použít částečné jako šablonu při formátování záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="34fbd-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="34fbd-114">Vytvoření tříd modelu</span><span class="sxs-lookup"><span data-stu-id="34fbd-114">Create the Model Classes</span></span>

<span data-ttu-id="34fbd-115">Budeme zobrazit sadu záznamů v tabulce databáze filmů.</span><span class="sxs-lookup"><span data-stu-id="34fbd-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="34fbd-116">V tabulce databáze filmů obsahuje následující sloupce:</span><span class="sxs-lookup"><span data-stu-id="34fbd-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="34fbd-117">**Název sloupce**</span><span class="sxs-lookup"><span data-stu-id="34fbd-117">**Column Name**</span></span> | <span data-ttu-id="34fbd-118">**Datový typ**</span><span class="sxs-lookup"><span data-stu-id="34fbd-118">**Data Type**</span></span> | <span data-ttu-id="34fbd-119">**Povolit hodnoty Null**</span><span class="sxs-lookup"><span data-stu-id="34fbd-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="34fbd-120">ID</span><span class="sxs-lookup"><span data-stu-id="34fbd-120">Id</span></span> | <span data-ttu-id="34fbd-121">int</span><span class="sxs-lookup"><span data-stu-id="34fbd-121">Int</span></span> | <span data-ttu-id="34fbd-122">False</span><span class="sxs-lookup"><span data-stu-id="34fbd-122">False</span></span> |
| <span data-ttu-id="34fbd-123">Název</span><span class="sxs-lookup"><span data-stu-id="34fbd-123">Title</span></span> | <span data-ttu-id="34fbd-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="34fbd-124">Nvarchar(200)</span></span> | <span data-ttu-id="34fbd-125">False</span><span class="sxs-lookup"><span data-stu-id="34fbd-125">False</span></span> |
| <span data-ttu-id="34fbd-126">Ředitel</span><span class="sxs-lookup"><span data-stu-id="34fbd-126">Director</span></span> | <span data-ttu-id="34fbd-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="34fbd-127">NVarchar(50)</span></span> | <span data-ttu-id="34fbd-128">False</span><span class="sxs-lookup"><span data-stu-id="34fbd-128">False</span></span> |
| <span data-ttu-id="34fbd-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="34fbd-129">DateReleased</span></span> | <span data-ttu-id="34fbd-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="34fbd-130">DateTime</span></span> | <span data-ttu-id="34fbd-131">False</span><span class="sxs-lookup"><span data-stu-id="34fbd-131">False</span></span> |


<span data-ttu-id="34fbd-132">Aby mohl představovat filmy tabulky v naší aplikaci ASP.NET MVC, musíme vytvořit třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="34fbd-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="34fbd-133">V tomto kurzu vytvoříme pomocí Microsoft Entity Framework našich tříd modelu.</span><span class="sxs-lookup"><span data-stu-id="34fbd-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34fbd-134">V tomto kurzu používáme Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="34fbd-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="34fbd-135">Je důležité pochopit, že můžete použít celou řadu různých technologií pro interakci s databází z aplikace ASP.NET MVC včetně technologie LINQ to SQL nebo NHibernate, ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="34fbd-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="34fbd-136">Použijte následující postup spuštění Průvodce datovým modelem Entity:</span><span class="sxs-lookup"><span data-stu-id="34fbd-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="34fbd-137">Klikněte pravým tlačítkem na složku modely v okně Průzkumník řešení a vyberte možnost nabídky **přidat, nová položka**.</span><span class="sxs-lookup"><span data-stu-id="34fbd-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="34fbd-138">Vyberte **Data** kategorie a vyberte **datový Model Entity ADO.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="34fbd-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="34fbd-139">Zadejte název datového modelu *MoviesDBModel.edmx* a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34fbd-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="34fbd-140">Po kliknutí na tlačítko Přidat, zobrazí se Průvodce datovým modelem Entity (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="34fbd-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="34fbd-141">Postupujte podle následujících kroků dokončete průvodce:</span><span class="sxs-lookup"><span data-stu-id="34fbd-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="34fbd-142">V **výběr obsahu modelu** kroku, vyberte **Generovat z databáze** možnost.</span><span class="sxs-lookup"><span data-stu-id="34fbd-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="34fbd-143">V **vyberte datové připojení** kroku, použijte *MoviesDB.mdf* datové připojení a názvu *MoviesDBEntities* pro nastavení připojení.</span><span class="sxs-lookup"><span data-stu-id="34fbd-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="34fbd-144">Klikněte na tlačítko **Další** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34fbd-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="34fbd-145">V **zvolte vaše databázové objekty** krok, rozbalte uzel tabulky, vyberte v tabulce videa.</span><span class="sxs-lookup"><span data-stu-id="34fbd-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="34fbd-146">Zadejte obor názvů *modely* a klikněte na tlačítko **Dokončit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34fbd-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="34fbd-147">[![Vytvoření LINQ na třídy SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="34fbd-148">**Obrázek 01**: vytváření třídy LINQ to SQL ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="34fbd-149">Po dokončení Průvodce datovým modelem Entity, otevře se Návrhář Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="34fbd-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="34fbd-150">Návrhář zobrazeno filmy entity (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="34fbd-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="34fbd-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="34fbd-152">**Obrázek 02**: Entity Data Model Designer ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="34fbd-153">Potřebujeme, aby jednu změnu, abychom mohli pokračovat.</span><span class="sxs-lookup"><span data-stu-id="34fbd-153">We need to make one change before we continue.</span></span> <span data-ttu-id="34fbd-154">Průvodce Entity Data vygeneruje třídu modelu s názvem *filmy* , který představuje tabulku databáze filmů.</span><span class="sxs-lookup"><span data-stu-id="34fbd-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="34fbd-155">Vzhledem k tomu použijeme filmy třídy představující konkrétní videa, potřeba změnit název třídy, která má být *film* místo *filmy* (singulární spíše než množné číslo).</span><span class="sxs-lookup"><span data-stu-id="34fbd-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="34fbd-156">Dvakrát klikněte na název třídy na návrhové ploše a změňte název třídy z videa na video.</span><span class="sxs-lookup"><span data-stu-id="34fbd-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="34fbd-157">Po provedení této změny, klikněte na tlačítko **Uložit** tlačítko (ikona disketu) ke generování třídy Video.</span><span class="sxs-lookup"><span data-stu-id="34fbd-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="34fbd-158">Vytvoří kontroler filmy</span><span class="sxs-lookup"><span data-stu-id="34fbd-158">Create the Movies Controller</span></span>

<span data-ttu-id="34fbd-159">Teď, když jsme způsob, jak reprezentaci našich záznamů databáze, můžeme vytvořit kontroler, který vrátí kolekce filmů.</span><span class="sxs-lookup"><span data-stu-id="34fbd-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="34fbd-160">V okně Průzkumník řešení Visual Studio klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, řadič** (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="34fbd-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="34fbd-161">[![Přidání Kontroleru nabídky](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="34fbd-162">**Obrázek 03**: The přidání Kontroleru nabídky ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="34fbd-163">Když **přidat kontroler** se zobrazí dialogové okno, zadejte název řadiče MovieController (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="34fbd-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="34fbd-164">Klikněte na tlačítko **přidat** tlačítko pro přidání nového řadiče.</span><span class="sxs-lookup"><span data-stu-id="34fbd-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="34fbd-165">[![Dialogové okno Přidat kontroler](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="34fbd-166">**Obrázek 04**: dialogové okno pro přidání Kontroleru ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="34fbd-167">Musíme akce Index() vystavené film řadič tak, aby vracel sadu záznamů databáze změnit.</span><span class="sxs-lookup"><span data-stu-id="34fbd-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="34fbd-168">Upravte kontrolér tak, aby vypadal jako řadič v informacích 1.</span><span class="sxs-lookup"><span data-stu-id="34fbd-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="34fbd-169">**Výpis 1 – Controllers\MovieController.vb**</span><span class="sxs-lookup"><span data-stu-id="34fbd-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="34fbd-170">Ve výpisu 1 je třída MoviesDBEntities používá k reprezentování MoviesDB databáze.</span><span class="sxs-lookup"><span data-stu-id="34fbd-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="34fbd-171">Výraz *entity. MovieSet.ToList()* vrací sadu všech filmy z databázové tabulky videa.</span><span class="sxs-lookup"><span data-stu-id="34fbd-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="34fbd-172">Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="34fbd-172">Create the View</span></span>

<span data-ttu-id="34fbd-173">Nejjednodušší způsob, jak zobrazit sadu záznamů databáze v tabulce HTML je využívat generování uživatelského rozhraní poskytovaný sadou Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34fbd-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="34fbd-174">Sestavení aplikace tak, že vyberete možnost nabídky **vytvořit, sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="34fbd-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="34fbd-175">Je nutné vytvořit aplikaci před otevřením **přidat zobrazení** nebude v dialogovém okně zobrazí dialogové okno nebo datových tříd.</span><span class="sxs-lookup"><span data-stu-id="34fbd-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="34fbd-176">Klikněte pravým tlačítkem na akce Index() a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="34fbd-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="34fbd-177">[![Přidání zobrazení](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="34fbd-178">**Obrázek 05**: Přidání zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="34fbd-179">V **přidat zobrazení** dialogového okna, zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**.</span><span class="sxs-lookup"><span data-stu-id="34fbd-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="34fbd-180">Vyberte třídu film, jako **zobrazení dat třídy**.</span><span class="sxs-lookup"><span data-stu-id="34fbd-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="34fbd-181">Vyberte *seznamu* jako **zobrazit obsah** (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="34fbd-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="34fbd-182">Výběr tyto možnosti budou generovat zobrazení silného typu, který zobrazí seznam filmy.</span><span class="sxs-lookup"><span data-stu-id="34fbd-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="34fbd-183">[![Dialogové okno Přidat zobrazení](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="34fbd-184">**Obrázek 06**: dialogové okno pro přidání zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="34fbd-185">Po klepnutí **přidat** automaticky generováno tlačítko, zobrazení, ve výpisu 2.</span><span class="sxs-lookup"><span data-stu-id="34fbd-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="34fbd-186">Toto zobrazení obsahuje kód potřebný k iteraci v rámci kolekce filmů a zobrazit vlastnosti videa.</span><span class="sxs-lookup"><span data-stu-id="34fbd-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="34fbd-187">**Výpis 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="34fbd-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="34fbd-188">Aplikaci můžete spustit tak, že vyberete možnost nabídky **ladit, spustit ladění** (nebo stisknutí klávesy F5).</span><span class="sxs-lookup"><span data-stu-id="34fbd-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="34fbd-189">Spuštění aplikace se spustí aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="34fbd-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="34fbd-190">Když přejdete na adresu URL /Movie uvidíte stránku na obrázku 7.</span><span class="sxs-lookup"><span data-stu-id="34fbd-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="34fbd-191">[![Tabulku filmy](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="34fbd-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="34fbd-192">**Obrázek 07**: tabulku filmy ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="34fbd-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="34fbd-193">Pokud se vám nic o vzhledu mřížky databázových záznamů na obrázku 7 můžete jednoduše upravit zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="34fbd-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="34fbd-194">Například můžete změnit *DateReleased* záhlaví *datum vydání* úpravou zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="34fbd-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="34fbd-195">Vytvoření šablony s částečné</span><span class="sxs-lookup"><span data-stu-id="34fbd-195">Create a Template with a Partial</span></span>

<span data-ttu-id="34fbd-196">Při zobrazení získá příliš složitý, je vhodné spustit rozdělení do částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="34fbd-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="34fbd-197">Pomocí částečných zobrazení usnadňuje zobrazení pochopit a udržovat.</span><span class="sxs-lookup"><span data-stu-id="34fbd-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="34fbd-198">Vytvoříme částečné, můžeme použít jako šablonu k formátování jednotlivých záznamů databáze filmů.</span><span class="sxs-lookup"><span data-stu-id="34fbd-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="34fbd-199">Postupujte podle těchto kroků můžete vytvořit části:</span><span class="sxs-lookup"><span data-stu-id="34fbd-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="34fbd-200">Klikněte pravým tlačítkem na složku Views\Movie a vyberte možnost nabídky **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="34fbd-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="34fbd-201">Zaškrtněte políčko s popiskem *vytvořit částečné zobrazení (prvků.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="34fbd-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="34fbd-202">Pojmenujte částečnou *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="34fbd-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="34fbd-203">Zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**.</span><span class="sxs-lookup"><span data-stu-id="34fbd-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="34fbd-204">Vyberte video jako *zobrazení dat třídy*.</span><span class="sxs-lookup"><span data-stu-id="34fbd-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="34fbd-205">Vyberte prázdný jako *zobrazit obsah*.</span><span class="sxs-lookup"><span data-stu-id="34fbd-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="34fbd-206">Klikněte na tlačítko **přidat** tlačítko pro přidání částečné do projektu.</span><span class="sxs-lookup"><span data-stu-id="34fbd-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="34fbd-207">Po dokončení těchto kroků upravte MovieTemplate částečné vypadat výpis 3.</span><span class="sxs-lookup"><span data-stu-id="34fbd-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="34fbd-208">**Výpis 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="34fbd-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="34fbd-209">Částečné v 3 výpis obsahuje šablony pro jeden řádek záznamů.</span><span class="sxs-lookup"><span data-stu-id="34fbd-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="34fbd-210">Upravené zobrazení indexu v informacích 4 používá MovieTemplate částečné.</span><span class="sxs-lookup"><span data-stu-id="34fbd-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="34fbd-211">**Část 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="34fbd-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="34fbd-212">Zobrazení v informacích 4 obsahuje pro každou smyčku, která iteruje přes všechny videa.</span><span class="sxs-lookup"><span data-stu-id="34fbd-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="34fbd-213">Pro každý film částečné MovieTemplate slouží k formátování videa.</span><span class="sxs-lookup"><span data-stu-id="34fbd-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="34fbd-214">MovieTemplate generován voláním metody helper RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="34fbd-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="34fbd-215">Upravené Index zobrazení vykreslí velmi stejné tabulky HTML záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="34fbd-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="34fbd-216">Ale zobrazení je výrazně zjednodušené.</span><span class="sxs-lookup"><span data-stu-id="34fbd-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="34fbd-217">Metoda RenderPartial() je jiná než většina jiných metod helper, protože nevrací řetězec.</span><span class="sxs-lookup"><span data-stu-id="34fbd-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="34fbd-218">Proto musí volat metoda RenderPartial() použití &lt;Html.RenderPartial() %&gt; místo &lt;% = Html.RenderPartial() %&gt;.</span><span class="sxs-lookup"><span data-stu-id="34fbd-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="34fbd-219">Souhrn</span><span class="sxs-lookup"><span data-stu-id="34fbd-219">Summary</span></span>

<span data-ttu-id="34fbd-220">Cílem tohoto kurzu bylo vysvětlují, jak můžete zobrazit sadu záznamů databáze v tabulku HTML.</span><span class="sxs-lookup"><span data-stu-id="34fbd-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="34fbd-221">Nejdřív jste zjistili, jak vrátit sadu záznamů databáze z akce kontroleru s využitím rozhraní Entity Framework společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34fbd-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="34fbd-222">Dále jste zjistili, jak používat generování uživatelského rozhraní sady Visual Studio vygenerovat zobrazení, která zobrazuje kolekce položek automaticky.</span><span class="sxs-lookup"><span data-stu-id="34fbd-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="34fbd-223">Nakonec jste zjistili, jak zjednodušit využití výhod částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="34fbd-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="34fbd-224">Naučili jste se použít částečné jako šablonu tak, aby každý záznam v databázi lze formátovat.</span><span class="sxs-lookup"><span data-stu-id="34fbd-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="34fbd-225">[Předchozí](creating-model-classes-with-linq-to-sql-vb.md)
> [další](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="34fbd-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
