---
title: Začínáme s stránky Razor v ASP.NET Core
author: rick-anderson
description: Zjistit základní informace o vytváření webové aplikace ASP.NET Core Razor stránky. Doporučuje se stránky Razor pro webové úlohy v ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278041"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="17bed-104">Začínáme s stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17bed-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="17bed-105">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17bed-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="17bed-106">Doporučujeme, abyste že podle verze 2.1 jádro ASP.NET v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="17bed-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="17bed-107">Má **mnohem** snáze sledovat a popisuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="17bed-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="17bed-108">Vyberte **ASP.NET Core 2.1** v modulu pro výběr verze.</span><span class="sxs-lookup"><span data-stu-id="17bed-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selektor verze obsahu](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="17bed-110">V tomto kurzu se dozvíte, jaké základní informace o vytváření webové aplikace ASP.NET Core Razor stránky.</span><span class="sxs-lookup"><span data-stu-id="17bed-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="17bed-111">Stránky Razor je doporučeným způsobem, jak sestavit uživatelského rozhraní pro webové aplikace v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="17bed-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="17bed-112">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="17bed-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="17bed-113">Windows: V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="17bed-113">Windows: This tutorial</span></span>
* <span data-ttu-id="17bed-114">Systému MacOS: [začít pracovat s stránky Razor pomocí sady Visual Studio pro Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="17bed-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="17bed-115">systému macOS, Linux a Windows: [Začínáme s ASP.NET Core Razor stránky v kódu Visual Studio](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="17bed-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="17bed-116">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="17bed-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="17bed-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17bed-117">Prerequisites</span></span>

<span data-ttu-id="17bed-118">[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="17bed-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="17bed-119">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="17bed-119">Create a Razor web app</span></span>

* <span data-ttu-id="17bed-120">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="17bed-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="17bed-121">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="17bed-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="17bed-122">Název projektu **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="17bed-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="17bed-123">Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.</span><span class="sxs-lookup"><span data-stu-id="17bed-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="17bed-124">![nové webové aplikace ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="17bed-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="17bed-125">Vyberte **ASP.NET Core 2.1** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17bed-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![nové webové aplikace ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="17bed-127">Šablony sady Visual Studio vytvoří projekt starter:</span><span class="sxs-lookup"><span data-stu-id="17bed-127">The Visual Studio template creates a starter project:</span></span>

![Průzkumník řešení](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="17bed-129">Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="17bed-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="17bed-130">Vyberte **přijmout** o souhlas pro sledování.</span><span class="sxs-lookup"><span data-stu-id="17bed-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="17bed-131">Tato aplikace nesleduje osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="17bed-131">This app doesn't track personal information.</span></span> <span data-ttu-id="17bed-132">Kód šablony generuje obsahuje prostředky ke splnění [obecné Data Protection nařízení (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="17bed-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="17bed-134">Následující obrázek znázorňuje aplikaci po přijetí sledování:</span><span class="sxs-lookup"><span data-stu-id="17bed-134">The following image shows the app after accepting tracking:</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="17bed-136">Visual Studio spustí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="17bed-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="17bed-137">Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="17bed-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="17bed-138">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="17bed-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="17bed-139">Localhost slouží pouze webových požadavků z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="17bed-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="17bed-140">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="17bed-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="17bed-141">Na předchozím obrázku číslo portu je 5 000.</span><span class="sxs-lookup"><span data-stu-id="17bed-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="17bed-142">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="17bed-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="17bed-143">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="17bed-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="17bed-144">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="17bed-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="17bed-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17bed-145">Prerequisites</span></span>

<span data-ttu-id="17bed-146">[! Zahrnout [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="17bed-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="17bed-147">Vytvoření webové aplikace Razor</span><span class="sxs-lookup"><span data-stu-id="17bed-147">Create a Razor web app</span></span>

* <span data-ttu-id="17bed-148">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="17bed-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="17bed-149">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="17bed-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="17bed-150">Název projektu **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="17bed-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="17bed-151">Je třeba název projektu *RazorPagesMovie* , obory názvů bude případy, kdy je zkopírujte a vložte kód.</span><span class="sxs-lookup"><span data-stu-id="17bed-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="17bed-152">![nové webové aplikace ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="17bed-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="17bed-153">Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17bed-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="17bed-154">Šablony sady Visual Studio vytvoří projekt starter:</span><span class="sxs-lookup"><span data-stu-id="17bed-154">The Visual Studio template creates a starter project:</span></span>

![Průzkumník řešení](razor-pages-start/_static/se.png)

<span data-ttu-id="17bed-156">Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu</span><span class="sxs-lookup"><span data-stu-id="17bed-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Index nebo Domovská stránka](razor-pages-start/_static/home.png)

* <span data-ttu-id="17bed-158">Visual Studio spustí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="17bed-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="17bed-159">Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="17bed-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="17bed-160">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="17bed-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="17bed-161">Localhost slouží pouze webových požadavků z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="17bed-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="17bed-162">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="17bed-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="17bed-163">Na předchozím obrázku číslo portu je 5 000.</span><span class="sxs-lookup"><span data-stu-id="17bed-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="17bed-164">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="17bed-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="17bed-165">Spuštění aplikace s **Ctrl + F5** (režim bez ladění) umožňuje provádět změny kódu, uložte soubor, aktualizujte stránku prohlížeče a podívejte se změny kódu.</span><span class="sxs-lookup"><span data-stu-id="17bed-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="17bed-166">Celá řada vývojářů dávají přednost používání režimu bez ladění k rychlému spusťte aplikaci a zobrazit změny.</span><span class="sxs-lookup"><span data-stu-id="17bed-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="17bed-167">Další krok: Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="17bed-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
