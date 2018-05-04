---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac
author: rick-anderson
description: Vytvoření webové rozhraní API s jádro ASP.NET MVC a Visual Studio pro Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 46050f4bbd6ae821c03d92c8750e839d491328cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="43efc-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="43efc-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="43efc-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="43efc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="43efc-106">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="43efc-106">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="43efc-107">Rozhraní není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="43efc-107">The UI isn't constructed.</span></span>

<span data-ttu-id="43efc-108">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="43efc-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="43efc-109">systému macOS: webové rozhraní API pomocí sady Visual Studio pro Mac (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="43efc-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="43efc-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="43efc-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="43efc-111">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="43efc-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="43efc-112">V tématu [Úvod do rozhraní ASP.NET MVC jádra v systému macOS nebo Linux](xref:tutorials/first-mvc-app-xplat/index) pro příklad, který používá trvalé databáze.</span><span class="sxs-lookup"><span data-stu-id="43efc-112">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43efc-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43efc-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="43efc-114">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="43efc-114">Create the project</span></span>

<span data-ttu-id="43efc-115">Ze sady Visual Studio, vyberte **soubor** > **nové řešení**.</span><span class="sxs-lookup"><span data-stu-id="43efc-115">From Visual Studio, select **File** > **New Solution**.</span></span>

![systému macOS nové řešení](first-web-api-mac/_static/sln.png)

<span data-ttu-id="43efc-117">Vyberte **aplikace .NET Core** > **ASP.NET Core webového rozhraní API** > **Další**.</span><span class="sxs-lookup"><span data-stu-id="43efc-117">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![systému macOS dialogové okno Nový projekt](first-web-api-mac/_static/1.png)

<span data-ttu-id="43efc-119">Zadejte *TodoApi* pro **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="43efc-119">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Dialogové okno Konfigurace](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="43efc-121">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="43efc-121">Launch the app</span></span>

<span data-ttu-id="43efc-122">V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="43efc-122">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="43efc-123">Visual Studio spustí prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43efc-123">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="43efc-124">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="43efc-124">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="43efc-125">Změňte adresu URL na `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="43efc-125">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="43efc-126">`ValuesController` Se zobrazí data:</span><span class="sxs-lookup"><span data-stu-id="43efc-126">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="43efc-127">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="43efc-127">Add support for Entity Framework Core</span></span>

<span data-ttu-id="43efc-128">Nainstalujte [Entity Framework Core InMemory](/ef/core/providers/in-memory/) zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="43efc-128">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="43efc-129">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="43efc-129">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="43efc-130">Z **projektu** nabídce vyberte možnost **přidání balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="43efc-130">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="43efc-131">Alternativně můžete můžete kliknout pravým tlačítkem **závislosti**a potom vyberte **přidat balíčky**.</span><span class="sxs-lookup"><span data-stu-id="43efc-131">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="43efc-132">Zadejte `EntityFrameworkCore.InMemory` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="43efc-132">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="43efc-133">Vyberte `Microsoft.EntityFrameworkCore.InMemory`a potom vyberte **přidat balíček**.</span><span class="sxs-lookup"><span data-stu-id="43efc-133">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="43efc-134">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="43efc-134">Add a model class</span></span>

<span data-ttu-id="43efc-135">Model je objekt reprezentující data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="43efc-135">A model is an object representing the data in your app.</span></span> <span data-ttu-id="43efc-136">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="43efc-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="43efc-137">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="43efc-137">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="43efc-138">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="43efc-138">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="43efc-139">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="43efc-139">Name the folder *Models*.</span></span>

![Nová složka](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="43efc-141">Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="43efc-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="43efc-142">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **nový soubor** > **Obecné**  >  **Prázdná třída**.</span><span class="sxs-lookup"><span data-stu-id="43efc-142">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="43efc-143">Název třídy *TodoItem*a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="43efc-143">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="43efc-144">Nahraďte generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="43efc-144">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="43efc-145">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="43efc-145">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="43efc-146">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="43efc-146">Create the database context</span></span>

<span data-ttu-id="43efc-147">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="43efc-147">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="43efc-148">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="43efc-148">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="43efc-149">Přidat `TodoContext` třídy k *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="43efc-149">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="43efc-150">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="43efc-150">Add a controller</span></span>

<span data-ttu-id="43efc-151">V Průzkumníku řešení v *řadiče* složky, přidejte třídu `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="43efc-151">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="43efc-152">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="43efc-152">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="43efc-153">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="43efc-153">Launch the app</span></span>

<span data-ttu-id="43efc-154">V sadě Visual Studio, vyberte **spustit** > **spustit s ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="43efc-154">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="43efc-155">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>`, kde `<port>` je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="43efc-155">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="43efc-156">Zobrazí chyba HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="43efc-156">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="43efc-157">Změňte adresu URL na `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="43efc-157">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="43efc-158">`ValuesController` Se zobrazí data:</span><span class="sxs-lookup"><span data-stu-id="43efc-158">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="43efc-159">Přejděte na `Todo` ovladač na `http://localhost:<port>/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="43efc-159">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="43efc-160">Implementace dalších operací CRUD</span><span class="sxs-lookup"><span data-stu-id="43efc-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="43efc-161">Přidáme `Create`, `Update`, a `Delete` metody pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="43efc-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="43efc-162">Tyto metody jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="43efc-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="43efc-163">Sestavení projektu po přidání nebo změna kódu.</span><span class="sxs-lookup"><span data-stu-id="43efc-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="43efc-164">Create</span><span class="sxs-lookup"><span data-stu-id="43efc-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43efc-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="43efc-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="43efc-166">Předchozí postup odpoví na HTTP POST, jak [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="43efc-166">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="43efc-167">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="43efc-167">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43efc-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="43efc-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="43efc-169">Předchozí postup odpoví na HTTP POST, jak [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="43efc-169">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="43efc-170">MVC získá hodnotu položky úkolů z textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="43efc-170">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="43efc-171">`CreatedAtRoute` Metoda vrátí 201 odpověď.</span><span class="sxs-lookup"><span data-stu-id="43efc-171">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="43efc-172">Je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.</span><span class="sxs-lookup"><span data-stu-id="43efc-172">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="43efc-173">`CreatedAtRoute` také přidá do odpovědi hlavičku umístění.</span><span class="sxs-lookup"><span data-stu-id="43efc-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="43efc-174">Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů.</span><span class="sxs-lookup"><span data-stu-id="43efc-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="43efc-175">V tématu [10.2.2 201 vytvořit](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="43efc-175">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="43efc-176">Použití Postman k odeslání požadavku na vytvořit</span><span class="sxs-lookup"><span data-stu-id="43efc-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="43efc-177">Spusťte aplikaci (**spustit** > **začínat ladění**).</span><span class="sxs-lookup"><span data-stu-id="43efc-177">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="43efc-178">Otevřete Postman.</span><span class="sxs-lookup"><span data-stu-id="43efc-178">Open Postman.</span></span>

![Konzole postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="43efc-180">Aktualizujte číslo portu v adrese URL místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="43efc-180">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="43efc-181">Nastavte jako metodu HTTP *POST*.</span><span class="sxs-lookup"><span data-stu-id="43efc-181">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="43efc-182">Klikněte **textu** kartě.</span><span class="sxs-lookup"><span data-stu-id="43efc-182">Click the **Body** tab.</span></span>
* <span data-ttu-id="43efc-183">Vyberte **nezpracovaná** přepínač.</span><span class="sxs-lookup"><span data-stu-id="43efc-183">Select the **raw** radio button.</span></span>
* <span data-ttu-id="43efc-184">Nastavte typ na *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="43efc-184">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="43efc-185">Zadejte obsah žádosti s úkol podobné následujícím kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="43efc-185">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="43efc-186">Klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="43efc-186">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="43efc-187">Pokud žádná odpověď zobrazí po kliknutí na **odeslat**, zakažte **SSL certifikační ověření** možnost.</span><span class="sxs-lookup"><span data-stu-id="43efc-187">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="43efc-188">To je v části **soubor** > **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="43efc-188">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="43efc-189">Klikněte **odeslat** tlačítko znovu po zakázání nastavení.</span><span class="sxs-lookup"><span data-stu-id="43efc-189">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="43efc-190">Klikněte na tlačítko **hlavičky** ve **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:</span><span class="sxs-lookup"><span data-stu-id="43efc-190">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Karta hlavičky konzoly Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="43efc-192">Hlavička umístění URI můžete použít pro přístup k prostředku, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="43efc-192">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="43efc-193">`Create` Metoda vrátí [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="43efc-193">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="43efc-194">První parametr předaný `CreatedAtRoute` představuje pojmenovanou trasu sloužící ke generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="43efc-194">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="43efc-195">Odvolat, který `GetById` vytvořili `"GetTodo"` s názvem trasy:</span><span class="sxs-lookup"><span data-stu-id="43efc-195">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="43efc-196">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="43efc-196">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43efc-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="43efc-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43efc-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="43efc-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="43efc-199">`Update` je podobná `Create`, ale používá HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="43efc-199">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="43efc-200">Odpověď [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="43efc-200">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="43efc-201">Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly.</span><span class="sxs-lookup"><span data-stu-id="43efc-201">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="43efc-202">Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="43efc-202">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="43efc-204">Odstranit</span><span class="sxs-lookup"><span data-stu-id="43efc-204">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="43efc-205">Odpověď [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="43efc-205">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
