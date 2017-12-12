---
title: "Vývoj aplikací ASP.NET Core pomocí sledování dotnet."
author: rick-anderson
description: "Tento kurz ukazuje, jak nainstalovat a používat nástroje sledovacích procesů (dotnet kukátko) souborů .NET Core CLI v aplikaci ASP.NET Core."
keywords: "ASP.NET Core, pomocí sledování dotnet."
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 9baf2ce2a1270a728616a8a2ab45deca9a9cde6f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="2bc33-104">Vývoj aplikací ASP.NET Core pomocí sledování dotnet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="2bc33-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="2bc33-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="2bc33-106">`dotnet watch`je nástroj, který běží [.NET Core rozhraní příkazového řádku](/dotnet/core/tools) příkaz Pokud zdrojové soubory změn.</span><span class="sxs-lookup"><span data-stu-id="2bc33-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="2bc33-107">Změna souboru můžete například aktivovat kompilace, spuštění testu nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="2bc33-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="2bc33-108">V tomto kurzu budeme používat existující aplikace webového rozhraní API s dva koncové body: jeden, který vrátí součet a jeden, který vrací produktu.</span><span class="sxs-lookup"><span data-stu-id="2bc33-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="2bc33-109">Metoda produkt obsahuje chybu, která jsme budete opravit v rámci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2bc33-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="2bc33-110">Stažení [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="2bc33-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="2bc33-111">Obsahuje dva projekty: *WebApp* (pro ASP.NET Core Web API) a *WebAppTests* (testy částí pro rozhraní Web API).</span><span class="sxs-lookup"><span data-stu-id="2bc33-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="2bc33-112">V příkazovém řádku přejděte do *WebApp* složky a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2bc33-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="2bc33-113">Výstup konzoly zobrazí zprávy podobné následujícím (což znamená, že je aplikace spuštěna a čeká na požadavky):</span><span class="sxs-lookup"><span data-stu-id="2bc33-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="2bc33-114">Ve webovém prohlížeči, přejděte na `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="2bc33-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="2bc33-115">Měli byste vidět výsledek `9`.</span><span class="sxs-lookup"><span data-stu-id="2bc33-115">You should see the result of `9`.</span></span>

<span data-ttu-id="2bc33-116">Přejděte do produktu rozhraní API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="2bc33-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="2bc33-117">Vrátí `9`, nikoli `20` jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="2bc33-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="2bc33-118">To jsme budete opravíme později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2bc33-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="2bc33-119">Přidat `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="2bc33-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="2bc33-120">Přidat `Microsoft.DotNet.Watcher.Tools` odkaz na balíček *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="2bc33-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="2bc33-121">Nainstalujte `Microsoft.DotNet.Watcher.Tools` balíček spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2bc33-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="2bc33-122">Spuštění příkazů .NET Core rozhraní příkazového řádku pomocí`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="2bc33-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="2bc33-123">Všechny [.NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools#cli-commands) lze spustit s `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="2bc33-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="2bc33-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2bc33-124">For example:</span></span>

| <span data-ttu-id="2bc33-125">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2bc33-125">Command</span></span> | <span data-ttu-id="2bc33-126">Příkaz s sledování</span><span class="sxs-lookup"><span data-stu-id="2bc33-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="2bc33-127">Spustit DotNet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-127">dotnet run</span></span> | <span data-ttu-id="2bc33-128">Spustit sledování DotNet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-128">dotnet watch run</span></span> |
| <span data-ttu-id="2bc33-129">Spustit -f netcoreapp2.0 DotNet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="2bc33-130">Spustit -f netcoreapp2.0 sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="2bc33-131">Spustit netcoreapp2.0 -f – DotNet – arg1</span><span class="sxs-lookup"><span data-stu-id="2bc33-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="2bc33-132">kukátko DotNet spustit netcoreapp2.0 -f –--arg1</span><span class="sxs-lookup"><span data-stu-id="2bc33-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="2bc33-133">test DotNet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-133">dotnet test</span></span> | <span data-ttu-id="2bc33-134">test sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="2bc33-134">dotnet watch test</span></span> |

<span data-ttu-id="2bc33-135">Spustit `dotnet watch run` v *WebApp* složky.</span><span class="sxs-lookup"><span data-stu-id="2bc33-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="2bc33-136">Určuje, bude výstup konzoly `watch` bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="2bc33-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="2bc33-137">Provedení změn pomocí`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="2bc33-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="2bc33-138">Zajistěte, aby `dotnet watch` běží.</span><span class="sxs-lookup"><span data-stu-id="2bc33-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="2bc33-139">Opravte chyby v `Product` metodu *MathController.cs* tak, aby vracel produktu a nikoli součet:</span><span class="sxs-lookup"><span data-stu-id="2bc33-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="2bc33-140">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="2bc33-140">Save the file.</span></span> <span data-ttu-id="2bc33-141">Výstup konzoly znamená, že `dotnet watch` zjištěna změna souboru a restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="2bc33-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="2bc33-142">Ověřte `http://localhost:<port number>/api/math/product?a=4&b=5` vrátí výsledek ve správné.</span><span class="sxs-lookup"><span data-stu-id="2bc33-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="2bc33-143">Spouštění testů pomocí aplikace`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="2bc33-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="2bc33-144">Změna `Product` metodu *MathController.cs* zpět do vrací součet a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="2bc33-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="2bc33-145">V příkazovém řádku přejděte do *WebAppTests* složky.</span><span class="sxs-lookup"><span data-stu-id="2bc33-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="2bc33-146">Run `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="2bc33-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="2bc33-147">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="2bc33-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="2bc33-148">Její výstup označuje, že test se nezdařil a že sledovací proces čeká na změny souboru:</span><span class="sxs-lookup"><span data-stu-id="2bc33-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="2bc33-149">Opravte `Product` metoda kód tak, aby vracel produktu.</span><span class="sxs-lookup"><span data-stu-id="2bc33-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="2bc33-150">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="2bc33-150">Save the file.</span></span>

<span data-ttu-id="2bc33-151">`dotnet watch`zjistí změnu souboru a znovu spustí testy.</span><span class="sxs-lookup"><span data-stu-id="2bc33-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="2bc33-152">Výstup konzoly znamená, testy úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2bc33-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="2bc33-153">kukátko DotNet v Githubu</span><span class="sxs-lookup"><span data-stu-id="2bc33-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="2bc33-154">kukátko DotNet je součástí Githubu [DotNetTools úložiště](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span><span class="sxs-lookup"><span data-stu-id="2bc33-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="2bc33-155">[MSBuild části](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) z [dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) vysvětluje, jak sledovat dotnet. lze konfigurovat v souboru projektu nástroje MSBuild sledovány.</span><span class="sxs-lookup"><span data-stu-id="2bc33-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="2bc33-156">[Dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) obsahuje informace o dotnet sledování nejsou zahrnuté v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2bc33-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
