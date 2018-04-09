---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="d0130-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0130-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="d0130-104">Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0130-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="d0130-105">V tématu [Začínáme s ASP.NET Core 1.1](xref:getting-started-1.1) 1.1 verze tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d0130-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="d0130-106">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="d0130-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="d0130-107">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0130-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="d0130-108">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="d0130-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="d0130-109">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="d0130-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="d0130-110">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d0130-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="d0130-111">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0130-111">Run the app.</span></span>

    <span data-ttu-id="d0130-112">Ke spuštění aplikace použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d0130-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="d0130-113">Přejděte na [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="d0130-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="d0130-114">Otevřete <em>Pages/About.cshtml</em> a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="d0130-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="d0130-115">Je čas na serveru @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="d0130-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="d0130-116">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="d0130-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="d0130-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0130-117">Next steps</span></span>

<span data-ttu-id="d0130-118">Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="d0130-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="d0130-119">Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="d0130-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="d0130-120">Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="d0130-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="d0130-121">Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d0130-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
