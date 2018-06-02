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
ms.openlocfilehash: 29890640223fe533cca82fb8d39a5ef26e8c6639
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729973"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="e54ed-103">Vývoj aplikací ASP.NET Core sledovacích procesů souboru</span><span class="sxs-lookup"><span data-stu-id="e54ed-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="e54ed-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="e54ed-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="e54ed-105">`dotnet watch` je nástroj, který běží [.NET Core rozhraní příkazového řádku](/dotnet/core/tools) příkaz Pokud zdrojové soubory změn.</span><span class="sxs-lookup"><span data-stu-id="e54ed-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="e54ed-106">Změna souboru můžete například aktivovat kompilace, spuštění testu nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="e54ed-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="e54ed-107">Tento kurz používá existující webové rozhraní API s dva koncové body: jeden, který vrátí součet a jeden, který vrací produktu.</span><span class="sxs-lookup"><span data-stu-id="e54ed-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="e54ed-108">Metoda produkt obsahuje chyby, která je stanovena v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e54ed-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="e54ed-109">Stažení [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="e54ed-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="e54ed-110">Skládá se ze dvou projektů: *WebApp* (ASP.NET Core webového rozhraní API) a *WebAppTests* (testy částí pro webové rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="e54ed-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="e54ed-111">V příkazovém řádku přejděte do *WebApp* složky.</span><span class="sxs-lookup"><span data-stu-id="e54ed-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="e54ed-112">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e54ed-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="e54ed-113">Výstup konzoly zobrazí zprávy podobné následujícím (což znamená, že je aplikace spuštěna a čeká na požadavky):</span><span class="sxs-lookup"><span data-stu-id="e54ed-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="e54ed-114">Ve webovém prohlížeči, přejděte na `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="e54ed-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="e54ed-115">Měli byste vidět výsledek `9`.</span><span class="sxs-lookup"><span data-stu-id="e54ed-115">You should see the result of `9`.</span></span>

<span data-ttu-id="e54ed-116">Přejděte do produktu rozhraní API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="e54ed-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="e54ed-117">Vrátí `9`, nikoli `20` jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="e54ed-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="e54ed-118">Tento problém vyřešen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e54ed-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="e54ed-119">Přidat `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="e54ed-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="e54ed-120">`dotnet watch` Souboru sledovacích procesů nástroj je součástí verze 2.1.300 .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="e54ed-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="e54ed-121">Následující kroky jsou požadovány při používání dřívější verzi rozhraní .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="e54ed-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="e54ed-122">Přidat `Microsoft.DotNet.Watcher.Tools` odkaz na balíček *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="e54ed-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="e54ed-123">Nainstalujte `Microsoft.DotNet.Watcher.Tools` balíček spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e54ed-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="e54ed-124">Spusťte příkazy .NET Core rozhraní příkazového řádku pomocí `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e54ed-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="e54ed-125">Všechny [.NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools#cli-commands) lze spustit s `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="e54ed-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="e54ed-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e54ed-126">For example:</span></span>

| <span data-ttu-id="e54ed-127">Příkaz</span><span class="sxs-lookup"><span data-stu-id="e54ed-127">Command</span></span> | <span data-ttu-id="e54ed-128">Příkaz s sledování</span><span class="sxs-lookup"><span data-stu-id="e54ed-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="e54ed-129">Spustit DotNet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-129">dotnet run</span></span> | <span data-ttu-id="e54ed-130">Spustit sledování DotNet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-130">dotnet watch run</span></span> |
| <span data-ttu-id="e54ed-131">Spustit -f netcoreapp2.0 DotNet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="e54ed-132">Spustit -f netcoreapp2.0 sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="e54ed-133">Spustit netcoreapp2.0 -f – DotNet – arg1</span><span class="sxs-lookup"><span data-stu-id="e54ed-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="e54ed-134">kukátko DotNet spustit netcoreapp2.0 -f –--arg1</span><span class="sxs-lookup"><span data-stu-id="e54ed-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="e54ed-135">test DotNet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-135">dotnet test</span></span> | <span data-ttu-id="e54ed-136">test sledovat DotNet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-136">dotnet watch test</span></span> |

<span data-ttu-id="e54ed-137">Spustit `dotnet watch run` v *WebApp* složky.</span><span class="sxs-lookup"><span data-stu-id="e54ed-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="e54ed-138">Určuje, bude výstup konzoly `watch` bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="e54ed-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="e54ed-139">Proveďte změny s `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e54ed-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="e54ed-140">Zajistěte, aby `dotnet watch` běží.</span><span class="sxs-lookup"><span data-stu-id="e54ed-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="e54ed-141">Opravte chyby v `Product` metodu *MathController.cs* tak, aby vracel produktu a nikoli součet:</span><span class="sxs-lookup"><span data-stu-id="e54ed-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="e54ed-142">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="e54ed-142">Save the file.</span></span> <span data-ttu-id="e54ed-143">Výstup konzoly znamená, že `dotnet watch` zjištěna změna souboru a restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="e54ed-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="e54ed-144">Ověřte `http://localhost:<port number>/api/math/product?a=4&b=5` vrátí výsledek ve správné.</span><span class="sxs-lookup"><span data-stu-id="e54ed-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="e54ed-145">Spouštění testů pomocí `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e54ed-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="e54ed-146">Změna `Product` metodu *MathController.cs* zpět na vrácení součet.</span><span class="sxs-lookup"><span data-stu-id="e54ed-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="e54ed-147">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="e54ed-147">Save the file.</span></span>
1. <span data-ttu-id="e54ed-148">V příkazovém řádku přejděte do *WebAppTests* složky.</span><span class="sxs-lookup"><span data-stu-id="e54ed-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="e54ed-149">Spustit [dotnet obnovení](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="e54ed-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="e54ed-150">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="e54ed-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="e54ed-151">Její výstup označuje, že testu se nezdařila a že sledovací proces čeká na změny souboru:</span><span class="sxs-lookup"><span data-stu-id="e54ed-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="e54ed-152">Opravte `Product` metoda kód tak, aby vracel produktu.</span><span class="sxs-lookup"><span data-stu-id="e54ed-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="e54ed-153">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="e54ed-153">Save the file.</span></span>

<span data-ttu-id="e54ed-154">`dotnet watch` zjistí změnu souboru a znovu spustí testy.</span><span class="sxs-lookup"><span data-stu-id="e54ed-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="e54ed-155">Výstup konzoly znamená, testy úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e54ed-155">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="e54ed-156">kukátko DotNet v Githubu</span><span class="sxs-lookup"><span data-stu-id="e54ed-156">dotnet-watch in GitHub</span></span>

<span data-ttu-id="e54ed-157">kukátko DotNet je součástí Githubu [DotNetTools úložiště](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="e54ed-157">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="e54ed-158">[MSBuild části](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) z [dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) vysvětluje, jak sledovat dotnet. lze konfigurovat v souboru projektu nástroje MSBuild sledovány.</span><span class="sxs-lookup"><span data-stu-id="e54ed-158">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="e54ed-159">[Dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) obsahuje informace o dotnet sledování nejsou zahrnuté v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e54ed-159">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) has information on dotnet-watch not covered in this tutorial.</span></span>
