---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac"
author: rick-anderson
description: "Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: c7825530146e5e4a879bf44db5a92bc7700de73b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="f32f5-103">Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="f32f5-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="f32f5-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f32f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f32f5-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="f32f5-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f32f5-106">Rozhraní není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="f32f5-106">The UI isn't constructed.</span></span>

<span data-ttu-id="f32f5-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="f32f5-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f32f5-108">systému macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="f32f5-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="f32f5-109">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f32f5-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="f32f5-110">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f32f5-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="f32f5-111">V tématu [Úvod do architektury ASP.NET MVC jádra na Mac nebo Linux](xref:tutorials/first-mvc-app-xplat/index) pro příklad, který používá trvalé databáze.</span><span class="sxs-lookup"><span data-stu-id="f32f5-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f32f5-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f32f5-112">Prerequisites</span></span>

<span data-ttu-id="f32f5-113">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="f32f5-113">Install the following:</span></span>

- <span data-ttu-id="f32f5-114">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější</span><span class="sxs-lookup"><span data-stu-id="f32f5-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="f32f5-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f32f5-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="f32f5-116">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="f32f5-116">Create the project</span></span>

<span data-ttu-id="f32f5-117">Ze sady Visual Studio, vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-117">From Visual Studio, select **File > New Solution**.</span></span>

![systému macOS nové řešení](first-web-api-mac/_static/sln.png)

<span data-ttu-id="f32f5-119">Vyberte **aplikace .NET Core > ASP.NET Core webového rozhraní API > Další**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![systému macOS dialogové okno Nový projekt](first-web-api-mac/_static/1.png)

<span data-ttu-id="f32f5-121">Zadejte **TodoApi** pro **název projektu**a vyberte možnost vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f32f5-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="f32f5-123">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="f32f5-123">Launch the app</span></span>

<span data-ttu-id="f32f5-124">V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f32f5-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f32f5-125">Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f32f5-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="f32f5-126">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="f32f5-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f32f5-127">Změňte adresu URL na `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f32f5-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f32f5-128">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f32f5-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f32f5-129">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f32f5-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="f32f5-130">Nainstalujte [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="f32f5-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="f32f5-131">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="f32f5-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="f32f5-132">Z **projektu** nabídce vyberte možnost **přidání balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="f32f5-133">Alternativně můžete můžete kliknout pravým tlačítkem **závislosti**a potom vyberte **přidat balíčky**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="f32f5-134">Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f32f5-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="f32f5-135">Vyberte `Microsoft.EntityFrameworkCore.InMemory`a potom vyberte **přidat balíček**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="f32f5-136">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="f32f5-136">Add a model class</span></span>

<span data-ttu-id="f32f5-137">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f32f5-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f32f5-138">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="f32f5-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f32f5-139">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="f32f5-139">Add a folder named *Models*.</span></span> <span data-ttu-id="f32f5-140">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="f32f5-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f32f5-141">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f32f5-142">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="f32f5-142">Name the folder *Models*.</span></span>

![Nová složka](first-web-api-mac/_static/folder.png)

<span data-ttu-id="f32f5-144">Poznámka: Můžete umístit třídy modelu kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="f32f5-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f32f5-145">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="f32f5-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="f32f5-146">Klikněte pravým tlačítkem myši *modely* složky a vyberte **Přidat > Nový soubor > Obecné > prázdné třídy**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="f32f5-147">Název třídy `TodoItem`a potom vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="f32f5-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="f32f5-148">Nahraďte generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="f32f5-148">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f32f5-149">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f32f5-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f32f5-150">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="f32f5-150">Create the database context</span></span>

<span data-ttu-id="f32f5-151">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="f32f5-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f32f5-152">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="f32f5-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f32f5-153">Přidat `TodoContext` třídy k *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="f32f5-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f32f5-154">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="f32f5-154">Add a controller</span></span>

<span data-ttu-id="f32f5-155">V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f32f5-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="f32f5-156">Nahradit následující generovaný kód (a přidejte uzavírací závorky):</span><span class="sxs-lookup"><span data-stu-id="f32f5-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f32f5-157">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="f32f5-157">Launch the app</span></span>

<span data-ttu-id="f32f5-158">V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f32f5-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f32f5-159">Visual Studio spustí prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="f32f5-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="f32f5-160">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="f32f5-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f32f5-161">Změňte adresu URL na `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f32f5-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f32f5-162">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f32f5-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="f32f5-163">Přejděte na `Todo` ovladač na`http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="f32f5-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f32f5-164">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="f32f5-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="f32f5-165">Přidáme `Create`, `Update`, a `Delete` metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="f32f5-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="f32f5-166">Tyto jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="f32f5-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="f32f5-167">Sestavení projektu po přidání nebo změna kódu.</span><span class="sxs-lookup"><span data-stu-id="f32f5-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="f32f5-168">Create</span><span class="sxs-lookup"><span data-stu-id="f32f5-168">Create</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f32f5-169">Toto je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="f32f5-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f32f5-170">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="f32f5-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f32f5-171">`CreatedAtRoute` Metoda vrátí odpověď 201, což je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="f32f5-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="f32f5-172">`CreatedAtRoute` také přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="f32f5-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="f32f5-173">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="f32f5-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f32f5-174">V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f32f5-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f32f5-175">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="f32f5-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="f32f5-176">Spusťte aplikaci (**spustit > spustit s laděním**).</span><span class="sxs-lookup"><span data-stu-id="f32f5-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="f32f5-177">Spusťte Postman.</span><span class="sxs-lookup"><span data-stu-id="f32f5-177">Start Postman.</span></span>

![Konzole postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="f32f5-179">Nastavte jako metodu HTTP `POST`</span><span class="sxs-lookup"><span data-stu-id="f32f5-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f32f5-180">Vyberte **textu** přepínač</span><span class="sxs-lookup"><span data-stu-id="f32f5-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="f32f5-181">Vyberte **nezpracovaná** přepínač</span><span class="sxs-lookup"><span data-stu-id="f32f5-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="f32f5-182">Nastavte typ do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="f32f5-182">Set the type to JSON</span></span>
* <span data-ttu-id="f32f5-183">V editoru klíč hodnota zadejte položky Todo</span><span class="sxs-lookup"><span data-stu-id="f32f5-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f32f5-184">Vyberte **odeslat**</span><span class="sxs-lookup"><span data-stu-id="f32f5-184">Select **Send**</span></span>

* <span data-ttu-id="f32f5-185">Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="f32f5-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta hlavičky konzoly Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="f32f5-187">Hlavička umístění URI můžete použít pro přístup k prostředku, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f32f5-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="f32f5-188">Odvolat `GetById` vytvořili `"GetTodo"` s názvem trasy:</span><span class="sxs-lookup"><span data-stu-id="f32f5-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="f32f5-189">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="f32f5-189">Update</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f32f5-190">`Update` je podobná `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f32f5-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f32f5-191">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f32f5-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f32f5-192">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="f32f5-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f32f5-193">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="f32f5-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f32f5-195">Odstranit</span><span class="sxs-lookup"><span data-stu-id="f32f5-195">Delete</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f32f5-196">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f32f5-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="f32f5-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f32f5-198">Next steps</span></span>

* [<span data-ttu-id="f32f5-199">Směrování do akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="f32f5-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="f32f5-200">Informace o nasazení rozhraní API najdete v tématu [hostitele a nasazení](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="f32f5-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="f32f5-201">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f32f5-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="f32f5-202">Postman</span><span class="sxs-lookup"><span data-stu-id="f32f5-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="f32f5-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="f32f5-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
