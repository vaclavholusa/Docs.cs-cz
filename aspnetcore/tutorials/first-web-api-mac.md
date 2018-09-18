---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac
author: rick-anderson
description: Vytvoření webového rozhraní API s ASP.NET Core MVC a sady Visual Studio pro Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011622"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="760e9-103">Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="760e9-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="760e9-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="760e9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="760e9-105">V tomto kurzu se vytvoření webového rozhraní API pro správu seznam "úkolů".</span><span class="sxs-lookup"><span data-stu-id="760e9-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="760e9-106">Není vytvořen uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="760e9-106">The UI isn't constructed.</span></span>

<span data-ttu-id="760e9-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="760e9-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="760e9-108">macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="760e9-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="760e9-109">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="760e9-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="760e9-110">macOS, Linux, Windows: [webového rozhraní API pomocí Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="760e9-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="760e9-111">Zobrazit [Úvod do ASP.NET Core MVC v systému macOS nebo Linux](xref:tutorials/first-mvc-app-xplat/index) příklad, který používá trvalé databáze.</span><span class="sxs-lookup"><span data-stu-id="760e9-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="760e9-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="760e9-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="760e9-113">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="760e9-113">Create the project</span></span>

<span data-ttu-id="760e9-114">Ze sady Visual Studio, vyberte **souboru** > **nové řešení**.</span><span class="sxs-lookup"><span data-stu-id="760e9-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![macOS nové řešení](first-web-api-mac/_static/sln.png)

<span data-ttu-id="760e9-116">Vyberte **aplikace .NET Core** > **webového rozhraní API ASP.NET Core** > **Další**.</span><span class="sxs-lookup"><span data-stu-id="760e9-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS dialogové okno nového projektu](first-web-api-mac/_static/1.png)

<span data-ttu-id="760e9-118">Zadejte *TodoApi* pro **název projektu**a potom klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="760e9-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="760e9-120">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="760e9-120">Launch the app</span></span>

<span data-ttu-id="760e9-121">V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="760e9-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="760e9-122">Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="760e9-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="760e9-123">Obdržíte chybu HTTP 404 (Nenalezeno).</span><span class="sxs-lookup"><span data-stu-id="760e9-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="760e9-124">Změňte adresu URL na `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="760e9-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="760e9-125">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="760e9-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="760e9-126">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="760e9-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="760e9-127">Nainstalujte [Entity Framework Core InMemory](/ef/core/providers/in-memory/) poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="760e9-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="760e9-128">Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="760e9-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="760e9-129">Z **projektu** nabídce vyberte možnost **přidat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="760e9-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="760e9-130">Alternativně je můžete kliknout pravým tlačítkem **závislosti**a pak vyberte **přidat balíčky**.</span><span class="sxs-lookup"><span data-stu-id="760e9-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="760e9-131">Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="760e9-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="760e9-132">Vyberte `Microsoft.EntityFrameworkCore.InMemory`a pak vyberte **přidat balíček**.</span><span class="sxs-lookup"><span data-stu-id="760e9-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="760e9-133">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="760e9-133">Add a model class</span></span>

<span data-ttu-id="760e9-134">Model je objekt, který reprezentuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="760e9-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="760e9-135">V tomto případě jediný model je položky úkolu.</span><span class="sxs-lookup"><span data-stu-id="760e9-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="760e9-136">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="760e9-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="760e9-137">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="760e9-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="760e9-138">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="760e9-138">Name the folder *Models*.</span></span>

![Nová složka](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="760e9-140">Třídy modelu můžete umístit kdekoli ve vašem projektu, ale *modely* složky používají konvence.</span><span class="sxs-lookup"><span data-stu-id="760e9-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="760e9-141">Klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat** > **nový soubor** > **Obecné**  >  **Prázdná třída**.</span><span class="sxs-lookup"><span data-stu-id="760e9-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="760e9-142">Název třídy *TodoItem*a potom klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="760e9-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="760e9-143">Nahraďte kód vygenerovaný pomocí:</span><span class="sxs-lookup"><span data-stu-id="760e9-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="760e9-144">Vytvoří databázi `Id` při `TodoItem` se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="760e9-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="760e9-145">Vytvořte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="760e9-145">Create the database context</span></span>

<span data-ttu-id="760e9-146">*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="760e9-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="760e9-147">Vytvoření této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="760e9-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="760e9-148">Přidat `TodoContext` třídu *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="760e9-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="760e9-149">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="760e9-149">Add a controller</span></span>

<span data-ttu-id="760e9-150">V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="760e9-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="760e9-151">Generovaného kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="760e9-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="760e9-152">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="760e9-152">Launch the app</span></span>

<span data-ttu-id="760e9-153">V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="760e9-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="760e9-154">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>`, kde `<port>` je číslo portu náhodně vybrané.</span><span class="sxs-lookup"><span data-stu-id="760e9-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="760e9-155">Obdržíte chybu HTTP 404 (Nenalezeno).</span><span class="sxs-lookup"><span data-stu-id="760e9-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="760e9-156">Změňte adresu URL na `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="760e9-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="760e9-157">`ValuesController` Data se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="760e9-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="760e9-158">Přejděte `Todo` ovladač na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="760e9-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="760e9-159">Vrátí následující JSON:</span><span class="sxs-lookup"><span data-stu-id="760e9-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="760e9-160">Provádění jiných operací CRUD</span><span class="sxs-lookup"><span data-stu-id="760e9-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="760e9-161">Přidáme `Create`, `Update`, a `Delete` metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="760e9-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="760e9-162">Tyto metody jsou varianty motiv, tak I budete jenom zobrazit kód a zvýraznit hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="760e9-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="760e9-163">Po přidání nebo změna kódu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="760e9-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="760e9-164">Create</span><span class="sxs-lookup"><span data-stu-id="760e9-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="760e9-165">Předchozí metoda odpovídá HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="760e9-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="760e9-166">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut oznamuje MVC k získání hodnoty položky seznamu úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="760e9-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="760e9-167">Předchozí metoda odpovídá HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="760e9-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="760e9-168">MVC získá hodnotu položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="760e9-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="760e9-169">`CreatedAtRoute` Metoda vrátí odezvě 201.</span><span class="sxs-lookup"><span data-stu-id="760e9-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="760e9-170">Je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="760e9-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="760e9-171">`CreatedAtRoute` také přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="760e9-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="760e9-172">Hlavička umístění Určuje identifikátor URI nově vytvořeného úkolu položky.</span><span class="sxs-lookup"><span data-stu-id="760e9-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="760e9-173">Zobrazit [10.2.2 201 vytvořili](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="760e9-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="760e9-174">Odeslat požadavek na vytvoření pomocí nástroje Postman</span><span class="sxs-lookup"><span data-stu-id="760e9-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="760e9-175">Spusťte aplikaci (**spustit** > **spustit s laděním**).</span><span class="sxs-lookup"><span data-stu-id="760e9-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="760e9-176">Otevřete nástroj Postman.</span><span class="sxs-lookup"><span data-stu-id="760e9-176">Open Postman.</span></span>

![Konzola nástroje postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="760e9-178">Aktualizujte číslo portu adresy URL místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="760e9-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="760e9-179">Nastavte jako metodu HTTP *příspěvek*.</span><span class="sxs-lookup"><span data-stu-id="760e9-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="760e9-180">Klikněte na tlačítko **tělo** kartu.</span><span class="sxs-lookup"><span data-stu-id="760e9-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="760e9-181">Vyberte **nezpracovaná** přepínač.</span><span class="sxs-lookup"><span data-stu-id="760e9-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="760e9-182">Nastavte typ *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="760e9-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="760e9-183">Zadejte text požadavku s položku úkolu připomínající následující JSON:</span><span class="sxs-lookup"><span data-stu-id="760e9-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="760e9-184">Klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="760e9-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="760e9-185">Pokud žádná odpověď zobrazí po kliknutí na tlačítko **odeslat**, zakažte **ověření certifikace SSL** možnost.</span><span class="sxs-lookup"><span data-stu-id="760e9-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="760e9-186">Se nachází v části **souboru** > **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="760e9-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="760e9-187">Klikněte na tlačítko **odeslat** tlačítko znovu po zakázání nastavení.</span><span class="sxs-lookup"><span data-stu-id="760e9-187">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="760e9-188">Klikněte na tlačítko **záhlaví** kartu **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:</span><span class="sxs-lookup"><span data-stu-id="760e9-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta hlavičky z konzoly nástroje Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="760e9-190">Hlavička umístění URI můžete použít pro přístup k prostředku, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="760e9-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="760e9-191">`Create` Vrátí metoda [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="760e9-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="760e9-192">První parametr předána `CreatedAtRoute` představuje pojmenovanou trasu sloužící ke generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="760e9-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="760e9-193">Vzpomeňte si, že `GetById` metoda vytvořené `"GetTodo"` trasy s názvem:</span><span class="sxs-lookup"><span data-stu-id="760e9-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="760e9-194">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="760e9-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="760e9-195">`Update` je podobný `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="760e9-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="760e9-196">Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="760e9-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="760e9-197">Podle specifikace HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovaná entita, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="760e9-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="760e9-198">Pro podporu částečné aktualizace, pomocí HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="760e9-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="760e9-200">Odstranit</span><span class="sxs-lookup"><span data-stu-id="760e9-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="760e9-201">Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="760e9-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
