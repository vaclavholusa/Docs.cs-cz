---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: '7. část: Přidání funkcí | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 7 přidává další funkce, jako je například účet revie...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 47402ccdfb702dc1bb1bdb4e634a7cd6f5ebc235
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831645"
---
<a name="part-7-adding-features"></a><span data-ttu-id="2ddf4-104">7. část: Přidání funkcí</span><span class="sxs-lookup"><span data-stu-id="2ddf4-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="2ddf4-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="2ddf4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="2ddf4-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="2ddf4-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="2ddf4-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="2ddf4-109">Část 7 přidává další funkce, jako je například účet revize, recenzemi produktů a "Oblíbené položky" a "také zakoupené" uživatelské ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="2ddf4-110">Přidání funkce</span><span class="sxs-lookup"><span data-stu-id="2ddf4-110">Adding Features</span></span>

<span data-ttu-id="2ddf4-111">I když můžou uživatelé procházet náš katalog, umístěte položky do nákupního košíku a dokončete proces platby u pokladny, jsou že počet podpůrné funkce zavedeme vylepšit náš web.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="2ddf4-112">Účet kontroly (seznamu objednávek umístit a zobrazit podrobnosti.)</span><span class="sxs-lookup"><span data-stu-id="2ddf4-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="2ddf4-113">Přidejte nějaký obsah konkrétní kontext na přední stránce.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="2ddf4-114">Přidáte funkci, která umožní uživatelům revize produktů v katalogu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="2ddf4-115">Vytvoření uživatelského ovládacího prvku se zobrazí na přední stránce oblíbených položek a místo, které řídí.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="2ddf4-116">Vytvořte uživatelský ovládací prvek "Také zakoupili" a přidejte na stránku podrobností produktu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="2ddf4-117">Přidat kontakt stránky.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="2ddf4-118">Přidat o stránce.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-118">Add an About Page.</span></span>
8. <span data-ttu-id="2ddf4-119">Globální zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="2ddf4-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="2ddf4-120">Účet kontroly</span><span class="sxs-lookup"><span data-stu-id="2ddf4-120">Account Review</span></span>

<span data-ttu-id="2ddf4-121">Ve složce "Účet" vytvořte dvě stránky ASPX, jednu s názvem OrderList.aspx a dalších pojmenované OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="2ddf4-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="2ddf4-122">OrderList.aspx bude využívat ovládací prvky GridView a EntityDataSoure podobně, jako máme k dispozici dříve.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="2ddf4-123">EntityDataSoure vybírá záznamy v tabulce objednávky filtrované podle uživatelského jména (viz WhereParameter), které jsme si nastavili v proměnné relace při uživateli přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="2ddf4-124">Všimněte si také v HyperlinkField prvku GridView. Tyto parametry:</span><span class="sxs-lookup"><span data-stu-id="2ddf4-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="2ddf4-125">Tyto zadejte odkaz na mohli zobrazit podrobnosti pro jednotlivé produkty, které zadáte jako parametr řetězce dotazu na stránku OrderDetails.aspx pole OrderID.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="2ddf4-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="2ddf4-126">OrderDetails.aspx</span></span>

<span data-ttu-id="2ddf4-127">Ovládací prvek EntityDataSource budeme používat pro přístup k objednávky a FormView zobrazují data objednávky a jiné EntityDataSource s GridView pro zobrazení položek řádku všechny objednávky.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="2ddf4-128">V souboru kódu za (OrderDetails.aspx.cs) máme dva bity malý údržbu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="2ddf4-129">Nejdřív potřebujeme Ujistěte se, že OrderDetails vždy získá OrderId.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="2ddf4-130">Musíme také vypočítávají a zobrazují celkový počet položek řádku pořadí.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="2ddf4-131">Na domovské stránce</span><span class="sxs-lookup"><span data-stu-id="2ddf4-131">The Home Page</span></span>

<span data-ttu-id="2ddf4-132">Přidejme nějaký statický obsah na stránku Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="2ddf4-133">Nejprve vytvoříte složku "Obsah" a v něm složku obrázky (a jsem bude obsahovat obrázek, který se použije na domovské stránce.)</span><span class="sxs-lookup"><span data-stu-id="2ddf4-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="2ddf4-134">V dolní části zástupný symbol stránku Default.aspx přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="2ddf4-135">Revize produktu</span><span class="sxs-lookup"><span data-stu-id="2ddf4-135">Product Reviews</span></span>

<span data-ttu-id="2ddf4-136">Nejdřív přidáme tlačítko s odkazem na formulář, který jsme můžete použít k zadání recenze produktu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="2ddf4-137">Všimněte si, že jsme v řetězci dotazu prochází ProductID</span><span class="sxs-lookup"><span data-stu-id="2ddf4-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="2ddf4-138">Další přidáme stránku s názvem ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="2ddf4-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="2ddf4-139">Tato stránka bude používat ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="2ddf4-140">Pokud jste tak ještě neučinili, můžete ji stáhnout [DevExpress](http://devexpress.com/act) a pokyny o nastavení sady nástrojů pro použití se službou Visual Studio zde [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="2ddf4-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="2ddf4-141">V režimu návrhu přetáhněte z panelu nástrojů ovládacích prvků a validátory a podobná té následující formulář vytvářet.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="2ddf4-142">Značky budou vypadat asi takhle nějak.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="2ddf4-143">Teď můžeme zadat revize, vám umožní zobrazit tyto kontroly na stránce produktu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="2ddf4-144">Přidejte tento kód na stránku ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="2ddf4-145">Spuštění naší aplikace teď a přejdete do produktu se zobrazí informace o produktech, včetně zapracováním připomínek zákazníků.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="2ddf4-146">Oblíbené položky ovládacího prvku (vytváření uživatelských ovládacích prvků)</span><span class="sxs-lookup"><span data-stu-id="2ddf4-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="2ddf4-147">Za účelem zvýšení prodeje na webové stránce přidáme několik funkcí "sugestivní sell" Oblíbené nebo souvisejících produktů.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="2ddf4-148">První z těchto funkcí bude zahrnovat další oblíbené produktu v našich katalog produktů.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="2ddf4-149">Vytvoříme "Uživatelský ovládací prvek" k zobrazení nejvyšší prodeje položky na domovské stránce naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="2ddf4-150">Vzhledem k tomu, že to bude ovládací prvek, jsme jednoduše přetahováním ovládací prvek v návrháři aplikace Visual Studio stránku, který budeme používat na libovolné stránce.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="2ddf4-151">V Průzkumníku řešení sady Visual Studio klikněte pravým tlačítkem myši na název řešení a vytvořte nový adresář s názvem "Prvky".</span><span class="sxs-lookup"><span data-stu-id="2ddf4-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="2ddf4-152">I když není potřeba udělat, bude pomůžeme našem projektu uspořádané podle vytváření všech našich uživatelské ovládací prvky v adresáři "Ovládací prvky".</span><span class="sxs-lookup"><span data-stu-id="2ddf4-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="2ddf4-153">Klikněte pravým tlačítkem na složku Ovládací prvky a zvolte možnost "Nová položka":</span><span class="sxs-lookup"><span data-stu-id="2ddf4-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="2ddf4-154">Zadejte název pro naše ovládací prvek "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="2ddf4-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="2ddf4-155">Všimněte si, že přípona souboru pro uživatelské ovládací prvky .ascx není .aspx.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="2ddf4-156">Naše Oblíbené položky uživatelský ovládací prvek bude definovaná následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="2ddf4-157">Tady používáme metoda, kterou jsme nepoužili ještě v této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="2ddf4-158">Používáme ovládací prvek repeater a namísto použití ovládací prvek zdroje dat jsme provádíte navázání ovládacím prvku opakovače s výsledky dotazu LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="2ddf4-159">V kódu naše ovládací prvek provedeme to následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="2ddf4-160">Všimněte si také tato důležité řádku v horní části kódu naše ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="2ddf4-161">Vzhledem k tomu, že Nejoblíbenější položky nebudou měnit na základě minut na minutu můžeme přidat direktivu bolavými ke zlepšení výkonu našich aplikací.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="2ddf4-162">Tato direktiva způsobí, že kód ovládacích prvků na spustit pouze když vyprší platnost výstup z mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="2ddf4-163">V opačném případě se použije verze uložené v mezipaměti ovládacího prvku výstupu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="2ddf4-164">Nyní je vše, co musíme udělat zahrňte naši stránku Default.aspc náš nový ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="2ddf4-165">Použití přetažení umístit ve sloupci otevřít v našem formuláři výchozí instanci ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="2ddf4-166">Nyní když jsme spuštění aplikace na domovské stránce zobrazí Oblíbené položky.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="2ddf4-167">"Také zakoupili" ovládací prvek (uživatelské ovládací prvky s parametry)</span><span class="sxs-lookup"><span data-stu-id="2ddf4-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="2ddf4-168">Druhý uživatelský ovládací prvek, který vytvoříme bude trvat sugestivní prodej na vyšší úroveň přidáním specifičnosti kontextu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="2ddf4-169">Logika pro výpočet "Také zakoupili" položky na nejvyšší úrovni je netriviální.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="2ddf4-170">Naše "Také zakoupili" ovládací prvek bude vyberte záznamy OrderDetails (jste dříve zakoupili) pro aktuálně vybrané ProductID a získejte OrderIDs pro každý jedinečných pořadí, ve kterém se nachází.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="2ddf4-171">Pak bude vybereme al produktů ze všech objednávek a množství zakoupených součet.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="2ddf4-172">Vytvoříme řadit tento součet množství produktů a zobrazte prvních pět položek.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="2ddf4-173">Vzhledem ke složitosti tuto logiku, jsme jako uloženou proceduru implementuje tento algoritmus.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="2ddf4-174">T-SQL pro úložnou proceduru vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="2ddf4-175">Všimněte si, že tuto uloženou proceduru (SelectPurchasedWithProducts) existoval v databázi, když jsme zahrnuli v naší aplikaci a jsme generování modelu Entity Data Model, který jsme zadali kromě tabulky a zobrazení, které jsme potřebovali, modelu Entity Data Model tuto uloženou proceduru by měla obsahovat.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="2ddf4-176">Pro přístup k uloženou proceduru z modelu Entity Data Model potřebujeme import funkce.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="2ddf4-177">Dvakrát klikněte na datový Model Entity v Průzkumníku řešení otevřete v návrháři a otevřít prohlížeč modelu pak v Návrháři klikněte pravým tlačítkem a vyberte "Přidání importované funkce".</span><span class="sxs-lookup"><span data-stu-id="2ddf4-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="2ddf4-178">Tím se otevře toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="2ddf4-179">Vyplňte pole, jak vidíte výše, vyberete "SelectPurchasedWithProducts" a použijte název procedury pro název naší importované funkce.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="2ddf4-180">Klikněte na tlačítko "Ok".</span><span class="sxs-lookup"><span data-stu-id="2ddf4-180">Click "Ok".</span></span>

<span data-ttu-id="2ddf4-181">Udělání to, které můžete jednoduše programujeme uloženou proceduru jako jsme může být jakoukoli jinou položku v modelu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="2ddf4-182">Tedy ve složce naše "Řídí" vytvořte nový uživatelský ovládací prvek s názvem AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="2ddf4-183">Značky pro tento ovládací prvek bude velmi povědomá do ovládacího prvku PopularItems.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="2ddf4-184">Významný rozdíl spočívá v tom, která nejsou ukládání do vyrovnávací paměti výstup vzhledem k tomu, že k vykreslení položky se budou lišit podle produktu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="2ddf4-185">ProductId bude "vlastnosti" do ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="2ddf4-186">V obslužné rutině ovládacího prvku událost PreRender jsme rychlost udělat tři věci.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="2ddf4-187">Ujistěte se, že je nastavena ProductID.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="2ddf4-188">Zkontrolujte, jestli všechny produkty, které jste zakoupili s aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="2ddf4-189">Výstupní některé položky, jak je stanoveno v #2.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="2ddf4-190">Všimněte si, jak snadné je volat úložnou proceduru prostřednictvím modelu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="2ddf4-191">Po určení, která existuje "také nakupujete" jsme můžete jednoduše vytvořit vazbu opakovače s výsledky vrácené dotazem.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="2ddf4-192">Pokud se všechny položky "také zakoupili" nebyly budete jednoduše zobrazují další oblíbené položky z našich katalogu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="2ddf4-193">Chcete-li zobrazit položky "Také koupit", otevřete stránku ProductDetails.aspx a přetáhněte ovládací prvek AlsoPurchased z Průzkumníka řešení, aby se zobrazovala na této pozici v kódu.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="2ddf4-194">Tím se vytvoří odkaz na ovládací prvek v horní části stránky ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="2ddf4-195">Protože AlsoPurchased uživatelský ovládací prvek vyžaduje ProductId číslo nastavíme vlastnost ProductID naše ovládacího prvku pomocí příkazu zkušební verze na aktuální položku modelu data stránky.</span><span class="sxs-lookup"><span data-stu-id="2ddf4-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="2ddf4-196">Když jsme sestavit a spustit nyní a přejděte do produktu vidíme položky "Také zakoupili".</span><span class="sxs-lookup"><span data-stu-id="2ddf4-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="2ddf4-197">[Předchozí](tailspin-spyworks-part-6.md)
> [další](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="2ddf4-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
