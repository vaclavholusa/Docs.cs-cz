---
title: Začínáme s Swashbuckle a ASP.NET Core
author: zuckerthoben
description: Zjistěte, jak přidat do projektu ASP.NET Core integrovat uživatelské rozhraní Swagger Swashbuckle.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="b123f-103">Začínáme s Swashbuckle a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b123f-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="b123f-104">Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="b123f-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="b123f-105">Existují tři hlavní komponenty k Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="b123f-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="b123f-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.</span><span class="sxs-lookup"><span data-stu-id="b123f-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="b123f-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely.</span><span class="sxs-lookup"><span data-stu-id="b123f-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="b123f-108">Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="b123f-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="b123f-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): embedded verzi nástroje uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="b123f-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="b123f-110">Interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b123f-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="b123f-111">Zahrnuje integrovanou testovací nevodivou pro veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="b123f-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="b123f-112">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="b123f-112">Package installation</span></span>

<span data-ttu-id="b123f-113">Swashbuckle lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="b123f-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b123f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b123f-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b123f-115">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="b123f-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="b123f-116">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b123f-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="b123f-117">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="b123f-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="b123f-118">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="b123f-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="b123f-119">Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="b123f-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="b123f-120">Vyberte balíček "Swashbuckle.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="b123f-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b123f-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b123f-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b123f-122">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="b123f-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="b123f-123">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="b123f-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="b123f-124">Do vyhledávacího pole zadejte Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="b123f-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="b123f-125">Vyberte balíček Swashbuckle.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="b123f-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b123f-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b123f-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b123f-127">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="b123f-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b123f-128">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="b123f-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b123f-129">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b123f-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="b123f-130">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="b123f-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="b123f-131">Přidání generátoru Swagger ke kolekci služby v `Startup.ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="b123f-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="b123f-132">Importovat následující názvů použitý `Info` třídy:</span><span class="sxs-lookup"><span data-stu-id="b123f-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="b123f-133">V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného dokumentu JSON a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="b123f-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="b123f-134">Spusťte aplikaci a přejděte na `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="b123f-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="b123f-135">Vygenerovaný dokument popisující koncové body se zobrazí, jak je znázorněno v [Swagger specifikace (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="b123f-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="b123f-136">Uživatelské rozhraní Swagger naleznete na adrese `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="b123f-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="b123f-137">Prozkoumejte rozhraní API přes uživatelské rozhraní Swagger a začlenit v jiných aplikacích.</span><span class="sxs-lookup"><span data-stu-id="b123f-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="b123f-138">K obsluze rozhraní Swagger, v kořenové aplikace (`http://localhost:<random_port>/`), můžete nastavit `RoutePrefix` vlastnost prázdný řetězec:</span><span class="sxs-lookup"><span data-stu-id="b123f-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="b123f-139">Přizpůsobit a rozšířit</span><span class="sxs-lookup"><span data-stu-id="b123f-139">Customize & extend</span></span>

<span data-ttu-id="b123f-140">Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.</span><span class="sxs-lookup"><span data-stu-id="b123f-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="b123f-141">Informace o rozhraní API a popis</span><span class="sxs-lookup"><span data-stu-id="b123f-141">API info and description</span></span>

<span data-ttu-id="b123f-142">Akce konfigurace předaný `AddSwaggerGen` metoda přidá informace, jako autor, licence a popis:</span><span class="sxs-lookup"><span data-stu-id="b123f-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="b123f-143">Uživatelské rozhraní Swagger zobrazí informace na verzi:</span><span class="sxs-lookup"><span data-stu-id="b123f-143">The Swagger UI displays the version's information:</span></span>

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="b123f-145">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="b123f-145">XML comments</span></span>

<span data-ttu-id="b123f-146">XML – komentáře lze je aktivovat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="b123f-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="b123f-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b123f-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="b123f-148">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="b123f-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="b123f-149">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:</span><span class="sxs-lookup"><span data-stu-id="b123f-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="b123f-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b123f-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="b123f-152">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="b123f-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="b123f-153">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:</span><span class="sxs-lookup"><span data-stu-id="b123f-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="b123f-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b123f-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="b123f-156">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="b123f-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="b123f-157">Povolení komentáře XML poskytuje informace o ladění pro nedokumentovanými veřejných typů a členů.</span><span class="sxs-lookup"><span data-stu-id="b123f-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="b123f-158">Upozornění jsou označeny nedokumentovanými typy a členy.</span><span class="sxs-lookup"><span data-stu-id="b123f-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="b123f-159">Například následující zpráva označuje porušení upozornění kód. 1591:</span><span class="sxs-lookup"><span data-stu-id="b123f-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="b123f-160">Potlačení upozornění definováním seznam kódů upozornění ignorovat v oddělených středníky *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="b123f-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="b123f-161">Nakonfigurujte Swagger používat generovaný soubor XML.</span><span class="sxs-lookup"><span data-stu-id="b123f-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="b123f-162">Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů.</span><span class="sxs-lookup"><span data-stu-id="b123f-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="b123f-163">Například *TodoApi.XML* je soubor na systém Windows, ale CentOS není platný.</span><span class="sxs-lookup"><span data-stu-id="b123f-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="b123f-164">V předchozí kód [reflexe](/dotnet/csharp/programming-guide/concepts/reflection) slouží k vytvoření, projekt webového rozhraní API odpovídající název souboru XML.</span><span class="sxs-lookup"><span data-stu-id="b123f-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="b123f-165">Tento přístup zajišťuje, že generovaný název souboru XML odpovídá názvu projektu.</span><span class="sxs-lookup"><span data-stu-id="b123f-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="b123f-166">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) vlastnost se používá pro konstrukci cestu k souboru XML.</span><span class="sxs-lookup"><span data-stu-id="b123f-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="b123f-167">Přidávání komentářů triple lomítko na akci vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části.</span><span class="sxs-lookup"><span data-stu-id="b123f-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="b123f-168">Přidat [ \<souhrnné >](/dotnet/csharp/programming-guide/xmldoc/summary) element výše `Delete` akce:</span><span class="sxs-lookup"><span data-stu-id="b123f-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="b123f-169">Vnitřní text předchozí kód zobrazí uživatelské rozhraní Swagger `<summary>` element:</span><span class="sxs-lookup"><span data-stu-id="b123f-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Zobrazuje komentáře XML odstraní konkrétní TodoItem. uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="b123f-172">Uživatelské rozhraní vycházejí z generovaného schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="b123f-172">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="b123f-173">Přidat [ \<Poznámky >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu, který chcete `Create` dokumentace metoda akce.</span><span class="sxs-lookup"><span data-stu-id="b123f-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="b123f-174">Doplňuje informace uvedené v `<summary>` elementu a poskytuje robustnější uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="b123f-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="b123f-175">`<remarks>` Obsah elementu se může skládat z textu JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="b123f-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="b123f-176">Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři:</span><span class="sxs-lookup"><span data-stu-id="b123f-176">Notice the UI enhancements with these additional comments:</span></span>

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="b123f-178">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="b123f-178">Data annotations</span></span>

<span data-ttu-id="b123f-179">Uspořádání model s atributy, které jsou součástí [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů, k rozvoji součásti uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="b123f-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="b123f-180">Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:</span><span class="sxs-lookup"><span data-stu-id="b123f-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="b123f-181">Přítomnost tento atribut se změní chování uživatelského rozhraní a mění základní schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="b123f-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="b123f-182">Přidat `[Produces("application/json")]` atribut kontroleru rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b123f-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="b123f-183">Jejím účelem je deklarovat, že akce kontroleru podporují typ obsahu odpovědi z *application/json*:</span><span class="sxs-lookup"><span data-stu-id="b123f-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="b123f-184">**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:</span><span class="sxs-lookup"><span data-stu-id="b123f-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="b123f-186">Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.</span><span class="sxs-lookup"><span data-stu-id="b123f-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="b123f-187">Popisují typy odezvy</span><span class="sxs-lookup"><span data-stu-id="b123f-187">Describe response types</span></span>

<span data-ttu-id="b123f-188">Využívání vývojáři jsou nejvíce zajímají co je vrácen&mdash;konkrétně odpovědi typy a kódy chyb (Pokud není standard).</span><span class="sxs-lookup"><span data-stu-id="b123f-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="b123f-189">V XML komentáře a data poznámky jsou rozlišeny odpovědi typy a kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="b123f-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="b123f-190">`Create` Akce vrátí kód stavu HTTP 201 v případě úspěchu.</span><span class="sxs-lookup"><span data-stu-id="b123f-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="b123f-191">Stavový kód HTTP 400 se vrátí při textu požadavku odeslaného má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="b123f-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="b123f-192">Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="b123f-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="b123f-193">Tento problém můžete vyřešte přidáním zvýrazněné řádky v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b123f-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="b123f-194">Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="b123f-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="b123f-196">Přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b123f-196">Customize the UI</span></span>

<span data-ttu-id="b123f-197">Stock uživatelského rozhraní je funkční a přístupné.</span><span class="sxs-lookup"><span data-stu-id="b123f-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="b123f-198">Ale stránky dokumentace k rozhraní API by měl představovat značku nebo motivu.</span><span class="sxs-lookup"><span data-stu-id="b123f-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="b123f-199">Branding komponenty Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a sestavování strukturu složek pro hostování těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="b123f-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="b123f-200">Pokud cílení na rozhraní .NET Framework nebo .NET Core 1.x, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) balíček NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="b123f-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="b123f-201">Předchozí balíček NuGet je již nainstalován, pokud cílení na .NET Core 2.x a pomocí [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b123f-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="b123f-202">Povolte middleware statické soubory:</span><span class="sxs-lookup"><span data-stu-id="b123f-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="b123f-203">Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="b123f-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="b123f-204">Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="b123f-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="b123f-205">Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.</span><span class="sxs-lookup"><span data-stu-id="b123f-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="b123f-206">Vytvoření *custom.css* v souboru *wwwroot/swagger/uživatelského rozhraní*, s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:</span><span class="sxs-lookup"><span data-stu-id="b123f-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="b123f-207">Referenční dokumentace *custom.css* v *index.html* souboru po další soubory šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="b123f-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="b123f-208">Vyhledejte *index.html* stránky v `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="b123f-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="b123f-209">Zadejte `http://localhost:<random_port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b123f-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="b123f-210">Výsledná stránka vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b123f-210">The resulting page looks as follows:</span></span>

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="b123f-212">Je mnohem víc, že můžete provést pomocí stránky.</span><span class="sxs-lookup"><span data-stu-id="b123f-212">There's much more you can do with the page.</span></span> <span data-ttu-id="b123f-213">Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="b123f-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
