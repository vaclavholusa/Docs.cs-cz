---
title: Přidání vyhledávání do ASP.NET Core Razor Pages
author: rick-anderson
description: Ukazuje, jak přidat vyhledávací technologie ASP.NET Core Razor Pages
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: c88441b39d8c96ec817c58fc56ebd51a0887b077
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045559"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="37191-103">Přidání vyhledávání do ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="37191-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="37191-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37191-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="37191-105">V tomto dokumentu, funkce vyhledávání je přidána na indexovou stránku, která umožňuje vyhledávání filmy podle *žánr* nebo *název*.</span><span class="sxs-lookup"><span data-stu-id="37191-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="37191-106">Aktualizovat indexovou stránku `OnGetAsync` metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="37191-106">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="37191-107">První řádek `OnGetAsync` metoda vytvoří [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) dotaz pro výběr videa:</span><span class="sxs-lookup"><span data-stu-id="37191-107">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="37191-108">Dotaz je *pouze* definované v tomto okamžiku, má **není** byly spuštěny v rámci databáze.</span><span class="sxs-lookup"><span data-stu-id="37191-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="37191-109">Pokud `searchString` parametr obsahuje řetězec, dotaz filmy je upravit tak, aby filtrování hledaný řetězec:</span><span class="sxs-lookup"><span data-stu-id="37191-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="37191-110">`s => s.Title.Contains()` Kód je [výraz Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="37191-110">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="37191-111">Výrazy lambda se používají v založených na volání metody [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) dotazuje jako argumenty pro standardní metody operátoru dotazu, jako [kde](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metoda nebo `Contains` (používá se v předchozím kódu).</span><span class="sxs-lookup"><span data-stu-id="37191-111">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="37191-112">Dotazy LINQ nejsou provedeny, když máte definovány, nebo data jejich voláním metody (například `Where`, `Contains` nebo `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="37191-112">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="37191-113">Místo toho provádění dotazu je odloženo.</span><span class="sxs-lookup"><span data-stu-id="37191-113">Rather, query execution is deferred.</span></span> <span data-ttu-id="37191-114">To znamená, že vyhodnocení výrazu je zpožděna, dokud není procházen jeho očekávané hodnoty nebo `ToListAsync` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="37191-114">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="37191-115">Zobrazit [provádění dotazu](/dotnet/framework/data/adonet/ef/language-reference/query-execution) Další informace.</span><span class="sxs-lookup"><span data-stu-id="37191-115">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="37191-116">**Poznámka:** [obsahuje](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metodu spustíte v databázi, není v kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="37191-116">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="37191-117">Rozlišování velikosti písmen u dotazu, závisí na databázi a kolace.</span><span class="sxs-lookup"><span data-stu-id="37191-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="37191-118">Na serveru SQL Server `Contains` mapuje na [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), což je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="37191-118">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="37191-119">V SQLite s výchozí kolace je velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="37191-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="37191-120">Přejděte na stránku filmy a připojte řetězec dotazu jako `?searchString=Ghost` na adresu URL (například `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="37191-120">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="37191-121">Zobrazují se filtrované filmy.</span><span class="sxs-lookup"><span data-stu-id="37191-121">The filtered movies are displayed.</span></span>

![Index zobrazení](search/_static/ghost.png)

<span data-ttu-id="37191-123">Pokud na indexovou stránku se přidá následující šablonu trasy, vyhledávací řetězec lze předat jako segment adresy URL (například `http://localhost:5000/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="37191-123">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="37191-124">Předcházející omezení trasy umožňuje hledání názvu jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="37191-124">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="37191-125">`?` v `"{searchString?}"` znamená, že toto je parametr trasy nepovinný.</span><span class="sxs-lookup"><span data-stu-id="37191-125">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Index zobrazení na mapách slovo ghost, přidá do adresy Url a vrácené film seznam dvou filmy, Ghostbusters a Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="37191-127">Ale nemůžete očekávat, že uživatelům změnit adresu URL pro hledání videa.</span><span class="sxs-lookup"><span data-stu-id="37191-127">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="37191-128">V tomto kroku se přidá uživatelského rozhraní pro filtrování videa.</span><span class="sxs-lookup"><span data-stu-id="37191-128">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="37191-129">Pokud jste přidali dané omezení trasy `"{searchString?}"`, odeberte ji.</span><span class="sxs-lookup"><span data-stu-id="37191-129">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="37191-130">Otevřít *Pages/Movies/Index.cshtml* a přidejte `<form>` značky v následujícím kódu zvýrazněno:</span><span class="sxs-lookup"><span data-stu-id="37191-130">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="37191-131">Kód HTML `<form>` označení používá [pomocné rutiny značky formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="37191-131">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="37191-132">Když se odešle formulář, řetězec filtru posílá *stránek/filmy/Index* stránky.</span><span class="sxs-lookup"><span data-stu-id="37191-132">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="37191-133">Uložte změny a filtr otestovat.</span><span class="sxs-lookup"><span data-stu-id="37191-133">Save the changes and test the filter.</span></span>

![Index zobrazení na mapách slovo ghost, zadaný do textového pole Název filtru](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="37191-135">Hledat podle žánru</span><span class="sxs-lookup"><span data-stu-id="37191-135">Search by genre</span></span>

<span data-ttu-id="37191-136">Přidejte následující zvýrazněný vlastnosti do *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="37191-136">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end


<span data-ttu-id="37191-137">`Genres` Vlastnost obsahuje seznam žánrů.</span><span class="sxs-lookup"><span data-stu-id="37191-137">The `Genres` property contains the list of genres.</span></span> <span data-ttu-id="37191-138">To umožňuje uživateli vybrat rozšířením podle tematických ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="37191-138">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="37191-139">`MovieGenre` Vlastnost obsahuje konkrétní žánr vybere uživatele (například "západní").</span><span class="sxs-lookup"><span data-stu-id="37191-139">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="37191-140">Aktualizace `OnGetAsync` metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="37191-140">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="37191-141">Následující kód je dotaz LINQ, který načte všechny žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="37191-141">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="37191-142">`SelectList` Žánrů se vytvořila projekci odlišné žánrů.</span><span class="sxs-lookup"><span data-stu-id="37191-142">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="37191-143">Přidání vyhledávání podle žánru</span><span class="sxs-lookup"><span data-stu-id="37191-143">Adding search by genre</span></span>

<span data-ttu-id="37191-144">Aktualizace *Index.cshtml* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="37191-144">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="37191-145">Otestujte aplikaci tak, že žánr, název filmu a obě.</span><span class="sxs-lookup"><span data-stu-id="37191-145">Test the app by searching by genre, by movie title, and by both.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37191-146">[Předchozí: Aktualizace stránek](xref:tutorials/razor-pages/da1)
> [Další: Přidání nového pole](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="37191-146">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
