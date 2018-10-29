---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Podpora relací prvků v OData v3 s webovým rozhraním API 2 | Dokumentace Microsoftu
author: MikeWasson
description: 'Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů. Použití protokolu OData, klienti se můžete dostat přes...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: f78b5cf36789032f90d3d073698f7a439507277f
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206858"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="2ce06-104">Podpora relací prvků v OData v3 s webovým rozhraním API 2</span><span class="sxs-lookup"><span data-stu-id="2ce06-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="2ce06-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2ce06-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2ce06-106">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="2ce06-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="2ce06-107">Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů.</span><span class="sxs-lookup"><span data-stu-id="2ce06-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="2ce06-108">Použití protokolu OData, klienti můžete přejít přes relací prvků.</span><span class="sxs-lookup"><span data-stu-id="2ce06-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="2ce06-109">Zadaný produkt, můžete najít dodavatele.</span><span class="sxs-lookup"><span data-stu-id="2ce06-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="2ce06-110">Můžete také vytvořit nebo odebrat relace.</span><span class="sxs-lookup"><span data-stu-id="2ce06-110">You can also create or remove relationships.</span></span> <span data-ttu-id="2ce06-111">Například můžete nastavit od dodavatele, produktu.</span><span class="sxs-lookup"><span data-stu-id="2ce06-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="2ce06-112">Tento kurz ukazuje, jak podporují tyto operace v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="2ce06-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="2ce06-113">Tento kurz vychází z kurzu [vytváření koncového bodu OData v3 s webovým rozhraním API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="2ce06-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2ce06-114">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="2ce06-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2ce06-115">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="2ce06-115">Web API 2</span></span>
> - <span data-ttu-id="2ce06-116">OData verze 3</span><span class="sxs-lookup"><span data-stu-id="2ce06-116">OData Version 3</span></span>
> - <span data-ttu-id="2ce06-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2ce06-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="2ce06-118">Přidání Entity dodavatele</span><span class="sxs-lookup"><span data-stu-id="2ce06-118">Add a Supplier Entity</span></span>

<span data-ttu-id="2ce06-119">Nejdřív potřebujeme přidat nový typ entity pro naše datového kanálu OData.</span><span class="sxs-lookup"><span data-stu-id="2ce06-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="2ce06-120">Přidáme `Supplier` třídy.</span><span class="sxs-lookup"><span data-stu-id="2ce06-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="2ce06-121">Tato třída používá řetězce pro klíč entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="2ce06-122">V praxi, který může být méně běžné než pomocí klíče celé číslo.</span><span class="sxs-lookup"><span data-stu-id="2ce06-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="2ce06-123">Ale je vhodné zobrazuje, jak OData zpracovává jiné typy klíčů kromě celých čísel.</span><span class="sxs-lookup"><span data-stu-id="2ce06-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="2ce06-124">V dalším kroku vytvoříme vztah tak, že přidáte `Supplier` vlastnost `Product` třídy:</span><span class="sxs-lookup"><span data-stu-id="2ce06-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="2ce06-125">Přidat nový **DbSet** k `ProductServiceContext` třídy, aby obsahovala Entity Frameworku `Supplier` tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="2ce06-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="2ce06-126">V WebApiConfig.cs přidejte do modelu EDM entity "Dodavatelé":</span><span class="sxs-lookup"><span data-stu-id="2ce06-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="2ce06-127">Navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2ce06-127">Navigation Properties</span></span>

<span data-ttu-id="2ce06-128">Pokud chcete získat od dodavatele pro produkt, klient odešle požadavek GET:</span><span class="sxs-lookup"><span data-stu-id="2ce06-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="2ce06-129">Tady "Poskytovatel" je vlastnost navigace `Product` typu.</span><span class="sxs-lookup"><span data-stu-id="2ce06-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="2ce06-130">V takovém případě `Supplier` odkazuje na jednu položku, ale navigační vlastnost také může vrátit kolekci (vztah jednoho k několika nebo many-to-many).</span><span class="sxs-lookup"><span data-stu-id="2ce06-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="2ce06-131">Chcete-li tuto žádost o podporu, přidejte následující metodu do `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="2ce06-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="2ce06-132">*Klíč* parametr je klíč produktu.</span><span class="sxs-lookup"><span data-stu-id="2ce06-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="2ce06-133">Metoda vrátí související entity&#8212;v tomto případě `Supplier` instance.</span><span class="sxs-lookup"><span data-stu-id="2ce06-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="2ce06-134">Název metody i název parametru jsou důležité.</span><span class="sxs-lookup"><span data-stu-id="2ce06-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="2ce06-135">Obecně platí Pokud navigační vlastnost s názvem "X", musíte přidat metodu s názvem "Metody GetX".</span><span class="sxs-lookup"><span data-stu-id="2ce06-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="2ce06-136">Metoda přijme parametr s názvem "*klíč*", shoduje s datovým typem nadřazené položky klíče.</span><span class="sxs-lookup"><span data-stu-id="2ce06-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="2ce06-137">Je také důležité zahrnout **[FromOdataUri]** atribut *klíč* parametru.</span><span class="sxs-lookup"><span data-stu-id="2ce06-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="2ce06-138">Tento atribut oznamuje webového rozhraní API pro použití pravidel syntaxe OData při analýze klíč z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="2ce06-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="2ce06-139">Vytváření a odstraňování propojení</span><span class="sxs-lookup"><span data-stu-id="2ce06-139">Creating and Deleting Links</span></span>

<span data-ttu-id="2ce06-140">OData podporuje vytváření nebo odebírání vztahy mezi dvěma entitami.</span><span class="sxs-lookup"><span data-stu-id="2ce06-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="2ce06-141">V terminologii OData je vztah "propojení".</span><span class="sxs-lookup"><span data-stu-id="2ce06-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="2ce06-142">Každý odkaz má identifikátor URI s formulářem *entity*/$links /*entity*.</span><span class="sxs-lookup"><span data-stu-id="2ce06-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="2ce06-143">Odkaz z produktu dodavateli například vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2ce06-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="2ce06-144">Pokud chcete vytvořit nový odkaz, klient odešle požadavek POST na identifikátor URI odkazu.</span><span class="sxs-lookup"><span data-stu-id="2ce06-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="2ce06-145">Tělo žádosti je identifikátor URI cílové entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="2ce06-146">Například předpokládejme, že je poskytovatel s klíčem "CTSO".</span><span class="sxs-lookup"><span data-stu-id="2ce06-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="2ce06-147">Pro vytvoření odkazu z "Produktu1" k "Supplier('CTSO')", klient odešle požadavek vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="2ce06-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="2ce06-148">Pokud chcete odstranit odkaz, klient odešle požadavek DELETE na identifikátor URI odkazu.</span><span class="sxs-lookup"><span data-stu-id="2ce06-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="2ce06-149">**Vytváření odkazů**</span><span class="sxs-lookup"><span data-stu-id="2ce06-149">**Creating Links**</span></span>

<span data-ttu-id="2ce06-150">Pokud chcete povolit klienta k vytvoření odkazů produktu dodavatele, přidejte následující kód, který `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="2ce06-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="2ce06-151">Tato metoda přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="2ce06-151">This method takes three parameters:</span></span>

- <span data-ttu-id="2ce06-152">*klíč*: klíč, který chcete nadřazená entita (produkt)</span><span class="sxs-lookup"><span data-stu-id="2ce06-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="2ce06-153">*objekt navigationProperty*: název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2ce06-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="2ce06-154">V tomto příkladu je platné pouze navigační vlastnost "Poskytovatel".</span><span class="sxs-lookup"><span data-stu-id="2ce06-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="2ce06-155">*odkaz*: URI protokolu OData související entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="2ce06-156">Tato hodnota je převzata z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2ce06-156">This value is taken from the request body.</span></span> <span data-ttu-id="2ce06-157">Například může být identifikátor URI odkazu "`http://localhost/odata/Suppliers('CTSO')`, což znamená dodavatele s ID ="CTSO".</span><span class="sxs-lookup"><span data-stu-id="2ce06-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="2ce06-158">Metoda používá odkaz k vyhledání od dodavatele.</span><span class="sxs-lookup"><span data-stu-id="2ce06-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="2ce06-159">Pokud je nalezen odpovídající dodavatele, metoda nastaví `Product.Supplier` vlastnost a uloží výsledek do databáze.</span><span class="sxs-lookup"><span data-stu-id="2ce06-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="2ce06-160">Identifikátor URI odkazu je analýza nejtěžší část.</span><span class="sxs-lookup"><span data-stu-id="2ce06-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="2ce06-161">V podstatě budete muset simulovat výsledek odeslat požadavek GET na tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="2ce06-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="2ce06-162">Následující Pomocná metoda ukazuje, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="2ce06-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="2ce06-163">Metoda vyvolá proces směrování rozhraní Web API a získá zpět **objekt ODataPath** instanci, která představuje cestu k analyzovanou klauzuli OData.</span><span class="sxs-lookup"><span data-stu-id="2ce06-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="2ce06-164">Odkaz na identifikátor URI by měla být jedna z segmenty klíč entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="2ce06-165">(Pokud ne, klient zaslal chybný identifikátor URI.)</span><span class="sxs-lookup"><span data-stu-id="2ce06-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="2ce06-166">**Odstranění propojení**</span><span class="sxs-lookup"><span data-stu-id="2ce06-166">**Deleting Links**</span></span>

<span data-ttu-id="2ce06-167">Pokud chcete odstranit odkaz, přidejte následující kód, který `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="2ce06-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="2ce06-168">V tomto příkladu je navigační vlastnost jednoho `Supplier` entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="2ce06-169">Pokud je navigační vlastnost kolekce, identifikátor URI pro odstranění odkazu musí obsahovat klíč související entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="2ce06-170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2ce06-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="2ce06-171">Tento požadavek zruší zákazníka 1 pořadí 1.</span><span class="sxs-lookup"><span data-stu-id="2ce06-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="2ce06-172">V tomto případě metodu DeleteLink mít následující podpis:</span><span class="sxs-lookup"><span data-stu-id="2ce06-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="2ce06-173">*RelatedKey* parametr poskytuje klíč pro související entity.</span><span class="sxs-lookup"><span data-stu-id="2ce06-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="2ce06-174">Ano v vaše `DeleteLink` metoda vyhledávací primární entity podle *klíč* parametr, Najít související entity podle *relatedKey* parametr a potom odeberte přidružení.</span><span class="sxs-lookup"><span data-stu-id="2ce06-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="2ce06-175">V závislosti na datovém modelu, může být nutné implementovat obě verze `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="2ce06-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="2ce06-176">Správná verze podle identifikátoru URI požadavku bude volat webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2ce06-176">Web API will call the correct version based on the request URI.</span></span>
