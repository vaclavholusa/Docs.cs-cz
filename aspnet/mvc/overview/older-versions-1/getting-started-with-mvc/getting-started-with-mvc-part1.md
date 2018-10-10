---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Úvod do ASP.NET MVC | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 2d9c1dd0dd3c9f892b42b0f29ac3361a7f2b638c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910852"
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="7d52a-104">Úvod do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7d52a-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="7d52a-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="7d52a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="7d52a-106">Pokud v tomto kurzu je k dispozici aktualizovaná verze [tady](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="7d52a-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="7d52a-107">Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7d52a-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="7d52a-108">Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7d52a-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="7d52a-109">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="7d52a-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="7d52a-110">Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.</span><span class="sxs-lookup"><span data-stu-id="7d52a-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="7d52a-111">Vytvoříme naši první webové aplikace ASP.NET MVC pomocí [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="7d52a-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="7d52a-112">Uděláme trochu film aplikaci v seznamu, který bude dejte nám vytvořit a seznamu videa.</span><span class="sxs-lookup"><span data-stu-id="7d52a-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="7d52a-113">Co budete vytvářet</span><span class="sxs-lookup"><span data-stu-id="7d52a-113">What You'll Build</span></span>

<span data-ttu-id="7d52a-114">Tady jsou dva snímky obrazovky aplikace, kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="7d52a-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="7d52a-115">Budete mít jednoduchou tabulku s filmů s různé sloupce.</span><span class="sxs-lookup"><span data-stu-id="7d52a-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="7d52a-116">[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d52a-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="7d52a-117">A proto jsme do seznamu Přidat videa budete mít formulář vytvořit.</span><span class="sxs-lookup"><span data-stu-id="7d52a-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="7d52a-118">[![Vytvořit aplikaci Windows Internet Explorer (2) film-](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7d52a-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="7d52a-119">Dovednosti, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="7d52a-119">Skills You'll Learn</span></span>

<span data-ttu-id="7d52a-120">V tomto kurzu se seznámíte se základy vytváření webové aplikace ASP.NET MVC pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d52a-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="7d52a-121">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="7d52a-121">You'll learn:</span></span>

- <span data-ttu-id="7d52a-122">Jak vytvořit nový projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7d52a-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="7d52a-123">Jak vytvořit novou databázi s využitím SQL serveru</span><span class="sxs-lookup"><span data-stu-id="7d52a-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="7d52a-124">Postup vytvoření zobrazení a Kontrolery ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7d52a-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="7d52a-125">Načtení a zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="7d52a-125">How to retrieve and display data</span></span>
- <span data-ttu-id="7d52a-126">Úprava dat a povolení ověření dat</span><span class="sxs-lookup"><span data-stu-id="7d52a-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="7d52a-127">Postup aktualizace schématu databáze</span><span class="sxs-lookup"><span data-stu-id="7d52a-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="7d52a-128">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7d52a-128">Get Started</span></span>

<span data-ttu-id="7d52a-129">Začněte tím, že spuštění Visual Web Developer 2010 Express (nazvu to "VWD" od této chvíle) a vyberte nový projekt z obrazovky Start.</span><span class="sxs-lookup"><span data-stu-id="7d52a-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="7d52a-130">Visual Web Developer je integrované vývojové prostředí, nebo integrované prostředí pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="7d52a-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="7d52a-131">Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací.</span><span class="sxs-lookup"><span data-stu-id="7d52a-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="7d52a-132">Zde je panel nástrojů podél horního zobrazuje různé možnosti, které jsou k dispozici pro vás, stejně jako nabídka může také použitých pro výběr souboru | Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="7d52a-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="7d52a-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7d52a-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="7d52a-134">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="7d52a-134">Creating Your First Application</span></span>

<span data-ttu-id="7d52a-135">Můžete vytvářet aplikace pomocí jazyka Visual Basic nebo Visual C#.</span><span class="sxs-lookup"><span data-stu-id="7d52a-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="7d52a-136">Teď vyberte Visual C# na levé straně pak vyberte "Webové aplikace ASP.NET MVC 2."</span><span class="sxs-lookup"><span data-stu-id="7d52a-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="7d52a-137">Pojmenujte svůj projekt "Filmy" a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="7d52a-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="7d52a-138">[![Nový projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7d52a-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="7d52a-139">Na pravé straně se Průzkumník řešení zobrazující všechny soubory a složky ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7d52a-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="7d52a-140">Kde úpravy kódu a tráví většinu svého času je okno velké objemy uprostřed.</span><span class="sxs-lookup"><span data-stu-id="7d52a-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="7d52a-141">Visual Studio použít výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá!</span><span class="sxs-lookup"><span data-stu-id="7d52a-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="7d52a-142">Toto je jednoduchý "Hello World</span><span class="sxs-lookup"><span data-stu-id="7d52a-142">This is a simple "Hello World!</span></span> <span data-ttu-id="7d52a-143">projekt a je vhodné oddělení na zahájení pro naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7d52a-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="7d52a-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="7d52a-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="7d52a-145">Vyberte na panelu nástrojů tlačítko "Přehrát akci".</span><span class="sxs-lookup"><span data-stu-id="7d52a-145">Select the "play" button to the toolbar.</span></span>

![Spustit ladění](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="7d52a-147">Je zelená šipka směřující doprava, který bude zkompilujete program a spusťte aplikaci ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7d52a-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="7d52a-148">*Poznámka: Můžete místo toho stiskněte klávesu F5 na klávesnici, nebo vybrat ladění –&gt;spustit ladění z nabídky "Ladění".*</span><span class="sxs-lookup"><span data-stu-id="7d52a-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="7d52a-149">To způsobí, že Visual Web Developer ke spuštění webový server vývoje a spuštění webové aplikace (neexistují žádné konfigurace nebo Ruční postup vyžadovaný pro tuto možnost povolte,).</span><span class="sxs-lookup"><span data-stu-id="7d52a-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="7d52a-150">Bude potom spusťte prohlížeč a nakonfigurovat, aby procházet Domovská stránka aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d52a-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="7d52a-151">Všimněte si, že níže, že na panelu Adresa prohlížeče říká "localhost" a vypadat example.com.</span><span class="sxs-lookup"><span data-stu-id="7d52a-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="7d52a-152">Důvodem je, localhost vždy odkazuje na vaše vlastní místní počítače – v tomto případě je spuštěna aplikace, kterou jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7d52a-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="7d52a-153">[![Domovská stránka](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="7d52a-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="7d52a-154">Ihned poskytuje tuto výchozí šablonu můžete dvě stránky přejděte a základní přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="7d52a-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="7d52a-155">Umožňuje změnit, jak tato aplikace funguje a ještě něco architekturu ASP.NET MVC hlouběji v procesu.</span><span class="sxs-lookup"><span data-stu-id="7d52a-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="7d52a-156">Zavřete okno prohlížeče a umožňuje změnit nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="7d52a-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7d52a-157">Next</span><span class="sxs-lookup"><span data-stu-id="7d52a-157">Next</span></span>](getting-started-with-mvc-part2.md)
