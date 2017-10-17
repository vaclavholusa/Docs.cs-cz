<span data-ttu-id="a2a72-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="a2a72-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="a2a72-102">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="a2a72-102">The preceding code:</span></span>

* <span data-ttu-id="a2a72-103">Definuje třídu prázdný kontroler.</span><span class="sxs-lookup"><span data-stu-id="a2a72-103">Defines an empty controller class.</span></span> <span data-ttu-id="a2a72-104">V následujících částech přidáme metody k implementaci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2a72-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="a2a72-105">Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`TodoContext `) do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a2a72-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="a2a72-106">Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a2a72-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="a2a72-107">Konstruktor přidá položku do databáze v paměti, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="a2a72-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="a2a72-108">Získávání položkami seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="a2a72-108">Getting to-do items</span></span>

<span data-ttu-id="a2a72-109">Chcete-li získat položkami seznamu úkolů, přidejte následující metody, které `TodoController` třídy.</span><span class="sxs-lookup"><span data-stu-id="a2a72-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="a2a72-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="a2a72-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="a2a72-111">Tyto metody implementovat tyto dvě metody GET:</span><span class="sxs-lookup"><span data-stu-id="a2a72-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="a2a72-112">Zde je odpověď příklad HTTP pro `GetAll` metoda:</span><span class="sxs-lookup"><span data-stu-id="a2a72-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="a2a72-113">Později v tomto kurzu I zobrazí, jak můžete zobrazit pomocí odpovědi HTTP [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="a2a72-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="a2a72-114">Směrování a adresy URL cesty</span><span class="sxs-lookup"><span data-stu-id="a2a72-114">Routing and URL paths</span></span>

<span data-ttu-id="a2a72-115">`[HttpGet]` Atribut určuje metody GET protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2a72-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="a2a72-116">Cesta URL pro každou metodu má následující formát:</span><span class="sxs-lookup"><span data-stu-id="a2a72-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="a2a72-117">Proveďte řetězec šablony v atributu kontroleru trasy:</span><span class="sxs-lookup"><span data-stu-id="a2a72-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="a2a72-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="a2a72-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="a2a72-119">Nahraďte název kontroleru, což je název třídy kontroleru minus příponou "Controller" "[Controller]".</span><span class="sxs-lookup"><span data-stu-id="a2a72-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="a2a72-120">Tato ukázka je název třídy kontroleru **Todo**řadiče a názvu kořenové je "úkolů".</span><span class="sxs-lookup"><span data-stu-id="a2a72-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="a2a72-121">ASP.NET Core [směrování](xref:mvc/controllers/routing) není velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a2a72-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="a2a72-122">Pokud `[HttpGet]` atribut má šablonu trasy (například `[HttpGet("/products")]`, která připojení k cestě.</span><span class="sxs-lookup"><span data-stu-id="a2a72-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="a2a72-123">Tato ukázka nepoužívá šablony.</span><span class="sxs-lookup"><span data-stu-id="a2a72-123">This sample doesn't use a template.</span></span> <span data-ttu-id="a2a72-124">V tématu [atribut směrování s atributy Http [akce]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a2a72-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="a2a72-125">V `GetById` metoda:</span><span class="sxs-lookup"><span data-stu-id="a2a72-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="a2a72-126">`"{id}"`je proměnná zástupný symbol pro ID `todo` položky.</span><span class="sxs-lookup"><span data-stu-id="a2a72-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="a2a72-127">Když `GetById` je vyvolána, přiřadí hodnotu "{id} v adrese URL pro metodu `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="a2a72-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="a2a72-128">`Name = "GetTodo"`Vytvoří pojmenovanou trasu a umožňuje propojit tato trasa v odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2a72-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="a2a72-129">I objasníme ho s příklad později.</span><span class="sxs-lookup"><span data-stu-id="a2a72-129">I'll explain it with an example later.</span></span> <span data-ttu-id="a2a72-130">V tématu [směrování do akce Kontroleru](xref:mvc/controllers/routing) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="a2a72-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="a2a72-131">Vrácené hodnoty</span><span class="sxs-lookup"><span data-stu-id="a2a72-131">Return values</span></span>

<span data-ttu-id="a2a72-132">`GetAll` Metoda vrátí `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="a2a72-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="a2a72-133">MVC automaticky serializuje objekt, který má [JSON](http://www.json.org/) a zapisuje JSON do textu zprávy s odpovědí.</span><span class="sxs-lookup"><span data-stu-id="a2a72-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="a2a72-134">Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="a2a72-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="a2a72-135">(Neošetřených výjimek jsou převedeny do 5xx chyby).</span><span class="sxs-lookup"><span data-stu-id="a2a72-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="a2a72-136">Naproti tomu `GetById` metoda vrátí další Obecné `IActionResult` typu, který představuje širokou škálu návratové typy.</span><span class="sxs-lookup"><span data-stu-id="a2a72-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="a2a72-137">`GetById`má dva různé návratové typy:</span><span class="sxs-lookup"><span data-stu-id="a2a72-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="a2a72-138">Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="a2a72-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="a2a72-139">K tomu je potřeba vrácení `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="a2a72-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="a2a72-140">Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="a2a72-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a2a72-141">K tomu je potřeba návrat`ObjectResult`</span><span class="sxs-lookup"><span data-stu-id="a2a72-141">This is done by returning an `ObjectResult`</span></span>
