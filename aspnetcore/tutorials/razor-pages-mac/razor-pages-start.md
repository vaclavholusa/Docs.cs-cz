---
title: "Začínáme s stránky Razor v ASP.NET Core v systému Mac"
author: rick-anderson
description: "Zjistit, jak začít pracovat s stránky Razor v ASP.NET Core pomocí sady Visual Studio for Mac."
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: e56799d321cc84c60188a72def9448a0b339d568
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="c7b8f-103">Začínáme s stránky Razor v ASP.NET Core pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="c7b8f-103">Get started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="c7b8f-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7b8f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7b8f-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="c7b8f-106">Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="c7b8f-107">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7b8f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c7b8f-108">Prerequisites</span></span>

<span data-ttu-id="c7b8f-109">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="c7b8f-109">Install the following:</span></span>

* <span data-ttu-id="c7b8f-110">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější</span><span class="sxs-lookup"><span data-stu-id="c7b8f-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="c7b8f-111">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c7b8f-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c7b8f-112">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="c7b8f-112">Create a Razor web app</span></span>

<span data-ttu-id="c7b8f-113">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="c7b8f-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="c7b8f-114">Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="c7b8f-115">Otevřete v prohlížeči na http://localhost: 5000 zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="c7b8f-117">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="c7b8f-117">Open the project</span></span>

<span data-ttu-id="c7b8f-118">Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="c7b8f-119">Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="c7b8f-120">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="c7b8f-120">Launch the app</span></span>

<span data-ttu-id="c7b8f-121">V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="c7b8f-122">Visual Studio spustí [Kestrel](xref:fundamentals/servers/kestrel), spustí se prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-122">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="c7b8f-123">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="c7b8f-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="c7b8f-124">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="c7b8f-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
