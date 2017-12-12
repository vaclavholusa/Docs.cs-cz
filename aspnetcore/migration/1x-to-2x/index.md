---
title: Migrace z ASP.NET Core 1.x na 2.0
author: scottaddie
description: "Tento článek popisuje požadavky a nejběžnější kroky migrace projektu ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra."
keywords: ASP.NET Core, migrace
ms.author: scaddie
manager: wpickett
ms.date: 10/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: 12734504953f2942458c3bfe1fe146f48d8f24ff
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="b15e1-104">Migrace z ASP.NET Core 1.x na jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="b15e1-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="b15e1-105">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b15e1-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="b15e1-106">V tomto článku vás provedeme prostřednictvím aktualizace existujícího projektu ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="b15e1-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="b15e1-107">Migrace vaší aplikace na technologii ASP.NET Core 2.0 umožňuje využít výhod [mnoho nových funkcí a vylepšení výkonu](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="b15e1-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="b15e1-108">Stávající aplikace ASP.NET Core 1.x vycházejí z šablony projektů specifické pro verzi.</span><span class="sxs-lookup"><span data-stu-id="b15e1-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="b15e1-109">Jako rozhraní ASP.NET Core zpracovaní, takže udělat šablony projektů a počáteční kód v nich obsažené.</span><span class="sxs-lookup"><span data-stu-id="b15e1-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="b15e1-110">Kromě aktualizace rozhraní ASP.NET Core, budete muset aktualizovat kód pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b15e1-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b15e1-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b15e1-111">Prerequisites</span></span>
<span data-ttu-id="b15e1-112">Najdete v tématu [Začínáme s ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="b15e1-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="b15e1-113">Aktualizujte cílový Framework Přezdívka (TFM)</span><span class="sxs-lookup"><span data-stu-id="b15e1-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="b15e1-114">Projektech zacílených na .NET Core měli používat [TFM](/dotnet/standard/frameworks#referring-to-frameworks) verze větší než nebo rovna hodnotě .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b15e1-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="b15e1-115">Vyhledejte `<TargetFramework>` uzlu *.csproj* souboru a nahraďte jeho vnitřní text s `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="b15e1-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="b15e1-116">Projekty cílení na rozhraní .NET Framework by měli používat TFM verze větší než nebo rovna hodnotě rozhraní .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="b15e1-116">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="b15e1-117">Vyhledejte `<TargetFramework>` uzlu *.csproj* souboru a nahraďte jeho vnitřní text s `net461`:</span><span class="sxs-lookup"><span data-stu-id="b15e1-117">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="b15e1-118">Rozhraní .NET 2.0 základní nabízí mnohem větší oblasti prostor než .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b15e1-118">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="b15e1-119">Pokud jste možnosti cílení na rozhraní .NET Framework výhradně z důvodu chybějících rozhraní API v .NET Core 1.x, cílení na rozhraní .NET Core 2.0 je pravděpodobně fungovat.</span><span class="sxs-lookup"><span data-stu-id="b15e1-119">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="b15e1-120">Verze rozhraní .NET Core SDK aktualizace v global.json</span><span class="sxs-lookup"><span data-stu-id="b15e1-120">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="b15e1-121">Pokud vaše řešení závisí na [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) souboru chcete zacílit na konkrétní verzi rozhraní .NET Core SDK, aktualizujte jeho `version` vlastnost na používání verze 2.0, který je nainstalovaný na počítači:</span><span class="sxs-lookup"><span data-stu-id="b15e1-121">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="b15e1-122">Odkazy na balíčku aktualizace</span><span class="sxs-lookup"><span data-stu-id="b15e1-122">Update package references</span></span>
<span data-ttu-id="b15e1-123">*.Csproj* souboru v projektu 1.x uvádí každý balíček NuGet používané v projektu.</span><span class="sxs-lookup"><span data-stu-id="b15e1-123">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="b15e1-124">V projektu ASP.NET 2.0 základní cílení na rozhraní .NET 2.0 jádra, jeden [metapackage](xref:fundamentals/metapackage) odkaz v *.csproj* souboru nahrazuje kolekce balíčků:</span><span class="sxs-lookup"><span data-stu-id="b15e1-124">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="b15e1-125">Všechny funkce technologie ASP.NET 2.0 jádra a Entity Framework Core 2.0 jsou součástí metapackage.</span><span class="sxs-lookup"><span data-stu-id="b15e1-125">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="b15e1-126">Projekty ASP.NET Core 2.0 cílení na rozhraní .NET Framework by měly být nadále odkazovat na jednotlivých balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="b15e1-126">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="b15e1-127">Aktualizace `Version` atribut jednotlivých `<PackageReference />` uzlu 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="b15e1-127">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="b15e1-128">Tady je seznam například `<PackageReference />` použitých v typickém cílení na rozhraní .NET Framework projektu ASP.NET 2.0 základní uzlů:</span><span class="sxs-lookup"><span data-stu-id="b15e1-128">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="b15e1-129">Aktualizace nástrojů rozhraní .NET Core rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b15e1-129">Update .NET Core CLI tools</span></span>
<span data-ttu-id="b15e1-130">V *.csproj* souboru, aktualizovat `Version` atribut jednotlivých `<DotNetCliToolReference />` uzlu 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="b15e1-130">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="b15e1-131">Zde je například seznam nástrojů příkazového řádku používá v typickém projektu ASP.NET 2.0 základní cílení na rozhraní .NET 2.0 jádra:</span><span class="sxs-lookup"><span data-stu-id="b15e1-131">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="b15e1-132">Přejmenujte vlastnost záložní cíl balíčku</span><span class="sxs-lookup"><span data-stu-id="b15e1-132">Rename Package Target Fallback property</span></span>
<span data-ttu-id="b15e1-133">*.Csproj* souboru projektu 1.x používá `PackageTargetFallback` uzlu a proměnnou:</span><span class="sxs-lookup"><span data-stu-id="b15e1-133">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="b15e1-134">Přejmenovat na uzel a proměnnou `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="b15e1-134">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="b15e1-135">Aktualizujte metodu Main v souboru Program.cs</span><span class="sxs-lookup"><span data-stu-id="b15e1-135">Update Main method in Program.cs</span></span>
<span data-ttu-id="b15e1-136">V projektech 1.x `Main` metodu *Program.cs* hledá takto:</span><span class="sxs-lookup"><span data-stu-id="b15e1-136">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="b15e1-137">V projektech 2.0 `Main` metodu *Program.cs* je jednodušší:</span><span class="sxs-lookup"><span data-stu-id="b15e1-137">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="b15e1-138">Přijetí této nové 2.0 vzor se důrazně doporučuje a je nutné na funkce produktu jako [migrace základní Entity Framework (EF)](xref:data/ef-mvc/migrations) pracovat.</span><span class="sxs-lookup"><span data-stu-id="b15e1-138">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="b15e1-139">Například běžet `Update-Database` z okna konzoly Správce balíčků nebo `dotnet ef database update` z příkazu řádku (na projekty převést na technologii ASP.NET 2.0 základní) generuje následující chybu:</span><span class="sxs-lookup"><span data-stu-id="b15e1-139">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="b15e1-140">Přidejte poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="b15e1-140">Add configuration providers</span></span>
<span data-ttu-id="b15e1-141">V projektech 1.x poskytovatelé konfigurace přidání do aplikace byla provést pomocí `Startup` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="b15e1-141">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="b15e1-142">Potřebný postup vytvoření instance `ConfigurationBuilder`, načítání použít poskytovatelé (proměnné prostředí, nastavení aplikace atd.) a inicializace členem `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="b15e1-142">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="b15e1-143">Načte v předchozím příkladu `Configuration` člena s nastavením konfigurace z *appSettings.JSON určený* a také všechny *appsettings.\< EnvironmentName\>.json* odpovídající soubor `IHostingEnvironment.EnvironmentName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b15e1-143">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="b15e1-144">Umístění těchto souborů se stejnou cestou jako *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b15e1-144">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="b15e1-145">V projektech 2.0 spustí servisní často používaný kód konfigurace vyplývajících na 1.x projekty.</span><span class="sxs-lookup"><span data-stu-id="b15e1-145">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="b15e1-146">Proměnné prostředí a nastavení aplikace jsou načtena při spuštění.</span><span class="sxs-lookup"><span data-stu-id="b15e1-146">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="b15e1-147">Ekvivalent *Startup.cs* kód zkrátila `IConfiguration` inicializace s vloženého instancí:</span><span class="sxs-lookup"><span data-stu-id="b15e1-147">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="b15e1-148">Odebrání výchozí zprostředkovatele přidal `WebHostBuilder.CreateDefaultBuilder`, vyvolání `Clear` metodu `IConfigurationBuilder.Sources` vlastnost uvnitř `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="b15e1-148">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="b15e1-149">Chcete-li přidat zprostředkovatele zpět, použít `ConfigureAppConfiguration` metoda v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b15e1-149">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="b15e1-150">Konfigurace používané `CreateDefaultBuilder` metoda v předchozím fragmentu kódu můžete vidět [zde](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="b15e1-150">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="b15e1-151">Další informace najdete v tématu [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b15e1-151">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="b15e1-152">Přesunutí databáze inicializace kódu</span><span class="sxs-lookup"><span data-stu-id="b15e1-152">Move database initialization code</span></span>
<span data-ttu-id="b15e1-153">V projektech 1.x pomocí EF základní 1.x, příkazy jako `dotnet ef migrations add` provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="b15e1-153">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="b15e1-154">Vytvoří `Startup` instance</span><span class="sxs-lookup"><span data-stu-id="b15e1-154">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="b15e1-155">Vyvolá `ConfigureServices` metody pro registraci všech služeb pomocí vkládání závislostí (včetně `DbContext` typy)</span><span class="sxs-lookup"><span data-stu-id="b15e1-155">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="b15e1-156">Provádí jeho požadavků úlohy</span><span class="sxs-lookup"><span data-stu-id="b15e1-156">Performs its requisite tasks</span></span>

<span data-ttu-id="b15e1-157">V 2.0 projektů pomocí EF základní 2.0 `Program.BuildWebHost` je vyvolána k získání aplikačních služeb.</span><span class="sxs-lookup"><span data-stu-id="b15e1-157">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="b15e1-158">Na rozdíl od 1.x, tato akce nemá další vedlejším účinkem vyvolání `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b15e1-158">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="b15e1-159">Pokud je vaše aplikace 1.x vyvolána kód inicializace databáze v jeho `Configure` metoda, může dojít neočekávané problémy.</span><span class="sxs-lookup"><span data-stu-id="b15e1-159">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="b15e1-160">Například pokud databáze ještě neexistuje, kód synchronizace replik indexů se spustí před spuštěním příkazu EF základní migrace.</span><span class="sxs-lookup"><span data-stu-id="b15e1-160">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="b15e1-161">Tento problém způsobí, že `dotnet ef migrations list` příkaz selže, pokud databáze ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b15e1-161">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="b15e1-162">Vezměte v úvahu následující kód inicializace 1.x počáteční hodnoty v `Configure` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b15e1-162">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="b15e1-163">V projektech 2.0, přesuňte `SeedData.Initialize` volat na `Main` metodu *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b15e1-163">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="b15e1-164">Od verze 2.0, je chybný postup udělat něco `BuildWebHost` s výjimkou sestavení a konfiguraci webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="b15e1-164">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="b15e1-165">Všechno, co je o spuštění aplikace by měly být zpracovány mimo `BuildWebHost` &mdash; obvykle v `Main` metodu *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="b15e1-165">Anything that is about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="b15e1-166">Zkontrolujte vaše nastavení kompilace zobrazení syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="b15e1-166">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="b15e1-167">Rychlejší doba spuštění aplikace a menší publikované sady mají velký důraz na vás.</span><span class="sxs-lookup"><span data-stu-id="b15e1-167">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="b15e1-168">Z těchto důvodů [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) je povoleno ve výchozím nastavení v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="b15e1-168">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="b15e1-169">Nastavení `MvcRazorCompileOnPublish` vlastnost na hodnotu true se už nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="b15e1-169">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="b15e1-170">Pokud se zakázání kompilace zobrazení, vlastnost mohl být odebrán z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="b15e1-170">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="b15e1-171">Při cílení na rozhraní .NET Framework, stále je třeba explicitně odkazovat [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) balíček NuGet ve vaší *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="b15e1-171">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="b15e1-172">Funkce Application Insights "Light-Up"</span><span class="sxs-lookup"><span data-stu-id="b15e1-172">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="b15e1-173">Snadné nastavení výkonu instrumentace aplikací je důležité.</span><span class="sxs-lookup"><span data-stu-id="b15e1-173">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="b15e1-174">Nyní můžete spoléhat na novém [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" funkce dostupné v nástroji Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b15e1-174">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="b15e1-175">Projekty ASP.NET Core 1.1 vytvořené v aplikaci Visual Studio 2017 přidat Application Insights ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b15e1-175">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="b15e1-176">Pokud nepoužíváte Application Insights SDK přímo, mimo *Program.cs* a *Startup.cs*, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b15e1-176">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="b15e1-177">Pokud cílení na .NET Core, odeberte následující `<PackageReference />` uzlu z *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="b15e1-177">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="b15e1-178">Pokud cílení na .NET Core, odeberte `UseApplicationInsights` volání metody rozšíření z *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b15e1-178">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="b15e1-179">Odeberte volání rozhraní API klienta služby Application Insights z *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b15e1-179">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="b15e1-180">Obsahuje následující dva řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="b15e1-180">It comprises the following two lines of code:</span></span>

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="b15e1-181">Pokud používáte Application Insights SDK přímo, nadále používat.</span><span class="sxs-lookup"><span data-stu-id="b15e1-181">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="b15e1-182">Rozhraní 2.0 [metapackage](xref:fundamentals/metapackage) obsahuje nejnovější verzi Application Insights, tak k chybě přechod na starší verzi balíčku se zobrazí v případě, že jste odkazující na starší verze.</span><span class="sxs-lookup"><span data-stu-id="b15e1-182">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="b15e1-183">Přijmout ověřování nebo vylepšení Identity</span><span class="sxs-lookup"><span data-stu-id="b15e1-183">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="b15e1-184">Jádro ASP.NET 2.0 obsahuje nový model ověřování a několik významných změn ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="b15e1-184">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="b15e1-185">Pokud jste vytvořili projekt s jednotlivé uživatelské účty, které jsou povolené, nebo pokud jste ručně přidali ověřování nebo Identity, najdete v části [migrace ověřování a identita na technologii ASP.NET 2.0 základní](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="b15e1-185">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b15e1-186">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="b15e1-186">Additional Resources</span></span>
- [<span data-ttu-id="b15e1-187">Nejnovější změny ve jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="b15e1-187">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
