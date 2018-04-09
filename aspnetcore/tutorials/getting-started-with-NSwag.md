---
title: Začínáme s NSwag a ASP.NET Core
author: zuckerthoben
description: Další informace o použití NSwag generovat dokumentaci a stránky pro aplikace ASP.NET Core webového rozhraní API.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="1b2d3-103">Začínáme s NSwag a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b2d3-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="1b2d3-104">Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="1b2d3-105">Pomocí [NSwag](https://github.com/RSuter/NSwag) s ASP.NET Core vyžaduje middlewaru [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="1b2d3-106">Balíček se skládá z generátor Swagger, uživatelské rozhraní Swagger (v2 a v3), a [uživatelského rozhraní ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="1b2d3-107">Má důrazně doporučujeme používat pro NSwag možnosti generování kódu.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="1b2d3-108">Vyberte jednu z následujících možností pro generování kódu:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="1b2d3-109">Použití [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikace na ploše systému Windows pro generování kódu klienta pro vaše rozhraní API v C# a TypeScript</span><span class="sxs-lookup"><span data-stu-id="1b2d3-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="1b2d3-110">Použití [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) nebo [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) balíčky NuGet pro generování uvnitř projektu kódu</span><span class="sxs-lookup"><span data-stu-id="1b2d3-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="1b2d3-111">Použít NSwag z [příkazového řádku](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="1b2d3-112">Použití [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="1b2d3-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="1b2d3-113">Funkce</span><span class="sxs-lookup"><span data-stu-id="1b2d3-113">Features</span></span>

<span data-ttu-id="1b2d3-114">Hlavním důvodem pro použití NSwag je schopnost nejen vzniku uživatelské rozhraní Swagger a Swagger generátor, ale ujistěte se, chcete-li použít možnosti generování flexibilní kódu.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="1b2d3-115">Nepotřebujete existujícího rozhraní API&mdash;můžete použít rozhraní API třetích stran, která začlenit Swagger a nechat NSwag generovat implementace klienta.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="1b2d3-116">V obou případech je rychlé cyklu vývoje a snadno můžete akceptovat změny rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="1b2d3-117">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="1b2d3-117">Package installation</span></span>

<span data-ttu-id="1b2d3-118">Balíček NSwag NuGet lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b2d3-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b2d3-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1b2d3-120">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="1b2d3-121">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="1b2d3-122">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="1b2d3-123">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="1b2d3-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="1b2d3-124">Do vyhledávacího pole zadejte "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="1b2d3-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="1b2d3-125">Vyberte balíček "NSwag.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b2d3-126">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1b2d3-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1b2d3-127">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="1b2d3-128">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="1b2d3-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="1b2d3-129">Do vyhledávacího pole zadejte NSwag.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="1b2d3-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="1b2d3-130">Vyberte balíček NSwag.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b2d3-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1b2d3-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1b2d3-132">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1b2d3-133">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1b2d3-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1b2d3-134">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="1b2d3-135">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="1b2d3-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="1b2d3-136">Importovat následující obory názvů v `Info` třídy:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="1b2d3-137">V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného specifikace Swagger a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="1b2d3-138">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-138">Launch the app.</span></span> <span data-ttu-id="1b2d3-139">Přejděte na `/swagger` Chcete-li zobrazit uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="1b2d3-140">Přejděte na `/swagger/v1/swagger.json` zobrazíte specifikace Swagger.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="1b2d3-141">Generování kódu</span><span class="sxs-lookup"><span data-stu-id="1b2d3-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="1b2d3-142">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="1b2d3-142">Via NSwagStudio</span></span>

* <span data-ttu-id="1b2d3-143">Nainstalujte `NSwagStudio` z oficiálního [úložiště GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="1b2d3-144">Spusťte NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-144">Launch NSwagStudio.</span></span> <span data-ttu-id="1b2d3-145">Zadejte umístění vaší *swagger.json* nebo ho zkopírujte přímo:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="1b2d3-147">Označuje typ výstupu požadované klienta.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-147">Indicate the desired client output type.</span></span> <span data-ttu-id="1b2d3-148">Mezi možnosti patří **TypeScript klienta**, **CSharp klienta**, nebo **kontroler Web API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="1b2d3-149">Pomocí řadiče webové rozhraní API je v podstatě zpětného generace.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="1b2d3-150">Specifikace služby používá k opětovnému sestavení službu.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="1b2d3-151">Klikněte na tlačítko **generovat výstupy**.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="1b2d3-152">Tady vidíte na implementace klientů *TodoApi.NSwag* ukázku v jazyce C#:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="1b2d3-154">Uložte soubor do projektu klienta (například [Xamarin.Forms](/xamarin/xamarin-forms/) aplikace).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="1b2d3-155">Začněte spotřebovávat rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="1b2d3-156">Základní adresu URL nebo klienta HTTP můžete vložit do rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="1b2d3-157">Osvědčeným postupem je vždy [znovu použít HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="1b2d3-158">Nyní můžete spustit snadno implementace rozhraní API do projektů klienta.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="1b2d3-159">Další způsoby, jak generovat kód klienta</span><span class="sxs-lookup"><span data-stu-id="1b2d3-159">Other ways to generate client code</span></span>

<span data-ttu-id="1b2d3-160">Můžete generovat kód jinými způsoby, další vhodné do pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="1b2d3-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="1b2d3-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="1b2d3-162">V kódu</span><span class="sxs-lookup"><span data-stu-id="1b2d3-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="1b2d3-163">Šablony T4</span><span class="sxs-lookup"><span data-stu-id="1b2d3-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="1b2d3-164">Přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="1b2d3-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="1b2d3-165">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="1b2d3-165">XML comments</span></span>

<span data-ttu-id="1b2d3-166">XML – komentáře jsou povolené pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="1b2d3-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b2d3-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="1b2d3-168">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="1b2d3-169">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="1b2d3-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1b2d3-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="1b2d3-172">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="1b2d3-173">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="1b2d3-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1b2d3-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="1b2d3-176">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="1b2d3-177">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="1b2d3-177">Data annotations</span></span>

<span data-ttu-id="1b2d3-178">Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučený postup pro akce webového rozhraní API je vrátit [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="1b2d3-179">V důsledku toho NSwag nelze odvodit činnosti akci a návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="1b2d3-180">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="1b2d3-181">Vrátí předchozí akce `IActionResult`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) nebo [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="1b2d3-182">Poznámky data se používají k sdělte který odpověď HTTP, tato akce vrací klientů.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="1b2d3-183">Uspořádání akce s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="1b2d3-184">Generátor Swagger můžete nyní přesně popisují tato akce a vygenerovaný klienti věděli, co se zobrazí při volání metody koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="1b2d3-185">Architekturu všechny akce s tyto atributy se důrazně doporučuje.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="1b2d3-186">Na jaké odpovědi HTTP vaše rozhraní API akce by měla vrátit, naleznete na adrese [specifikaci RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
