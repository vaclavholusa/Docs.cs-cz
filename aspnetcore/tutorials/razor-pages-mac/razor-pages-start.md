---
title: "Začínáme s stránky Razor v ASP.NET Core v systému Mac"
author: rick-anderson
description: "Začínáme s stránky Razor v ASP.NET Core pomocí sady Visual Studio pro Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: d4d8c7c543273aa38d1599b51d00a348df7182de
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/26/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="cb7d3-103">Začínáme s stránky Razor v ASP.NET Core pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="cb7d3-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="cb7d3-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cb7d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cb7d3-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="cb7d3-106">Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="cb7d3-107">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb7d3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cb7d3-108">Prerequisites</span></span>

<span data-ttu-id="cb7d3-109">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="cb7d3-109">Install the following:</span></span>

* <span data-ttu-id="cb7d3-110">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější</span><span class="sxs-lookup"><span data-stu-id="cb7d3-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="cb7d3-111">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="cb7d3-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="cb7d3-112">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="cb7d3-112">Create a Razor web app</span></span>

<span data-ttu-id="cb7d3-113">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="cb7d3-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="cb7d3-114">Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="cb7d3-115">Otevřete v prohlížeči na http://localhost: 5000 zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="cb7d3-117">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="cb7d3-117">Open the project</span></span>

<span data-ttu-id="cb7d3-118">Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="cb7d3-119">Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="cb7d3-120">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="cb7d3-120">Launch the app</span></span>

<span data-ttu-id="cb7d3-121">V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="cb7d3-122">Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), spustí se prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="cb7d3-123">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="cb7d3-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="cb7d3-124">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="cb7d3-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
