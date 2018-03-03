---
title: "Začínáme s ASP.NET Core 2.0"
author: rick-anderson
description: "Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 6f6b580f700a8e2ce09901668e6b026251024619
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="92283-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92283-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="92283-104">Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="92283-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="92283-105">Hledáte začít pracovat se starší verzí?</span><span class="sxs-lookup"><span data-stu-id="92283-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="92283-106">V tématu [1.1 verzi v tomto kurzu](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="92283-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="92283-107">Nainstalujte [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="92283-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="92283-108">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="92283-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="92283-109">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="92283-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="92283-110">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="92283-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="92283-111">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="92283-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="92283-112">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92283-112">Run the app.</span></span>

    <span data-ttu-id="92283-113">Ke spuštění aplikace použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="92283-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="92283-114">Přejděte do [http://localhost: 5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="92283-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="92283-115">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="92283-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="92283-116">Je čas na serveru @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="92283-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="92283-117">Přejděte do [http://localhost: 5000/o](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="92283-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="92283-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92283-118">Next steps</span></span>

<span data-ttu-id="92283-119">Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="92283-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="92283-120">Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="92283-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="92283-121">Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="92283-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="92283-122">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="92283-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
