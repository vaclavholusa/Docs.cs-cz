---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: '1. část: Soubor -> Nový projekt | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 1. část obsahuje přehled a soubor/nový projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752862"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="e5ad9-104">1. část: Soubor -> Nový projekt</span><span class="sxs-lookup"><span data-stu-id="e5ad9-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="e5ad9-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e5ad9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e5ad9-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e5ad9-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e5ad9-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e5ad9-109">1. část obsahuje přehled a soubor/nový projekt.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="e5ad9-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="e5ad9-110">Overview</span></span>

<span data-ttu-id="e5ad9-111">Tento kurz je Úvod do webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="e5ad9-112">Jsme budete spuštění pomalu, takže Začátečník úrovně webové vývojové prostředí je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="e5ad9-113">Aplikace, kterou jsme vám sestavení je jednoduché online úložiště.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="e5ad9-114">Návštěvníci můžete přejít pomocí produktů podle kategorie:</span><span class="sxs-lookup"><span data-stu-id="e5ad9-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="e5ad9-115">Mohou zobrazit jednoho produktu a přidat do košíku jejich:</span><span class="sxs-lookup"><span data-stu-id="e5ad9-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="e5ad9-116">Jejich košíku odebrat všechny položky, které už nechcete, můžete zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="e5ad9-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="e5ad9-117">Přistoupíte k ověření je vyzve k</span><span class="sxs-lookup"><span data-stu-id="e5ad9-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="e5ad9-118">Po změně pořadí, se zobrazí obrazovka s potvrzením jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="e5ad9-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="e5ad9-119">Budete Začneme tím, že vytvoříte nový projekt webových formulářů ASP.NET v sadě Visual Studio 2010 a postupně přidáme funkce, které chcete vytvořit kompletní funkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="e5ad9-120">Cestou se budeme zabývat přístup k databázi, zobrazením seznamu a mřížky, stránky aktualizace dat, ověřování dat pomocí stránek předlohy pro rozložení konzistentní stránek, AJAX, ověřování, členství uživatele a další.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="e5ad9-121">Podrobný postup sledovat, nebo si můžete stáhnout hotovou aplikaci [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="e5ad9-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="e5ad9-122">Můžete použít Visual Studio 2010 nebo na bezplatné Visual Web Developer 2010 z [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="e5ad9-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="e5ad9-123">K sestavení aplikace, můžete použít SQL Server nebo bezplatný systém SQL Server Express pro hostování databáze.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="e5ad9-124">Soubor / nový projekt</span><span class="sxs-lookup"><span data-stu-id="e5ad9-124">File / New Project</span></span>

<span data-ttu-id="e5ad9-125">Začneme tak, že vyberete nový projekt v sadě Visual Studio v nabídce Soubor.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="e5ad9-126">Tím se zobrazí dialogové okno Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="e5ad9-127">Vybereme Visual C# / webové šablony skupiny na levé straně a potom v prostředním sloupci zvolte šablonu "Webovou aplikaci ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="e5ad9-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="e5ad9-128">Pojmenujte svůj projekt TailspinSpyworks a kliknutím na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="e5ad9-129">Tím se vytvoří našem projektu.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-129">This will create our project.</span></span> <span data-ttu-id="e5ad9-130">Pojďme se podívat na složky, které jsou součástí naší aplikace v Průzkumníku řešení na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="e5ad9-131">Prázdné řešení není zcela prázdný – přidá strukturu základní složky:</span><span class="sxs-lookup"><span data-stu-id="e5ad9-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="e5ad9-132">Všimněte si konvence implementované výchozí šablona projektu ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="e5ad9-133">Složce "Účet" implementuje základní uživatelské rozhraní pro ASP. Subsystém členství vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="e5ad9-134">Složce "Skripty" slouží jako úložiště pro soubory jazyka JavaScript na straně klienta a souborů .js jQuery core jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="e5ad9-135">Složce "Styly" se používá k uspořádání naše vizuální prvky webového serveru (šablony stylů CSS)</span><span class="sxs-lookup"><span data-stu-id="e5ad9-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="e5ad9-136">Při stisknutí klávesy F5 ke spuštění aplikace a vykreslování stránku default.aspx jsme v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="e5ad9-137">Vylepšení našich první aplikaci budete moct soubor Style.css z výchozí šablony webových formulářů nahradit třídy šablony stylů CSS a přidružené obrazových souborů, které budou vykreslovat visual asthetics, která chceme v naší aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="e5ad9-138">Až to uděláte naši stránku default.aspx vykreslí následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="e5ad9-139">Všimněte si, že odkazy obrázku nahoře napravo od stránce a položky nabídky, které byly přidány k hlavní stránce.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="e5ad9-140">Pouze "Sign In" a "Účet" odkazy odkazují na stránky, které existují (generované výchozí šablony) a zbytek stránky, které jsme se implementují jako jsme integrovali naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="e5ad9-141">Budeme také přemístit stránky předlohy se stránkou k adresáři styly.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="e5ad9-142">Je to jenom předvoleb však může být věcí o něco jednodušší Pokud se rozhodneme, aby naši aplikaci "skinable" v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="e5ad9-143">Po této budeme muset změnit na stránce předlohy generované odkazy do všech souborů .aspx ve výchozím nastavení stránky webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ad9-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e5ad9-144">Next</span><span class="sxs-lookup"><span data-stu-id="e5ad9-144">Next</span></span>](tailspin-spyworks-part-2.md)
