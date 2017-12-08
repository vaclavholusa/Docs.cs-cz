---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: "Zobrazení tabulky dat databáze (C#) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu ukazují I dvě metody zobrazení sadu záznamů databáze. Zobrazit dvě metody formátování sadu záznamů databáze v kódu HTML tových..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 37ea081df2ee26e186669b815a4d769e1976ae9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="e0e00-104">Zobrazení tabulky dat databáze (C#)</span><span class="sxs-lookup"><span data-stu-id="e0e00-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="e0e00-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e0e00-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="e0e00-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="e0e00-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="e0e00-107">V tomto kurzu ukazují I dvě metody zobrazení sadu záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="e0e00-108">Zobrazit dvě metody formátování sadu záznamů databáze v tabulce jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="e0e00-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="e0e00-109">Nejprve I ukazují použití formátu záznamů databáze přímo v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e0e00-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="e0e00-110">V dalším kroku I ukazují, jak můžete využít výhod částečné při formátování záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="e0e00-111">Cílem tohoto kurzu je vysvětlují, jak můžete zobrazit tabulky HTML dat z databáze v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e0e00-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="e0e00-112">První můžete další informace o použití nástrojů generování uživatelského rozhraní v sadě Visual Studio ke generování zobrazení, které se automaticky zobrazí sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="e0e00-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="e0e00-113">Dále můžete další informace o použití částečné jako šablonu při formátování záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="e0e00-114">Vytvoření třídy modelu</span><span class="sxs-lookup"><span data-stu-id="e0e00-114">Create the Model Classes</span></span>

<span data-ttu-id="e0e00-115">Nyní klikněte na Zobrazit sadu záznamů z filmy databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0e00-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="e0e00-116">V tabulce databáze filmy obsahuje následující sloupce:</span><span class="sxs-lookup"><span data-stu-id="e0e00-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="e0e00-117">**Název sloupce**</span><span class="sxs-lookup"><span data-stu-id="e0e00-117">**Column Name**</span></span> | <span data-ttu-id="e0e00-118">**Datový typ**</span><span class="sxs-lookup"><span data-stu-id="e0e00-118">**Data Type**</span></span> | <span data-ttu-id="e0e00-119">**Povolit hodnoty Null**</span><span class="sxs-lookup"><span data-stu-id="e0e00-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0e00-120">ID</span><span class="sxs-lookup"><span data-stu-id="e0e00-120">Id</span></span> | <span data-ttu-id="e0e00-121">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e0e00-121">Int</span></span> | <span data-ttu-id="e0e00-122">False</span><span class="sxs-lookup"><span data-stu-id="e0e00-122">False</span></span> |
| <span data-ttu-id="e0e00-123">Název</span><span class="sxs-lookup"><span data-stu-id="e0e00-123">Title</span></span> | <span data-ttu-id="e0e00-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="e0e00-124">Nvarchar(200)</span></span> | <span data-ttu-id="e0e00-125">False</span><span class="sxs-lookup"><span data-stu-id="e0e00-125">False</span></span> |
| <span data-ttu-id="e0e00-126">Adresář nacházející</span><span class="sxs-lookup"><span data-stu-id="e0e00-126">Director</span></span> | <span data-ttu-id="e0e00-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="e0e00-127">NVarchar(50)</span></span> | <span data-ttu-id="e0e00-128">False</span><span class="sxs-lookup"><span data-stu-id="e0e00-128">False</span></span> |
| <span data-ttu-id="e0e00-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="e0e00-129">DateReleased</span></span> | <span data-ttu-id="e0e00-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="e0e00-130">DateTime</span></span> | <span data-ttu-id="e0e00-131">False</span><span class="sxs-lookup"><span data-stu-id="e0e00-131">False</span></span> |


<span data-ttu-id="e0e00-132">Aby mohl představovat filmy tabulky v naší aplikaci ASP.NET MVC, musíme vytvořit třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="e0e00-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="e0e00-133">V tomto kurzu používáme k vytváření tříd naše modelu Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0e00-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e0e00-134">V tomto kurzu používáme Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0e00-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="e0e00-135">Je ale důležité si uvědomit, že můžete použít řadu různých technologií pro interakci s databází z aplikace ASP.NET MVC, včetně technologie LINQ to SQL, NHibernate nebo ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="e0e00-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="e0e00-136">Postupujte podle těchto kroků spusťte průvodce Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="e0e00-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="e0e00-137">Klikněte pravým tlačítkem na složku modely v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky**.</span><span class="sxs-lookup"><span data-stu-id="e0e00-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="e0e00-138">Vyberte **Data** kategorie a vyberte **ADO.NET Entity Data Model** šablony.</span><span class="sxs-lookup"><span data-stu-id="e0e00-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="e0e00-139">Zadejte název datového modelu *MoviesDBModel.edmx* a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0e00-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="e0e00-140">Po kliknutí na tlačítko Přidat, zobrazí se průvodce Entity Data Model (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="e0e00-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="e0e00-141">Postupujte podle následujících kroků dokončete průvodce:</span><span class="sxs-lookup"><span data-stu-id="e0e00-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="e0e00-142">V **zvolte obsah modelu** krok, vyberte **generování z databáze** možnost.</span><span class="sxs-lookup"><span data-stu-id="e0e00-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="e0e00-143">V **vybrat datové připojení** krok, použijte *MoviesDB.mdf* datové připojení a názvu *MoviesDBEntities* pro nastavení připojení.</span><span class="sxs-lookup"><span data-stu-id="e0e00-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="e0e00-144">Klikněte **Další** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0e00-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="e0e00-145">V **zvolte si databázové objekty** krok, rozbalte uzel tabulky, vyberte tabulku, filmy.</span><span class="sxs-lookup"><span data-stu-id="e0e00-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="e0e00-146">Zadejte obor názvů *modely* a klikněte na **Dokončit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0e00-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="e0e00-147">[![Vytváření pro třídy SQL LINQ](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="e0e00-148">**Obrázek 01**: vytváření třídy LINQ to SQL ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="e0e00-149">Po dokončení průvodce Entity Data Model Entity Data Model Designer se otevře.</span><span class="sxs-lookup"><span data-stu-id="e0e00-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="e0e00-150">Návrhář by měl zobrazit entity filmy (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="e0e00-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="e0e00-151">[![Model dat Entity Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="e0e00-152">**Obrázek 02**: Entity Data Model Designer ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="e0e00-153">Je potřeba jeden změnu provést, abychom mohli pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e0e00-153">We need to make one change before we continue.</span></span> <span data-ttu-id="e0e00-154">Průvodce dat Entity vygeneruje třídu modelu s názvem *filmy* představující filmy databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0e00-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="e0e00-155">Protože třída filmy použijeme k reprezentaci konkrétní film, je potřeba změnit název třídy, která má být *film* místo *filmy* (singulární spíše než množném čísle).</span><span class="sxs-lookup"><span data-stu-id="e0e00-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="e0e00-156">Dvakrát klikněte na název třídy na plochu návrháře a změňte název třídy z filmy film.</span><span class="sxs-lookup"><span data-stu-id="e0e00-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="e0e00-157">Po provedení této změny, klikněte **Uložit** tlačítko (ikona disketu) ke generování film třídy.</span><span class="sxs-lookup"><span data-stu-id="e0e00-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="e0e00-158">Vytvoření řadiče filmy</span><span class="sxs-lookup"><span data-stu-id="e0e00-158">Create the Movies Controller</span></span>

<span data-ttu-id="e0e00-159">Teď, když máme způsob, jak představují našich záznamů databáze, můžeme vytvořit kontroler, který vrátí kolekce filmy.</span><span class="sxs-lookup"><span data-stu-id="e0e00-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="e0e00-160">V okně Průzkumníka řešení Visual Studio klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, řadiče** (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="e0e00-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="e0e00-161">[![Řadiči nabídka přidat](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="e0e00-162">**Obrázek 03**: nabídce přidat řadič ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="e0e00-163">Když **přidat kontroler** otevře se dialogové okno, zadejte název řadiče MovieController (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="e0e00-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="e0e00-164">Klikněte **přidat** tlačítko přidáte nový řadič.</span><span class="sxs-lookup"><span data-stu-id="e0e00-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="e0e00-165">[![Dialogové okno Přidat kontroler](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="e0e00-166">**Obrázek 04**: dialogové okno Přidat kontroler ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="e0e00-167">Potřebujeme upravit akce Index() vystavené řadičem film tak, aby vracel sadu záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="e0e00-168">Upravte kontroleru, aby vypadal jako řadič v výpis 1.</span><span class="sxs-lookup"><span data-stu-id="e0e00-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="e0e00-169">**Výpis 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="e0e00-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="e0e00-170">Třída MoviesDBEntities je v výpis 1, používá k reprezentování MoviesDB databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="e0e00-171">Pro tuto třídu použít, musíte importovat obor názvů MvcApplication1.Models takto:</span><span class="sxs-lookup"><span data-stu-id="e0e00-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="e0e00-172">pomocí MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="e0e00-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="e0e00-173">Výraz *entity. MovieSet.ToList()* vrací sadu všech filmy z filmy databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0e00-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="e0e00-174">Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="e0e00-174">Create the View</span></span>

<span data-ttu-id="e0e00-175">Nejjednodušší způsob, jak zobrazit také sadu záznamů databáze do tabulky HTML je využít výhod generování uživatelského rozhraní poskytované sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0e00-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="e0e00-176">Sestavit aplikaci tak, že vyberete možnost nabídky **sestavení, sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="e0e00-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="e0e00-177">Musíte sestavit aplikaci před otevřením **přidat zobrazení** dialogového nebo datových tříd se nezobrazí v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="e0e00-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="e0e00-178">Klikněte pravým tlačítkem na Index() akce a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="e0e00-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="e0e00-179">[![Přidání zobrazení](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="e0e00-180">**Obrázek 05**: Přidání zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="e0e00-181">V **přidat zobrazení** dialogové okno, zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**.</span><span class="sxs-lookup"><span data-stu-id="e0e00-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="e0e00-182">Vyberte třídu film, jako **zobrazit třída dat**.</span><span class="sxs-lookup"><span data-stu-id="e0e00-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="e0e00-183">Vyberte *seznamu* jako **zobrazit obsah** (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="e0e00-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="e0e00-184">Výběr těchto možnosti vygeneruje zobrazení silného typu, který zobrazí seznam filmy.</span><span class="sxs-lookup"><span data-stu-id="e0e00-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="e0e00-185">[![Dialogové okno Přidat zobrazení](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="e0e00-186">**Obrázek 06**: Přidat zobrazení dialogu ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="e0e00-187">Po kliknutí **přidat** tlačítko zobrazení v výpis 2 je generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="e0e00-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="e0e00-188">Toto zobrazení obsahuje je kód potřebný k procházení kolekce filmy a zobrazení ke každé z vlastností přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="e0e00-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="e0e00-189">**Výpis 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="e0e00-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="e0e00-190">Aplikaci můžete spustit výběrem možnosti nabídky **ladit, spustit ladění** (nebo stiskne klávesu F5).</span><span class="sxs-lookup"><span data-stu-id="e0e00-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="e0e00-191">Spuštění aplikace spustí se aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e0e00-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="e0e00-192">Pokud přejdete na adresu URL /Movie zobrazí stránku na obrázku 7.</span><span class="sxs-lookup"><span data-stu-id="e0e00-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="e0e00-193">[![Tabulku filmy](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e0e00-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="e0e00-194">**Obrázek 07**: tabulku filmy ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="e0e00-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="e0e00-195">Pokud chcete nic o vzhledu mřížky záznamů databáze na obrázku 7 můžete jednoduše upravit zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="e0e00-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="e0e00-196">Například můžete změnit *DateReleased* hlavičky k *datum vydání* úpravou zobrazení indexu.</span><span class="sxs-lookup"><span data-stu-id="e0e00-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="e0e00-197">Vytvoření šablony se částečné</span><span class="sxs-lookup"><span data-stu-id="e0e00-197">Create a Template with a Partial</span></span>

<span data-ttu-id="e0e00-198">Při zobrazení získá příliš složitý, je vhodné spustit rozdělení zobrazení na částečné.</span><span class="sxs-lookup"><span data-stu-id="e0e00-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="e0e00-199">Použití částečné usnadňuje zobrazení pro pochopení a údržbu.</span><span class="sxs-lookup"><span data-stu-id="e0e00-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="e0e00-200">Vytvoříme částečné, můžeme použít jako šablonu pro každý záznam film databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="e0e00-201">Postupujte podle těchto kroků můžete vytvořit s částečným:</span><span class="sxs-lookup"><span data-stu-id="e0e00-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="e0e00-202">Klikněte pravým tlačítkem na složku Views\Movie a vyberte možnost nabídky **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e0e00-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="e0e00-203">Zaškrtněte políčko s názvem bez přípony *vytvořit částečné zobrazení (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="e0e00-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="e0e00-204">Název s částečným *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="e0e00-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="e0e00-205">Zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**.</span><span class="sxs-lookup"><span data-stu-id="e0e00-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="e0e00-206">Vyberte video jako *zobrazit třída dat*.</span><span class="sxs-lookup"><span data-stu-id="e0e00-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="e0e00-207">Vyberte prázdný jako *zobrazit obsah*.</span><span class="sxs-lookup"><span data-stu-id="e0e00-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="e0e00-208">Klikněte **přidat** tlačítko s částečným přidáte do projektu.</span><span class="sxs-lookup"><span data-stu-id="e0e00-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="e0e00-209">Po dokončení těchto kroků upravte částečné MovieTemplate, aby vypadala jako výpis 3.</span><span class="sxs-lookup"><span data-stu-id="e0e00-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="e0e00-210">**Výpis 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="e0e00-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="e0e00-211">Partial v výpis 3 obsahuje šablonu pro jeden řádek záznamů.</span><span class="sxs-lookup"><span data-stu-id="e0e00-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="e0e00-212">Upravené zobrazení indexu v výpis 4 používá částečné MovieTemplate.</span><span class="sxs-lookup"><span data-stu-id="e0e00-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="e0e00-213">**Výpis 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="e0e00-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="e0e00-214">Zobrazení v výpis 4 obsahuje smyčka typu foreach, který iteruje v rámci všech videa.</span><span class="sxs-lookup"><span data-stu-id="e0e00-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="e0e00-215">Pro každý film částečné MovieTemplate slouží k formátování videa.</span><span class="sxs-lookup"><span data-stu-id="e0e00-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="e0e00-216">MovieTemplate je vykreslen metodou voláním metody helper RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="e0e00-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="e0e00-217">Upravené zobrazení indexu vykreslí velmi stejné tabulky HTML záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="e0e00-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="e0e00-218">Však zobrazení je výrazně jednodušší.</span><span class="sxs-lookup"><span data-stu-id="e0e00-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="e0e00-219">Metoda RenderPartial() je jiný než většina jiných metod helper, protože nevrací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e0e00-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="e0e00-220">Proto musí volat pomocí metody RenderPartial() &lt;% Html.RenderPartial(); %&gt; místo &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="e0e00-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="e0e00-221">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e0e00-221">Summary</span></span>

<span data-ttu-id="e0e00-222">Cílem tohoto kurzu bylo vysvětlují, jak můžete zobrazit také sadu záznamů databáze do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="e0e00-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="e0e00-223">Nejprve jste zjistili, jak vracet sadu záznamů databáze z akce kontroleru a využívají k Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0e00-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="e0e00-224">V dalším kroku jste zjistili, jak generovat zobrazení, které zobrazuje kolekce položek automaticky pomocí generování uživatelského rozhraní sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0e00-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="e0e00-225">Nakonec jste zjistili, jak můžete zjednodušit využitím částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e0e00-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="e0e00-226">Jste zjistili, jak používat částečné jako šablona, tak, že každý záznam v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0e00-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e0e00-227">[Předchozí](creating-model-classes-with-linq-to-sql-cs.md)
[další](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e0e00-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
