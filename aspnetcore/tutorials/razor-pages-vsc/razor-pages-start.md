---
title: Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio
author: rick-anderson
description: Přečtěte si základní informace o vytváření webové aplikace ASP.NET Core Razor stránky s kódem jazyka Visual Studio.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: b1f1dcc1401d707cff79f3269ab70b900e9fc21c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275938"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="1a89e-103">Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a89e-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="1a89e-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a89e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a89e-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="1a89e-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="1a89e-106">Doporučujeme, abyste dokončení [Úvod do stránky Razor](xref:razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1a89e-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="1a89e-107">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a89e-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a89e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1a89e-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="1a89e-109">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="1a89e-109">Create a Razor web app</span></span>

<span data-ttu-id="1a89e-110">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1a89e-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="1a89e-112">Předchozí příkazy použijte [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/dotnet) k vytvoření a spuštění projektu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="1a89e-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="1a89e-113">Otevřete prohlížeč na http://localhost:5000 zobrazíte aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a89e-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Index nebo Domovská stránka](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="1a89e-115">Otevřete projekt</span><span class="sxs-lookup"><span data-stu-id="1a89e-115">Open the project</span></span>

<span data-ttu-id="1a89e-116">Stiskněte kombinaci kláves Ctrl + C vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a89e-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="1a89e-117">Visual Studio Code (kód VS), vyberte **soubor > Otevřít složku**a pak vyberte *RazorPagesMovie* složky.</span><span class="sxs-lookup"><span data-stu-id="1a89e-117">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="1a89e-118">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="1a89e-118">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="1a89e-119">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="1a89e-119">Add them?"</span></span>
- <span data-ttu-id="1a89e-120">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="1a89e-120">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1a89e-121">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="1a89e-121">Launch the app</span></span>

<span data-ttu-id="1a89e-122">Stisknutím kombinace kláves Ctrl + F5 a spusťte aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="1a89e-122">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="1a89e-123">Můžete také z **ladění** nabídce vyberte možnost **spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="1a89e-123">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="1a89e-124">V dalším kurzu přidáme modelu do projektu.</span><span class="sxs-lookup"><span data-stu-id="1a89e-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="1a89e-125">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="1a89e-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
