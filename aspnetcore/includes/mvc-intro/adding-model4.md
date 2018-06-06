<span data-ttu-id="00068-101">Zvýrazněných code výš ukazuje kontext databáze film, který se přidává do [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru (v *Startup.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="00068-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="00068-102">`services.AddDbContext<MvcMovieContext>(options =>` Určuje použití a připojovací řetězec databáze.</span><span class="sxs-lookup"><span data-stu-id="00068-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="00068-103">`=>` je [lambda operátor](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="00068-103">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="00068-104">Otevřete *Controllers/MoviesController.cs* soubor a zkontrolujte konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="00068-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="00068-105">Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`MvcMovieContext `) do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="00068-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="00068-106">Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="00068-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="00068-107">Silného typu modely a @model – klíčové slovo</span><span class="sxs-lookup"><span data-stu-id="00068-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="00068-108">V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení pomocí `ViewData` slovníku.</span><span class="sxs-lookup"><span data-stu-id="00068-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="00068-109">`ViewData` Slovník je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="00068-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="00068-110">MVC obsahuje také možnost předat silně typované objekty modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="00068-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="00068-111">Tento přístup umožňuje lepší kompilaci Kontrola kódu silného typu.</span><span class="sxs-lookup"><span data-stu-id="00068-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="00068-112">Tento mechanismus generování uživatelského rozhraní použít tento přístup (který předává silného typu modelu) s `MoviesController` třídy a zobrazení, když vytvořeno metody a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="00068-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="00068-113">Zkontrolujte vygenerovaného `Details` metoda v *Controllers/MoviesController.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="00068-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="00068-114">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="00068-114">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="00068-115">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="00068-115">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>
::: moniker-end


<span data-ttu-id="00068-116">`id` Parametru se obecně předá jako data trasy.</span><span class="sxs-lookup"><span data-stu-id="00068-116">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="00068-117">Například `http://localhost:5000/movies/details/1` nastaví:</span><span class="sxs-lookup"><span data-stu-id="00068-117">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="00068-118">Aby řadič `movies` řadiče (první segment adresy URL).</span><span class="sxs-lookup"><span data-stu-id="00068-118">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="00068-119">Akce, která `details` (druhý segment adresy URL).</span><span class="sxs-lookup"><span data-stu-id="00068-119">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="00068-120">Id na hodnotu 1 (poslední segment adresy URL).</span><span class="sxs-lookup"><span data-stu-id="00068-120">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="00068-121">Můžete také předáte `id` s dotazem řetězec následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="00068-121">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="00068-122">`id` Parametr je definován jako [typ s možnou hodnotou Null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) v případě, že hodnotu ID není zadaný.</span><span class="sxs-lookup"><span data-stu-id="00068-122">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>



::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="00068-123">A [výrazu lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) je předané do `FirstOrDefaultAsync` vyberte film entit, které odpovídají hodnotě trasy dat nebo dotaz, řetězec.</span><span class="sxs-lookup"><span data-stu-id="00068-123">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```
::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="00068-124">A [výrazu lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) je předané do `SingleOrDefaultAsync` vyberte film entit, které odpovídají hodnotě trasy dat nebo dotaz, řetězec.</span><span class="sxs-lookup"><span data-stu-id="00068-124">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```
::: moniker-end



<span data-ttu-id="00068-125">Pokud se najde film, instanci `Movie` model předán `Details` zobrazení:</span><span class="sxs-lookup"><span data-stu-id="00068-125">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="00068-126">Zkontrolujte obsah *Views/Movies/Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="00068-126">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="00068-127">Zahrnutím `@model` příkaz v horní části souboru zobrazení, můžete zadat typ objektu, která očekává zobrazení.</span><span class="sxs-lookup"><span data-stu-id="00068-127">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="00068-128">Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Details.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="00068-128">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="00068-129">To `@model` – direktiva umožňuje přístup k video, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu.</span><span class="sxs-lookup"><span data-stu-id="00068-129">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="00068-130">Například v *Details.cshtml* zobrazení, kód předá pro každé pole film `DisplayNameFor` a `DisplayFor` pomocné objekty HTML s silného typu `Model` objektu.</span><span class="sxs-lookup"><span data-stu-id="00068-130">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="00068-131">`Create` a `Edit` metody a zobrazení předat také `Movie` objekt modelu.</span><span class="sxs-lookup"><span data-stu-id="00068-131">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="00068-132">Zkontrolujte *Index.cshtml* zobrazení a `Index` metoda v filmy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="00068-132">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="00068-133">Všimněte si, jak kód vytvoří `List` objektu při volání `View` metoda.</span><span class="sxs-lookup"><span data-stu-id="00068-133">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="00068-134">Kód předá to `Movies` ze seznamu `Index` metody akce k zobrazení:</span><span class="sxs-lookup"><span data-stu-id="00068-134">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="00068-135">Když vytvoříte řadič filmy, generování uživatelského rozhraní automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="00068-135">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="00068-136">`@model` – Direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu.</span><span class="sxs-lookup"><span data-stu-id="00068-136">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="00068-137">Například v *Index.cshtml* zobrazit, smyčky kód prostřednictvím filmy s `foreach` příkaz přes silného typu `Model` objektu:</span><span class="sxs-lookup"><span data-stu-id="00068-137">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="00068-138">Protože `Model` objektu je silného typu (jako `IEnumerable<Movie>` objektu), každá položka v smyčky je zadán jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="00068-138">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="00068-139">Mezi další výhody, to znamená, že dostanete kompilaci Kontrola kódu:</span><span class="sxs-lookup"><span data-stu-id="00068-139">Among other benefits, this means that you get compile-time checking of the code:</span></span>
