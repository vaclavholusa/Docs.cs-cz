---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Úvod do architektury ASP.NET MVC | Microsoft Docs
author: shanselman
description: Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="3e3cc-104">Úvod do architektury ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3e3cc-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="3e3cc-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="3e3cc-106">Pokud v tomto kurzu je k dispozici aktualizovaná verze [sem](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="3e3cc-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="3e3cc-107">Nový kurz používá ASP.NET MVC 5, který obsahuje mnoho vylepšení v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="3e3cc-108">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="3e3cc-109">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="3e3cc-110">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="3e3cc-111">Provedeme naše první webové aplikace ASP.NET MVC pomocí [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="3e3cc-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="3e3cc-112">Vytočit trochu film seznamu aplikace, která bude dejte nám vytvořit a seznam filmy.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="3e3cc-113">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="3e3cc-113">What You'll Build</span></span>

<span data-ttu-id="3e3cc-114">Tady jsou snímky dvě aplikace, kterou budete sestavení.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="3e3cc-115">Budete mít jednoduchou tabulku filmy s různé sloupce.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="3e3cc-116">[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="3e3cc-117">A, musíte vytvořit formulář, filmy jsme můžete přidat do seznamu.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="3e3cc-118">[![Vytvořit film – Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="3e3cc-119">Dovedností, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="3e3cc-119">Skills You'll Learn</span></span>

<span data-ttu-id="3e3cc-120">V tomto kurzu naučit se základy vytváření webová aplikace ASP.NET MVC pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="3e3cc-121">Naučíte:</span><span class="sxs-lookup"><span data-stu-id="3e3cc-121">You'll learn:</span></span>

- <span data-ttu-id="3e3cc-122">Postup vytvoření nového projektu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3e3cc-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="3e3cc-123">Postup vytvoření nové databáze s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="3e3cc-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="3e3cc-124">Postup vytvoření zobrazení a Kontrolery architektury ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3e3cc-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="3e3cc-125">Jak načíst a zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="3e3cc-125">How to retrieve and display data</span></span>
- <span data-ttu-id="3e3cc-126">Jak upravit data a povolit ověřování dat</span><span class="sxs-lookup"><span data-stu-id="3e3cc-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="3e3cc-127">Postup aktualizace schématu databáze</span><span class="sxs-lookup"><span data-stu-id="3e3cc-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="3e3cc-128">Začínáme</span><span class="sxs-lookup"><span data-stu-id="3e3cc-128">Get Started</span></span>

<span data-ttu-id="3e3cc-129">Spusťte spuštěním Visual Web Developer 2010 Express (I budete volání "VWD" od této chvíle) a vyberte nový projekt z obrazovky Start.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="3e3cc-130">Visual Web Developer je IDE, nebo integrované vývojářského prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="3e3cc-131">Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="3e3cc-132">Je panelu nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici je, a také v nabídce může také použili jste vyberte souboru | Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="3e3cc-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="3e3cc-134">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="3e3cc-134">Creating Your First Application</span></span>

<span data-ttu-id="3e3cc-135">Můžete vytvořit aplikací pomocí jazyka Visual Basic a Visual C#.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="3e3cc-136">Nyní, vyberte Visual C# na levé straně pak vyberte "Webová aplikace ASP.NET MVC 2."</span><span class="sxs-lookup"><span data-stu-id="3e3cc-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="3e3cc-137">Název projektu "Filmy" a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="3e3cc-138">[![Nový projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="3e3cc-139">Na pravé straně je Průzkumník řešení zobrazující všechny soubory a složky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="3e3cc-140">Okno big uprostřed je, kde můžete upravovat kód a většinu doby.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="3e3cc-141">Visual Studio použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce!</span><span class="sxs-lookup"><span data-stu-id="3e3cc-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="3e3cc-142">Toto je jednoduchý "Hello, World!</span><span class="sxs-lookup"><span data-stu-id="3e3cc-142">This is a simple "Hello World!</span></span> <span data-ttu-id="3e3cc-143">projekt a je vhodná pro naši aplikaci spusťte.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="3e3cc-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="3e3cc-145">Kliknutím na tlačítko "play" na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-145">Select the "play" button to the toolbar.</span></span>

![Spuštění ladění](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="3e3cc-147">Je zelenou šipku vpravo, který bude kompilovat vašeho programu a spusťte aplikaci ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="3e3cc-148">*Poznámka: Můžete místo toho na klávesnici stiskněte klávesu F5, nebo vybrat ladění -&gt;spustit ladění z nabídky "Debug".*</span><span class="sxs-lookup"><span data-stu-id="3e3cc-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="3e3cc-149">To způsobí, že Visual Web Developer spustí webový vývoj-server a spuštění webové aplikace (neexistují žádné konfigurace nebo Ruční postup vyžadovaný pro povolení této).</span><span class="sxs-lookup"><span data-stu-id="3e3cc-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="3e3cc-150">Pak se spustí prohlížeč a nakonfigurovat, aby procházet Domovská stránka aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="3e3cc-151">Všimněte si níže, panelu Adresa prohlížeče říká "localhost" a podobný example.com. Je to způsobeno localhost vždycky směřuje na vlastní místní počítač – v takovém případě je spuštěna aplikace, kterou jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="3e3cc-152">[![Domovská stránka](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="3e3cc-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="3e3cc-153">Ihned poskytuje tuto výchozí šablonu můžete dvě stránky přejděte a základní přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="3e3cc-154">Umožňuje změnit, jak tato aplikace funguje a chvíli informace o architektuře ASP.NET MVC v procesu.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="3e3cc-155">Zavřete okno prohlížeče a umožňuje změnit nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="3e3cc-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3e3cc-156">Next</span><span class="sxs-lookup"><span data-stu-id="3e3cc-156">Next</span></span>](getting-started-with-mvc-part2.md)
