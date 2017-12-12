---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "Část 3: Rozložení a kategorie nabídky | Microsoft Docs"
author: JoeStagner
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 3 zahrnuje přidání rozložení a kategorie nabídky."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="fd157-104">Část 3: Rozložení a kategorie nabídky</span><span class="sxs-lookup"><span data-stu-id="fd157-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="fd157-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="fd157-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="fd157-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="fd157-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="fd157-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="fd157-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="fd157-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="fd157-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="fd157-109">Část 3 zahrnuje přidání rozložení a kategorie nabídky.</span><span class="sxs-lookup"><span data-stu-id="fd157-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a><span data-ttu-id="fd157-110">Přidání některé rozložení a kategorie nabídky</span><span class="sxs-lookup"><span data-stu-id="fd157-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="fd157-111">V našem stránka přidáme div pro sloupec levé straně, který bude obsahovat našich produktů kategorie nabídky.</span><span class="sxs-lookup"><span data-stu-id="fd157-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="fd157-112">Všimněte si, že požadované zarovnání a další formátování bude poskytována třídy CSS, který jsme přidali do našich soubor Style.css.</span><span class="sxs-lookup"><span data-stu-id="fd157-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="fd157-113">V nabídce kategorie produktu dynamicky vytvoří za běhu prostřednictvím dotazů na databázi a obchodu Spojených států pro existující kategorie produktů a vytváření položek nabídky a odpovídající odkazy.</span><span class="sxs-lookup"><span data-stu-id="fd157-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="fd157-114">K tomu budeme používat dvě ASP. Ovládací prvky na NET výkonné data.</span><span class="sxs-lookup"><span data-stu-id="fd157-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="fd157-115">Prvek "Zdroje dat Entity" a "ListView" řízení.</span><span class="sxs-lookup"><span data-stu-id="fd157-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="fd157-116">Umožňuje přepnout do "Zobrazení návrhu" a použijte pomocníky ke konfiguraci naše ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="fd157-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="fd157-117">Umožňuje nastavit vlastnost EntityDataSource ID na EDS\_kategorie\_nabídky a klikněte na zdroj dat"konfigurace".</span><span class="sxs-lookup"><span data-stu-id="fd157-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="fd157-118">Vyberte CommerceEntities připojení, která byla vytvořena pro nás, když jsme vytvořili Model Entity zdroje dat pro naše obchodu Spojených států v databázi a klikněte na tlačítko "Další".</span><span class="sxs-lookup"><span data-stu-id="fd157-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="fd157-119">Vyberte název sady entit "Kategorie" a ponechejte zbývající možnosti jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="fd157-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="fd157-120">Klikněte na tlačítko "Dokončit".</span><span class="sxs-lookup"><span data-stu-id="fd157-120">Click "Finish".</span></span>

<span data-ttu-id="fd157-121">Teď umožňuje nastavit vlastnosti ID instance ovládacího prvku ListView, která nemůžeme kladena na naší stránce ListView\_ProductsMenu a aktivujte její pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="fd157-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="fd157-122">Když jsme může používat možnosti řízení k formátování položky dat zobrazení a formátování, naše vytvoření nabídky bude pouze vyžadovat jednoduchých značek tak jsme zadá kód v zobrazení zdroje.</span><span class="sxs-lookup"><span data-stu-id="fd157-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="fd157-123">Poznámka: příkaz "Eval": &lt;% Eval("CategoryName") % #&gt;</span><span class="sxs-lookup"><span data-stu-id="fd157-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="fd157-124">Syntaxe ASP.NET &lt;% # %&gt; je sdružená konvence, který se dá pokyn modulu runtime spustit ať je obsažena v a výstup výsledků "v řádku".</span><span class="sxs-lookup"><span data-stu-id="fd157-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="fd157-125">Příkaz Eval("CategoryName") dá pokyn, že aktuální položku v vázané kolekce datových položek, načtěte hodnotě názvů položka modelu Entity "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="fd157-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="fd157-126">Toto je stručný syntaxe pro velmi výkonné funkce.</span><span class="sxs-lookup"><span data-stu-id="fd157-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="fd157-127">Umožňuje spustit nyní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fd157-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="fd157-128">Všimněte si, že našich produktů kategorie nabídky se nyní zobrazí a když jsme více než jedné z položek nabídky kategorie vidíme body odkaz položky nabídky na stránku, musíme ještě implementovat najeďte pojmenovaný ProductsList.aspx a že Vyvinuli jsme argument řetězce dynamické dotazu, obsahuje  id kategorie.</span><span class="sxs-lookup"><span data-stu-id="fd157-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fd157-129">[Předchozí](tailspin-spyworks-part-2.md)
[další](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="fd157-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
