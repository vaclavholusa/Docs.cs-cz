---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Vytvoření řadiče (VB) | Microsoft Docs
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak přidat řadič pro aplikaci ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="cdf96-103">Vytvoření řadiče (VB)</span><span class="sxs-lookup"><span data-stu-id="cdf96-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="cdf96-104">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="cdf96-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="cdf96-105">V tomto kurzu Stephen Walther ukazuje, jak přidat řadič pro aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cdf96-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="cdf96-106">Cílem tohoto kurzu je vysvětlují, jak vytvořit nové rozhraní ASP.NET MVC řadiče.</span><span class="sxs-lookup"><span data-stu-id="cdf96-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="cdf96-107">Zjistíte, jak vytvořit řadiče pomocí příkazu přidat kontroler Visual Studio i tak, že ručně vytvoříte soubor třídy.</span><span class="sxs-lookup"><span data-stu-id="cdf96-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="cdf96-108">Pomocí přidejte řadiče nabídky</span><span class="sxs-lookup"><span data-stu-id="cdf96-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="cdf96-109">Nejjednodušší způsob, jak vytvořit nový řadič je složce řadiče v okně Průzkumníka řešení Visual Studio klikněte pravým tlačítkem a vyberte **přidat, řadiče** nabídky (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="cdf96-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="cdf96-110">Výběrem této možnosti nabídky otevře **přidat kontroler** dialogové okno (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="cdf96-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="cdf96-111">[![Dialogové okno Nový projekt](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cdf96-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="cdf96-112">**Obrázek 01**: přidávání nového řadiče ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="cdf96-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="cdf96-113">[![Dialogové okno Nový projekt](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cdf96-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="cdf96-114">**Obrázek 02**: dialogové okno Přidat kontroler ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="cdf96-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="cdf96-115">Všimněte si, že první část názvu kontroleru je označený na **přidat kontroler** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cdf96-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="cdf96-116">Každý název kontroleru musí končit příponou *řadič*.</span><span class="sxs-lookup"><span data-stu-id="cdf96-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="cdf96-117">Například můžete vytvořit řadič s názvem *ProductController* , ale není řadič s názvem *produktu*.</span><span class="sxs-lookup"><span data-stu-id="cdf96-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="cdf96-118">Pokud vytvoříte kontroler, který nebyl nalezen *řadič* přípona pak nebude možné k vyvolání kontroleru.</span><span class="sxs-lookup"><span data-stu-id="cdf96-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="cdf96-119">Není to – I jste ke znehodnocení části obrovském množství hodin život po provedení této chybě.</span><span class="sxs-lookup"><span data-stu-id="cdf96-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="cdf96-120">**Výpis 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="cdf96-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="cdf96-121">Vždy byste měli vytvořit řadiče ve složce řadiče.</span><span class="sxs-lookup"><span data-stu-id="cdf96-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="cdf96-122">Jinak budete narušení konvence rozhraní ASP.NET MVC a jinými vývojáři bude mít obtížnější čas pochopení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdf96-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="cdf96-123">Metody akce generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="cdf96-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="cdf96-124">Když vytvoříte řadič, máte možnost automaticky vygenerovat vytvoření, aktualizace a informace metody akce (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="cdf96-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="cdf96-125">Pokud vyberete tuto možnost se vygeneruje třídy kontroleru v výpis 2.</span><span class="sxs-lookup"><span data-stu-id="cdf96-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="cdf96-126">[![Automatické vytváření metody akce](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="cdf96-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="cdf96-127">**Obrázek 03**: vytvoření metody akce automaticky ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="cdf96-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="cdf96-128">**Výpis 2 - Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="cdf96-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="cdf96-129">Tyto metody generované jsou se zakázaným inzerováním metody.</span><span class="sxs-lookup"><span data-stu-id="cdf96-129">These generated methods are stub methods.</span></span> <span data-ttu-id="cdf96-130">Je nutné přidat skutečné logiku pro vytváření, aktualizaci a zobrazení podrobností pro zákazníka sami.</span><span class="sxs-lookup"><span data-stu-id="cdf96-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="cdf96-131">Ale se zakázaným inzerováním metody poskytují dobrý výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="cdf96-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="cdf96-132">Vytvoření třídy Kontroleru</span><span class="sxs-lookup"><span data-stu-id="cdf96-132">Creating a Controller Class</span></span>

<span data-ttu-id="cdf96-133">Kontroler ASP.NET MVC je právě třída.</span><span class="sxs-lookup"><span data-stu-id="cdf96-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="cdf96-134">Pokud dáváte přednost, můžete ignorovat generování uživatelského rozhraní pohodlný Visual Studio řadiče a ručně vytvořit třídu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="cdf96-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="cdf96-135">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="cdf96-135">Follow these steps:</span></span>

1. <span data-ttu-id="cdf96-136">Klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, nové položky** a vyberte **třída** šablony (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="cdf96-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="cdf96-137">Pojmenujte novou třídu PersonController.vb a klikněte na **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cdf96-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="cdf96-138">Výsledný soubor třída upravte tak, aby třídy dědí vlastnosti ze základní třídy System.Web.Mvc.Controller (viz seznam 3).</span><span class="sxs-lookup"><span data-stu-id="cdf96-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="cdf96-139">[![Vytvoření nové třídy](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cdf96-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="cdf96-140">**Obrázek 04**: vytvoření nové třídy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="cdf96-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="cdf96-141">**Výpis 3 - Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="cdf96-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="cdf96-142">Řadič v výpis 3 zpřístupní jednu akci s názvem Index(), která vrací řetězec "Hello, World!".</span><span class="sxs-lookup"><span data-stu-id="cdf96-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="cdf96-143">Tato akce kontroleru můžete vyvolat spuštěním aplikace a vyžaduje adresu URL podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="cdf96-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="cdf96-144">Vývojový Server ASP.NET používá číslo portu náhodné (například 40071).</span><span class="sxs-lookup"><span data-stu-id="cdf96-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="cdf96-145">Při zadávání adresy URL pro vyvolání řadič, budete muset zadat číslo pravý port.</span><span class="sxs-lookup"><span data-stu-id="cdf96-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="cdf96-146">Ukázáním myší na ikonu pro vývojový Server ASP.NET v oznamovací oblasti systému Windows (pravém dolním rohu obrazovky) můžete zjistit číslo portu.</span><span class="sxs-lookup"><span data-stu-id="cdf96-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="cdf96-147">[Předchozí](adding-dynamic-content-to-a-cached-page-vb.md)
> [další](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cdf96-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
