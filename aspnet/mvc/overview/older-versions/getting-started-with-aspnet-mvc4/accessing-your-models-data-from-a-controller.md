---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Přístup k datům modelu z Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf1d27088c1e65d55a6820825eebe63f7fdcb515
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804934"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="821ca-104">Přístup k datům modelu z Kontroleru</span><span class="sxs-lookup"><span data-stu-id="821ca-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="821ca-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="821ca-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="821ca-106">Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="821ca-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="821ca-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="821ca-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="821ca-108">V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data o filmech a zobrazí v prohlížeči pomocí zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="821ca-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="821ca-109">**Sestavení aplikace** před přechodem k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="821ca-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="821ca-110">Klikněte pravým tlačítkem myši *řadiče* složky a vytvořte nový `MoviesController` kontroleru.</span><span class="sxs-lookup"><span data-stu-id="821ca-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="821ca-111">Níže uvedené možnosti nezobrazí, dokud sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="821ca-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="821ca-112">Vyberte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="821ca-112">Select the following options:</span></span>

- <span data-ttu-id="821ca-113">Název kontroleru: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="821ca-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="821ca-114">(Toto je výchozí.</span><span class="sxs-lookup"><span data-stu-id="821ca-114">(This is the default.</span></span> <span data-ttu-id="821ca-115">)</span><span class="sxs-lookup"><span data-stu-id="821ca-115">)</span></span>
- <span data-ttu-id="821ca-116">Šablona: **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s použitím Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="821ca-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="821ca-117">Třída modelu: **Movie (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="821ca-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="821ca-118">Třída kontextu dat: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="821ca-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="821ca-119">Zobrazení: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="821ca-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="821ca-120">(Výchozí nastavení.)</span><span class="sxs-lookup"><span data-stu-id="821ca-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="821ca-122">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="821ca-122">Click **Add**.</span></span> <span data-ttu-id="821ca-123">Visual Studio Express vytvoří následující soubory a složky:</span><span class="sxs-lookup"><span data-stu-id="821ca-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="821ca-124">*MoviesController.cs* souboru v projektu *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="821ca-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="821ca-125">A *filmy* složky v projektu *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="821ca-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="821ca-126">*Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* na novém *Views\Movies* složky.</span><span class="sxs-lookup"><span data-stu-id="821ca-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="821ca-127">ASP.NET MVC 4 automaticky vytvoří CRUD (vytváření, čtení, aktualizace a odstranění) metody akce a zobrazení pro vás (automatické vytváření metody akcí CRUD a zobrazení se označuje jako generování uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="821ca-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="821ca-128">Teď máte plně funkční webovou aplikaci, která umožňuje vytvořit, seznam, upravovat a odstraňovat položky video.</span><span class="sxs-lookup"><span data-stu-id="821ca-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="821ca-129">Spusťte aplikaci a přejděte `Movies` řadič přidáním */Movies* na adresu URL do adresního řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="821ca-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="821ca-130">Protože aplikace se spoléhá na výchozí směrování (definované v *Global.asax* souboru), žádost prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí hodnotu `Index` metody akce `Movies` kontroleru.</span><span class="sxs-lookup"><span data-stu-id="821ca-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="821ca-131">Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je v podstatě totéž jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="821ca-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="821ca-132">Výsledkem je prázdný seznam filmy, protože ještě jste nepřidali žádné.</span><span class="sxs-lookup"><span data-stu-id="821ca-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="821ca-133">Vytváření video</span><span class="sxs-lookup"><span data-stu-id="821ca-133">Creating a Movie</span></span>

<span data-ttu-id="821ca-134">Vyberte **vytvořit nový** odkaz.</span><span class="sxs-lookup"><span data-stu-id="821ca-134">Select the **Create New** link.</span></span> <span data-ttu-id="821ca-135">Zadejte podrobnosti o videa a poté klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="821ca-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="821ca-136">Kliknutím **vytvořit** tlačítko způsobí, že formulář, který se má publikovat na server, kde je video informace uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="821ca-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="821ca-137">Pak budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video v seznamu.</span><span class="sxs-lookup"><span data-stu-id="821ca-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="821ca-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="821ca-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="821ca-139">Vytvořte několik další položky video.</span><span class="sxs-lookup"><span data-stu-id="821ca-139">Create a couple more movie entries.</span></span> <span data-ttu-id="821ca-140">Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.</span><span class="sxs-lookup"><span data-stu-id="821ca-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="821ca-141">Zkoumání generovaného kódu</span><span class="sxs-lookup"><span data-stu-id="821ca-141">Examining the Generated Code</span></span>

<span data-ttu-id="821ca-142">Otevřít *Controllers\MoviesController.cs* souboru a prozkoumání generované `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="821ca-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="821ca-143">Část kontroler filmů s `Index` metoda je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="821ca-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="821ca-144">Následující řádek ze `MoviesController` třídy vytvoří kontext databáze filmů, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="821ca-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="821ca-145">Kontext databáze filmů slouží k dotazování, upravovat a odstraňovat videa.</span><span class="sxs-lookup"><span data-stu-id="821ca-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="821ca-146">Požadavek na `Movies` kontroler vrací všechny položky v `Movies` tabulky movie databáze a potom předává výsledky `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="821ca-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="821ca-147">Modely se silnými typy a @model – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="821ca-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="821ca-148">Dříve v tomto kurzu jste viděli, jak kontroleru můžete předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu.</span><span class="sxs-lookup"><span data-stu-id="821ca-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="821ca-149">`ViewBag` Je dynamický objekt, který představuje pohodlný způsob s pozdní vazbou k předávání informací do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="821ca-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="821ca-150">ASP.NET MVC rovněž poskytuje možnost předávání silně typované dat nebo objekty, které chcete zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="821ca-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="821ca-151">Tento přístup umožňuje lepší kompilace Kontrola kódu a propracovanější IntelliSense v editoru sady Visual Studio silného typu.</span><span class="sxs-lookup"><span data-stu-id="821ca-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="821ca-152">Tento přístup se používá mechanismus generování uživatelského rozhraní v sadě Visual Studio `MoviesController` šablony třídy a zobrazení při vytvoření rovnou metody a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="821ca-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="821ca-153">V *Controllers\MoviesController.cs* zkontrolujte vygenerovaný soubor `Details` metody.</span><span class="sxs-lookup"><span data-stu-id="821ca-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="821ca-154">Část kontroler filmů s `Details` metoda je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="821ca-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="821ca-155">Pokud `Movie` nachází instance `Movie` modelu je předán zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="821ca-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="821ca-156">Zkontrolovat obsah *Views\Movies\Details.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="821ca-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="821ca-157">Zahrnutím `@model` příkazu v horní části souboru šablony zobrazení, můžete určit typ objektu, který očekává, že zobrazení.</span><span class="sxs-lookup"><span data-stu-id="821ca-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="821ca-158">Při vytvoření kontroleru video Visual Studio automaticky zahrnuty následující `@model` příkazu v horní části *Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="821ca-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="821ca-159">To `@model` – direktiva umožňuje přístup k video, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno.</span><span class="sxs-lookup"><span data-stu-id="821ca-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="821ca-160">Například v *Details.cshtml* šablony, že kód předá jednotlivých polí filmů a `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocných rutin HTML se silnými typy `Model` objektu.</span><span class="sxs-lookup"><span data-stu-id="821ca-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="821ca-161">Metody vytvoření a úprava a zobrazení šablony také předat objekt modelu videa.</span><span class="sxs-lookup"><span data-stu-id="821ca-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="821ca-162">Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metodu *MoviesController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="821ca-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="821ca-163">Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` pomocnou metodu v `Index` metody akce.</span><span class="sxs-lookup"><span data-stu-id="821ca-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="821ca-164">Tento kód pak předá `Movies` seznamu z kontroleru zobrazení:</span><span class="sxs-lookup"><span data-stu-id="821ca-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="821ca-165">Při vytvoření kontroleru video Visual Studio Express automaticky zahrnuty následující `@model` příkazu v horní části *Index.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="821ca-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="821ca-166">To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno.</span><span class="sxs-lookup"><span data-stu-id="821ca-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="821ca-167">Například v *Index.cshtml* šablony, kód prochází videa prováděním `foreach` příkaz přes silného typu `Model` objektu:</span><span class="sxs-lookup"><span data-stu-id="821ca-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="821ca-168">Protože `Model` objektu je silně typováno (jako `IEnumerable<Movie>` objektu), každý `item` objektu ve smyčce je zadán jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="821ca-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="821ca-169">Kromě dalších výhod to znamená získání kompilace Kontrola kódu a plnou podporu technologie IntelliSense v editoru kódu:</span><span class="sxs-lookup"><span data-stu-id="821ca-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="821ca-171">Práce s verzí SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="821ca-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="821ca-172">Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla k dispozici na který je odkazováno `Movies` databázi, která tenkrát neexistovaly, takže Code First vytvořit databáze automaticky.</span><span class="sxs-lookup"><span data-stu-id="821ca-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="821ca-173">Můžete ověřit, že se vytvořil zobrazením *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="821ca-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="821ca-174">Pokud se nezobrazí *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítko **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a pak rozbalte *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="821ca-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="821ca-175">Dvakrát klikněte na panel *Movies.mdf* otevřete **PRŮZKUMNÍK databáze**, potom rozbalte **tabulky** složky najdete v tabulce videa.</span><span class="sxs-lookup"><span data-stu-id="821ca-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="821ca-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="821ca-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="821ca-177">Pokud se nezobrazí Průzkumník databáze, z **nástroje** nabídce vyberte možnost **připojit k databázi**, pak zrušit **zvolit zdroj dat** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="821ca-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="821ca-178">Tato akce vynutí otevřít Průzkumník databáze.</span><span class="sxs-lookup"><span data-stu-id="821ca-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="821ca-179">Pokud používáte VWD nebo Visual Studio 2010 a objevit chyba podobná ke kterékoli z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="821ca-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="821ca-180">Databáze ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' nelze otevřít, protože je verze 706.</span><span class="sxs-lookup"><span data-stu-id="821ca-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="821ca-181">Tento server podporuje verzi 655 a starší.</span><span class="sxs-lookup"><span data-stu-id="821ca-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="821ca-182">Downgradu není podporován.</span><span class="sxs-lookup"><span data-stu-id="821ca-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="821ca-183">&quot;InvalidOperation výjimka je ošetřena uživatelským kódem&quot; v poskytnutém objektu SqlConnection není určen počáteční katalog.</span><span class="sxs-lookup"><span data-stu-id="821ca-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="821ca-184">Je potřeba nainstalovat [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) a [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="821ca-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="821ca-185">Ověřte, `MovieDBContext` připojovací řetězec zadanou na předchozí stránce.</span><span class="sxs-lookup"><span data-stu-id="821ca-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="821ca-186">Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **zobrazit Data tabulky** k zobrazení dat, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="821ca-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="821ca-187">Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **Otevřít definici tabulky** zobrazíte tabulky struktury této Entity Framework Code First pro vás vytvořili.</span><span class="sxs-lookup"><span data-stu-id="821ca-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="821ca-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="821ca-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="821ca-189">Všimněte si, že jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="821ca-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="821ca-190">Entity Framework Code First automaticky vytvoří toto schéma na základě vašich `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="821ca-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="821ca-191">Jakmile budete hotovi, ukončete připojení kliknutím pravým tlačítkem *MovieDBContext* a vyberete **zavřít připojení**.</span><span class="sxs-lookup"><span data-stu-id="821ca-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="821ca-192">(Pokud není ukončení připojení, pravděpodobně dojde k chybě při příštím spuštění projektu).</span><span class="sxs-lookup"><span data-stu-id="821ca-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="821ca-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="821ca-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="821ca-194">Teď máte databázi a jednoduchý seznam stránku k zobrazení obsahu z něj.</span><span class="sxs-lookup"><span data-stu-id="821ca-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="821ca-195">V dalším kurzu vytvoříme zkontrolujte zbytek automaticky generovaný kód a přidat `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat videa v této databázi.</span><span class="sxs-lookup"><span data-stu-id="821ca-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="821ca-196">[Předchozí](adding-a-model.md)
> [další](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="821ca-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
