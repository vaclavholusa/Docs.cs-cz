---
title: Začínáme s stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučuje se stránky Razor pro webové úlohy v ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: cf9877a4fd3ef0e24348aa68a0a2bd401203c2e5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="812c8-104">Začínáme s stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="812c8-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="812c8-105">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="812c8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="812c8-106">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="812c8-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="812c8-107">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="812c8-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="812c8-108">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="812c8-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="812c8-109">Windows: V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="812c8-109">Windows: This tutorial</span></span>
* <span data-ttu-id="812c8-110">Systému MacOS: [začít pracovat s stránky Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="812c8-110">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="812c8-111">systému macOS, Linux a Windows: [Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="812c8-111">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="812c8-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="812c8-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="812c8-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="812c8-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="812c8-114">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="812c8-114">Create a Razor web app</span></span>

* <span data-ttu-id="812c8-115">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="812c8-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="812c8-116">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="812c8-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="812c8-117">Název projektu **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="812c8-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="812c8-118">Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.</span><span class="sxs-lookup"><span data-stu-id="812c8-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="812c8-119">![nové webové aplikace ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="812c8-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="812c8-120">Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="812c8-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="812c8-121">Šablony sady Visual Studio vytvoří projekt starter:</span><span class="sxs-lookup"><span data-stu-id="812c8-121">The Visual Studio template creates a starter project:</span></span>

![Průzkumník řešení](razor-pages-start/_static/se.png)

<span data-ttu-id="812c8-123">Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu</span><span class="sxs-lookup"><span data-stu-id="812c8-123">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* <span data-ttu-id="812c8-125">Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="812c8-125">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="812c8-126">Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="812c8-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="812c8-127">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="812c8-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="812c8-128">Localhost slouží pouze webových požadavků z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="812c8-128">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="812c8-129">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="812c8-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="812c8-130">Na předchozím obrázku číslo portu je 5 000.</span><span class="sxs-lookup"><span data-stu-id="812c8-130">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="812c8-131">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="812c8-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="812c8-132">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="812c8-132">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="812c8-133">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="812c8-133">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [<span data-ttu-id="812c8-134">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="812c8-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
