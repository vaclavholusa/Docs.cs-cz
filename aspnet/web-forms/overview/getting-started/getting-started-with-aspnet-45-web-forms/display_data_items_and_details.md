---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Zobrazení datových položek a podrobností | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 08efafc1ae076bcf481c5c7d8aea2bbaa704af17
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379735"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="efba0-103">Zobrazení datových položek a podrobností</span><span class="sxs-lookup"><span data-stu-id="efba0-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="efba0-104">podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="efba0-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="efba0-105">[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="efba0-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="efba0-106">V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="efba0-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="efba0-107">Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="efba0-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="efba0-108">Tento kurz popisuje způsob zobrazení datových položek a podrobnosti o položkách data pomocí webových formulářů ASP.NET a Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="efba0-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="efba0-109">V tomto kurzu vychází z předchozí kurz o službě "Uživatelské rozhraní a navigace" a je součástí série kurzů Wingtip Slonovi Store.</span><span class="sxs-lookup"><span data-stu-id="efba0-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="efba0-110">Po dokončení tohoto kurzu, budete moct zobrazit produkty na *ProductsList.aspx* stránky a podrobnosti o jednotlivých produktů na *ProductDetails.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="efba0-111">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="efba0-111">What you'll learn:</span></span>

- <span data-ttu-id="efba0-112">Postup přidání ovládacího prvku dat pro zobrazení produktů z databáze.</span><span class="sxs-lookup"><span data-stu-id="efba0-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="efba0-113">Postup připojení ovládacího prvku na vybraná data.</span><span class="sxs-lookup"><span data-stu-id="efba0-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="efba0-114">Postup přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu z databáze.</span><span class="sxs-lookup"><span data-stu-id="efba0-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="efba0-115">Jak načíst hodnotu z řetězce dotazu a tuto hodnotu použít k omezení dat načtených z databáze.</span><span class="sxs-lookup"><span data-stu-id="efba0-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="efba0-116">Toto jsou vlastnosti představené v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="efba0-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="efba0-117">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="efba0-117">Model Binding</span></span>
- <span data-ttu-id="efba0-118">Zprostředkovatele hodnot</span><span class="sxs-lookup"><span data-stu-id="efba0-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="efba0-119">Přidání ovládacího prvku dat pro zobrazení produktů</span><span class="sxs-lookup"><span data-stu-id="efba0-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="efba0-120">Při vytváření vazby dat k ovládacímu prvku serveru, několika různými způsoby, kterými můžete.</span><span class="sxs-lookup"><span data-stu-id="efba0-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="efba0-121">Nejběžnější možnosti zahrnují přidání ovládací prvek zdroje dat, přidání kódu ručně nebo pomocí vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="efba0-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="efba0-122">Vytvoření vazby dat pomocí ovládacího prvku zdroje dat</span><span class="sxs-lookup"><span data-stu-id="efba0-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="efba0-123">Přidání ovládacího prvku zdroje dat umožňuje propojit ovládací prvek zdroje dat k ovládacímu prvku, který se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="efba0-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="efba0-124">Tento přístup umožňuje vám umožní deklarativně připojit se přímo do zdroje dat, nikoli pomocí programový přístup serverové ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="efba0-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="efba0-125">Ručně kódování až po vytvoření vazby dat</span><span class="sxs-lookup"><span data-stu-id="efba0-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="efba0-126">Přidání kódu ručně zahrnuje čtení hodnoty, kontrola hodnot null, pokusu o převod na příslušný typ, kontroluje, zda byl převod úspěšný a nakonec pomocí hodnoty v dotazu.</span><span class="sxs-lookup"><span data-stu-id="efba0-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="efba0-127">Tento postup byste použili, když budete chtít zachovat plnou kontrolu nad logiky přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="efba0-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="efba0-128">Pomocí modelu vazba k vytvoření vazby dat</span><span class="sxs-lookup"><span data-stu-id="efba0-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="efba0-129">Pomocí vazby modelu umožňuje vytvořit vazbu výsledky pomocí mnohem méně kódu a poskytuje možnosti opakovaně používat funkce v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="efba0-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="efba0-130">Vazby modelu cílem je zjednodušit práci s logikou zaměřený na kód – přístup k datům při zachování výhod framework bohatě vybaveným a datové vazby.</span><span class="sxs-lookup"><span data-stu-id="efba0-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="efba0-131">Zobrazení produkty</span><span class="sxs-lookup"><span data-stu-id="efba0-131">Displaying Products</span></span>

<span data-ttu-id="efba0-132">V tomto kurzu použijete k vytvoření vazby dat vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="efba0-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="efba0-133">Chcete-li nakonfigurovat ovládací prvek data pomocí vazby modelu vyberte data, nastavte ovládacího prvku `SelectMethod` nastavte název metody v kódu stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="efba0-134">Ovládací prvek dat volá metodu v příslušnou dobu v životním cyklu stránky a automaticky sváže s vrácenými daty.</span><span class="sxs-lookup"><span data-stu-id="efba0-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="efba0-135">Není nutné explicitně volat `DataBind` metody.</span><span class="sxs-lookup"><span data-stu-id="efba0-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="efba0-136">Pomocí následujícího postupu upravíte značky *ProductList.aspx* stránce tak, aby na stránce můžete zobrazit produkty.</span><span class="sxs-lookup"><span data-stu-id="efba0-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="efba0-137">V **Průzkumníka řešení**, otevřete *ProductList.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="efba0-138">Nahraďte stávající kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="efba0-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="efba0-139">Tento kód používá **ListView** ovládací prvek s názvem "productList" pro zobrazení produktů.</span><span class="sxs-lookup"><span data-stu-id="efba0-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="efba0-140">**ListView** ovládací prvek zobrazuje data ve formátu, který definujete pomocí šablony a styly.</span><span class="sxs-lookup"><span data-stu-id="efba0-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="efba0-141">Je vhodné pro data v libovolné opakující se struktura.</span><span class="sxs-lookup"><span data-stu-id="efba0-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="efba0-142">To **ListView** příklad jednoduše zobrazí data z databáze, ale můžete povolit uživatelům upravit, vložení a odstranění dat a seřadit a datům stránky, aniž byste museli kód.</span><span class="sxs-lookup"><span data-stu-id="efba0-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="efba0-143">Nastavením `ItemType` vlastnost **ListView** řídit vazbový výraz `Item` je k dispozici a ovládací prvek stane silného typu.</span><span class="sxs-lookup"><span data-stu-id="efba0-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="efba0-144">Jak je uvedeno v předchozím kurzu, můžete vybrat Podrobnosti objektu položky díky technologii IntelliSense, jako je například určení `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="efba0-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![Zobrazení dat položek a podrobnosti o – technologie IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="efba0-146">Kromě toho používáte vazby modelu k určení `SelectMethod` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="efba0-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="efba0-147">Tato hodnota (`GetProducts`) bude odpovídat metoda, která přidáte do kódu pro zobrazení produktů v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="efba0-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="efba0-148">Přidání kódu pro zobrazení produktů</span><span class="sxs-lookup"><span data-stu-id="efba0-148">Adding Code to Display Products</span></span>

<span data-ttu-id="efba0-149">V tomto kroku přidáte kód k vyplnění **ListView** ovládacího prvku pomocí produktu data z databáze.</span><span class="sxs-lookup"><span data-stu-id="efba0-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="efba0-150">Kód bude podporovat zobrazující produktů podle jednotlivých kategorií, jakož i zobrazující všechny produkty.</span><span class="sxs-lookup"><span data-stu-id="efba0-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="efba0-151">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *ProductList.aspx* a potom klikněte na tlačítko **zobrazit kód**.</span><span class="sxs-lookup"><span data-stu-id="efba0-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="efba0-152">Nahraďte existující kód ve třídě *ProductList.aspx.cs* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="efba0-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="efba0-153">Tento kód ukazuje `GetProducts` metodu, která odkazuje `ItemType` vlastnost **ListView** v ovládacím prvku *ProductList.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="efba0-154">Omezit rozsah výsledků do určité kategorie v databázi, kód nastaví `categoryId` hodnotu z hodnotu řetězce dotazu, který je předán *ProductList.aspx* stránce, pokud *ProductList.aspx* stránka je Přejde.</span><span class="sxs-lookup"><span data-stu-id="efba0-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="efba0-155">`QueryStringAttribute` Třídy v `System.Web.ModelBinding` obor názvů slouží k načtení hodnoty id proměnné řetězce dotazu. Toto dá pokyn vazby modelu pro pokus o vytvoření vazby hodnotu z řetězce dotazu do `categoryId` parametr v době běhu.</span><span class="sxs-lookup"><span data-stu-id="efba0-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="efba0-156">Když platnou kategorii je předán jako řetězec dotazu na stránce výsledky dotazu jsou omezené na tyto produkty v databázi, které odpovídají `categoryId` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="efba0-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="efba0-157">Například pokud adresa URL *ProductsList.aspx* stránka je následující:</span><span class="sxs-lookup"><span data-stu-id="efba0-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="efba0-158">Na stránce se zobrazí pouze produkty, kde `category` rovná `1`.</span><span class="sxs-lookup"><span data-stu-id="efba0-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="efba0-159">Pokud žádný řetězec dotazu je dostupná při přechodu na *ProductList.aspx* stránce se zobrazí všechny produkty.</span><span class="sxs-lookup"><span data-stu-id="efba0-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="efba0-160">Zdroje hodnoty pro tyto metody jsou označovány jako *hodnota poskytovatelé* (například *řetězce dotazu*), a parametr atributy, které označují které zprostředkovatele hodnot pro použití se označují jako hodnota Zprostředkovatel atributů (jako například "`id`").</span><span class="sxs-lookup"><span data-stu-id="efba0-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="efba0-161">Technologie ASP.NET obsahuje zprostředkovatele hodnot a odpovídající atributy pro všemi typické zdroji uživatelský vstup v aplikaci webových formulářů, jako je například řetězec dotazu, soubory cookie, hodnot formuláře, ovládací prvky, zobrazit stav, stav relace a vlastnosti profilu.</span><span class="sxs-lookup"><span data-stu-id="efba0-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="efba0-162">Můžete taky psát vlastní hodnotu poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="efba0-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="efba0-163">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="efba0-163">Running the Application</span></span>

<span data-ttu-id="efba0-164">Spusťte aplikaci teď zobrazíte, jak můžete zobrazit všechny produkty nebo jenom sadu produktů podle kategorie omezené.</span><span class="sxs-lookup"><span data-stu-id="efba0-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="efba0-165">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Default.aspx* stránku a vybrat **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="efba0-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="efba0-166">V prohlížeči se otevře a zobrazí *Default.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="efba0-167">Vyberte **auta** z navigační nabídky kategorie produktu.</span><span class="sxs-lookup"><span data-stu-id="efba0-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="efba0-168">*ProductList.aspx* se zobrazí stránka zobrazující pouze produkty, které jsou zahrnuté v kategorii cars (auta).</span><span class="sxs-lookup"><span data-stu-id="efba0-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="efba0-169">Později v tomto kurzu se zobrazí podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="efba0-169">Later in this tutorial, you will display product details.</span></span>  

    ![Zobrazení dat položek a podrobnosti - automobilů](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="efba0-171">Vyberte **produkty** z navigační nabídce v horní části.</span><span class="sxs-lookup"><span data-stu-id="efba0-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="efba0-172">Znovu *ProductList.aspx* se zobrazí stránka, ale tentokrát zobrazuje úplný seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="efba0-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Zobrazení dat položek a podrobnosti - produkty](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="efba0-174">Zavřete prohlížeč a vraťte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efba0-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="efba0-175">Přidání ovládacího prvku dat. Chcete-li zobrazit podrobnosti o produktu</span><span class="sxs-lookup"><span data-stu-id="efba0-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="efba0-176">V dalším kroku upravíte značky *ProductDetails.aspx* stránka, která jste přidali v předchozím kurzu tak, aby na stránce můžete zobrazit informace o jednotlivých produktů.</span><span class="sxs-lookup"><span data-stu-id="efba0-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="efba0-177">V **Průzkumníka řešení**, otevřete *ProductDetails.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="efba0-178">Nahraďte stávající kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="efba0-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="efba0-179">Tento kód používá **FormView** ovládací prvek pro zobrazení podrobností o jednotlivých produktů.</span><span class="sxs-lookup"><span data-stu-id="efba0-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="efba0-180">Tento kód použije metody podobné těm, které se používají k zobrazení dat v *ProductList.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="efba0-181">**FormView** ovládacího prvku se používá k zobrazení jeden záznam v čase ze zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="efba0-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="efba0-182">Při použití **FormView** ovládacího prvku, je vytvořit šablony můžete zobrazit a upravit hodnoty vázané na data.</span><span class="sxs-lookup"><span data-stu-id="efba0-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="efba0-183">Šablony obsahují ovládací prvky, vazba výrazy a formátování, které definují vzhled a funkce formuláře.</span><span class="sxs-lookup"><span data-stu-id="efba0-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="efba0-184">Pro připojení k databázi výše uvedené značky, je nutné přidat dalšího kódu, který *ProductDetails.aspx* kódu.</span><span class="sxs-lookup"><span data-stu-id="efba0-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="efba0-185">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *ProductDetails.aspx* a potom klikněte na tlačítko **zobrazit kód**.</span><span class="sxs-lookup"><span data-stu-id="efba0-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="efba0-186">*ProductDetails.aspx.cs* souboru se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="efba0-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="efba0-187">Nahraďte stávající kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="efba0-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="efba0-188">Tento kód kontroluje "`productID`" hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="efba0-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="efba0-189">Pokud není nalezena hodnota platný řetězec dotazu, zobrazí se odpovídající produkt.</span><span class="sxs-lookup"><span data-stu-id="efba0-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="efba0-190">Pokud se nenajde žádný řetězec dotazu, nebo hodnotu řetězce dotazu není platný, žádný produkt, který se zobrazí na *ProductDetails.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="efba0-191">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="efba0-191">Running the Application</span></span>

<span data-ttu-id="efba0-192">Nyní můžete spustit aplikaci, abyste viděli zobrazí jednotlivé produkty na základě id produktu.</span><span class="sxs-lookup"><span data-stu-id="efba0-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="efba0-193">Stisknutím klávesy **F5** během činnosti v sadě Visual Studio ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="efba0-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="efba0-194">V prohlížeči se otevře a zobrazí *Default.aspx* stránky.</span><span class="sxs-lookup"><span data-stu-id="efba0-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="efba0-195">Z kategorie navigační nabídce vyberte "Lodě".</span><span class="sxs-lookup"><span data-stu-id="efba0-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="efba0-196">*ProductList.aspx* zobrazí se stránka.</span><span class="sxs-lookup"><span data-stu-id="efba0-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="efba0-197">Vyberte produkt "Papíru loď" ze seznamu produktů.</span><span class="sxs-lookup"><span data-stu-id="efba0-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="efba0-198">*ProductDetails.aspx* zobrazí se stránka.</span><span class="sxs-lookup"><span data-stu-id="efba0-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Zobrazení dat položek a podrobnosti - produkty](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="efba0-200">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="efba0-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="efba0-201">Souhrn</span><span class="sxs-lookup"><span data-stu-id="efba0-201">Summary</span></span>

<span data-ttu-id="efba0-202">V tomto kurzu této série mají přidání značek a kódu k zobrazení seznamu produktů a zobrazit podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="efba0-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="efba0-203">Během tohoto procesu jste se dozvěděli o ovládací prvky dat silného typu, vazby modelu a zprostředkovatele hodnot.</span><span class="sxs-lookup"><span data-stu-id="efba0-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="efba0-204">V dalším kurzu přidáte do nákupního košíku ukázkové aplikace Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="efba0-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="efba0-205">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="efba0-205">Additional Resources</span></span>

[<span data-ttu-id="efba0-206">Načtení a zobrazení dat ovládacím prvkem vazby modelu a webové formuláře</span><span class="sxs-lookup"><span data-stu-id="efba0-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="efba0-207">[Předchozí](ui_and_navigation.md)
> [další](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="efba0-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
