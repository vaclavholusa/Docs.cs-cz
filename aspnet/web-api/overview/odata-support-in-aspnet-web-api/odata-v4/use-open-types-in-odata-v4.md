---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Otevřete typy v OData v4 s rozhraním ASP.NET Web API | Dokumentace Microsoftu
author: microsoft
description: V OData v4 je otevřený typ stuctured typ, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarovány v definici typu. Otevřete...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 0eae376830e70199a9692df5a154168646f99716
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375522"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="6bc23-104">Otevřete typy v OData v4 s rozhraním ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="6bc23-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="6bc23-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6bc23-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6bc23-106">V OData v4 *otevřený typ.* stuctured typ, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarovány v definici typu.</span><span class="sxs-lookup"><span data-stu-id="6bc23-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="6bc23-107">Otevřené typy umožňují zvýšit flexibilitu datových modelů.</span><span class="sxs-lookup"><span data-stu-id="6bc23-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="6bc23-108">Tento kurz ukazuje, jak používat otevřené typy v ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="6bc23-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="6bc23-109">Tento kurz předpokládá, že už víte, jak vytvořit koncový bod OData v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="6bc23-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="6bc23-110">Pokud ne, začněte tím, že čtení [vytvoření koncového bodu OData v4](create-an-odata-v4-endpoint.md) první.</span><span class="sxs-lookup"><span data-stu-id="6bc23-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6bc23-111">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="6bc23-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6bc23-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="6bc23-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="6bc23-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="6bc23-113">OData v4</span></span>


<span data-ttu-id="6bc23-114">První, některé OData terminologie:</span><span class="sxs-lookup"><span data-stu-id="6bc23-114">First, some OData terminology:</span></span>

- <span data-ttu-id="6bc23-115">Typ entity: strukturovaného typu pomocí klíče.</span><span class="sxs-lookup"><span data-stu-id="6bc23-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="6bc23-116">Komplexní typ: strukturovaného typu bez klíče.</span><span class="sxs-lookup"><span data-stu-id="6bc23-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="6bc23-117">Otevřete typu: typ s dynamických vlastností.</span><span class="sxs-lookup"><span data-stu-id="6bc23-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="6bc23-118">Typy entit a komplexní typy možné otevřít.</span><span class="sxs-lookup"><span data-stu-id="6bc23-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="6bc23-119">Hodnotu dynamické vlastnosti může být primitivní typ, komplexního typu nebo typu výčtu nebo jakýkoli z těchto typů kolekce.</span><span class="sxs-lookup"><span data-stu-id="6bc23-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="6bc23-120">Další informace o typech otevřít, najdete v článku [specifikace v4 pracovní verze protokolu OData](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="6bc23-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="6bc23-121">Instalace knihoven OData Web</span><span class="sxs-lookup"><span data-stu-id="6bc23-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="6bc23-122">Použití Správce balíčků NuGet k instalaci nejnovějších knihoven Web API OData.</span><span class="sxs-lookup"><span data-stu-id="6bc23-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="6bc23-123">Z okna konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="6bc23-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="6bc23-124">Definování typů CLR</span><span class="sxs-lookup"><span data-stu-id="6bc23-124">Define the CLR Types</span></span>

<span data-ttu-id="6bc23-125">Začněte tím, že definice modelu EDM jako typy CLR.</span><span class="sxs-lookup"><span data-stu-id="6bc23-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="6bc23-126">Vytvoření Entity Data Model (EDM)</span><span class="sxs-lookup"><span data-stu-id="6bc23-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="6bc23-127">`Category` je typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="6bc23-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="6bc23-128">`Address` je komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="6bc23-128">`Address` is a complex type.</span></span> <span data-ttu-id="6bc23-129">(Nemá klíč, takže není typem entity.)</span><span class="sxs-lookup"><span data-stu-id="6bc23-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="6bc23-130">`Customer` je typ entity.</span><span class="sxs-lookup"><span data-stu-id="6bc23-130">`Customer` is an entity type.</span></span> <span data-ttu-id="6bc23-131">(Nemá klíč.)</span><span class="sxs-lookup"><span data-stu-id="6bc23-131">(It has a key.)</span></span>
- <span data-ttu-id="6bc23-132">`Press` je otevřít komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="6bc23-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="6bc23-133">`Book` je typu open entity.</span><span class="sxs-lookup"><span data-stu-id="6bc23-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="6bc23-134">Chcete-li vytvořit otevřený typ, typ CLR musí mít zadanou vlastnost typu `IDictionary<string, object>`, který obsahuje dynamických vlastností.</span><span class="sxs-lookup"><span data-stu-id="6bc23-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="6bc23-135">Sestavení modelu EDM</span><span class="sxs-lookup"><span data-stu-id="6bc23-135">Build the EDM Model</span></span>

<span data-ttu-id="6bc23-136">Pokud používáte **ODataConventionModelBuilder** k vytvoření modelu EDM `Press` a `Book` jsou automaticky přidány jako typy open, podle přítomnosti `IDictionary<string, object>` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6bc23-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="6bc23-137">Můžete také sestavit EDM explicitně, pomocí **Tvůrce ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="6bc23-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="6bc23-138">Přidat kontroler OData</span><span class="sxs-lookup"><span data-stu-id="6bc23-138">Add an OData Controller</span></span>

<span data-ttu-id="6bc23-139">V dalším kroku přidáte kontroler OData.</span><span class="sxs-lookup"><span data-stu-id="6bc23-139">Next, add an OData controller.</span></span> <span data-ttu-id="6bc23-140">Pro účely tohoto kurzu používáme zjednodušený kontroler, podporuje jenom získat po požadavků a přehledu v paměti používá k uložení entity.</span><span class="sxs-lookup"><span data-stu-id="6bc23-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="6bc23-141">Všimněte si, že první `Book` instance nemá žádné dynamické vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6bc23-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="6bc23-142">Druhá `Book` instance má následující dynamické vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="6bc23-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="6bc23-143">"Publikováno": primitivní typ.</span><span class="sxs-lookup"><span data-stu-id="6bc23-143">"Published": Primitive type</span></span>
- <span data-ttu-id="6bc23-144">"Autoři": kolekce primitivních typů</span><span class="sxs-lookup"><span data-stu-id="6bc23-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="6bc23-145">"OtherCategories": kolekci typů výčtu.</span><span class="sxs-lookup"><span data-stu-id="6bc23-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="6bc23-146">Také `Press` vlastnost, která `Book` instance má následující dynamické vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="6bc23-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="6bc23-147">"Blogu": primitivní typ.</span><span class="sxs-lookup"><span data-stu-id="6bc23-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="6bc23-148">"Address": komplexní typ</span><span class="sxs-lookup"><span data-stu-id="6bc23-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="6bc23-149">Dotaz Metadata</span><span class="sxs-lookup"><span data-stu-id="6bc23-149">Query the Metadata</span></span>

<span data-ttu-id="6bc23-150">K získání dokumentu metadata OData, odeslat požadavek GET na `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="6bc23-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="6bc23-151">Tělo odpovědi by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="6bc23-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="6bc23-152">Dokument metadat uvidíte, které:</span><span class="sxs-lookup"><span data-stu-id="6bc23-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="6bc23-153">Pro `Book` a `Press` zadá hodnotu `OpenType` atribut má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="6bc23-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="6bc23-154">`Customer` a `Address` typy nemají tento atribut.</span><span class="sxs-lookup"><span data-stu-id="6bc23-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="6bc23-155">`Book` Typ entity má tři vlastnosti prohlášené: ISBN, název a stiskněte klávesu.</span><span class="sxs-lookup"><span data-stu-id="6bc23-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="6bc23-156">OData metadata se nenachází `Book.Properties` vlastnost z třídy CLR.</span><span class="sxs-lookup"><span data-stu-id="6bc23-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="6bc23-157">Podobně platí `Press` komplexní typ obsahuje pouze dvě vlastnosti prohlášené: názvů a kategorie.</span><span class="sxs-lookup"><span data-stu-id="6bc23-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="6bc23-158">Metadata nezahrnují `Press.DynamicProperties` vlastnost z třídy CLR.</span><span class="sxs-lookup"><span data-stu-id="6bc23-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="6bc23-159">Dotazování Entity</span><span class="sxs-lookup"><span data-stu-id="6bc23-159">Query an Entity</span></span>

<span data-ttu-id="6bc23-160">Chcete-li získat knihu s ISBN rovno "978-0-7356-7942-9", odešlete požadavek GET na `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="6bc23-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="6bc23-161">Tělo odpovědi by měl vypadat nějak takto.</span><span class="sxs-lookup"><span data-stu-id="6bc23-161">The response body should look similar to the following.</span></span> <span data-ttu-id="6bc23-162">(Chcete-li lépe čitelný odsazena.)</span><span class="sxs-lookup"><span data-stu-id="6bc23-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="6bc23-163">Všimněte si, že dynamické vlastnosti jsou zahrnuty vložené pomocí vlastnosti prohlášené.</span><span class="sxs-lookup"><span data-stu-id="6bc23-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="6bc23-164">PŘÍSPĚVEK Entity</span><span class="sxs-lookup"><span data-stu-id="6bc23-164">POST an Entity</span></span>

<span data-ttu-id="6bc23-165">Přidání entity knihy, odeslat požadavek POST do `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="6bc23-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="6bc23-166">Klienta můžete nastavit dynamické vlastnosti datové části požadavku.</span><span class="sxs-lookup"><span data-stu-id="6bc23-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="6bc23-167">Tady je příklad žádosti.</span><span class="sxs-lookup"><span data-stu-id="6bc23-167">Here is an example request.</span></span> <span data-ttu-id="6bc23-168">Všimněte si vlastnosti "Price" a "Publikováno".</span><span class="sxs-lookup"><span data-stu-id="6bc23-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="6bc23-169">Pokud nastavíte zarážku v metodě kontroleru, uvidíte, že webové rozhraní API přidá vlastnosti tak, aby `Properties` slovníku.</span><span class="sxs-lookup"><span data-stu-id="6bc23-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="6bc23-170">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6bc23-170">Additional Resources</span></span>

[<span data-ttu-id="6bc23-171">Typ vzorku OData Open</span><span class="sxs-lookup"><span data-stu-id="6bc23-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
