---
title: Začínáme s Swashbuckle a ASP.NET Core
author: zuckerthoben
description: Zjistěte, jak přidat do projektu ASP.NET Core webové rozhraní API pro integraci uživatelské rozhraní Swagger Swashbuckle.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: eaeb27903c462ef002edbb0b84cd5a751db2bb9d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729727"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="22d92-103">Začínáme s Swashbuckle a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22d92-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="22d92-104">Podle [Shayne Boyer](https://twitter.com/spboyer) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="22d92-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="22d92-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22d92-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="22d92-106">Existují tři hlavní komponenty k Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="22d92-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="22d92-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger objektový model a middleware vystavit `SwaggerDocument` objekty jako koncové body JSON.</span><span class="sxs-lookup"><span data-stu-id="22d92-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="22d92-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): Generátor Swagger, který sestaví `SwaggerDocument` objekty přímo z vašeho tras, kontrolerů a modely.</span><span class="sxs-lookup"><span data-stu-id="22d92-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="22d92-109">Obvykle se zkombinuje s middlewarem koncového bodu Swaggeru automaticky vystavit JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="22d92-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="22d92-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): embedded verzi nástroje uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="22d92-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="22d92-111">Interpretuje JSON pro Swagger bohaté, přizpůsobitelné prostředí pro popisující funkce webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="22d92-111">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="22d92-112">Zahrnuje integrovanou testovací nevodivou pro veřejné metody.</span><span class="sxs-lookup"><span data-stu-id="22d92-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="22d92-113">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="22d92-113">Package installation</span></span>

<span data-ttu-id="22d92-114">Swashbuckle lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="22d92-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22d92-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22d92-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="22d92-116">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="22d92-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="22d92-117">Přejděte na **zobrazení** > **jinými** > **Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="22d92-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="22d92-118">Přejděte do adresáře, ve kterém *TodoApi.csproj* soubor existuje</span><span class="sxs-lookup"><span data-stu-id="22d92-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="22d92-119">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="22d92-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="22d92-120">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="22d92-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="22d92-121">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="22d92-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="22d92-122">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="22d92-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="22d92-123">Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="22d92-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="22d92-124">Vyberte balíček "Swashbuckle.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="22d92-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="22d92-125">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="22d92-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="22d92-126">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="22d92-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="22d92-127">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="22d92-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="22d92-128">Do vyhledávacího pole zadejte "Swashbuckle.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="22d92-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="22d92-129">V podokně výsledků vyberte balíček "Swashbuckle.AspNetCore" a klikněte na **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="22d92-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22d92-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22d92-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="22d92-131">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="22d92-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="22d92-132">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="22d92-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="22d92-133">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="22d92-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="22d92-134">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="22d92-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="22d92-135">Přidání generátoru Swagger ke kolekci služby v `Startup.ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="22d92-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="22d92-136">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="22d92-136">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22d92-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span><span class="sxs-lookup"><span data-stu-id="22d92-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span></span>

::: moniker-end

<span data-ttu-id="22d92-138">Importovat následující názvů použitý `Info` třídy:</span><span class="sxs-lookup"><span data-stu-id="22d92-138">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="22d92-139">V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného dokumentu JSON a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="22d92-139">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="22d92-140">Spusťte aplikaci a přejděte na `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="22d92-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="22d92-141">Vygenerovaný dokument popisující koncové body se zobrazí, jak je znázorněno v [Swagger specifikace (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="22d92-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="22d92-142">Uživatelské rozhraní Swagger naleznete na adrese `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="22d92-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="22d92-143">Prozkoumejte rozhraní API přes uživatelské rozhraní Swagger a začlenit v jiných aplikacích.</span><span class="sxs-lookup"><span data-stu-id="22d92-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="22d92-144">K obsluze rozhraní Swagger, v kořenové aplikace (`http://localhost:<port>/`), můžete nastavit `RoutePrefix` vlastnost prázdný řetězec:</span><span class="sxs-lookup"><span data-stu-id="22d92-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize-and-extend"></a><span data-ttu-id="22d92-145">Přizpůsobit a rozšířit</span><span class="sxs-lookup"><span data-stu-id="22d92-145">Customize and extend</span></span>

<span data-ttu-id="22d92-146">Swagger poskytuje možnosti pro dokumentaci v objektovém modelu a přizpůsobení uživatelského rozhraní tak, aby odpovídaly vaší motivu.</span><span class="sxs-lookup"><span data-stu-id="22d92-146">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="22d92-147">Informace o rozhraní API a popis</span><span class="sxs-lookup"><span data-stu-id="22d92-147">API info and description</span></span>

<span data-ttu-id="22d92-148">Akce konfigurace předaný `AddSwaggerGen` metoda přidá informace, jako autor, licence a popis:</span><span class="sxs-lookup"><span data-stu-id="22d92-148">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="22d92-149">Uživatelské rozhraní Swagger zobrazí informace na verzi:</span><span class="sxs-lookup"><span data-stu-id="22d92-149">The Swagger UI displays the version's information:</span></span>

![Uživatelské rozhraní swagger se informace o verzi: popis, vytvářet a další odkaz](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="22d92-151">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="22d92-151">XML comments</span></span>

<span data-ttu-id="22d92-152">XML – komentáře lze je aktivovat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="22d92-152">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="22d92-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22d92-153">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="22d92-154">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="22d92-154">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="22d92-155">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karta</span><span class="sxs-lookup"><span data-stu-id="22d92-155">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="22d92-156">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="22d92-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="22d92-157">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="22d92-157">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="22d92-158">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části</span><span class="sxs-lookup"><span data-stu-id="22d92-158">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="22d92-159">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22d92-159">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="22d92-160">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="22d92-160">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

---

<span data-ttu-id="22d92-161">Povolení komentáře XML poskytuje informace o ladění pro nedokumentovanými veřejných typů a členů.</span><span class="sxs-lookup"><span data-stu-id="22d92-161">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="22d92-162">Upozornění jsou označeny nedokumentovanými typy a členy.</span><span class="sxs-lookup"><span data-stu-id="22d92-162">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="22d92-163">Například následující zpráva označuje porušení upozornění kód. 1591:</span><span class="sxs-lookup"><span data-stu-id="22d92-163">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="22d92-164">Potlačení upozornění definováním seznam kódů upozornění ignorovat v oddělených středníky *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="22d92-164">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="22d92-165">Nakonfigurujte Swagger používat generovaný soubor XML.</span><span class="sxs-lookup"><span data-stu-id="22d92-165">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="22d92-166">Pro operační systémy jiný systém než Windows nebo Linux může být malá a velká písmena názvů a cest souborů.</span><span class="sxs-lookup"><span data-stu-id="22d92-166">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="22d92-167">Například *TodoApi.XML* je soubor na systém Windows, ale CentOS není platný.</span><span class="sxs-lookup"><span data-stu-id="22d92-167">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="22d92-168">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]</span><span class="sxs-lookup"><span data-stu-id="22d92-168">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="22d92-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span><span class="sxs-lookup"><span data-stu-id="22d92-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22d92-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span><span class="sxs-lookup"><span data-stu-id="22d92-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span></span>

::: moniker-end

<span data-ttu-id="22d92-171">V předchozí kód [reflexe](/dotnet/csharp/programming-guide/concepts/reflection) slouží k vytvoření, projekt webového rozhraní API odpovídající název souboru XML.</span><span class="sxs-lookup"><span data-stu-id="22d92-171">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="22d92-172">Tento přístup zajišťuje, že generovaný název souboru XML odpovídá názvu projektu.</span><span class="sxs-lookup"><span data-stu-id="22d92-172">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="22d92-173">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) vlastnost se používá pro konstrukci cestu k souboru XML.</span><span class="sxs-lookup"><span data-stu-id="22d92-173">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="22d92-174">Přidávání komentářů triple lomítko na akci vylepšuje uživatelské rozhraní Swagger přidáním popis do záhlaví části.</span><span class="sxs-lookup"><span data-stu-id="22d92-174">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="22d92-175">Přidat [ \<souhrnné >](/dotnet/csharp/programming-guide/xmldoc/summary) element výše `Delete` akce:</span><span class="sxs-lookup"><span data-stu-id="22d92-175">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="22d92-176">Vnitřní text předchozí kód zobrazí uživatelské rozhraní Swagger `<summary>` element:</span><span class="sxs-lookup"><span data-stu-id="22d92-176">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Zobrazuje komentáře XML odstraní konkrétní TodoItem. uživatelské rozhraní swagger](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="22d92-179">Uživatelské rozhraní vycházejí z generovaného schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="22d92-179">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="22d92-180">Přidat [ \<Poznámky >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu, který chcete `Create` dokumentace metoda akce.</span><span class="sxs-lookup"><span data-stu-id="22d92-180">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="22d92-181">Doplňuje informace uvedené v `<summary>` elementu a poskytuje robustnější uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="22d92-181">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="22d92-182">`<remarks>` Obsah elementu se může skládat z textu JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="22d92-182">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="22d92-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="22d92-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22d92-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="22d92-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>

::: moniker-end

<span data-ttu-id="22d92-185">Všimněte si, vylepšení uživatelského rozhraní s tyto další komentáři:</span><span class="sxs-lookup"><span data-stu-id="22d92-185">Notice the UI enhancements with these additional comments:</span></span>

![Uživatelské rozhraní swagger s další komentáři zobrazí](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="22d92-187">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="22d92-187">Data annotations</span></span>

<span data-ttu-id="22d92-188">Uspořádání model s atributy, které jsou součástí [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů, k rozvoji součásti uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="22d92-188">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="22d92-189">Přidat `[Required]` atribut `Name` vlastnost `TodoItem` třídy:</span><span class="sxs-lookup"><span data-stu-id="22d92-189">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="22d92-190">Přítomnost tento atribut se změní chování uživatelského rozhraní a mění základní schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="22d92-190">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="22d92-191">Přidat `[Produces("application/json")]` atribut kontroleru rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="22d92-191">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="22d92-192">Jejím účelem je deklarovat, že akce kontroleru podporují typ obsahu odpovědi z *application/json*:</span><span class="sxs-lookup"><span data-stu-id="22d92-192">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="22d92-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="22d92-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22d92-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="22d92-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="22d92-195">**Typ obsahu odpovědi** rozevíracího seznamu vybere jako výchozí pro akce kontroleru GET tento typ obsahu:</span><span class="sxs-lookup"><span data-stu-id="22d92-195">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Uživatelské rozhraní swagger s výchozí typ obsahu odpovědi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="22d92-197">Při použití datové poznámky v rozhraní Web API rostoucí, uživatelského rozhraní a rozhraní API pomohou stránky stát větší popisnosti a užitečné.</span><span class="sxs-lookup"><span data-stu-id="22d92-197">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="22d92-198">Popisují typy odezvy</span><span class="sxs-lookup"><span data-stu-id="22d92-198">Describe response types</span></span>

<span data-ttu-id="22d92-199">Využívání vývojáři jsou nejvíce zajímají co je vrácen&mdash;konkrétně odpovědi typy a kódy chyb (Pokud není standard).</span><span class="sxs-lookup"><span data-stu-id="22d92-199">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="22d92-200">V XML komentáře a data poznámky jsou rozlišeny odpovědi typy a kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="22d92-200">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="22d92-201">`Create` Akce vrátí kód stavu HTTP 201 v případě úspěchu.</span><span class="sxs-lookup"><span data-stu-id="22d92-201">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="22d92-202">Stavový kód HTTP 400 se vrátí při textu požadavku odeslaného má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="22d92-202">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="22d92-203">Bez správné dokumentace v uživatelském rozhraní Swagger nemá příjemce znalost těchto očekávaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="22d92-203">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="22d92-204">Tento problém můžete vyřešte přidáním zvýrazněné řádky v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="22d92-204">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="22d92-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="22d92-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22d92-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="22d92-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>

::: moniker-end

<span data-ttu-id="22d92-207">Uživatelské rozhraní Swagger teď jasně dokumenty očekávané kódy odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="22d92-207">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Zobrazuje popis třídy odpovědi POST, vrátí nově vytvořená položka Todo, uživatelské rozhraní swagger a '400 - Pokud položka má hodnotu null' stavový kód a důvod pod odpovědí na zprávy](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="22d92-209">Přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="22d92-209">Customize the UI</span></span>

<span data-ttu-id="22d92-210">Stock uživatelského rozhraní je funkční a přístupné.</span><span class="sxs-lookup"><span data-stu-id="22d92-210">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="22d92-211">Ale stránky dokumentace k rozhraní API by měl představovat značku nebo motivu.</span><span class="sxs-lookup"><span data-stu-id="22d92-211">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="22d92-212">Branding komponenty Swashbuckle vyžaduje přidání prostředků poskytovat statické soubory a sestavování strukturu složek pro hostování těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="22d92-212">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="22d92-213">Pokud cílení na rozhraní .NET Framework nebo .NET Core 1.x, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) balíček NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="22d92-213">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="22d92-214">Předchozí balíček NuGet je již nainstalován, pokud cílení na .NET Core 2.x a pomocí [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="22d92-214">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="22d92-215">Povolte middleware statické soubory:</span><span class="sxs-lookup"><span data-stu-id="22d92-215">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="22d92-216">Získat obsah *dist* složky z [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="22d92-216">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="22d92-217">Tato složka obsahuje nezbytné prostředky pro stránka uživatelského rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="22d92-217">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="22d92-218">Vytvoření *wwwroot/swagger/uživatelského rozhraní* složku a zkopírujte do ní obsah *dist* složky.</span><span class="sxs-lookup"><span data-stu-id="22d92-218">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="22d92-219">Vytvoření *custom.css* v souboru *wwwroot/swagger/uživatelského rozhraní*, s následující šablon stylů CSS, chcete-li přizpůsobit záhlaví stránky:</span><span class="sxs-lookup"><span data-stu-id="22d92-219">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="22d92-220">Referenční dokumentace *custom.css* v *index.html* souboru po další soubory šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="22d92-220">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="22d92-221">Vyhledejte *index.html* stránky v `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="22d92-221">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="22d92-222">Zadejte `http://localhost:<port>/swagger/v1/swagger.json` textové pole hlavičky a klikněte na **prozkoumat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22d92-222">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="22d92-223">Výsledná stránka vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="22d92-223">The resulting page looks as follows:</span></span>

![Uživatelské rozhraní swagger s názvem vlastní hlavičky](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="22d92-225">Je mnohem víc, že můžete provést pomocí stránky.</span><span class="sxs-lookup"><span data-stu-id="22d92-225">There's much more you can do with the page.</span></span> <span data-ttu-id="22d92-226">Najdete v části úplné možnosti pro prostředky uživatelského rozhraní na [úložiště GitHub uživatelského rozhraní Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="22d92-226">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
