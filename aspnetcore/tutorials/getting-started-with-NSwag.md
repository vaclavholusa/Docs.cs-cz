---
title: "Začínáme s NSwag"
author: zuckerthoben
description: "V tomto kurzu poskytuje návod k přidávání NSwag generovat dokumentaci a stránky pro aplikaci pomocí webového rozhraní API."
keywords: "ASP.NET Core, Swagger, NSwag, pomůže stránky webového rozhraní API"
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: ce30ad3538c6294003a4f38ca80ebd73c0f52542
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-nswag"></a><span data-ttu-id="3a368-104">Začínáme s NSwag</span><span class="sxs-lookup"><span data-stu-id="3a368-104">Get started with NSwag</span></span>

<span data-ttu-id="3a368-105">Podle [Christoph Nienaber](https://twitter.com/zuckerthoben) a [Portoriku Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="3a368-105">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="3a368-106">Pomocí [NSwag](https://github.com/RSuter/NSwag) s ASP.NET Core vyžaduje middlewaru [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="3a368-106">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="3a368-107">Balíček se skládá z generátor Swagger, uživatelské rozhraní Swagger (v2 a v3), a [uživatelského rozhraní ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="3a368-107">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="3a368-108">Má důrazně doporučujeme používat pro NSwag možnosti generování kódu.</span><span class="sxs-lookup"><span data-stu-id="3a368-108">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="3a368-109">Vyberte jednu z následujících možností pro generování kódu:</span><span class="sxs-lookup"><span data-stu-id="3a368-109">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="3a368-110">Použití [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikace na ploše systému Windows pro generování kódu klienta pro vaše rozhraní API v C# a TypeScript</span><span class="sxs-lookup"><span data-stu-id="3a368-110">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="3a368-111">Použití [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) nebo [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) balíčky NuGet pro generování uvnitř projektu kódu</span><span class="sxs-lookup"><span data-stu-id="3a368-111">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="3a368-112">Použít NSwag z [příkazového řádku](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="3a368-112">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="3a368-113">Použití [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="3a368-113">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="3a368-114">Funkce</span><span class="sxs-lookup"><span data-stu-id="3a368-114">Features</span></span>

<span data-ttu-id="3a368-115">Hlavním důvodem pro použití NSwag je schopnost nejen vzniku uživatelské rozhraní Swagger a Swagger generátor, ale ujistěte se, chcete-li použít možnosti generování flexibilní kódu.</span><span class="sxs-lookup"><span data-stu-id="3a368-115">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="3a368-116">Nepotřebujete existujícího rozhraní API&mdash;můžete použít rozhraní API třetích stran, která začlenit Swagger a nechat NSwag generovat implementace klienta.</span><span class="sxs-lookup"><span data-stu-id="3a368-116">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="3a368-117">V obou případech je rychlé cyklu vývoje a snadno můžete akceptovat změny rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3a368-117">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="3a368-118">Instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="3a368-118">Package installation</span></span>

<span data-ttu-id="3a368-119">Balíček NSwag NuGet lze přidat pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="3a368-119">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3a368-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3a368-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3a368-121">Z **Konzola správce balíčků** okno:</span><span class="sxs-lookup"><span data-stu-id="3a368-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="3a368-122">Z **spravovat balíčky NuGet** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="3a368-122">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="3a368-123">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="3a368-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="3a368-124">Nastavte **zdroj balíčku** na "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="3a368-124">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="3a368-125">Do vyhledávacího pole zadejte "NSwag.AspNetCore"</span><span class="sxs-lookup"><span data-stu-id="3a368-125">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="3a368-126">Vyberte balíček "NSwag.AspNetCore" z **Procházet** a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="3a368-126">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3a368-127">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3a368-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3a368-128">Klikněte pravým tlačítkem myši *balíčky* složky v **řešení Pad** > **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="3a368-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="3a368-129">Nastavte **přidat balíčky** uživatele systému Windows **zdroj** rozevírací seznam pro "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="3a368-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="3a368-130">Do vyhledávacího pole zadejte NSwag.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="3a368-130">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="3a368-131">Vyberte balíček NSwag.AspNetCore v podokně výsledků a klikněte na tlačítko **přidat balíček**</span><span class="sxs-lookup"><span data-stu-id="3a368-131">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3a368-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3a368-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3a368-133">Spusťte následující příkaz z **integrované Terminálové**:</span><span class="sxs-lookup"><span data-stu-id="3a368-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3a368-134">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="3a368-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3a368-135">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3a368-135">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="3a368-136">Přidejte a nakonfigurujte Swagger middleware</span><span class="sxs-lookup"><span data-stu-id="3a368-136">Add and configure Swagger middleware</span></span>

<span data-ttu-id="3a368-137">Importovat následující obory názvů v `Info` třídy:</span><span class="sxs-lookup"><span data-stu-id="3a368-137">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="3a368-138">V `Startup.Configure` metoda, povolit middleware pro obsluhující generovaného specifikace Swagger a uživatelské rozhraní Swagger:</span><span class="sxs-lookup"><span data-stu-id="3a368-138">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="3a368-139">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3a368-139">Launch the app.</span></span> <span data-ttu-id="3a368-140">Přejděte na `/swagger` Chcete-li zobrazit uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="3a368-140">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="3a368-141">Přejděte na `/swagger/v1/swagger.json` zobrazíte specifikace Swagger.</span><span class="sxs-lookup"><span data-stu-id="3a368-141">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="3a368-142">Generování kódu</span><span class="sxs-lookup"><span data-stu-id="3a368-142">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="3a368-143">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="3a368-143">Via NSwagStudio</span></span>

* <span data-ttu-id="3a368-144">Nainstalujte `NSwagStudio` z oficiálního [úložiště GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="3a368-144">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="3a368-145">Spusťte NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="3a368-145">Launch NSwagStudio.</span></span> <span data-ttu-id="3a368-146">Zadejte umístění vaší *swagger.json* nebo ho zkopírujte přímo:</span><span class="sxs-lookup"><span data-stu-id="3a368-146">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="3a368-148">Označuje typ výstupu požadované klienta.</span><span class="sxs-lookup"><span data-stu-id="3a368-148">Indicate the desired client output type.</span></span> <span data-ttu-id="3a368-149">Mezi možnosti patří **TypeScript klienta**, **CSharp klienta**, nebo **kontroler Web API CSharp**.</span><span class="sxs-lookup"><span data-stu-id="3a368-149">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="3a368-150">Pomocí řadiče webové rozhraní API je v podstatě zpětného generace.</span><span class="sxs-lookup"><span data-stu-id="3a368-150">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="3a368-151">Specifikace služby používá k opětovnému sestavení službu.</span><span class="sxs-lookup"><span data-stu-id="3a368-151">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="3a368-152">Klikněte na tlačítko **generovat výstupy**.</span><span class="sxs-lookup"><span data-stu-id="3a368-152">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="3a368-153">Tady vidíte na implementace klientů *TodoApi.NSwag* ukázku v jazyce C#:</span><span class="sxs-lookup"><span data-stu-id="3a368-153">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="3a368-155">Uložte soubor do projektu klienta (například [Xamarin.Forms](/xamarin/xamarin-forms/) aplikace).</span><span class="sxs-lookup"><span data-stu-id="3a368-155">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="3a368-156">Začněte spotřebovávat rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="3a368-156">Start consuming the API:</span></span>

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
> <span data-ttu-id="3a368-157">Základní adresu URL nebo klienta HTTP můžete vložit do rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="3a368-157">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="3a368-158">Osvědčeným postupem je vždy [znovu použít HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="3a368-158">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="3a368-159">Nyní můžete spustit snadno implementace rozhraní API do projektů klienta.</span><span class="sxs-lookup"><span data-stu-id="3a368-159">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="3a368-160">Další způsoby, jak generovat kód klienta</span><span class="sxs-lookup"><span data-stu-id="3a368-160">Other ways to generate client code</span></span>

<span data-ttu-id="3a368-161">Můžete generovat kód jinými způsoby, další vhodné do pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="3a368-161">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="3a368-162">MSBuild</span><span class="sxs-lookup"><span data-stu-id="3a368-162">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="3a368-163">V kódu</span><span class="sxs-lookup"><span data-stu-id="3a368-163">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="3a368-164">Šablony T4</span><span class="sxs-lookup"><span data-stu-id="3a368-164">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="3a368-165">Přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="3a368-165">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="3a368-166">XML – komentáře</span><span class="sxs-lookup"><span data-stu-id="3a368-166">XML comments</span></span>

<span data-ttu-id="3a368-167">XML – komentáře jsou povolené pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="3a368-167">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="3a368-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3a368-168">Visual Studio</span></span>](#tab/visual-studio-xml)

* <span data-ttu-id="3a368-169">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="3a368-169">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="3a368-170">Zkontrolujte **souborů dokumentace XML** pole v části **výstup** části **sestavení** karty:</span><span class="sxs-lookup"><span data-stu-id="3a368-170">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Sestavení projektu vlastností](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="3a368-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3a368-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml)

* <span data-ttu-id="3a368-173">Otevřete **možnosti projektu** dialogové okno > **sestavení** > **kompilátoru**</span><span class="sxs-lookup"><span data-stu-id="3a368-173">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="3a368-174">Zkontrolujte **generování dokumentace xml** pole v části **Obecné možnosti** části:</span><span class="sxs-lookup"><span data-stu-id="3a368-174">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Obecné možnosti část možnosti projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="3a368-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3a368-176">Visual Studio Code</span></span>](#tab/visual-studio-code-xml)

<span data-ttu-id="3a368-177">Ručně přidejte následující fragment k *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="3a368-177">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

---

## <a name="data-annotations"></a><span data-ttu-id="3a368-178">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="3a368-178">Data annotations</span></span>

<span data-ttu-id="3a368-179">Používá NSwag [reflexe](/dotnet/csharp/programming-guide/concepts/reflection), a doporučený postup pro akce webového rozhraní API je vrátit [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="3a368-179">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="3a368-180">V důsledku toho NSwag nelze odvodit činnosti akci a návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3a368-180">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="3a368-181">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="3a368-181">Consider the following example:</span></span>

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

<span data-ttu-id="3a368-182">Vrátí předchozí akce `IActionResult`, ale uvnitř akce vrací buď [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) nebo [struktura BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="3a368-182">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="3a368-183">Poznámky data se používají k sdělte který odpověď HTTP, tato akce vrací klientů.</span><span class="sxs-lookup"><span data-stu-id="3a368-183">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="3a368-184">Uspořádání akce s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="3a368-184">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="3a368-185">Generátor Swagger můžete nyní přesně popisují tato akce a vygenerovaný klienti věděli, co se zobrazí při volání metody koncový bod.</span><span class="sxs-lookup"><span data-stu-id="3a368-185">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="3a368-186">Architekturu všechny akce s tyto atributy se důrazně doporučuje.</span><span class="sxs-lookup"><span data-stu-id="3a368-186">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="3a368-187">Na jaké odpovědi HTTP vaše rozhraní API akce by měla vrátit, naleznete na adrese [specifikaci RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="3a368-187">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
