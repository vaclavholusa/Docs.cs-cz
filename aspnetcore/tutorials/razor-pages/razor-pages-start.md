---
title: Začínáme se stránkami Razor v ASP.NET Core
author: rick-anderson
description: Seznamte se se základy vytváření webové aplikace ASP.NET Core Razor Pages. Stránky Razor se doporučuje pro webové úlohy v ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 20fc1dfebda66a23c4682aa8e94523a710abf8b2
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340014"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e66bc-104">Začínáme se stránkami Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e66bc-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e66bc-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e66bc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e66bc-106">Doporučujeme, abyste že podle ASP.NET Core 2.1 verzi tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e66bc-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="e66bc-107">Má **většinu** usnadňuje její sledování a zahrnuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="e66bc-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="e66bc-108">Vyberte **ASP.NET Core 2.1** ve výběru verze.</span><span class="sxs-lookup"><span data-stu-id="e66bc-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Volič verze obsahu](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="e66bc-110">V tomto kurzu se naučíte se základy vytváření webové aplikace ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e66bc-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="e66bc-111">Stránky Razor je doporučený postup pro vytváření uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e66bc-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="e66bc-112">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="e66bc-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="e66bc-113">Windows: V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="e66bc-113">Windows: This tutorial</span></span>
* <span data-ttu-id="e66bc-114">MacOS: [Začínáme se stránkami Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e66bc-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="e66bc-115">macOS, Linux a Windows: [Začínáme s ASP.NET Core Razor Pages ve Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e66bc-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="e66bc-116">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e66bc-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="e66bc-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e66bc-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e66bc-118">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="e66bc-118">Create a Razor web app</span></span>

* <span data-ttu-id="e66bc-119">Ze sady Visual Studio **souboru** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e66bc-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e66bc-120">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e66bc-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e66bc-121">Pojmenujte projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e66bc-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e66bc-122">Je důležité projekt pojmenujte *RazorPagesMovie* tak obory názvů budou porovnávány názvy při zkopírujete/vložíte kód.</span><span class="sxs-lookup"><span data-stu-id="e66bc-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="e66bc-123">![Nová webová aplikace ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="e66bc-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="e66bc-124">Vyberte **ASP.NET Core 2.1** v rozevíracím seznamu a pak vyberte **webovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="e66bc-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nová webová aplikace ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="e66bc-126">Šablony sady Visual Studio vytvoří počáteční projekt:</span><span class="sxs-lookup"><span data-stu-id="e66bc-126">The Visual Studio template creates a starter project:</span></span>

![Průzkumník řešení](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="e66bc-128">Stisknutím klávesy **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** spustit bez připojení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="e66bc-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="e66bc-129">Vyberte **přijmout** souhlas sledování.</span><span class="sxs-lookup"><span data-stu-id="e66bc-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e66bc-130">Tato aplikace nesleduje osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="e66bc-130">This app doesn't track personal information.</span></span> <span data-ttu-id="e66bc-131">Tento kód vygenerovanou šablonu obsahuje prostředky, které vám pomohou splnit [obecného Regulation (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e66bc-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="e66bc-133">Následující obrázek znázorňuje aplikaci po přijetí sledování:</span><span class="sxs-lookup"><span data-stu-id="e66bc-133">The following image shows the app after accepting tracking:</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="e66bc-135">Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e66bc-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e66bc-136">Zobrazí se panel Adresa `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e66bc-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e66bc-137">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="e66bc-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e66bc-138">Localhost slouží pouze webové požadavky z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="e66bc-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e66bc-139">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="e66bc-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e66bc-140">Na předchozím obrázku číslo portu je 5001.</span><span class="sxs-lookup"><span data-stu-id="e66bc-140">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="e66bc-141">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="e66bc-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e66bc-142">Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu.</span><span class="sxs-lookup"><span data-stu-id="e66bc-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e66bc-143">Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.</span><span class="sxs-lookup"><span data-stu-id="e66bc-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="e66bc-144">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e66bc-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e66bc-145">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="e66bc-145">Create a Razor web app</span></span>

* <span data-ttu-id="e66bc-146">Ze sady Visual Studio **souboru** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e66bc-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e66bc-147">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e66bc-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e66bc-148">Pojmenujte projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e66bc-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e66bc-149">Je důležité projekt pojmenujte *RazorPagesMovie* tak obory názvů budou porovnávány názvy při zkopírujete/vložíte kód.</span><span class="sxs-lookup"><span data-stu-id="e66bc-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="e66bc-150">![Nová webová aplikace ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="e66bc-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="e66bc-151">Vyberte **ASP.NET Core 2.0** v rozevíracím seznamu a pak vyberte **webovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="e66bc-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="e66bc-152">Šablony sady Visual Studio vytvoří počáteční projekt:</span><span class="sxs-lookup"><span data-stu-id="e66bc-152">The Visual Studio template creates a starter project:</span></span>

![Průzkumník řešení](razor-pages-start/_static/se.png)

<span data-ttu-id="e66bc-154">Stisknutím klávesy **F5** ke spuštění aplikace v režimu ladění nebo **Ctrl-F5** spustit bez připojení ladicího programu</span><span class="sxs-lookup"><span data-stu-id="e66bc-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* <span data-ttu-id="e66bc-156">Visual Studio spustí [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spouští vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="e66bc-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e66bc-157">Zobrazí se panel Adresa `localhost:port#` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e66bc-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e66bc-158">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="e66bc-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e66bc-159">Localhost slouží pouze webové požadavky z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="e66bc-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e66bc-160">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="e66bc-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e66bc-161">Na předchozím obrázku je číslo portu 5000.</span><span class="sxs-lookup"><span data-stu-id="e66bc-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="e66bc-162">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="e66bc-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e66bc-163">Spuštění aplikace s **Ctrl + F5** (bez ladění režim) umožňuje provádět změny kódu, uložte soubor, aktualizujte prohlížeč a zobrazení změn kódu.</span><span class="sxs-lookup"><span data-stu-id="e66bc-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e66bc-164">Mnoho vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a podívejte se na změny.</span><span class="sxs-lookup"><span data-stu-id="e66bc-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="e66bc-165">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="e66bc-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
