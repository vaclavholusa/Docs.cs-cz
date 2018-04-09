---
title: Migrace z ASP.NET Core 1.x na 2.0
author: scottaddie
description: Tento článek popisuje požadavky a nejběžnější kroky migrace projektu ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra.
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: 4b7a13b31340f01c4f1527f602b925d3ac4e8241
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="e0136-103">Migrace z ASP.NET Core 1.x na 2.0</span><span class="sxs-lookup"><span data-stu-id="e0136-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="e0136-104">Podle [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e0136-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e0136-105">V tomto článku vás provedeme prostřednictvím aktualizace existujícího projektu ASP.NET Core 1.x na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="e0136-105">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="e0136-106">Migrace vaší aplikace na technologii ASP.NET Core 2.0 umožňuje využít výhod [mnoho nových funkcí a vylepšení výkonu](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e0136-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="e0136-107">Stávající aplikace ASP.NET Core 1.x vycházejí z šablony projektů specifické pro verzi.</span><span class="sxs-lookup"><span data-stu-id="e0136-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="e0136-108">Jako rozhraní ASP.NET Core zpracovaní, takže udělat šablony projektů a počáteční kód v nich obsažené.</span><span class="sxs-lookup"><span data-stu-id="e0136-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="e0136-109">Kromě aktualizace rozhraní ASP.NET Core, budete muset aktualizovat kód pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e0136-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="e0136-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0136-110">Prerequisites</span></span>
<span data-ttu-id="e0136-111">Najdete v tématu [Začínáme s ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="e0136-111">Please see [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="e0136-112">Aktualizujte cílový Framework Přezdívka (TFM)</span><span class="sxs-lookup"><span data-stu-id="e0136-112">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="e0136-113">Projektech zacílených na .NET Core měli používat [TFM](/dotnet/standard/frameworks#referring-to-frameworks) verze větší než nebo rovna hodnotě .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e0136-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="e0136-114">Vyhledejte `<TargetFramework>` uzlu *.csproj* souboru a nahraďte jeho vnitřní text s `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="e0136-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="e0136-115">Projekty cílení na rozhraní .NET Framework by měli používat TFM verze větší než nebo rovna hodnotě rozhraní .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="e0136-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="e0136-116">Vyhledejte `<TargetFramework>` uzlu *.csproj* souboru a nahraďte jeho vnitřní text s `net461`:</span><span class="sxs-lookup"><span data-stu-id="e0136-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="e0136-117">Rozhraní .NET 2.0 základní nabízí mnohem větší oblasti prostor než .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e0136-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="e0136-118">Pokud jste možnosti cílení na rozhraní .NET Framework výhradně z důvodu chybějících rozhraní API v .NET Core 1.x, cílení na rozhraní .NET Core 2.0 je pravděpodobně fungovat.</span><span class="sxs-lookup"><span data-stu-id="e0136-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="e0136-119">Update .NET Core SDK version in global.json</span><span class="sxs-lookup"><span data-stu-id="e0136-119">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="e0136-120">Pokud vaše řešení závisí na [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) souboru chcete zacílit na konkrétní verzi rozhraní .NET Core SDK, aktualizujte jeho `version` vlastnost na používání verze 2.0, který je nainstalovaný na počítači:</span><span class="sxs-lookup"><span data-stu-id="e0136-120">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="e0136-121">Odkazy na balíčku aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0136-121">Update package references</span></span>
<span data-ttu-id="e0136-122">*.Csproj* souboru v projektu 1.x uvádí každý balíček NuGet používané v projektu.</span><span class="sxs-lookup"><span data-stu-id="e0136-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="e0136-123">V projektu ASP.NET 2.0 základní cílení na rozhraní .NET 2.0 jádra, jeden [metapackage](xref:fundamentals/metapackage) odkaz v *.csproj* souboru nahrazuje kolekce balíčků:</span><span class="sxs-lookup"><span data-stu-id="e0136-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="e0136-124">Všechny funkce technologie ASP.NET 2.0 jádra a Entity Framework Core 2.0 jsou součástí metapackage.</span><span class="sxs-lookup"><span data-stu-id="e0136-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="e0136-125">Projekty ASP.NET Core 2.0 cílení na rozhraní .NET Framework by měly být nadále odkazovat na jednotlivých balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="e0136-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="e0136-126">Aktualizace `Version` atribut jednotlivých `<PackageReference />` uzlu 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="e0136-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="e0136-127">Tady je seznam například `<PackageReference />` použitých v typickém cílení na rozhraní .NET Framework projektu ASP.NET 2.0 základní uzlů:</span><span class="sxs-lookup"><span data-stu-id="e0136-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="e0136-128">Aktualizace nástrojů rozhraní .NET Core rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e0136-128">Update .NET Core CLI tools</span></span>
<span data-ttu-id="e0136-129">V *.csproj* souboru, aktualizovat `Version` atribut jednotlivých `<DotNetCliToolReference />` uzlu 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="e0136-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="e0136-130">Zde je například seznam nástrojů příkazového řádku používá v typickém projektu ASP.NET 2.0 základní cílení na rozhraní .NET 2.0 jádra:</span><span class="sxs-lookup"><span data-stu-id="e0136-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="e0136-131">Přejmenujte vlastnost záložní cíl balíčku</span><span class="sxs-lookup"><span data-stu-id="e0136-131">Rename Package Target Fallback property</span></span>
<span data-ttu-id="e0136-132">*.Csproj* souboru projektu 1.x používá `PackageTargetFallback` uzlu a proměnnou:</span><span class="sxs-lookup"><span data-stu-id="e0136-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="e0136-133">Přejmenovat na uzel a proměnnou `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="e0136-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="e0136-134">Aktualizujte metodu Main v souboru Program.cs</span><span class="sxs-lookup"><span data-stu-id="e0136-134">Update Main method in Program.cs</span></span>
<span data-ttu-id="e0136-135">V projektech 1.x `Main` metodu *Program.cs* hledá takto:</span><span class="sxs-lookup"><span data-stu-id="e0136-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="e0136-136">V projektech 2.0 `Main` metodu *Program.cs* je jednodušší:</span><span class="sxs-lookup"><span data-stu-id="e0136-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="e0136-137">Přijetí této nové 2.0 vzor se důrazně doporučuje a je nutné na funkce produktu jako [migrace základní Entity Framework (EF)](xref:data/ef-mvc/migrations) pracovat.</span><span class="sxs-lookup"><span data-stu-id="e0136-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="e0136-138">Například běžet `Update-Database` z okna konzoly Správce balíčků nebo `dotnet ef database update` z příkazu řádku (na projekty převést na technologii ASP.NET 2.0 základní) generuje následující chybu:</span><span class="sxs-lookup"><span data-stu-id="e0136-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="e0136-139">Přidejte poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="e0136-139">Add configuration providers</span></span>
<span data-ttu-id="e0136-140">V projektech 1.x poskytovatelé konfigurace přidání do aplikace byla provést pomocí `Startup` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="e0136-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="e0136-141">Potřebný postup vytvoření instance `ConfigurationBuilder`, načítání použít poskytovatelé (proměnné prostředí, nastavení aplikace atd.) a inicializace členem `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="e0136-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="e0136-142">Načte v předchozím příkladu `Configuration` člena s nastavením konfigurace z *appSettings.JSON určený* a také všechny *appsettings.\< EnvironmentName\>.json* odpovídající soubor `IHostingEnvironment.EnvironmentName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e0136-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="e0136-143">Umístění těchto souborů se stejnou cestou jako *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e0136-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="e0136-144">V projektech 2.0 spustí servisní často používaný kód konfigurace vyplývajících na 1.x projekty.</span><span class="sxs-lookup"><span data-stu-id="e0136-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="e0136-145">Proměnné prostředí a nastavení aplikace jsou načtena při spuštění.</span><span class="sxs-lookup"><span data-stu-id="e0136-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="e0136-146">Ekvivalent *Startup.cs* kód zkrátila `IConfiguration` inicializace s vloženého instancí:</span><span class="sxs-lookup"><span data-stu-id="e0136-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="e0136-147">Odebrání výchozí zprostředkovatele přidal `WebHostBuilder.CreateDefaultBuilder`, vyvolání `Clear` metodu `IConfigurationBuilder.Sources` vlastnost uvnitř `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="e0136-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="e0136-148">Chcete-li přidat zprostředkovatele zpět, použít `ConfigureAppConfiguration` metoda v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0136-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="e0136-149">Konfigurace používané `CreateDefaultBuilder` metoda v předchozím fragmentu kódu můžete vidět [zde](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="e0136-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="e0136-150">Další informace najdete v tématu [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e0136-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="e0136-151">Přesunutí databáze inicializace kódu</span><span class="sxs-lookup"><span data-stu-id="e0136-151">Move database initialization code</span></span>
<span data-ttu-id="e0136-152">V projektech 1.x pomocí EF základní 1.x, příkazy jako `dotnet ef migrations add` provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="e0136-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="e0136-153">Vytvoří `Startup` instance</span><span class="sxs-lookup"><span data-stu-id="e0136-153">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="e0136-154">Vyvolá `ConfigureServices` metody pro registraci všech služeb pomocí vkládání závislostí (včetně `DbContext` typy)</span><span class="sxs-lookup"><span data-stu-id="e0136-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="e0136-155">Provádí jeho požadavků úlohy</span><span class="sxs-lookup"><span data-stu-id="e0136-155">Performs its requisite tasks</span></span>

<span data-ttu-id="e0136-156">V 2.0 projektů pomocí EF základní 2.0 `Program.BuildWebHost` je vyvolána k získání aplikačních služeb.</span><span class="sxs-lookup"><span data-stu-id="e0136-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="e0136-157">Na rozdíl od 1.x, tato akce nemá další vedlejším účinkem vyvolání `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e0136-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="e0136-158">Pokud je vaše aplikace 1.x vyvolána kód inicializace databáze v jeho `Configure` metoda, může dojít neočekávané problémy.</span><span class="sxs-lookup"><span data-stu-id="e0136-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="e0136-159">Například pokud databáze ještě neexistuje, kód synchronizace replik indexů se spustí před spuštěním příkazu EF základní migrace.</span><span class="sxs-lookup"><span data-stu-id="e0136-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="e0136-160">Tento problém způsobí, že `dotnet ef migrations list` příkaz selže, pokud databáze ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e0136-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="e0136-161">Vezměte v úvahu následující kód inicializace 1.x počáteční hodnoty v `Configure` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0136-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="e0136-162">V projektech 2.0, přesuňte `SeedData.Initialize` volat na `Main` metodu *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0136-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="e0136-163">Od verze 2.0, je chybný postup udělat něco `BuildWebHost` s výjimkou sestavení a konfiguraci webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="e0136-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="e0136-164">Všechno, co je o spuštění aplikace by měly být zpracovány mimo `BuildWebHost` &mdash; obvykle v `Main` metodu *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="e0136-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="e0136-165">Zkontrolujte nastavení kompilace zobrazení syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="e0136-165">Review Razor view compilation setting</span></span>
<span data-ttu-id="e0136-166">Rychlejší doba spuštění aplikace a menší publikované sady mají velký důraz na vás.</span><span class="sxs-lookup"><span data-stu-id="e0136-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="e0136-167">Z těchto důvodů [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) je povoleno ve výchozím nastavení v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="e0136-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="e0136-168">Nastavení `MvcRazorCompileOnPublish` vlastnost na hodnotu true se už nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="e0136-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="e0136-169">Pokud se zakázání kompilace zobrazení, vlastnost mohl být odebrán z *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="e0136-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="e0136-170">Při cílení na rozhraní .NET Framework, stále je třeba explicitně odkazovat [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) balíček NuGet ve vaší *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="e0136-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="e0136-171">Funkce "light-up" Application Insights</span><span class="sxs-lookup"><span data-stu-id="e0136-171">Rely on Application Insights "light-up" features</span></span>
<span data-ttu-id="e0136-172">Snadné nastavení výkonu instrumentace aplikací je důležité.</span><span class="sxs-lookup"><span data-stu-id="e0136-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="e0136-173">Nyní můžete spoléhat na novém [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" funkce dostupné v nástroji Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e0136-173">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="e0136-174">Projekty ASP.NET Core 1.1 vytvořené v aplikaci Visual Studio 2017 přidat Application Insights ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e0136-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="e0136-175">Pokud nepoužíváte Application Insights SDK přímo, mimo *Program.cs* a *Startup.cs*, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e0136-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="e0136-176">Pokud cílení na .NET Core, odeberte následující `<PackageReference />` uzlu z *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="e0136-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="e0136-177">Pokud cílení na .NET Core, odeberte `UseApplicationInsights` volání metody rozšíření z *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0136-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="e0136-178">Odeberte volání rozhraní API klienta služby Application Insights z *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e0136-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="e0136-179">Obsahuje následující dva řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="e0136-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="e0136-180">Pokud používáte Application Insights SDK přímo, nadále používat.</span><span class="sxs-lookup"><span data-stu-id="e0136-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="e0136-181">Rozhraní 2.0 [metapackage](xref:fundamentals/metapackage) obsahuje nejnovější verzi Application Insights, tak k chybě přechod na starší verzi balíčku se zobrazí v případě, že jste odkazující na starší verze.</span><span class="sxs-lookup"><span data-stu-id="e0136-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="e0136-182">Přijmout nebo identitu ověřování vylepšení</span><span class="sxs-lookup"><span data-stu-id="e0136-182">Adopt authentication/Identity improvements</span></span>
<span data-ttu-id="e0136-183">Jádro ASP.NET 2.0 obsahuje nový model ověřování a několik významných změn ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="e0136-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="e0136-184">Pokud jste vytvořili projekt s jednotlivé uživatelské účty, které jsou povolené, nebo pokud jste ručně přidali ověřování nebo Identity, najdete v části [migrovat ověřování a identita na technologii ASP.NET 2.0 základní](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="e0136-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0136-185">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e0136-185">Additional resources</span></span>

* [<span data-ttu-id="e0136-186">Nejnovější změny ve jádro ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="e0136-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
