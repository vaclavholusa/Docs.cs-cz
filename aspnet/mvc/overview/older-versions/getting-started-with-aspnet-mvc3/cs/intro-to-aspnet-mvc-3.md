---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Úvod do architektury ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868926"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="bc8f8-103">Úvod do architektury ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="bc8f8-104">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="bc8f8-105">Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="bc8f8-106">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="bc8f8-107">V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="bc8f8-108">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="bc8f8-109">Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="bc8f8-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="bc8f8-110">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="bc8f8-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="bc8f8-111">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="bc8f8-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="bc8f8-112">Aktualizace nástrojů rozhraní ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="bc8f8-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="bc8f8-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="bc8f8-114">Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="bc8f8-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="bc8f8-115">Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="bc8f8-116">[Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="bc8f8-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="bc8f8-117">Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="bc8f8-118">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="bc8f8-118">What You'll Build</span></span>

<span data-ttu-id="bc8f8-119">Budete implementovat jednoduchou aplikaci seznamu film, která podporuje vytváření, úpravy a výpis filmy z databáze.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="bc8f8-120">Níže jsou dva snímky obrazovky aplikace, kterou budete sestavení.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="bc8f8-121">Obsahuje stránky, který zobrazí seznam filmy z databáze:</span><span class="sxs-lookup"><span data-stu-id="bc8f8-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="bc8f8-123">Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete podrobnosti o jednotlivých ty.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="bc8f8-124">Všechny scénáře zadávání dat zahrnují ověření a zkontrolujte, že data uložená v databázi správné.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="bc8f8-125">Dovedností, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="bc8f8-125">Skills You'll Learn</span></span>

<span data-ttu-id="bc8f8-126">Zde je, co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="bc8f8-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="bc8f8-127">Postup vytvoření nového projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="bc8f8-128">Jak vytvořit rozhraní ASP.NET MVC, kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="bc8f8-129">Jak vytvořit novou databázi pomocí Entity Framework Code First zlepší.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="bc8f8-130">Jak načíst a zobrazit data.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="bc8f8-131">Jak upravit data a povolit data ověření.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="bc8f8-132">Začínáme</span><span class="sxs-lookup"><span data-stu-id="bc8f8-132">Getting Started</span></span>

<span data-ttu-id="bc8f8-133">Spusťte spuštěním Visual Web Developer 2010 Express ("Visual Web Developer" pro zkrácení) a vyberte **nový projekt** z **spustit** stránky.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="bc8f8-134">Visual Web Developer je IDE, nebo integrované vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="bc8f8-135">Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="bc8f8-136">V aplikaci Visual Web Developer je panelu nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="bc8f8-137">Je také nabídku, která obsahuje jiný způsob, jak provádět úlohy v prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="bc8f8-138">(Například místo výběru **nový projekt** z **spustit** stránky, můžete použít nabídku a vyberte **soubor** &gt; **nový projekt**.)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="bc8f8-139">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="bc8f8-139">Creating Your First Application</span></span>

<span data-ttu-id="bc8f8-140">Můžete vytvořit pomocí jazyka Visual Basic a Visual C# jako programovací jazyk aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="bc8f8-141">Vyberte Visual C# na levé straně a pak vyberte **webové aplikace ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="bc8f8-142">Název projektu "MvcMovie" a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="bc8f8-143">(Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.)</span><span class="sxs-lookup"><span data-stu-id="bc8f8-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="bc8f8-144">V **nový ASP.NET MVC 3 projekt** dialogové okno, vyberte **Internetové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="bc8f8-145">Zkontrolujte **HTML5 použití značek** a nechte **Razor** jako výchozí modul zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="bc8f8-146">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-146">Click **OK**.</span></span> <span data-ttu-id="bc8f8-147">Visual Web Developer použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce!</span><span class="sxs-lookup"><span data-stu-id="bc8f8-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="bc8f8-148">Jedná se jednoduché "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="bc8f8-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="bc8f8-149">projektu ale je dobrým místem, kde spustit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="bc8f8-150">Z **ladění** nabídce vyberte možnost **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="bc8f8-151">Všimněte si, že je klávesové zkratky pro spuštění ladění F5.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="bc8f8-152">F5 způsobí, že Visual Web Developer spustí webový server vývoj a spuštění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="bc8f8-153">Visual Web Developer pak spustí prohlížeč a otevře domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="bc8f8-154">Všimněte si, že panelu Adresa prohlížeče uvádí `localhost` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="bc8f8-155">Je to způsobeno `localhost` vždycky směřuje na vlastní místní počítač, který běží v tomto případě aplikace, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="bc8f8-156">Po spuštění webového projektu Visual Web Developer náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="bc8f8-157">Na následujícím obrázku je číslo portu náhodných 43246.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="bc8f8-158">Při spuštění aplikace, pravděpodobně uvidíte jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="bc8f8-159">Okamžitě po nasazení této výchozí šablony vám dává dvě stránky přejděte a základní přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="bc8f8-160">Dalším krokem je a změnit tak, jak tato aplikace funguje trochu informace o architektuře ASP.NET MVC v procesu.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="bc8f8-161">Zavřete prohlížeč a zkuste změnit nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="bc8f8-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bc8f8-162">Next</span><span class="sxs-lookup"><span data-stu-id="bc8f8-162">Next</span></span>](adding-a-controller.md)
