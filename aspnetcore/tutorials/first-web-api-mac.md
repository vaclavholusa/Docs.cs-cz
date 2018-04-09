---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac
author: rick-anderson
description: Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: ce94091447452a51654f2cd4dad9043b63c737ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="f8b57-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="f8b57-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="f8b57-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f8b57-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f8b57-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="f8b57-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f8b57-106">Rozhraní není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="f8b57-106">The UI isn't constructed.</span></span>

<span data-ttu-id="f8b57-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="f8b57-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f8b57-108">systému macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="f8b57-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="f8b57-109">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f8b57-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="f8b57-110">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f8b57-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [template files](../includes/webApi/intro.md)]

<span data-ttu-id="f8b57-111">V tématu [Úvod do rozhraní ASP.NET MVC jádra v systému macOS nebo Linux](xref:tutorials/first-mvc-app-xplat/index) pro příklad, který používá trvalé databáze.</span><span class="sxs-lookup"><span data-stu-id="f8b57-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8b57-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8b57-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="f8b57-113">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="f8b57-113">Create the project</span></span>

<span data-ttu-id="f8b57-114">Ze sady Visual Studio, vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-114">From Visual Studio, select **File > New Solution**.</span></span>

![systému macOS nové řešení](first-web-api-mac/_static/sln.png)

<span data-ttu-id="f8b57-116">Vyberte **aplikace .NET Core > ASP.NET Core webového rozhraní API > Další**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-116">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![systému macOS dialogové okno Nový projekt](first-web-api-mac/_static/1.png)

<span data-ttu-id="f8b57-118">Zadejte **TodoApi** pro **název projektu**a vyberte možnost vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f8b57-118">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="f8b57-120">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="f8b57-120">Launch the app</span></span>

<span data-ttu-id="f8b57-121">V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8b57-121">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f8b57-122">Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f8b57-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="f8b57-123">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="f8b57-123">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f8b57-124">Změňte adresu URL na `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f8b57-124">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f8b57-125">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f8b57-125">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f8b57-126">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f8b57-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="f8b57-127">Nainstalujte [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="f8b57-127">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="f8b57-128">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="f8b57-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="f8b57-129">Z **projektu** nabídce vyberte možnost **přidání balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-129">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="f8b57-130">Alternativně můžete můžete kliknout pravým tlačítkem **závislosti**a potom vyberte **přidat balíčky**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-130">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="f8b57-131">Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f8b57-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="f8b57-132">Vyberte `Microsoft.EntityFrameworkCore.InMemory`a potom vyberte **přidat balíček**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="f8b57-133">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="f8b57-133">Add a model class</span></span>

<span data-ttu-id="f8b57-134">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8b57-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f8b57-135">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="f8b57-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f8b57-136">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="f8b57-136">Add a folder named *Models*.</span></span> <span data-ttu-id="f8b57-137">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="f8b57-137">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f8b57-138">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-138">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f8b57-139">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="f8b57-139">Name the folder *Models*.</span></span>

![Nová složka](first-web-api-mac/_static/folder.png)

<span data-ttu-id="f8b57-141">Poznámka: Můžete umístit třídy modelu kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="f8b57-141">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f8b57-142">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="f8b57-142">Add a `TodoItem` class.</span></span> <span data-ttu-id="f8b57-143">Klikněte pravým tlačítkem myši *modely* složky a vyberte **Přidat > Nový soubor > Obecné > prázdné třídy**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-143">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="f8b57-144">Název třídy `TodoItem`a potom vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="f8b57-144">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="f8b57-145">Nahraďte generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="f8b57-145">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f8b57-146">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f8b57-146">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f8b57-147">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="f8b57-147">Create the database context</span></span>

<span data-ttu-id="f8b57-148">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="f8b57-148">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f8b57-149">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="f8b57-149">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f8b57-150">Přidat `TodoContext` třídy k *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="f8b57-150">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f8b57-151">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="f8b57-151">Add a controller</span></span>

<span data-ttu-id="f8b57-152">V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f8b57-152">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="f8b57-153">Nahradit následující generovaný kód (a přidejte uzavírací závorky):</span><span class="sxs-lookup"><span data-stu-id="f8b57-153">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f8b57-154">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="f8b57-154">Launch the app</span></span>

<span data-ttu-id="f8b57-155">V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8b57-155">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f8b57-156">Visual Studio spustí prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="f8b57-156">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="f8b57-157">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="f8b57-157">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f8b57-158">Změňte adresu URL na `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f8b57-158">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f8b57-159">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f8b57-159">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="f8b57-160">Přejděte na `Todo` ovladač na`http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="f8b57-160">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f8b57-161">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="f8b57-161">Implement the other CRUD operations</span></span>

<span data-ttu-id="f8b57-162">Přidáme `Create`, `Update`, a `Delete` metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="f8b57-162">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="f8b57-163">Tyto jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="f8b57-163">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="f8b57-164">Sestavení projektu po přidání nebo změna kódu.</span><span class="sxs-lookup"><span data-stu-id="f8b57-164">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="f8b57-165">Create</span><span class="sxs-lookup"><span data-stu-id="f8b57-165">Create</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f8b57-166">Toto je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="f8b57-166">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f8b57-167">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8b57-167">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f8b57-168">`CreatedAtRoute` Metoda vrátí odpověď 201, což je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="f8b57-168">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="f8b57-169">`CreatedAtRoute` také přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="f8b57-169">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="f8b57-170">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="f8b57-170">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f8b57-171">V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f8b57-171">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f8b57-172">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="f8b57-172">Use Postman to send a Create request</span></span>

* <span data-ttu-id="f8b57-173">Spusťte aplikaci (**spustit > spustit s laděním**).</span><span class="sxs-lookup"><span data-stu-id="f8b57-173">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="f8b57-174">Spusťte Postman.</span><span class="sxs-lookup"><span data-stu-id="f8b57-174">Start Postman.</span></span>

![Konzole postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="f8b57-176">Nastavte jako metodu HTTP `POST`</span><span class="sxs-lookup"><span data-stu-id="f8b57-176">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f8b57-177">Vyberte **textu** přepínač</span><span class="sxs-lookup"><span data-stu-id="f8b57-177">Select the **Body** radio button</span></span>
* <span data-ttu-id="f8b57-178">Vyberte **nezpracovaná** přepínač</span><span class="sxs-lookup"><span data-stu-id="f8b57-178">Select the **raw** radio button</span></span>
* <span data-ttu-id="f8b57-179">Nastavte typ do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="f8b57-179">Set the type to JSON</span></span>
* <span data-ttu-id="f8b57-180">V editoru klíč hodnota zadejte položky Todo</span><span class="sxs-lookup"><span data-stu-id="f8b57-180">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f8b57-181">Vyberte **odeslat**</span><span class="sxs-lookup"><span data-stu-id="f8b57-181">Select **Send**</span></span>

* <span data-ttu-id="f8b57-182">Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="f8b57-182">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta hlavičky konzoly Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="f8b57-184">Hlavička umístění URI můžete použít pro přístup k prostředku, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f8b57-184">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="f8b57-185">Odvolat `GetById` vytvořili `"GetTodo"` s názvem trasy:</span><span class="sxs-lookup"><span data-stu-id="f8b57-185">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="f8b57-186">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="f8b57-186">Update</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f8b57-187">`Update` je podobná `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f8b57-187">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f8b57-188">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f8b57-188">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f8b57-189">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="f8b57-189">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f8b57-190">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="f8b57-190">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f8b57-192">Odstranit</span><span class="sxs-lookup"><span data-stu-id="f8b57-192">Delete</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f8b57-193">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f8b57-193">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="f8b57-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8b57-195">Next steps</span></span>

* [<span data-ttu-id="f8b57-196">Směrování na akce kontroleru</span><span class="sxs-lookup"><span data-stu-id="f8b57-196">Route to controller actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="f8b57-197">Informace o nasazení rozhraní API najdete v tématu [hostitele a nasazení](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="f8b57-197">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="f8b57-198">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f8b57-198">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="f8b57-199">Postman</span><span class="sxs-lookup"><span data-stu-id="f8b57-199">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="f8b57-200">Fiddler</span><span class="sxs-lookup"><span data-stu-id="f8b57-200">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
