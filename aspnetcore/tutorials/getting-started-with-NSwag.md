---
title: Začínáme se službou NSwag a ASP.NET Core
author: zuckerthoben
description: Další informace o použití službou NSwag generovat dokumentaci a stránky pro webovému rozhraní API ASP.NET Core nápovědy.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: ba20ccfbe2610eb4e3ec3a4c35f8e3b4cae0bb9c
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011872"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="087c6-103">Začínáme se službou NSwag a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="087c6-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="087c6-104">Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="087c6-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="087c6-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="087c6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="087c6-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="087c6-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="087c6-107">Pomocí [službou NSwag](https://github.com/RSuter/NSwag) pomocí ASP.NET Core vyžaduje middlewaru [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="087c6-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="087c6-108">Balíček se skládá z generátoru Swagger, uživatelské rozhraní Swagger (v2 a v3), a [uživatelského rozhraní ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="087c6-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="087c6-109">Je důrazně doporučujeme používat vaší službou NSwag možnosti generování kódu.</span><span class="sxs-lookup"><span data-stu-id="087c6-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="087c6-110">Pro generování kódu vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="087c6-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="087c6-111">Použití [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), desktopové aplikace Windows pro generování klientského kódu v C# a TypeScript pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="087c6-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="087c6-112">Použití [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) nebo [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) balíčky NuGet, se generování uvnitř projektu kódu.</span><span class="sxs-lookup"><span data-stu-id="087c6-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="087c6-113">Použití službou NSwag z [příkazového řádku](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="087c6-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="087c6-114">Použití [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="087c6-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="087c6-115">Funkce</span><span class="sxs-lookup"><span data-stu-id="087c6-115">Features</span></span>

<span data-ttu-id="087c6-116">Hlavním důvodem pro použití službou NSwag je schopnost nejen zavést uživatelské rozhraní Swagger a Swagger generátor, ale ujistěte se, chcete-li využívají možnosti generace pružný kódu.</span><span class="sxs-lookup"><span data-stu-id="087c6-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="087c6-117">Není nutné existujícího rozhraní API&mdash;můžete použít rozhraní API třetích stran, která začlenit Swagger a nechat službou NSwag generovat implementace klienta.</span><span class="sxs-lookup"><span data-stu-id="087c6-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="087c6-118">V obou případech urychlené vývojový cyklus a snadno přizpůsobit změn rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="087c6-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="087c6-119">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="087c6-119">Package installation</span></span>

<span data-ttu-id="087c6-120">Balíček NuGet službou NSwag lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="087c6-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="087c6-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="087c6-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="087c6-122">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="087c6-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="087c6-123">Přejděte na **zobrazení** > **jiných Windows** > **Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="087c6-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="087c6-124">Přejděte do adresáře, ve kterém *TodoApi.csproj* soubor existuje</span><span class="sxs-lookup"><span data-stu-id="087c6-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="087c6-125">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="087c6-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="087c6-126">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="087c6-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="087c6-127">Klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="087c6-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="087c6-128">Nastavte **zdroj balíčku** do "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="087c6-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="087c6-129">Do vyhledávacího pole zadejte "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="087c6-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="087c6-130">Vyberte balíček "NSwag.AspNetCore" z **Procházet** kartě a klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="087c6-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="087c6-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="087c6-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="087c6-132">Klikněte pravým tlačítkem myši *balíčky* složky v **oblasti řešení** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="087c6-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="087c6-133">Nastavte **přidat balíčky** okna **zdroj** rozevíracího seznamu "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="087c6-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="087c6-134">Do vyhledávacího pole zadejte "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="087c6-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="087c6-135">Vyberte v podokně výsledků "NSwag.AspNetCore" balíček a klikněte na tlačítko **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="087c6-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="087c6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="087c6-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="087c6-137">Spuštěním následujícího příkazu z **integrovaný terminál**:</span><span class="sxs-lookup"><span data-stu-id="087c6-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="087c6-138">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="087c6-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="087c6-139">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="087c6-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="087c6-140">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="087c6-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="087c6-141">Importujte následující obory názvů v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="087c6-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="087c6-142">V `Startup.Configure` metoda, povolí middleware pro poskytování generované specifikace Swagger a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="087c6-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="087c6-143">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="087c6-143">Launch the app.</span></span> <span data-ttu-id="087c6-144">Přejděte na `http://localhost:<port>/swagger` Chcete-li zobrazit uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="087c6-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="087c6-145">Přejděte do `http://localhost:<port>/swagger/v1/swagger.json` zobrazíte specifikace Swagger.</span><span class="sxs-lookup"><span data-stu-id="087c6-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="087c6-146">Generování kódu</span><span class="sxs-lookup"><span data-stu-id="087c6-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="087c6-147">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="087c6-147">Via NSwagStudio</span></span>

* <span data-ttu-id="087c6-148">Nainstalujte NSwagStudio z oficiální [úložiště GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="087c6-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="087c6-149">Spusťte NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="087c6-149">Launch NSwagStudio.</span></span> <span data-ttu-id="087c6-150">Zadejte *swagger.json* adresy URL v souboru **adresa URL specifikace Swaggeru** textového pole a klikněte na tlačítko **vytvořit místní kopii** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="087c6-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="087c6-151">Vyberte **CSharp klienta** typ výstupu klienta.</span><span class="sxs-lookup"><span data-stu-id="087c6-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="087c6-152">Další možnosti zahrnují **TypeScript klienta** a **Kontroleru webového rozhraní API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="087c6-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="087c6-153">Použití Kontroleru webového rozhraní API je v podstatě obrácenou generace.</span><span class="sxs-lookup"><span data-stu-id="087c6-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="087c6-154">Specifikace služby používá k opětovnému sestavení služby.</span><span class="sxs-lookup"><span data-stu-id="087c6-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="087c6-155">Klikněte na tlačítko **generovat výstupy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="087c6-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="087c6-156">Na dokončení C# klienta implementace *TodoApi.NSwag* projekt je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="087c6-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="087c6-157">Klikněte na tlačítko **CSharp klienta** karty **výstupy** část kódu generovaného klienta:</span><span class="sxs-lookup"><span data-stu-id="087c6-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="087c6-158">Generování kódu klienta jazyka C# na základě definované v nastavení **nastavení** kartě **CSharp klienta** kartu. Změňte nastavení a provádět úlohy, například přejmenování výchozí obor názvů a generování synchronní metody.</span><span class="sxs-lookup"><span data-stu-id="087c6-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="087c6-159">Zkopírujte vygenerovaný kód jazyka C# do souboru v projektu klienta (například [Xamarin.Forms](/xamarin/xamarin-forms/) aplikace).</span><span class="sxs-lookup"><span data-stu-id="087c6-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="087c6-160">Spusťte využívající webové rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="087c6-160">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="087c6-161">Základní adresu URL a/nebo klienta HTTP můžete vložit do rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="087c6-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="087c6-162">Osvědčeným postupem je vždy [opakovaně používat HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="087c6-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="087c6-163">Další možnosti, jak generovat kód klienta</span><span class="sxs-lookup"><span data-stu-id="087c6-163">Other ways to generate client code</span></span>

<span data-ttu-id="087c6-164">Můžete vygenerovat kód klienta jinými způsoby, další vhodné do svého pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="087c6-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="087c6-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="087c6-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="087c6-166">V kódu</span><span class="sxs-lookup"><span data-stu-id="087c6-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="087c6-167">Šablony T4</span><span class="sxs-lookup"><span data-stu-id="087c6-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="087c6-168">Přizpůsobit</span><span class="sxs-lookup"><span data-stu-id="087c6-168">Customize</span></span>

<span data-ttu-id="087c6-169">Swagger poskytuje možnosti pro dokumentace objektového modelu pro usnadnění spotřeby webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="087c6-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="087c6-170">Informace o rozhraní API a popis</span><span class="sxs-lookup"><span data-stu-id="087c6-170">API info and description</span></span>

<span data-ttu-id="087c6-171">V `Startup.Configure` metody akce konfigurace předán `UseSwagger` přidá informace, jako je vytváření, licence a popis metody:</span><span class="sxs-lookup"><span data-stu-id="087c6-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="087c6-172">Uživatelské rozhraní Swagger zobrazí informace na verzi:</span><span class="sxs-lookup"><span data-stu-id="087c6-172">The Swagger UI displays the version's information:</span></span>

![Uživatelské rozhraní swagger s informací o verzi](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="087c6-174">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="087c6-174">XML comments</span></span>

<span data-ttu-id="087c6-175">Komentáře XML jsou povolené pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="087c6-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="087c6-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="087c6-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="087c6-177">Klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **upravit < project_name > .csproj**.</span><span class="sxs-lookup"><span data-stu-id="087c6-177">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="087c6-178">Ručně přidejte zvýrazněné řádky a *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="087c6-178">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="087c6-179">Klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="087c6-179">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="087c6-180">Zkontrolujte **soubor dokumentace XML** pole v rámci **výstup** část **sestavení** kartu</span><span class="sxs-lookup"><span data-stu-id="087c6-180">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="087c6-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="087c6-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="087c6-182">Z *oblasti řešení*, stiskněte klávesu **ovládací prvek** a klikněte na název projektu.</span><span class="sxs-lookup"><span data-stu-id="087c6-182">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="087c6-183">Přejděte do **nástroje** > **upravit soubor**.</span><span class="sxs-lookup"><span data-stu-id="087c6-183">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="087c6-184">Ručně přidejte zvýrazněné řádky a *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="087c6-184">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="087c6-185">Otevřít **možnosti projektu** dialogového okna > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="087c6-185">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="087c6-186">Zkontrolujte **generovat dokumentaci xml** pole v rámci **Obecné možnosti** oddílu</span><span class="sxs-lookup"><span data-stu-id="087c6-186">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="087c6-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="087c6-187">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="087c6-188">Ručně přidejte zvýrazněné řádky a *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="087c6-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="087c6-189">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="087c6-189">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="087c6-190">Používá se službou NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučené návratový typ pro akce webové rozhraní API je [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="087c6-190">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="087c6-191">V důsledku toho službou NSwag nelze odvodit, co dělá akce a návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="087c6-191">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="087c6-192">Vezměte v úvahu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="087c6-192">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="087c6-193">Vrátí předchozí akce `IActionResult`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) nebo [chybného požadavku](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="087c6-193">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="087c6-194">Datové poznámky se používají zjistit klientům, kterých se ví, že tato akce vrátit stavové kódy HTTP.</span><span class="sxs-lookup"><span data-stu-id="087c6-194">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="087c6-195">Uspořádání akce s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="087c6-195">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="087c6-196">Používá se službou NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučené návratový typ pro akce webové rozhraní API je [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="087c6-196">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="087c6-197">V důsledku toho může službou NSwag pouze odvodit návratový typ určené `T`.</span><span class="sxs-lookup"><span data-stu-id="087c6-197">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="087c6-198">Nejde odvodit další možné návratové typy v akci.</span><span class="sxs-lookup"><span data-stu-id="087c6-198">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="087c6-199">Vezměte v úvahu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="087c6-199">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="087c6-200">Vrátí předchozí akce `ActionResult<T>`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="087c6-200">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="087c6-201">Protože kontroler je doplněn [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut, [chybného požadavku](/dotnet/api/system.web.http.apicontroller.badrequest) odpovědi je také možné.</span><span class="sxs-lookup"><span data-stu-id="087c6-201">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="087c6-202">Zobrazit [odpovědi HTTP 400 automatické](xref:web-api/index#automatic-http-400-responses) pro další informace.</span><span class="sxs-lookup"><span data-stu-id="087c6-202">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="087c6-203">Datové poznámky se používají zjistit klientům, kterých se ví, že tato akce vrátit stavové kódy HTTP.</span><span class="sxs-lookup"><span data-stu-id="087c6-203">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="087c6-204">Uspořádání akce s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="087c6-204">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

<span data-ttu-id="087c6-205">Generátoru Swagger můžete nyní přesně popište tuto akci a vygenerovaný klienti věděli, co se zobrazí při volání koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="087c6-205">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="087c6-206">Upravení všechny akce s těmito atributy důrazně doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="087c6-206">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="087c6-207">Pokyny pro jaké odpovědi protokolu HTTP, by měla vrátit akce rozhraní API najdete v článku [specifikaci RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="087c6-207">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
