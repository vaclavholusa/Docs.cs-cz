---
title: Vývoj aplikací ASP.NET Core sledovacích procesů souboru
author: rick-anderson
description: Tento kurz ukazuje, jak nainstalovat a používat nástroje sledovacích procesů (dotnet kukátko) souborů .NET Core CLI v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 016ee107ae646ed43d8a98e97fd2d5b41356910e
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341844"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="aa733-103">Vývoj aplikací ASP.NET Core sledovacích procesů souboru</span><span class="sxs-lookup"><span data-stu-id="aa733-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="aa733-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="aa733-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="aa733-105">`dotnet watch` je nástroj, který běží [.NET Core rozhraní příkazového řádku](/dotnet/core/tools) příkaz Pokud zdrojové soubory změn.</span><span class="sxs-lookup"><span data-stu-id="aa733-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="aa733-106">Změna souboru můžete například aktivovat kompilace, spuštění testu nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="aa733-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="aa733-107">Tento kurz používá existující webové rozhraní API s dva koncové body: jeden, který vrátí součet a jeden, který vrací produktu.</span><span class="sxs-lookup"><span data-stu-id="aa733-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="aa733-108">Metoda produkt obsahuje chyby, která je stanovena v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="aa733-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="aa733-109">Stažení [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="aa733-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="aa733-110">Skládá se ze dvou projektů: *WebApp* (ASP.NET Core webového rozhraní API) a *WebAppTests* (testy částí pro webové rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="aa733-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="aa733-111">V příkazovém řádku přejděte do *WebApp* složky.</span><span class="sxs-lookup"><span data-stu-id="aa733-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="aa733-112">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="aa733-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="aa733-113">Výstup konzoly zobrazí zprávy podobné následujícím (což znamená, že je aplikace spuštěna a čeká na požadavky):</span><span class="sxs-lookup"><span data-stu-id="aa733-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="aa733-114">Ve webovém prohlížeči, přejděte na `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="aa733-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="aa733-115">Měli byste vidět výsledek `9`.</span><span class="sxs-lookup"><span data-stu-id="aa733-115">You should see the result of `9`.</span></span>

<span data-ttu-id="aa733-116">Přejděte do produktu rozhraní API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="aa733-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="aa733-117">Vrátí `9`, nikoli `20` jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="aa733-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="aa733-118">Tento problém vyřešen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="aa733-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="aa733-119">Přidat `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="aa733-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="aa733-120">`dotnet watch` Souboru sledovacích procesů nástroj je součástí verze 2.1.300 .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="aa733-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="aa733-121">Následující kroky jsou požadovány při používání dřívější verzi rozhraní .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="aa733-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="aa733-122">Přidat `Microsoft.DotNet.Watcher.Tools` odkaz na balíček *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="aa733-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="aa733-123">Nainstalujte `Microsoft.DotNet.Watcher.Tools` balíček spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="aa733-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="aa733-124">Spusťte příkazy .NET Core rozhraní příkazového řádku pomocí `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="aa733-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="aa733-125">Všechny [.NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools#cli-commands) lze spustit s `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="aa733-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="aa733-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aa733-126">For example:</span></span>

| <span data-ttu-id="aa733-127">Příkaz</span><span class="sxs-lookup"><span data-stu-id="aa733-127">Command</span></span> | <span data-ttu-id="aa733-128">Příkaz s sledování</span><span class="sxs-lookup"><span data-stu-id="aa733-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="aa733-129">Spustit DotNet.</span><span class="sxs-lookup"><span data-stu-id="aa733-129">dotnet run</span></span> | <span data-ttu-id="aa733-130">Spustit sledování DotNet.</span><span class="sxs-lookup"><span data-stu-id="aa733-130">dotnet watch run</span></span> |
| <span data-ttu-id="aa733-131">Spustit -f netcoreapp2.0 DotNet.</span><span class="sxs-lookup"><span data-stu-id="aa733-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="aa733-132">Spustit -f netcoreapp2.0 sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="aa733-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="aa733-133">Spustit netcoreapp2.0 -f – DotNet – arg1</span><span class="sxs-lookup"><span data-stu-id="aa733-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="aa733-134">kukátko DotNet spustit netcoreapp2.0 -f –--arg1</span><span class="sxs-lookup"><span data-stu-id="aa733-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="aa733-135">test DotNet.</span><span class="sxs-lookup"><span data-stu-id="aa733-135">dotnet test</span></span> | <span data-ttu-id="aa733-136">test sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="aa733-136">dotnet watch test</span></span> |

<span data-ttu-id="aa733-137">Spustit `dotnet watch run` v *WebApp* složky.</span><span class="sxs-lookup"><span data-stu-id="aa733-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="aa733-138">Určuje, bude výstup konzoly `watch` bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="aa733-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="aa733-139">Proveďte změny s `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="aa733-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="aa733-140">Zajistěte, aby `dotnet watch` běží.</span><span class="sxs-lookup"><span data-stu-id="aa733-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="aa733-141">Opravte chyby v `Product` metodu *MathController.cs* tak, aby vracel produktu a nikoli součet:</span><span class="sxs-lookup"><span data-stu-id="aa733-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="aa733-142">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="aa733-142">Save the file.</span></span> <span data-ttu-id="aa733-143">Výstup konzoly znamená, že `dotnet watch` zjištěna změna souboru a restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa733-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="aa733-144">Ověřte `http://localhost:<port number>/api/math/product?a=4&b=5` vrátí výsledek ve správné.</span><span class="sxs-lookup"><span data-stu-id="aa733-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="aa733-145">Spouštění testů pomocí `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="aa733-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="aa733-146">Změna `Product` metodu *MathController.cs* zpět na vrácení součet.</span><span class="sxs-lookup"><span data-stu-id="aa733-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="aa733-147">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="aa733-147">Save the file.</span></span>
1. <span data-ttu-id="aa733-148">V příkazovém řádku přejděte do *WebAppTests* složky.</span><span class="sxs-lookup"><span data-stu-id="aa733-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="aa733-149">Spustit [dotnet obnovení](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="aa733-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="aa733-150">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="aa733-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="aa733-151">Její výstup označuje, že testu se nezdařila a že sledovací proces čeká na změny souboru:</span><span class="sxs-lookup"><span data-stu-id="aa733-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="aa733-152">Opravte `Product` metoda kód tak, aby vracel produktu.</span><span class="sxs-lookup"><span data-stu-id="aa733-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="aa733-153">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="aa733-153">Save the file.</span></span>

<span data-ttu-id="aa733-154">`dotnet watch` zjistí změnu souboru a znovu spustí testy.</span><span class="sxs-lookup"><span data-stu-id="aa733-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="aa733-155">Výstup konzoly znamená, testy úspěšně.</span><span class="sxs-lookup"><span data-stu-id="aa733-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="aa733-156">Přizpůsobení seznamu souborů si chcete přehrát</span><span class="sxs-lookup"><span data-stu-id="aa733-156">Customize files list to watch</span></span>

<span data-ttu-id="aa733-157">Ve výchozím nastavení `dotnet-watch` sleduje všechny soubory odpovídající následující glob vzorce:</span><span class="sxs-lookup"><span data-stu-id="aa733-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="aa733-158">Další položky můžete přidat do seznamu sledování úpravou *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="aa733-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="aa733-159">Položky lze zadat, samostatně nebo pomocí glob vzory.</span><span class="sxs-lookup"><span data-stu-id="aa733-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="aa733-160">Výslovný nesouhlas soubory sledovat</span><span class="sxs-lookup"><span data-stu-id="aa733-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="aa733-161">`dotnet-watch` může být nakonfigurován pro ignorovat výchozího nastavení.</span><span class="sxs-lookup"><span data-stu-id="aa733-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="aa733-162">Ignorovat konkrétní soubory, přidejte `Watch="false"` atribut v definici položky *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="aa733-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="aa733-163">Vlastní sledování projekty</span><span class="sxs-lookup"><span data-stu-id="aa733-163">Custom watch projects</span></span>

<span data-ttu-id="aa733-164">`dotnet-watch` není omezena na projektů C#.</span><span class="sxs-lookup"><span data-stu-id="aa733-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="aa733-165">Vlastní sledování projekty mohou být vytvořeny pro zpracování různých scénářů.</span><span class="sxs-lookup"><span data-stu-id="aa733-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="aa733-166">Vezměte v úvahu následující rozložení projektu:</span><span class="sxs-lookup"><span data-stu-id="aa733-166">Consider the following project layout:</span></span>

* <span data-ttu-id="aa733-167">**testování nebo**</span><span class="sxs-lookup"><span data-stu-id="aa733-167">**test/**</span></span>
  * <span data-ttu-id="aa733-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="aa733-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="aa733-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="aa733-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="aa733-170">Pokud si chcete přehrát obou projektů je cílem, vytvořte soubor vlastních projektů, který je nakonfigurován tak, aby podívejte se na obou projektů:</span><span class="sxs-lookup"><span data-stu-id="aa733-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="aa733-171">Chcete-li spustit soubor sledováním v obou projektů, změňte nastavení na *testování* složky.</span><span class="sxs-lookup"><span data-stu-id="aa733-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="aa733-172">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="aa733-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="aa733-173">VSTest provede, když se změní všechny soubory v buď testovacího projektu.</span><span class="sxs-lookup"><span data-stu-id="aa733-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="aa733-174">`dotnet-watch` v Githubu</span><span class="sxs-lookup"><span data-stu-id="aa733-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="aa733-175">`dotnet-watch` je součástí Githubu [DotNetTools úložiště](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="aa733-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
