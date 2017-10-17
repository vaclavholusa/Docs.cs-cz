# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Přidání hledání do jádra ASP.NET MVC aplikace

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části můžete přidat možnosti vyhledávání `Index` metody akce, která umožňuje vyhledávání filmy podle *genre* nebo *název*.

Aktualizace `Index` metoda následujícím kódem:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

První řádek `Index` metoda akce vytvoří [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) dotazu a vyberte filmy:

```csharp
var movies = from m in _context.Movie
             select m;
```

Dotaz je *pouze* definované v tomto okamžiku, má **není** byly spuštěny s databází.

Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na základě hodnoty řetězec pro hledání:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Je výše uvedený kód [výrazu Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambdas se používají v na základě metod [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://docs.microsoft.com//dotnet/api/system.linq.enumerable.where) metoda nebo `Contains` (používá se ve výše uvedeném kódu). Dotazy LINQ nebudou provedeny, když jsou definovány nebo když jsou upraveny voláním metody `Where`, `Contains` nebo `OrderBy`. Místo toho je odložen spuštění dotazu.  To znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je ve skutečnosti vstupní přes nebo `ToListAsync` metoda je volána. Další informace o provádění odložené dotazů najdete v tématu [provádění dotazu](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Poznámka: [obsahuje](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda běží v databázi, není ve výše uvedeném kódu c#. Rozlišování velkých a malých písmen na dotaz závisí na databázi a kolace. Na serveru SQL Server [obsahuje](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), což je malá a velká písmena. V SQLlite s výchozí kolace, je velká a malá písmena.

Přejděte na `/Movies/Index`. Připojit řetězec dotazu, jako `?searchString=Ghost` na adresu URL. Filtrované filmy jsou zobrazeny.

![Zobrazení indexu](../../tutorials/first-mvc-app/search/_static/ghost.png)

Pokud změníte podpis `Index` metoda tak, aby měl parametr s názvem `id`, `id` bude shodovat s nepovinný parametr `{id}` zástupný symbol pro výchozí směruje sada v *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
