---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Přidání modelu (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 797a4b26960881c9df60a47389a0f979b00e45cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371597"
---
<a name="adding-a-model-c"></a><span data-ttu-id="e0b23-104">Přidání modelu (C#)</span><span class="sxs-lookup"><span data-stu-id="e0b23-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="e0b23-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e0b23-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="e0b23-106">V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0b23-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="e0b23-107">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="e0b23-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="e0b23-108">Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="e0b23-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="e0b23-109">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="e0b23-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="e0b23-110">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="e0b23-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="e0b23-111">ASP.NET MVC 3 nástroje Update</span><span class="sxs-lookup"><span data-stu-id="e0b23-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="e0b23-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)</span><span class="sxs-lookup"><span data-stu-id="e0b23-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="e0b23-113">Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="e0b23-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="e0b23-114">Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="e0b23-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="e0b23-115">[Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="e0b23-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="e0b23-116">Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/adding-a-model.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e0b23-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="e0b23-117">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="e0b23-117">Adding a Model</span></span>

<span data-ttu-id="e0b23-118">V této části přidáte některé třídy pro správu filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0b23-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="e0b23-119">Tyto třídy bude "model" část aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e0b23-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="e0b23-120">Přístup k datům technologie rozhraní .NET Framework označované jako Entity Framework budete používat k definování a pracovat s tyto třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="e0b23-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="e0b23-121">Entity Framework (často označované jako EF) podporuje vývoj paradigma volá *Code First*.</span><span class="sxs-lookup"><span data-stu-id="e0b23-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="e0b23-122">Kód nejprve umožňuje vytvořit objekty modelu napsáním jednoduché třídy.</span><span class="sxs-lookup"><span data-stu-id="e0b23-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="e0b23-123">(Tyto jsou také známé jako POCO tříd, z "prostý staré objekty CLR.") Potom můžete mít databázi vytvořené v reálném čase ze třídy, která umožňuje pracovní postupy s velmi čistá a rychlý vývoj.</span><span class="sxs-lookup"><span data-stu-id="e0b23-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="e0b23-124">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="e0b23-124">Adding Model Classes</span></span>

<span data-ttu-id="e0b23-125">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a pak vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="e0b23-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="e0b23-126">Název *třídy* "Video".</span><span class="sxs-lookup"><span data-stu-id="e0b23-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="e0b23-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="e0b23-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="e0b23-128">Následujících pět vlastnosti, které chcete přidat `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="e0b23-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="e0b23-129">Použijeme `Movie` pro reprezentaci filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0b23-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="e0b23-130">Každá instance `Movie` objektu bude odpovídat řádek do tabulky databáze a každou vlastnost `Movie` třídy bude mapovat na sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e0b23-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="e0b23-131">Ve stejném souboru přidejte následující `MovieDBContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="e0b23-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="e0b23-132">`MovieDBContext` Třída reprezentuje kontext databáze filmů Entity Framework, která zpracovává načítání, ukládání a aktualizaci `Movie` třídy instancí v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0b23-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="e0b23-133">`MovieDBContext` Je odvozen od `DbContext` základní třída poskytované rozhraním Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0b23-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="e0b23-134">Další informace o `DbContext` a `DbSet`, naleznete v tématu [vylepšení produktivity pro Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="e0b23-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="e0b23-135">Pokud chcete mít možnost odkazovat `DbContext` a `DbSet`, budete muset přidat následující `using` příkazu v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="e0b23-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="e0b23-136">Kompletní *Movie.cs* souboru je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="e0b23-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="e0b23-137">Vytvoření připojovacího řetězce a práce s SQL serverem Compact</span><span class="sxs-lookup"><span data-stu-id="e0b23-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="e0b23-138">`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0b23-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e0b23-139">Jednu otázku, kterou můžete pokládat, je, jak určit databázi, kterou se připojí k.</span><span class="sxs-lookup"><span data-stu-id="e0b23-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="e0b23-140">Můžete to udělat tak, že přidáte informace o připojení v *Web.config* souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0b23-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="e0b23-141">Otevřete kořenový adresář aplikace *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="e0b23-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="e0b23-142">(Ne *Web.config* ve *zobrazení* složky.) Na následujícím obrázku zobrazit obojí *Web.config* soubory; otevřít *Web.config* soubor v kruhu červeně.</span><span class="sxs-lookup"><span data-stu-id="e0b23-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="e0b23-143">Přidejte následující připojovací řetězec do `<connectionStrings>` prvek *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="e0b23-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="e0b23-144">Následující příklad ukazuje část *Web.config* soubor se přidat nový připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="e0b23-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="e0b23-145">Malé množství kódu a XML je všechno, co potřebujete k zápisu představují a ukládat data o filmech v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0b23-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="e0b23-146">V dalším kroku vytvoříte novou `MoviesController` třídu, která vám pomůže se zobrazí data o filmech a umožní uživatelům vytvářet nové video výpisy.</span><span class="sxs-lookup"><span data-stu-id="e0b23-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0b23-147">[Předchozí](adding-a-view.md)
> [další](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="e0b23-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
