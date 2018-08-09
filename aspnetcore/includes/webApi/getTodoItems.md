::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="b509c-101">Předcházející kód definuje třídu kontroleru rozhraní API bez metody.</span><span class="sxs-lookup"><span data-stu-id="b509c-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="b509c-102">V následujících částech jsou přidány metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b509c-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="b509c-103">Předcházející kód definuje třídu kontroleru rozhraní API bez metody.</span><span class="sxs-lookup"><span data-stu-id="b509c-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="b509c-104">V následujících částech jsou přidány metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b509c-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="b509c-105">Třída je opatřen poznámkou `[ApiController]` atribut pro povolení některé vhodné funkce.</span><span class="sxs-lookup"><span data-stu-id="b509c-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="b509c-106">Informace o funkcích, které jsou povolené v atributu naleznete v tématu [opatřit poznámkami třída s atributem ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="b509c-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="b509c-107">Kontroleru konstruktor používá [injektáž závislostí](xref:fundamentals/dependency-injection) vkládat kontext databáze (`TodoContext`) do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b509c-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="b509c-108">Kontext databáze se používá ve všech [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b509c-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="b509c-109">Konstruktor přidá položku do databáze v paměti, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b509c-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="b509c-110">Získat položky seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="b509c-110">Get to-do items</span></span>

<span data-ttu-id="b509c-111">Získat položky úkolů, přidejte následující metody, které `TodoController` třídy:</span><span class="sxs-lookup"><span data-stu-id="b509c-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="b509c-112">Tyto metody implementovat tyto dvě metody GET:</span><span class="sxs-lookup"><span data-stu-id="b509c-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="b509c-113">Tady je ukázková odpověď HTTP pro `GetAll` metody:</span><span class="sxs-lookup"><span data-stu-id="b509c-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="b509c-114">V pozdější části kurzu ukážeme, jak se dají zobrazit odpověď HTTP [Postman](https://www.getpostman.com/) nebo [curl](https://curl.haxx.se/docs/manpage.html).</span><span class="sxs-lookup"><span data-stu-id="b509c-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="b509c-115">Směrování a adresa URL cesty</span><span class="sxs-lookup"><span data-stu-id="b509c-115">Routing and URL paths</span></span>

<span data-ttu-id="b509c-116">`[HttpGet]` Atribut označuje metodu, která reaguje na požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="b509c-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="b509c-117">Cesta adresy URL pro každou metodu se vypočte takto:</span><span class="sxs-lookup"><span data-stu-id="b509c-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="b509c-118">Využijte řetězec šablony v kontroleru `Route` atribut:</span><span class="sxs-lookup"><span data-stu-id="b509c-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="b509c-119">Nahraďte `[controller]` název kontroleru, což je název třídy kontroleru minus příponu "Kontroleru".</span><span class="sxs-lookup"><span data-stu-id="b509c-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="b509c-120">V tomto příkladu je název třídy kontroleru **Todo**řadiče a názvu kořenového je "todo".</span><span class="sxs-lookup"><span data-stu-id="b509c-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="b509c-121">ASP.NET Core [směrování](xref:mvc/controllers/routing) velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="b509c-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="b509c-122">Pokud `[HttpGet]` atribut má šablona trasy (například `[HttpGet("/products")]`, připojení, která k cestě.</span><span class="sxs-lookup"><span data-stu-id="b509c-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="b509c-123">Tato ukázka nepoužívá šablony.</span><span class="sxs-lookup"><span data-stu-id="b509c-123">This sample doesn't use a template.</span></span> <span data-ttu-id="b509c-124">Další informace najdete v tématu [atribut směrování pomocí protokolu Http [příkaz] atributy](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="b509c-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="b509c-125">V následujícím `GetById` metody `"{id}"` je proměnná zástupný symbol pro jedinečný identifikátor položky úkolů.</span><span class="sxs-lookup"><span data-stu-id="b509c-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="b509c-126">Když `GetById` je vyvolána, přiřadí hodnotu `"{id}"` v adrese URL do metody `id` parametru.</span><span class="sxs-lookup"><span data-stu-id="b509c-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="b509c-127">`Name = "GetTodo"` Vytvoří trasu s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="b509c-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="b509c-128">Pojmenované trasy:</span><span class="sxs-lookup"><span data-stu-id="b509c-128">Named routes:</span></span>

* <span data-ttu-id="b509c-129">Povolte aplikaci vytvářet propojení HTTP pomocí názvu trasy.</span><span class="sxs-lookup"><span data-stu-id="b509c-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="b509c-130">Jsou vysvětleny dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b509c-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="b509c-131">Vrácené hodnoty</span><span class="sxs-lookup"><span data-stu-id="b509c-131">Return values</span></span>

<span data-ttu-id="b509c-132">`GetAll` Metoda vrátí kolekci `TodoItem` objekty.</span><span class="sxs-lookup"><span data-stu-id="b509c-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="b509c-133">MVC automaticky serializuje objekt, který má [JSON](https://www.json.org/) a zapíše do datové části zprávy s odpovědí JSON.</span><span class="sxs-lookup"><span data-stu-id="b509c-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="b509c-134">Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="b509c-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="b509c-135">Nezpracované výjimky jsou přeloženy do chyby 5xx.</span><span class="sxs-lookup"><span data-stu-id="b509c-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b509c-136">Naproti tomu `GetById` metoda vrátí obecnější [IActionResult typ](xref:web-api/action-return-types#iactionresult-type), která představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="b509c-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="b509c-137">`GetById` má dva různé typy vrácené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b509c-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="b509c-138">Pokud žádná položka odpovídá požadovaným ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="b509c-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="b509c-139">Vrací [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="b509c-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="b509c-140">V opačném případě vrátí metoda 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="b509c-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="b509c-141">Vrací [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) výsledkem odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="b509c-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b509c-142">Naproti tomu `GetById` vrátí metoda [ActionResult\<T > typ](xref:web-api/action-return-types#actionresultt-type), která představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="b509c-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="b509c-143">`GetById` má dva různé typy vrácené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b509c-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="b509c-144">Pokud žádná položka odpovídá požadovaným ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="b509c-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="b509c-145">Vrací [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="b509c-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="b509c-146">V opačném případě vrátí metoda 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="b509c-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="b509c-147">Vrací `item` výsledkem odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="b509c-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
