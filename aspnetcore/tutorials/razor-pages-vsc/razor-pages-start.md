---
title: Začínáme s ASP.NET Core Razor Pages ve Visual Studio Code
author: rick-anderson
description: Naučte se základy vytváření webovou aplikaci ASP.NET Core Razor Pages s Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244850"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="98926-103">Začínáme s ASP.NET Core Razor Pages ve Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98926-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="98926-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="98926-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="98926-105">V tomto kurzu se naučíte se základy vytváření webové aplikace ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="98926-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="98926-106">Doporučujeme je provést [Úvod do stránky Razor](xref:razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="98926-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="98926-107">Stránky Razor je doporučený způsob sestavení uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98926-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98926-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="98926-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="98926-109">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="98926-109">Create a Razor web app</span></span>

<span data-ttu-id="98926-110">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="98926-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="98926-111">Předchozí příkazy použití [rozhraní příkazového řádku .NET Core](/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="98926-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="98926-112">Otevřete prohlížeč na http://localhost:5000 zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="98926-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="98926-114">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="98926-114">Open the project</span></span>

<span data-ttu-id="98926-115">Stisknutím kláves Ctrl + C vypnout aplikaci.</span><span class="sxs-lookup"><span data-stu-id="98926-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="98926-116">Visual Studio Code (VS Code), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.</span><span class="sxs-lookup"><span data-stu-id="98926-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="98926-117">Vyberte **Ano** k **upozornit** zpráva "požadované prostředky pro vytvoření a odladění chybí"RazorPagesMovie".</span><span class="sxs-lookup"><span data-stu-id="98926-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="98926-118">Přidáním?"</span><span class="sxs-lookup"><span data-stu-id="98926-118">Add them?"</span></span>
- <span data-ttu-id="98926-119">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="98926-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="98926-120">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="98926-120">Launch the app</span></span>

<span data-ttu-id="98926-121">Z **ladění** nabídce vyberte možnost **spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="98926-121">From the **Debug** menu, select **Start Without Debugging**.</span></span> <span data-ttu-id="98926-122">Alternativně můžete stiskněte klávesovou zkratku, zobrazí vedle možnosti nabídky.</span><span class="sxs-lookup"><span data-stu-id="98926-122">Alternatively, you can press the keyboard shortcut displayed next to the menu option.</span></span> <span data-ttu-id="98926-123">Tento zástupce se liší podle operačního systému.</span><span class="sxs-lookup"><span data-stu-id="98926-123">This shortcut varies depending on your operating system.</span></span>

<span data-ttu-id="98926-124">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="98926-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="98926-125">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="98926-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
