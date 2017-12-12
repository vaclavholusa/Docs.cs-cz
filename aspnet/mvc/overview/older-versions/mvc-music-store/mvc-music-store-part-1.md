---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: "Část 1: Přehled a soubor -> Nový projekt | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 1 obsahuje přehled a soubor -> Nový projekt."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1e3373a21c7d1766cfad390a7ba68b1363d8d895
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="dd47d-104">Část 1: Přehled a soubor -> Nový projekt</span><span class="sxs-lookup"><span data-stu-id="dd47d-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="dd47d-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="dd47d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="dd47d-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="dd47d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="dd47d-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="dd47d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="dd47d-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd47d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="dd47d-109">Část 1 obsahuje přehled a soubor -&gt;nový projekt.</span><span class="sxs-lookup"><span data-stu-id="dd47d-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="dd47d-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="dd47d-110">Overview</span></span>

<span data-ttu-id="dd47d-111">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje podrobné postupy pro vývoj webů používat rozhraní ASP.NET MVC a aplikace Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="dd47d-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="dd47d-112">Jsme budete od pomalu, takže vývoj webů úrovni začátečník prostředí je to v pořádku.</span><span class="sxs-lookup"><span data-stu-id="dd47d-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="dd47d-113">Aplikace, kterou jsme budete sestavení je jednoduchý Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd47d-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="dd47d-114">Existují tři hlavní části aplikace: nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="dd47d-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="dd47d-115">Návštěvníky můžete procházet alba Genre:</span><span class="sxs-lookup"><span data-stu-id="dd47d-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="dd47d-116">Mohou zobrazovat jednoho alba a přidat jej do jejich košíku:</span><span class="sxs-lookup"><span data-stu-id="dd47d-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="dd47d-117">Jejich můžete zkontrolovat jejich košíku odebrání všech položek, které už mají:</span><span class="sxs-lookup"><span data-stu-id="dd47d-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="dd47d-118">Přistoupíte k ověření se výzva k přihlášení nebo registraci pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="dd47d-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="dd47d-119">Po vytvoření účtu, mohou být provedeny pořadí tak, že vyplníte informace o přesouvání a platba.</span><span class="sxs-lookup"><span data-stu-id="dd47d-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="dd47d-120">Pro zjednodušení jsme používáte úžasné povýšení: vše je zdarma, pokud se kód povýšení "FREE"!</span><span class="sxs-lookup"><span data-stu-id="dd47d-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="dd47d-121">Po změně pořadí, uvidí jednoduché potvrzovací obrazovce:</span><span class="sxs-lookup"><span data-stu-id="dd47d-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="dd47d-122">Kromě stránek faceing zákazníka jsme také sestavíte oddílu správce, který obsahuje seznam alb, ze kterých mohou správci vytvořit, upravit a odstranit alb:</span><span class="sxs-lookup"><span data-stu-id="dd47d-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="dd47d-123">1. Soubor -&gt; nový projekt</span><span class="sxs-lookup"><span data-stu-id="dd47d-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="dd47d-124">Instalace softwaru</span><span class="sxs-lookup"><span data-stu-id="dd47d-124">Installing the software</span></span>

<span data-ttu-id="dd47d-125">V tomto kurzu začneme vytvořením nového projektu ASP.NET MVC 3 pomocí volné Visual Web Developer 2010 Express (což je bezplatná) a pak postupně přidáme funkce k vytvoření kompletní funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dd47d-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="dd47d-126">Na této cestě jsme zaměříme přístup k databázi, scénáře příspěvků formuláře, ověřování dat pomocí stránky předlohy pro rozložení konzistentní stránky, pomocí rozhraní AJAX pro aktualizace stránky a ověřování, přihlášení uživatele a další.</span><span class="sxs-lookup"><span data-stu-id="dd47d-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="dd47d-127">Můžete absolvovat krok za krokem, nebo si můžete stáhnout hotová aplikace z [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="dd47d-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="dd47d-128">Visual Studio 2010 SP1 nebo Visual Web Developer 2010 Express SP1 (bezplatné verze sady Visual Studio 2010) slouží k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd47d-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="dd47d-129">Budeme používat systému SQL Server Compact (také zdarma) k hostování databáze.</span><span class="sxs-lookup"><span data-stu-id="dd47d-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="dd47d-130">Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="dd47d-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="dd47d-131">[Visual Studio Web Developer Express SP1 požadavky]</span><span class="sxs-lookup"><span data-stu-id="dd47d-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="dd47d-132">[Aktualizace nástrojů rozhraní ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="dd47d-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="dd47d-133">[SQL Server Compact 4.0] - včetně modul runtime a nástroje podpory</span><span class="sxs-lookup"><span data-stu-id="dd47d-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="dd47d-134">Vytvoření nového projektu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="dd47d-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="dd47d-135">Začneme výběrem "Nový projekt" v nabídce Soubor v aplikaci Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="dd47d-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="dd47d-136">Otevře dialogové okno Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="dd47d-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="dd47d-137">Jsme vyberte Visual C# -&gt; webové šablony skupiny na levé straně, a potom vyberte šablonu "ASP.NET MVC 3 webové aplikace" v prostředním sloupci.</span><span class="sxs-lookup"><span data-stu-id="dd47d-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="dd47d-138">Název projektu MvcMusicStore a klepněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="dd47d-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="dd47d-139">Tato akce zobrazí sekundární dialog, který umožňuje, aby některých specifických nastavení MVC pro naše projekt.</span><span class="sxs-lookup"><span data-stu-id="dd47d-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="dd47d-140">Vyberte následující položky:</span><span class="sxs-lookup"><span data-stu-id="dd47d-140">Select the following:</span></span>

<span data-ttu-id="dd47d-141">Šablona projektu – vyberte prázdný</span><span class="sxs-lookup"><span data-stu-id="dd47d-141">Project Template - select Empty</span></span>

<span data-ttu-id="dd47d-142">Zobrazit modul – vyberte Razor</span><span class="sxs-lookup"><span data-stu-id="dd47d-142">View Engine - select Razor</span></span>

<span data-ttu-id="dd47d-143">Použití sémantického kód HTML5 - zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="dd47d-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="dd47d-144">Ověřte, zda jsou následující nastavení, poté na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="dd47d-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="dd47d-145">Tím se vytvoří naše projektu.</span><span class="sxs-lookup"><span data-stu-id="dd47d-145">This will create our project.</span></span> <span data-ttu-id="dd47d-146">Podívejme se na složky, které jsou přidané do naší aplikaci v Průzkumníku řešení na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="dd47d-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="dd47d-147">Není úplně prázdné šablonu prázdný MVC 3 – přidá do základní složky struktury:</span><span class="sxs-lookup"><span data-stu-id="dd47d-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="dd47d-148">ASP.NET MVC využívá některé základní zásady vytváření názvů pro názvy složek:</span><span class="sxs-lookup"><span data-stu-id="dd47d-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="dd47d-149">**Složka**</span><span class="sxs-lookup"><span data-stu-id="dd47d-149">**Folder**</span></span> | <span data-ttu-id="dd47d-150">**Účel**</span><span class="sxs-lookup"><span data-stu-id="dd47d-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="dd47d-151">**/ Řadiče**</span><span class="sxs-lookup"><span data-stu-id="dd47d-151">**/Controllers**</span></span> | <span data-ttu-id="dd47d-152">Řadiče reagovat na vstup z prohlížeče, rozhodněte, jak ho použít a vrátí odpověď uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd47d-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="dd47d-153">**Nebo zobrazení.**</span><span class="sxs-lookup"><span data-stu-id="dd47d-153">**/Views**</span></span> | <span data-ttu-id="dd47d-154">Zobrazení uložení naše šablony uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="dd47d-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="dd47d-155">**/ Modely**</span><span class="sxs-lookup"><span data-stu-id="dd47d-155">**/Models**</span></span> | <span data-ttu-id="dd47d-156">Modely uložení a pracovat s daty</span><span class="sxs-lookup"><span data-stu-id="dd47d-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="dd47d-157">**A obsah**</span><span class="sxs-lookup"><span data-stu-id="dd47d-157">**/Content**</span></span> | <span data-ttu-id="dd47d-158">Tato složka obsahuje naše obrázky, CSS a dalšího statického obsahu</span><span class="sxs-lookup"><span data-stu-id="dd47d-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="dd47d-159">**A skriptů**</span><span class="sxs-lookup"><span data-stu-id="dd47d-159">**/Scripts**</span></span> | <span data-ttu-id="dd47d-160">Tato složka obsahuje soubory naše JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd47d-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="dd47d-161">Tyto složky jsou zahrnuty i v aplikaci ASP.NET MVC prázdný, protože rozhraní ASP.NET MVC ve výchozím nastavení používá přístup "konvence přes konfigurace" a některé výchozí předpokladů podle konvence pojmenování složek.</span><span class="sxs-lookup"><span data-stu-id="dd47d-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="dd47d-162">Pro instanci řadiče vyhledejte zobrazení ve složce zobrazení ve výchozím nastavení bez nutnosti explicitně určovat to ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="dd47d-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="dd47d-163">Provedením vykrvovacího vpichu s výchozích konvencí snižuje množství kód, který potřebujete k zápisu, a můžete také usnadňují jinými vývojáři pochopit projektu.</span><span class="sxs-lookup"><span data-stu-id="dd47d-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="dd47d-164">Vysvětlíme tyto konvence další, jak jsme sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd47d-164">We'll explain these conventions more as we build our application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="dd47d-165">Další</span><span class="sxs-lookup"><span data-stu-id="dd47d-165">Next</span></span>](mvc-music-store-part-2.md)
