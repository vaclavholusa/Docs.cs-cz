---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Přidání modelu (C#) | Microsoft Docs
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-c"></a><span data-ttu-id="36526-104">Přidání modelu (C#)</span><span class="sxs-lookup"><span data-stu-id="36526-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="36526-105">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="36526-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="36526-106">V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36526-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="36526-107">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="36526-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="36526-108">Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="36526-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="36526-109">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="36526-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="36526-110">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="36526-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="36526-111">Aktualizace nástrojů rozhraní ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="36526-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="36526-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)</span><span class="sxs-lookup"><span data-stu-id="36526-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="36526-113">Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="36526-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="36526-114">Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="36526-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="36526-115">[Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="36526-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="36526-116">Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/adding-a-model.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="36526-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="36526-117">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="36526-117">Adding a Model</span></span>

<span data-ttu-id="36526-118">V této části přidáte některé třídy pro správu filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="36526-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="36526-119">Tyto třídy bude část "model" aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="36526-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="36526-120">Technologie pro přístup k datům rozhraní .NET Framework označuje jako rozhraní Entity Framework budete používat k definování a pracovat s tyto třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="36526-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="36526-121">Podporuje rozhraní Entity Framework (často označované jako EF) vývoj zlepší názvem *Code First*.</span><span class="sxs-lookup"><span data-stu-id="36526-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="36526-122">Kód nejprve vám umožní vytvořit objekty modelu vytvořením jednoduchého třídy.</span><span class="sxs-lookup"><span data-stu-id="36526-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="36526-123">(Jedná se také označují jako tříd objektů POCO, z "prostý starý objekty CLR".) Pak můžete mít databázi vytvořené za chodu ze třídy, která umožňuje velmi vyčištění a rychlý vývoj pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="36526-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="36526-124">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="36526-124">Adding Model Classes</span></span>

<span data-ttu-id="36526-125">V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="36526-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="36526-126">Název *třída* "Film".</span><span class="sxs-lookup"><span data-stu-id="36526-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="36526-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="36526-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="36526-128">Následujících pět vlastnosti, které chcete přidat `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="36526-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="36526-129">Použijeme `Movie` třída představující filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="36526-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="36526-130">Každá instance `Movie` objektu bude odpovídat na řádek v tabulce databáze a každou vlastnost `Movie` třída bude mapovat na sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="36526-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="36526-131">Do stejného souboru přidejte následující `MovieDBContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="36526-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="36526-132">`MovieDBContext` Třída reprezentuje kontext Entity Framework film databáze, která zpracovává načítání, ukládání a aktualizace `Movie` třídy instance v databázi.</span><span class="sxs-lookup"><span data-stu-id="36526-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="36526-133">`MovieDBContext` Je odvozena z `DbContext` základní třída poskytuje rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="36526-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="36526-134">Další informace o `DbContext` a `DbSet`, najdete v části [produktivitu vylepšení rozhraní Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="36526-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="36526-135">Aby bylo možné odkazovat `DbContext` a `DbSet`, je nutné přidat následující `using` příkaz v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="36526-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="36526-136">Kompletní *Movie.cs* souboru je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="36526-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="36526-137">Vytvoření připojovacího řetězce a práci s SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="36526-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="36526-138">`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="36526-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="36526-139">Jeden otázku může, ale je uveden postup určit se připojí k databázi.</span><span class="sxs-lookup"><span data-stu-id="36526-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="36526-140">Můžete to udělat tak, že přidáte informace o připojení v *Web.config* souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="36526-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="36526-141">Otevřete kořenový adresář aplikace *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="36526-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="36526-142">(Není *Web.config* v soubor *zobrazení* složky.) Následující obrázek zobrazit i *Web.config* soubory; otevřete *Web.config* souboru v kroužku červeně.</span><span class="sxs-lookup"><span data-stu-id="36526-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="36526-143">Přidejte následující připojovací řetězec k `<connectionStrings>` element v *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="36526-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="36526-144">Následující příklad ukazuje část *Web.config* soubor s přidat nový připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="36526-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="36526-145">Malé množství kódu a XML je všechno, co potřebujete k zápisu, aby bylo možné představují a ukládat data film v databázi.</span><span class="sxs-lookup"><span data-stu-id="36526-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="36526-146">Dále budete vytvářet nové `MoviesController` třídu, která můžete použít k zobrazení dat film a povolit uživatelům vytvářet nové výpisy film.</span><span class="sxs-lookup"><span data-stu-id="36526-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36526-147">[Předchozí](adding-a-view.md)
> [další](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="36526-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
