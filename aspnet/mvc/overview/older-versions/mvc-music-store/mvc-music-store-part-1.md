---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Část 1: Přehled a soubor -> Nový projekt | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 1 zahrnuje přehled a soubor -> Nový projekt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755091"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="b53bf-104">Část 1: Přehled a soubor -> Nový projekt</span><span class="sxs-lookup"><span data-stu-id="b53bf-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="b53bf-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b53bf-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b53bf-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b53bf-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b53bf-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="b53bf-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b53bf-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="b53bf-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b53bf-109">1. část obsahuje přehled a soubor -&gt;nový projekt.</span><span class="sxs-lookup"><span data-stu-id="b53bf-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="b53bf-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="b53bf-110">Overview</span></span>

<span data-ttu-id="b53bf-111">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a aplikace Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="b53bf-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="b53bf-112">Jsme budete spuštění pomalu, takže Začátečník úrovně webové vývojové prostředí je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="b53bf-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="b53bf-113">Aplikace, kterou jsme vám sestavení je jednoduché music store.</span><span class="sxs-lookup"><span data-stu-id="b53bf-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="b53bf-114">Existují tři hlavní části aplikace: nákupní, registrace a správa.</span><span class="sxs-lookup"><span data-stu-id="b53bf-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="b53bf-115">Návštěvníci můžete procházet alb podle žánru:</span><span class="sxs-lookup"><span data-stu-id="b53bf-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="b53bf-116">Mohou zobrazit jednu alba a přidat do košíku jejich:</span><span class="sxs-lookup"><span data-stu-id="b53bf-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="b53bf-117">Jejich košíku odebrat všechny položky, které už nechcete, můžete zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="b53bf-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="b53bf-118">Přistoupíte k ověření je vyzve k přihlásit nebo zaregistrovat u určitého uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="b53bf-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="b53bf-119">Po vytvoření účtu, můžete provést pořadí vyplněním dopravě a platební informace.</span><span class="sxs-lookup"><span data-stu-id="b53bf-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="b53bf-120">Abychom si to nekomplikovali, máme spuštěnou úžasné propagační akce: vše, co je zdarma po zadání propagační kód, který "FREE"!</span><span class="sxs-lookup"><span data-stu-id="b53bf-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="b53bf-121">Po změně pořadí, se zobrazí obrazovka s potvrzením jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="b53bf-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="b53bf-122">Kromě stránek faceing zákazníka budete také vytváříme správce oddílu, který se zobrazí seznam objektů alb, ze kterých můžou správci vytvářet, upravovat a odstranit alb:</span><span class="sxs-lookup"><span data-stu-id="b53bf-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="b53bf-123">1. Soubor –&gt; nový projekt</span><span class="sxs-lookup"><span data-stu-id="b53bf-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="b53bf-124">Instalace softwaru</span><span class="sxs-lookup"><span data-stu-id="b53bf-124">Installing the software</span></span>

<span data-ttu-id="b53bf-125">V tomto kurzu se začne tím, že vytvoříte nový projekt ASP.NET MVC 3 s použitím na bezplatné Visual Web Developer 2010 Express (které je zdarma) a pak postupně přidáme funkce, které chcete vytvořit kompletní funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b53bf-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="b53bf-126">Cestou se budeme zabývat přístup k databázi, scénáře účtování formuláře a ověřování dat pomocí stránek předlohy pro rozložení konzistentní stránky pomocí rozhraní AJAX pro aktualizace stránky a ověřování, přihlašování uživatelů a další.</span><span class="sxs-lookup"><span data-stu-id="b53bf-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="b53bf-127">Podrobný postup sledovat, nebo si můžete stáhnout hotovou aplikaci [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="b53bf-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="b53bf-128">Visual Studio 2010 SP1 nebo Visual Web Developer 2010 Express SP1 (bezplatná verze sady Visual Studio 2010) můžete použít k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b53bf-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="b53bf-129">Budeme používat systému SQL Server Compact (také zdarma) k hostování databáze.</span><span class="sxs-lookup"><span data-stu-id="b53bf-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="b53bf-130">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="b53bf-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="b53bf-131">[Požadavky na visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="b53bf-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="b53bf-132">[Technologie ASP.NET MVC 3 nástroje Update]</span><span class="sxs-lookup"><span data-stu-id="b53bf-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="b53bf-133">[SQL Server Compact 4.0] – včetně podpory modulu runtime a nástroje</span><span class="sxs-lookup"><span data-stu-id="b53bf-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="b53bf-134">Vytvoření nového projektu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="b53bf-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="b53bf-135">Začneme tak, že vyberete "Nový projekt" nabídka soubor v aplikaci Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="b53bf-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="b53bf-136">Tím se zobrazí dialogové okno Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="b53bf-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="b53bf-137">Vybereme Visual C# -&gt; webové šablony skupinu na levé straně, a potom v prostředním sloupci zvolte šablonu "Aplikace pro Web ASP.NET MVC 3".</span><span class="sxs-lookup"><span data-stu-id="b53bf-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="b53bf-138">Pojmenujte svůj projekt MvcMusicStore a kliknutím na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="b53bf-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="b53bf-139">Zobrazí se sekundární dialogové okno, které umožňuje provádět některé konkrétní nastavení MVC pro náš projekt.</span><span class="sxs-lookup"><span data-stu-id="b53bf-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="b53bf-140">Vyberte následující položky:</span><span class="sxs-lookup"><span data-stu-id="b53bf-140">Select the following:</span></span>

<span data-ttu-id="b53bf-141">Šablona projektu – výběr možnosti prázdná</span><span class="sxs-lookup"><span data-stu-id="b53bf-141">Project Template - select Empty</span></span>

<span data-ttu-id="b53bf-142">Zobrazit modul – Výběr Razor</span><span class="sxs-lookup"><span data-stu-id="b53bf-142">View Engine - select Razor</span></span>

<span data-ttu-id="b53bf-143">Použití sémantického značek HTML5 - checked</span><span class="sxs-lookup"><span data-stu-id="b53bf-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="b53bf-144">Zkontrolujte, že vaše nastavení se, jak je znázorněno níže, a kliknutím na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="b53bf-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="b53bf-145">Tím se vytvoří našem projektu.</span><span class="sxs-lookup"><span data-stu-id="b53bf-145">This will create our project.</span></span> <span data-ttu-id="b53bf-146">Pojďme se podívat na složky, které byly přidány do naší aplikace v Průzkumníku řešení na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="b53bf-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="b53bf-147">Není úplně prázdná šablona prázdná MVC 3 – přidá strukturu základní složky:</span><span class="sxs-lookup"><span data-stu-id="b53bf-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="b53bf-148">ASP.NET MVC používá některé základní konvence pojmenování pro názvy složek:</span><span class="sxs-lookup"><span data-stu-id="b53bf-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="b53bf-149">**Složka**</span><span class="sxs-lookup"><span data-stu-id="b53bf-149">**Folder**</span></span> | <span data-ttu-id="b53bf-150">**Účel**</span><span class="sxs-lookup"><span data-stu-id="b53bf-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="b53bf-151">**/ Řadiče**</span><span class="sxs-lookup"><span data-stu-id="b53bf-151">**/Controllers**</span></span> | <span data-ttu-id="b53bf-152">Kontrolery reagovat na vstup z prohlížeče, rozhodněte, jak ho použít a vrátit odpověď uživatele.</span><span class="sxs-lookup"><span data-stu-id="b53bf-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="b53bf-153">**/ Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="b53bf-153">**/Views**</span></span> | <span data-ttu-id="b53bf-154">Zobrazení obsahovat naše šablony uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b53bf-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="b53bf-155">**/ Modelů**</span><span class="sxs-lookup"><span data-stu-id="b53bf-155">**/Models**</span></span> | <span data-ttu-id="b53bf-156">Modely uchování a manipulaci s daty</span><span class="sxs-lookup"><span data-stu-id="b53bf-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="b53bf-157">**/ Obsahu**</span><span class="sxs-lookup"><span data-stu-id="b53bf-157">**/Content**</span></span> | <span data-ttu-id="b53bf-158">Tato složka obsahuje naše Image, šablon stylů CSS a dalšího statického obsahu</span><span class="sxs-lookup"><span data-stu-id="b53bf-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="b53bf-159">**/ Skripty**</span><span class="sxs-lookup"><span data-stu-id="b53bf-159">**/Scripts**</span></span> | <span data-ttu-id="b53bf-160">Tato složka obsahuje naši soubory jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="b53bf-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="b53bf-161">Tyto složky jsou zahrnuta i v aplikaci ASP.NET MVC prázdný, protože rozhraní ASP.NET MVC ve výchozím nastavení používá přístup "konvence nad konfigurací" a některé výchozí předpokladů podle konvence pojmenování složek.</span><span class="sxs-lookup"><span data-stu-id="b53bf-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="b53bf-162">Například řadiče hledat zobrazení v zobrazení složky ve výchozím nastavení aniž by bylo nutné explicitně určit ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="b53bf-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="b53bf-163">Nastolit pomocí výchozích konvencí snižuje množství kódu, které musíte napsat, a také usnadnit pro jiné vývojáře k pochopení vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="b53bf-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="b53bf-164">Vysvětlíme, tato konvence další, jako jsme integrovali naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b53bf-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b53bf-165">Next</span><span class="sxs-lookup"><span data-stu-id="b53bf-165">Next</span></span>](mvc-music-store-part-2.md)
