---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Zobrazení dat položky a podrobnosti | Microsoft Docs
author: Erikre
description: Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="display-data-items-and-details"></a><span data-ttu-id="2dac3-103">Zobrazení dat položky a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="2dac3-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="2dac3-104">Podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="2dac3-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="2dac3-105">[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="2dac3-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="2dac3-106">Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web.</span><span class="sxs-lookup"><span data-stu-id="2dac3-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="2dac3-107">Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.</span><span class="sxs-lookup"><span data-stu-id="2dac3-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="2dac3-108">Tento kurz popisuje, jak zobrazit datové položky a podrobnosti o položkách dat pomocí webových formulářů ASP.NET a Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="2dac3-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="2dac3-109">V tomto kurzu vychází předchozí kurz "A uživatelského rozhraní navigace" a je součástí řady kurz Wingtip hračka úložiště.</span><span class="sxs-lookup"><span data-stu-id="2dac3-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="2dac3-110">Po dokončení tohoto kurzu, budete moci zobrazit produkty na *ProductsList.aspx* stránky a podrobnosti o jednotlivých produktů na *ProductDetails.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="2dac3-111">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="2dac3-111">What you'll learn:</span></span>

- <span data-ttu-id="2dac3-112">Postup přidání ovládacího prvku data k zobrazení produkty z databáze.</span><span class="sxs-lookup"><span data-stu-id="2dac3-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="2dac3-113">Postup připojení ovládací prvek dat na vybraná data.</span><span class="sxs-lookup"><span data-stu-id="2dac3-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="2dac3-114">Postup přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu z databáze.</span><span class="sxs-lookup"><span data-stu-id="2dac3-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="2dac3-115">Jak načíst hodnotu z řetězce dotazu a tuto hodnotu použít k omezení dat, které se načítají z databáze.</span><span class="sxs-lookup"><span data-stu-id="2dac3-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="2dac3-116">Tyto jsou funkce, zavedená v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="2dac3-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="2dac3-117">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="2dac3-117">Model Binding</span></span>
- <span data-ttu-id="2dac3-118">Zprostředkovatele hodnot</span><span class="sxs-lookup"><span data-stu-id="2dac3-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="2dac3-119">Přidání ovládacího prvku Data k zobrazení produktů</span><span class="sxs-lookup"><span data-stu-id="2dac3-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="2dac3-120">Při vytváření vazby dat k ovládacímu prvku serveru, několika různými způsoby, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="2dac3-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="2dac3-121">Nejběžnější možnosti zahrnují přidání ovládacího prvku zdroje dat, přidání kódu ručně nebo pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="2dac3-122">K vytvoření vazby dat pomocí ovládacího prvku zdroje dat</span><span class="sxs-lookup"><span data-stu-id="2dac3-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="2dac3-123">Přidání ovládacího prvku zdroje dat můžete propojit prvek zdroje dat k ovládacímu prvku, který zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="2dac3-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="2dac3-124">Tento přístup umožňuje vám umožní deklarativně připojit se přímo do zdroje dat, nikoli pomocí programový přístup ovládacích prvků na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="2dac3-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="2dac3-125">Kódování ručně vázat Data</span><span class="sxs-lookup"><span data-stu-id="2dac3-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="2dac3-126">Přidání kódu ručně zahrnuje čtení hodnoty, kontrola hodnotu null, pokusu o převod na příslušný typ, kontrola, zda byl převod úspěšný a nakonec pomocí hodnoty v dotazu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="2dac3-127">Tento postup by použít, pokud je potřeba zachovat plnou kontrolu nad logika přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="2dac3-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="2dac3-128">Pomocí vazby vázat Data modelu</span><span class="sxs-lookup"><span data-stu-id="2dac3-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="2dac3-129">Pomocí vazby modelu umožňuje svázání výsledky pomocí mnohem méně kódu a budete moci znovu použít funkci v celé vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2dac3-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="2dac3-130">Pro zjednodušení práce s logiku zaměřené na kód přístup k datům a zároveň zachovat výhody bohaté, vazby dat framework cílem je vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="2dac3-131">Zobrazení produktů</span><span class="sxs-lookup"><span data-stu-id="2dac3-131">Displaying Products</span></span>

<span data-ttu-id="2dac3-132">V tomto kurzu použijete k vytvoření vazby dat vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="2dac3-133">Pokud chcete konfigurovat ovládací prvek dat pomocí vazby modelu vyberte data, nastavte ovládacího prvku `SelectMethod` vlastnost na název metody v kódu stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="2dac3-134">Ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky vytvoří vazbu vrácená data.</span><span class="sxs-lookup"><span data-stu-id="2dac3-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="2dac3-135">Není nutné explicitně volat `DataBind` metoda.</span><span class="sxs-lookup"><span data-stu-id="2dac3-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="2dac3-136">Pomocí následujícího postupu, budete upravovat kód v *ProductList.aspx* stránky tak, aby stránce můžete zobrazit produkty.</span><span class="sxs-lookup"><span data-stu-id="2dac3-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="2dac3-137">V **Průzkumníku řešení**, otevřete *ProductList.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="2dac3-138">Nahraďte kód existující následující kód:</span><span class="sxs-lookup"><span data-stu-id="2dac3-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="2dac3-139">Tento kód používá **ListView** ovládací prvek s názvem "productList" zobrazíte produkty.</span><span class="sxs-lookup"><span data-stu-id="2dac3-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="2dac3-140">**ListView** ovládací prvek zobrazí data ve formátu, který definujete pomocí šablony a stylů.</span><span class="sxs-lookup"><span data-stu-id="2dac3-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="2dac3-141">Je vhodné pro data v jakékoli opakující se podobě.</span><span class="sxs-lookup"><span data-stu-id="2dac3-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="2dac3-142">To **ListView** příklad jednoduše zobrazuje data z databáze, ale můžete povolit uživatelům upravit, insert a odstranění dat a řazení a datům stránky, aniž byste museli kód.</span><span class="sxs-lookup"><span data-stu-id="2dac3-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="2dac3-143">Nastavením `ItemType` vlastnost v **ListView** řídit, vazby dat výraz `Item` je k dispozici a bude ovládacího prvku silného typu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="2dac3-144">Jak je uvedeno v předchozí kurzu, můžete vybrat Podrobnosti objektu položku pomocí IntelliSense, například určení `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="2dac3-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![Zobrazení dat položky a podrobnosti - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="2dac3-146">Kromě toho zadejte pomocí vazby modelu `SelectMethod` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="2dac3-147">Tato hodnota (`GetProducts`) bude odpovídat metoda, která přidáte do kódu zobrazíte produkty v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="2dac3-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="2dac3-148">Přidání kódu k zobrazení produktů</span><span class="sxs-lookup"><span data-stu-id="2dac3-148">Adding Code to Display Products</span></span>

<span data-ttu-id="2dac3-149">V tomto kroku přidáte kód k vyplnění **ListView** ovládacího prvku pomocí produktu data z databáze.</span><span class="sxs-lookup"><span data-stu-id="2dac3-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="2dac3-150">Kód bude podporovat zobrazující produkty jednotlivé kategorie, jakož i zobrazuje všechny produkty.</span><span class="sxs-lookup"><span data-stu-id="2dac3-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="2dac3-151">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *ProductList.aspx* a pak klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="2dac3-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="2dac3-152">Nahraďte stávající kód v *ProductList.aspx.cs* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2dac3-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="2dac3-153">Tento kód ukazuje `GetProducts` metoda, která odkazuje `ItemType` vlastnost **ListView** řídit ve *ProductList.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="2dac3-154">Omezit výsledky do určité kategorie v databázi, nastaví kód `categoryId` hodnotu z hodnotu řetězce dotazu, který je předán *ProductList.aspx* stránce, pokud *ProductList.aspx* stránka je přešli.</span><span class="sxs-lookup"><span data-stu-id="2dac3-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="2dac3-155">`QueryStringAttribute` Třídy v `System.Web.ModelBinding` obor názvů se používá k načtení hodnoty id proměnné řetězce dotazu. Tím se nastaví vazby modelů a pokuste se vytvořit vazbu hodnotu z řetězce dotazu do `categoryId` parametr za běhu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="2dac3-156">Když platný kategorie předána jako řetězec dotazu na stránku, výsledky dotazu jsou omezeny na tyto produkty v databázi, které odpovídají `categoryId` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="2dac3-157">Například pokud adresu URL *ProductsList.aspx* stránka je následující:</span><span class="sxs-lookup"><span data-stu-id="2dac3-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="2dac3-158">Na stránce se zobrazuje pouze produkty, kde `category` rovná `1`.</span><span class="sxs-lookup"><span data-stu-id="2dac3-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="2dac3-159">Pokud žádný řetězec dotazu je zahrnuto při přechodu *ProductList.aspx* stránce se zobrazí všechny produkty.</span><span class="sxs-lookup"><span data-stu-id="2dac3-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="2dac3-160">Zdroje hodnot pro tyto metody jsou označovány jako *hodnotu zprostředkovatelé* (například *řetězce dotazu*), a atributy parametru, které indikují hodnota poskytovatele, kterého chcete použít se označují jako hodnota Zprostředkovatel atributy (například "`id`").</span><span class="sxs-lookup"><span data-stu-id="2dac3-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="2dac3-161">Technologie ASP.NET obsahuje zprostředkovatele hodnot a odpovídající atributy pro všechny z typických zdrojů uživatelský vstup ve formulářové webové aplikaci, například řetězec dotazu, soubory cookie, hodnot formuláře, ovládací prvky, stav zobrazení, stav relace a vlastnosti profilu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="2dac3-162">Je také možné zapsat zprostředkovatele vlastních hodnot.</span><span class="sxs-lookup"><span data-stu-id="2dac3-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="2dac3-163">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="2dac3-163">Running the Application</span></span>

<span data-ttu-id="2dac3-164">Spusťte aplikaci teď chcete zobrazit, jak můžete zobrazit všechny produkty nebo jenom sadu produktů omezené podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="2dac3-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="2dac3-165">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Default.aspx* a vyberte **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="2dac3-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="2dac3-166">Prohlížeč a zobrazit *Default.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="2dac3-167">Vyberte **aut** v navigační nabídce kategorie produktu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="2dac3-168">*ProductList.aspx* se zobrazí stránka zobrazuje jenom produkty, které jsou zahrnuty v kategorii "Aut".</span><span class="sxs-lookup"><span data-stu-id="2dac3-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="2dac3-169">Později v tomto kurzu se zobrazí podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-169">Later in this tutorial, you will display product details.</span></span>  

    ![Zobrazení dat položky a podrobnosti - automobilů](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="2dac3-171">Vyberte **produkty** v navigační nabídce v horní části.</span><span class="sxs-lookup"><span data-stu-id="2dac3-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="2dac3-172">Znovu *ProductList.aspx* se zobrazí stránka, ale tentokrát zobrazuje celý seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="2dac3-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Zobrazení dat položky a podrobnosti - produktů](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="2dac3-174">Zavřete prohlížeč a vraťte se k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2dac3-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="2dac3-175">Přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu</span><span class="sxs-lookup"><span data-stu-id="2dac3-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="2dac3-176">Dále budete upravovat kód v *ProductDetails.aspx* stránku, kterou jste přidali v předchozím kurzu tak, aby stránce můžete zobrazit informace o jednotlivých produktů.</span><span class="sxs-lookup"><span data-stu-id="2dac3-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="2dac3-177">V **Průzkumníku řešení**, otevřete *ProductDetails.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="2dac3-178">Nahraďte kód existující následující kód:</span><span class="sxs-lookup"><span data-stu-id="2dac3-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="2dac3-179">Tento kód používá **FormView** ovládací prvek zobrazí podrobnosti o jednotlivých produktů.</span><span class="sxs-lookup"><span data-stu-id="2dac3-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="2dac3-180">Tento kód používá metody, třeba ty, které se používají k zobrazení dat v *ProductList.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="2dac3-181">**FormView** řízení slouží k zobrazení jeden záznam ze zdroje dat najednou.</span><span class="sxs-lookup"><span data-stu-id="2dac3-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="2dac3-182">Při použití **FormView** ovládací prvek, je vytvořit šablony k zobrazení a úpravě hodnoty vázané na data.</span><span class="sxs-lookup"><span data-stu-id="2dac3-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="2dac3-183">Šablony obsahují ovládací prvky, formátování a vazby výrazy definující vzhled a funkčnost formuláře.</span><span class="sxs-lookup"><span data-stu-id="2dac3-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="2dac3-184">Výše uvedený kód připojit k databázi, je nutné přidat další kód pro *ProductDetails.aspx* kódu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="2dac3-185">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *ProductDetails.aspx* a pak klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="2dac3-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="2dac3-186">*ProductDetails.aspx.cs* souboru se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="2dac3-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="2dac3-187">Existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2dac3-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="2dac3-188">Tento kód zkontroluje "`productID`" hodnota řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="2dac3-189">Pokud je nalezen hodnotu platný řetězec dotazu, zobrazí se odpovídající produktu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="2dac3-190">Pokud nenajde žádný řetězec dotazu, nebo hodnotu řetězce dotazu není platný, žádný produkt se zobrazí na *ProductDetails.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="2dac3-191">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="2dac3-191">Running the Application</span></span>

<span data-ttu-id="2dac3-192">Teď můžete spustit aplikace, abyste viděli zobrazí jednotlivé produkty na základě id produktu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="2dac3-193">Stiskněte klávesu **F5** při v sadě Visual Studio a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2dac3-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="2dac3-194">Prohlížeč a zobrazit *Default.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="2dac3-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="2dac3-195">Vyberte "Lodě" v navigační nabídce kategorie.</span><span class="sxs-lookup"><span data-stu-id="2dac3-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="2dac3-196">*ProductList.aspx* zobrazí se stránka.</span><span class="sxs-lookup"><span data-stu-id="2dac3-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="2dac3-197">Vyberte produkt "Dokumentu člun" ze seznamu produktu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="2dac3-198">*ProductDetails.aspx* zobrazí se stránka.</span><span class="sxs-lookup"><span data-stu-id="2dac3-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Zobrazení dat položky a podrobnosti - produktů](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="2dac3-200">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="2dac3-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="2dac3-201">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2dac3-201">Summary</span></span>

<span data-ttu-id="2dac3-202">V tomto kurzu řady mají přidáte značek a kódu, pokud chcete zobrazit seznam produktů a zobrazit podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="2dac3-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="2dac3-203">Během tohoto procesu jste se naučili o ovládacích prvcích silného typu dat, vazby modelu a zprostředkovatele hodnot.</span><span class="sxs-lookup"><span data-stu-id="2dac3-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="2dac3-204">V dalším kurzu přidáte nákupní košík na adresář Wingtip Toys ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2dac3-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2dac3-205">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="2dac3-205">Additional Resources</span></span>

[<span data-ttu-id="2dac3-206">Načítání a zobrazování dat pomocí vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="2dac3-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="2dac3-207">[Předchozí](ui_and_navigation.md)
> [další](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="2dac3-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
