---
title: Začínáme s NSwag a ASP.NET Core
author: zuckerthoben
description: Další informace o použití NSwag generovat dokumentaci a stránky pro webové ASP.NET Core rozhraní API.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 0f0aa0eeaa174ef40f03aab2500a8b3ce37e9448
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094889"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="43c9b-103">Začínáme s NSwag a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43c9b-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="43c9b-104">Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="43c9b-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43c9b-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43c9b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43c9b-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43c9b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="43c9b-107">Pomocí [NSwag](https://github.com/RSuter/NSwag) s ASP.NET Core vyžaduje middlewaru [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="43c9b-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="43c9b-108">Balíček se skládá z generátor Swagger, uživatelské rozhraní Swagger (v2 a v3), a [uživatelského rozhraní ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="43c9b-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="43c9b-109">Má důrazně doporučujeme používat pro NSwag možnosti generování kódu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="43c9b-110">Vyberte jednu z následujících možností pro generování kódu:</span><span class="sxs-lookup"><span data-stu-id="43c9b-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="43c9b-111">Použití [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikace na ploše systému Windows pro generování kódu klienta v C# a TypeScript pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="43c9b-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="43c9b-112">Použití [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) nebo [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) balíčky NuGet pro generování uvnitř projektu kódu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="43c9b-113">Použít NSwag z [příkazového řádku](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="43c9b-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="43c9b-114">Použití [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="43c9b-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="43c9b-115">Funkce</span><span class="sxs-lookup"><span data-stu-id="43c9b-115">Features</span></span>

<span data-ttu-id="43c9b-116">Hlavním důvodem pro použití NSwag je schopnost nejen vzniku uživatelské rozhraní Swagger a Swagger generátor, ale ujistěte se, chcete-li použít možnosti generování flexibilní kódu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="43c9b-117">Nepotřebujete existujícího rozhraní API&mdash;můžete použít rozhraní API třetích stran, která začlenit Swagger a nechat NSwag generovat implementace klienta.</span><span class="sxs-lookup"><span data-stu-id="43c9b-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="43c9b-118">V obou případech je rychlé cyklu vývoje a snadno můžete akceptovat změny rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="43c9b-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="43c9b-119">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="43c9b-119">Package installation</span></span>

<span data-ttu-id="43c9b-120">Balíček NSwag NuGet lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="43c9b-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43c9b-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43c9b-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="43c9b-122">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="43c9b-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="43c9b-123">Přejděte na **zobrazení** > **jinými** > **Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="43c9b-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="43c9b-124">Přejděte do adresáře, ve kterém *TodoApi.csproj* soubor existuje</span><span class="sxs-lookup"><span data-stu-id="43c9b-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="43c9b-125">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43c9b-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="43c9b-126">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="43c9b-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="43c9b-127">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="43c9b-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="43c9b-128">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="43c9b-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="43c9b-129">Do vyhledávacího pole zadejte "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="43c9b-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="43c9b-130">Vyberte balíček "NSwag.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="43c9b-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43c9b-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="43c9b-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="43c9b-132">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="43c9b-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="43c9b-133">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="43c9b-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="43c9b-134">Do vyhledávacího pole zadejte "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="43c9b-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="43c9b-135">V podokně výsledků vyberte balíček "NSwag.AspNetCore" a klikněte na **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="43c9b-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43c9b-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43c9b-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43c9b-137">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="43c9b-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43c9b-138">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="43c9b-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="43c9b-139">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43c9b-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="43c9b-140">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="43c9b-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="43c9b-141">Importovat následující obory názvů v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="43c9b-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="43c9b-142">V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného specifikace Swagger a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="43c9b-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="43c9b-143">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="43c9b-143">Launch the app.</span></span> <span data-ttu-id="43c9b-144">Přejděte na `http://localhost:<port>/swagger` Chcete-li zobrazit uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="43c9b-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="43c9b-145">Přejděte na `http://localhost:<port>/swagger/v1/swagger.json` zobrazíte specifikace Swagger.</span><span class="sxs-lookup"><span data-stu-id="43c9b-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="43c9b-146">Generování kódu</span><span class="sxs-lookup"><span data-stu-id="43c9b-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="43c9b-147">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="43c9b-147">Via NSwagStudio</span></span>

* <span data-ttu-id="43c9b-148">Nainstalujte NSwagStudio z oficiální [úložiště GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="43c9b-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="43c9b-149">Spusťte NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="43c9b-149">Launch NSwagStudio.</span></span> <span data-ttu-id="43c9b-150">Zadejte *swagger.json* adresa URL v souboru **adresa URL Swaggeru specifikace** textovému poli a klikněte na tlačítko **vytvořit místní kopii** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="43c9b-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="43c9b-151">Vyberte **CSharp klienta** klienta výstup typu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="43c9b-152">Další možnosti zahrnují **TypeScript klienta** a **kontroler Web API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="43c9b-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="43c9b-153">Pomocí řadiče webové rozhraní API je v podstatě zpětného generace.</span><span class="sxs-lookup"><span data-stu-id="43c9b-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="43c9b-154">Specifikace služby používá k opětovnému sestavení službu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="43c9b-155">Klikněte **generovat výstupy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="43c9b-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="43c9b-156">Na dokončení C# klienta implementace *TodoApi.NSwag* vytváří projektu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="43c9b-157">Klikněte **CSharp klienta** kartě **výstupy** oddílu kód klienta vygenerovaný:</span><span class="sxs-lookup"><span data-stu-id="43c9b-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="43c9b-158">Generování kódu klienta C# na základě nastavení definované v **nastavení** kartě **CSharp klienta** kartě. Změňte nastavení a provádět úkoly jako výchozí obor názvů přejmenování a synchronní metoda generace.</span><span class="sxs-lookup"><span data-stu-id="43c9b-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="43c9b-159">Zkopírujte vygenerovaný kód C# do souboru v projektu klienta (například [Xamarin.Forms](/xamarin/xamarin-forms/) aplikace).</span><span class="sxs-lookup"><span data-stu-id="43c9b-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="43c9b-160">Začněte spotřebovávat webové rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="43c9b-160">Start consuming the web API:</span></span>

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
> <span data-ttu-id="43c9b-161">Základní adresu URL nebo klienta HTTP můžete vložit do rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="43c9b-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="43c9b-162">Osvědčeným postupem je vždy [znovu použít HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="43c9b-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="43c9b-163">Další způsoby, jak generovat kód klienta</span><span class="sxs-lookup"><span data-stu-id="43c9b-163">Other ways to generate client code</span></span>

<span data-ttu-id="43c9b-164">Můžete generovat kód klienta jinými způsoby, další vhodné do pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="43c9b-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="43c9b-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="43c9b-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="43c9b-166">V kódu</span><span class="sxs-lookup"><span data-stu-id="43c9b-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="43c9b-167">Šablony T4</span><span class="sxs-lookup"><span data-stu-id="43c9b-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="43c9b-168">Přizpůsobit</span><span class="sxs-lookup"><span data-stu-id="43c9b-168">Customize</span></span>

<span data-ttu-id="43c9b-169">Swagger poskytuje možnosti pro objektový model dokumentace k usnadnění používání webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="43c9b-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="43c9b-170">Informace o rozhraní API a popis</span><span class="sxs-lookup"><span data-stu-id="43c9b-170">API info and description</span></span>

<span data-ttu-id="43c9b-171">V `Startup.Configure` metody akce konfigurace předán `UseSwagger` metoda přidá informace, jako autor, licence a popis:</span><span class="sxs-lookup"><span data-stu-id="43c9b-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="43c9b-172">Uživatelské rozhraní Swagger zobrazí informace na verzi:</span><span class="sxs-lookup"><span data-stu-id="43c9b-172">The Swagger UI displays the version's information:</span></span>

![Uživatelské rozhraní swagger se informace o verzi](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="43c9b-174">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="43c9b-174">XML comments</span></span>

<span data-ttu-id="43c9b-175">XML – komentáře jsou povolené pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="43c9b-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="43c9b-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43c9b-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="43c9b-177">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="43c9b-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="43c9b-178">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karta</span><span class="sxs-lookup"><span data-stu-id="43c9b-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="43c9b-179">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="43c9b-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="43c9b-180">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="43c9b-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="43c9b-181">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části</span><span class="sxs-lookup"><span data-stu-id="43c9b-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="43c9b-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43c9b-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="43c9b-183">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="43c9b-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="43c9b-184">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="43c9b-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="43c9b-185">Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučené návratový typ pro akce webového rozhraní API je [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="43c9b-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="43c9b-186">V důsledku toho NSwag nelze odvodit činnosti akci a návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="43c9b-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="43c9b-187">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="43c9b-187">Consider the following example:</span></span>

<span data-ttu-id="43c9b-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="43c9b-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="43c9b-189">Vrátí předchozí akce `IActionResult`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) nebo [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="43c9b-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="43c9b-190">Datových poznámek se používají klienti říct, kterých se ví, že tato akce vrátí stavové kódy HTTP.</span><span class="sxs-lookup"><span data-stu-id="43c9b-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="43c9b-191">Uspořádání akce s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="43c9b-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="43c9b-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="43c9b-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="43c9b-193">Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučené návratový typ pro akce webového rozhraní API je [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="43c9b-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="43c9b-194">V důsledku toho můžete NSwag pouze odvození návratový typ definované `T`.</span><span class="sxs-lookup"><span data-stu-id="43c9b-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="43c9b-195">Nejde odvodit další možné návratové typy v akci.</span><span class="sxs-lookup"><span data-stu-id="43c9b-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="43c9b-196">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="43c9b-196">Consider the following example:</span></span>

<span data-ttu-id="43c9b-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="43c9b-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="43c9b-198">Vrátí předchozí akce `ActionResult<T>`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="43c9b-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="43c9b-199">Vzhledem k tomu, že kontroler opatřen s [[objektu ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atribut [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) odpovědi je možné příliš.</span><span class="sxs-lookup"><span data-stu-id="43c9b-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="43c9b-200">V tématu [automatické HTTP 400 odpovědí](xref:web-api/index#automatic-http-400-responses) Další informace.</span><span class="sxs-lookup"><span data-stu-id="43c9b-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="43c9b-201">Datových poznámek se používají klienti říct, kterých se ví, že tato akce vrátí stavové kódy HTTP.</span><span class="sxs-lookup"><span data-stu-id="43c9b-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="43c9b-202">Uspořádání akce s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="43c9b-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="43c9b-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="43c9b-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="43c9b-204">Generátor Swagger můžete nyní přesně popisují tato akce a vygenerovaný klienti věděli, co se zobrazí při volání metody koncový bod.</span><span class="sxs-lookup"><span data-stu-id="43c9b-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="43c9b-205">Architekturu všechny akce s tyto atributy se důrazně doporučuje.</span><span class="sxs-lookup"><span data-stu-id="43c9b-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="43c9b-206">Na jaké odpovědi HTTP vaše rozhraní API akce by měla vrátit, naleznete na adrese [specifikaci RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="43c9b-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
