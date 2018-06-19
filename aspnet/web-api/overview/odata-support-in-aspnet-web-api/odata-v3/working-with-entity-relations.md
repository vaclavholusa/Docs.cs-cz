---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Podpora vztahy entit v prostředí s Web API 2 OData v3 | Microsoft Docs
author: MikeWasson
description: 'Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů. Použití protokolu OData, klienti můžete přejít přes...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566827"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="6dc87-104">Podpora vztahy entit v prostředí s Web API 2 OData v3</span><span class="sxs-lookup"><span data-stu-id="6dc87-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="6dc87-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6dc87-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6dc87-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="6dc87-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="6dc87-107">Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů.</span><span class="sxs-lookup"><span data-stu-id="6dc87-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="6dc87-108">Použití protokolu OData, klienti můžete přejít přes vztahy entit.</span><span class="sxs-lookup"><span data-stu-id="6dc87-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="6dc87-109">Zadaný produkt, můžete najít dodavatele.</span><span class="sxs-lookup"><span data-stu-id="6dc87-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="6dc87-110">Můžete také vytvářet nebo odebírat vztahy.</span><span class="sxs-lookup"><span data-stu-id="6dc87-110">You can also create or remove relationships.</span></span> <span data-ttu-id="6dc87-111">Například můžete nastavit dodavatele pro produkt.</span><span class="sxs-lookup"><span data-stu-id="6dc87-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="6dc87-112">Tento kurz ukazuje, jak pro podporu těchto operací v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="6dc87-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="6dc87-113">Tento kurz je založený na kurz [vytváření koncový bod OData v3 s webovém rozhraní API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6dc87-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6dc87-114">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="6dc87-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6dc87-115">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="6dc87-115">Web API 2</span></span>
> - <span data-ttu-id="6dc87-116">OData verze 3</span><span class="sxs-lookup"><span data-stu-id="6dc87-116">OData Version 3</span></span>
> - <span data-ttu-id="6dc87-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6dc87-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="6dc87-118">Přidání Entity dodavatele</span><span class="sxs-lookup"><span data-stu-id="6dc87-118">Add a Supplier Entity</span></span>

<span data-ttu-id="6dc87-119">Nejprve musíme přidejte nový typ entity pro naše datového kanálu OData.</span><span class="sxs-lookup"><span data-stu-id="6dc87-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="6dc87-120">Přidáme `Supplier` třídy.</span><span class="sxs-lookup"><span data-stu-id="6dc87-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="6dc87-121">Tato třída se používá řetězec pro klíč entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="6dc87-122">V praxi, který může být méně častých než použití klíče celé číslo.</span><span class="sxs-lookup"><span data-stu-id="6dc87-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="6dc87-123">Ale je vhodné zobrazuje, jak OData zpracovává jiné typy klíčů kromě celých čísel.</span><span class="sxs-lookup"><span data-stu-id="6dc87-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="6dc87-124">V dalším kroku vytvoříme vztah přidáním `Supplier` vlastnost, která má `Product` třídy:</span><span class="sxs-lookup"><span data-stu-id="6dc87-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="6dc87-125">Přidejte nový **DbSet** k `ProductServiceContext` třídy, tak, aby rozhraní Entity Framework zahrne `Supplier` tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="6dc87-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="6dc87-126">V WebApiConfig.cs přidejte do modelu EDM entity "Dodavatelé":</span><span class="sxs-lookup"><span data-stu-id="6dc87-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="6dc87-127">Navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="6dc87-127">Navigation Properties</span></span>

<span data-ttu-id="6dc87-128">Pokud chcete získat dodavatele pro produkt, klient odešle požadavek GET:</span><span class="sxs-lookup"><span data-stu-id="6dc87-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="6dc87-129">Tady je vlastnost navigace na "Dodavatel" `Product` typu.</span><span class="sxs-lookup"><span data-stu-id="6dc87-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="6dc87-130">V takovém případě `Supplier` odkazuje na jednu položku, ale navigační vlastnost mohou vracet kolekci (vztah jeden mnoho nebo n: n).</span><span class="sxs-lookup"><span data-stu-id="6dc87-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="6dc87-131">Pro podporu této žádosti, přidejte následující metodu do `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="6dc87-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="6dc87-132">*Klíč* parametr je klíč produktu.</span><span class="sxs-lookup"><span data-stu-id="6dc87-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="6dc87-133">Metoda vrátí související entity & #8212 v takovém případě `Supplier` instance.</span><span class="sxs-lookup"><span data-stu-id="6dc87-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="6dc87-134">Název metody a název parametru jsou důležité.</span><span class="sxs-lookup"><span data-stu-id="6dc87-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="6dc87-135">Obecně platí Pokud navigační vlastnost s názvem "X", budete muset přidat metodu s názvem "GetX".</span><span class="sxs-lookup"><span data-stu-id="6dc87-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="6dc87-136">Metoda vyžaduje parametr s názvem "*klíč*" odpovídající datový typ klíče nadřazeného objektu.</span><span class="sxs-lookup"><span data-stu-id="6dc87-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="6dc87-137">Je také důležité zahrnout **[FromOdataUri]** atribut *klíč* parametr.</span><span class="sxs-lookup"><span data-stu-id="6dc87-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="6dc87-138">Tento atribut určuje webového rozhraní API používat OData syntaxe pravidla v případě, že analyzuje klíč z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="6dc87-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="6dc87-139">Vytváření a odstraňování odkazů</span><span class="sxs-lookup"><span data-stu-id="6dc87-139">Creating and Deleting Links</span></span>

<span data-ttu-id="6dc87-140">OData podporuje vytváření nebo odebírání vztahy mezi dvěma entitami.</span><span class="sxs-lookup"><span data-stu-id="6dc87-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="6dc87-141">V terminologii OData relace je "link".</span><span class="sxs-lookup"><span data-stu-id="6dc87-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="6dc87-142">Identifikátor URI s formuláři má každý odkaz *entity*/$links /*entity*.</span><span class="sxs-lookup"><span data-stu-id="6dc87-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="6dc87-143">Odkaz z produktu dodavatele například vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6dc87-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="6dc87-144">Pokud chcete vytvořit nové propojení, klient odešle požadavek POST odkaz identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6dc87-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="6dc87-145">Text žádosti je identifikátor URI cílové entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="6dc87-146">Předpokládejme například, že je dodavatele s klíčem "CTSO".</span><span class="sxs-lookup"><span data-stu-id="6dc87-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="6dc87-147">Pokud chcete vytvořit odkaz ze "Produktu1" na "Supplier('CTSO')", klient odešle požadavek takto:</span><span class="sxs-lookup"><span data-stu-id="6dc87-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="6dc87-148">Pokud chcete odstranit odkaz, klient odešle žádost o odstranění propojení identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6dc87-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="6dc87-149">**Vytváření odkazů**</span><span class="sxs-lookup"><span data-stu-id="6dc87-149">**Creating Links**</span></span>

<span data-ttu-id="6dc87-150">Chcete-li klient k vytváření propojení produktu dodavatele, přidejte následující kód do `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="6dc87-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="6dc87-151">Tato metoda přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="6dc87-151">This method takes three parameters:</span></span>

- <span data-ttu-id="6dc87-152">*klíč*: klíč k nadřazená entita (produktu)</span><span class="sxs-lookup"><span data-stu-id="6dc87-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="6dc87-153">*Vlastnost navigationProperty*: název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6dc87-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="6dc87-154">V tomto příkladu je jediná platná navigační vlastnost "Dodavatel".</span><span class="sxs-lookup"><span data-stu-id="6dc87-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="6dc87-155">*odkaz*: identifikátoru URI protokolu OData související entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="6dc87-156">Tato hodnota jsou převzaty z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="6dc87-156">This value is taken from the request body.</span></span> <span data-ttu-id="6dc87-157">Například může být identifikátor URI na odkaz "`http://localhost/odata/Suppliers('CTSO')`, což znamená dodavatele s ID = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="6dc87-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="6dc87-158">Metoda používá připojení k vyhledání dodavatele.</span><span class="sxs-lookup"><span data-stu-id="6dc87-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="6dc87-159">Pokud je nalezen odpovídající dodavatele, metoda nastaví `Product.Supplier` vlastnost a výsledek uloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="6dc87-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="6dc87-160">Nejtěžší část je analýza odkaz identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6dc87-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="6dc87-161">V podstatě budete muset simulovat výsledek odeslání požadavek GET na tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6dc87-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="6dc87-162">Následující pomocnou metodu ukazuje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="6dc87-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="6dc87-163">Metoda vyvolá proces směrování rozhraní Web API a získá zpět **ODataPath** instanci, která představuje Analyzovaná cesta OData.</span><span class="sxs-lookup"><span data-stu-id="6dc87-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="6dc87-164">Pro odkaz identifikátor URI jedna segmenty by měla být klíč entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="6dc87-165">(Pokud ne, klient zaslal chybný identifikátor URI.)</span><span class="sxs-lookup"><span data-stu-id="6dc87-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="6dc87-166">**Odstraňování odkazů**</span><span class="sxs-lookup"><span data-stu-id="6dc87-166">**Deleting Links**</span></span>

<span data-ttu-id="6dc87-167">Chcete-li odstranit odkaz, přidejte následující kód do `ProductsController` třídy:</span><span class="sxs-lookup"><span data-stu-id="6dc87-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="6dc87-168">V tomto příkladu je navigační vlastnost jednoho `Supplier` entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="6dc87-169">Pokud je vlastnost navigace kolekce, identifikátor URI pro odstranění odkazu musí obsahovat klíč pro související entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="6dc87-170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6dc87-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="6dc87-171">Tento požadavek odebere pořadí 1 zákazníka 1.</span><span class="sxs-lookup"><span data-stu-id="6dc87-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="6dc87-172">V takovém případě bude mít metoda DeleteLink následující podpis:</span><span class="sxs-lookup"><span data-stu-id="6dc87-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="6dc87-173">*RelatedKey* parametr poskytuje klíč pro související entity.</span><span class="sxs-lookup"><span data-stu-id="6dc87-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="6dc87-174">Ano v vaše `DeleteLink` metoda vyhledávání primární entity podle *klíč* parametr najít související entity podle *relatedKey* parametr a potom odeberte přidružení.</span><span class="sxs-lookup"><span data-stu-id="6dc87-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="6dc87-175">V závislosti na datový model, možná budete muset implementovat obě verze `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="6dc87-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="6dc87-176">Webové rozhraní API bude volat správnou verzi podle identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="6dc87-176">Web API will call the correct version based on the request URI.</span></span>
