---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "Otevřete typy v OData v4 s rozhraním ASP.NET Web API | Microsoft Docs"
author: microsoft
description: "V prostředí OData v4 je otevřený typ stuctured typu, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarované v definici typu. Otevřete..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="8c9b4-104">Otevřete typy v OData v4 s rozhraním ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="8c9b4-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="8c9b4-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8c9b4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="8c9b4-106">V prostředí OData v4 *otevřete typ* je stuctured typu, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarované v definici typu.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="8c9b4-107">Otevřete typy umožňují zvýšit flexibilitu datové modely.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="8c9b4-108">Tento kurz ukazuje, jak používat otevřete typy v prostředí ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="8c9b4-109">Tento kurz předpokládá, že již víte, jak vytvořit koncový bod OData v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="8c9b4-110">Pokud ne, spustit načtením [vytvořit koncový bod OData v4](create-an-odata-v4-endpoint.md) první.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8c9b4-111">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="8c9b4-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8c9b4-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="8c9b4-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="8c9b4-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="8c9b4-113">OData v4</span></span>


<span data-ttu-id="8c9b4-114">První, některé terminologie OData:</span><span class="sxs-lookup"><span data-stu-id="8c9b4-114">First, some OData terminology:</span></span>

- <span data-ttu-id="8c9b4-115">Typ entity: strukturovaného typu s klíčem.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="8c9b4-116">Komplexní typ: strukturovaného typu bez klíče.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="8c9b4-117">Otevřete typu: typ s dynamických vlastností.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="8c9b4-118">Typy entit a komplexní typy možné otevřít.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="8c9b4-119">Hodnota dynamické vlastnosti, může být primitivní typ, komplexní typ nebo typ výčtu; nebo kolekci jakýkoli z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="8c9b4-120">Další informace o typech otevřete najdete v tématu [OData v4 specifikace](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="8c9b4-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="8c9b4-121">Instalace webové knihovny OData</span><span class="sxs-lookup"><span data-stu-id="8c9b4-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="8c9b4-122">Použití Správce balíčků NuGet k instalaci nejnovějších knihoven Web API OData.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="8c9b4-123">Z okna konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="8c9b4-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="8c9b4-124">Definování typů CLR</span><span class="sxs-lookup"><span data-stu-id="8c9b4-124">Define the CLR Types</span></span>

<span data-ttu-id="8c9b4-125">Začněte definováním modelech EDM jako typy CLR.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="8c9b4-126">Když je vytvořena Entity Data Model (EDM),</span><span class="sxs-lookup"><span data-stu-id="8c9b4-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="8c9b4-127">`Category`je typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="8c9b4-128">`Address`je komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-128">`Address` is a complex type.</span></span> <span data-ttu-id="8c9b4-129">(Nemá klíče, takže není typ entity.)</span><span class="sxs-lookup"><span data-stu-id="8c9b4-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="8c9b4-130">`Customer`je typ entity.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-130">`Customer` is an entity type.</span></span> <span data-ttu-id="8c9b4-131">(Má klíč.)</span><span class="sxs-lookup"><span data-stu-id="8c9b4-131">(It has a key.)</span></span>
- <span data-ttu-id="8c9b4-132">`Press`je otevřený komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="8c9b4-133">`Book`je typu open entity.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="8c9b4-134">Pokud chcete vytvořit otevřený typ, typ CLR musí mít vlastnost typu `IDictionary<string, object>`, který obsahuje dynamických vlastností.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="8c9b4-135">Vytvoření modelu EDM</span><span class="sxs-lookup"><span data-stu-id="8c9b4-135">Build the EDM Model</span></span>

<span data-ttu-id="8c9b4-136">Pokud používáte **ODataConventionModelBuilder** pro vytvoření modelu EDM `Press` a `Book` se automaticky přidají jako otevřete typy, založené na přítomnost `IDictionary<string, object>` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="8c9b4-137">Můžete také vytvořit EDM explicitně, pomocí **Tvůrce ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="8c9b4-138">Přidání Kontroleru OData</span><span class="sxs-lookup"><span data-stu-id="8c9b4-138">Add an OData Controller</span></span>

<span data-ttu-id="8c9b4-139">Dál přidejte kontroleru OData.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-139">Next, add an OData controller.</span></span> <span data-ttu-id="8c9b4-140">V tomto kurzu použijeme zjednodušené řadiče, podporuje jenom získat POST požadavky a používá seznam aplikace v paměti k ukládání entit.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="8c9b4-141">Všimněte si, že první `Book` instance nemá žádné dynamické vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="8c9b4-142">Druhý `Book` instance má následující dynamické vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8c9b4-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="8c9b4-143">"Publikovat": primitivní typ</span><span class="sxs-lookup"><span data-stu-id="8c9b4-143">"Published": Primitive type</span></span>
- <span data-ttu-id="8c9b4-144">"Autoři": kolekce primitivní typy</span><span class="sxs-lookup"><span data-stu-id="8c9b4-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="8c9b4-145">"OtherCategories": kolekce výčtové typy.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="8c9b4-146">Navíc `Press` vlastnost této `Book` instance má následující dynamické vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8c9b4-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="8c9b4-147">"Blog": primitivní typ</span><span class="sxs-lookup"><span data-stu-id="8c9b4-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="8c9b4-148">"Address": komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="8c9b4-149">Dotazu Metadata</span><span class="sxs-lookup"><span data-stu-id="8c9b4-149">Query the Metadata</span></span>

<span data-ttu-id="8c9b4-150">Chcete-li získat dokument metadat OData, odešlete požadavek GET na `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="8c9b4-151">Text odpovědi by měl vypadat podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="8c9b4-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="8c9b4-152">Z dokumentu metadat můžete zjistit, který:</span><span class="sxs-lookup"><span data-stu-id="8c9b4-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="8c9b4-153">Pro `Book` a `Press` typy, hodnota `OpenType` atribut hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="8c9b4-154">`Customer` a `Address` typy nemají tento atribut.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="8c9b4-155">`Book` Typ entity má tři deklarované vlastnosti: ISBN, název a stiskněte klávesu.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="8c9b4-156">OData metadata nezahrnuje `Book.Properties` vlastnost z třídy CLR.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="8c9b4-157">Podobně `Press` komplexní typ má jenom dvě deklarované vlastnosti: název a kategorie.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="8c9b4-158">Metadata se nenachází `Press.DynamicProperties` vlastnost z třídy CLR.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="8c9b4-159">Dotaz Entity</span><span class="sxs-lookup"><span data-stu-id="8c9b4-159">Query an Entity</span></span>

<span data-ttu-id="8c9b4-160">Potřebujete kniha s ISBN rovno "978-0-7356-7942-9", pošlete požadavek GET na `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="8c9b4-161">Text odpovědi by měla vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-161">The response body should look similar to the following.</span></span> <span data-ttu-id="8c9b4-162">(Odsazeny, aby byl čitelnější.)</span><span class="sxs-lookup"><span data-stu-id="8c9b4-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="8c9b4-163">Všimněte si, že dynamické vlastnosti jsou zahrnuty vložené s deklarované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="8c9b4-164">POST Entity</span><span class="sxs-lookup"><span data-stu-id="8c9b4-164">POST an Entity</span></span>

<span data-ttu-id="8c9b4-165">Přidání entity adresáře, odeslání požadavek POST do `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="8c9b4-166">Klienta můžete nastavit dynamické vlastnosti v datová část požadavku.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="8c9b4-167">Tady je příklad požadavek.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-167">Here is an example request.</span></span> <span data-ttu-id="8c9b4-168">Poznámka: vlastnosti "Cena" a "Publikováno".</span><span class="sxs-lookup"><span data-stu-id="8c9b4-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="8c9b4-169">Pokud jste nastavili zarážka v metodě řadiče, uvidíte, webového rozhraní API přidat tyto vlastnosti, které chcete `Properties` slovníku.</span><span class="sxs-lookup"><span data-stu-id="8c9b4-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="8c9b4-170">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="8c9b4-170">Additional Resources</span></span>

[<span data-ttu-id="8c9b4-171">Ukázka typu OData Open</span><span class="sxs-lookup"><span data-stu-id="8c9b4-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
