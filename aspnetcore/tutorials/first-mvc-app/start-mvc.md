---
title: Začínáme s ASP.NET Core MVC a sady Visual Studio
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET Core MVC a sady Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: fe555e4cfcaec5d4bb8ccee00b06d1bbcaae9dcd
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391203"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="56692-103">Začínáme s ASP.NET Core MVC a sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56692-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="56692-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56692-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="56692-105">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="56692-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="56692-106">macOS: [vytvoření aplikace ASP.NET Core MVC se sadou Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="56692-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="56692-107">Windows: [vytvořit v aplikaci MVC ASP.NET Core pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="56692-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="56692-108">macOS, Linux a Windows: [vytvoření aplikace ASP.NET Core MVC pomocí Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="56692-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="56692-109">Instalace sady Visual Studio a .NET Core</span><span class="sxs-lookup"><span data-stu-id="56692-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="56692-110">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="56692-110">Create a web app</span></span>

<span data-ttu-id="56692-111">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="56692-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="56692-113">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="56692-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="56692-114">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="56692-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="56692-115">V prostředním podokně, klepněte na **webová aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="56692-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="56692-116">Pojmenujte projekt "MvcMovie" (je třeba název projektu "MvcMovie", takže při kopírování kódu bude odpovídat oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="56692-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="56692-117">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="56692-117">Tap **OK**</span></span>

![<span data-ttu-id="56692-118">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56692-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="56692-119">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="56692-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="56692-120">V poli verze selektoru rozevíracího seznamu vyberte **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="56692-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="56692-121">Vyberte **webová aplikace (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="56692-121">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="56692-122">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="56692-122">Tap **OK**.</span></span>

![<span data-ttu-id="56692-123">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56692-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="56692-124">Visual Studio používá výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="56692-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="56692-125">Zadáním názvu projektu a výběr několika možností Teď máte funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56692-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="56692-126">Toto je základní počáteční projekt a je dobrým začátkem.</span><span class="sxs-lookup"><span data-stu-id="56692-126">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="56692-127">Klepněte na **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="56692-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="56692-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="56692-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="56692-129">Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="56692-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="56692-130">Všimněte si, že do adresního řádku ukazuje `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="56692-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="56692-131">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="56692-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="56692-132">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="56692-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="56692-133">Na obrázku výše je číslo portu 5000.</span><span class="sxs-lookup"><span data-stu-id="56692-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="56692-134">Adresa URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="56692-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="56692-135">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="56692-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="56692-136">Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu.</span><span class="sxs-lookup"><span data-stu-id="56692-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="56692-137">Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.</span><span class="sxs-lookup"><span data-stu-id="56692-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="56692-138">Spustíte ji v ladění nebo v režimu bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="56692-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="56692-140">Můžete ladit aplikaci klepnutím **služby IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="56692-140">You can debug the app by tapping the **IIS Express** button</span></span>

![Služba IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="56692-142">Výchozí šablony vám práci **domácí o** a **kontakt** odkazy.</span><span class="sxs-lookup"><span data-stu-id="56692-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="56692-143">Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů.</span><span class="sxs-lookup"><span data-stu-id="56692-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="56692-144">V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="56692-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona navigace v vpravo nahoře](start-mvc/_static/2.png)

<span data-ttu-id="56692-146">Pokud jsou spuštěné v režimu ladění, klepněte na **Shift + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="56692-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="56692-147">V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.</span><span class="sxs-lookup"><span data-stu-id="56692-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56692-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="56692-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56692-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56692-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="56692-150">Instalace sady Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="56692-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="56692-151">Vyberte stáhnout Community.</span><span class="sxs-lookup"><span data-stu-id="56692-151">Select the Community download.</span></span> <span data-ttu-id="56692-152">Tento krok přeskočte, pokud máte nainstalovanou sadu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="56692-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="56692-153">Instalační program sady Visual Studio 2017 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="56692-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="56692-154">Spusťte instalační program a vybrat následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="56692-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="56692-155">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="56692-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="56692-156">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="56692-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![\*\*ASP.NET a webového vývoje \*\* (v části \*\* Web a Cloud \*\*)](start-mvc/_static/web_workload.png)

![\* \*.NET core napříč. mezi platfrom vývoj \*\* (v části \*\* Další sady nástrojů \*\*)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="56692-159">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="56692-159">Create a web app</span></span>

<span data-ttu-id="56692-160">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="56692-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="56692-162">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="56692-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="56692-163">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="56692-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="56692-164">V prostředním podokně, klepněte na **webová aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="56692-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="56692-165">Pojmenujte projekt "MvcMovie" (je třeba název projektu "MvcMovie", takže při kopírování kódu bude odpovídat oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="56692-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="56692-166">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="56692-166">Tap **OK**</span></span>

![<span data-ttu-id="56692-167">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56692-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56692-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="56692-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="56692-169">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="56692-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="56692-170">V poli verze selektoru rozevíracího seznamu vyberte **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="56692-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="56692-171">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="56692-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="56692-172">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="56692-172">Tap **OK**.</span></span>

![<span data-ttu-id="56692-173">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56692-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56692-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56692-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="56692-175">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="56692-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="56692-176">V tap verze selektoru rozevíracího seznamu **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="56692-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="56692-177">Klepněte na **webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="56692-177">Tap **Web Application**</span></span>
* <span data-ttu-id="56692-178">Ponechte výchozí **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="56692-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="56692-179">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="56692-179">Tap **OK**.</span></span>

![Nová webová aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="56692-181">Visual Studio používá výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="56692-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="56692-182">Zadáním názvu projektu a výběr několika možností Teď máte funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56692-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="56692-183">Toto je základní počáteční projekt, a je vhodné oddělení na zahájení,</span><span class="sxs-lookup"><span data-stu-id="56692-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="56692-184">Klepněte na **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="56692-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="56692-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="56692-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="56692-186">Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="56692-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="56692-187">Všimněte si, že do adresního řádku ukazuje `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="56692-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="56692-188">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="56692-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="56692-189">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="56692-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="56692-190">Na obrázku výše je číslo portu 5000.</span><span class="sxs-lookup"><span data-stu-id="56692-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="56692-191">Adresa URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="56692-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="56692-192">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="56692-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="56692-193">Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu.</span><span class="sxs-lookup"><span data-stu-id="56692-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="56692-194">Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.</span><span class="sxs-lookup"><span data-stu-id="56692-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="56692-195">Spustíte ji v ladění nebo v režimu bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="56692-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="56692-197">Můžete ladit aplikaci klepnutím **služby IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="56692-197">You can debug the app by tapping the **IIS Express** button</span></span>

![Služba IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="56692-199">Výchozí šablony vám práci **domácí o** a **kontakt** odkazy.</span><span class="sxs-lookup"><span data-stu-id="56692-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="56692-200">Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů.</span><span class="sxs-lookup"><span data-stu-id="56692-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="56692-201">V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="56692-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona navigace v vpravo nahoře](start-mvc/_static/2.png)

<span data-ttu-id="56692-203">Pokud jsou spuštěné v režimu ladění, klepněte na **Shift + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="56692-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="56692-204">V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.</span><span class="sxs-lookup"><span data-stu-id="56692-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="56692-205">Next</span><span class="sxs-lookup"><span data-stu-id="56692-205">Next</span></span>](adding-controller.md)  
