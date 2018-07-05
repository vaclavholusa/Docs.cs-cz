---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relace prvků v OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: MikeWasson
description: 'Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů. Použití protokolu OData, klienti se můžete dostat přes...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 56cadbabaa7ca64ba39232495e25178849d5e3c2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377643"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="0a317-104">Relace prvků v OData v4 pomocí rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="0a317-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="0a317-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0a317-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0a317-106">Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů.</span><span class="sxs-lookup"><span data-stu-id="0a317-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="0a317-107">Použití protokolu OData, klienti můžete přejít přes relací prvků.</span><span class="sxs-lookup"><span data-stu-id="0a317-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="0a317-108">Zadaný produkt, můžete najít dodavatele.</span><span class="sxs-lookup"><span data-stu-id="0a317-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="0a317-109">Můžete také vytvořit nebo odebrat relace.</span><span class="sxs-lookup"><span data-stu-id="0a317-109">You can also create or remove relationships.</span></span> <span data-ttu-id="0a317-110">Například můžete nastavit od dodavatele, produktu.</span><span class="sxs-lookup"><span data-stu-id="0a317-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="0a317-111">Tento kurz ukazuje, jak podporují tyto operace v OData v4, pomocí rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="0a317-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="0a317-112">Tento kurz vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="0a317-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0a317-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="0a317-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0a317-114">Webové rozhraní API 2.1</span><span class="sxs-lookup"><span data-stu-id="0a317-114">Web API 2.1</span></span>
> - <span data-ttu-id="0a317-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="0a317-115">OData v4</span></span>
> - [<span data-ttu-id="0a317-116">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="0a317-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="0a317-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="0a317-117">Entity Framework 6</span></span>
> - <span data-ttu-id="0a317-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0a317-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="0a317-119">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="0a317-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="0a317-120">OData verze 3, naleznete v tématu [podpora relací prvků v OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="0a317-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="0a317-121">Přidání Entity dodavatele</span><span class="sxs-lookup"><span data-stu-id="0a317-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="0a317-122">Tento kurz vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="0a317-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="0a317-123">Nejdřív potřebujeme související entity.</span><span class="sxs-lookup"><span data-stu-id="0a317-123">First, we need a related entity.</span></span> <span data-ttu-id="0a317-124">Přidejte třídu pojmenovanou `Supplier` ve složce modely.</span><span class="sxs-lookup"><span data-stu-id="0a317-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="0a317-125">Přidání navigační vlastnost, která má `Product` třídy:</span><span class="sxs-lookup"><span data-stu-id="0a317-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="0a317-126">Přidat nový **DbSet** k `ProductsContext` třídy, aby rozhraní Entity Framework obsahovala dodavatele tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="0a317-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="0a317-127">V WebApiConfig.cs, přidejte &quot;Dodavatelé&quot; sada entit k modelu entity data model:</span><span class="sxs-lookup"><span data-stu-id="0a317-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="0a317-128">Přidání Kontroleru dodavatelů</span><span class="sxs-lookup"><span data-stu-id="0a317-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="0a317-129">Přidat `SuppliersController` třídy do složky řadiče.</span><span class="sxs-lookup"><span data-stu-id="0a317-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="0a317-130">Můžu nebude ukazují, jak přidat operace CRUD pro tento kontroler.</span><span class="sxs-lookup"><span data-stu-id="0a317-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="0a317-131">Postup je stejný jako u kontroleru produktů (viz [vytvoření koncového bodu OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="0a317-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="0a317-132">Získat související entity</span><span class="sxs-lookup"><span data-stu-id="0a317-132">Getting Related Entities</span></span>

<span data-ttu-id="0a317-133">Pokud chcete získat od dodavatele pro produkt, klient odešle požadavek GET:</span><span class="sxs-lookup"><span data-stu-id="0a317-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="0a317-134">Chcete-li tuto žádost o podporu, přidejte následující metodu do `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="0a317-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="0a317-135">Tato metoda používá výchozí zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="0a317-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="0a317-136">Název metody: metody GetX, kde X je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a317-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="0a317-137">Název parametru: *klíč*</span><span class="sxs-lookup"><span data-stu-id="0a317-137">Parameter name: *key*</span></span>

<span data-ttu-id="0a317-138">Pokud budete postupovat podle této zásady vytváření názvů, webové rozhraní API automaticky mapují požadavku HTTP k metodě kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a317-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="0a317-139">Příklad HTTP žádosti:</span><span class="sxs-lookup"><span data-stu-id="0a317-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="0a317-140">Příklad HTTP odpovědi:</span><span class="sxs-lookup"><span data-stu-id="0a317-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="0a317-141">Získání související kolekce</span><span class="sxs-lookup"><span data-stu-id="0a317-141">Getting a related collection</span></span>

<span data-ttu-id="0a317-142">V předchozím příkladu má produkt jednoho dodavatele.</span><span class="sxs-lookup"><span data-stu-id="0a317-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="0a317-143">Navigační vlastnost také může vrátit kolekci.</span><span class="sxs-lookup"><span data-stu-id="0a317-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="0a317-144">Následující kód načte produktů pro dodavatele:</span><span class="sxs-lookup"><span data-stu-id="0a317-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="0a317-145">V takovém případě vrátí metoda **IQueryable** místo **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="0a317-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="0a317-146">Příklad HTTP žádosti:</span><span class="sxs-lookup"><span data-stu-id="0a317-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="0a317-147">Příklad HTTP odpovědi:</span><span class="sxs-lookup"><span data-stu-id="0a317-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="0a317-148">Vytvoření relace mezi entitami</span><span class="sxs-lookup"><span data-stu-id="0a317-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="0a317-149">OData podporuje vytváření nebo odebírání relací mezi dva existující entity.</span><span class="sxs-lookup"><span data-stu-id="0a317-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="0a317-150">V terminologii OData v4, je vztah &quot;odkaz&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a317-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="0a317-151">(V OData v3 relace byla volána *odkaz*.</span><span class="sxs-lookup"><span data-stu-id="0a317-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="0a317-152">Rozdíly protokolu na mezerách nezáleží pro účely tohoto kurzu.)</span><span class="sxs-lookup"><span data-stu-id="0a317-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="0a317-153">Odkaz má svůj vlastní identifikátor URI, že je formulář `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="0a317-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="0a317-154">Například tady je identifikátor URI odkazu mezi produktu a jeho dodavatele řešení:</span><span class="sxs-lookup"><span data-stu-id="0a317-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="0a317-155">Přidání relace, klient odešle požadavek POST a PUT na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="0a317-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="0a317-156">Pokud se vlastnost navigace, jako je jedna entita, `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="0a317-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="0a317-157">PŘÍSPĚVEK, pokud navigační vlastnost, jako je kolekce, `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="0a317-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="0a317-158">Text žádosti obsahuje identifikátor URI z druhé entity ve vztahu.</span><span class="sxs-lookup"><span data-stu-id="0a317-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="0a317-159">Tady je příklad žádosti:</span><span class="sxs-lookup"><span data-stu-id="0a317-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="0a317-160">V tomto příkladu, klient odešle požadavek PUT pro `/Products(6)/Supplier/$ref`, což je identifikátor URI $ref `Supplier` produkt s ID = 6.</span><span class="sxs-lookup"><span data-stu-id="0a317-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="0a317-161">Pokud neproběhne úspěšně, odešle server odezvu 204 (žádný obsah):</span><span class="sxs-lookup"><span data-stu-id="0a317-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="0a317-162">Tady je metoda kontroleru Přidat relaci `Product`:</span><span class="sxs-lookup"><span data-stu-id="0a317-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="0a317-163">*NavigationProperty* parametr určuje, které vztah k nastavení.</span><span class="sxs-lookup"><span data-stu-id="0a317-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="0a317-164">(Pokud existuje více než jednu vlastnost navigace u entity, můžete přidat další `case` příkazy.)</span><span class="sxs-lookup"><span data-stu-id="0a317-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="0a317-165">*Odkaz* parametr obsahuje identifikátor URI od dodavatele.</span><span class="sxs-lookup"><span data-stu-id="0a317-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="0a317-166">Webové rozhraní API automaticky analyzuje text požadavku má být získána hodnota tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="0a317-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="0a317-167">K vyhledání od dodavatele, potřebujeme ID (nebo klíče), který je součástí *odkaz* parametru.</span><span class="sxs-lookup"><span data-stu-id="0a317-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="0a317-168">Chcete-li to provést, použijte následující pomocnou metodu:</span><span class="sxs-lookup"><span data-stu-id="0a317-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="0a317-169">V podstatě tato metoda používá knihovnou OData rozdělit do segmentů cesty identifikátoru URI, najděte segment, který obsahuje klíč a převést na správný typ klíče.</span><span class="sxs-lookup"><span data-stu-id="0a317-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="0a317-170">Odstraňuje se relace mezi entitami</span><span class="sxs-lookup"><span data-stu-id="0a317-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="0a317-171">Odstranění relace, klient odešle požadavek HTTP DELETE na identifikátor URI $ref:</span><span class="sxs-lookup"><span data-stu-id="0a317-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="0a317-172">Tady je metoda kontroleru odstranit vztah mezi produktem a Dodavatel:</span><span class="sxs-lookup"><span data-stu-id="0a317-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="0a317-173">V takovém případě `Product.Supplier` je &quot;1&quot; konec vztah 1: n, takže vztahu můžete odebrat tak, že nastavení `Product.Supplier` k `null`.</span><span class="sxs-lookup"><span data-stu-id="0a317-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="0a317-174">V &quot;mnoho&quot; konec relace, klient musí určovat, která související entitu, kterou chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="0a317-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="0a317-175">Uděláte to tak, klient odešle v řetězci dotazu požadavku URI související entity.</span><span class="sxs-lookup"><span data-stu-id="0a317-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="0a317-176">Například chcete-li odebrat "Produktu 1" z "1" na dodavatele:</span><span class="sxs-lookup"><span data-stu-id="0a317-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="0a317-177">V rozhraní Web API z toho důvodu musíme zahrnout další parametr v `DeleteRef` metody.</span><span class="sxs-lookup"><span data-stu-id="0a317-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="0a317-178">Tady je metoda kontroleru odebrat produkt ze `Supplier.Products` vztah.</span><span class="sxs-lookup"><span data-stu-id="0a317-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="0a317-179">*Klíč* parametr je klíč pro dodavatele a *relatedKey* parametr je klíčem k produktu k odebrání `Products` vztah.</span><span class="sxs-lookup"><span data-stu-id="0a317-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="0a317-180">Všimněte si, že webové rozhraní API automaticky získá klíč z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="0a317-180">Note that Web API automatically gets the key from the query string.</span></span>
