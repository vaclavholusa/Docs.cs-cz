---
title: Začínáme s stránky Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Zjistit, jak začít pracovat s stránky Razor v ASP.NET Core pomocí sady Visual Studio for Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 2da9b34f797758c02132d5cf6cc2f2fb2fe6f05a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="f3f12-103">Začínáme s stránky Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="f3f12-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="f3f12-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f3f12-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f3f12-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="f3f12-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="f3f12-106">Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f3f12-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="f3f12-107">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3f12-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3f12-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3f12-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f3f12-109">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="f3f12-109">Create a Razor web app</span></span>

<span data-ttu-id="f3f12-110">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f3f12-110">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="f3f12-111">Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="f3f12-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="f3f12-112">Otevřete prohlížeč na http://localhost:5000 zobrazíte aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3f12-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="f3f12-114">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="f3f12-114">Open the project</span></span>

<span data-ttu-id="f3f12-115">Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3f12-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="f3f12-116">Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f3f12-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="f3f12-117">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="f3f12-117">Launch the app</span></span>

<span data-ttu-id="f3f12-118">V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f3f12-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="f3f12-119">Visual Studio spustí [Kestrel](xref:fundamentals/servers/kestrel), spustí se prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f3f12-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="f3f12-120">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="f3f12-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f3f12-121">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="f3f12-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
