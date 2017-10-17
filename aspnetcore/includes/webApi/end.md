## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f41f8-101">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="f41f8-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="f41f8-102">Přidáme `Create`, `Update`, a `Delete` metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="f41f8-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="f41f8-103">Tyto jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="f41f8-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="f41f8-104">Sestavení projektu po přidání nebo změna kódu.</span><span class="sxs-lookup"><span data-stu-id="f41f8-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="f41f8-105">Create</span><span class="sxs-lookup"><span data-stu-id="f41f8-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f41f8-106">Toto je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="f41f8-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f41f8-107">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="f41f8-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f41f8-108">`CreatedAtRoute` Metoda vrátí odpověď 201, což je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="f41f8-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="f41f8-109">`CreatedAtRoute`také přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="f41f8-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="f41f8-110">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="f41f8-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f41f8-111">V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f41f8-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f41f8-112">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="f41f8-112">Use Postman to send a Create request</span></span>

![Konzole postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="f41f8-114">Nastavte jako metodu HTTP`POST`</span><span class="sxs-lookup"><span data-stu-id="f41f8-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f41f8-115">Vyberte **textu** přepínač</span><span class="sxs-lookup"><span data-stu-id="f41f8-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="f41f8-116">Vyberte **nezpracovaná** přepínač</span><span class="sxs-lookup"><span data-stu-id="f41f8-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="f41f8-117">Nastavte typ do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="f41f8-117">Set the type to JSON</span></span>
* <span data-ttu-id="f41f8-118">V editoru klíč hodnota zadejte položky Todo</span><span class="sxs-lookup"><span data-stu-id="f41f8-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f41f8-119">Vyberte **odeslat**</span><span class="sxs-lookup"><span data-stu-id="f41f8-119">Select **Send**</span></span>

* <span data-ttu-id="f41f8-120">Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="f41f8-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta hlavičky konzoly Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="f41f8-122">Hlavička umístění URI můžete použít pro přístup k prostředku, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f41f8-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="f41f8-123">Odvolat `GetById` vytvořili `"GetTodo"` s názvem trasy:</span><span class="sxs-lookup"><span data-stu-id="f41f8-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="f41f8-124">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="f41f8-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f41f8-125">`Update`je podobná `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f41f8-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f41f8-126">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f41f8-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f41f8-127">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="f41f8-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f41f8-128">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="f41f8-128">To support partial updates, use HTTP PATCH.</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f41f8-130">Odstranit</span><span class="sxs-lookup"><span data-stu-id="f41f8-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f41f8-131">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f41f8-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmd.png)
