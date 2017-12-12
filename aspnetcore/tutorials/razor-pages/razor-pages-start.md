---
title: "Začínáme s stránky Razor v ASP.NET Core"
author: rick-anderson
description: "Začínáme s stránky Razor v ASP.NET Core"
keywords: "ASP.NET Core, stránky Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 5c58b5156f62572687755c9c0878db10c3c14eb1
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="bbba9-104">Začínáme s stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bbba9-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="bbba9-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bbba9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bbba9-106">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="bbba9-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="bbba9-107">Doporučujeme, abyste dokončení [Úvod do stránky Razor](xref:mvc/razor-pages/index) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bbba9-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="bbba9-108">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbba9-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

<span data-ttu-id="bbba9-109">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="bbba9-109">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="bbba9-110">Windows: V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="bbba9-110">Windows: This tutorial</span></span>
* <span data-ttu-id="bbba9-111">Systému MacOS: [Začínáme s stránky Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="bbba9-111">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="bbba9-112">systému macOS, Linux a Windows: [Začínáme s stránky Razor v ASP.NET Core s kódem jazyka Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="bbba9-112">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbba9-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bbba9-113">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="bbba9-114">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="bbba9-114">Create a Razor web app</span></span>

* <span data-ttu-id="bbba9-115">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="bbba9-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bbba9-116">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbba9-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="bbba9-117">Název projektu **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="bbba9-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="bbba9-118">Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.</span><span class="sxs-lookup"><span data-stu-id="bbba9-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="bbba9-119">![nové webové aplikace ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="bbba9-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="bbba9-120">Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bbba9-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
  <span data-ttu-id="bbba9-121">![Webové aplikace (stránky Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="bbba9-121">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="bbba9-122">Šablony sady Visual Studio vytvoří projekt starter:</span><span class="sxs-lookup"><span data-stu-id="bbba9-122">The Visual Studio template creates a starter project:</span></span>

![Průzkumník řešení](razor-pages-start/_static/se.png)

<span data-ttu-id="bbba9-124">Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu</span><span class="sxs-lookup"><span data-stu-id="bbba9-124">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* <span data-ttu-id="bbba9-126">Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbba9-126">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="bbba9-127">Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="bbba9-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bbba9-128">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="bbba9-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="bbba9-129">Localhost slouží pouze webových požadavků z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="bbba9-129">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="bbba9-130">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="bbba9-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="bbba9-131">Na předchozím obrázku číslo portu je 5 000.</span><span class="sxs-lookup"><span data-stu-id="bbba9-131">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="bbba9-132">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="bbba9-132">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="bbba9-133">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="bbba9-133">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="bbba9-134">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="bbba9-134">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="bbba9-135">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="bbba9-135">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
