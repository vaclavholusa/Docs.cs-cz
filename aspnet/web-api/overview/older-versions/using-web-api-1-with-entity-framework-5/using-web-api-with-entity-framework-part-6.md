---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Část 6: Vytvoření Kontrolerů produktů a objednávek | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752020"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="aedd0-102">Část 6: Vytvoření Kontrolerů produktů a objednávek</span><span class="sxs-lookup"><span data-stu-id="aedd0-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="aedd0-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aedd0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="aedd0-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="aedd0-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="aedd0-105">Přidání Kontroleru produkty</span><span class="sxs-lookup"><span data-stu-id="aedd0-105">Add a Products Controller</span></span>

<span data-ttu-id="aedd0-106">Kontroleru pro správce je pro uživatele, kteří mají oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="aedd0-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="aedd0-107">Zákazníci, na druhé straně může zobrazit produkty, ale nejde vytvořit, aktualizovat nebo je odstranit.</span><span class="sxs-lookup"><span data-stu-id="aedd0-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="aedd0-108">Jsme můžete snadno omezit přístup na metody Post, Put a Delete a ponechání otevřít metody Get.</span><span class="sxs-lookup"><span data-stu-id="aedd0-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="aedd0-109">Ale podívejte se na data, která je vrácena pro produkt:</span><span class="sxs-lookup"><span data-stu-id="aedd0-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="aedd0-110">`ActualCost` Vlastnosti by neměly být viditelné pro zákazníky!</span><span class="sxs-lookup"><span data-stu-id="aedd0-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="aedd0-111">Toto řešení je k definování *objekt pro přenos dat* (DTO), který obsahuje podmnožinu vlastností, které by měly být viditelné pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="aedd0-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="aedd0-112">Použijeme k projektu LINQ `Product` instance na `ProductDTO` instancí.</span><span class="sxs-lookup"><span data-stu-id="aedd0-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="aedd0-113">Přidejte třídu pojmenovanou `ProductDTO` ke složce modely.</span><span class="sxs-lookup"><span data-stu-id="aedd0-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="aedd0-114">Nyní přidejte kontroler.</span><span class="sxs-lookup"><span data-stu-id="aedd0-114">Now add the controller.</span></span> <span data-ttu-id="aedd0-115">V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="aedd0-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="aedd0-116">Vyberte **přidat**a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="aedd0-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="aedd0-117">V **přidat kontroler** dialogového okna, názvu kontroleru &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="aedd0-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="aedd0-118">V části **šablony**vyberte **kontroler API s prázdnou**.</span><span class="sxs-lookup"><span data-stu-id="aedd0-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="aedd0-119">Všechno, co ve zdrojovém souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="aedd0-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="aedd0-120">Kontroler stále používá `OrdersContext` dáte dotaz na databázi.</span><span class="sxs-lookup"><span data-stu-id="aedd0-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="aedd0-121">Ale místo vrácení `Product` instance přímo, říkáme `MapProducts` do projektu je na `ProductDTO` instancí:</span><span class="sxs-lookup"><span data-stu-id="aedd0-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="aedd0-122">`MapProducts` Vrátí metoda **IQueryable**, takže jsme tvoří výsledek s další parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="aedd0-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="aedd0-123">Vidíte to `GetProduct` metodu, která přidá **kde** klauzule dotazu:</span><span class="sxs-lookup"><span data-stu-id="aedd0-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="aedd0-124">Přidat kontroler Orders</span><span class="sxs-lookup"><span data-stu-id="aedd0-124">Add an Orders Controller</span></span>

<span data-ttu-id="aedd0-125">V dalším kroku přidáte kontroler, který umožňuje uživatelům vytvořit a zobrazit příkazy.</span><span class="sxs-lookup"><span data-stu-id="aedd0-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="aedd0-126">Začneme jiný objekt DTO.</span><span class="sxs-lookup"><span data-stu-id="aedd0-126">We'll start with another DTO.</span></span> <span data-ttu-id="aedd0-127">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a přidejte třídu pojmenovanou `OrderDTO` použijte následující implementaci:</span><span class="sxs-lookup"><span data-stu-id="aedd0-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="aedd0-128">Nyní přidejte kontroler.</span><span class="sxs-lookup"><span data-stu-id="aedd0-128">Now add the controller.</span></span> <span data-ttu-id="aedd0-129">V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="aedd0-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="aedd0-130">Vyberte **přidat**a pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="aedd0-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="aedd0-131">V **přidat kontroler** dialogové okno, nastavte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="aedd0-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="aedd0-132">V části **názvu Kontroleru**, zadejte "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="aedd0-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="aedd0-133">V části **šablony**vyberte "Kontroler API s akcemi čtení/zápisu, používá nástroj Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="aedd0-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="aedd0-134">V části **třída modelu**vyberte &quot;pořadí (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="aedd0-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="aedd0-135">V části **třída kontextu dat**vyberte &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="aedd0-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="aedd0-136">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="aedd0-136">Click **Add**.</span></span> <span data-ttu-id="aedd0-137">Tím se přidá soubor s názvem OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="aedd0-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="aedd0-138">Dále musíme upravit výchozí implementace kontroleru.</span><span class="sxs-lookup"><span data-stu-id="aedd0-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="aedd0-139">Nejprve odstraňte `PutOrder` a `DeleteOrder` metody.</span><span class="sxs-lookup"><span data-stu-id="aedd0-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="aedd0-140">V tomto příkladu zákazníkům nemůžete upravovat ani odstranit existující objednávky.</span><span class="sxs-lookup"><span data-stu-id="aedd0-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="aedd0-141">V reálné aplikaci je třeba velké množství back-end logiku pro tyto případy zpracují.</span><span class="sxs-lookup"><span data-stu-id="aedd0-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="aedd0-142">(Například byl pořadí již dodán?)</span><span class="sxs-lookup"><span data-stu-id="aedd0-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="aedd0-143">Změnit `GetOrders` metoda vrátí objednávky, které patří uživateli:</span><span class="sxs-lookup"><span data-stu-id="aedd0-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="aedd0-144">Změnit `GetOrder` metodu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="aedd0-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="aedd0-145">Tady jsou změny, které jsme provedli metodu:</span><span class="sxs-lookup"><span data-stu-id="aedd0-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="aedd0-146">Vrácená hodnota je `OrderDTO` instance, místo `Order`.</span><span class="sxs-lookup"><span data-stu-id="aedd0-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="aedd0-147">Když zadáme dotaz databázi pro objednávku, používáme [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metoda načíst související `OrderDetail` a `Product` entity.</span><span class="sxs-lookup"><span data-stu-id="aedd0-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="aedd0-148">Můžeme sloučit výsledek pomocí projekce.</span><span class="sxs-lookup"><span data-stu-id="aedd0-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="aedd0-149">Odpověď HTTP bude obsahovat pole s množství produktů:</span><span class="sxs-lookup"><span data-stu-id="aedd0-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="aedd0-150">Tento formát je jednodušší klienti využívat než původní graf objektu, který obsahuje vnořené entity (pořadí, podrobnosti a produkty).</span><span class="sxs-lookup"><span data-stu-id="aedd0-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="aedd0-151">Poslední metody, která považuje za `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="aedd0-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="aedd0-152">V tuto chvíli, tato metoda přebírá `Order` instance.</span><span class="sxs-lookup"><span data-stu-id="aedd0-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="aedd0-153">Ale zvažte, co se stane, pokud klient odešle textu žádosti podobně jako toto:</span><span class="sxs-lookup"><span data-stu-id="aedd0-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="aedd0-154">Toto je dobře strukturovaných pořadí a Entity Framework se využívá elastic vložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="aedd0-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="aedd0-155">Ale obsahuje entitu produktů, které dříve neexistoval.</span><span class="sxs-lookup"><span data-stu-id="aedd0-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="aedd0-156">Klient právě vytvořili nového produktu v databázi!</span><span class="sxs-lookup"><span data-stu-id="aedd0-156">The client just created a new product in our database!</span></span> <span data-ttu-id="aedd0-157">Bude jím neočekávaném pořadí jsou oddělení, když se jim objednávku medvídek nese.</span><span class="sxs-lookup"><span data-stu-id="aedd0-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="aedd0-158">Je ponaučení, buďte velmi opatrní při data, která můžete přijmout v požadavku POST a PUT.</span><span class="sxs-lookup"><span data-stu-id="aedd0-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="aedd0-159">Chcete-li tomuto problému vyhnout, změňte `PostOrder` metoda provést `OrderDTO` instance.</span><span class="sxs-lookup"><span data-stu-id="aedd0-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="aedd0-160">Použití `OrderDTO` vytvořit `Order`.</span><span class="sxs-lookup"><span data-stu-id="aedd0-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="aedd0-161">Všimněte si, že používáme `ProductID` a `Quantity` vlastnosti a My ignorovat všechny hodnoty, které klient zaslal pro název produktu nebo cenu.</span><span class="sxs-lookup"><span data-stu-id="aedd0-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="aedd0-162">Pokud ID produktu není platná, ji budou porušovat omezení cizího klíče v databázi a insert se nezdaří, jak by mělo.</span><span class="sxs-lookup"><span data-stu-id="aedd0-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="aedd0-163">Tady je úplný `PostOrder` metody:</span><span class="sxs-lookup"><span data-stu-id="aedd0-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="aedd0-164">Nakonec přidejte **Authorize** atribut kontroleru:</span><span class="sxs-lookup"><span data-stu-id="aedd0-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="aedd0-165">Jenom registrovaní uživatelé nyní můžete vytvořit nebo zobrazení objednávek.</span><span class="sxs-lookup"><span data-stu-id="aedd0-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aedd0-166">[Předchozí](using-web-api-with-entity-framework-part-5.md)
> [další](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="aedd0-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
