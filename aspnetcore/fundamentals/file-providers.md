---
title: "Soubor zprostředkovatele v ASP.NET Core"
author: ardalis
description: "Zjistěte, jak ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: db207f19b7ddc24dea36009138840be6efebdb84
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="ad0db-103">Soubor zprostředkovatele v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad0db-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="ad0db-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ad0db-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ad0db-105">ASP.NET Core abstrahuje přístupu k systému souborů prostřednictvím poskytovatelů souboru.</span><span class="sxs-lookup"><span data-stu-id="ad0db-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="ad0db-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ad0db-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="ad0db-107">Abstrakce zprostředkovatele souboru</span><span class="sxs-lookup"><span data-stu-id="ad0db-107">File Provider abstractions</span></span>

<span data-ttu-id="ad0db-108">Soubor poskytovatelé jsou abstrakci přes systémy souborů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="ad0db-109">Hlavní rozhraní je `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="ad0db-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="ad0db-110">`IFileProvider`zpřístupní metody pro získání informací o souboru (`IFileInfo`), informace o adresáři (`IDirectoryContents`) a k nastavení oznámení o změnách (pomocí `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="ad0db-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="ad0db-111">`IFileInfo`poskytuje metody a vlastnosti o jednotlivé soubory nebo adresáře.</span><span class="sxs-lookup"><span data-stu-id="ad0db-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="ad0db-112">Má dvě logická hodnota vlastnosti `Exists` a `IsDirectory`, a také vlastnosti popisující souboru `Name`, `Length` (v bajtech), a `LastModified` datum.</span><span class="sxs-lookup"><span data-stu-id="ad0db-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="ad0db-113">Můžete si přečíst ze souboru pomocí jeho `CreateReadStream` metoda.</span><span class="sxs-lookup"><span data-stu-id="ad0db-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="ad0db-114">Implementace zprostředkovatele souboru</span><span class="sxs-lookup"><span data-stu-id="ad0db-114">File Provider implementations</span></span>

<span data-ttu-id="ad0db-115">Tři implementace `IFileProvider` jsou k dispozici: fyzické, vložených a složené.</span><span class="sxs-lookup"><span data-stu-id="ad0db-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="ad0db-116">Fyzické poskytovatele používá pro přístup k souborům konkrétního systému.</span><span class="sxs-lookup"><span data-stu-id="ad0db-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="ad0db-117">Vložené zprostředkovatele se používá pro přístup k souborům, které jsou součástí sestavení.</span><span class="sxs-lookup"><span data-stu-id="ad0db-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="ad0db-118">Složené zprostředkovatele slouží k poskytování kombinovaný přístup k souborů a adresářů z jednoho nebo více poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="ad0db-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="ad0db-119">PhysicalFileProvider</span></span>

<span data-ttu-id="ad0db-120">`PhysicalFileProvider` Poskytuje přístup k fyzické systému souborů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="ad0db-121">Zabalí ho `System.IO.File` typu (pro fyzické zprostředkovatele), oborů všechny cesty k adresáři a její podřízené položky.</span><span class="sxs-lookup"><span data-stu-id="ad0db-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="ad0db-122">Tento obor omezuje přístup na určité adresáře a její podřízené položky, brání přístupu k systému souborů mimo tuto hranici.</span><span class="sxs-lookup"><span data-stu-id="ad0db-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="ad0db-123">Při vytváření instance tohoto zprostředkovatele, je nutné zadat s cestu k adresáři, který slouží jako základní cesta pro všechny požadavky provedené pro tohoto zprostředkovatele (a která omezuje přístup mimo tuto cestu).</span><span class="sxs-lookup"><span data-stu-id="ad0db-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="ad0db-124">V aplikaci ASP.NET Core, můžete vytvořit instanci `PhysicalFileProvider` zprostředkovatele přímo, nebo můžete požádat o `IFileProvider` v řadiči nebo konstruktor služby prostřednictvím [vkládání závislostí](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ad0db-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="ad0db-125">Pozdější přístup obvykle předá flexibilnější a možností intenzivního testování řešení.</span><span class="sxs-lookup"><span data-stu-id="ad0db-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="ad0db-126">Následující ukázka ukazuje, jak vytvořit `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="ad0db-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="ad0db-127">Můžete iteraci v rámci jeho obsah adresáře nebo získat informace o konkrétní soubor tím, že poskytuje je její částí.</span><span class="sxs-lookup"><span data-stu-id="ad0db-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="ad0db-128">K vyžádání zprostředkovatele z řadič, zadejte v konstruktoru kontroleru a přiřaďte ho pro místní pole.</span><span class="sxs-lookup"><span data-stu-id="ad0db-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="ad0db-129">Použijte místní instanci z vaší metody akce:</span><span class="sxs-lookup"><span data-stu-id="ad0db-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="ad0db-130">Pak vytvořte poskytovatele v dané aplikaci `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="ad0db-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="ad0db-131">V *Index.cshtml* zobrazit, iteraci v rámci `IDirectoryContents` poskytuje:</span><span class="sxs-lookup"><span data-stu-id="ad0db-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="ad0db-132">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="ad0db-132">The result:</span></span>

![Soubor zprostředkovatele ukázkové aplikace výpis fyzické soubory a složky](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="ad0db-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="ad0db-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="ad0db-135">`EmbeddedFileProvider` Se používá pro přístup k souborům, které jsou součástí sestavení.</span><span class="sxs-lookup"><span data-stu-id="ad0db-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="ad0db-136">V rozhraní .NET Core, vložíte do sestavení se soubory `<EmbeddedResource>` element v *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="ad0db-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="ad0db-137">Můžete použít [expanze názvů vzory](#globbing-patterns) při zadávání soubory pro vložení do sestavení.</span><span class="sxs-lookup"><span data-stu-id="ad0db-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="ad0db-138">Tyto vzory použít tak, aby odpovídaly jeden nebo více souborů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="ad0db-139">Pravděpodobně že by někdy budete chtít ve skutečnosti vložení každý soubor .js ve vašem projektu v jeho sestavení; ukázka výše je ukázkový pouze pro účely.</span><span class="sxs-lookup"><span data-stu-id="ad0db-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="ad0db-140">Při vytváření `EmbeddedFileProvider`, předat sestavení, zobrazí se informace pro jeho konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ad0db-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="ad0db-141">Demonstruje postup vytvoření výše uvedeném fragmentu `EmbeddedFileProvider` s přístupem k aktuálně prováděné sestavení.</span><span class="sxs-lookup"><span data-stu-id="ad0db-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="ad0db-142">Aktualizace ukázková aplikace sloužící `EmbeddedFileProvider` výsledkem následující výstup:</span><span class="sxs-lookup"><span data-stu-id="ad0db-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Soubor zprostředkovatele ukázkovou aplikaci seznam vložených souborů](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="ad0db-144">Vložené prostředky nezveřejňují adresáře.</span><span class="sxs-lookup"><span data-stu-id="ad0db-144">Embedded resources do not expose directories.</span></span> <span data-ttu-id="ad0db-145">Místo toho je cesta k prostředek (prostřednictvím svého oboru názvů) vložených v jeho název souboru pomocí `.` oddělovačů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="ad0db-146">`EmbeddedFileProvider` Konstruktor přijímá volitelný `baseNamespace` parametr.</span><span class="sxs-lookup"><span data-stu-id="ad0db-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="ad0db-147">Určení to bude obor volání `GetDirectoryContents` na tyto prostředky v rámci zadaného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="ad0db-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="ad0db-148">CompositeFileProvider</span></span>

<span data-ttu-id="ad0db-149">`CompositeFileProvider` Kombinuje `IFileProvider` instancí vystavení jednoho rozhraní pro práci se soubory z více poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="ad0db-150">Při vytváření `CompositeFileProvider`, předáte jeden nebo více `IFileProvider` instance jeho konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="ad0db-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="ad0db-151">Aktualizace ukázková aplikace používat `CompositeFileProvider` která obsahuje oba fyzické a embedded zprostředkovatele dříve nakonfigurován, výsledkem následující výstup:</span><span class="sxs-lookup"><span data-stu-id="ad0db-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Soubor zprostředkovatele ukázkovou aplikaci výpis fyzické soubory a složky a vložené soubory](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="ad0db-153">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="ad0db-153">Watching for changes</span></span>

<span data-ttu-id="ad0db-154">`IFileProvider` `Watch` Metoda poskytuje způsob, jak sledovat jeden nebo více souborů či adresářů pro změny.</span><span class="sxs-lookup"><span data-stu-id="ad0db-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="ad0db-155">Tato metoda přijímá řetězec cesty, které můžete použít [expanze názvů vzory](#globbing-patterns) zadat více souborů a vrátí `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="ad0db-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="ad0db-156">Tento token zpřístupní `HasChanged` vlastnost, která může být prověřovány, a `RegisterChangeCallback` metoda, která je volána při zjištění změny na zadané cestě řetězec.</span><span class="sxs-lookup"><span data-stu-id="ad0db-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="ad0db-157">Všimněte si, že každý token změnu pouze volá jeho přidružené zpětného volání v reakci na jediné změny.</span><span class="sxs-lookup"><span data-stu-id="ad0db-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="ad0db-158">Chcete-li povolit konstantní monitorování, můžete použít `TaskCompletionSource` jak je uvedeno níže, nebo znovu vytvořte `IChangeToken` instancí v reakci na změny.</span><span class="sxs-lookup"><span data-stu-id="ad0db-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="ad0db-159">V tomto článku ukázce je konzolová aplikace nakonfigurován k zobrazení zprávy změně textového souboru je:</span><span class="sxs-lookup"><span data-stu-id="ad0db-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="ad0db-160">Výsledek po uložení souboru několikrát:</span><span class="sxs-lookup"><span data-stu-id="ad0db-160">The result, after saving the file several times:</span></span>

![Příkazové okno po provádění dotnet. Spusťte ukazuje na soubor quotes.txt změny monitorování aplikací a pětkrát došlo ke změně souboru.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="ad0db-162">Některé systémy souborů, jako je například Docker kontejnery a sdílené síťové složky a nemusí spolehlivě odeslat oznámení o změnách.</span><span class="sxs-lookup"><span data-stu-id="ad0db-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="ad0db-163">Nastavte `DOTNET_USE_POLLINGFILEWATCHER` proměnnou prostředí `1` nebo `true` k dotazování systému souborů pro změny každé 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="ad0db-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="ad0db-164">Vzory expanze názvů</span><span class="sxs-lookup"><span data-stu-id="ad0db-164">Globbing patterns</span></span>

<span data-ttu-id="ad0db-165">Systémové cesty k souborům použít zástupné znaky názvem *expanze názvů vzory*.</span><span class="sxs-lookup"><span data-stu-id="ad0db-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="ad0db-166">Tyto jednoduché vzory slouží k určení skupiny souborů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="ad0db-167">Dva zástupné znaky jsou `*` a `**`.</span><span class="sxs-lookup"><span data-stu-id="ad0db-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="ad0db-168">Odpovídá nic na aktuální úrovni složky, nebo libovolný název souboru nebo libovolnou příponou.</span><span class="sxs-lookup"><span data-stu-id="ad0db-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="ad0db-169">Shoduje se ukončila příkazem `/` a `.` znaků v cestě k souboru.</span><span class="sxs-lookup"><span data-stu-id="ad0db-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="ad0db-170">Odpovídá všemu napříč více úrovní adresáře.</span><span class="sxs-lookup"><span data-stu-id="ad0db-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="ad0db-171">Slouží k rekurzivnímu odpovídat mnoho souborů v rámci hierarchie adresářů.</span><span class="sxs-lookup"><span data-stu-id="ad0db-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="ad0db-172">Příklady vzor expanze názvů</span><span class="sxs-lookup"><span data-stu-id="ad0db-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="ad0db-173">Odpovídá konkrétní soubor v konkrétní adresář.</span><span class="sxs-lookup"><span data-stu-id="ad0db-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="ad0db-174">Odpovídá všechny soubory s `.txt` rozšíření v konkrétní adresář.</span><span class="sxs-lookup"><span data-stu-id="ad0db-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="ad0db-175">Odpovídá všem `bower.json` soubory v adresářích přesně jednu úroveň níže `directory` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ad0db-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="ad0db-176">Odpovídá všechny soubory s `.txt` nalezeno rozšíření kdekoli v části `directory` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ad0db-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="ad0db-177">Soubor zprostředkovatele využití v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad0db-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="ad0db-178">Několik částí ASP.NET Core využívat souboru zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ad0db-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="ad0db-179">`IHostingEnvironment`zpřístupní obsahu kořenové aplikace a webové kořenový jako `IFileProvider` typy.</span><span class="sxs-lookup"><span data-stu-id="ad0db-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="ad0db-180">Middleware statické soubory používá zprostředkovatele souboru k vyhledání statické soubory.</span><span class="sxs-lookup"><span data-stu-id="ad0db-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="ad0db-181">Syntaxe Razor umožňuje výrazně využívá `IFileProvider` v vyhledání zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ad0db-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="ad0db-182">Pro DotNet publikování funkce používá soubor zprostředkovatele a vzory expanze názvů určit soubory, které je nutné ji publikovat.</span><span class="sxs-lookup"><span data-stu-id="ad0db-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="ad0db-183">Doporučení pro použití v aplikacích</span><span class="sxs-lookup"><span data-stu-id="ad0db-183">Recommendations for use in apps</span></span>

<span data-ttu-id="ad0db-184">Pokud vaše aplikace ASP.NET Core vyžaduje přístupu k systému souborů, můžete požádat o instanci `IFileProvider` pomocí vkládání závislostí a pak použít její metody k provádění přístupu, jak je znázorněno v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="ad0db-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="ad0db-185">To umožňuje konfiguraci poskytovatele za jednou, když se aplikace spouští a snižuje počet typů implementace, které vaše aplikace vytvoří.</span><span class="sxs-lookup"><span data-stu-id="ad0db-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
