---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "Vztahy entit v OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Microsoft Docs"
author: MikeWasson
description: "Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů. Použití protokolu OData, klienti můžete přejít přes..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="8e5de-104">Vztahy entit v OData v4 pomocí rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="8e5de-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="8e5de-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8e5de-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8e5de-106">Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů.</span><span class="sxs-lookup"><span data-stu-id="8e5de-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="8e5de-107">Použití protokolu OData, klienti můžete přejít přes vztahy entit.</span><span class="sxs-lookup"><span data-stu-id="8e5de-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="8e5de-108">Zadaný produkt, můžete najít dodavatele.</span><span class="sxs-lookup"><span data-stu-id="8e5de-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="8e5de-109">Můžete také vytvářet nebo odebírat vztahy.</span><span class="sxs-lookup"><span data-stu-id="8e5de-109">You can also create or remove relationships.</span></span> <span data-ttu-id="8e5de-110">Například můžete nastavit dodavatele pro produkt.</span><span class="sxs-lookup"><span data-stu-id="8e5de-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="8e5de-111">Tento kurz ukazuje, jak pro podporu těchto operací OData v4, pomocí rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="8e5de-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="8e5de-112">Tento kurz je založený na kurz [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8e5de-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e5de-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="8e5de-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8e5de-114">Webové rozhraní API 2.1</span><span class="sxs-lookup"><span data-stu-id="8e5de-114">Web API 2.1</span></span>
> - <span data-ttu-id="8e5de-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="8e5de-115">OData v4</span></span>
> - [<span data-ttu-id="8e5de-116">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="8e5de-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="8e5de-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8e5de-117">Entity Framework 6</span></span>
> - <span data-ttu-id="8e5de-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8e5de-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="8e5de-119">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="8e5de-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="8e5de-120">OData verze 3, najdete v části [podpora vztahy entit v OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="8e5de-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="8e5de-121">Přidání Entity dodavatele</span><span class="sxs-lookup"><span data-stu-id="8e5de-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="8e5de-122">Tento kurz je založený na kurz [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8e5de-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="8e5de-123">Nejdřív potřebujeme související entity.</span><span class="sxs-lookup"><span data-stu-id="8e5de-123">First, we need a related entity.</span></span> <span data-ttu-id="8e5de-124">Přidejte třídu s názvem `Supplier` ve složce modelů.</span><span class="sxs-lookup"><span data-stu-id="8e5de-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="8e5de-125">Přidat navigační vlastnost, která má `Product` třídy:</span><span class="sxs-lookup"><span data-stu-id="8e5de-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="8e5de-126">Přidejte nový **DbSet** k `ProductsContext` třídy, tak, aby rozhraní Entity Framework bude obsahovat dodavatele tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="8e5de-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="8e5de-127">V WebApiConfig.cs, přidejte &quot;Dodavatelé&quot; sada entit do datového modelu entity:</span><span class="sxs-lookup"><span data-stu-id="8e5de-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="8e5de-128">Přidat řadič dodavatelů</span><span class="sxs-lookup"><span data-stu-id="8e5de-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="8e5de-129">Přidat `SuppliersController` třídy ke složce řadiče.</span><span class="sxs-lookup"><span data-stu-id="8e5de-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="8e5de-130">I nebude ukazují, jak přidat operace CRUD pro tento kontroler.</span><span class="sxs-lookup"><span data-stu-id="8e5de-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="8e5de-131">Kroky jsou stejné jako pro řadič produkty (viz [vytvořit koncový bod OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="8e5de-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="8e5de-132">Získávání entit v relaci</span><span class="sxs-lookup"><span data-stu-id="8e5de-132">Getting Related Entities</span></span>

<span data-ttu-id="8e5de-133">Pokud chcete získat dodavatele pro produkt, klient odešle požadavek GET:</span><span class="sxs-lookup"><span data-stu-id="8e5de-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="8e5de-134">Pro podporu této žádosti, přidejte následující metodu do `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="8e5de-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="8e5de-135">Tato metoda používá výchozí zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="8e5de-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="8e5de-136">Název metody: GetX, kde X je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8e5de-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="8e5de-137">Název parametru: *klíč*</span><span class="sxs-lookup"><span data-stu-id="8e5de-137">Parameter name: *key*</span></span>

<span data-ttu-id="8e5de-138">Pokud budete postupovat podle tyto zásady vytváření názvů, webového rozhraní API automaticky mapuje požadavek HTTP metodu řadiče.</span><span class="sxs-lookup"><span data-stu-id="8e5de-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="8e5de-139">Příklad protokolu HTTP žádosti:</span><span class="sxs-lookup"><span data-stu-id="8e5de-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="8e5de-140">Příklad HTTP odpovědi:</span><span class="sxs-lookup"><span data-stu-id="8e5de-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="8e5de-141">Získání související kolekce</span><span class="sxs-lookup"><span data-stu-id="8e5de-141">Getting a related collection</span></span>

<span data-ttu-id="8e5de-142">V předchozím příkladu má produkt jednoho dodavatele.</span><span class="sxs-lookup"><span data-stu-id="8e5de-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="8e5de-143">Vlastnost navigace mohou vracet kolekci.</span><span class="sxs-lookup"><span data-stu-id="8e5de-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="8e5de-144">Následující kód získá produkty pro dodavatele:</span><span class="sxs-lookup"><span data-stu-id="8e5de-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="8e5de-145">V takovém případě metoda vrátí **IQueryable** místo **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="8e5de-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="8e5de-146">Příklad protokolu HTTP žádosti:</span><span class="sxs-lookup"><span data-stu-id="8e5de-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="8e5de-147">Příklad HTTP odpovědi:</span><span class="sxs-lookup"><span data-stu-id="8e5de-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="8e5de-148">Vytvoření vztahu mezi entitami</span><span class="sxs-lookup"><span data-stu-id="8e5de-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="8e5de-149">OData podporuje vytváření nebo odebírání vztahy mezi dvěma entitami existující.</span><span class="sxs-lookup"><span data-stu-id="8e5de-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="8e5de-150">V terminologii OData v4, je relace &quot;odkaz&quot;.</span><span class="sxs-lookup"><span data-stu-id="8e5de-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="8e5de-151">(V OData v3, byla volána relace *odkaz*.</span><span class="sxs-lookup"><span data-stu-id="8e5de-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="8e5de-152">Rozdíly protokolu nejsou důležité pro účely tohoto kurzu.)</span><span class="sxs-lookup"><span data-stu-id="8e5de-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="8e5de-153">Odkaz má svou vlastní identifikátor URI s formuláři `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="8e5de-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="8e5de-154">Zde je ukázka, identifikátor URI k vyřešení odkaz mezi produkt a jeho dodavatele:</span><span class="sxs-lookup"><span data-stu-id="8e5de-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="8e5de-155">Chcete-li přidat relaci, klient odešle požadavek POST nebo PUT na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="8e5de-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="8e5de-156">Pokud navigační vlastnost je jedna entita, jako například `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="8e5de-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="8e5de-157">POST, pokud navigační vlastnost kolekce, jako například `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="8e5de-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="8e5de-158">Text žádosti obsahuje identifikátor URI entity v vztah.</span><span class="sxs-lookup"><span data-stu-id="8e5de-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="8e5de-159">Tady je příklad požadavek:</span><span class="sxs-lookup"><span data-stu-id="8e5de-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="8e5de-160">V tomto příkladu, klient odešle požadavek PUT pro `/Products(6)/Supplier/$ref`, což je identifikátor URI $ref `Supplier` produktu s ID = 6.</span><span class="sxs-lookup"><span data-stu-id="8e5de-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="8e5de-161">Pokud neproběhne úspěšně, server odešle 204 odpovědi (ne obsahu):</span><span class="sxs-lookup"><span data-stu-id="8e5de-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="8e5de-162">Tady je metoda kontroleru přidat vztah k `Product`:</span><span class="sxs-lookup"><span data-stu-id="8e5de-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="8e5de-163">*NavigationProperty* parametr určuje, které vztah k nastavení.</span><span class="sxs-lookup"><span data-stu-id="8e5de-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="8e5de-164">(Pokud existuje více než jednu navigační vlastnost u entity, můžete přidat více `case` příkazy.)</span><span class="sxs-lookup"><span data-stu-id="8e5de-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="8e5de-165">*Odkaz* parametr obsahuje identifikátor URI dodavatele.</span><span class="sxs-lookup"><span data-stu-id="8e5de-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="8e5de-166">Webové rozhraní API automaticky analyzuje text požadavku má být získána hodnota tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="8e5de-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="8e5de-167">K vyhledání dodavatele, potřebujeme ID (nebo klíče), který je součástí *odkaz* parametr.</span><span class="sxs-lookup"><span data-stu-id="8e5de-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="8e5de-168">K tomu použijte následující pomocnou metodu:</span><span class="sxs-lookup"><span data-stu-id="8e5de-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="8e5de-169">V podstatě tato metoda používá knihovnou OData rozdělit cesta k identifikátoru URI na segmenty, najděte segment, který obsahuje klíč a převést klíč do správného typu.</span><span class="sxs-lookup"><span data-stu-id="8e5de-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="8e5de-170">Odstranění vztahu mezi entitami</span><span class="sxs-lookup"><span data-stu-id="8e5de-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="8e5de-171">Pokud chcete odstranit relaci, klient odešle žádost HTTP DELETE k identifikátoru URI $ref:</span><span class="sxs-lookup"><span data-stu-id="8e5de-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="8e5de-172">Tady je metoda kontroleru odstranit vztah mezi produktu a dodavatele:</span><span class="sxs-lookup"><span data-stu-id="8e5de-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="8e5de-173">V takovém případě `Product.Supplier` je &quot;1&quot; konec relace 1: n, tak odeberete vztah právě nastavením `Product.Supplier` k `null`.</span><span class="sxs-lookup"><span data-stu-id="8e5de-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="8e5de-174">V &quot;mnoho&quot; konci relace, klient musíte zadat, která relaci entity k odebrání.</span><span class="sxs-lookup"><span data-stu-id="8e5de-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="8e5de-175">Uděláte to tak, pošle klient identifikátor URI entity v relaci v řetězci dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="8e5de-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="8e5de-176">Chcete-li například odebrat "Produkt 1" z "dodavatele 1":</span><span class="sxs-lookup"><span data-stu-id="8e5de-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="8e5de-177">Pro podporu tohoto v rozhraní Web API, musíme zahrnout další parametr v `DeleteRef` metoda.</span><span class="sxs-lookup"><span data-stu-id="8e5de-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="8e5de-178">Tady je metoda kontroleru odebrat z produktu `Supplier.Products` vztah.</span><span class="sxs-lookup"><span data-stu-id="8e5de-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="8e5de-179">*Klíč* parametr je klíč pro dodavatele a *relatedKey* parametr je klíč pro odebrání z produktu `Products` vztah.</span><span class="sxs-lookup"><span data-stu-id="8e5de-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="8e5de-180">Všimněte si, že webového rozhraní API automaticky získá klíč z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="8e5de-180">Note that Web API automatically gets the key from the query string.</span></span>
