---
title: "Začínáme s stránky Razor v ASP.NET Core s kódem jazyka Visual Studio"
author: rick-anderson
description: "Začínáme s stránky Razor v ASP.NET Core pomocí Visual Studio Code"
keywords: "Jádro ASP.NET Razor stránky generování uživatelského rozhraní, Entity Framework Core, EF, EF Core, databáze, mac, systému macOS, Visual Studio Code, kód"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="bc8a3-104">Začínáme s stránky Razor v ASP.NET Core s kódem jazyka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc8a3-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="bc8a3-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc8a3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bc8a3-106">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="bc8a3-107">Doporučujeme, abyste dokončení [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="bc8a3-108">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc8a3-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bc8a3-109">Prerequisites</span></span>

<span data-ttu-id="bc8a3-110">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="bc8a3-110">Install the following:</span></span>

* <span data-ttu-id="bc8a3-111">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější</span><span class="sxs-lookup"><span data-stu-id="bc8a3-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="bc8a3-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bc8a3-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="bc8a3-113">VS Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="bc8a3-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="bc8a3-114">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="bc8a3-114">Create a Razor web app</span></span>

<span data-ttu-id="bc8a3-115">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bc8a3-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="bc8a3-116">Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="bc8a3-117">Otevřete v prohlížeči na http://localhost: 5000 zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="bc8a3-119">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="bc8a3-119">Open the project</span></span>

<span data-ttu-id="bc8a3-120">Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="bc8a3-121">Visual Studio Code (kód VS), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="bc8a3-122">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="bc8a3-123">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="bc8a3-123">Add them?"</span></span>
- <span data-ttu-id="bc8a3-124">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="bc8a3-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="bc8a3-125">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="bc8a3-125">Launch the app</span></span>

<span data-ttu-id="bc8a3-126">Stisknutím kombinace kláves Ctrl + F5 a spusťte aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="bc8a3-127">Můžete také z **ladění** nabídce vyberte možnost **spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="bc8a3-128">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="bc8a3-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="bc8a3-129">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="bc8a3-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
