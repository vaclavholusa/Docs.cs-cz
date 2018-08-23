---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: '4. část: Seznam produktů | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 4. část obsahuje seznam produktů s GridView sml...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756920"
---
<a name="part-4-listing-products"></a><span data-ttu-id="e11c1-104">4. část: Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="e11c1-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="e11c1-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e11c1-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e11c1-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="e11c1-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e11c1-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="e11c1-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e11c1-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e11c1-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e11c1-109">4. část obsahuje seznam produktů pomocí ovládacího prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="e11c1-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="e11c1-110">Seznam produktů pomocí ovládacího prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="e11c1-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="e11c1-111">Začněme implementace naši stránku ProductsList.aspx "Klikněte pravým tlačítkem myši klikněte na" na naše řešení a výběrem možnosti "Add" a "Nová položka".</span><span class="sxs-lookup"><span data-stu-id="e11c1-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="e11c1-112">Zvolte možnost "Webové formuláře pomocí – stránka předlohy" a zadejte název stránky ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="e11c1-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="e11c1-113">Klikněte na tlačítko "Přidat".</span><span class="sxs-lookup"><span data-stu-id="e11c1-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="e11c1-114">Dále vyberte složku "Styly", které jsme umístili Site.Master stránky a vyberte ho z okna "Obsah složky".</span><span class="sxs-lookup"><span data-stu-id="e11c1-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="e11c1-115">Klikněte na tlačítko "Ok" k vytvoření stránky.</span><span class="sxs-lookup"><span data-stu-id="e11c1-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="e11c1-116">Databáze je naplněný daty produktu, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="e11c1-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="e11c1-117">Po vytvoření naši stránku znovu použijeme Entity Data Source přistupovat k těmto datům produktu, ale v tomto případě potřebujeme vyberte entity, produktu a My potřebujeme k omezení položky, které jsou vráceny pouze na ty pro vybranou kategorii.</span><span class="sxs-lookup"><span data-stu-id="e11c1-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="e11c1-118">K tomu budete tom EntityDataSource automaticky generovat klauzuli WHERE a zadáme budete WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="e11c1-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="e11c1-119">Budete si možná Vzpomínáte, že když jsme vytvořili položky nabídky v našich "kategorie nabídky produktů" dynamicky sestavili jsme odkaz tak, že přidáte CatagoryID řetězec dotazu pro každý odkaz.</span><span class="sxs-lookup"><span data-stu-id="e11c1-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="e11c1-120">Budete informováni Entity zdroje dat se odvodit parametr WHERE z parametru řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e11c1-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="e11c1-121">V dalším kroku nakonfigurujeme ovládací prvek ListView pro zobrazení seznamu produktů.</span><span class="sxs-lookup"><span data-stu-id="e11c1-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="e11c1-122">Chcete-li vytvořit optimální prostředí pro nákup jsme vám několik funkcí stručné komprimace zobrazují v našem ListVew jednotlivé produkty.</span><span class="sxs-lookup"><span data-stu-id="e11c1-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="e11c1-123">Název produktu, bude obsahovat odkaz k zobrazení podrobností produktu.</span><span class="sxs-lookup"><span data-stu-id="e11c1-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="e11c1-124">Ceny produktu se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e11c1-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="e11c1-125">Zobrazí bitovou kopii produktu a dynamicky vybereme image z Image adresáře katalogu v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e11c1-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="e11c1-126">Společnost Microsoft bude obsahovat odkaz okamžitě přidat konkrétního produktu do nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="e11c1-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="e11c1-127">Toto je zápis pro naše instance ovládacího prvku ListView.</span><span class="sxs-lookup"><span data-stu-id="e11c1-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="e11c1-128">Dynamicky vytváříme několik odkazů pro každý zobrazený produkt.</span><span class="sxs-lookup"><span data-stu-id="e11c1-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="e11c1-129">Také před testujeme vlastní nová stránka potřebujeme vytvořit strukturu adresářů pro produkt obrázky katalogu následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e11c1-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="e11c1-130">Jakmile naše Image produktu jsou přístupné můžeme otestovat naše stránky produktu seznamu.</span><span class="sxs-lookup"><span data-stu-id="e11c1-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="e11c1-131">Na domovské stránce webu klikněte na jeden z odkazů seznamu kategorií.</span><span class="sxs-lookup"><span data-stu-id="e11c1-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="e11c1-132">Teď musíme implementovat ProductDetials.apsx stránky a funkce AddToCart.</span><span class="sxs-lookup"><span data-stu-id="e11c1-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="e11c1-133">Použijte soubor -&gt;nový název stránky ProductDetails.aspx pomocí web stránku předlohy, jako jsme to udělali dříve.</span><span class="sxs-lookup"><span data-stu-id="e11c1-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="e11c1-134">Znovu použijeme ovládací prvek třídu EntityDataSource platí pro přístup k určitým produktem záznam v databázi a zobrazení dat produktu takto použijeme ovládacího prvku FormView technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e11c1-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="e11c1-135">Nedělejte si starosti, pokud formátování vypadá trochu... pro vás.</span><span class="sxs-lookup"><span data-stu-id="e11c1-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="e11c1-136">Výše uvedené značky ponechá místa v zobrazení rozložení pro několik funkcí, které budete později na implementaci.</span><span class="sxs-lookup"><span data-stu-id="e11c1-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="e11c1-137">Nákupního košíku se reprezentují složitější logiku v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e11c1-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="e11c1-138">Abyste mohli začít, použijte soubor -&gt;nová vytvořit stránku s názvem MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="e11c1-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="e11c1-139">Všimněte si, že jsme nejsou volba názvu ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="e11c1-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="e11c1-140">Naše databáze obsahuje tabulku s názvem "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="e11c1-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="e11c1-141">Jsme generování modelu Entity Data Model třídu byla vytvořena pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="e11c1-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="e11c1-142">Proto modelu Entity Data Model vygeneruje třídu Entity s názvem "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="e11c1-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="e11c1-143">Model jsme může upravit tak, že jsme mohli použít tento název pro naše implementace nákupního košíku nebo ji rozšířit pro naše požadavky na, ale jsme se místo toho vyjádřit souhlas se službou jednoduše vyberte název, který bude nedošlo ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="e11c1-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="e11c1-144">Je také vhodné poznamenat, že budeme vytvářet jednoduché nákupního košíku a vkládání nákupního košíku logiky pomocí zobrazení nákupního košíku.</span><span class="sxs-lookup"><span data-stu-id="e11c1-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="e11c1-145">Také jsme rozhodnout k implementaci naše řešení nákupního košíku ve zcela samostatném obchodní vrstvě.</span><span class="sxs-lookup"><span data-stu-id="e11c1-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e11c1-146">[Předchozí](tailspin-spyworks-part-3.md)
> [další](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="e11c1-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
