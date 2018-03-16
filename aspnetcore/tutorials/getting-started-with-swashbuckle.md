---
title: "Začínáme s Swashbuckle"
author: zuckerthoben
description: "V tomto kurzu poskytuje návod k přidávání Swashbuckle do projektu integrovat uživatelské rozhraní Swagger."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 148ddbebab9afa4d9c78d3bfca5526dbbd8a5ecb
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-swashbuckle"></a><span data-ttu-id="50d7d-103">Začínáme s Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="50d7d-103">Get started with Swashbuckle</span></span>

<span data-ttu-id="50d7d-104">Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="50d7d-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="50d7d-105">Existují tři hlavní komponenty k Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="50d7d-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="50d7d-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.</span><span class="sxs-lookup"><span data-stu-id="50d7d-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="50d7d-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely.</span><span class="sxs-lookup"><span data-stu-id="50d7d-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="50d7d-108">Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="50d7d-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="50d7d-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): embedded verzi nástroj uživatelské rozhraní Swagger, který interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="50d7d-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="50d7d-110">Zahrnuje integrovanou testovací nevodivou pro veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="50d7d-110">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="50d7d-111">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="50d7d-111">Package installation</span></span>

<span data-ttu-id="50d7d-112">Swashbuckle lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="50d7d-112">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="50d7d-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50d7d-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="50d7d-114">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="50d7d-114">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="50d7d-115">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="50d7d-115">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="50d7d-116">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="50d7d-116">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="50d7d-117">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="50d7d-117">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="50d7d-118">Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="50d7d-118">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="50d7d-119">Vyberte balíček "Swashbuckle.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="50d7d-119">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="50d7d-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="50d7d-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="50d7d-121">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="50d7d-121">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="50d7d-122">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="50d7d-122">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="50d7d-123">Do vyhledávacího pole zadejte Swashbuckle.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="50d7d-123">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="50d7d-124">Vyberte balíček Swashbuckle.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="50d7d-124">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="50d7d-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="50d7d-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="50d7d-126">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="50d7d-126">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="50d7d-127">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="50d7d-127">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="50d7d-128">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d7d-128">Run the following command:</span></span>

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="50d7d-129">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="50d7d-129">Add and configure Swagger middleware</span></span>

<span data-ttu-id="50d7d-130">Přidání generátoru Swagger ke kolekci služby v `Startup.ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="50d7d-130">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="50d7d-131">Importovat následující obory názvů v `Info` třídy:</span><span class="sxs-lookup"><span data-stu-id="50d7d-131">Import the following namespaces in the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="50d7d-132">V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného dokumentu JSON a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="50d7d-132">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="50d7d-133">Spusťte aplikaci a přejděte na `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="50d7d-133">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="50d7d-134">Vygenerovaný dokument popisující koncové body se zobrazí, jak je znázorněno v [Swagger specifikace (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="50d7d-134">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="50d7d-135">Uživatelské rozhraní Swagger naleznete na adrese `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="50d7d-135">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="50d7d-136">Nyní vám umožní zkoumat rozhraní API přes uživatelské rozhraní Swagger a začlenit v jiných aplikacích.</span><span class="sxs-lookup"><span data-stu-id="50d7d-136">Now you can explore the API via Swagger UI and incorporate it in other programs.</span></span>

## <a name="customize--extend"></a><span data-ttu-id="50d7d-137">Přizpůsobit a rozšířit</span><span class="sxs-lookup"><span data-stu-id="50d7d-137">Customize & extend</span></span>

<span data-ttu-id="50d7d-138">Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.</span><span class="sxs-lookup"><span data-stu-id="50d7d-138">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="50d7d-139">Informace o rozhraní API a popis</span><span class="sxs-lookup"><span data-stu-id="50d7d-139">API info and description</span></span>

<span data-ttu-id="50d7d-140">Akce konfigurace předaný `AddSwaggerGen` metoda slouží k přidání informací jako autor, licence a popis:</span><span class="sxs-lookup"><span data-stu-id="50d7d-140">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?range=20-30,36)]

<span data-ttu-id="50d7d-141">Následující obrázek znázorňuje rozhraní Swagger, zobrazení informací o verzi:</span><span class="sxs-lookup"><span data-stu-id="50d7d-141">The following image depicts the Swagger UI displaying the version information:</span></span>

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="50d7d-143">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="50d7d-143">XML comments</span></span>

<span data-ttu-id="50d7d-144">XML – komentáře lze je aktivovat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="50d7d-144">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="50d7d-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50d7d-145">Visual Studio</span></span>](#tab/visual-studio-xml)

* <span data-ttu-id="50d7d-146">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="50d7d-146">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="50d7d-147">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:</span><span class="sxs-lookup"><span data-stu-id="50d7d-147">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="50d7d-149">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="50d7d-149">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml)

* <span data-ttu-id="50d7d-150">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="50d7d-150">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="50d7d-151">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:</span><span class="sxs-lookup"><span data-stu-id="50d7d-151">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="50d7d-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="50d7d-153">Visual Studio Code</span></span>](#tab/visual-studio-code-xml)

<span data-ttu-id="50d7d-154">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="50d7d-154">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?range=7-9)]

---

<span data-ttu-id="50d7d-155">Povolení komentáře XML poskytuje informace o ladění pro nedokumentovanými veřejných typů a členů.</span><span class="sxs-lookup"><span data-stu-id="50d7d-155">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="50d7d-156">Upozornění jsou označeny nedokumentovanými typy a členy.</span><span class="sxs-lookup"><span data-stu-id="50d7d-156">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="50d7d-157">Například následující zpráva označuje porušení upozornění kód. 1591:</span><span class="sxs-lookup"><span data-stu-id="50d7d-157">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="50d7d-158">Potlačení upozornění definováním seznam kódů upozornění ignorovat v oddělených středníky *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="50d7d-158">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="50d7d-159">Nakonfigurujte Swagger používat generovaný soubor XML.</span><span class="sxs-lookup"><span data-stu-id="50d7d-159">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="50d7d-160">Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů.</span><span class="sxs-lookup"><span data-stu-id="50d7d-160">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="50d7d-161">Například *TodoApi.Swashbuckle.XML* je soubor na systém Windows, ale CentOS není platný.</span><span class="sxs-lookup"><span data-stu-id="50d7d-161">For example, a *TodoApi.Swashbuckle.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="50d7d-162">V předchozí kód `ApplicationBasePath` získá základní cesta aplikace.</span><span class="sxs-lookup"><span data-stu-id="50d7d-162">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="50d7d-163">Základní cesta se používá k vyhledání souboru komentáře XML.</span><span class="sxs-lookup"><span data-stu-id="50d7d-163">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="50d7d-164">*TodoApi.Swashbuckle.xml* funguje pouze v tomto příkladu vzhledem k tomu, že název generovaného kódu XML komentáře soubor podle názvu aplikace.</span><span class="sxs-lookup"><span data-stu-id="50d7d-164">*TodoApi.Swashbuckle.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="50d7d-165">Přidávání komentářů triple lomítko metodě vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části:</span><span class="sxs-lookup"><span data-stu-id="50d7d-165">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Zobrazuje komentáře XML odstraní konkrétní TodoItem. uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="50d7d-168">Vygenerovaný soubor JSON, který také obsahuje tyto komentáře doprovází uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="50d7d-168">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="50d7d-169">Přidat [ \<Poznámky >](/dotnet/csharp/programming-guide/xmldoc/remarks) označení pro `Create` dokumentace metoda akce.</span><span class="sxs-lookup"><span data-stu-id="50d7d-169">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="50d7d-170">Doplňuje informace uvedené v [ \<souhrnné >](/dotnet/csharp/programming-guide/xmldoc/summary) značky a poskytuje robustnější uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="50d7d-170">It supplements information specified in the [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="50d7d-171">`<remarks>` Obsah značky se může skládat z textu JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="50d7d-171">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="50d7d-172">Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři.</span><span class="sxs-lookup"><span data-stu-id="50d7d-172">Notice the UI enhancements with these additional comments.</span></span>

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="50d7d-174">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="50d7d-174">Data annotations</span></span>

<span data-ttu-id="50d7d-175">Uspořádání model s atributy, které jsou součástí [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů, k rozvoji součásti uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="50d7d-175">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="50d7d-176">Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:</span><span class="sxs-lookup"><span data-stu-id="50d7d-176">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="50d7d-177">Přítomnost tento atribut se změní chování uživatelského rozhraní a mění základní schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="50d7d-177">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="50d7d-178">Přidat `[Produces("application/json")]` atribut kontroleru rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="50d7d-178">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="50d7d-179">Jejím účelem je deklarovat, že akce kontroleru podporují typ obsahu odpovědi z *application/json*:</span><span class="sxs-lookup"><span data-stu-id="50d7d-179">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="50d7d-180">**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:</span><span class="sxs-lookup"><span data-stu-id="50d7d-180">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="50d7d-182">Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.</span><span class="sxs-lookup"><span data-stu-id="50d7d-182">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="50d7d-183">Popisují typy odezvy</span><span class="sxs-lookup"><span data-stu-id="50d7d-183">Describe response types</span></span>

<span data-ttu-id="50d7d-184">Využívání vývojáři jsou nejvíce zajímají co je vrácen&mdash;konkrétně odpovědi typy a kódy chyb (Pokud není standard).</span><span class="sxs-lookup"><span data-stu-id="50d7d-184">Consuming developers are most concerned with what is returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="50d7d-185">Tyto jsou zpracovávány v XML komentáře a data poznámky.</span><span class="sxs-lookup"><span data-stu-id="50d7d-185">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="50d7d-186">`Create` Akce vrátí `201 Created` v případě úspěchu nebo `400 Bad Request` při textu požadavku odeslaného má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="50d7d-186">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="50d7d-187">Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="50d7d-187">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="50d7d-188">Tento problém vyřešen přidáním zvýrazněné řádky v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="50d7d-188">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="50d7d-189">Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="50d7d-189">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="50d7d-191">Přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="50d7d-191">Customize the UI</span></span>

<span data-ttu-id="50d7d-192">Stock uživatelského rozhraní je funkční a přístupné; ale při vytváření stránky dokumentace pro vaše rozhraní API, chcete ho k reprezentaci značky nebo motiv.</span><span class="sxs-lookup"><span data-stu-id="50d7d-192">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="50d7d-193">Provedení této úlohy se součástmi Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a pak vytváření strukturu složek pro hostování těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="50d7d-193">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="50d7d-194">Pokud cílení na rozhraní .NET Framework nebo .NET Core 1.x, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) balíček NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="50d7d-194">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="50d7d-195">Předchozí balíček NuGet je již nainstalován, pokud cílení na .NET Core 2.x a pomocí [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="50d7d-195">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="50d7d-196">Povolte middleware statické soubory:</span><span class="sxs-lookup"><span data-stu-id="50d7d-196">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="50d7d-197">Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="50d7d-197">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="50d7d-198">Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="50d7d-198">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="50d7d-199">Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.</span><span class="sxs-lookup"><span data-stu-id="50d7d-199">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="50d7d-200">Vytvoření *wwwroot/swagger/ui/css/custom.css* soubor s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:</span><span class="sxs-lookup"><span data-stu-id="50d7d-200">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="50d7d-201">Referenční dokumentace *custom.css* v *index.html* souboru:</span><span class="sxs-lookup"><span data-stu-id="50d7d-201">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="50d7d-202">Vyhledejte *index.html* stránky v `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="50d7d-202">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="50d7d-203">Zadejte `http://localhost:<random_port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="50d7d-203">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="50d7d-204">Výsledná stránka vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="50d7d-204">The resulting page looks as follows:</span></span>

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="50d7d-206">Je mnohem víc, že můžete provést pomocí stránky.</span><span class="sxs-lookup"><span data-stu-id="50d7d-206">There's much more you can do with the page.</span></span> <span data-ttu-id="50d7d-207">Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="50d7d-207">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
