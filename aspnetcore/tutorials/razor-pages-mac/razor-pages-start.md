---
title: Začínáme se stránkami Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Objevte, jak začít pracovat se stránkami Razor v ASP.NET Core pomocí sady Visual Studio pro Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8a4cc6c52684558ce9176f98205e439096e88cbf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089648"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="e1a8c-103">Začínáme se stránkami Razor v ASP.NET Core v systému macOS pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="e1a8c-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="e1a8c-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1a8c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e1a8c-105">V tomto kurzu se naučíte se základy vytváření webové aplikace ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="e1a8c-106">Doporučujeme vám přečíst si téma [Úvod do stránky Razor](xref:razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-106">We recommend you review [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="e1a8c-107">Stránky Razor je doporučený způsob sestavení uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1a8c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1a8c-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e1a8c-109">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="e1a8c-109">Create a Razor web app</span></span>

<span data-ttu-id="e1a8c-110">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e1a8c-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="e1a8c-111">Předchozí příkazy použití [rozhraní příkazového řádku .NET Core](/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="e1a8c-112">Otevřete prohlížeč na http://localhost:5000 zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="e1a8c-114">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="e1a8c-114">Open the project</span></span>

<span data-ttu-id="e1a8c-115">Stisknutím kláves Ctrl + C vypnout aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="e1a8c-116">Ze sady Visual Studio, vyberte **soubor > Otevřít**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="e1a8c-117">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e1a8c-117">Launch the app</span></span>

<span data-ttu-id="e1a8c-118">V sadě Visual Studio, vyberte **spuštění > Spustit bez ladění** aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e1a8c-119">Visual Studio spustí [Kestrel](xref:fundamentals/servers/kestrel), spustí se prohlížeč a přejde na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="e1a8c-120">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="e1a8c-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e1a8c-121">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="e1a8c-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
