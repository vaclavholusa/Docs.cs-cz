---
title: "Vývoj aplikací ASP.NET Core pomocí sledování dotnet."
author: rick-anderson
description: "Tento kurz ukazuje, jak nainstalovat a používat nástroje sledovacích procesů (dotnet kukátko) souborů .NET Core CLI v aplikaci ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: cb15e28cb98ea82091cf5ddeed12df8926079e52
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="1184c-103">Vývoj aplikací ASP.NET Core pomocí sledování dotnet.</span><span class="sxs-lookup"><span data-stu-id="1184c-103">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="1184c-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="1184c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="1184c-105">`dotnet watch`je nástroj, který běží [.NET Core rozhraní příkazového řádku](/dotnet/core/tools) příkaz Pokud zdrojové soubory změn.</span><span class="sxs-lookup"><span data-stu-id="1184c-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="1184c-106">Změna souboru můžete například aktivovat kompilace, spuštění testu nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="1184c-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="1184c-107">V tomto kurzu budeme používat existující aplikace webového rozhraní API s dva koncové body: jeden, který vrátí součet a jeden, který vrací produktu.</span><span class="sxs-lookup"><span data-stu-id="1184c-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="1184c-108">Metoda produkt obsahuje chybu, která jsme budete opravit v rámci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1184c-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="1184c-109">Stažení [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="1184c-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="1184c-110">Obsahuje dva projekty: *WebApp* (pro ASP.NET Core Web API) a *WebAppTests* (testy částí pro rozhraní Web API).</span><span class="sxs-lookup"><span data-stu-id="1184c-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="1184c-111">V příkazovém řádku přejděte do *WebApp* složky a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1184c-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="1184c-112">Výstup konzoly zobrazí zprávy podobné následujícím (což znamená, že je aplikace spuštěna a čeká na požadavky):</span><span class="sxs-lookup"><span data-stu-id="1184c-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="1184c-113">Ve webovém prohlížeči, přejděte na `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="1184c-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="1184c-114">Měli byste vidět výsledek `9`.</span><span class="sxs-lookup"><span data-stu-id="1184c-114">You should see the result of `9`.</span></span>

<span data-ttu-id="1184c-115">Přejděte do produktu rozhraní API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="1184c-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="1184c-116">Vrátí `9`, nikoli `20` jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="1184c-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="1184c-117">To jsme budete opravíme později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1184c-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="1184c-118">Přidat `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="1184c-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="1184c-119">Přidat `Microsoft.DotNet.Watcher.Tools` odkaz na balíček *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="1184c-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="1184c-120">Nainstalujte `Microsoft.DotNet.Watcher.Tools` balíček spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="1184c-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="1184c-121">Spuštění příkazů .NET Core rozhraní příkazového řádku pomocí`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="1184c-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="1184c-122">Všechny [.NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools#cli-commands) lze spustit s `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="1184c-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="1184c-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1184c-123">For example:</span></span>

| <span data-ttu-id="1184c-124">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1184c-124">Command</span></span> | <span data-ttu-id="1184c-125">Příkaz s sledování</span><span class="sxs-lookup"><span data-stu-id="1184c-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="1184c-126">Spustit DotNet.</span><span class="sxs-lookup"><span data-stu-id="1184c-126">dotnet run</span></span> | <span data-ttu-id="1184c-127">Spustit sledování DotNet.</span><span class="sxs-lookup"><span data-stu-id="1184c-127">dotnet watch run</span></span> |
| <span data-ttu-id="1184c-128">Spustit -f netcoreapp2.0 DotNet.</span><span class="sxs-lookup"><span data-stu-id="1184c-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="1184c-129">Spustit -f netcoreapp2.0 sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="1184c-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="1184c-130">Spustit netcoreapp2.0 -f – DotNet – arg1</span><span class="sxs-lookup"><span data-stu-id="1184c-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="1184c-131">kukátko DotNet spustit netcoreapp2.0 -f –--arg1</span><span class="sxs-lookup"><span data-stu-id="1184c-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="1184c-132">test DotNet.</span><span class="sxs-lookup"><span data-stu-id="1184c-132">dotnet test</span></span> | <span data-ttu-id="1184c-133">test sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="1184c-133">dotnet watch test</span></span> |

<span data-ttu-id="1184c-134">Spustit `dotnet watch run` v *WebApp* složky.</span><span class="sxs-lookup"><span data-stu-id="1184c-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="1184c-135">Určuje, bude výstup konzoly `watch` bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="1184c-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="1184c-136">Provedení změn pomocí`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="1184c-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="1184c-137">Zajistěte, aby `dotnet watch` běží.</span><span class="sxs-lookup"><span data-stu-id="1184c-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="1184c-138">Opravte chyby v `Product` metodu *MathController.cs* tak, aby vracel produktu a nikoli součet:</span><span class="sxs-lookup"><span data-stu-id="1184c-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="1184c-139">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="1184c-139">Save the file.</span></span> <span data-ttu-id="1184c-140">Výstup konzoly znamená, že `dotnet watch` zjištěna změna souboru a restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="1184c-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="1184c-141">Ověřte `http://localhost:<port number>/api/math/product?a=4&b=5` vrátí výsledek ve správné.</span><span class="sxs-lookup"><span data-stu-id="1184c-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="1184c-142">Spouštění testů pomocí aplikace`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="1184c-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="1184c-143">Změna `Product` metodu *MathController.cs* zpět do vrací součet a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="1184c-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="1184c-144">V příkazovém řádku přejděte do *WebAppTests* složky.</span><span class="sxs-lookup"><span data-stu-id="1184c-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="1184c-145">Run `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="1184c-145">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="1184c-146">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="1184c-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="1184c-147">Její výstup označuje, že test se nezdařil a že sledovací proces čeká na změny souboru:</span><span class="sxs-lookup"><span data-stu-id="1184c-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="1184c-148">Opravte `Product` metoda kód tak, aby vracel produktu.</span><span class="sxs-lookup"><span data-stu-id="1184c-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="1184c-149">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="1184c-149">Save the file.</span></span>

<span data-ttu-id="1184c-150">`dotnet watch`zjistí změnu souboru a znovu spustí testy.</span><span class="sxs-lookup"><span data-stu-id="1184c-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="1184c-151">Výstup konzoly znamená, testy úspěšně.</span><span class="sxs-lookup"><span data-stu-id="1184c-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="1184c-152">kukátko DotNet v Githubu</span><span class="sxs-lookup"><span data-stu-id="1184c-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="1184c-153">kukátko DotNet je součástí Githubu [DotNetTools úložiště](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="1184c-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="1184c-154">[MSBuild části](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) z [dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) vysvětluje, jak sledovat dotnet. lze konfigurovat v souboru projektu nástroje MSBuild sledovány.</span><span class="sxs-lookup"><span data-stu-id="1184c-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="1184c-155">[Dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) obsahuje informace o dotnet sledování nejsou zahrnuté v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1184c-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
