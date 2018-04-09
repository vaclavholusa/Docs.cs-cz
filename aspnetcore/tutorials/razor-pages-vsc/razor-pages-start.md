---
title: Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio
author: rick-anderson
description: Přečtěte si základní informace o vytváření webové aplikace ASP.NET Core Razor stránky s kódem jazyka Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 11164fda108e7cc87791e777a66e58f33fe558ae
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="ec47f-103">Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec47f-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="ec47f-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec47f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ec47f-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="ec47f-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ec47f-106">Doporučujeme, abyste dokončení [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ec47f-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="ec47f-107">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ec47f-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec47f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec47f-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ec47f-109">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="ec47f-109">Create a Razor web app</span></span>

<span data-ttu-id="ec47f-110">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ec47f-110">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="ec47f-111">Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="ec47f-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="ec47f-112">Otevřete prohlížeč na http://localhost:5000 zobrazíte aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec47f-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="ec47f-114">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="ec47f-114">Open the project</span></span>

<span data-ttu-id="ec47f-115">Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec47f-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="ec47f-116">Visual Studio Code (kód VS), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.</span><span class="sxs-lookup"><span data-stu-id="ec47f-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="ec47f-117">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="ec47f-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="ec47f-118">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="ec47f-118">Add them?"</span></span>
- <span data-ttu-id="ec47f-119">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="ec47f-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="ec47f-120">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="ec47f-120">Launch the app</span></span>

<span data-ttu-id="ec47f-121">Stisknutím kombinace kláves Ctrl + F5 a spusťte aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="ec47f-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="ec47f-122">Můžete také z **ladění** nabídce vyberte možnost **spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="ec47f-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="ec47f-123">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="ec47f-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="ec47f-124">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="ec47f-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
