---
title: "Začínáme s ASP.NET Core 1.1"
author: rick-anderson
description: "Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core 1.1."
keywords: "ASP.NET Core, kurzu Začínáme"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="6e310-104">Začínáme s ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="6e310-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="6e310-105">Tyto pokyny jsou určené pro ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="6e310-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="6e310-106">Hledáte nejnovější verzi?</span><span class="sxs-lookup"><span data-stu-id="6e310-106">Looking for the latest version?</span></span> <span data-ttu-id="6e310-107">V tématu [aktuální verzi v tomto kurzu](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="6e310-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="6e310-108">Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 stáhne stránky](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="6e310-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="6e310-109">Vytvořte složku pro nový projekt .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e310-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="6e310-110">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="6e310-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="6e310-111">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="6e310-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="6e310-112">Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="6e310-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="6e310-113">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e310-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="6e310-114">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="6e310-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="6e310-115">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e310-115">Run the app.</span></span>

   <span data-ttu-id="6e310-116">`dotnet run` Příkaz sestavení aplikace nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6e310-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="6e310-117">Přejděte na`http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="6e310-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="6e310-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e310-118">Next steps</span></span>

<span data-ttu-id="6e310-119">Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="6e310-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="6e310-120">Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="6e310-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="6e310-121">Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="6e310-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="6e310-122">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="6e310-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
