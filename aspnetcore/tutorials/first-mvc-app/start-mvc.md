---
title: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
keywords: "Jádro ASP.NET, MVC"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 6e233a0114967405a67b05365e0125620441efb4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="e7879-104">Začínáme s ASP.NET MVC jádra a sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7879-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="e7879-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e7879-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e7879-106">V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e7879-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="e7879-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="e7879-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e7879-108">systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e7879-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e7879-109">Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e7879-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e7879-110">systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e7879-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="e7879-111">Verze sady Visual Studio 2015 v tomto kurzu, najdete v článku [VS 2015 verzi ASP.NET Core dokumentaci ve formátu PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="e7879-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="e7879-112">Instalace sady Visual Studio a .NET Core</span><span class="sxs-lookup"><span data-stu-id="e7879-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7879-113">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="e7879-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7879-114">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="e7879-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e7879-115">Nainstalujte Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="e7879-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="e7879-116">Vyberte stahování komunity.</span><span class="sxs-lookup"><span data-stu-id="e7879-116">Select the Community download.</span></span> <span data-ttu-id="e7879-117">Tento krok přeskočte, pokud máte Visual Studio 2017 nainstalována.</span><span class="sxs-lookup"><span data-stu-id="e7879-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="e7879-118">Instalační program Visual Studio 2017 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="e7879-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="e7879-119">Spusťte instalační program a vyberte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="e7879-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="e7879-120">**Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="e7879-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="e7879-121">**Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)</span><span class="sxs-lookup"><span data-stu-id="e7879-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET a webové vývoj ** (v části ** Web a Cloud **)](start-mvc/_static/web_workload.png)

![* *.NET základní cross-cross-platfrom vývoj ** (v části ** ostatní modulové **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="e7879-124">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e7879-124">Create a web app</span></span>

<span data-ttu-id="e7879-125">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="e7879-125">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e7879-127">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e7879-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e7879-128">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="e7879-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e7879-129">V prostředním podokně, klepněte na **webové aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="e7879-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e7879-130">Název projektu "MvcMovie" (je třeba název projektu "MvcMovie", při kopírování kód se bude shodovat s oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="e7879-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e7879-131">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e7879-131">Tap **OK**</span></span>

![<span data-ttu-id="e7879-132">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7879-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7879-133">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="e7879-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e7879-134">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e7879-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e7879-135">V poli verze selektor rozevíracího seznamu vyberte **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="e7879-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="e7879-136">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="e7879-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="e7879-137">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e7879-137">Tap **OK**.</span></span>

![<span data-ttu-id="e7879-138">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7879-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7879-139">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="e7879-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e7879-140">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e7879-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e7879-141">V klepnutí rozevíracího seznamu pro výběr verze **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="e7879-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="e7879-142">Klepněte na **webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="e7879-142">Tap **Web Application**</span></span>
* <span data-ttu-id="e7879-143">Ponechte výchozí **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="e7879-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="e7879-144">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e7879-144">Tap **OK**.</span></span>

![Nové webové aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="e7879-146">Visual Studio použít výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e7879-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e7879-147">Zadáním název projektu a výběrem možnosti několik Teď máte aplikaci práci.</span><span class="sxs-lookup"><span data-stu-id="e7879-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e7879-148">Toto je jednoduchá starter projektu a je dobrým místem, kde začít,</span><span class="sxs-lookup"><span data-stu-id="e7879-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="e7879-149">Klepněte na **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="e7879-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* <span data-ttu-id="e7879-151">Visual Studio spustí [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7879-151">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e7879-152">Všimněte si, že se zobrazí na panelu Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e7879-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e7879-153">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="e7879-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e7879-154">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="e7879-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e7879-155">Na obrázku výše je číslo portu 5 000.</span><span class="sxs-lookup"><span data-stu-id="e7879-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e7879-156">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="e7879-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e7879-157">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="e7879-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e7879-158">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="e7879-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e7879-159">Můžete spustit aplikaci v ladění nebo režim bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="e7879-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e7879-161">Aplikace můžete ladit, klepnutím **IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="e7879-161">You can debug the app by tapping the **IIS Express** button</span></span>

![Služby IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e7879-163">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="e7879-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e7879-164">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="e7879-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e7879-165">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="e7879-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](start-mvc/_static/2.png)

<span data-ttu-id="e7879-167">Pokud byla spuštěna v režimu ladění, klepněte na **Shift + F5** Zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="e7879-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e7879-168">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="e7879-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e7879-169">Další</span><span class="sxs-lookup"><span data-stu-id="e7879-169">Next</span></span>](adding-controller.md)  
