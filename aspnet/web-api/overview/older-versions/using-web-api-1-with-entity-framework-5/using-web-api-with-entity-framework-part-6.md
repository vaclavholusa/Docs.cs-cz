---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Část 6: Vytvoření produktu a pořadí řadiče | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869384"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="46691-102">Část 6: Vytvoření produktu a pořadí řadiče</span><span class="sxs-lookup"><span data-stu-id="46691-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="46691-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="46691-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="46691-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="46691-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="46691-105">Přidat řadič produkty</span><span class="sxs-lookup"><span data-stu-id="46691-105">Add a Products Controller</span></span>

<span data-ttu-id="46691-106">Správce řadiče je pro uživatele, kteří mají oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="46691-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="46691-107">Zákazníci, na druhé straně, můžete zobrazit produkty, ale nelze vytvořit, aktualizovat nebo je odstranit.</span><span class="sxs-lookup"><span data-stu-id="46691-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="46691-108">Jsme můžete snadno omezit přístup pro metodu Post, Put a Delete a nechat otevřené metody Get.</span><span class="sxs-lookup"><span data-stu-id="46691-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="46691-109">Ale podívejte se na data, která je vrácena produktu:</span><span class="sxs-lookup"><span data-stu-id="46691-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="46691-110">`ActualCost` Vlastnosti by neměly být viditelné zákazníkům!</span><span class="sxs-lookup"><span data-stu-id="46691-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="46691-111">Řešení je k definování *objekt pro přenos dat* (DTO), který obsahuje podmnožinu vlastností, které by měly být viditelné pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="46691-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="46691-112">Použijeme k projektu LINQ `Product` instance k `ProductDTO` instance.</span><span class="sxs-lookup"><span data-stu-id="46691-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="46691-113">Přidejte třídu s názvem `ProductDTO` ke složce modelů.</span><span class="sxs-lookup"><span data-stu-id="46691-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="46691-114">Nyní přidejte kontroleru.</span><span class="sxs-lookup"><span data-stu-id="46691-114">Now add the controller.</span></span> <span data-ttu-id="46691-115">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="46691-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="46691-116">Vyberte **přidat**, pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="46691-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="46691-117">V **přidat kontroler** dialogové okno, názvu kontroleru &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="46691-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="46691-118">V části **šablony**, vyberte **kontroler API prázdný**.</span><span class="sxs-lookup"><span data-stu-id="46691-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="46691-119">Všechno, co ve zdrojovém souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="46691-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="46691-120">Řadičem stále používá `OrdersContext` dotazu databázi.</span><span class="sxs-lookup"><span data-stu-id="46691-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="46691-121">Ale namísto vrácení `Product` instance přímo, říkáme `MapProducts` do projektu je do `ProductDTO` instancí:</span><span class="sxs-lookup"><span data-stu-id="46691-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="46691-122">`MapProducts` Metoda vrátí **IQueryable**, takže jsme můžete vytvořit výsledek s jinými parametry.</span><span class="sxs-lookup"><span data-stu-id="46691-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="46691-123">Zobrazí se v `GetProduct` metodu, která přidá **kde** klauzule dotazu:</span><span class="sxs-lookup"><span data-stu-id="46691-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="46691-124">Přidat řadič objednávky</span><span class="sxs-lookup"><span data-stu-id="46691-124">Add an Orders Controller</span></span>

<span data-ttu-id="46691-125">V dalším kroku přidáte kontroler, který umožňuje uživatelům vytvářet a prohlížet objednávky.</span><span class="sxs-lookup"><span data-stu-id="46691-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="46691-126">Začneme s jinou DTO.</span><span class="sxs-lookup"><span data-stu-id="46691-126">We'll start with another DTO.</span></span> <span data-ttu-id="46691-127">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a přidejte třídu s názvem `OrderDTO` použijte následující implementace:</span><span class="sxs-lookup"><span data-stu-id="46691-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="46691-128">Nyní přidejte kontroleru.</span><span class="sxs-lookup"><span data-stu-id="46691-128">Now add the controller.</span></span> <span data-ttu-id="46691-129">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="46691-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="46691-130">Vyberte **přidat**, pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="46691-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="46691-131">V **přidat kontroler** dialogové okno, nastavte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="46691-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="46691-132">V části **názvu Kontroleru**, zadejte "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="46691-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="46691-133">V části **šablony**, vyberte možnost "Kontroler API s akcemi čtení/zápisu používající rozhraní Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="46691-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="46691-134">V části **třída modelu**, vyberte &quot;pořadí (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="46691-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="46691-135">V části **třída kontextu dat**, vyberte &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="46691-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="46691-136">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="46691-136">Click **Add**.</span></span> <span data-ttu-id="46691-137">Tento postup přidá soubor s názvem OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="46691-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="46691-138">Dále je potřeba upravit výchozí implementaci třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="46691-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="46691-139">Nejprve odstraňte `PutOrder` a `DeleteOrder` metody.</span><span class="sxs-lookup"><span data-stu-id="46691-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="46691-140">Tato ukázka zákazníci nelze upravit nebo odstranit existující objednávky.</span><span class="sxs-lookup"><span data-stu-id="46691-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="46691-141">V reálné aplikaci bude třeba velké množství logiku back-end pro tyto případy zpracují.</span><span class="sxs-lookup"><span data-stu-id="46691-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="46691-142">(Například byl pořadí již dodán?)</span><span class="sxs-lookup"><span data-stu-id="46691-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="46691-143">Změna `GetOrders` metoda vrátí objednávky, které patří uživateli:</span><span class="sxs-lookup"><span data-stu-id="46691-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="46691-144">Změna `GetOrder` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="46691-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="46691-145">Zde jsou změny, které jsme provedli metody:</span><span class="sxs-lookup"><span data-stu-id="46691-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="46691-146">Vrácená hodnota je `OrderDTO` instance, místo `Order`.</span><span class="sxs-lookup"><span data-stu-id="46691-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="46691-147">Když jsme dotazování databáze pro pořadí, používáme [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metoda načíst související `OrderDetail` a `Product` entity.</span><span class="sxs-lookup"><span data-stu-id="46691-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="46691-148">Výsledek jsme vyrovnání pomocí projekce.</span><span class="sxs-lookup"><span data-stu-id="46691-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="46691-149">Odpověď HTTP bude obsahovat pole produkty s počty:</span><span class="sxs-lookup"><span data-stu-id="46691-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="46691-150">Tento formát je jednodušší, aby klienti mohli využívat než původní grafu objektu, která obsahuje vnořené entity (pořadí, podrobnosti a produkty).</span><span class="sxs-lookup"><span data-stu-id="46691-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="46691-151">Poslední metoda ji zvažovat `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="46691-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="46691-152">Teď, tato metoda přebírá `Order` instance.</span><span class="sxs-lookup"><span data-stu-id="46691-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="46691-153">Ale zvažte, co se stane, pokud klient pošle obsah žádosti takto:</span><span class="sxs-lookup"><span data-stu-id="46691-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="46691-154">Toto je dobře strukturovaných zakázky a Entity Framework se brouka vložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="46691-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="46691-155">Ale obsahuje entity produktu, který dříve neexistoval.</span><span class="sxs-lookup"><span data-stu-id="46691-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="46691-156">Klient právě vytvořili nového produktu v naší databázi!</span><span class="sxs-lookup"><span data-stu-id="46691-156">The client just created a new product in our database!</span></span> <span data-ttu-id="46691-157">Bude jím neočekávaném pořadí jsou oddělení, když se jim pořadí pro medvídek nese.</span><span class="sxs-lookup"><span data-stu-id="46691-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="46691-158">Je ponaučení, skutečně opatrně data, která je přijmout v požadavku POST nebo PUT.</span><span class="sxs-lookup"><span data-stu-id="46691-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="46691-159">Chcete-li se tomuto problému vyhnout, změňte `PostOrder` metoda provést `OrderDTO` instance.</span><span class="sxs-lookup"><span data-stu-id="46691-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="46691-160">Použití `OrderDTO` vytvořit `Order`.</span><span class="sxs-lookup"><span data-stu-id="46691-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="46691-161">Všimněte si, že používáme `ProductID` a `Quantity` vlastnosti a My ignorujte všechny hodnoty, které klient zaslal pro název produktu nebo cena.</span><span class="sxs-lookup"><span data-stu-id="46691-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="46691-162">Pokud není platné ID produktu, nesplňují omezení cizího klíče v databázi a úlohy insert se nezdaří, jako by měl.</span><span class="sxs-lookup"><span data-stu-id="46691-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="46691-163">Tady je kompletní `PostOrder` metoda:</span><span class="sxs-lookup"><span data-stu-id="46691-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="46691-164">Nakonec přidejte **Autorizovat** atribut kontroleru:</span><span class="sxs-lookup"><span data-stu-id="46691-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="46691-165">Jenom registrovaní uživatelé nyní můžete vytvořit nebo zobrazit objednávky.</span><span class="sxs-lookup"><span data-stu-id="46691-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="46691-166">[Předchozí](using-web-api-with-entity-framework-part-5.md)
> [další](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="46691-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
