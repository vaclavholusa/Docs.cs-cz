---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Přístup k vašemu modelu datům z řadiče | Microsoft Docs
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873414"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="0ce70-104">Přístup k vašemu modelu datům z řadiče</span><span class="sxs-lookup"><span data-stu-id="0ce70-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="0ce70-105">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0ce70-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0ce70-106">Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0ce70-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0ce70-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="0ce70-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="0ce70-108">V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data pro film a zobrazí v prohlížeči pomocí šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0ce70-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="0ce70-109">**Sestavení aplikace** potom přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="0ce70-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="0ce70-110">Klikněte pravým tlačítkem myši *řadiče* složky a vytvořte novou `MoviesController` řadiče.</span><span class="sxs-lookup"><span data-stu-id="0ce70-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="0ce70-111">Níže uvedené možnosti nezobrazí, dokud sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ce70-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="0ce70-112">Vyberte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="0ce70-112">Select the following options:</span></span>

- <span data-ttu-id="0ce70-113">Název řadiče: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="0ce70-114">(Toto je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="0ce70-114">(This is the default.</span></span> <span data-ttu-id="0ce70-115">)</span><span class="sxs-lookup"><span data-stu-id="0ce70-115">)</span></span>
- <span data-ttu-id="0ce70-116">Šablona: **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s použitím rozhraní Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="0ce70-117">Třída modelu: **Movie (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="0ce70-118">Třída kontextu dat: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="0ce70-119">Zobrazení: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="0ce70-120">(Výchozí nastavení.)</span><span class="sxs-lookup"><span data-stu-id="0ce70-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="0ce70-122">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-122">Click **Add**.</span></span> <span data-ttu-id="0ce70-123">Visual Studio Express vytvoří následující soubory a složky:</span><span class="sxs-lookup"><span data-stu-id="0ce70-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="0ce70-124">*MoviesController.cs* souboru v projektu *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="0ce70-125">A *filmy* složky v projektu *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="0ce70-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* v novém *Views\Movies* složky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="0ce70-127">Automaticky vytvoří CRUD ASP.NET MVC 4 (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení pro vás (Automatická tvorba metody akce CRUD a zobrazení se označuje jako generování uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="0ce70-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="0ce70-128">Nyní máte plně funkční webovou aplikaci, která umožňuje vytvářet, seznam, upravit a odstranit film položky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="0ce70-129">Spusťte aplikaci a přejděte do `Movies` řadiče připojením */Movies* na adresu URL na panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0ce70-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="0ce70-130">Vzhledem k tomu, že se aplikace spoléhá na výchozí směrování (definované v *Global.asax* soubor), požadavek prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí `Index` metody akce `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="0ce70-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="0ce70-131">Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je efektivně stejný jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="0ce70-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="0ce70-132">Výsledkem je prázdný seznam filmy, protože zatím jste nepřidali žádné.</span><span class="sxs-lookup"><span data-stu-id="0ce70-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="0ce70-133">Vytváření film</span><span class="sxs-lookup"><span data-stu-id="0ce70-133">Creating a Movie</span></span>

<span data-ttu-id="0ce70-134">Vyberte **vytvořit nový** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0ce70-134">Select the **Create New** link.</span></span> <span data-ttu-id="0ce70-135">Zadejte některé podrobnosti o film a poté klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0ce70-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="0ce70-136">Kliknutím **vytvořit** tlačítko způsobí, že formulář odeslat na server, kde film informace se ukládají v databázi.</span><span class="sxs-lookup"><span data-stu-id="0ce70-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="0ce70-137">Potom budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video ve výpisu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="0ce70-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="0ce70-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="0ce70-139">Vytvořte několik další film položky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-139">Create a couple more movie entries.</span></span> <span data-ttu-id="0ce70-140">Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.</span><span class="sxs-lookup"><span data-stu-id="0ce70-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="0ce70-141">Zkoumání generovaný kód</span><span class="sxs-lookup"><span data-stu-id="0ce70-141">Examining the Generated Code</span></span>

<span data-ttu-id="0ce70-142">Otevřete *Controllers\MoviesController.cs* soubor a zkontrolujte vygenerovaného `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ce70-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="0ce70-143">Část řadičem film s `Index` metoda jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="0ce70-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="0ce70-144">Následující řádek z `MoviesController` třída vytvoří kontext databáze film, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="0ce70-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="0ce70-145">Dotaz, upravovat a odstraňovat filmy, můžete použít film kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="0ce70-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="0ce70-146">Požadavek na `Movies` vrátí všechny položky v `Movies` tabulky databáze film a pak předá výsledky do `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0ce70-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="0ce70-147">Silného typu modely a @model – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="0ce70-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="0ce70-148">V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="0ce70-149">`ViewBag` Je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0ce70-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="0ce70-150">ASP.NET MVC obsahuje také možnost předat silně typované data nebo objekty, které chcete zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="0ce70-151">Tento přístup umožňuje lepší kompilaci Kontrola kódu a bohatší IntelliSense v editoru Visual Studio silného typu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="0ce70-152">Tento postup se používá mechanismus generování uživatelského rozhraní v sadě Visual Studio `MoviesController` třídy a zobrazení šablony při jeho vytvoření metody a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0ce70-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="0ce70-153">V *Controllers\MoviesController.cs* zkontrolujte vygenerovaného souboru `Details` metoda.</span><span class="sxs-lookup"><span data-stu-id="0ce70-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="0ce70-154">Část řadičem film s `Details` metoda jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="0ce70-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="0ce70-155">Pokud `Movie` nenajde, instanci `Movie` modelu je předán zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="0ce70-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="0ce70-156">Zkontrolujte obsah *Views\Movies\Details.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="0ce70-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="0ce70-157">Zahrnutím `@model` příkaz v horní části souboru šablony zobrazení, můžete zadat typ objektu, která očekává zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0ce70-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="0ce70-158">Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0ce70-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="0ce70-159">To `@model` – direktiva umožňuje přístup k video, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0ce70-160">Například v *Details.cshtml* šablony, kód předá pro každé pole film `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocné objekty HTML s silného typu `Model` objektu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="0ce70-161">Metody vytvoření a úprava a zobrazení šablony předat také film objektu modelu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="0ce70-162">Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metoda v *MoviesController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="0ce70-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="0ce70-163">Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` Pomocná metoda v `Index` metody akce.</span><span class="sxs-lookup"><span data-stu-id="0ce70-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="0ce70-164">Tento kód pak předá `Movies` seznamu z řadiče zobrazení:</span><span class="sxs-lookup"><span data-stu-id="0ce70-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="0ce70-165">Pokud jste vytvořili řadičem film, Visual Studio Express automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0ce70-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="0ce70-166">To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu.</span><span class="sxs-lookup"><span data-stu-id="0ce70-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0ce70-167">Například v *Index.cshtml* šablony, kód prochází filmy nástrojem `foreach` příkaz přes silného typu `Model` objektu:</span><span class="sxs-lookup"><span data-stu-id="0ce70-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="0ce70-168">Protože `Model` je silného typu objektu (jako `IEnumerable<Movie>` objektu), každý `item` objekt ve smyčce je zadán jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0ce70-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="0ce70-169">Mezi další výhody to znamená, že získat kontrola kompilace kódu a úplné podporu technologie IntelliSense v editoru kódu:</span><span class="sxs-lookup"><span data-stu-id="0ce70-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="0ce70-171">Práce s LocalDB serveru SQL</span><span class="sxs-lookup"><span data-stu-id="0ce70-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="0ce70-172">Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla poskytnuta ukazuje `Movies` databáze, který nebyl ještě neexistuje, Code First vytvoří databázi automaticky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="0ce70-173">Můžete ověřit, že je vytvořeno podle *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="0ce70-174">Pokud nevidíte *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a potom rozbalte *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="0ce70-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="0ce70-175">Klikněte dvakrát na *Movies.mdf* otevřete **PRŮZKUMNÍK databáze**, pak rozbalte **tabulky** složku pro najdete v tabulce filmy.</span><span class="sxs-lookup"><span data-stu-id="0ce70-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="0ce70-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="0ce70-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="0ce70-177">Pokud se nezobrazí Průzkumník databáze, z **nástroje** nabídce vyberte možnost **připojit k databázi**, zrušení **zvolit zdroj dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ce70-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="0ce70-178">Tato akce vynutí otevřete Průzkumník databáze.</span><span class="sxs-lookup"><span data-stu-id="0ce70-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="0ce70-179">Pokud používáte VWD nebo Visual Studio 2010 a dojde k chybě podobná následující následující:</span><span class="sxs-lookup"><span data-stu-id="0ce70-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="0ce70-180">Databáze se C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF, nelze otevřít, protože je verze 706.</span><span class="sxs-lookup"><span data-stu-id="0ce70-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="0ce70-181">Tento server podporuje verzi 655 a starší.</span><span class="sxs-lookup"><span data-stu-id="0ce70-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="0ce70-182">Přechod na starší verzi cesta není podporována.</span><span class="sxs-lookup"><span data-stu-id="0ce70-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="0ce70-183">&quot;Pomocí uživatelského kódu se neošetřená výjimka InvalidOperation&quot; v zadaném objektu SqlConnection není určený počáteční katalog.</span><span class="sxs-lookup"><span data-stu-id="0ce70-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="0ce70-184">Je potřeba nainstalovat [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) a [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="0ce70-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="0ce70-185">Ověřte `MovieDBContext` připojovacího řetězce zadaného na předchozí stránce.</span><span class="sxs-lookup"><span data-stu-id="0ce70-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="0ce70-186">Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **zobrazit Data tabulky** chcete zobrazit data, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0ce70-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="0ce70-187">Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **otevřete definici tabulky** zobrazíte v tabulce struktury této Entity Framework Code First automaticky vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="0ce70-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="0ce70-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="0ce70-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="0ce70-189">Všimněte si jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0ce70-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="0ce70-190">Entity Framework Code First automaticky vytvoří tento schématu na základě vaší `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="0ce70-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="0ce70-191">Až budete hotovi, zavřete připojení kliknutím pravým tlačítkem *MovieDBContext* a výběrem **zavřít připojení**.</span><span class="sxs-lookup"><span data-stu-id="0ce70-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="0ce70-192">(Pokud nemáte ukončení připojení, může dojde k chybě při příštím spuštění projektu).</span><span class="sxs-lookup"><span data-stu-id="0ce70-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="0ce70-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="0ce70-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="0ce70-194">Nyní máte databázi a jednoduchý výpis stránku, kterou chcete zobrazit obsah z něj.</span><span class="sxs-lookup"><span data-stu-id="0ce70-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="0ce70-195">V dalším kurzu jsme budete zkontrolujte zbytek automaticky generovaný kód a přidejte `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat filmy v této databázi.</span><span class="sxs-lookup"><span data-stu-id="0ce70-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ce70-196">[Předchozí](adding-a-model.md)
> [další](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="0ce70-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
