<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Teď při odesílání vyhledávání, adresa URL obsahuje řetězec dotazu. Hledání budou také moct `HttpGet Index` metody akce, i když máte `HttpPost Index` metoda.

![Okno prohlížeče zobrazující hledaný_řetězec = neodstraněných adresu Url a vrátí, filmy Ghostbusters a Ghostbusters 2, obsahovat neodstraněných aplikace word](../../tutorials/first-mvc-app/search/_static/search_get.png)

Následující kód ukazuje změnu `form` značky:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Přidání vyhledávání podle genre

Přidejte následující `MovieGenreViewModel` třídy k *modely* složky:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Model film genre zobrazení bude obsahovat:

   * Seznam filmy.
   * A `SelectList` obsahující seznam žánry. To vám umožní uživateli vybrat genre ze seznamu.
   * `movieGenre`, která obsahuje vybrané genre.

Nahraďte `Index` metoda v `MoviesController.cs` následujícím kódem:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Následující kód je `LINQ` dotaz, který načte všechny žánry z databáze.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` z žánry vytvoří projekce odlišné žánry (jsme nechcete, aby naše seznamu výběru tak, aby měl duplicitní žánry).

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>Přidání vyhledávání podle genre do zobrazení indexu

Aktualizace `Index.cshtml` následujícím způsobem:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
V předchozí kód `DisplayNameFor` zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti. Vzhledem k tomu, že výrazu lambda je prověřovány spíše než vyhodnotit, jste neobdrželi porušení přístupu při `model`, `model.movies`, nebo `model.movies[0]` jsou `null` nebo je prázdný. Při vyhodnocení výrazu lambda (například `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.

Testování aplikací tak, že genre, název filmu a oběma.
