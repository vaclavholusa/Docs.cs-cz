::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="cb0e5-101">Předcházející kód definuje třídu kontroleru rozhraní API bez metody.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="cb0e5-102">V následujících částech jsou přidány metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="cb0e5-103">Předcházející kód definuje třídu kontroleru rozhraní API bez metody.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="cb0e5-104">V následujících částech jsou přidány metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="cb0e5-105">Třída je opatřen poznámkou `[ApiController]` atribut pro povolení některé vhodné funkce.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="cb0e5-106">Informace o funkcích, které jsou povolené v atributu naleznete v tématu [opatřit poznámkami třída s atributem ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="cb0e5-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="cb0e5-107">Kontroleru konstruktor používá [injektáž závislostí](xref:fundamentals/dependency-injection) vkládat kontext databáze (`TodoContext`) do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="cb0e5-108">Kontext databáze se používá ve všech [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="cb0e5-109">Konstruktor přidá položku do databáze v paměti, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="cb0e5-110">Získat položky seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="cb0e5-110">Get to-do items</span></span>

<span data-ttu-id="cb0e5-111">Získat položky úkolů, přidejte následující metody, které `TodoController` třídy:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="cb0e5-112">Tyto metody implementovat tyto dvě metody GET:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="cb0e5-113">Tady je ukázková odpověď HTTP pro `GetAll` metody:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="cb0e5-114">V pozdější části kurzu ukážeme, jak se dají zobrazit odpověď HTTP [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="cb0e5-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="cb0e5-115">Směrování a adresa URL cesty</span><span class="sxs-lookup"><span data-stu-id="cb0e5-115">Routing and URL paths</span></span>

<span data-ttu-id="cb0e5-116">`[HttpGet]` Atribut označuje metodu, která reaguje na požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="cb0e5-117">Cesta adresy URL pro každou metodu se vypočte takto:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="cb0e5-118">Využijte řetězec šablony v kontroleru `Route` atribut:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="cb0e5-119">Nahraďte `[controller]` název kontroleru, což je název třídy kontroleru minus příponu "Kontroleru".</span><span class="sxs-lookup"><span data-stu-id="cb0e5-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="cb0e5-120">V tomto příkladu je název třídy kontroleru **Todo**řadiče a názvu kořenového je "todo".</span><span class="sxs-lookup"><span data-stu-id="cb0e5-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="cb0e5-121">ASP.NET Core [směrování](xref:mvc/controllers/routing) velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="cb0e5-122">Pokud `[HttpGet]` atribut má šablona trasy (například `[HttpGet("/products")]`, připojení, která k cestě.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="cb0e5-123">Tato ukázka nepoužívá šablony.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-123">This sample doesn't use a template.</span></span> <span data-ttu-id="cb0e5-124">Další informace najdete v tématu [atribut směrování pomocí protokolu Http [příkaz] atributy](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="cb0e5-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="cb0e5-125">V následujícím `GetById` metody `"{id}"` je proměnná zástupný symbol pro jedinečný identifikátor položky úkolů.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="cb0e5-126">Když `GetById` je vyvolána, přiřadí hodnotu `"{id}"` v adrese URL do metody `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="cb0e5-127">`Name = "GetTodo"` Vytvoří trasu s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="cb0e5-128">Pojmenované trasy:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-128">Named routes:</span></span>

* <span data-ttu-id="cb0e5-129">Povolte aplikaci vytvářet propojení HTTP pomocí názvu trasy.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="cb0e5-130">Jsou vysvětleny dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="cb0e5-131">Vrácené hodnoty</span><span class="sxs-lookup"><span data-stu-id="cb0e5-131">Return values</span></span>

<span data-ttu-id="cb0e5-132">`GetAll` Metoda vrátí kolekci `TodoItem` objekty.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="cb0e5-133">MVC automaticky serializuje objekt, který má [JSON](https://www.json.org/) a zapíše do datové části zprávy s odpovědí JSON.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="cb0e5-134">Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="cb0e5-135">Nezpracované výjimky jsou přeloženy do chyby 5xx.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="cb0e5-136">Naproti tomu `GetById` metoda vrátí obecnější [IActionResult typ](xref:web-api/action-return-types#iactionresult-type), která představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="cb0e5-137">`GetById` má dva různé typy vrácené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="cb0e5-138">Pokud žádná položka odpovídá požadovaným ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="cb0e5-139">Vrací [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="cb0e5-140">V opačném případě vrátí metoda 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="cb0e5-141">Vrací [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) výsledkem odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="cb0e5-142">Naproti tomu `GetById` vrátí metoda [ActionResult\<T > typ](xref:web-api/action-return-types#actionresultt-type), která představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="cb0e5-143">`GetById` má dva různé typy vrácené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cb0e5-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="cb0e5-144">Pokud žádná položka odpovídá požadovaným ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="cb0e5-145">Vrací [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="cb0e5-146">V opačném případě vrátí metoda 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="cb0e5-147">Vrací `item` výsledkem odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="cb0e5-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
