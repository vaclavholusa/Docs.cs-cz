## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="74ce8-101">Provádění jiných operací CRUD</span><span class="sxs-lookup"><span data-stu-id="74ce8-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="74ce8-102">V následujících částech `Create`, `Update`, a `Delete` metody se přidají do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="74ce8-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="74ce8-103">Create</span><span class="sxs-lookup"><span data-stu-id="74ce8-103">Create</span></span>

<span data-ttu-id="74ce8-104">Přidejte následující `Create` metody:</span><span class="sxs-lookup"><span data-stu-id="74ce8-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="74ce8-105">Předchozí kód je metoda HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="74ce8-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="74ce8-106">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut oznamuje MVC k získání hodnoty položky seznamu úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="74ce8-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="74ce8-107">Předchozí kód je metoda HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="74ce8-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="74ce8-108">MVC získá hodnotu položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="74ce8-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="74ce8-109">`CreatedAtRoute` Metody:</span><span class="sxs-lookup"><span data-stu-id="74ce8-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="74ce8-110">Vrátí odezvě 201.</span><span class="sxs-lookup"><span data-stu-id="74ce8-110">Returns a 201 response.</span></span> <span data-ttu-id="74ce8-111">HTTP 201 je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="74ce8-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="74ce8-112">Přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="74ce8-112">Adds a Location header to the response.</span></span> <span data-ttu-id="74ce8-113">Hlavička umístění Určuje identifikátor URI nově vytvořeného úkolu položky.</span><span class="sxs-lookup"><span data-stu-id="74ce8-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="74ce8-114">Zobrazit [10.2.2 201 vytvořili](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="74ce8-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="74ce8-115">K vytvoření adresy URL používá "GetTodo" s názvem trasy.</span><span class="sxs-lookup"><span data-stu-id="74ce8-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="74ce8-116">"GetTodo" s názvem trasy je definována v `GetById`:</span><span class="sxs-lookup"><span data-stu-id="74ce8-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="74ce8-117">Odeslat požadavek na vytvoření pomocí nástroje Postman</span><span class="sxs-lookup"><span data-stu-id="74ce8-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="74ce8-118">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74ce8-118">Start the app.</span></span>
* <span data-ttu-id="74ce8-119">Otevřete nástroj Postman.</span><span class="sxs-lookup"><span data-stu-id="74ce8-119">Open Postman.</span></span>

![Konzola nástroje postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="74ce8-121">Aktualizujte číslo portu adresy URL místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="74ce8-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="74ce8-122">Nastavte jako metodu HTTP *příspěvek*.</span><span class="sxs-lookup"><span data-stu-id="74ce8-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="74ce8-123">Klikněte na tlačítko **tělo** kartu.</span><span class="sxs-lookup"><span data-stu-id="74ce8-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="74ce8-124">Vyberte **nezpracovaná** přepínač.</span><span class="sxs-lookup"><span data-stu-id="74ce8-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="74ce8-125">Nastavte typ *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="74ce8-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="74ce8-126">Zadejte text požadavku s položku úkolu připomínající následující JSON:</span><span class="sxs-lookup"><span data-stu-id="74ce8-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="74ce8-127">Klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74ce8-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="74ce8-128">Pokud žádná odpověď zobrazí po kliknutí na tlačítko **odeslat**, zakažte **ověření certifikace SSL** možnost.</span><span class="sxs-lookup"><span data-stu-id="74ce8-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="74ce8-129">Se nachází v části **souboru** > **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="74ce8-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="74ce8-130">Klikněte na tlačítko **odeslat** tlačítko znovu po zakázání nastavení.</span><span class="sxs-lookup"><span data-stu-id="74ce8-130">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="74ce8-131">Klikněte na tlačítko **záhlaví** kartu **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:</span><span class="sxs-lookup"><span data-stu-id="74ce8-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta hlavičky z konzoly nástroje Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="74ce8-133">Hlavička umístění URI slouží pro přístup k nové položky.</span><span class="sxs-lookup"><span data-stu-id="74ce8-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="74ce8-134">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="74ce8-134">Update</span></span>

<span data-ttu-id="74ce8-135">Přidejte následující `Update` metody:</span><span class="sxs-lookup"><span data-stu-id="74ce8-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="74ce8-136">`Update` je podobný `Create`, s výjimkou používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="74ce8-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="74ce8-137">Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="74ce8-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="74ce8-138">Podle specifikace HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovaná entita, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="74ce8-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="74ce8-139">Pro podporu částečné aktualizace, pomocí HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="74ce8-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="74ce8-140">Pomocí nástroje Postman aktualizovat název položky seznamu úkolů "procesem cat":</span><span class="sxs-lookup"><span data-stu-id="74ce8-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="74ce8-142">Odstranit</span><span class="sxs-lookup"><span data-stu-id="74ce8-142">Delete</span></span>

<span data-ttu-id="74ce8-143">Přidejte následující `Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="74ce8-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="74ce8-144">`Delete` Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="74ce8-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="74ce8-145">Pomocí nástroje Postman odstranění položky seznamu úkolů:</span><span class="sxs-lookup"><span data-stu-id="74ce8-145">Use Postman to delete the to-do item:</span></span>

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](../../tutorials/first-web-api/_static/pmd.png)
