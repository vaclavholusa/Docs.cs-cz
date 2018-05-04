---
title: Přidání vyhledávání na stránky ASP.NET Core Razor
author: rick-anderson
description: Ukazuje, jak přidat vyhledávání na stránky ASP.NET Core Razor
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/search
ms.openlocfilehash: b547b67b3e51562633ea06d3730145f49c6043ea
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>Přidání vyhledávání na stránky ASP.NET Core Razor

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V tomto dokumentu je přidána vyhledávací funkci na indexovou stránku, která umožňuje vyhledávání filmy podle *genre* nebo *název*.

Aktualizovat indexovou stránku `OnGetAsync` metoda následujícím kódem:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

První řádek `OnGetAsync` metoda vytvoří [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) dotazu a vyberte filmy:

```csharp
var movies = from m in _context.Movie
             select m;
```

Dotaz je *pouze* definované v tomto okamžiku, má **není** byly spuštěny s databází.

Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na řetězec pro hledání:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kód [výrazu Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambdas se používají v na základě metod [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metoda nebo `Contains` (používá se v předchozí kód). Pokud tyto jste definovány nebo pokud se změnil voláním metody již nebudou provedeny dotazů LINQ (například `Where`, `Contains` nebo `OrderBy`). Místo toho je odložen spuštění dotazu. To znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je vstupní přes nebo `ToListAsync` metoda je volána. V tématu [provádění dotazu](/dotnet/framework/data/adonet/ef/language-reference/query-execution) Další informace.

**Poznámka:** [obsahuje](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda běží v databázi, není v kódu jazyka C#. Rozlišování velkých a malých písmen na dotaz závisí na databázi a kolace. Na serveru SQL Server `Contains` mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), což je malá a velká písmena. V SQLite s výchozí kolace, je velká a malá písmena.

Přejděte na stránku filmy a připojte řetězec dotazu, jako `?searchString=Ghost` na adresu URL (například `http://localhost:5000/Movies?searchString=Ghost`). Filtrované filmy jsou zobrazeny.

![Zobrazení indexu](search/_static/ghost.png)

Pokud na indexovou stránku se přidá následující šablonu trasy, řetězec pro hledání lze předat jako segment adresy URL (například `http://localhost:5000/Movies/ghost`).

```cshtml
@page "{searchString?}"
```

Předchozí omezení trasy umožňuje hledání názvu jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.  `?` v `"{searchString?}"` znamená to je parametr volitelný trasy.

![Index zobrazení s neodstraněných Wordu přidat adresu Url a vrácený film seznam dvou filmy, Ghostbusters a Ghostbusters 2](search/_static/g2.png)

Nelze však budou uživatelé chcete upravit adresu URL pro vyhledání film. V tomto kroku se přidá uživatelského rozhraní pro filtrování filmy. Pokud jste přidali dané omezení trasy `"{searchString?}"`, odeberte ji.

Otevřete *Pages/Movies/Index.cshtml* souboru a přidejte `<form>` značek zvýrazněných v následujícím kódem:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` značku používá [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper). Při odeslání formuláře je odeslán řetězec filtru *stránkách nebo filmy nebo Index* stránky. Uložte změny a testovat filtr.

![Index zobrazení s neodstraněných word zadali do textového pole Název filtru](search/_static/filter.png)

## <a name="search-by-genre"></a>Hledat podle genre

Přidejte následující zvýrazněný vlastnosti pro *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

`SelectList Genres` Obsahuje seznam žánry. To umožňuje uživateli vybrat genre ze seznamu.

`MovieGenre` Vlastnost obsahuje konkrétní genre vybere uživatele (například "západní").

Aktualizace `OnGetAsync` metoda následujícím kódem:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Následující kód je dotaz LINQ, který načte všechny žánry z databáze.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` z žánry vytvoří projekce odlišné žánry.

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a>Přidání vyhledávání podle genre

Aktualizace *Index.cshtml* následujícím způsobem:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Testování aplikací tak, že genre, název filmu a oběma.

> [!div class="step-by-step"]
> [Předchozí: Aktualizace stránky](xref:tutorials/razor-pages/da1)
> [Další: Přidání nové pole](xref:tutorials/razor-pages/new-field)
