---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Úvod do ASP.NET MVC 3 (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: b0ff8da1911b36e6e74e5c7057f27d891ad57f61
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832286"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="2daee-103">Úvod do ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="2daee-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="2daee-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2daee-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="2daee-105">Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2daee-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="2daee-106">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="2daee-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="2daee-107">V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2daee-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="2daee-108">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="2daee-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="2daee-109">Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="2daee-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="2daee-110">Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="2daee-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="2daee-111">Visual Studio Web Developer Express SP1 požadavky</span><span class="sxs-lookup"><span data-stu-id="2daee-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="2daee-112">ASP.NET MVC 3 nástroje Update</span><span class="sxs-lookup"><span data-stu-id="2daee-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="2daee-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)</span><span class="sxs-lookup"><span data-stu-id="2daee-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="2daee-114">Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="2daee-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="2daee-115">Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="2daee-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="2daee-116">[Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="2daee-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="2daee-117">Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2daee-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="2daee-118">Co budete vytvářet</span><span class="sxs-lookup"><span data-stu-id="2daee-118">What You'll Build</span></span>

<span data-ttu-id="2daee-119">Budete implementujte jednoduchou aplikaci výpis film, který podporuje vytváření, úpravy a výpisu videa z databáze.</span><span class="sxs-lookup"><span data-stu-id="2daee-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="2daee-120">Níže jsou uvedeny dva snímky obrazovky z aplikace, kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="2daee-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="2daee-121">Stránka zobrazující seznam videa z databáze obsahuje:</span><span class="sxs-lookup"><span data-stu-id="2daee-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="2daee-123">Aplikace také umožňuje přidávat, upravovat a odstraňovat filmy, jakož i najdete v podrobnostech o jednotlivé značky.</span><span class="sxs-lookup"><span data-stu-id="2daee-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="2daee-124">Všechny scénáře zadávání dat zahrnout ověřování k ověření, že data uložená v databázi je správná.</span><span class="sxs-lookup"><span data-stu-id="2daee-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="2daee-125">Dovednosti, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="2daee-125">Skills You'll Learn</span></span>

<span data-ttu-id="2daee-126">Zde je, co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="2daee-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="2daee-127">Postup vytvoření nového projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2daee-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="2daee-128">Jak vytvořit rozhraní ASP.NET MVC, kontrolerů a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2daee-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="2daee-129">Jak vytvořit novou databázi pomocí Entity Framework Code First paradigma.</span><span class="sxs-lookup"><span data-stu-id="2daee-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="2daee-130">Jak načíst a zobrazit data.</span><span class="sxs-lookup"><span data-stu-id="2daee-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="2daee-131">Postup úpravy dat a povolení ověření dat.</span><span class="sxs-lookup"><span data-stu-id="2daee-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2daee-132">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2daee-132">Getting Started</span></span>

<span data-ttu-id="2daee-133">Začněte tím, že spuštění Visual Web Developer 2010 Express ("Visual Web Developer" zkráceně) a vyberte **nový projekt** z **Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="2daee-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="2daee-134">Visual Web Developer je integrované vývojové prostředí, nebo integrovaného vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="2daee-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="2daee-135">Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací.</span><span class="sxs-lookup"><span data-stu-id="2daee-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="2daee-136">V aplikaci Visual Web Developer je panel nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici pro vás.</span><span class="sxs-lookup"><span data-stu-id="2daee-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="2daee-137">Je také nabídka, která nabízí další způsob, jak provádět úkoly v integrovaném vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="2daee-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="2daee-138">(Například místo výběru **nový projekt** z **Start** stránky, můžete použít nabídku a vyberte **souboru** &gt; **nový projekt**.)</span><span class="sxs-lookup"><span data-stu-id="2daee-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="2daee-139">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="2daee-139">Creating Your First Application</span></span>

<span data-ttu-id="2daee-140">Můžete vytvářet aplikace pomocí jazyka Visual Basic nebo Visual C# jako programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="2daee-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="2daee-141">Vyberte jazyk Visual C# na levé straně a pak vyberte **webové aplikace ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="2daee-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="2daee-142">Pojmenujte svůj projekt "MvcMovie" a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2daee-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="2daee-143">(Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.)</span><span class="sxs-lookup"><span data-stu-id="2daee-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="2daee-144">V **nového projektu ASP.NET MVC 3** dialogu **internetovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="2daee-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="2daee-145">Zkontrolujte **značek HTML5 použití** a nechat **Razor** jako výchozí modul zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2daee-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="2daee-146">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2daee-146">Click **OK**.</span></span> <span data-ttu-id="2daee-147">Visual Web Developer používá výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá!</span><span class="sxs-lookup"><span data-stu-id="2daee-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="2daee-148">Jde jednoduchý "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="2daee-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="2daee-149">projekt a je vhodné místo pro spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2daee-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="2daee-150">Z **ladění** nabídce vyberte možnost **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="2daee-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="2daee-151">Všimněte si, že klávesové zkratky pro spuštění ladění F5.</span><span class="sxs-lookup"><span data-stu-id="2daee-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="2daee-152">F5 způsobí, že spuštění vývojovému webovému serveru a spuštění webové aplikace Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="2daee-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="2daee-153">Visual Web Developer pak spustí prohlížeč a otevře domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="2daee-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="2daee-154">Všimněte si, že na panelu Adresa prohlížeče `localhost` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2daee-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="2daee-155">Důvodem je, že `localhost` vždy odkazuje na vaše vlastní místní počítače v tomto případě je spuštěna aplikace právě jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2daee-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="2daee-156">Při spuštění projektu webové aplikace Visual Web Developer náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="2daee-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2daee-157">Na následujícím obrázku je náhodný port číslo 43246.</span><span class="sxs-lookup"><span data-stu-id="2daee-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="2daee-158">Když aplikaci spouštíte, pravděpodobně uvidíte jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="2daee-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="2daee-159">Předem připravené tuto výchozí šablonu nabízí dvě stránky na web a základní přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="2daee-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="2daee-160">Dalším krokem je změnit, jak tato aplikace funguje a ještě něco architekturu ASP.NET MVC hlouběji v procesu.</span><span class="sxs-lookup"><span data-stu-id="2daee-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="2daee-161">Zavřete okno prohlížeče a Změníme kód.</span><span class="sxs-lookup"><span data-stu-id="2daee-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2daee-162">Next</span><span class="sxs-lookup"><span data-stu-id="2daee-162">Next</span></span>](adding-a-controller.md)
