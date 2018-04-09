---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Část 7: Vytvoření hlavní stránky | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="a7ea2-102">Část 7: Vytvoření hlavní stránky</span><span class="sxs-lookup"><span data-stu-id="a7ea2-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="a7ea2-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a7ea2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a7ea2-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="a7ea2-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="a7ea2-105">Vytváření hlavní stránky</span><span class="sxs-lookup"><span data-stu-id="a7ea2-105">Creating the Main Page</span></span>

<span data-ttu-id="a7ea2-106">V této části vytvoříte stránky hlavní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="a7ea2-107">Tato stránka bude složitější než stránky pro správu, proto jsme budete postupovat v několika krocích.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="a7ea2-108">Na této cestě se zobrazí některé pokročilejší Knockout.js techniky.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="a7ea2-109">Tady je základní rozložení stránky:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="a7ea2-110">"Produkty" obsahuje řadu produktů.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="a7ea2-111">"Košíku" obsahuje řadu produktů s počty.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="a7ea2-112">Kliknutím na tlačítko "Přidat do košíku" aktualizujete košíku.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="a7ea2-113">"Objednávky" obsahuje pole pořadí ID.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="a7ea2-114">"Informace" obsahuje podrobnosti o pořadí, což je pole položky (produkty s počty)</span><span class="sxs-lookup"><span data-stu-id="a7ea2-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="a7ea2-115">Začneme definováním některé základní rozložení ve formátu HTML, bez vazby dat nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="a7ea2-116">Otevřete soubor Views/Home/Index.cshtml a veškerý obsah, nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="a7ea2-117">V dalším kroku přidat oddíl skripty a vytvořit prázdný model zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="a7ea2-118">Podle návrhu šrafují dříve, našeho modelu zobrazení musí pozorovatelné objekty pro produkty, košíku, objednávek a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="a7ea2-119">Přidejte následující proměnné na `AppViewModel` objektu:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="a7ea2-120">Uživatele můžete přidat do košíku položky ze seznamu produktů a odebrání položek z košíku.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="a7ea2-121">K zapouzdření tyto funkce, vytvoříme jiné třídy modelu zobrazení, která představuje produkt.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="a7ea2-122">Přidejte následující kód, který `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="a7ea2-123">`ProductViewModel` Třída obsahuje dvě funkce, které se používají k přesun produktu do a z košíku: `addItemToCart` přidá do košíku, jednu jednotku produktu a `removeAllFromCart` odebere všechny počty produktu.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="a7ea2-124">Uživatele můžete vybrat existující pořadí a získat podrobnosti o pořadí.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="a7ea2-125">Zapouzdříme tuto funkci do jiného modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="a7ea2-126">`OrderDetailsViewModel` Inicializován s pořadí, a podrobnosti o pořadí ho načte odesláním požadavek AJAX na server.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="a7ea2-127">Všimněte si také, `total` vlastnost `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="a7ea2-128">Tato vlastnost je zvláštní druh názvem lze zobrazit [počítaný lze zobrazit](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="a7ea2-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="a7ea2-129">Jak již název napovídá, počítaný lze zobrazit umožňuje vazbě dat na vypočtenou hodnotou&#8212;v tomto případě celkové náklady pořadí.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="a7ea2-130">Dál přidejte tyto funkce `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="a7ea2-131">`resetCart` Odebere všechny položky z košíku.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="a7ea2-132">`getDetails` Získá podrobnosti pro pořadí (podle pusing a nové `OrderDetailsViewModel` na `details` seznamu).</span><span class="sxs-lookup"><span data-stu-id="a7ea2-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="a7ea2-133">`createOrder` Vytvoří nové pořadí a vyprázdní košíku.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="a7ea2-134">Nakonec inicializovat model zobrazení tak, že požadavky AJAX pro produkty a objednávky:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="a7ea2-135">OK velké množství kódu, který je ale jsme je sestavena si krok za krokem, takže zpravidla návrhu je zrušte.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="a7ea2-136">Nyní můžete přidáme některé Knockout.js vazby v kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="a7ea2-137">**Produkty**</span><span class="sxs-lookup"><span data-stu-id="a7ea2-137">**Products**</span></span>

<span data-ttu-id="a7ea2-138">Zde jsou vazby u seznamu produktu:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="a7ea2-139">To iteruje nad pole produkty a zobrazuje název a ceny.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="a7ea2-140">Tlačítko "Přidat do pořadí" je viditelná jenom v případě, že uživatel je přihlášen.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="a7ea2-141">Volání tlačítko "Přidat do pořadí" `addItemToCart` na `ProductViewModel` instance pro produkt.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="a7ea2-142">Tento příklad ukazuje dobrý funkce Knockout.js: když model zobrazení obsahuje jinými modely zobrazení, můžete použít vazby pro vnitřní modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="a7ea2-143">V tomto příkladu vazby v rámci `foreach` jsou použitá pro každé z `ProductViewModel` instance.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="a7ea2-144">Tento přístup je mnohem čisticí než uvedení všechny funkce do jednoho zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="a7ea2-145">**Košíku**</span><span class="sxs-lookup"><span data-stu-id="a7ea2-145">**Cart**</span></span>

<span data-ttu-id="a7ea2-146">Zde jsou vazby u košíku:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="a7ea2-147">To iteruje nad pole košíku a zobrazí název, ceny a množství.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="a7ea2-148">Všimněte si, že tlačítko "Vytvořit pořadí" a "Odebrat" odkaz je vázána na modelu zobrazení funkce.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="a7ea2-149">**Objednávky**</span><span class="sxs-lookup"><span data-stu-id="a7ea2-149">**Orders**</span></span>

<span data-ttu-id="a7ea2-150">Zde jsou vazby u seznamu objednávky:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="a7ea2-151">To iteruje nad objednávky a zobrazuje ID pořadí.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="a7ea2-152">Událost kliknutím na odkaz je vázána `getDetails` funkce.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="a7ea2-153">**Podrobnosti o objednávce**</span><span class="sxs-lookup"><span data-stu-id="a7ea2-153">**Order Details**</span></span>

<span data-ttu-id="a7ea2-154">Zde jsou vazby podrobnosti o pořadí:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="a7ea2-155">To iteruje nad položky v pořadí a zobrazí produktu, ceny a množství.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="a7ea2-156">Okolního div je viditelná pouze v případě, že pole podrobností obsahuje jednu nebo více položek.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a7ea2-157">Závěr</span><span class="sxs-lookup"><span data-stu-id="a7ea2-157">Conclusion</span></span>

<span data-ttu-id="a7ea2-158">V tomto kurzu jste vytvořili aplikaci, která používá rozhraní Entity Framework pro komunikaci s databáze a webového rozhraní API ASP.NET zajistit veřejné rozhraní na datové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="a7ea2-159">Používáme k vykreslení stránky HTML a Knockout.js a jQuery zajistit dynamické interakce bez zavážky stránky ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a7ea2-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="a7ea2-160">Další zdroje informací:</span><span class="sxs-lookup"><span data-stu-id="a7ea2-160">Additional resources:</span></span>

- [<span data-ttu-id="a7ea2-161">Mapa obsahu přístupu k Data technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a7ea2-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="a7ea2-162">Středisko pro vývojáře Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a7ea2-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="a7ea2-163">Předchozí</span><span class="sxs-lookup"><span data-stu-id="a7ea2-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
