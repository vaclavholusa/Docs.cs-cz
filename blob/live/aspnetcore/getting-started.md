---
title: "Začínáme s ASP.NET Core 2.0"
author: rick-anderson
description: "Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core."
keywords: "ASP.NET Core, kurzu Začínáme"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: e944f0f5a3da6d1686ca8a3036666d8dadc9a0f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="8e88a-104">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e88a-104">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="8e88a-105">Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e88a-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="8e88a-106">Hledáte začít pracovat se starší verzí?</span><span class="sxs-lookup"><span data-stu-id="8e88a-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="8e88a-107">V tématu [1.1 verzi v tomto kurzu](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="8e88a-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="8e88a-108">Nainstalujte [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="8e88a-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="8e88a-109">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e88a-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="8e88a-110">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="8e88a-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="8e88a-111">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="8e88a-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="8e88a-112">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8e88a-112">Run the app.</span></span>

    <span data-ttu-id="8e88a-113">Ke spuštění aplikace použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8e88a-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="8e88a-114">Přejděte do [http://localhost: 5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="8e88a-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="8e88a-115">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="8e88a-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="8e88a-116">Je čas na serveru @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="8e88a-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="8e88a-117">Přejděte do [http://localhost: 5000/o](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="8e88a-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="8e88a-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e88a-118">Next steps</span></span>

<span data-ttu-id="8e88a-119">Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="8e88a-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="8e88a-120">Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="8e88a-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="8e88a-121">Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="8e88a-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="8e88a-122">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="8e88a-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
