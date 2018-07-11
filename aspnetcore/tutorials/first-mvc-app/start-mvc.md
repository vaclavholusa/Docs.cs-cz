---
title: Začínáme s ASP.NET Core MVC a sady Visual Studio
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET Core MVC a sady Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217975"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="191a4-103">Začínáme s ASP.NET Core MVC a sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="191a4-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="191a4-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="191a4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="191a4-105">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="191a4-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="191a4-106">macOS: [vytvoření aplikace ASP.NET Core MVC se sadou Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="191a4-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="191a4-107">Windows: [vytvořit v aplikaci MVC ASP.NET Core pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="191a4-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="191a4-108">macOS, Linux a Windows: [vytvoření aplikace ASP.NET Core MVC pomocí Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="191a4-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="191a4-109">Instalace sady Visual Studio a .NET Core</span><span class="sxs-lookup"><span data-stu-id="191a4-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="191a4-110">[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="191a4-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="191a4-111">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="191a4-111">Create a web app</span></span>

<span data-ttu-id="191a4-112">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="191a4-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="191a4-114">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="191a4-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="191a4-115">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="191a4-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="191a4-116">V prostředním podokně, klepněte na **webová aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="191a4-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="191a4-117">Pojmenujte projekt "MvcMovie" (je třeba název projektu "MvcMovie", takže při kopírování kódu bude odpovídat oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="191a4-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="191a4-118">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="191a4-118">Tap **OK**</span></span>

![<span data-ttu-id="191a4-119">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="191a4-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="191a4-120">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="191a4-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="191a4-121">V poli verze selektoru rozevíracího seznamu vyberte **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="191a4-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="191a4-122">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="191a4-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="191a4-123">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="191a4-123">Tap **OK**.</span></span>

![<span data-ttu-id="191a4-124">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="191a4-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="191a4-125">Visual Studio používá výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="191a4-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="191a4-126">Zadáním názvu projektu a výběr několika možností Teď máte funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="191a4-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="191a4-127">Toto je základní počáteční projekt, a je vhodné oddělení na zahájení,</span><span class="sxs-lookup"><span data-stu-id="191a4-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="191a4-128">Klepněte na **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="191a4-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="191a4-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="191a4-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="191a4-130">Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="191a4-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="191a4-131">Všimněte si, že do adresního řádku ukazuje `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="191a4-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="191a4-132">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="191a4-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="191a4-133">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="191a4-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="191a4-134">Na obrázku výše je číslo portu 5000.</span><span class="sxs-lookup"><span data-stu-id="191a4-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="191a4-135">Adresa URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="191a4-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="191a4-136">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="191a4-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="191a4-137">Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu.</span><span class="sxs-lookup"><span data-stu-id="191a4-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="191a4-138">Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.</span><span class="sxs-lookup"><span data-stu-id="191a4-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="191a4-139">Spustíte ji v ladění nebo v režimu bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="191a4-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="191a4-141">Můžete ladit aplikaci klepnutím **služby IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="191a4-141">You can debug the app by tapping the **IIS Express** button</span></span>

![Služba IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="191a4-143">Výchozí šablony vám práci **domácí o** a **kontakt** odkazy.</span><span class="sxs-lookup"><span data-stu-id="191a4-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="191a4-144">Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů.</span><span class="sxs-lookup"><span data-stu-id="191a4-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="191a4-145">V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="191a4-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona navigace v vpravo nahoře](start-mvc/_static/2.png)

<span data-ttu-id="191a4-147">Pokud jsou spuštěné v režimu ladění, klepněte na **Shift + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="191a4-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="191a4-148">V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.</span><span class="sxs-lookup"><span data-stu-id="191a4-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="191a4-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="191a4-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="191a4-150">[! Zahrnout [] (~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="191a4-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="191a4-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="191a4-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="191a4-152">Instalace sady Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="191a4-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="191a4-153">Vyberte stáhnout Community.</span><span class="sxs-lookup"><span data-stu-id="191a4-153">Select the Community download.</span></span> <span data-ttu-id="191a4-154">Tento krok přeskočte, pokud máte nainstalovanou sadu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="191a4-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="191a4-155">Instalační program sady Visual Studio 2017 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="191a4-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="191a4-156">Spusťte instalační program a vybrat následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="191a4-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="191a4-157">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="191a4-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="191a4-158">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="191a4-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET a webového vývoje ** (v části ** Web a Cloud **)](start-mvc/_static/web_workload.png)

![* *.NET core napříč. mezi platfrom vývoj ** (v části ** Další sady nástrojů **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="191a4-161">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="191a4-161">Create a web app</span></span>

<span data-ttu-id="191a4-162">Ze sady Visual Studio, vyberte **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="191a4-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Soubor > Nový > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="191a4-164">Dokončení **nový projekt** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="191a4-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="191a4-165">V levém podokně, klepněte na **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="191a4-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="191a4-166">V prostředním podokně, klepněte na **webová aplikace ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="191a4-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="191a4-167">Pojmenujte projekt "MvcMovie" (je třeba název projektu "MvcMovie", takže při kopírování kódu bude odpovídat oboru názvů.)</span><span class="sxs-lookup"><span data-stu-id="191a4-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="191a4-168">Klepněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="191a4-168">Tap **OK**</span></span>

![<span data-ttu-id="191a4-169">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="191a4-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="191a4-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="191a4-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="191a4-171">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="191a4-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="191a4-172">V poli verze selektoru rozevíracího seznamu vyberte **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="191a4-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="191a4-173">Vyberte **webové Application(Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="191a4-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="191a4-174">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="191a4-174">Tap **OK**.</span></span>

![<span data-ttu-id="191a4-175">Dialogové okno Nový projekt, .net core v levém podokně, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="191a4-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="191a4-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="191a4-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="191a4-177">Dokončení **nové základní webové aplikace ASP.NET (.NET Core) – MvcMovie** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="191a4-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="191a4-178">V tap verze selektoru rozevíracího seznamu **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="191a4-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="191a4-179">Klepněte na **webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="191a4-179">Tap **Web Application**</span></span>
* <span data-ttu-id="191a4-180">Ponechte výchozí **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="191a4-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="191a4-181">Klepněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="191a4-181">Tap **OK**.</span></span>

![Nová webová aplikace ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="191a4-183">Visual Studio používá výchozí šablonu pro projekt MVC, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="191a4-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="191a4-184">Zadáním názvu projektu a výběr několika možností Teď máte funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="191a4-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="191a4-185">Toto je základní počáteční projekt, a je vhodné oddělení na zahájení,</span><span class="sxs-lookup"><span data-stu-id="191a4-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="191a4-186">Klepněte na **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** v režimu bez ladění.</span><span class="sxs-lookup"><span data-stu-id="191a4-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="191a4-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![spuštění aplikace](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="191a4-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="191a4-188">Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="191a4-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="191a4-189">Všimněte si, že do adresního řádku ukazuje `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="191a4-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="191a4-190">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="191a4-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="191a4-191">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="191a4-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="191a4-192">Na obrázku výše je číslo portu 5000.</span><span class="sxs-lookup"><span data-stu-id="191a4-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="191a4-193">Adresa URL v prohlížeči zobrazí `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="191a4-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="191a4-194">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="191a4-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="191a4-195">Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu.</span><span class="sxs-lookup"><span data-stu-id="191a4-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="191a4-196">Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.</span><span class="sxs-lookup"><span data-stu-id="191a4-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="191a4-197">Spustíte ji v ladění nebo v režimu bez ladění z **ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="191a4-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Ladění nabídky](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="191a4-199">Můžete ladit aplikaci klepnutím **služby IIS Express** tlačítko</span><span class="sxs-lookup"><span data-stu-id="191a4-199">You can debug the app by tapping the **IIS Express** button</span></span>

![Služba IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="191a4-201">Výchozí šablony vám práci **domácí o** a **kontakt** odkazy.</span><span class="sxs-lookup"><span data-stu-id="191a4-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="191a4-202">Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů.</span><span class="sxs-lookup"><span data-stu-id="191a4-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="191a4-203">V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="191a4-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona navigace v vpravo nahoře](start-mvc/_static/2.png)

<span data-ttu-id="191a4-205">Pokud jsou spuštěné v režimu ladění, klepněte na **Shift + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="191a4-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="191a4-206">V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.</span><span class="sxs-lookup"><span data-stu-id="191a4-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="191a4-207">Next</span><span class="sxs-lookup"><span data-stu-id="191a4-207">Next</span></span>](adding-controller.md)  
