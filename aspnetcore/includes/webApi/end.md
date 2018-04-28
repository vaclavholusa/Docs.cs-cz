## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="dfa11-101">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="dfa11-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="dfa11-102">V následujících částech `Create`, `Update`, a `Delete` metody jsou přidány do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="dfa11-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="dfa11-103">Create</span><span class="sxs-lookup"><span data-stu-id="dfa11-103">Create</span></span>

<span data-ttu-id="dfa11-104">Přidejte následující `Create` metoda:</span><span class="sxs-lookup"><span data-stu-id="dfa11-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="dfa11-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="dfa11-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="dfa11-106">Předchozí kód je metoda HTTP POST, jak [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="dfa11-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="dfa11-107">[[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="dfa11-107">The [[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="dfa11-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="dfa11-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="dfa11-109">Předchozí kód je metoda HTTP POST, jak [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="dfa11-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="dfa11-110">MVC získá hodnotu položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="dfa11-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="dfa11-111">`CreatedAtRoute` Metoda:</span><span class="sxs-lookup"><span data-stu-id="dfa11-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="dfa11-112">Vrátí 201 odpověď.</span><span class="sxs-lookup"><span data-stu-id="dfa11-112">Returns a 201 response.</span></span> <span data-ttu-id="dfa11-113">HTTP 201 je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="dfa11-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="dfa11-114">Přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="dfa11-114">Adds a Location header to the response.</span></span> <span data-ttu-id="dfa11-115">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="dfa11-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="dfa11-116">V tématu [10.2.2 201 vytvořit](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="dfa11-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="dfa11-117">Vytvoří adresu URL pomocí "GetTodo" s názvem trasy.</span><span class="sxs-lookup"><span data-stu-id="dfa11-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="dfa11-118">"GetTodo" s názvem trasy je definována v `GetById`:</span><span class="sxs-lookup"><span data-stu-id="dfa11-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="dfa11-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="dfa11-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="dfa11-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="dfa11-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="dfa11-121">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="dfa11-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="dfa11-122">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dfa11-122">Start the app.</span></span>
* <span data-ttu-id="dfa11-123">Otevřete Postman.</span><span class="sxs-lookup"><span data-stu-id="dfa11-123">Open Postman.</span></span>

![Konzole postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="dfa11-125">Aktualizujte číslo portu v adrese URL místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="dfa11-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="dfa11-126">Nastavte jako metodu HTTP *POST*.</span><span class="sxs-lookup"><span data-stu-id="dfa11-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="dfa11-127">Klikněte **textu** kartě.</span><span class="sxs-lookup"><span data-stu-id="dfa11-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="dfa11-128">Vyberte **nezpracovaná** přepínač.</span><span class="sxs-lookup"><span data-stu-id="dfa11-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="dfa11-129">Nastavte typ na *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="dfa11-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="dfa11-130">Zadejte obsah žádosti s úkol podobné následujícím kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="dfa11-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="dfa11-131">Klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dfa11-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="dfa11-132">Pokud žádná odpověď zobrazí po kliknutí na **odeslat**, zakažte **SSL certifikační ověření** možnost.</span><span class="sxs-lookup"><span data-stu-id="dfa11-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="dfa11-133">To je v části **soubor** > **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="dfa11-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="dfa11-134">Klikněte **odeslat** tlačítko znovu po zakázání nastavení.</span><span class="sxs-lookup"><span data-stu-id="dfa11-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="dfa11-135">Klikněte na tlačítko **hlavičky** ve **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:</span><span class="sxs-lookup"><span data-stu-id="dfa11-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta hlavičky konzoly Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="dfa11-137">Hlavička umístění URI slouží pro přístup k novou položku.</span><span class="sxs-lookup"><span data-stu-id="dfa11-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="dfa11-138">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="dfa11-138">Update</span></span>

<span data-ttu-id="dfa11-139">Přidejte následující `Update` metoda:</span><span class="sxs-lookup"><span data-stu-id="dfa11-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="dfa11-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="dfa11-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="dfa11-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="dfa11-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="dfa11-142">`Update` je podobná `Create`, s výjimkou používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="dfa11-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="dfa11-143">Odpověď [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="dfa11-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="dfa11-144">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="dfa11-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="dfa11-145">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="dfa11-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="dfa11-146">Použijte Postman k aktualizaci název položky úkolů vás "cat":</span><span class="sxs-lookup"><span data-stu-id="dfa11-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="dfa11-148">Odstranit</span><span class="sxs-lookup"><span data-stu-id="dfa11-148">Delete</span></span>

<span data-ttu-id="dfa11-149">Přidejte následující `Delete` metoda:</span><span class="sxs-lookup"><span data-stu-id="dfa11-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="dfa11-150">`Delete` Odpověď je [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="dfa11-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="dfa11-151">Pomocí Postman odstranit položku seznamu úkolů:</span><span class="sxs-lookup"><span data-stu-id="dfa11-151">Use Postman to delete the to-do item:</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmd.png)
