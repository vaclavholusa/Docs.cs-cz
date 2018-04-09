---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Část 4: Výpis produkty | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 4 obsahuje výpis produkty s sml GridView...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a><span data-ttu-id="39fa8-104">Část 4: Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="39fa8-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="39fa8-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="39fa8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="39fa8-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="39fa8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="39fa8-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="39fa8-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="39fa8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="39fa8-109">Část 4 obsahuje výpis produkty pomocí ovládacího prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="39fa8-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="39fa8-110">Výpis produkty pomocí ovládacího prvku GridView</span><span class="sxs-lookup"><span data-stu-id="39fa8-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="39fa8-111">Začněme implementace naší stránce s ProductsList.aspx kliknutím pravým tlačítkem"" na naše řešení a výběr "Přidat" a "Nová položka".</span><span class="sxs-lookup"><span data-stu-id="39fa8-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="39fa8-112">Zvolte "Webové formuláře pomocí – stránka předlohy" a zadejte název stránky ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="39fa8-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="39fa8-113">Klikněte na tlačítko "Přidat".</span><span class="sxs-lookup"><span data-stu-id="39fa8-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="39fa8-114">Dále vyberte "Styly" složku, kam jsme umístit stránku Site.Master a vyberte ho v okně "Obsah složky".</span><span class="sxs-lookup"><span data-stu-id="39fa8-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="39fa8-115">Klikněte na tlačítko "Ok" k vytvoření stránky.</span><span class="sxs-lookup"><span data-stu-id="39fa8-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="39fa8-116">Databáze je naplněný daty produktu, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="39fa8-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="39fa8-117">Po vytvoření naši stránku znovu použijeme k zdroji dat Entity přístup k těmto datům produktu, ale v této instanci, je potřeba vyberte entity produktu a je potřeba omezit položky, které se vrátí jenom ty pro vybranou kategorii.</span><span class="sxs-lookup"><span data-stu-id="39fa8-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="39fa8-118">K tomu můžeme EntityDataSource pro automatické generování informujeme klauzule WHERE a určíme budete WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="39fa8-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="39fa8-119">Budete si Vzpomínáte, že pokud jsme vytvořili položek nabídky v našem "kategorie nabídky produktů" jsme dynamicky vytvořili odkaz tak, že přidáte do řetězce dotazu pro každý odkaz CatagoryID.</span><span class="sxs-lookup"><span data-stu-id="39fa8-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="39fa8-120">Budete informováni Entity zdroje dat pro parametr kde odvozena z tohoto parametru řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="39fa8-121">V dalším kroku nakonfigurujeme ovládacího prvku ListView zobrazíte seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="39fa8-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="39fa8-122">Chcete-li vytvořit nákupní optimálních jsme budete na každé jednotlivé produkt zobrazí v našem ListVew compact několik stručným funkcí.</span><span class="sxs-lookup"><span data-stu-id="39fa8-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="39fa8-123">Název produktu, bude odkaz na zobrazení podrobností produktu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="39fa8-124">Zobrazí se ceny produktu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="39fa8-125">Zobrazí se obrázek produktu a jsme dynamicky vyberte bitovou kopii z adresáře, bitové kopie katalogu v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="39fa8-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="39fa8-126">Jsme bude obsahovat odkaz na okamžitě přidat konkrétní produkt nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="39fa8-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="39fa8-127">Zde je kód pro naše instance ovládacího prvku ListView.</span><span class="sxs-lookup"><span data-stu-id="39fa8-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="39fa8-128">Dynamicky vytváříte několik odkazů pro jednotlivé zobrazené produkty.</span><span class="sxs-lookup"><span data-stu-id="39fa8-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="39fa8-129">Také jsme otestovat vlastní nová stránka musíme vytvořit strukturu adresáře pro produkt obrázky katalogu takto.</span><span class="sxs-lookup"><span data-stu-id="39fa8-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="39fa8-130">Jakmile jsou přístupné obrázky našich produktů, abychom mohli otestovat naše stránky produktu seznamu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="39fa8-131">Z domovské stránky daného webu klikněte na jeden z odkazů seznamu kategorie.</span><span class="sxs-lookup"><span data-stu-id="39fa8-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="39fa8-132">Teď musíme implementovat stránce ProductDetials.apsx a AddToCart funkce.</span><span class="sxs-lookup"><span data-stu-id="39fa8-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="39fa8-133">Pomocí souboru -&gt;nová vytvořit název stránky ProductDetails.aspx pomocí webu – stránka předlohy, jako jsme to udělali dřív.</span><span class="sxs-lookup"><span data-stu-id="39fa8-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="39fa8-134">Ovládací prvek EntityDataSource budeme znovu používat pro přístup k určitým produktem záznam v databázi a budeme používat ovládací prvek ASP.NET FormView zobrazíte data produktu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="39fa8-135">Nebojte se, pokud formátování, které vypadá trochu věc pro vás.</span><span class="sxs-lookup"><span data-stu-id="39fa8-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="39fa8-136">Kód výše ponechá místa v zobrazení rozložení pro několik funkcí, které budete později na implementaci.</span><span class="sxs-lookup"><span data-stu-id="39fa8-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="39fa8-137">Nákupní košík bude reprezentovat složitější logiku v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="39fa8-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="39fa8-138">Chcete-li začít, použijte soubor -&gt;nová vytvořit stránku s názvem MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="39fa8-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="39fa8-139">Všimněte si, že jsme nejsou volba názvu ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="39fa8-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="39fa8-140">Naše databáze obsahuje tabulku s názvem "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="39fa8-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="39fa8-141">Když jsme datového modelu Entity třídu byl vytvořen pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="39fa8-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="39fa8-142">Proto datového modelu Entity vygeneruje třídu Entity s názvem "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="39fa8-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="39fa8-143">Model jsme může upravit tak, aby jsme může použijte tento název pro naše nákupní košík implementace nebo rozšířit pro naše potřeby, ale jsme se místo toho opt jednoduše vyberte název, který bude nedošlo ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="39fa8-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="39fa8-144">Je také vhodné poznamenat, že budeme vytvářet jednoduché nákupní košík a vkládání logice nákupní košík s hodnotou display nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="39fa8-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="39fa8-145">Také jsme možné implementovat naše řešení nákupního košíku zcela oddělenou obchodní vrstvy.</span><span class="sxs-lookup"><span data-stu-id="39fa8-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="39fa8-146">[Předchozí](tailspin-spyworks-part-3.md)
> [další](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="39fa8-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
