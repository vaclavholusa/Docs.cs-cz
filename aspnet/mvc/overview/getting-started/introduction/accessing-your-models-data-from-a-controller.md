---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: "Přístup k vašemu modelu datům z řadiče | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b60913cef4b62745cf167e6074834bf7d0c228d1
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/19/2017
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="0558e-102">Přístup k vašemu modelu datům z řadiče</span><span class="sxs-lookup"><span data-stu-id="0558e-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="0558e-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0558e-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="0558e-104">V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data pro film a zobrazí v prohlížeči pomocí šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0558e-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="0558e-105">**Sestavení aplikace** potom přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="0558e-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="0558e-106">Pokud nemáte sestavení aplikace, budete dojde k chybě přidáním řadiče.</span><span class="sxs-lookup"><span data-stu-id="0558e-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="0558e-107">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složku a pak klikněte na tlačítko **přidat**, pak **řadič**.</span><span class="sxs-lookup"><span data-stu-id="0558e-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="0558e-108">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0558e-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="0558e-109">Vyberte **Movie (MvcMovie.Models)** pro třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="0558e-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="0558e-110">Vyberte **MovieDBContext (MvcMovie.Models)** pro třída kontextu dat.</span><span class="sxs-lookup"><span data-stu-id="0558e-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="0558e-111">Pro daný název Kontroleru zadejte **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="0558e-111">For the Controller name enter **MoviesController**.</span></span>

 <span data-ttu-id="0558e-112">Následující obrázek ukazuje dialog dokončené.</span><span class="sxs-lookup"><span data-stu-id="0558e-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="0558e-113">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0558e-113">Click **Add**.</span></span> <span data-ttu-id="0558e-114">(Pokud dojde k chybě, budete pravděpodobně nebyla sestavení aplikace před zahájením přidání řadiče.) Visual Studio vytvoří následující soubory a složky:</span><span class="sxs-lookup"><span data-stu-id="0558e-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="0558e-115">*MoviesController.cs* v soubor *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="0558e-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="0558e-116">A *Views\Movies* složky.</span><span class="sxs-lookup"><span data-stu-id="0558e-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="0558e-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* v novém *Views\Movies* složky.</span><span class="sxs-lookup"><span data-stu-id="0558e-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="0558e-118">Visual Studio automaticky vytvoří [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení pro vás (Automatická tvorba metody akce CRUD a zobrazení se označuje jako generování uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="0558e-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="0558e-119">Nyní máte plně funkční webovou aplikaci, která umožňuje vytvářet, seznam, upravit a odstranit film položky.</span><span class="sxs-lookup"><span data-stu-id="0558e-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="0558e-120">Spusťte aplikaci a klikněte na **MVC film** odkaz (nebo vyhledejte `Movies` řadiče připojením */Movies* adresy URL v adresním řádku prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="0558e-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="0558e-121">Vzhledem k tomu, že se aplikace spoléhá na výchozí směrování (definované v *aplikace\_Start\RouteConfig.cs* soubor), požadavek prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí `Index` metody akce `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="0558e-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="0558e-122">Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je efektivně stejný jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="0558e-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="0558e-123">Výsledkem je prázdný seznam filmy, protože zatím jste nepřidali žádné.</span><span class="sxs-lookup"><span data-stu-id="0558e-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="0558e-124">Vytváření film</span><span class="sxs-lookup"><span data-stu-id="0558e-124">Creating a Movie</span></span>

<span data-ttu-id="0558e-125">Vyberte **vytvořit nový** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0558e-125">Select the **Create New** link.</span></span> <span data-ttu-id="0558e-126">Zadejte některé podrobnosti o film a poté klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0558e-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="0558e-127">Nelze zadat desetinných míst nebo čárkami pole cena.</span><span class="sxs-lookup"><span data-stu-id="0558e-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="0558e-128">Pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) pro desetinné čárky a formát data neanglických USA, musíte zahrnout *globalize.js* a konkrétní  *cultures/Globalize.cultures.js* souboru (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="0558e-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="0558e-129">I budete ukazují, jak to udělat v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="0558e-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="0558e-130">Teď zadejte jenom celá čísla, jako je 10.</span><span class="sxs-lookup"><span data-stu-id="0558e-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="0558e-131">Kliknutím **vytvořit** tlačítko způsobí, že formulář odeslat na server, kde film informace se ukládají v databázi.</span><span class="sxs-lookup"><span data-stu-id="0558e-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="0558e-132">Potom budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video ve výpisu.</span><span class="sxs-lookup"><span data-stu-id="0558e-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="0558e-133">Vytvořte několik další film položky.</span><span class="sxs-lookup"><span data-stu-id="0558e-133">Create a couple more movie entries.</span></span> <span data-ttu-id="0558e-134">Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.</span><span class="sxs-lookup"><span data-stu-id="0558e-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="0558e-135">Zkoumání generovaný kód</span><span class="sxs-lookup"><span data-stu-id="0558e-135">Examining the Generated Code</span></span>

<span data-ttu-id="0558e-136">Otevřete *Controllers\MoviesController.cs* soubor a zkontrolujte vygenerovaného `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="0558e-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="0558e-137">Část řadičem film s `Index` metoda jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="0558e-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="0558e-138">Požadavek na `Movies` vrátí všechny položky v `Movies` tabulce a pak předá výsledky do `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0558e-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="0558e-139">Následující řádek z `MoviesController` třída vytvoří kontext databáze film, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="0558e-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="0558e-140">Dotaz, upravovat a odstraňovat filmy, můžete použít film kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="0558e-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="0558e-141">Silného typu modely a @model – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="0558e-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="0558e-142">V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu.</span><span class="sxs-lookup"><span data-stu-id="0558e-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="0558e-143">`ViewBag` Je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0558e-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="0558e-144">MVC rovněž poskytuje možnost předat *důrazně* zadali objekty, které chcete zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="0558e-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="0558e-145">Tento přístup silného typu umožňuje lepší kompilaci vracení se změnami kódu a bohatší [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0558e-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="0558e-146">Mechanismus generování uživatelského rozhraní v sadě Visual Studio používá tento přístup (to znamená, předávání *důrazně* typu modelu) s `MoviesController` třídy a zobrazení šablony při jeho vytvoření metody a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0558e-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="0558e-147">V *Controllers\MoviesController.cs* zkontrolujte vygenerovaného souboru `Details` metoda.</span><span class="sxs-lookup"><span data-stu-id="0558e-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="0558e-148">`Details` Metoda jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="0558e-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="0558e-149">`id` Parametru se obecně předá jako data trasy, například `http://localhost:1234/movies/details/1` řadičem nastaví řadič film, akce, která `details` a `id` na 1.</span><span class="sxs-lookup"><span data-stu-id="0558e-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="0558e-150">Můžete také předat v id s řetězcem dotazu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0558e-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="0558e-151">Pokud `Movie` nenajde, instanci `Movie` model předán `Details` zobrazení:</span><span class="sxs-lookup"><span data-stu-id="0558e-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="0558e-152">Zkontrolujte obsah *Views\Movies\Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0558e-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="0558e-153">Zahrnutím `@model` příkaz v horní části souboru šablony zobrazení, můžete zadat typ objektu, která očekává zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0558e-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="0558e-154">Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0558e-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="0558e-155">To `@model` – direktiva umožňuje přístup k video, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu.</span><span class="sxs-lookup"><span data-stu-id="0558e-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0558e-156">Například v *Details.cshtml* šablony, kód předá pro každé pole film `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocné objekty HTML s silného typu `Model` objektu.</span><span class="sxs-lookup"><span data-stu-id="0558e-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="0558e-157">`Create` a `Edit` metody a zobrazit šablony předat také film objektu modelu.</span><span class="sxs-lookup"><span data-stu-id="0558e-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="0558e-158">Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metoda v *MoviesController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="0558e-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="0558e-159">Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) objektu při volání `View` Pomocná metoda v `Index` metody akce.</span><span class="sxs-lookup"><span data-stu-id="0558e-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="0558e-160">Tento kód pak předá `Movies` ze seznamu `Index` metody akce k zobrazení:</span><span class="sxs-lookup"><span data-stu-id="0558e-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="0558e-161">Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0558e-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="0558e-162">To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu.</span><span class="sxs-lookup"><span data-stu-id="0558e-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0558e-163">Například v *Index.cshtml* šablony, kód prochází filmy nástrojem `foreach` příkaz přes silného typu `Model` objektu:</span><span class="sxs-lookup"><span data-stu-id="0558e-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="0558e-164">Protože `Model` je silného typu objektu (jako `IEnumerable<Movie>` objektu), každý `item` objekt ve smyčce je zadán jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0558e-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="0558e-165">Mezi další výhody to znamená, že získat kontrola kompilace kódu a úplné podporu technologie IntelliSense v editoru kódu:</span><span class="sxs-lookup"><span data-stu-id="0558e-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="0558e-167">Práce s LocalDB serveru SQL</span><span class="sxs-lookup"><span data-stu-id="0558e-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="0558e-168">Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla poskytnuta ukazuje `Movies` databáze, který nebyl ještě neexistuje, Code First vytvoří databázi automaticky.</span><span class="sxs-lookup"><span data-stu-id="0558e-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="0558e-169">Můžete ověřit, že je vytvořeno podle *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="0558e-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="0558e-170">Pokud nevidíte *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a potom rozbalte *aplikace\_Data* složky.</span><span class="sxs-lookup"><span data-stu-id="0558e-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="0558e-171">Klikněte dvakrát na *Movies.mdf* otevřete **PRŮZKUMNÍKA serveru**, pak rozbalte **tabulky** složku pro najdete v tabulce filmy.</span><span class="sxs-lookup"><span data-stu-id="0558e-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="0558e-172">Poznámka: na ikonu klíče vedle ID.</span><span class="sxs-lookup"><span data-stu-id="0558e-172">Note the key icon next to ID.</span></span> <span data-ttu-id="0558e-173">Ve výchozím nastavení budou EF vlastnost s názvem ID primární klíč.</span><span class="sxs-lookup"><span data-stu-id="0558e-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="0558e-174">Další informace o EF a MVC naleznete v části kurzu vynikající tní Dykstra na [MVC a EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="0558e-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="0558e-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="0558e-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="0558e-176">Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **zobrazit Data tabulky** chcete zobrazit data, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0558e-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="0558e-177">Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **otevřete definici tabulky** zobrazíte v tabulce struktury této Entity Framework Code First automaticky vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="0558e-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="0558e-178">Všimněte si jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0558e-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="0558e-179">Entity Framework Code First automaticky vytvoří tento schématu na základě vaší `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="0558e-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="0558e-180">Až budete hotovi, zavřete připojení kliknutím pravým tlačítkem *MovieDBContext* a výběrem **zavřít připojení**.</span><span class="sxs-lookup"><span data-stu-id="0558e-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="0558e-181">(Pokud nemáte ukončení připojení, může dojde k chybě při příštím spuštění projektu).</span><span class="sxs-lookup"><span data-stu-id="0558e-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="0558e-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="0558e-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="0558e-183">Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data.</span><span class="sxs-lookup"><span data-stu-id="0558e-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="0558e-184">V dalším kurzu jsme budete zkontrolujte zbytek automaticky generovaný kód a přidejte `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat filmy v této databázi.</span><span class="sxs-lookup"><span data-stu-id="0558e-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="0558e-185">Další informace o používání rozhraní Entity Framework s MVC najdete v tématu [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="0558e-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0558e-186">[Předchozí](creating-a-connection-string.md)
[další](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="0558e-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
