Zvýrazněných code výš ukazuje kontext databáze film, který se přidává do [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru (v *Startup.cs* souboru). `services.AddDbContext<MvcMovieContext>(options =>` Určuje použití a připojovací řetězec databáze. `=>` je [lambda operátor](/dotnet/articles/csharp/language-reference/operators/lambda-operator).

Otevřete *Controllers/MoviesController.cs* soubor a zkontrolujte konstruktoru:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`MvcMovieContext `) do kontroleru. Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silného typu modely a @model – klíčové slovo

V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení pomocí `ViewData` slovníku. `ViewData` Slovník je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.

MVC obsahuje také možnost předat silně typované objekty modelu zobrazení. Tento přístup umožňuje lepší kompilaci Kontrola kódu silného typu. Tento mechanismus generování uživatelského rozhraní použít tento přístup (který předává silného typu modelu) s `MoviesController` třídy a zobrazení, když vytvořeno metody a zobrazení.

Zkontrolujte vygenerovaného `Details` metoda v *Controllers/MoviesController.cs* souboru:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]
::: moniker-end


`id` Parametru se obecně předá jako data trasy. Například `http://localhost:5000/movies/details/1` nastaví:

* Aby řadič `movies` řadiče (první segment adresy URL).
* Akce, která `details` (druhý segment adresy URL).
* Id na hodnotu 1 (poslední segment adresy URL).

Můžete také předáte `id` s dotazem řetězec následujícím způsobem:

`http://localhost:1234/movies/details?id=1`

`id` Parametr je definován jako [typ s možnou hodnotou Null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) v případě, že hodnotu ID není zadaný.



::: moniker range=">= aspnetcore-2.1"

A [výrazu lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) je předané do `FirstOrDefaultAsync` vyberte film entit, které odpovídají hodnotě trasy dat nebo dotaz, řetězec.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```
::: moniker-end

::: moniker range="<= aspnetcore-2.0"

A [výrazu lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) je předané do `SingleOrDefaultAsync` vyberte film entit, které odpovídají hodnotě trasy dat nebo dotaz, řetězec.

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```
::: moniker-end



Pokud se najde film, instanci `Movie` model předán `Details` zobrazení:

```csharp
return View(movie);
   ```

Zkontrolujte obsah *Views/Movies/Details.cshtml* souboru:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

Zahrnutím `@model` příkaz v horní části souboru zobrazení, můžete zadat typ objektu, která očekává zobrazení. Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Details.cshtml* souboru:

```HTML
@model MvcMovie.Models.Movie
   ```

To `@model` – direktiva umožňuje přístup k video, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Details.cshtml* zobrazení, kód předá pro každé pole film `DisplayNameFor` a `DisplayFor` pomocné objekty HTML s silného typu `Model` objektu. `Create` a `Edit` metody a zobrazení předat také `Movie` objekt modelu.

Zkontrolujte *Index.cshtml* zobrazení a `Index` metoda v filmy kontroleru. Všimněte si, jak kód vytvoří `List` objektu při volání `View` metoda. Kód předá to `Movies` ze seznamu `Index` metody akce k zobrazení:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

Když vytvoříte řadič filmy, generování uživatelského rozhraní automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` – Direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Index.cshtml* zobrazit, smyčky kód prostřednictvím filmy s `foreach` příkaz přes silného typu `Model` objektu:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Protože `Model` objektu je silného typu (jako `IEnumerable<Movie>` objektu), každá položka v smyčky je zadán jako `Movie`. Mezi další výhody, to znamená, že dostanete kompilaci Kontrola kódu:
