---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Přístup k datům modelu z Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817548"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="7685b-102">Přístup k datům modelu z Kontroleru</span><span class="sxs-lookup"><span data-stu-id="7685b-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="7685b-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7685b-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="7685b-104">V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data o filmech a zobrazí v prohlížeči pomocí zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="7685b-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="7685b-105">**Sestavení aplikace** před přechodem k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="7685b-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="7685b-106">Pokud není sestavení aplikace, získáte k chybě při přidávání kontroleru.</span><span class="sxs-lookup"><span data-stu-id="7685b-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="7685b-107">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složku a potom klikněte na **přidat**, pak **řadič**.</span><span class="sxs-lookup"><span data-stu-id="7685b-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="7685b-108">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 se zobrazeními, používá nástroj Entity Framework**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7685b-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="7685b-109">Vyberte **Movie (MvcMovie.Models)** pro třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="7685b-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="7685b-110">Vyberte **MovieDBContext (MvcMovie.Models)** pro třídy datového kontextu.</span><span class="sxs-lookup"><span data-stu-id="7685b-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="7685b-111">Zadejte název řadiče **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="7685b-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="7685b-112">Následující obrázek ukazuje dokončený dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7685b-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="7685b-113">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7685b-113">Click **Add**.</span></span> <span data-ttu-id="7685b-114">(Pokud dojde k chybě, pravděpodobně nebyla sestavení aplikace před zahájením přidat kontroler.) Visual Studio vytvoří následující soubory a složky:</span><span class="sxs-lookup"><span data-stu-id="7685b-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="7685b-115">*MoviesController.cs* soubor *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="7685b-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="7685b-116">A *Views\Movies* složky.</span><span class="sxs-lookup"><span data-stu-id="7685b-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="7685b-117">*Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* na novém *Views\Movies* složky.</span><span class="sxs-lookup"><span data-stu-id="7685b-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="7685b-118">Visual Studio automaticky vytvoří [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvoření, čtení, aktualizace a odstranění) metody akce a zobrazení pro vás (automatické vytváření metody akcí CRUD a zobrazení se označuje jako generování uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="7685b-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="7685b-119">Teď máte plně funkční webovou aplikaci, která umožňuje vytvořit, seznam, upravovat a odstraňovat položky video.</span><span class="sxs-lookup"><span data-stu-id="7685b-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="7685b-120">Spusťte aplikaci a klikněte na **MVC Movie** odkaz (nebo vyhledejte `Movies` řadič přidáním */Movies* na adresu URL do adresního řádku prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="7685b-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="7685b-121">Protože aplikace se spoléhá na výchozí směrování (definované v *aplikace\_Start\RouteConfig.cs* souboru), žádost prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí hodnotu `Index` metody akce `Movies` kontroleru.</span><span class="sxs-lookup"><span data-stu-id="7685b-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="7685b-122">Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je v podstatě totéž jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="7685b-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="7685b-123">Výsledkem je prázdný seznam filmy, protože ještě jste nepřidali žádné.</span><span class="sxs-lookup"><span data-stu-id="7685b-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="7685b-124">Vytváření video</span><span class="sxs-lookup"><span data-stu-id="7685b-124">Creating a Movie</span></span>

<span data-ttu-id="7685b-125">Vyberte **vytvořit nový** odkaz.</span><span class="sxs-lookup"><span data-stu-id="7685b-125">Select the **Create New** link.</span></span> <span data-ttu-id="7685b-126">Zadejte podrobnosti o videa a poté klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7685b-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="7685b-127">Nebudete moci zadejte desetinné čárky nebo čárkami v poli pro cenu.</span><span class="sxs-lookup"><span data-stu-id="7685b-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="7685b-128">pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárku (&quot;,&quot;) pro desetinné čárky a USA retweetovat neanglické formáty kalendářního data, musíte zahrnout *globalize.js* a konkrétní  *cultures/Globalize.cultures.js* souboru (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) a JavaScript, který chcete použít `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="7685b-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="7685b-129">Ukážeme si jak to provést v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="7685b-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="7685b-130">Teď zadejte celá čísla, jako je 10.</span><span class="sxs-lookup"><span data-stu-id="7685b-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="7685b-131">Kliknutím **vytvořit** tlačítko způsobí, že formulář, který se má publikovat na server, kde je video informace uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="7685b-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="7685b-132">Pak budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video v seznamu.</span><span class="sxs-lookup"><span data-stu-id="7685b-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="7685b-133">Vytvořte několik další položky video.</span><span class="sxs-lookup"><span data-stu-id="7685b-133">Create a couple more movie entries.</span></span> <span data-ttu-id="7685b-134">Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.</span><span class="sxs-lookup"><span data-stu-id="7685b-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="7685b-135">Zkoumání generovaného kódu</span><span class="sxs-lookup"><span data-stu-id="7685b-135">Examining the Generated Code</span></span>

<span data-ttu-id="7685b-136">Otevřít *Controllers\MoviesController.cs* souboru a prozkoumání generované `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="7685b-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="7685b-137">Část kontroler filmů s `Index` metoda je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="7685b-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="7685b-138">Požadavek na `Movies` kontroler vrací všechny položky v `Movies` tabulky a potom předává výsledky `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7685b-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="7685b-139">Následující řádek ze `MoviesController` třídy vytvoří kontext databáze filmů, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="7685b-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="7685b-140">Kontext databáze filmů slouží k dotazování, upravovat a odstraňovat videa.</span><span class="sxs-lookup"><span data-stu-id="7685b-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="7685b-141">Modely se silnými typy a @model – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="7685b-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="7685b-142">Dříve v tomto kurzu jste viděli, jak kontroleru můžete předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu.</span><span class="sxs-lookup"><span data-stu-id="7685b-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="7685b-143">`ViewBag` Je dynamický objekt, který představuje pohodlný způsob s pozdní vazbou k předávání informací do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7685b-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="7685b-144">MVC nabízí také možnost předat *důrazně* objektů zobrazit šablonu typem.</span><span class="sxs-lookup"><span data-stu-id="7685b-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="7685b-145">Tento přístup silného typu umožňuje lepší kompilace kontroly kódu a bohatší [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) v editoru sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7685b-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="7685b-146">Tento přístup používá mechanismus generování uživatelského rozhraní v sadě Visual Studio (to znamená, předávání *důrazně* zadaný model) s `MoviesController` šablony třídy a zobrazení při vytvoření rovnou metody a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7685b-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="7685b-147">V *Controllers\MoviesController.cs* zkontrolujte vygenerovaný soubor `Details` metody.</span><span class="sxs-lookup"><span data-stu-id="7685b-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="7685b-148">`Details` Metoda je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="7685b-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="7685b-149">`id` Parametr je obvykle předán jako data trasy, například `http://localhost:1234/movies/details/1` film kontroler, akci, která se nastaví kontroler `details` a `id` na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="7685b-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="7685b-150">Můžete také předat ID s řetězcem dotazu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7685b-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="7685b-151">Pokud `Movie` nachází instance `Movie` modelu je předán `Details` zobrazení:</span><span class="sxs-lookup"><span data-stu-id="7685b-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="7685b-152">Zkontrolovat obsah *Views\Movies\Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="7685b-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="7685b-153">Zahrnutím `@model` příkazu v horní části souboru šablony zobrazení, můžete určit typ objektu, který očekává, že zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7685b-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="7685b-154">Při vytvoření kontroleru video Visual Studio automaticky zahrnuty následující `@model` příkazu v horní části *Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="7685b-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="7685b-155">To `@model` – direktiva umožňuje přístup k video, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno.</span><span class="sxs-lookup"><span data-stu-id="7685b-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7685b-156">Například v *Details.cshtml* šablony, že kód předá jednotlivých polí filmů a `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocných rutin HTML se silnými typy `Model` objektu.</span><span class="sxs-lookup"><span data-stu-id="7685b-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="7685b-157">`Create` a `Edit` metody a zobrazení šablony také předat objekt modelu videa.</span><span class="sxs-lookup"><span data-stu-id="7685b-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="7685b-158">Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metodu *MoviesController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="7685b-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="7685b-159">Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` pomocnou metodu v `Index` metody akce.</span><span class="sxs-lookup"><span data-stu-id="7685b-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="7685b-160">Tento kód pak předá `Movies` ze seznamu `Index` metody akce k zobrazení:</span><span class="sxs-lookup"><span data-stu-id="7685b-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="7685b-161">Při vytvoření kontroleru video Visual Studio automaticky zahrnuty následující `@model` příkazu v horní části *Index.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="7685b-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="7685b-162">To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno.</span><span class="sxs-lookup"><span data-stu-id="7685b-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7685b-163">Například v *Index.cshtml* šablony, kód prochází videa prováděním `foreach` příkaz přes silného typu `Model` objektu:</span><span class="sxs-lookup"><span data-stu-id="7685b-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="7685b-164">Protože `Model` objektu je silně typováno (jako `IEnumerable<Movie>` objektu), každý `item` objektu ve smyčce je zadán jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7685b-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="7685b-165">Kromě dalších výhod to znamená získání kompilace Kontrola kódu a plnou podporu technologie IntelliSense v editoru kódu:</span><span class="sxs-lookup"><span data-stu-id="7685b-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="7685b-167">Práce s verzí SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="7685b-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="7685b-168">Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla k dispozici na který je odkazováno `Movies` databázi, která tenkrát neexistovaly, takže Code First vytvořit databáze automaticky.</span><span class="sxs-lookup"><span data-stu-id="7685b-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="7685b-169">Můžete ověřit, že se vytvořil zobrazením *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="7685b-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="7685b-170">Pokud se nezobrazí *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítko **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a pak rozbalte *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="7685b-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="7685b-171">Dvakrát klikněte na panel *Movies.mdf* otevřete **PRŮZKUMNÍKA serveru**, potom rozbalte **tabulky** složky najdete v tabulce filmy.</span><span class="sxs-lookup"><span data-stu-id="7685b-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="7685b-172">Všimněte si, na ikonu klíče vedle ID.</span><span class="sxs-lookup"><span data-stu-id="7685b-172">Note the key icon next to ID.</span></span> <span data-ttu-id="7685b-173">Ve výchozím nastavení EF způsobí, že vlastnost s názvem ID primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="7685b-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="7685b-174">Další informace o EF a MVC najdete v kurzu vynikající Dykstra Petr na [MVC a EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7685b-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="7685b-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="7685b-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="7685b-176">Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **zobrazit Data tabulky** k zobrazení dat, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7685b-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="7685b-177">Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **Otevřít definici tabulky** zobrazíte tabulky struktury této Entity Framework Code First pro vás vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7685b-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="7685b-178">Všimněte si, že jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="7685b-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="7685b-179">Entity Framework Code First automaticky vytvoří toto schéma na základě vašich `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="7685b-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="7685b-180">Jakmile budete hotovi, ukončete připojení kliknutím pravým tlačítkem *MovieDBContext* a vyberete **zavřít připojení**.</span><span class="sxs-lookup"><span data-stu-id="7685b-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="7685b-181">(Pokud není ukončení připojení, pravděpodobně dojde k chybě při příštím spuštění projektu).</span><span class="sxs-lookup"><span data-stu-id="7685b-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="7685b-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="7685b-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="7685b-183">Teď máte databázi a stránkami zobrazit, upravit, aktualizovat a odstraňovat data.</span><span class="sxs-lookup"><span data-stu-id="7685b-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="7685b-184">V dalším kurzu vytvoříme zkontrolujte zbytek automaticky generovaný kód a přidat `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat videa v této databázi.</span><span class="sxs-lookup"><span data-stu-id="7685b-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="7685b-185">Další informace o použití rozhraní Entity Framework s MVC najdete v tématu [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7685b-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7685b-186">[Předchozí](creating-a-connection-string.md)
> [další](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="7685b-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
