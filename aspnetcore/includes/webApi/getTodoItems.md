[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e6a53-101">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="e6a53-101">The preceding code:</span></span>

* <span data-ttu-id="e6a53-102">Definuje třídu prázdný kontroler.</span><span class="sxs-lookup"><span data-stu-id="e6a53-102">Defines an empty controller class.</span></span> <span data-ttu-id="e6a53-103">V následujících částech se přidají metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e6a53-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="e6a53-104">Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`TodoContext `) do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e6a53-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="e6a53-105">Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e6a53-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="e6a53-106">Konstruktor přidá položku do databáze v paměti, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e6a53-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="e6a53-107">Získávání položkami seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="e6a53-107">Getting to-do items</span></span>

<span data-ttu-id="e6a53-108">Chcete-li získat položkami seznamu úkolů, přidejte následující metody, které `TodoController` třídy.</span><span class="sxs-lookup"><span data-stu-id="e6a53-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="e6a53-109">Tyto metody implementovat tyto dvě metody GET:</span><span class="sxs-lookup"><span data-stu-id="e6a53-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e6a53-110">Zde je odpověď příklad HTTP pro `GetAll` metoda:</span><span class="sxs-lookup"><span data-stu-id="e6a53-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="e6a53-111">Později v tomto kurzu I zobrazí, jak lze zobrazit odpověď HTTP s [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="e6a53-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="e6a53-112">Směrování a adresy URL cesty</span><span class="sxs-lookup"><span data-stu-id="e6a53-112">Routing and URL paths</span></span>

<span data-ttu-id="e6a53-113">`[HttpGet]` Atribut určuje metody GET protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e6a53-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="e6a53-114">Cesta URL pro každou metodu má následující formát:</span><span class="sxs-lookup"><span data-stu-id="e6a53-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e6a53-115">Trvat řetězec šablony v kontroleru `Route` atribut:</span><span class="sxs-lookup"><span data-stu-id="e6a53-115">Take the template string in the controller's `Route` attribute:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="e6a53-116">Nahradí `[controller]` název kontroleru, což je název třídy kontroleru minus příponou "Controller".</span><span class="sxs-lookup"><span data-stu-id="e6a53-116">Replaces `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e6a53-117">Tato ukázka je název třídy kontroleru **Todo**řadiče a názvu kořenové je "úkolů".</span><span class="sxs-lookup"><span data-stu-id="e6a53-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="e6a53-118">ASP.NET Core [směrování](xref:mvc/controllers/routing) není velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="e6a53-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="e6a53-119">Pokud `[HttpGet]` atribut má šablonu trasy (například `[HttpGet("/products")]`, která připojení k cestě.</span><span class="sxs-lookup"><span data-stu-id="e6a53-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="e6a53-120">Tato ukázka nepoužívá šablony.</span><span class="sxs-lookup"><span data-stu-id="e6a53-120">This sample doesn't use a template.</span></span> <span data-ttu-id="e6a53-121">V tématu [atribut směrování s atributy Http [akce]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e6a53-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="e6a53-122">V `GetById` metoda:</span><span class="sxs-lookup"><span data-stu-id="e6a53-122">In the `GetById` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="e6a53-123">`"{id}"` je proměnná zástupný symbol pro ID `todo` položky.</span><span class="sxs-lookup"><span data-stu-id="e6a53-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="e6a53-124">Když `GetById` je vyvolána, přiřadí hodnotu "{id} v adrese URL pro metodu `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="e6a53-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="e6a53-125">`Name = "GetTodo"` Vytvoří pojmenovanou trasu.</span><span class="sxs-lookup"><span data-stu-id="e6a53-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="e6a53-126">Pojmenované trasy:</span><span class="sxs-lookup"><span data-stu-id="e6a53-126">Named routes:</span></span>

* <span data-ttu-id="e6a53-127">Povolte aplikaci vytvořit propojení HTTP pomocí názvu trasy.</span><span class="sxs-lookup"><span data-stu-id="e6a53-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="e6a53-128">Jsou vysvětleny později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e6a53-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="e6a53-129">Vrácené hodnoty</span><span class="sxs-lookup"><span data-stu-id="e6a53-129">Return values</span></span>

<span data-ttu-id="e6a53-130">`GetAll` Metoda vrátí `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="e6a53-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="e6a53-131">MVC automaticky serializuje objekt, který má [JSON](http://www.json.org/) a zapisuje JSON do textu zprávy s odpovědí.</span><span class="sxs-lookup"><span data-stu-id="e6a53-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e6a53-132">Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="e6a53-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e6a53-133">(Neošetřených výjimek jsou převedeny do 5xx chyby).</span><span class="sxs-lookup"><span data-stu-id="e6a53-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="e6a53-134">Naproti tomu `GetById` metoda vrátí další Obecné `IActionResult` typu, který představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="e6a53-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="e6a53-135">`GetById` má dva různé návratové typy:</span><span class="sxs-lookup"><span data-stu-id="e6a53-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e6a53-136">Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="e6a53-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e6a53-137">Vrácení `NotFound` vrátí odpověď HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e6a53-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="e6a53-138">Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="e6a53-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e6a53-139">Vrácení `ObjectResult` vrátí odpověď HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e6a53-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
