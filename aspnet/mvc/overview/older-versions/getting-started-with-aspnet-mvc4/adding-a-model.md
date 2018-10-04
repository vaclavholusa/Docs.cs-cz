---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Přidání modelu | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 5308d2d1d11f954db8a4502adb42223f69e0c675
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578500"
---
<a name="adding-a-model"></a><span data-ttu-id="5531e-104">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="5531e-104">Adding a Model</span></span>
====================
<span data-ttu-id="5531e-105">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="5531e-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="5531e-106">Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5531e-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="5531e-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="5531e-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="5531e-108">V této části přidáte některé třídy pro správu filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="5531e-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="5531e-109">Tyto třídy bude &quot;modelu&quot; součástí aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5531e-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="5531e-110">Budete používat technologii .NET Framework přístup k datům označované jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) definovat a využívat tyto třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="5531e-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="5531e-111">Entity Framework (často označované jako EF) podporuje vývoj paradigma volá *Code First*.</span><span class="sxs-lookup"><span data-stu-id="5531e-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="5531e-112">Kód nejprve umožňuje vytvořit objekty modelu napsáním jednoduché třídy.</span><span class="sxs-lookup"><span data-stu-id="5531e-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="5531e-113">(Jedná se také označuje jako POCO třídy, z &quot;prostý staré objekty CLR.&quot;) Potom můžete mít databázi vytvořené v reálném čase ze třídy, která umožňuje pracovní postupy s velmi čistá a rychlý vývoj.</span><span class="sxs-lookup"><span data-stu-id="5531e-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="5531e-114">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="5531e-114">Adding Model Classes</span></span>

<span data-ttu-id="5531e-115">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a pak vyberte **třídy**.</span><span class="sxs-lookup"><span data-stu-id="5531e-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="5531e-116">Zadejte *třídy* název &quot;filmu&quot;.</span><span class="sxs-lookup"><span data-stu-id="5531e-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="5531e-117">Následujících pět vlastnosti, které chcete přidat `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="5531e-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="5531e-118">Použijeme `Movie` pro reprezentaci filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="5531e-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="5531e-119">Každá instance `Movie` objektu bude odpovídat řádek do tabulky databáze a každou vlastnost `Movie` třídy bude mapovat na sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="5531e-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="5531e-120">Ve stejném souboru přidejte následující `MovieDBContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="5531e-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="5531e-121">`MovieDBContext` Třída reprezentuje kontext databáze filmů Entity Framework, která zpracovává načítání, ukládání a aktualizaci `Movie` třídy instancí v databázi.</span><span class="sxs-lookup"><span data-stu-id="5531e-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="5531e-122">`MovieDBContext` Je odvozen od `DbContext` základní třída poskytované rozhraním Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5531e-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="5531e-123">Pokud chcete mít možnost odkazovat `DbContext` a `DbSet`, budete muset přidat následující `using` příkazu v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="5531e-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="5531e-124">Kompletní *Movie.cs* souboru je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="5531e-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="5531e-125">(Několik pomocí příkazů, které nejsou potřebné byly odebrány.)</span><span class="sxs-lookup"><span data-stu-id="5531e-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="5531e-126">Vytvoření připojovacího řetězce a práce s verzí SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="5531e-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="5531e-127">`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty se záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="5531e-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="5531e-128">Jednu otázku, kterou můžete pokládat, je, jak určit databázi, kterou se připojí k.</span><span class="sxs-lookup"><span data-stu-id="5531e-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="5531e-129">Můžete to udělat tak, že přidáte informace o připojení v *Web.config* souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="5531e-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="5531e-130">Otevřete kořenový adresář aplikace *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="5531e-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="5531e-131">(Ne *Web.config* ve *zobrazení* složky.) Otevřít *Web.config* souboru červeně.</span><span class="sxs-lookup"><span data-stu-id="5531e-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="5531e-132">Přidejte následující připojovací řetězec do `<connectionStrings>` prvek *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="5531e-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="5531e-133">Následující příklad ukazuje část *Web.config* soubor se přidat nový připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="5531e-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="5531e-134">Malé množství kódu a XML je všechno, co potřebujete k zápisu představují a ukládat data o filmech v databázi.</span><span class="sxs-lookup"><span data-stu-id="5531e-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="5531e-135">V dalším kroku vytvoříte novou `MoviesController` třídu, která vám pomůže se zobrazí data o filmech a umožní uživatelům vytvářet nové video výpisy.</span><span class="sxs-lookup"><span data-stu-id="5531e-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5531e-136">[Předchozí](adding-a-view.md)
> [další](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="5531e-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
