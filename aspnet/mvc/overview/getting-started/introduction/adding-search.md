---
uid: mvc/overview/getting-started/introduction/adding-search
title: Hledání | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 8afa72d4dbc4695e7d26c6ef4052be08a7c69080
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="search"></a><span data-ttu-id="a738d-102">Hledat</span><span class="sxs-lookup"><span data-stu-id="a738d-102">Search</span></span>
====================
<span data-ttu-id="a738d-103">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a738d-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="a738d-104">Přidání metodu Search a zobrazení vyhledávání</span><span class="sxs-lookup"><span data-stu-id="a738d-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="a738d-105">V této části přidáte možnosti vyhledávání `Index` metody akce, která umožňuje vyhledávání filmy genre nebo název.</span><span class="sxs-lookup"><span data-stu-id="a738d-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="a738d-106">Aktualizace indexu formuláře</span><span class="sxs-lookup"><span data-stu-id="a738d-106">Updating the Index Form</span></span>

<span data-ttu-id="a738d-107">Začněte tím, že aktualizace `Index` metoda akce ke stávající `MoviesController` třídy.</span><span class="sxs-lookup"><span data-stu-id="a738d-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="a738d-108">Zde je kód:</span><span class="sxs-lookup"><span data-stu-id="a738d-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="a738d-109">První řádek `Index` metoda vytvoří následující [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazu a vyberte filmy:</span><span class="sxs-lookup"><span data-stu-id="a738d-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="a738d-110">Dotaz je definována v tomto okamžiku, ale dosud nebyla spuštěna v databázi.</span><span class="sxs-lookup"><span data-stu-id="a738d-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="a738d-111">Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na základě hodnoty řetězec pro hledání, pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="a738d-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="a738d-112">`s => s.Title` Je výše uvedený kód [výrazu Lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="a738d-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="a738d-113">Lambdas se používají v na základě metod [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metoda použitá ve výše uvedeném kódu.</span><span class="sxs-lookup"><span data-stu-id="a738d-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="a738d-114">Dotazy LINQ nebudou provedeny, když jsou definovány nebo když jsou upraveny voláním metody `Where` nebo `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="a738d-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="a738d-115">Místo toho při provádění dotazu je odložení, což znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je ve skutečnosti vstupní přes nebo [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="a738d-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="a738d-116">V `Search` ukázce dotaz se spouštějí v tom *Index.cshtml* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a738d-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="a738d-117">Další informace o provádění odložené dotazů najdete v tématu [provádění dotazu](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="a738d-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="a738d-118">[Obsahuje](https://msdn.microsoft.com/library/bb155125.aspx) metoda běží v databázi, není c# výše uvedený kód.</span><span class="sxs-lookup"><span data-stu-id="a738d-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="a738d-119">V databázi [obsahuje](https://msdn.microsoft.com/library/bb155125.aspx) mapuje [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), což je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="a738d-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="a738d-120">Teď můžete aktualizovat `Index` zobrazení, které se uživateli zobrazí formulář.</span><span class="sxs-lookup"><span data-stu-id="a738d-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="a738d-121">Spusťte aplikaci a přejděte do *nebo filmy nebo Index*.</span><span class="sxs-lookup"><span data-stu-id="a738d-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="a738d-122">Připojit řetězec dotazu, jako `?searchString=ghost` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a738d-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="a738d-123">Filtrované filmy jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="a738d-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="a738d-125">Pokud změníte podpis `Index` metoda tak, aby měl parametr s názvem `id`, `id` parametr bude odpovídat `{id}` zástupný symbol pro výchozí směruje sada v *aplikace\_Start\ Soubor RouteConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="a738d-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="a738d-126">Původní `Index` metoda vypadá takto::</span><span class="sxs-lookup"><span data-stu-id="a738d-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="a738d-127">Upravenou `Index` metoda vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a738d-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="a738d-128">Název hledání můžete nyní předat jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="a738d-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="a738d-129">Nelze však budou uživatelé chcete upravit adresu URL pokaždé, když chtějí hledat film.</span><span class="sxs-lookup"><span data-stu-id="a738d-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="a738d-130">Ano, teď můžete přidáte uživatelského rozhraní, můžete je filtrovat filmy.</span><span class="sxs-lookup"><span data-stu-id="a738d-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="a738d-131">Pokud jste změnili podpis `Index` metoda k testování jak předat parametr ID vázané na trasy ho změnit tak, aby vaše `Index` metoda přijímá řetězcový parametr s názvem `searchString`:</span><span class="sxs-lookup"><span data-stu-id="a738d-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="a738d-132">Otevřete *Views\Movies\Index.cshtml* souborů a právě po `@Html.ActionLink("Create New", "Create")`, přidejte značku formuláře zvýrazněná níže:</span><span class="sxs-lookup"><span data-stu-id="a738d-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="a738d-133">`Html.BeginForm` Pomocné rutiny vytvoří počáteční `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="a738d-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="a738d-134">`Html.BeginForm` Pomocná způsobí, že formulář post sám na sebe, když uživatel odešle formulář kliknutím **filtru** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a738d-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="a738d-135">Visual Studio 2013 má dobrý zlepšování při zobrazení a úpravy souborů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a738d-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="a738d-136">Při spuštění aplikace s soubor zobrazení otevřít, Visual Studio 2013 vyvolá metoda akce kontroleru správné zobrazíte zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a738d-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="a738d-137">Pomocí zobrazení indexu otevřete v sadě Visual Studio (jak je znázorněno na obrázku výše), klepněte na pev.cenu F5 nebo F5 spusťte aplikaci a pak zkuste vyhledat film.</span><span class="sxs-lookup"><span data-stu-id="a738d-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="a738d-138">Neexistuje žádné `HttpPost` přetížení z `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="a738d-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="a738d-139">Není nutné, protože metoda není změny stavu aplikace, právě filtrování dat.</span><span class="sxs-lookup"><span data-stu-id="a738d-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="a738d-140">Můžete přidat následující `HttpPost Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="a738d-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="a738d-141">V takovém případě by odpovídat původce volání akce `HttpPost Index` metody a `HttpPost Index` metoda by spustit jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="a738d-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="a738d-143">Ale i v případě, přidejte tuto `HttpPost` verzi `Index` metoda, existuje omezení v tom, jak to všechny byl implementován.</span><span class="sxs-lookup"><span data-stu-id="a738d-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="a738d-144">Představte si, že chcete bookmark konkrétní hledání nebo chcete poslat odkaz přátel, chcete-li zobrazit stejný filtrovaný seznam filmy mohou klepnutím.</span><span class="sxs-lookup"><span data-stu-id="a738d-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="a738d-145">Všimněte si, že adresa URL požadavku HTTP POST je stejný jako adresu URL pro požadavek GET (localhost:xxxxx nebo filmy nebo Index) – chybí informace o vyhledávání v adrese URL sám sebe.</span><span class="sxs-lookup"><span data-stu-id="a738d-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="a738d-146">Práva nyní informace řetězec hledání je odeslána na server jako hodnotu pole formuláře.</span><span class="sxs-lookup"><span data-stu-id="a738d-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="a738d-147">To znamená, že nelze zaznamenáte tyto informace vyhledávání bookmark nebo poslat přátel v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="a738d-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="a738d-148">Řešení, je použít přetížení `BeginForm` který určuje, že požadavek POST měli přidat informace o vyhledávání na adresu URL, a že by měl být směrované na `HttpGet` verzi `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="a738d-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="a738d-149">Nahradit existující bez parametrů `BeginForm` metoda s následující kód:</span><span class="sxs-lookup"><span data-stu-id="a738d-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="a738d-151">Teď při odesílání vyhledávání, adresa URL obsahuje řetězec dotazu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="a738d-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="a738d-152">Hledání budou také moct `HttpGet Index` metody akce, i když máte `HttpPost Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="a738d-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="a738d-154">Přidání vyhledávání podle Genre</span><span class="sxs-lookup"><span data-stu-id="a738d-154">Adding Search by Genre</span></span>

<span data-ttu-id="a738d-155">Pokud jste přidali `HttpPost` verzi `Index` metoda, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="a738d-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="a738d-156">V dalším kroku přidáte funkci tak, aby uživatelé vyhledejte filmy podle genre.</span><span class="sxs-lookup"><span data-stu-id="a738d-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="a738d-157">Nahraďte `Index` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a738d-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="a738d-158">Tato verze `Index` metoda přebírá další parametr, a to `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="a738d-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="a738d-159">Vytvoření první několika řádků kódu `List` objekt pro uložení žánry film z databáze.</span><span class="sxs-lookup"><span data-stu-id="a738d-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="a738d-160">Následující kód je dotaz LINQ, který načte všechny žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="a738d-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="a738d-161">Kód používá `AddRange` metoda obecná `List` kolekce odlišné žánry přidat do seznamu.</span><span class="sxs-lookup"><span data-stu-id="a738d-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="a738d-162">(Bez `Distinct` modifikátor, by byl přidán duplicitní žánry – například komedie by byl přidán dvakrát v naše ukázka).</span><span class="sxs-lookup"><span data-stu-id="a738d-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="a738d-163">Kód pak uloží seznam žánry v `ViewBag.MovieGenre` objektu.</span><span class="sxs-lookup"><span data-stu-id="a738d-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="a738d-164">Ukládání dat kategorie (takové film genre společnosti) jako [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) objekt v `ViewBag`, pak se typické postup pro aplikace MVC, přístup k datům kategorii v rozevírací pole se seznamem.</span><span class="sxs-lookup"><span data-stu-id="a738d-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="a738d-165">Následující kód ukazuje, jak zkontrolovat `movieGenre` parametr.</span><span class="sxs-lookup"><span data-stu-id="a738d-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="a738d-166">Pokud není prázdná, omezí kód další dotaz filmy omezit vybrané filmy k zadané genre.</span><span class="sxs-lookup"><span data-stu-id="a738d-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="a738d-167">Jak jsme uvedli dříve, dotaz se nespouští na databáze, dokud je vstupní seznamu video přes (který se stane v zobrazení po `Index` metoda akce vrací).</span><span class="sxs-lookup"><span data-stu-id="a738d-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="a738d-168">Přidání značek k zobrazení Index pro podporu vyhledávání podle Genre</span><span class="sxs-lookup"><span data-stu-id="a738d-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="a738d-169">Přidat `Html.DropDownList` pomocníka, který má *Views\Movies\Index.cshtml* souboru, těsně před `TextBox` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="a738d-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="a738d-170">Dokončený kód je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="a738d-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="a738d-171">V následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="a738d-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="a738d-172">Poskytuje klíč pro parametr "MovieGenre" `DropDownList` pomocná rutina pro vyhledání `IEnumerable<SelectListItem>` v `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="a738d-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="a738d-173">`ViewBag` Byly zadané v metodě akce:</span><span class="sxs-lookup"><span data-stu-id="a738d-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="a738d-174">Parametr "Vše" poskytuje popisku.</span><span class="sxs-lookup"><span data-stu-id="a738d-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="a738d-175">Pokud tuto volbu si prohlédnout v prohlížeči, uvidíte, že jeho atribut "value" je prázdný.</span><span class="sxs-lookup"><span data-stu-id="a738d-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="a738d-176">Vzhledem k tomu, že naše řadiče pouze filtry `if` řetězec není `null` nebo je prázdná, odesílání prázdnou hodnotu pro `movieGenre` ukazuje všechny žánry.</span><span class="sxs-lookup"><span data-stu-id="a738d-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="a738d-177">Můžete také nastavit možnost být vybrán ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a738d-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="a738d-178">Pokud byste chtěli "Komedie" jako výchozí možnost, chcete změnit kód v Kontroleru takto:</span><span class="sxs-lookup"><span data-stu-id="a738d-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="a738d-179">Spusťte aplikaci a přejděte do *nebo filmy nebo Index*.</span><span class="sxs-lookup"><span data-stu-id="a738d-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="a738d-180">Zkuste hledání genre, název film a obě kritéria.</span><span class="sxs-lookup"><span data-stu-id="a738d-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="a738d-181">V této části můžete vytvořit metody akce vyhledávání a zobrazení, která umožní uživatelům vyhledávat podle názvu film a genre.</span><span class="sxs-lookup"><span data-stu-id="a738d-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="a738d-182">V další části se budete podíváte na tom, jak přidat vlastnost, která má `Movie` modelu a postup přidání inicializátoru, který se automaticky vytvoří testovací databáze.</span><span class="sxs-lookup"><span data-stu-id="a738d-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a738d-183">[Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="a738d-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
