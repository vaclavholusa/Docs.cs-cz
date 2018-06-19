---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Část 1: Soubor -> Nový projekt | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 1 obsahuje přehled a soubor nebo nový projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892462"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="9de10-104">Část 1: Soubor -> Nový projekt</span><span class="sxs-lookup"><span data-stu-id="9de10-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="9de10-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9de10-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9de10-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="9de10-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9de10-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="9de10-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9de10-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9de10-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9de10-109">Část 1 obsahuje přehled a soubor nebo nový projekt.</span><span class="sxs-lookup"><span data-stu-id="9de10-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="9de10-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="9de10-110">Overview</span></span>

<span data-ttu-id="9de10-111">Tento kurz je určen Úvod do webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9de10-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="9de10-112">Jsme budete od pomalu, takže vývoj webů úrovni začátečník prostředí je to v pořádku.</span><span class="sxs-lookup"><span data-stu-id="9de10-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="9de10-113">Aplikace, kterou jsme budete sestavení je jednoduchý online úložiště.</span><span class="sxs-lookup"><span data-stu-id="9de10-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="9de10-114">Návštěvníky můžete vyhledat produkty podle kategorie:</span><span class="sxs-lookup"><span data-stu-id="9de10-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="9de10-115">Mohou zobrazovat jednoho produktu a přidat jej do jejich košíku:</span><span class="sxs-lookup"><span data-stu-id="9de10-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="9de10-116">Jejich můžete zkontrolovat jejich košíku odebrání všech položek, které už mají:</span><span class="sxs-lookup"><span data-stu-id="9de10-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="9de10-117">Přistoupíte k ověření se výzva k</span><span class="sxs-lookup"><span data-stu-id="9de10-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="9de10-118">Po změně pořadí, uvidí jednoduché potvrzovací obrazovce:</span><span class="sxs-lookup"><span data-stu-id="9de10-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="9de10-119">Budete začneme vytvořením nového projektu ASP.NET WebForms v sadě Visual Studio 2010 a přidáme přírůstkově funkce k vytvoření kompletní funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9de10-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="9de10-120">Na této cestě jsme zaměříme přístup k databázi, zobrazení seznamu a mřížky, stránky aktualizace dat, ověření dat pomocí stránky předlohy pro rozložení konzistentní stránky, AJAX, ověření, členství uživatele a další.</span><span class="sxs-lookup"><span data-stu-id="9de10-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="9de10-121">Můžete absolvovat krok za krokem, nebo si můžete stáhnout z hotová aplikace [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="9de10-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="9de10-122">Můžete použít Visual Studio 2010 nebo bezplatné Visual Web Developer 2010 z [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="9de10-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="9de10-123">Jestli Pokud chcete vytvářet aplikace, můžete použít SQL Server nebo volné SQL Server Express na hostitele databáze.</span><span class="sxs-lookup"><span data-stu-id="9de10-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="9de10-124">Soubor nový projekt</span><span class="sxs-lookup"><span data-stu-id="9de10-124">File / New Project</span></span>

<span data-ttu-id="9de10-125">Začneme výběrem nový projekt v sadě Visual Studio v nabídce Soubor.</span><span class="sxs-lookup"><span data-stu-id="9de10-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="9de10-126">Otevře dialogové okno Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="9de10-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="9de10-127">Jsme vyberte Visual C# / skupiny na levé straně webové šablony a potom vyberte šablonu "Webové aplikace ASP.NET" v prostředním sloupci.</span><span class="sxs-lookup"><span data-stu-id="9de10-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="9de10-128">Název projektu TailspinSpyworks a klepněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="9de10-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="9de10-129">Tím se vytvoří naše projektu.</span><span class="sxs-lookup"><span data-stu-id="9de10-129">This will create our project.</span></span> <span data-ttu-id="9de10-130">Podívejme se na složky, které jsou zahrnuty v naší aplikaci v Průzkumníku řešení na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="9de10-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="9de10-131">Prázdný řešení není úplně prázdný – přidá do základní složky struktury:</span><span class="sxs-lookup"><span data-stu-id="9de10-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="9de10-132">Poznámka: konvence implementované výchozí šablona projektu ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="9de10-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="9de10-133">Složce "Účet" implementuje základní uživatelské rozhraní pro ASP. Subsystém pro NET členství.</span><span class="sxs-lookup"><span data-stu-id="9de10-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="9de10-134">Složce "Skripty" slouží jako úložiště pro klientské soubory JavaScript a soubory .js jQuery jádra, která jsou k dispozici ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9de10-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="9de10-135">Složce "Styly" slouží k uspořádání naše webu vizuály (šablony stylů CSS)</span><span class="sxs-lookup"><span data-stu-id="9de10-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="9de10-136">Když jsme stisknutím klávesy F5 spusťte aplikaci a vykreslení stránky default.aspx jsme v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9de10-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="9de10-137">Naše první vylepšení aplikace bude nahradit soubor Style.css z výchozí šablony webových formulářů s tříd CSS a přidruženou bitovou kopii souborů, které budou vykreslovat visual asthetics, který má být pro naši aplikaci Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9de10-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="9de10-138">Až to uděláte naší stránce s default.aspx vykreslí následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="9de10-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="9de10-139">Všimněte si odkazy bitové kopie v horní pravé části stránky a položky nabídky, které byly přidány k hlavní stránce.</span><span class="sxs-lookup"><span data-stu-id="9de10-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="9de10-140">Pouze odkazy "Přihlášení" a "Účet" bodu stránek, které existují (generované výchozí šablony) a zbytek stránky, které bude implementaci jako jsme sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9de10-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="9de10-141">Vytvoříme také přemístit na adresáři stylů stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="9de10-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="9de10-142">I když toto je pouze předvolbu ho může usnadnily trochu Pokud jsme rozhodnout, aby naše aplikace "skinable" v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="9de10-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="9de10-143">Po této budeme potřebovat změnit hlavní stránce odkazy v všechny soubory .aspx generována výchozí stránky webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9de10-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9de10-144">Next</span><span class="sxs-lookup"><span data-stu-id="9de10-144">Next</span></span>](tailspin-spyworks-part-2.md)
