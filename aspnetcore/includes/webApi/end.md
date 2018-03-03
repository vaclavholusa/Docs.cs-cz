## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="209ff-101">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="209ff-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="209ff-102">V následujících částech `Create`, `Update`, a `Delete` metody jsou přidány do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="209ff-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="209ff-103">Create</span><span class="sxs-lookup"><span data-stu-id="209ff-103">Create</span></span>

<span data-ttu-id="209ff-104">Přidejte následující `Create` metoda.</span><span class="sxs-lookup"><span data-stu-id="209ff-104">Add the following `Create` method.</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="209ff-105">Předchozí kód je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="209ff-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="209ff-106">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="209ff-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="209ff-107">`CreatedAtRoute` Metoda:</span><span class="sxs-lookup"><span data-stu-id="209ff-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="209ff-108">Vrátí 201 odpověď.</span><span class="sxs-lookup"><span data-stu-id="209ff-108">Returns a 201 response.</span></span> <span data-ttu-id="209ff-109">HTTP 201 je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="209ff-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="209ff-110">Přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="209ff-110">Adds a Location header to the response.</span></span> <span data-ttu-id="209ff-111">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="209ff-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="209ff-112">V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="209ff-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="209ff-113">Vytvoří adresu URL pomocí "GetTodo" s názvem trasy.</span><span class="sxs-lookup"><span data-stu-id="209ff-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="209ff-114">"GetTodo" s názvem trasy je definována v `GetById`:</span><span class="sxs-lookup"><span data-stu-id="209ff-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="209ff-115">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="209ff-115">Use Postman to send a Create request</span></span>

![Konzole postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="209ff-117">Nastavte jako metodu HTTP `POST`</span><span class="sxs-lookup"><span data-stu-id="209ff-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="209ff-118">Vyberte **textu** přepínač</span><span class="sxs-lookup"><span data-stu-id="209ff-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="209ff-119">Vyberte **nezpracovaná** přepínač</span><span class="sxs-lookup"><span data-stu-id="209ff-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="209ff-120">Nastavte typ do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="209ff-120">Set the type to JSON</span></span>
* <span data-ttu-id="209ff-121">V editoru klíč hodnota zadejte položky Todo</span><span class="sxs-lookup"><span data-stu-id="209ff-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="209ff-122">Vyberte **odeslat**</span><span class="sxs-lookup"><span data-stu-id="209ff-122">Select **Send**</span></span>
* <span data-ttu-id="209ff-123">Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="209ff-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta hlavičky konzoly Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="209ff-125">Hlavička umístění URI slouží pro přístup k novou položku.</span><span class="sxs-lookup"><span data-stu-id="209ff-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="209ff-126">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="209ff-126">Update</span></span>

<span data-ttu-id="209ff-127">Přidejte následující `Update` metoda:</span><span class="sxs-lookup"><span data-stu-id="209ff-127">Add the following `Update` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="209ff-128">`Update` je podobná `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="209ff-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="209ff-129">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="209ff-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="209ff-130">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="209ff-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="209ff-131">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="209ff-131">To support partial updates, use HTTP PATCH.</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="209ff-133">Odstranit</span><span class="sxs-lookup"><span data-stu-id="209ff-133">Delete</span></span>

<span data-ttu-id="209ff-134">Přidejte následující `Delete` metoda:</span><span class="sxs-lookup"><span data-stu-id="209ff-134">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="209ff-135">`Delete` Odpověď je [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="209ff-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="209ff-136">Test `Delete`:</span><span class="sxs-lookup"><span data-stu-id="209ff-136">Test `Delete`:</span></span> 

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmd.png)
