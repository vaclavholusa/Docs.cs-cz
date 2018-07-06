---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Přidání modelu (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 138d21d5e33384fbc0dd99c33a9e43a95976ed06
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827803"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="72ef5-103">Přidání modelu (VB)</span><span class="sxs-lookup"><span data-stu-id="72ef5-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="72ef5-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="72ef5-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="72ef5-105">V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="72ef5-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="72ef5-106">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="72ef5-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="72ef5-107">Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="72ef5-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="72ef5-108">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="72ef5-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="72ef5-109">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="72ef5-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="72ef5-110">ASP.NET MVC 3 nástroje Update</span><span class="sxs-lookup"><span data-stu-id="72ef5-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="72ef5-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)</span><span class="sxs-lookup"><span data-stu-id="72ef5-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="72ef5-112">Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="72ef5-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="72ef5-113">Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="72ef5-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="72ef5-114">[Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="72ef5-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="72ef5-115">Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/adding-a-model.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="72ef5-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="72ef5-116">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="72ef5-116">Adding a Model</span></span>

<span data-ttu-id="72ef5-117">V této části přidáte některé třídy pro správu filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="72ef5-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="72ef5-118">Tyto třídy bude "model" část aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72ef5-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="72ef5-119">Přístup k datům technologie rozhraní .NET Framework označované jako Entity Framework budete používat k definování a pracovat s tyto třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="72ef5-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="72ef5-120">Entity Framework (často označované jako EF) podporuje vývoj paradigma volá *Code First*.</span><span class="sxs-lookup"><span data-stu-id="72ef5-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="72ef5-121">Kód nejprve umožňuje vytvořit objekty modelu napsáním jednoduché třídy.</span><span class="sxs-lookup"><span data-stu-id="72ef5-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="72ef5-122">(Tyto jsou také známé jako POCO tříd, z "prostý staré objekty CLR.") Potom můžete mít databázi vytvořené v reálném čase ze třídy, která umožňuje pracovní postupy s velmi čistá a rychlý vývoj.</span><span class="sxs-lookup"><span data-stu-id="72ef5-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="72ef5-123">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="72ef5-123">Adding Model Classes</span></span>

<span data-ttu-id="72ef5-124">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a pak vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="72ef5-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="72ef5-125">Název třídy "Video".</span><span class="sxs-lookup"><span data-stu-id="72ef5-125">Name the class "Movie".</span></span>

<span data-ttu-id="72ef5-126">Následujících pět vlastnosti, které chcete přidat `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="72ef5-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="72ef5-127">Použijeme `Movie` pro reprezentaci filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="72ef5-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="72ef5-128">Každá instance `Movie` objektu bude odpovídat řádek do tabulky databáze a každou vlastnost `Movie` třídy bude mapovat na sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="72ef5-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="72ef5-129">Ve stejném souboru přidejte následující `MovieDBContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="72ef5-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="72ef5-130">`MovieDBContext` Třída reprezentuje kontext databáze filmů Entity Framework, která zpracovává načítání, ukládání a aktualizaci `Movie` třídy instancí v databázi.</span><span class="sxs-lookup"><span data-stu-id="72ef5-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="72ef5-131">`MovieDBContext` Je odvozen od `DbContext` základní třída poskytované rozhraním Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="72ef5-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="72ef5-132">Další informace o `DbContext` a `DbSet`, naleznete v tématu [vylepšení produktivity pro Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="72ef5-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="72ef5-133">Pokud chcete mít možnost odkazovat `DbContext` a `DbSet`, budete muset přidat následující `imports` příkazu v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="72ef5-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="72ef5-134">Kompletní *Movie.vb* souboru je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="72ef5-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="72ef5-135">Vytvoření připojovacího řetězce a práce s SQL serverem Compact</span><span class="sxs-lookup"><span data-stu-id="72ef5-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="72ef5-136">`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="72ef5-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="72ef5-137">Jednu otázku, kterou můžete pokládat, je, jak určit databázi, kterou se připojí k.</span><span class="sxs-lookup"><span data-stu-id="72ef5-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="72ef5-138">Můžete to udělat tak, že přidáte informace o připojení v *Web.config* souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="72ef5-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="72ef5-139">Otevřete kořenový adresář aplikace *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="72ef5-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="72ef5-140">(Ne *Web.config* ve *zobrazení* složky.) Na následujícím obrázku zobrazit obojí *Web.config* soubory; otevřít *Web.config* soubor v kruhu červeně.</span><span class="sxs-lookup"><span data-stu-id="72ef5-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="72ef5-141">Přidejte následující připojovací řetězec do `<connectionStrings>` prvek *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="72ef5-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="72ef5-142">Následující příklad ukazuje část *Web.config* soubor se přidat nový připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="72ef5-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="72ef5-143">Malé množství kódu a XML je všechno, co potřebujete k zápisu představují a ukládat data o filmech v databázi.</span><span class="sxs-lookup"><span data-stu-id="72ef5-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="72ef5-144">V dalším kroku vytvoříte novou `MoviesController` třídu, která vám pomůže se zobrazí data o filmech a umožní uživatelům vytvářet nové video výpisy.</span><span class="sxs-lookup"><span data-stu-id="72ef5-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="72ef5-145">[Předchozí](adding-a-view.md)
> [další](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="72ef5-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
