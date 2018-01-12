---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac"
description: "Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: "Jádro ASP.NET WebAPI, webového rozhraní API, REST, mac, systému macOS, HTTP, služby, služba HTTP"
manager: wpickett
ms.openlocfilehash: 3f84fa55baf6d808185a28db290d9e6d3c46bdac
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="5e3dc-104">Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="5e3dc-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="5e3dc-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="5e3dc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="5e3dc-106">V tomto kurzu vytvoříte webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="5e3dc-107">Nebude sestavení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-107">You won’t build a UI.</span></span>

<span data-ttu-id="5e3dc-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="5e3dc-109">systému macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="5e3dc-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="5e3dc-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="5e3dc-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="5e3dc-111">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="5e3dc-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="5e3dc-112">V tématu [Úvod do architektury ASP.NET MVC jádra na Mac nebo Linux](xref:tutorials/first-mvc-app-xplat/index) pro příklad, který používá trvalé databáze.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e3dc-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e3dc-113">Prerequisites</span></span>

<span data-ttu-id="5e3dc-114">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-114">Install the following:</span></span>

- <span data-ttu-id="5e3dc-115">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější</span><span class="sxs-lookup"><span data-stu-id="5e3dc-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="5e3dc-116">Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="5e3dc-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="5e3dc-117">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="5e3dc-117">Create the project</span></span>

<span data-ttu-id="5e3dc-118">Ze sady Visual Studio, vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-118">From Visual Studio, select **File > New Solution**.</span></span>

![systému macOS nové řešení](first-web-api-mac/_static/sln.png)

<span data-ttu-id="5e3dc-120">Vyberte **aplikace .NET Core > ASP.NET Core webového rozhraní API > Další**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![systému macOS dialogové okno Nový projekt](first-web-api-mac/_static/1.png)

<span data-ttu-id="5e3dc-122">Zadejte **TodoApi** pro **název projektu**a vyberte možnost vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="5e3dc-124">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="5e3dc-124">Launch the app</span></span>

<span data-ttu-id="5e3dc-125">V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5e3dc-126">Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="5e3dc-127">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="5e3dc-128">Změňte adresu URL na `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="5e3dc-129">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="5e3dc-130">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5e3dc-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="5e3dc-131">Nainstalujte [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="5e3dc-132">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="5e3dc-133">Z **projektu** nabídce vyberte možnost **přidání balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="5e3dc-134">Alternativně můžete můžete kliknout pravým tlačítkem **závislosti**a potom vyberte **přidat balíčky**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="5e3dc-135">Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="5e3dc-136">Vyberte `Microsoft.EntityFrameworkCore.InMemory`a potom vyberte **přidat balíček**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="5e3dc-137">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="5e3dc-137">Add a model class</span></span>

<span data-ttu-id="5e3dc-138">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="5e3dc-139">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="5e3dc-140">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-140">Add a folder named *Models*.</span></span> <span data-ttu-id="5e3dc-141">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="5e3dc-142">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5e3dc-143">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-143">Name the folder *Models*.</span></span>

![Nová složka](first-web-api-mac/_static/folder.png)

<span data-ttu-id="5e3dc-145">Poznámka: Můžete umístit třídy modelu kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="5e3dc-146">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="5e3dc-147">Klikněte pravým tlačítkem myši *modely* složky a vyberte **Přidat > Nový soubor > Obecné > prázdné třídy**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="5e3dc-148">Název třídy `TodoItem`a potom vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="5e3dc-149">Nahraďte generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-149">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="5e3dc-150">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-150">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="5e3dc-151">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="5e3dc-151">Create the database context</span></span>

<span data-ttu-id="5e3dc-152">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-152">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="5e3dc-153">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-153">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="5e3dc-154">Přidat `TodoContext` třídy k *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-154">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="5e3dc-155">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="5e3dc-155">Add a controller</span></span>

<span data-ttu-id="5e3dc-156">V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-156">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="5e3dc-157">Nahradit následující generovaný kód (a přidejte uzavírací závorky):</span><span class="sxs-lookup"><span data-stu-id="5e3dc-157">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="5e3dc-158">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="5e3dc-158">Launch the app</span></span>

<span data-ttu-id="5e3dc-159">V sadě Visual Studio, vyberte **spustit > spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-159">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5e3dc-160">Visual Studio spustí prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-160">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="5e3dc-161">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-161">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="5e3dc-162">Změňte adresu URL na `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-162">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="5e3dc-163">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-163">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="5e3dc-164">Přejděte na `Todo` ovladač na`http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-164">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5e3dc-165">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="5e3dc-165">Implement the other CRUD operations</span></span>

<span data-ttu-id="5e3dc-166">Přidáme `Create`, `Update`, a `Delete` metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-166">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="5e3dc-167">Tyto jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-167">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="5e3dc-168">Sestavení projektu po přidání nebo změna kódu.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-168">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="5e3dc-169">Create</span><span class="sxs-lookup"><span data-stu-id="5e3dc-169">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5e3dc-170">Toto je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-170">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5e3dc-171">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-171">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="5e3dc-172">`CreatedAtRoute` Metoda vrátí odpověď 201, což je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-172">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="5e3dc-173">`CreatedAtRoute`také přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="5e3dc-174">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5e3dc-175">V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-175">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5e3dc-176">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="5e3dc-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="5e3dc-177">Spusťte aplikaci (**spustit > spustit s laděním**).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-177">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="5e3dc-178">Spusťte Postman.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-178">Start Postman.</span></span>

![Konzole postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="5e3dc-180">Nastavte jako metodu HTTP`POST`</span><span class="sxs-lookup"><span data-stu-id="5e3dc-180">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="5e3dc-181">Vyberte **textu** přepínač</span><span class="sxs-lookup"><span data-stu-id="5e3dc-181">Select the **Body** radio button</span></span>
* <span data-ttu-id="5e3dc-182">Vyberte **nezpracovaná** přepínač</span><span class="sxs-lookup"><span data-stu-id="5e3dc-182">Select the **raw** radio button</span></span>
* <span data-ttu-id="5e3dc-183">Nastavte typ do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="5e3dc-183">Set the type to JSON</span></span>
* <span data-ttu-id="5e3dc-184">V editoru klíč hodnota zadejte položky Todo</span><span class="sxs-lookup"><span data-stu-id="5e3dc-184">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="5e3dc-185">Vyberte **odeslat**</span><span class="sxs-lookup"><span data-stu-id="5e3dc-185">Select **Send**</span></span>

* <span data-ttu-id="5e3dc-186">Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-186">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Karta hlavičky konzoly Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="5e3dc-188">Hlavička umístění URI můžete použít pro přístup k prostředku, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-188">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="5e3dc-189">Odvolat `GetById` vytvořili `"GetTodo"` s názvem trasy:</span><span class="sxs-lookup"><span data-stu-id="5e3dc-189">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="5e3dc-190">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="5e3dc-190">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="5e3dc-191">`Update`je podobná `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-191">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="5e3dc-192">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-192">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5e3dc-193">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-193">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5e3dc-194">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="5e3dc-194">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5e3dc-196">Odstranit</span><span class="sxs-lookup"><span data-stu-id="5e3dc-196">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5e3dc-197">Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-197">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="5e3dc-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e3dc-199">Next steps</span></span>

* [<span data-ttu-id="5e3dc-200">Směrování do akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="5e3dc-200">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="5e3dc-201">Informace o nasazení rozhraní API najdete v tématu [hostitele a nasazení](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="5e3dc-201">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="5e3dc-202">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e3dc-202">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="5e3dc-203">Postman</span><span class="sxs-lookup"><span data-stu-id="5e3dc-203">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="5e3dc-204">Fiddler</span><span class="sxs-lookup"><span data-stu-id="5e3dc-204">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
