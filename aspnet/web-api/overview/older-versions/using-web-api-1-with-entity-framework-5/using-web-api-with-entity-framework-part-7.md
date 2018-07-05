---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Část 7: Vytvoření hlavní stránky | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: b748813046637b9972bc96e22c35d2eeb9b57f9d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378894"
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="72276-102">Část 7: Vytvoření hlavní stránky</span><span class="sxs-lookup"><span data-stu-id="72276-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="72276-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="72276-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="72276-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="72276-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="72276-105">Vytvoření hlavní stránky</span><span class="sxs-lookup"><span data-stu-id="72276-105">Creating the Main Page</span></span>

<span data-ttu-id="72276-106">V této části vytvoříte stránky hlavní aplikace.</span><span class="sxs-lookup"><span data-stu-id="72276-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="72276-107">Tato stránka bude složitější než na stránce správy, takže jsme budete postupovat v několika krocích.</span><span class="sxs-lookup"><span data-stu-id="72276-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="72276-108">Na cestě zobrazí se vám některé pokročilé techniky knihovnou Knockout.js.</span><span class="sxs-lookup"><span data-stu-id="72276-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="72276-109">Tady je základní rozložení stránky:</span><span class="sxs-lookup"><span data-stu-id="72276-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="72276-110">"Produkty" obsahuje celou řadu produktů.</span><span class="sxs-lookup"><span data-stu-id="72276-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="72276-111">"Košíku" obsahuje celou řadu produktů s množství.</span><span class="sxs-lookup"><span data-stu-id="72276-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="72276-112">Kliknutím na tlačítko "Přidat do košíku" aktualizujete košíku.</span><span class="sxs-lookup"><span data-stu-id="72276-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="72276-113">"Orders" obsahuje pole ID objednávek.</span><span class="sxs-lookup"><span data-stu-id="72276-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="72276-114">"Details" obsahuje detaily objednávky, což je pole položky (produkty s množství)</span><span class="sxs-lookup"><span data-stu-id="72276-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="72276-115">Začneme tak, že definujete některé základní rozložení ve formátu HTML, bez vytváření datových vazeb nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="72276-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="72276-116">Otevřete soubor Views/Home/Index.cshtml a veškerý obsah nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="72276-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="72276-117">V dalším kroku přidejte oddíl skripty a vytvořte prázdný model zobrazení:</span><span class="sxs-lookup"><span data-stu-id="72276-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="72276-118">Podle návrhu šrafují dříve, náš model zobrazení musí pozorovatelné objekty pro produkty, košík, objednávek a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="72276-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="72276-119">Přidejte následující proměnné tak, `AppViewModel` objektu:</span><span class="sxs-lookup"><span data-stu-id="72276-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="72276-120">Uživatele můžete přidat položky ze seznamu produktů do košíku a odebrání položek z košíku.</span><span class="sxs-lookup"><span data-stu-id="72276-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="72276-121">K zapouzdření tyto funkce, vytvoříme jiné třídy zobrazení modelu, který představuje produkt.</span><span class="sxs-lookup"><span data-stu-id="72276-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="72276-122">Přidejte následující kód, který `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="72276-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="72276-123">`ProductViewModel` Třída obsahuje dvě funkce, které se používají k přesunu produktu do a z košíku: `addItemToCart` přidá jednu jednotku produktu do nákupního košíku, a `removeAllFromCart` odebere všechny množství produktu.</span><span class="sxs-lookup"><span data-stu-id="72276-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="72276-124">Uživatele můžete vybrat existující pořadí a získat podrobnosti objednávky.</span><span class="sxs-lookup"><span data-stu-id="72276-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="72276-125">Zapouzdříme tuto funkci do jiného zobrazení modelu:</span><span class="sxs-lookup"><span data-stu-id="72276-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="72276-126">`OrderDetailsViewModel` Je inicializován s objednávkou, a načte podrobnosti objednávky odesláním požadavku AJAX na server.</span><span class="sxs-lookup"><span data-stu-id="72276-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="72276-127">Všimněte si také, `total` vlastnost `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="72276-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="72276-128">Tato vlastnost je zvláštní druh pozorovat volána [vypočítané pozorovat](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="72276-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="72276-129">Jak již název napovídá, počítaný pozorovat vám umožní vytvořit datovou vazbu s vypočtenou hodnotou&#8212;v tomto případě celkové náklady na pořadí.</span><span class="sxs-lookup"><span data-stu-id="72276-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="72276-130">V dalším kroku přidejte tyto funkce k `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="72276-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="72276-131">`resetCart` Odebere všechny položky z košíku.</span><span class="sxs-lookup"><span data-stu-id="72276-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="72276-132">`getDetails` načte podrobnosti objednávky (podle pusing nový `OrderDetailsViewModel` na `details` seznamu).</span><span class="sxs-lookup"><span data-stu-id="72276-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="72276-133">`createOrder` Vytvoří novou objednávku a vyprázdní košíku.</span><span class="sxs-lookup"><span data-stu-id="72276-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="72276-134">Nakonec inicializujte model zobrazení tím, že odesílání požadavků AJAX pro produktů a objednávek:</span><span class="sxs-lookup"><span data-stu-id="72276-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="72276-135">Tak dobře, to je spousta kódu, ale sestavili jsme ji si krok za krokem, takže snad návrhu je jasné.</span><span class="sxs-lookup"><span data-stu-id="72276-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="72276-136">Teď můžeme přidat některé knihovnou Knockout.js vazby v kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="72276-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="72276-137">**Produkty**</span><span class="sxs-lookup"><span data-stu-id="72276-137">**Products**</span></span>

<span data-ttu-id="72276-138">Tady jsou vazby pro seznam produktů:</span><span class="sxs-lookup"><span data-stu-id="72276-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="72276-139">To Iteruje přes pole produkty a zobrazí název a ceny.</span><span class="sxs-lookup"><span data-stu-id="72276-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="72276-140">Tlačítko "Přidat do pořadí" je viditelná pouze v případě, že uživatel je přihlášen.</span><span class="sxs-lookup"><span data-stu-id="72276-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="72276-141">Volání tlačítko "Přidat do pořadí" `addItemToCart` na `ProductViewModel` instance pro produkt.</span><span class="sxs-lookup"><span data-stu-id="72276-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="72276-142">Tento příklad ukazuje užitečnou funkci z rozhraní Knockout.js: při zobrazení modelu obsahuje jiné Zobrazit modely, můžete použít vazby vnitřní modelu.</span><span class="sxs-lookup"><span data-stu-id="72276-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="72276-143">V tomto příkladu vazby v rámci `foreach` se použijí u všech `ProductViewModel` instancí.</span><span class="sxs-lookup"><span data-stu-id="72276-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="72276-144">Tento přístup je mnohem přehlednější, než všechny funkce vložení do jednoho modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="72276-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="72276-145">**Košíku**</span><span class="sxs-lookup"><span data-stu-id="72276-145">**Cart**</span></span>

<span data-ttu-id="72276-146">Tady jsou vazby pro košíku:</span><span class="sxs-lookup"><span data-stu-id="72276-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="72276-147">To Iteruje přes pole košíku a zobrazí název, cena a množství.</span><span class="sxs-lookup"><span data-stu-id="72276-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="72276-148">Všimněte si, že odkaz "Odebrat" a "Vytvoření objednávky" tlačítko jsou vázány na model zobrazení funkce.</span><span class="sxs-lookup"><span data-stu-id="72276-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="72276-149">**Objednávky**</span><span class="sxs-lookup"><span data-stu-id="72276-149">**Orders**</span></span>

<span data-ttu-id="72276-150">Tady jsou vazby u seznamu objednávek:</span><span class="sxs-lookup"><span data-stu-id="72276-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="72276-151">To Iteruje přes objednávky a zobrazuje ID objednávky.</span><span class="sxs-lookup"><span data-stu-id="72276-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="72276-152">Událost kliknutím na odkaz je vázána `getDetails` funkce.</span><span class="sxs-lookup"><span data-stu-id="72276-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="72276-153">**Podrobnosti objednávky**</span><span class="sxs-lookup"><span data-stu-id="72276-153">**Order Details**</span></span>

<span data-ttu-id="72276-154">Tady jsou vazby pro podrobnosti objednávky:</span><span class="sxs-lookup"><span data-stu-id="72276-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="72276-155">To iteruje položky v pořadí a zobrazí produktu, cena a množství.</span><span class="sxs-lookup"><span data-stu-id="72276-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="72276-156">Okolní div je viditelná pouze v případě, že pole Podrobnosti obsahuje jednu nebo více položek.</span><span class="sxs-lookup"><span data-stu-id="72276-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="72276-157">Závěr</span><span class="sxs-lookup"><span data-stu-id="72276-157">Conclusion</span></span>

<span data-ttu-id="72276-158">V tomto kurzu jste vytvořili aplikaci, která používá Entity Framework pro komunikaci s databází a ASP.NET Web API a poskytuje veřejná rozhraní na datové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="72276-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="72276-159">ASP.NET MVC 4 používáme k vykreslení stránky HTML a knihovnou Knockout.js plus jQuery zajistit dynamické interakce bez opětovné načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="72276-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="72276-160">Další zdroje informací:</span><span class="sxs-lookup"><span data-stu-id="72276-160">Additional resources:</span></span>

- [<span data-ttu-id="72276-161">Mapa obsahu přístupu k datům v technologii ASP.NET</span><span class="sxs-lookup"><span data-stu-id="72276-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="72276-162">Středisko pro vývojáře Entity Framework</span><span class="sxs-lookup"><span data-stu-id="72276-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="72276-163">Předchozí</span><span class="sxs-lookup"><span data-stu-id="72276-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
