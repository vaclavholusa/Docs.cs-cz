---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Část 7: Přidávání funkcí | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 7 přidává další funkce, jako je například účet revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a><span data-ttu-id="c9d9d-104">Část 7: Přidání funkcí</span><span class="sxs-lookup"><span data-stu-id="c9d9d-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="c9d9d-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="c9d9d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="c9d9d-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="c9d9d-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="c9d9d-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="c9d9d-109">Část 7 přidává další funkce, jako je například účet kontrolní, recenzemi produktů a "Oblíbené položky" a "také zakoupených" uživatelské ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="c9d9d-110">Přidávání funkcí</span><span class="sxs-lookup"><span data-stu-id="c9d9d-110">Adding Features</span></span>

<span data-ttu-id="c9d9d-111">Když uživatelé procházet naše katalogu, umístěte položek do nákupního košíku a dokončete proces najdete v článku věnovaném, se, že počet podpora funkce, že jsme bude obsahovat vylepšit náš web neexistují.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="c9d9d-112">Zkontrolujte účet (seznam objednávky umístěn a zobrazit podrobnosti.)</span><span class="sxs-lookup"><span data-stu-id="c9d9d-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="c9d9d-113">Přidejte nějaký kontextu konkrétní obsah na stránku front.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="c9d9d-114">Přidání funkce tak, aby uživatelé zkontrolujte produktů v katalogu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="c9d9d-115">Vytvoření uživatelského ovládacího prvku zobrazení oblíbených položek a místo, které řídí na stránce front.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="c9d9d-116">Vytvoření uživatelského ovládacího prvku "Také zakoupili" a přidejte na stránku podrobností produktu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="c9d9d-117">Přidat a obraťte se na stránku.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="c9d9d-118">Přidat o stránce.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-118">Add an About Page.</span></span>
8. <span data-ttu-id="c9d9d-119">Globální chyby</span><span class="sxs-lookup"><span data-stu-id="c9d9d-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="c9d9d-120">Zkontrolujte účet</span><span class="sxs-lookup"><span data-stu-id="c9d9d-120">Account Review</span></span>

<span data-ttu-id="c9d9d-121">Ve složce "Účet" vytvořte dvě stránky .aspx jednu s názvem OrderList.aspx a dalších pojmenované OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="c9d9d-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="c9d9d-122">OrderList.aspx využije funkci GridView a EntityDataSoure ovládacích prvků, podobně jako dříve máme.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="c9d9d-123">EntityDataSoure vybere záznamy v tabulce filtrováno uživatelské jméno (viz WhereParameter), který jsme nastavena v proměnné relace, když uživateli přihlášení je.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="c9d9d-124">Všimněte si také tyto parametry v HyperlinkField prvku GridView:</span><span class="sxs-lookup"><span data-stu-id="c9d9d-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="c9d9d-125">Tyto zadejte odkaz na zobrazení podrobností pořadí pro každý produkt zadat jako parametr řetězce dotazu na stránku OrderDetails.aspx pole OrderID.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="c9d9d-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="c9d9d-126">OrderDetails.aspx</span></span>

<span data-ttu-id="c9d9d-127">Ovládací prvek EntityDataSource budeme používat pro přístup k objednávky a FormView zobrazují data pořadí a jiné EntityDataSource s GridView pro zobrazení všech pořadí položek řádku.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="c9d9d-128">V souboru kódu za (OrderDetails.aspx.cs) máme dvě málo bity housekeeping.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="c9d9d-129">Nejprve musíme Ujistěte se, že Rozpis objednávek vždy získá OrderId.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="c9d9d-130">Také potřebujeme k výpočtu a celkový počet položek řádku pořadí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="c9d9d-131">Domovská stránka</span><span class="sxs-lookup"><span data-stu-id="c9d9d-131">The Home Page</span></span>

<span data-ttu-id="c9d9d-132">Přidejme nějaký statický obsah na stránku Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="c9d9d-133">Nejprve budete vytvořit složku "Obsah" a v něm složku obrázků (a I bude obsahovat obrázek, který se použije na domovské stránce).</span><span class="sxs-lookup"><span data-stu-id="c9d9d-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="c9d9d-134">Do dolní zástupného textu stránky Default.aspx přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="c9d9d-135">Recenzemi produktů</span><span class="sxs-lookup"><span data-stu-id="c9d9d-135">Product Reviews</span></span>

<span data-ttu-id="c9d9d-136">Nejprve přidáme tlačítko s odkazem na formulář, který jsme můžete použít k zadání kontrolu produktu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="c9d9d-137">Všimněte si, že jsme předali ProductID v řetězci dotazu</span><span class="sxs-lookup"><span data-stu-id="c9d9d-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="c9d9d-138">Další přidejme stránku s názvem ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="c9d9d-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="c9d9d-139">Tato stránka bude používat sadu ovládacího prvku ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="c9d9d-140">Pokud jste již neučinili, si můžete stáhnout z [DevExpress](http://devexpress.com/act) a pokyny o nastavení sady nástrojů pro použití se sadou Visual Studio zde [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="c9d9d-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="c9d9d-141">V režimu návrhu přetáhněte ovládací prvky a validátory z panelu nástrojů a sestavení formuláře podobná té následující.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="c9d9d-142">Zápis bude vypadat přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="c9d9d-143">Teď, když jsme můžete zadat recenze, umožňuje zobrazit tyto recenze na stránce produktu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="c9d9d-144">Tento kód přidejte na stránku ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="c9d9d-145">Spuštěné aplikace teď a přejdete na produkt zobrazuje informace o produktu včetně zákaznické recenze.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="c9d9d-146">Oblíbené položky ovládacího prvku (vytvoření uživatelské ovládací prvky)</span><span class="sxs-lookup"><span data-stu-id="c9d9d-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="c9d9d-147">Chcete-li zvýšit prodej na svém webu přidáme několik funkcí do oblíbených nebo souvisejících produktů "sugestivní prodává".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="c9d9d-148">První z těchto funkcí bude seznam oblíbenější produktu v našem katalog produktů.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="c9d9d-149">Vytvoříme "Uživatelského ovládacího prvku" zobrazíte nejlépe prodávané položky na domovské stránce naše aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="c9d9d-150">Vzhledem k tomu, že to bude ovládacího prvku, jsme ji používat na libovolné stránce jednoduše přetahování a vkládání v návrháři Visual Studio na žádné stránce, které jsme jako ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="c9d9d-151">V Průzkumníku řešení sady Visual Studio klikněte pravým tlačítkem na název řešení a vytvořte nový adresář s názvem "Ovládacích prvků".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="c9d9d-152">I když není nezbytné k tomu, nám pomůže zachovat naše projektu uspořádané podle vytváření všechny naše uživatelské ovládací prvky v adresáři "Ovládacích prvků".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="c9d9d-153">Klikněte pravým tlačítkem na složku, ovládací prvky a zvolte "Nová položka":</span><span class="sxs-lookup"><span data-stu-id="c9d9d-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="c9d9d-154">Zadejte název pro naše řízení "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="c9d9d-155">Všimněte si, že přípona souboru pro uživatelské ovládací prvky .ascx není .aspx.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="c9d9d-156">Naše oblíbených položek uživatelský ovládací prvek bude definován následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="c9d9d-157">Tady používáme metodu, kterou jsme nepoužili ještě v této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="c9d9d-158">Používáme ovládacím prvku repeater a místo použití prvku zdroje dat jsme se do výsledků dotazu LINQ to Entities vazbu ovládacím prvku opakovače.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="c9d9d-159">V kódu naše řízení to následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="c9d9d-160">Všimněte si také tohoto důležité řádku v horní části značky naše ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="c9d9d-161">Vzhledem k tomu většinou oblíbených položek nebude změna na základě minutu na minutu přidáme direktivu bolavými zlepšit výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="c9d9d-162">Tato direktiva způsobí, že kód ovládací prvky, který lze spustit pouze když vyprší platnost výstup z mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="c9d9d-163">Jinak se použije v mezipaměti verzi výstupu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="c9d9d-164">Nyní všechny, které se musí provést je zahrňte do naší stránce s Default.aspc naše nový ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="c9d9d-165">Použití přetažení umístit instanci ovládacího prvku ve sloupci otevřete v našem výchozího formuláře.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="c9d9d-166">Nyní když jsme spuštění aplikace na domovskou stránku, zobrazí se většinou oblíbených položek.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="c9d9d-167">"Taky zakoupili" řízení (uživatelské ovládací prvky s parametry)</span><span class="sxs-lookup"><span data-stu-id="c9d9d-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="c9d9d-168">Druhý uživatelský ovládací prvek, který vytvoříme bude trvat sugestivní prodávané na další úroveň přidáním kontextu specifické podobě.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="c9d9d-169">Logika pro vypočítat nejvyšší položky "Také zakoupili" je netriviální.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="c9d9d-170">Naše "Také zakoupili" řízení bude vyberte záznamy Rozpis objednávek (dřív pořídili) pro aktuálně vybrané ProductID a získat OrderIDs pro každý jedinečný pořadí, která se nachází.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="c9d9d-171">Potom jsme vybere al produkty z těchto objednávek a součet množství zakoupili.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="c9d9d-172">Jsme budete seřadit tento součet objemu produktů a zobrazit prvních pět položek.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="c9d9d-173">S ohledem na složitost tuto logiku, bude tento algoritmus implementaci jako uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="c9d9d-174">T-SQL pro uloženou proceduru je následující.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="c9d9d-175">Všimněte si, že tuto uloženou proceduru (SelectPurchasedWithProducts) existovalo v databázi jsme obsažen v naší aplikaci a jsme generování Entity Data Model, který jsme zadali kromě tabulky a zobrazení, které jsme potřeby datového modelu Entity by měla obsahovat tuto uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="c9d9d-176">Pro přístup k uloženou proceduru z datového modelu Entity musíme import funkce.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="c9d9d-177">Dvakrát klikněte na datový Model Entity v Průzkumníku řešení otevřete v návrháři a otevřít prohlížeč modelu, potom klikněte pravým tlačítkem myši v návrháři a vyberte možnost "Přidat funkce Import".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="c9d9d-178">Tím se otevře dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="c9d9d-179">Vyplňte pole, jak můžete vidět výše, výběr "SelectPurchasedWithProducts" a používat název procedury pro název naše importované funkce.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="c9d9d-180">Klikněte na tlačítko "Ok".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-180">Click "Ok".</span></span>

<span data-ttu-id="c9d9d-181">Má to neudělali, které jsme můžete jednoduše programu proti uloženou proceduru jako jsme může jiné položky v modelu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="c9d9d-182">Ano v našem složce "Ovládací prvky" vytvořte nový uživatelský ovládací prvek s názvem AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="c9d9d-183">Kód pro tento ovládací prvek bude vypadat dobře obeznámeni do ovládacího prvku PopularItems.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="c9d9d-184">Pozoruhodný rozdíl je, že vzhledem k tomu, že k vykreslení položky se budou lišit podle produktu, nejsou ukládání výstupu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="c9d9d-185">ProductId bude "vlastnost" do ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="c9d9d-186">V obslužná rutina ovládacího prvku událost PreRender jsme rychlost tři věci.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="c9d9d-187">Ujistěte se, že je nastaven ProductID.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="c9d9d-188">Viděli, zda jsou všechny produkty, které byly zakoupeny aktuální publikací.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="c9d9d-189">Výstup některé položky určené v #. 2.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="c9d9d-190">Všimněte si, jak je snadné volání uložené procedury prostřednictvím modelu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="c9d9d-191">Po určení, že jsou "také zakoupili" jsme do výsledků vrácených dotazem jednoduše vazbu opakovače.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="c9d9d-192">Pokud všechny položky "také zakoupili" nebyly budete jednoduše zobrazujeme dalších oblíbených položek z našich katalogu.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="c9d9d-193">Zobrazit položky "Také zakoupili", otevřete stránku ProductDetails.aspx a přetáhněte ovládací prvek AlsoPurchased v Průzkumníku řešení tak, aby se zobrazí v této pozici ve značkách.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="c9d9d-194">Tím se vytvoří odkaz na ovládací prvek v horní části stránky ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="c9d9d-195">Vzhledem k tomu, že AlsoPurchased uživatelského ovládacího prvku vyžaduje číslo ProductId jsme se nastavit vlastnost ProductID naše ovládacího prvku pomocí příkazu zkušební verze na aktuální položku modelu dat stránky.</span><span class="sxs-lookup"><span data-stu-id="c9d9d-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="c9d9d-196">Pokud jsme sestavit a spustit nyní a přejděte do produktu jsme viděli položky, které "Také zakoupili".</span><span class="sxs-lookup"><span data-stu-id="c9d9d-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="c9d9d-197">[Předchozí](tailspin-spyworks-part-6.md)
> [další](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="c9d9d-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
