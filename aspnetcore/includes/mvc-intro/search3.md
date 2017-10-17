<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="59a01-101">Teď při odesílání vyhledávání, adresa URL obsahuje řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="59a01-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="59a01-102">Hledání budou také moct `HttpGet Index` metody akce, i když máte `HttpPost Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="59a01-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Okno prohlížeče zobrazující hledaný_řetězec = neodstraněných adresu Url a vrátí, filmy Ghostbusters a Ghostbusters 2, obsahovat neodstraněných aplikace word](../../tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="59a01-104">Následující kód ukazuje změnu `form` značky:</span><span class="sxs-lookup"><span data-stu-id="59a01-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="59a01-105">Přidání vyhledávání podle genre</span><span class="sxs-lookup"><span data-stu-id="59a01-105">Adding Search by genre</span></span>

<span data-ttu-id="59a01-106">Přidejte následující `MovieGenreViewModel` třídy k *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="59a01-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

<span data-ttu-id="59a01-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="59a01-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span></span>

<span data-ttu-id="59a01-108">Model film genre zobrazení bude obsahovat:</span><span class="sxs-lookup"><span data-stu-id="59a01-108">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="59a01-109">Seznam filmy.</span><span class="sxs-lookup"><span data-stu-id="59a01-109">A list of movies.</span></span>
   * <span data-ttu-id="59a01-110">A `SelectList` obsahující seznam žánry.</span><span class="sxs-lookup"><span data-stu-id="59a01-110">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="59a01-111">To vám umožní uživateli vybrat genre ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="59a01-111">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="59a01-112">`movieGenre`, která obsahuje vybrané genre.</span><span class="sxs-lookup"><span data-stu-id="59a01-112">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="59a01-113">Nahraďte `Index` metoda v `MoviesController.cs` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="59a01-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

<span data-ttu-id="59a01-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="59a01-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="59a01-115">Následující kód je `LINQ` dotaz, který načte všechny žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="59a01-115">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="59a01-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="59a01-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="59a01-117">`SelectList` z žánry vytvoří projekce odlišné žánry (jsme nechcete, aby naše seznamu výběru tak, aby měl duplicitní žánry).</span><span class="sxs-lookup"><span data-stu-id="59a01-117">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="59a01-118">Přidání vyhledávání podle genre do zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="59a01-118">Adding search by genre to the Index view</span></span>

<span data-ttu-id="59a01-119">Aktualizace `Index.cshtml` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="59a01-119">Update `Index.cshtml` as follows:</span></span>

<span data-ttu-id="59a01-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span><span class="sxs-lookup"><span data-stu-id="59a01-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span></span>

<span data-ttu-id="59a01-121">Zkontrolujte výrazu lambda použít v následujících pomocné rutiny HTML:</span><span class="sxs-lookup"><span data-stu-id="59a01-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="59a01-122">V předchozí kód `DisplayNameFor` zkontroluje pomocné rutiny HTML `Title` odkazovaným v tomto výrazu lambda k určení zobrazovaný název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="59a01-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="59a01-123">Vzhledem k tomu, že výrazu lambda je prověřovány spíše než vyhodnotit, jste neobdrželi porušení přístupu při `model`, `model.movies`, nebo `model.movies[0]` jsou `null` nebo je prázdný.</span><span class="sxs-lookup"><span data-stu-id="59a01-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="59a01-124">Při vyhodnocení výrazu lambda (například `@Html.DisplayFor(modelItem => item.Title)`), se vyhodnocují hodnoty vlastností modelu.</span><span class="sxs-lookup"><span data-stu-id="59a01-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="59a01-125">Testování aplikací tak, že genre, název filmu a oběma.</span><span class="sxs-lookup"><span data-stu-id="59a01-125">Test the app by searching by genre, by movie title, and by both.</span></span>
