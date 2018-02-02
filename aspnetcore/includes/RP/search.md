# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="62157-101">Přidání hledání do aplikace pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="62157-101">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="62157-102">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="62157-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="62157-103">V tomto dokumentu je přidána vyhledávací funkci na indexovou stránku, která umožňuje vyhledávání filmy podle *genre* nebo *název*.</span><span class="sxs-lookup"><span data-stu-id="62157-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="62157-104">Aktualizovat indexovou stránku `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="62157-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="62157-105">První řádek `OnGetAsync` metoda vytvoří [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) dotazu a vyberte filmy:</span><span class="sxs-lookup"><span data-stu-id="62157-105">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="62157-106">Dotaz je *pouze* definované v tomto okamžiku, má **není** byly spuštěny s databází.</span><span class="sxs-lookup"><span data-stu-id="62157-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="62157-107">Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na řetězec pro hledání:</span><span class="sxs-lookup"><span data-stu-id="62157-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="62157-108">`s => s.Title.Contains()` Kód [výrazu Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="62157-108">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="62157-109">Lambdas se používají v na základě metod [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metoda nebo `Contains` (používá se v předchozí kód).</span><span class="sxs-lookup"><span data-stu-id="62157-109">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="62157-110">Pokud tyto jste definovány nebo pokud se změnil voláním metody již nebudou provedeny dotazů LINQ (například `Where`, `Contains` nebo `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="62157-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="62157-111">Místo toho je odložen spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="62157-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="62157-112">To znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je vstupní přes nebo `ToListAsync` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="62157-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="62157-113">V tématu [provádění dotazu](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) Další informace.</span><span class="sxs-lookup"><span data-stu-id="62157-113">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="62157-114">**Poznámka:** [obsahuje](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda běží v databázi, není v kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="62157-114">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="62157-115">Rozlišování velkých a malých písmen na dotaz závisí na databázi a kolace.</span><span class="sxs-lookup"><span data-stu-id="62157-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="62157-116">Na serveru SQL Server `Contains` mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), což je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="62157-116">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="62157-117">V SQLite s výchozí kolace, je velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="62157-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="62157-118">Přejděte na stránku filmy a připojte řetězec dotazu, jako `?searchString=Ghost` na adresu URL (například `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="62157-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="62157-119">Filtrované filmy jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="62157-119">The filtered movies are displayed.</span></span>

![Zobrazení indexu](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="62157-121">Pokud na indexovou stránku se přidá následující šablonu trasy, řetězec pro hledání lze předat jako segment adresy URL (například `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="62157-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="62157-122">Předchozí omezení trasy umožňuje hledání názvu jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="62157-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="62157-123">`?` v `"{searchString?}"` znamená to je parametr volitelný trasy.</span><span class="sxs-lookup"><span data-stu-id="62157-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Index zobrazení s neodstraněných Wordu přidat adresu Url a vrácený film seznam dvou filmy, Ghostbusters a Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="62157-125">Nelze však budou uživatelé chcete upravit adresu URL pro vyhledání film.</span><span class="sxs-lookup"><span data-stu-id="62157-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="62157-126">V tomto kroku se přidá uživatelského rozhraní pro filtrování filmy.</span><span class="sxs-lookup"><span data-stu-id="62157-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="62157-127">Pokud jste přidali dané omezení trasy `"{searchString?}"`, odeberte ji.</span><span class="sxs-lookup"><span data-stu-id="62157-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="62157-128">Otevřete *Pages/Movies/Index.cshtml* souboru a přidejte `<form>` značek zvýrazněných v následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="62157-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="62157-129">HTML `<form>` značku používá [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="62157-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="62157-130">Při odeslání formuláře je odeslán řetězec filtru *stránkách nebo filmy nebo Index* stránky.</span><span class="sxs-lookup"><span data-stu-id="62157-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="62157-131">Uložte změny a testovat filtr.</span><span class="sxs-lookup"><span data-stu-id="62157-131">Save the changes and test the filter.</span></span>

![Index zobrazení s neodstraněných word zadali do textového pole Název filtru](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="62157-133">Hledat podle genre</span><span class="sxs-lookup"><span data-stu-id="62157-133">Search by genre</span></span>

<span data-ttu-id="62157-134">Přidat na následující zvýrazněnou vlastnosti, které chcete *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="62157-134">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="62157-135">`SelectList Genres` Obsahuje seznam žánry.</span><span class="sxs-lookup"><span data-stu-id="62157-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="62157-136">To umožňuje uživateli vybrat genre ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="62157-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="62157-137">`MovieGenre` Vlastnost obsahuje konkrétní genre vybere uživatele (například "západní").</span><span class="sxs-lookup"><span data-stu-id="62157-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="62157-138">Aktualizace `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="62157-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="62157-139">Následující kód je dotaz LINQ, který načte všechny žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="62157-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="62157-140">`SelectList` z žánry vytvoří projekce odlišné žánry.</span><span class="sxs-lookup"><span data-stu-id="62157-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="62157-141">Přidání vyhledávání podle genre</span><span class="sxs-lookup"><span data-stu-id="62157-141">Adding search by genre</span></span>

<span data-ttu-id="62157-142">Aktualizace *Index.cshtml* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="62157-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="62157-143">Testování aplikací tak, že genre, název filmu a oběma.</span><span class="sxs-lookup"><span data-stu-id="62157-143">Test the app by searching by genre, by movie title, and by both.</span></span>
