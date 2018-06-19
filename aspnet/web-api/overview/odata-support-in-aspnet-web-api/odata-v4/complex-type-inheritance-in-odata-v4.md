---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Komplexní typ dědičnosti v OData v4 s rozhraním ASP.NET Web API | Microsoft Docs
author: microsoft
description: Podle specifikace prostředí OData v4 komplexního typu. může dědit vlastnosti z jiného komplexního typu. (Pro komplexní typ je strukturovaného typu bez klíče.) Webové rozhraní API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566830"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="f0797-104">Komplexní typ dědičnosti v OData v4 s rozhraním ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f0797-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="f0797-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f0797-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f0797-106">Podle OData v4 [specifikace](http://www.odata.org/documentation/odata-version-4-0/), komplexního typu. může dědit vlastnosti z jiného komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="f0797-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="f0797-107">(A *komplexní* typ je strukturovaného typu bez klíče.) Web API OData 5.3 podporuje dědičnosti komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="f0797-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="f0797-108">Toto téma ukazuje, jak k vytvoření datového modelu entity (EDM) s dědičnosti komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="f0797-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="f0797-109">Úplný zdrojový kód, najdete v části [OData komplexní typ dědičnosti ukázka](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="f0797-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f0797-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="f0797-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f0797-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="f0797-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="f0797-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="f0797-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="f0797-113">Model hierarchie</span><span class="sxs-lookup"><span data-stu-id="f0797-113">Model Hierarchy</span></span>

<span data-ttu-id="f0797-114">Pro ilustraci dědičnosti komplexní typ, použijeme následující hierarchie tříd.</span><span class="sxs-lookup"><span data-stu-id="f0797-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="f0797-115">`Shape`je abstraktní komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="f0797-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="f0797-116">`Rectangle`, `Triangle`, a `Circle` komplexní typy jsou odvozeny od `Shape`, a `RoundRectangle` je odvozena z `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="f0797-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="f0797-117">`Window`Typ entity a obsahuje `Shape` instance.</span><span class="sxs-lookup"><span data-stu-id="f0797-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="f0797-118">Tady jsou třídy CLR, které definují tyto typy.</span><span class="sxs-lookup"><span data-stu-id="f0797-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="f0797-119">Vytvoření modelu EDM</span><span class="sxs-lookup"><span data-stu-id="f0797-119">Build the EDM Model</span></span>

<span data-ttu-id="f0797-120">Pokud chcete vytvořit EDM, můžete použít **ODataConventionModelBuilder**, který odvodí vztahy dědičnost z typů CLR.</span><span class="sxs-lookup"><span data-stu-id="f0797-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="f0797-121">Můžete také vytvořit EDM explicitně, pomocí **Tvůrce ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="f0797-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="f0797-122">To provede další kód, ale vám dává větší kontrolu nad EDM.</span><span class="sxs-lookup"><span data-stu-id="f0797-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="f0797-123">Tyto dva příklady vytvořit stejné schéma EDM.</span><span class="sxs-lookup"><span data-stu-id="f0797-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="f0797-124">Dokument metadat</span><span class="sxs-lookup"><span data-stu-id="f0797-124">Metadata Document</span></span>

<span data-ttu-id="f0797-125">Zde je dokument metadat OData, zobrazení dědičnosti komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="f0797-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="f0797-126">Z dokumentu metadat můžete zjistit, který:</span><span class="sxs-lookup"><span data-stu-id="f0797-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="f0797-127">`Shape` Komplexní typ je abstraktní.</span><span class="sxs-lookup"><span data-stu-id="f0797-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="f0797-128">`Rectangle`, `Triangle`, A `Circle` komplexní typ mít základní typ. `Shape`.</span><span class="sxs-lookup"><span data-stu-id="f0797-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="f0797-129">`RoundRectangle` Typ má základní typ `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="f0797-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="f0797-130">Přetypování komplexní typy</span><span class="sxs-lookup"><span data-stu-id="f0797-130">Casting Complex Types</span></span>

<span data-ttu-id="f0797-131">Přetypování na komplexní typy se teď podporuje.</span><span class="sxs-lookup"><span data-stu-id="f0797-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="f0797-132">Například následující dotaz přetypování `Shape` k `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="f0797-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="f0797-133">Tady je datové části odpovědi:</span><span class="sxs-lookup"><span data-stu-id="f0797-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
