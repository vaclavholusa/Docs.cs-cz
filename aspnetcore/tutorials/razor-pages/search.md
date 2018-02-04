---
title: "Přidání vyhledávání na stránky Razor jádro ASP.NET"
author: rick-anderson
description: "Ukazuje, jak přidat vyhledávání na stránky ASP.NET Core Razor"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/search
ms.openlocfilehash: 440635219bae666e968c14280a3cace4596aa973
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="6dc8a-103">Přidání hledání do aplikace pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="6dc8a-103">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="6dc8a-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6dc8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6dc8a-105">V tomto dokumentu je přidána vyhledávací funkci na indexovou stránku, která umožňuje vyhledávání filmy podle *genre* nebo *název*.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="6dc8a-106">Aktualizovat indexovou stránku `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-106">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="6dc8a-107">První řádek `OnGetAsync` metoda vytvoří [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) dotazu a vyberte filmy:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-107">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="6dc8a-108">Dotaz je *pouze* definované v tomto okamžiku, má **není** byly spuštěny s databází.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="6dc8a-109">Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na řetězec pro hledání:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="6dc8a-110">`s => s.Title.Contains()` Kód [výrazu Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="6dc8a-110">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="6dc8a-111">Lambdas se používají v na základě metod [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metoda nebo `Contains` (používá se v předchozí kód).</span><span class="sxs-lookup"><span data-stu-id="6dc8a-111">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="6dc8a-112">Pokud tyto jste definovány nebo pokud se změnil voláním metody již nebudou provedeny dotazů LINQ (například `Where`, `Contains` nebo `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="6dc8a-112">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="6dc8a-113">Místo toho je odložen spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-113">Rather, query execution is deferred.</span></span> <span data-ttu-id="6dc8a-114">To znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je vstupní přes nebo `ToListAsync` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-114">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="6dc8a-115">V tématu [provádění dotazu](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-115">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="6dc8a-116">**Poznámka:** [obsahuje](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda běží v databázi, není v kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-116">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="6dc8a-117">Rozlišování velkých a malých písmen na dotaz závisí na databázi a kolace.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="6dc8a-118">Na serveru SQL Server `Contains` mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), což je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-118">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="6dc8a-119">V SQLite s výchozí kolace, je velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="6dc8a-120">Přejděte na stránku filmy a připojte řetězec dotazu, jako `?searchString=Ghost` na adresu URL (například `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="6dc8a-120">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="6dc8a-121">Filtrované filmy jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-121">The filtered movies are displayed.</span></span>

![Zobrazení indexu](search/_static/ghost.png)

<span data-ttu-id="6dc8a-123">Pokud na indexovou stránku se přidá následující šablonu trasy, řetězec pro hledání lze předat jako segment adresy URL (například `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="6dc8a-123">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="6dc8a-124">Předchozí omezení trasy umožňuje hledání názvu jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-124">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="6dc8a-125">`?` v `"{searchString?}"` znamená to je parametr volitelný trasy.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-125">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Index zobrazení s neodstraněných Wordu přidat adresu Url a vrácený film seznam dvou filmy, Ghostbusters a Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="6dc8a-127">Nelze však budou uživatelé chcete upravit adresu URL pro vyhledání film.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-127">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="6dc8a-128">V tomto kroku se přidá uživatelského rozhraní pro filtrování filmy.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-128">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="6dc8a-129">Pokud jste přidali dané omezení trasy `"{searchString?}"`, odeberte ji.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-129">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="6dc8a-130">Otevřete *Pages/Movies/Index.cshtml* souboru a přidejte `<form>` značek zvýrazněných v následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-130">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="6dc8a-131">HTML `<form>` značku používá [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="6dc8a-131">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="6dc8a-132">Při odeslání formuláře je odeslán řetězec filtru *stránkách nebo filmy nebo Index* stránky.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-132">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="6dc8a-133">Uložte změny a testovat filtr.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-133">Save the changes and test the filter.</span></span>

![Index zobrazení s neodstraněných word zadali do textového pole Název filtru](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="6dc8a-135">Hledat podle genre</span><span class="sxs-lookup"><span data-stu-id="6dc8a-135">Search by genre</span></span>

<span data-ttu-id="6dc8a-136">Přidat na následující zvýrazněnou vlastnosti, které chcete *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-136">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="6dc8a-137">`SelectList Genres` Obsahuje seznam žánry.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-137">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="6dc8a-138">To umožňuje uživateli vybrat genre ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-138">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="6dc8a-139">`MovieGenre` Vlastnost obsahuje konkrétní genre vybere uživatele (například "západní").</span><span class="sxs-lookup"><span data-stu-id="6dc8a-139">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="6dc8a-140">Aktualizace `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-140">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="6dc8a-141">Následující kód je dotaz LINQ, který načte všechny žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-141">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="6dc8a-142">`SelectList` z žánry vytvoří projekce odlišné žánry.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-142">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="6dc8a-143">Přidání vyhledávání podle genre</span><span class="sxs-lookup"><span data-stu-id="6dc8a-143">Adding search by genre</span></span>

<span data-ttu-id="6dc8a-144">Aktualizace *Index.cshtml* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6dc8a-144">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="6dc8a-145">Testování aplikací tak, že genre, název filmu a oběma.</span><span class="sxs-lookup"><span data-stu-id="6dc8a-145">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6dc8a-146">[Předchozí: Aktualizace stránky](xref:tutorials/razor-pages/da1)
[Další: Přidání nové pole](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="6dc8a-146">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
