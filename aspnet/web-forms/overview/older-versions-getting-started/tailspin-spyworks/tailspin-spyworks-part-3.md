---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: '3. část: Nabídka rozložení a kategorie | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 3. část popisuje přidání rozložení a kategorie nabídky.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: f55b29a271dbdb72d3e2249ed74517b77d78cf5e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756648"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="a9b34-104">3. část: Nabídka rozložení a kategorie</span><span class="sxs-lookup"><span data-stu-id="a9b34-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="a9b34-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a9b34-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a9b34-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="a9b34-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a9b34-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="a9b34-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a9b34-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a9b34-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a9b34-109">3. část popisuje přidání rozložení a kategorie nabídky.</span><span class="sxs-lookup"><span data-stu-id="a9b34-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="a9b34-110">Přidání některých rozložení a kategorie nabídky</span><span class="sxs-lookup"><span data-stu-id="a9b34-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="a9b34-111">V našem stránku předlohy přidáme div sloupce na levé straně, který bude obsahovat naší nabídce kategorie produktu.</span><span class="sxs-lookup"><span data-stu-id="a9b34-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="a9b34-112">Všimněte si, že požadované zarovnání a další formátování budou poskytovat třídu šablony stylů CSS, který jsme přidali na náš soubor Style.css.</span><span class="sxs-lookup"><span data-stu-id="a9b34-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="a9b34-113">V nabídce kategorie produktu dynamicky vytvoří za běhu pomocí dotazu Commerce databázi pro existující kategorie produktů a vytváření položek nabídky a odpovídající odkazy.</span><span class="sxs-lookup"><span data-stu-id="a9b34-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="a9b34-114">K tomu že použijeme dvě ASP. Výkonné datové ovládací prvky vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="a9b34-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="a9b34-115">Ovládací prvek "Entity Data Source" a "ListView" ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="a9b34-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="a9b34-116">Umožňuje přepnout na "Návrhové zobrazení" a použijte pomocníky ke konfiguraci naše ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="a9b34-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="a9b34-117">Pojďme nastavit vlastnost EntityDataSource ID EDS\_kategorie\_nabídky a klikněte na zdroj dat"konfigurace".</span><span class="sxs-lookup"><span data-stu-id="a9b34-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="a9b34-118">Vyberte CommerceEntities připojení, který byl vytvořen pro nás, když jsme vytvořili Model Entity zdroje dat pro naše obchodní databáze a klikněte na tlačítko "Následující".</span><span class="sxs-lookup"><span data-stu-id="a9b34-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="a9b34-119">Vyberte název sady entit "Kategorie" a ponechte zbývající možnosti jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="a9b34-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="a9b34-120">Klikněte na tlačítko "Dokončit".</span><span class="sxs-lookup"><span data-stu-id="a9b34-120">Click "Finish".</span></span>

<span data-ttu-id="a9b34-121">Nyní Pojďme nastavit vlastnost ID instance ovládacího prvku ListView, který se nám na naší stránce ListView\_ProductsMenu a aktivovat její pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="a9b34-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="a9b34-122">I když možnosti ovládání bychom mohli použít k formátování položky zobrazení dat a formátování, vytváří nabídky budete potřebovat jenom jednoduché značky tak jsme přejde kód v zobrazení zdroje.</span><span class="sxs-lookup"><span data-stu-id="a9b34-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="a9b34-123">Poznámka: příkaz "Eval": &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="a9b34-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="a9b34-124">Syntaxe ASP.NET &lt;% # %&gt; je zkrácený tvar vlastností konvence, který dává pokyn modul runtime umožňující spouštění cokoli, co je součástí a vypíše výsledky "na řádku".</span><span class="sxs-lookup"><span data-stu-id="a9b34-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="a9b34-125">Příkaz Eval("CategoryName") dává pokyn, že pro aktuální položku v vázané kolekce položek dat, načíst změnu hodnoty názvy položek Entity Model "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="a9b34-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="a9b34-126">Toto je stručné syntaxe pro velmi výkonné funkce.</span><span class="sxs-lookup"><span data-stu-id="a9b34-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="a9b34-127">Můžete spustit nyní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a9b34-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="a9b34-128">Mějte na paměti, že naše nabídky kategorie produktu se teď zobrazuje a kdy jsme při najetí myší více než jednu z položek nabídky kategorie vidíme odkaz směřuje položky nabídky na stránku, musíme ještě implementovat pojmenované ProductsList.aspx a, jsme vyvinuli řetězcový argument dynamický dotaz, který obsahuje  id kategorie.</span><span class="sxs-lookup"><span data-stu-id="a9b34-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9b34-129">[Předchozí](tailspin-spyworks-part-2.md)
> [další](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a9b34-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
