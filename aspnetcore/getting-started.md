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
ms.openlocfilehash: 5b8c9f770e749c13bc562f157b4ebfd25a88a4e0
ms.sourcegitcommit: 019e5a0342fd49a94056d14fc7a1a1d0f81d2a39
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="e9952-104">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9952-104">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="e9952-105">Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9952-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="e9952-106">Hledáte začít pracovat se starší verzí?</span><span class="sxs-lookup"><span data-stu-id="e9952-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="e9952-107">V tématu [1.1 verzi v tomto kurzu](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="e9952-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="e9952-108">Nainstalujte [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="e9952-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="e9952-109">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9952-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="e9952-110">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e9952-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e9952-111">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="e9952-111">On Windows, open a command prompt.</span></span> <span data-ttu-id="e9952-112">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e9952-112">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="e9952-113">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e9952-113">Run the app.</span></span>

    <span data-ttu-id="e9952-114">Ke spuštění aplikace použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e9952-114">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="e9952-115">Přejděte do [http://localhost: 5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="e9952-115">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="e9952-116">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="e9952-116">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="e9952-117">Je čas na serveru @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="e9952-117">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="e9952-118">Přejděte do [http://localhost: 5000/o](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="e9952-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="e9952-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9952-119">Next steps</span></span>

<span data-ttu-id="e9952-120">Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="e9952-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="e9952-121">Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="e9952-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="e9952-122">Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="e9952-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="e9952-123">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e9952-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
