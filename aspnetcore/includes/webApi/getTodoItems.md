::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43dfa-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="43dfa-102">Předchozí kód definuje třídu kontroleru rozhraní API bez metod.</span><span class="sxs-lookup"><span data-stu-id="43dfa-102">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="43dfa-103">V následujících částech se přidají metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="43dfa-103">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43dfa-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="43dfa-105">Předchozí kód definuje třídu kontroleru rozhraní API bez metod.</span><span class="sxs-lookup"><span data-stu-id="43dfa-105">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="43dfa-106">V následujících částech se přidají metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="43dfa-106">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="43dfa-107">Třída je opatřen poznámkou `[ApiController]` atribut pro povolení některé pohodlné funkce.</span><span class="sxs-lookup"><span data-stu-id="43dfa-107">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="43dfa-108">Informace o funkcích, které jsou povolené v atributu najdete v tématu [opatřit poznámkami se ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="43dfa-108">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="43dfa-109">Konstruktor kontroleru používá [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`TodoContext`) do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="43dfa-109">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="43dfa-110">Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="43dfa-110">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="43dfa-111">Konstruktor přidá položku do databáze v paměti, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="43dfa-111">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="43dfa-112">Získat položkami seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="43dfa-112">Get to-do items</span></span>

<span data-ttu-id="43dfa-113">Chcete-li získat položkami seznamu úkolů, přidejte následující metody, které `TodoController` třídy:</span><span class="sxs-lookup"><span data-stu-id="43dfa-113">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43dfa-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43dfa-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end

<span data-ttu-id="43dfa-116">Tyto metody implementovat tyto dvě metody GET:</span><span class="sxs-lookup"><span data-stu-id="43dfa-116">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="43dfa-117">Zde je ukázka odpovědi HTTP pro `GetAll` metoda:</span><span class="sxs-lookup"><span data-stu-id="43dfa-117">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="43dfa-118">Později v tomto kurzu I zobrazí, jak lze zobrazit odpověď HTTP s [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="43dfa-118">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="43dfa-119">Směrování a adresy URL cesty</span><span class="sxs-lookup"><span data-stu-id="43dfa-119">Routing and URL paths</span></span>

<span data-ttu-id="43dfa-120">`[HttpGet]` Atribut označuje metodu, která reaguje na požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="43dfa-120">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="43dfa-121">Cesta URL pro každou metodu má následující formát:</span><span class="sxs-lookup"><span data-stu-id="43dfa-121">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="43dfa-122">Trvat řetězec šablony v kontroleru `Route` atribut:</span><span class="sxs-lookup"><span data-stu-id="43dfa-122">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43dfa-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43dfa-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end

* <span data-ttu-id="43dfa-125">Nahraďte `[controller]` název kontroleru, což je název třídy kontroleru minus příponou "Controller".</span><span class="sxs-lookup"><span data-stu-id="43dfa-125">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="43dfa-126">Tato ukázka je název třídy kontroleru **Todo**řadiče a názvu kořenové je "úkolů".</span><span class="sxs-lookup"><span data-stu-id="43dfa-126">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="43dfa-127">ASP.NET Core [směrování](xref:mvc/controllers/routing) malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="43dfa-127">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="43dfa-128">Pokud `[HttpGet]` atribut má šablonu trasy (například `[HttpGet("/products")]`, která připojení k cestě.</span><span class="sxs-lookup"><span data-stu-id="43dfa-128">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="43dfa-129">Tato ukázka nepoužívá šablony.</span><span class="sxs-lookup"><span data-stu-id="43dfa-129">This sample doesn't use a template.</span></span> <span data-ttu-id="43dfa-130">Další informace najdete v tématu [atribut směrování s atributy Http [akce]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="43dfa-130">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="43dfa-131">V následujícím `GetById` metody `"{id}"` je proměnná zástupný symbol pro jedinečný identifikátor položky seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="43dfa-131">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="43dfa-132">Když `GetById` je vyvolána, přiřadí hodnota `"{id}"` v adrese URL pro metodu `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="43dfa-132">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43dfa-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43dfa-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="43dfa-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

<span data-ttu-id="43dfa-135">`Name = "GetTodo"` Vytvoří pojmenovanou trasu.</span><span class="sxs-lookup"><span data-stu-id="43dfa-135">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="43dfa-136">Pojmenované trasy:</span><span class="sxs-lookup"><span data-stu-id="43dfa-136">Named routes:</span></span>

* <span data-ttu-id="43dfa-137">Povolte aplikaci vytvořit propojení HTTP pomocí názvu trasy.</span><span class="sxs-lookup"><span data-stu-id="43dfa-137">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="43dfa-138">Jsou vysvětleny později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="43dfa-138">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="43dfa-139">Vrácené hodnoty</span><span class="sxs-lookup"><span data-stu-id="43dfa-139">Return values</span></span>

<span data-ttu-id="43dfa-140">`GetAll` Metoda vrátí kolekci `TodoItem` objekty.</span><span class="sxs-lookup"><span data-stu-id="43dfa-140">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="43dfa-141">MVC automaticky serializuje objekt, který má [JSON](https://www.json.org/) a zapisuje JSON do textu zprávy s odpovědí.</span><span class="sxs-lookup"><span data-stu-id="43dfa-141">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="43dfa-142">Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="43dfa-142">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="43dfa-143">Nezpracované výjimky jsou převedeny do 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="43dfa-143">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43dfa-144">Naproti tomu `GetById` metoda vrátí další Obecné [IActionResult typu](xref:web-api/action-return-types#iactionresult-type), která představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="43dfa-144">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="43dfa-145">`GetById` má dva různé návratové typy:</span><span class="sxs-lookup"><span data-stu-id="43dfa-145">`GetById` has two different return types:</span></span>

* <span data-ttu-id="43dfa-146">Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="43dfa-146">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="43dfa-147">Vrácení [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="43dfa-147">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="43dfa-148">Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="43dfa-148">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="43dfa-149">Vrácení [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) výsledkem odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="43dfa-149">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43dfa-150">Naproti tomu `GetById` metoda vrátí [ActionResult\<T > typ](xref:web-api/action-return-types#actionresultt-type), která představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="43dfa-150">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="43dfa-151">`GetById` má dva různé návratové typy:</span><span class="sxs-lookup"><span data-stu-id="43dfa-151">`GetById` has two different return types:</span></span>

* <span data-ttu-id="43dfa-152">Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="43dfa-152">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="43dfa-153">Vrácení [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="43dfa-153">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="43dfa-154">Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="43dfa-154">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="43dfa-155">Vrácení `item` výsledkem odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="43dfa-155">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end