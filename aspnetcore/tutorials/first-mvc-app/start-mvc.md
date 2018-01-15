---
title: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
keywords: "Jádro ASP.NET, MVC"
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="04f4d-104">Začínáme s ASP.NET MVC jádra a sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04f4d-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="04f4d-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="04f4d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="04f4d-106">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="04f4d-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="04f4d-107">systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="04f4d-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="04f4d-108">Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="04f4d-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="04f4d-109">systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="04f4d-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="04f4d-110">Instalace sady Visual Studio a .NET Core</span><span class="sxs-lookup"><span data-stu-id="04f4d-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="04f4d-111">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="04f4d-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="04f4d-112">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="04f4d-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="04f4d-113">Nainstalujte Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="04f4d-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="04f4d-114">Vyberte stahování komunity.</span><span class="sxs-lookup"><span data-stu-id="04f4d-114">Select the Community download.</span></span> <span data-ttu-id="04f4d-115">Tento krok přeskočte, pokud máte Visual Studio 2017 nainstalována.</span><span class="sxs-lookup"><span data-stu-id="04f4d-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="04f4d-116">Instalační program Visual Studio 2017 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="04f4d-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="04f4d-117">Spusťte instalační program a vyberte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="04f4d-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="04f4d-118">**Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="04f4d-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="04f4d-119">**Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)</span><span class="sxs-lookup"><span data-stu-id="04f4d-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET a webové vývoj ** (v části ** Web a Cloud **)](start-mvc/_static/web_workload.png)

![* *.NET základní cross-cross-platfrom vývoj ** (v části ** ostatní modulové **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="04f4d-122">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="04f4d-122">Create a web app</span></span>

<span data-ttu-id="04f4d-123">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="04f4d-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="04f4d-125">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="04f4d-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="04f4d-126">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="04f4d-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="04f4d-127">V prostředním podokně, klepněte na **webové aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="04f4d-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="04f4d-128">Název projektu "MvcMovie" (je třeba název projektu "MvcMovie", při kopírování kód se bude shodovat s oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="04f4d-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="04f4d-129">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="04f4d-129">Tap **OK**</span></span>

![<span data-ttu-id="04f4d-130">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04f4d-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="04f4d-131">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="04f4d-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="04f4d-132">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="04f4d-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="04f4d-133">V poli verze selektor rozevíracího seznamu vyberte **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="04f4d-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="04f4d-134">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="04f4d-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="04f4d-135">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="04f4d-135">Tap **OK**.</span></span>

![<span data-ttu-id="04f4d-136">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04f4d-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="04f4d-137">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="04f4d-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="04f4d-138">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="04f4d-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="04f4d-139">V klepnutí rozevíracího seznamu pro výběr verze **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="04f4d-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="04f4d-140">Klepněte na **webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="04f4d-140">Tap **Web Application**</span></span>
* <span data-ttu-id="04f4d-141">Ponechte výchozí **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="04f4d-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="04f4d-142">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="04f4d-142">Tap **OK**.</span></span>

![Nové webové aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="04f4d-144">Visual Studio použít výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="04f4d-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="04f4d-145">Zadáním název projektu a výběrem možnosti několik Teď máte aplikaci práci.</span><span class="sxs-lookup"><span data-stu-id="04f4d-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="04f4d-146">Toto je jednoduchá starter projektu a je dobrým místem, kde začít,</span><span class="sxs-lookup"><span data-stu-id="04f4d-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="04f4d-147">Klepněte na **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="04f4d-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* <span data-ttu-id="04f4d-149">Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="04f4d-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="04f4d-150">Všimněte si, že se zobrazí na panelu Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="04f4d-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="04f4d-151">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="04f4d-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="04f4d-152">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="04f4d-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="04f4d-153">Na obrázku výše je číslo portu 5 000.</span><span class="sxs-lookup"><span data-stu-id="04f4d-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="04f4d-154">Adresu URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="04f4d-154">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="04f4d-155">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="04f4d-155">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="04f4d-156">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="04f4d-156">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="04f4d-157">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="04f4d-157">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="04f4d-158">Můžete spustit aplikaci v ladění nebo režim bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="04f4d-158">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="04f4d-160">Aplikace můžete ladit, klepnutím **IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="04f4d-160">You can debug the app by tapping the **IIS Express** button</span></span>

![Služby IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="04f4d-162">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="04f4d-162">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="04f4d-163">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="04f4d-163">The browser image above doesn't show these links.</span></span> <span data-ttu-id="04f4d-164">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="04f4d-164">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](start-mvc/_static/2.png)

<span data-ttu-id="04f4d-166">Pokud byla spuštěna v režimu ladění, klepněte na **Shift + F5** Zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="04f4d-166">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="04f4d-167">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="04f4d-167">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="04f4d-168">Next</span><span class="sxs-lookup"><span data-stu-id="04f4d-168">Next</span></span>](adding-controller.md)  
