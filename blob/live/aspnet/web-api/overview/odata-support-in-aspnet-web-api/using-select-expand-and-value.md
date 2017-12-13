---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "Pomocí $select, $, rozbalte položku a $value v prostředí ASP.NET Web API 2 OData | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="63272-102">Pomocí $select, $, rozbalte položku a $value v prostředí ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="63272-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="63272-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="63272-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="63272-104">Webové rozhraní API 2 přidává podporu pro rozšířit $expand, $select a možnosti $value protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="63272-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="63272-105">Tyto možnosti Povolit klienta k řízení reprezentaci, který získá zpět ze serveru.</span><span class="sxs-lookup"><span data-stu-id="63272-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="63272-106">**Rozbalte $** způsobí, že mají být zahrnuty vložené v odpovědi související entity.</span><span class="sxs-lookup"><span data-stu-id="63272-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="63272-107">**$select** vybere podmnožinu vlastnosti, které chcete zahrnout do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="63272-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="63272-108">**$value** získá nezpracovanou hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63272-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="63272-109">Příklad schématu</span><span class="sxs-lookup"><span data-stu-id="63272-109">Example Schema</span></span>

<span data-ttu-id="63272-110">V tomto článku budete použít služby OData, která definuje tři entity: produktu, dodavatel a kategorie.</span><span class="sxs-lookup"><span data-stu-id="63272-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="63272-111">Každý produkt má jednu kategorii a jednoho dodavatele.</span><span class="sxs-lookup"><span data-stu-id="63272-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="63272-112">Zde jsou tříd jazyka C#, které definují modelem entity:</span><span class="sxs-lookup"><span data-stu-id="63272-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="63272-113">Všimněte si, že `Product` třída definuje navigační vlastnosti pro `Supplier` a `Category`.</span><span class="sxs-lookup"><span data-stu-id="63272-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="63272-114">`Category` Třída definuje navigační vlastnost pro produkty v každé kategorii.</span><span class="sxs-lookup"><span data-stu-id="63272-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="63272-115">Chcete-li vytvořit koncový bod OData pro toto schéma, použijte generování uživatelského rozhraní sady Visual Studio 2013, jak je popsáno v [vytváření koncový bod OData v rozhraní ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="63272-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="63272-116">Přidáte samostatné řadiče pro produkt, kategorie a dodavatele.</span><span class="sxs-lookup"><span data-stu-id="63272-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="63272-117">Povolení $rozbalte a $select</span><span class="sxs-lookup"><span data-stu-id="63272-117">Enabling $expand and $select</span></span>

<span data-ttu-id="63272-118">Ve Visual Studiu 2013 se vytvoří generování uživatelského rozhraní Web API OData řadič této automaticky podporuje $expand a $select.</span><span class="sxs-lookup"><span data-stu-id="63272-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="63272-119">Pro referenci tady jsou požadavky na podporu $rozbalte a $select v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="63272-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="63272-120">Pro kolekce, řadičem na `Get` metoda musí vrátit **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="63272-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="63272-121">Pro jednotlivé entity vrátit **SingleResult&lt;T&gt;**, kde T představuje **IQueryable** obsahující žádnou nebo jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="63272-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="63272-122">Navíc uspořádání vaší `Get` metody s **[Queryable]** atributu, jak je znázorněno v předchozích fragmentů kódu.</span><span class="sxs-lookup"><span data-stu-id="63272-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="63272-123">Alternativně volání **EnableQuerySupport** na **HttpConfiguration** objekt při spuštění.</span><span class="sxs-lookup"><span data-stu-id="63272-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="63272-124">(Další informace najdete v tématu [povolení možnosti dotazu OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="63272-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="63272-125">Pomocí $rozbalte</span><span class="sxs-lookup"><span data-stu-id="63272-125">Using $expand</span></span>

<span data-ttu-id="63272-126">Při dotazu OData entity nebo kolekci, výchozí odpověď neobsahuje entit v relaci.</span><span class="sxs-lookup"><span data-stu-id="63272-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="63272-127">Zde je ukázka, výchozí odpověď pro sadu entit kategorií:</span><span class="sxs-lookup"><span data-stu-id="63272-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="63272-128">Jak vidíte, odpověď neobsahuje všechny produkty, i když kategorie entity má navigační odkaz produkty.</span><span class="sxs-lookup"><span data-stu-id="63272-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="63272-129">Klient však můžete použít $rozbalte získat seznam produktů pro každou kategorii.</span><span class="sxs-lookup"><span data-stu-id="63272-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="63272-130">Rozbalte možnost $expand přejde v řetězci dotazu požadavku:</span><span class="sxs-lookup"><span data-stu-id="63272-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="63272-131">Server bude nyní zahrnují tyto produkty pro každou kategorii vložením s kategorií.</span><span class="sxs-lookup"><span data-stu-id="63272-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="63272-132">Tady je datové části odpovědi:</span><span class="sxs-lookup"><span data-stu-id="63272-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="63272-133">Všimněte si, že každou položku v poli "value" obsahuje seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="63272-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="63272-134">$Expand rozbalte možnost trvá čárkami oddělený seznam navigačních vlastností pro rozbalení.</span><span class="sxs-lookup"><span data-stu-id="63272-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="63272-135">Následující požadavek rozšíří kategorii a dodavatel pro produkt.</span><span class="sxs-lookup"><span data-stu-id="63272-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="63272-136">Tady je text odpovědi:</span><span class="sxs-lookup"><span data-stu-id="63272-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="63272-137">Můžete rozbalit více než jednu úroveň navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63272-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="63272-138">Následující příklad obsahuje všechny produkty pro kategorii a také dodavatel pro každý produkt.</span><span class="sxs-lookup"><span data-stu-id="63272-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="63272-139">Tady je text odpovědi:</span><span class="sxs-lookup"><span data-stu-id="63272-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="63272-140">Ve výchozím omezení webového rozhraní API rozšíření maximální hloubka 2.</span><span class="sxs-lookup"><span data-stu-id="63272-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="63272-141">Který brání klientovi odesílání komplexní požadavků jako `$expand=Orders/OrderDetails/Product/Supplier/Region`, což může být neefektivní pro dotazování a vytvořit velké odpovědi.</span><span class="sxs-lookup"><span data-stu-id="63272-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="63272-142">Chcete-li přepsat výchozí nastavení, nastavte **MaxExpansionDepth** vlastnost **[Queryable]** atribut.</span><span class="sxs-lookup"><span data-stu-id="63272-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="63272-143">Další informace o $rozšířit možnost, najdete v části [rozbalte možností dotazu systému ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) v oficiální dokumentaci OData.</span><span class="sxs-lookup"><span data-stu-id="63272-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="63272-144">Pomocí $select</span><span class="sxs-lookup"><span data-stu-id="63272-144">Using $select</span></span>

<span data-ttu-id="63272-145">Možnost $select určuje podmnožinu vlastnosti, které chcete zahrnout do text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="63272-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="63272-146">Například získat jenom název a cena jednotlivých produktů, použijte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="63272-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="63272-147">Tady je text odpovědi:</span><span class="sxs-lookup"><span data-stu-id="63272-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="63272-148">Můžete kombinovat $select a $expand rozbalte ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="63272-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="63272-149">Nezapomeňte zahrnout vlastnost rozšířené možnosti $select.</span><span class="sxs-lookup"><span data-stu-id="63272-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="63272-150">Například následující požadavek získá název produktu a dodavatele.</span><span class="sxs-lookup"><span data-stu-id="63272-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="63272-151">Tady je text odpovědi:</span><span class="sxs-lookup"><span data-stu-id="63272-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="63272-152">Můžete také vybrat vlastnosti v rámci rozšířené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63272-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="63272-153">Následující žádost o rozšíří produkty a vybere název kategorie a název produktu.</span><span class="sxs-lookup"><span data-stu-id="63272-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="63272-154">Tady je text odpovědi:</span><span class="sxs-lookup"><span data-stu-id="63272-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="63272-155">Další informace o možnost $select najdete v tématu [vyberte možností dotazu systému ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) v oficiální dokumentaci OData.</span><span class="sxs-lookup"><span data-stu-id="63272-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="63272-156">Získávání jednotlivé vlastnosti entity ($value)</span><span class="sxs-lookup"><span data-stu-id="63272-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="63272-157">Existují dva způsoby pro klienta OData k získání jednotlivých vlastnost z entity.</span><span class="sxs-lookup"><span data-stu-id="63272-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="63272-158">Klienta můžete získat hodnotu ve formátu OData nebo získá nezpracovanou hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63272-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="63272-159">Následující požadavek získá vlastnost ve formátu OData.</span><span class="sxs-lookup"><span data-stu-id="63272-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="63272-160">Tady je příklad odpověď ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="63272-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="63272-161">Chcete-li získat nezpracovanou hodnotu vlastnosti, připojte $value identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="63272-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="63272-162">Zde je odpovědi.</span><span class="sxs-lookup"><span data-stu-id="63272-162">Here is the response.</span></span> <span data-ttu-id="63272-163">Všimněte si, že typ obsahu, který není "text/plain" JSON.</span><span class="sxs-lookup"><span data-stu-id="63272-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="63272-164">Pro podporu těchto dotazů do kontroleru OData, přidejte metodu s názvem `GetProperty`, kde `Property` je název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63272-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="63272-165">Například by s názvem metodu GET pro vlastnost název `GetName`.</span><span class="sxs-lookup"><span data-stu-id="63272-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="63272-166">Metoda by měla vrátit hodnotu této vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="63272-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
