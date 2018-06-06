---
title: Začínáme s ASP.NET MVC jádra a sady Visual Studio
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET MVC jádra a sady Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 3272700c7739778a6a341ae8ee424fd69605ca53
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729714"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="a2d2f-103">Začínáme s ASP.NET MVC jádra a sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2d2f-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="a2d2f-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="a2d2f-105">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a2d2f-106">systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="a2d2f-107">Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="a2d2f-108">systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="a2d2f-109">Instalace sady Visual Studio a .NET Core</span><span class="sxs-lookup"><span data-stu-id="a2d2f-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a2d2f-110">[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="a2d2f-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="a2d2f-111">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="a2d2f-111">Create a web app</span></span>

<span data-ttu-id="a2d2f-112">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="a2d2f-114">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="a2d2f-115">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="a2d2f-116">V prostředním podokně, klepněte na **webové aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="a2d2f-117">Název projektu "MvcMovie" (je třeba název projektu "MvcMovie", při kopírování kód se bude shodovat s oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="a2d2f-118">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-118">Tap **OK**</span></span>

![<span data-ttu-id="a2d2f-119">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2d2f-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="a2d2f-120">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a2d2f-121">V poli verze selektor rozevíracího seznamu vyberte **2.1 jádro ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="a2d2f-122">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="a2d2f-123">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-123">Tap **OK**.</span></span>

![<span data-ttu-id="a2d2f-124">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2d2f-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="a2d2f-125">Visual Studio použít výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="a2d2f-126">Zadáním název projektu a výběrem možnosti několik Teď máte aplikaci práci.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a2d2f-127">Toto je základní starter projektu a je dobrým místem, kde začít,</span><span class="sxs-lookup"><span data-stu-id="a2d2f-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="a2d2f-128">Klepněte na **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* <span data-ttu-id="a2d2f-130">Visual Studio spustí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="a2d2f-131">Všimněte si, že se zobrazí na panelu Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a2d2f-132">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a2d2f-133">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a2d2f-134">Na obrázku výše je číslo portu 5 000.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="a2d2f-135">Adresu URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="a2d2f-136">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a2d2f-137">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a2d2f-138">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a2d2f-139">Můžete spustit aplikaci v ladění nebo režim bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a2d2f-141">Aplikace můžete ladit, klepnutím **IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="a2d2f-141">You can debug the app by tapping the **IIS Express** button</span></span>

![Služby IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="a2d2f-143">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a2d2f-144">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a2d2f-145">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](start-mvc/_static/2.png)

<span data-ttu-id="a2d2f-147">Pokud byla spuštěna v režimu ladění, klepněte na **Shift + F5** Zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="a2d2f-148">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a2d2f-149">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="a2d2f-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a2d2f-150">[! Zahrnout [] (~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="a2d2f-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a2d2f-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a2d2f-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a2d2f-152">Nainstalujte Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="a2d2f-153">Vyberte stahování komunity.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-153">Select the Community download.</span></span> <span data-ttu-id="a2d2f-154">Tento krok přeskočte, pokud máte Visual Studio 2017 nainstalována.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="a2d2f-155">Instalační program Visual Studio 2017 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="a2d2f-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="a2d2f-156">Spusťte instalační program a vyberte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="a2d2f-157">**Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="a2d2f-158">**Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET a webové vývoj ** (v části ** Web a Cloud **)](start-mvc/_static/web_workload.png)

![* *.NET základní cross-cross-platfrom vývoj ** (v části ** ostatní modulové **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="a2d2f-161">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="a2d2f-161">Create a web app</span></span>

<span data-ttu-id="a2d2f-162">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="a2d2f-164">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="a2d2f-165">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="a2d2f-166">V prostředním podokně, klepněte na **webové aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="a2d2f-167">Název projektu "MvcMovie" (je třeba název projektu "MvcMovie", při kopírování kód se bude shodovat s oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="a2d2f-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="a2d2f-168">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-168">Tap **OK**</span></span>

![<span data-ttu-id="a2d2f-169">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2d2f-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a2d2f-170">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="a2d2f-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a2d2f-171">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a2d2f-172">V poli verze selektor rozevíracího seznamu vyberte **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="a2d2f-173">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="a2d2f-174">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-174">Tap **OK**.</span></span>

![<span data-ttu-id="a2d2f-175">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2d2f-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a2d2f-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a2d2f-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a2d2f-177">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a2d2f-178">V klepnutí rozevíracího seznamu pro výběr verze **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="a2d2f-179">Klepněte na **webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-179">Tap **Web Application**</span></span>
* <span data-ttu-id="a2d2f-180">Ponechte výchozí **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="a2d2f-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="a2d2f-181">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-181">Tap **OK**.</span></span>

![Nové webové aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="a2d2f-183">Visual Studio použít výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="a2d2f-184">Zadáním název projektu a výběrem možnosti několik Teď máte aplikaci práci.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a2d2f-185">Toto je základní starter projektu a je dobrým místem, kde začít,</span><span class="sxs-lookup"><span data-stu-id="a2d2f-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="a2d2f-186">Klepněte na **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)

* <span data-ttu-id="a2d2f-188">Visual Studio spustí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="a2d2f-189">Všimněte si, že se zobrazí na panelu Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a2d2f-190">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a2d2f-191">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a2d2f-192">Na obrázku výše je číslo portu 5 000.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="a2d2f-193">Adresu URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="a2d2f-194">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a2d2f-195">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a2d2f-196">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a2d2f-197">Můžete spustit aplikaci v ladění nebo režim bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="a2d2f-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a2d2f-199">Aplikace můžete ladit, klepnutím **IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="a2d2f-199">You can debug the app by tapping the **IIS Express** button</span></span>

![Služby IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="a2d2f-201">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a2d2f-202">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a2d2f-203">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](start-mvc/_static/2.png)

<span data-ttu-id="a2d2f-205">Pokud byla spuštěna v režimu ladění, klepněte na **Shift + F5** Zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="a2d2f-206">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="a2d2f-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="a2d2f-207">Next</span><span class="sxs-lookup"><span data-stu-id="a2d2f-207">Next</span></span>](adding-controller.md)  
