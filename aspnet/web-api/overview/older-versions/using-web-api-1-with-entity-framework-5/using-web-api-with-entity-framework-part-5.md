---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Část 5: Vytvoření dynamické uživatelského rozhraní s Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3e728-102">Část 5: Vytvoření dynamické uživatelského rozhraní s Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3e728-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="3e728-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3e728-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3e728-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="3e728-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3e728-105">Vytvoření s Knockout.js dynamické uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3e728-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="3e728-106">V této části použijeme Knockout.js k přidání funkcí do zobrazení správce.</span><span class="sxs-lookup"><span data-stu-id="3e728-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="3e728-107">[Knockout.js](http://knockoutjs.com/) je knihovna jazyka Javascript, který usnadňuje vytvoření vazby ovládacích prvků HTML k datům.</span><span class="sxs-lookup"><span data-stu-id="3e728-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="3e728-108">Knockout.js využívá vzor Model-View-ViewModel (modelem MVVM).</span><span class="sxs-lookup"><span data-stu-id="3e728-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="3e728-109">*Modelu* je serverové reprezentace dat v doméně obchodní (v našem případu, produkty a objednávky).</span><span class="sxs-lookup"><span data-stu-id="3e728-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="3e728-110">*Zobrazení* je prezentační vrstvy (HTML).</span><span class="sxs-lookup"><span data-stu-id="3e728-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="3e728-111">*Modelu zobrazení* se objekt jazyka Javascript, který obsahuje data modelu.</span><span class="sxs-lookup"><span data-stu-id="3e728-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="3e728-112">Model zobrazení je abstrakce kód uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3e728-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="3e728-113">Nemá žádné znalosti reprezentace HTML.</span><span class="sxs-lookup"><span data-stu-id="3e728-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="3e728-114">Místo toho představuje abstraktní funkce zobrazení, jako je například "seznam položek".</span><span class="sxs-lookup"><span data-stu-id="3e728-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="3e728-115">Zobrazení je vázané na data do modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3e728-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="3e728-116">Aktualizace do modelu zobrazení, se automaticky promítnou v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3e728-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="3e728-117">Model zobrazení také získá události ze zobrazení, jako je například kliknutí na tlačítko a provádí operace v modelu, například vytváření pořadí.</span><span class="sxs-lookup"><span data-stu-id="3e728-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="3e728-118">Nejprve budeme definovat model zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3e728-118">First we'll define the view-model.</span></span> <span data-ttu-id="3e728-119">Potom jsme vytvoří vazbu kód HTML zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="3e728-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="3e728-120">Přidejte následující části Razor Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="3e728-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="3e728-121">V této části můžete přidat libovolné místo v souboru.</span><span class="sxs-lookup"><span data-stu-id="3e728-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="3e728-122">Když je zobrazení vykresleno, v dolní části stránky HTML, zobrazí se v části pravým před uzavírací &lt;/body&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="3e728-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="3e728-123">Všechny skriptu pro tuto stránku přejde uvnitř značky script indikován komentář:</span><span class="sxs-lookup"><span data-stu-id="3e728-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="3e728-124">Nejprve definujte třídu modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="3e728-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="3e728-125">**ko.observableArray** je zvláštní druh objektu v Knockout, volána *lze zobrazit*.</span><span class="sxs-lookup"><span data-stu-id="3e728-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="3e728-126">Z [Knockout.js dokumentace](http://knockoutjs.com/documentation/observables.html): existuje zjištěný je "JavaScript objekt, který může upozornit odběratele o změnách."</span><span class="sxs-lookup"><span data-stu-id="3e728-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="3e728-127">Při změně obsahu existuje zjištěný, se automaticky aktualizuje zobrazení tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="3e728-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="3e728-128">K naplnění `products` pole, je požadavek AJAX webovému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3e728-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="3e728-129">Odvolat jsme uložené základní identifikátor URI pro rozhraní API v kontejner zobrazení (viz [část 4](using-web-api-with-entity-framework-part-4.md) kurzu).</span><span class="sxs-lookup"><span data-stu-id="3e728-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="3e728-130">Dál přidejte funkce do modelu zobrazení vytvářet, aktualizovat a odstraňovat produkty.</span><span class="sxs-lookup"><span data-stu-id="3e728-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="3e728-131">Tyto funkce odeslání volání AJAX webovému rozhraní API a aktualizovat model zobrazení pomocí výsledky.</span><span class="sxs-lookup"><span data-stu-id="3e728-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="3e728-132">Nyní nejdůležitější část: Pokud modelu DOM je fulled načíst, volání **ko.applyBindings** funkce a předávat novou instanci třídy `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="3e728-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="3e728-133">**Ko.applyBindings** metoda aktivuje Knockout a sváže model zobrazení do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3e728-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="3e728-134">Teď, když máme modelu zobrazení, můžeme vytvořit vazby.</span><span class="sxs-lookup"><span data-stu-id="3e728-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="3e728-135">V Knockout.js, to uděláte tak, že přidáte `data-bind` atributy prvků HTML.</span><span class="sxs-lookup"><span data-stu-id="3e728-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="3e728-136">Například do pole vytvořit vazbu seznamu HTML, použijte `foreach` vazby:</span><span class="sxs-lookup"><span data-stu-id="3e728-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="3e728-137">`foreach` Vazba iteruje v rámci pole a vytvoří podřízené elementy pro každý objekt v poli.</span><span class="sxs-lookup"><span data-stu-id="3e728-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="3e728-138">Vazby na podřízené elementy mohou odkazovat na vlastnosti na pole objektů.</span><span class="sxs-lookup"><span data-stu-id="3e728-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="3e728-139">Přidejte následující vazby do seznamu "aktualizace produkty":</span><span class="sxs-lookup"><span data-stu-id="3e728-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="3e728-140">`<li>` Element dochází v rámci oboru **foreach** vazby.</span><span class="sxs-lookup"><span data-stu-id="3e728-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="3e728-141">Aby znamená Knockout bude vykreslení elementu jednou pro každý produkt v `products` pole.</span><span class="sxs-lookup"><span data-stu-id="3e728-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="3e728-142">Všechny vazby v rámci `<li>` element odkazovat na tuto instanci produktu.</span><span class="sxs-lookup"><span data-stu-id="3e728-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="3e728-143">Například `$data.Name` odkazuje `Name` vlastnost na produktu.</span><span class="sxs-lookup"><span data-stu-id="3e728-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="3e728-144">Pokud chcete nastavit hodnoty vstupy text, použijte `value` vazby.</span><span class="sxs-lookup"><span data-stu-id="3e728-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="3e728-145">Tlačítka je vázána na funkce na zobrazení modelu pomocí `click` vazby.</span><span class="sxs-lookup"><span data-stu-id="3e728-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="3e728-146">Instance produktu je jako parametr předaný jednotlivé funkce.</span><span class="sxs-lookup"><span data-stu-id="3e728-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="3e728-147">Další informace najdete [Knockout.js dokumentace](http://knockoutjs.com/documentation/observables.html) má dobrou popisy různých vazby.</span><span class="sxs-lookup"><span data-stu-id="3e728-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="3e728-148">V dalším kroku přidat vazbu pro **odeslání** událostí na formuláři přidat produkt:</span><span class="sxs-lookup"><span data-stu-id="3e728-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="3e728-149">Volá tuto vazbu `create` funkce na zobrazení modelu k vytvoření nového produktu.</span><span class="sxs-lookup"><span data-stu-id="3e728-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="3e728-150">Tady je kompletní kód pro zobrazení správce:</span><span class="sxs-lookup"><span data-stu-id="3e728-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="3e728-151">Spusťte aplikaci, přihlaste se pomocí účtu správce a klikněte na odkaz "Admin".</span><span class="sxs-lookup"><span data-stu-id="3e728-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="3e728-152">By měl zobrazit seznam produktů a moct vytvářet, aktualizovat nebo odstranit produkty.</span><span class="sxs-lookup"><span data-stu-id="3e728-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e728-153">[Předchozí](using-web-api-with-entity-framework-part-4.md)
> [další](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="3e728-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
