---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Omezení v OData v4 použití rozhraní Web API 2.2 | Microsoft Docs"
author: rick-anderson
description: "Obvyklým entita může používat pouze v případě měla zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="d5d4d-104">Omezení v OData v4 použití rozhraní Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="d5d4d-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="d5d4d-105">podle Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="d5d4d-105">by Jinfu Tan</span></span>

> <span data-ttu-id="d5d4d-106">Obvyklým entita může používat pouze v případě měla zapouzdřený v sadu entit.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="d5d4d-107">Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a členství ve skupině, které podporuje WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="d5d4d-108">Toto téma ukazuje, jak definovat omezení v koncový bod OData v WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="d5d4d-109">Další informace o členství ve skupině najdete v tématu [členství ve skupině pochází s OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5d4d-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="d5d4d-110">Vytvořit koncový bod OData V4 v rozhraní Web API, přečtěte si článek [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d5d4d-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="d5d4d-111">Nejdříve vytvoříme modelu omezení domény ve službě OData pomocí tento datový model:</span><span class="sxs-lookup"><span data-stu-id="d5d4d-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Datový model](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="d5d4d-113">Účet obsahuje mnoho PaymentInstruments (PI), ale nemáte definujeme entitu pí.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="d5d4d-114">Místo toho můžete instrukce pro zpracování přistupovat pouze prostřednictvím účtu.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="d5d4d-115">Si můžete stáhnout řešení použitým v tomto tématu z [webu CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="d5d4d-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="d5d4d-116">Definování datový model</span><span class="sxs-lookup"><span data-stu-id="d5d4d-116">Defining the data model</span></span>

1. <span data-ttu-id="d5d4d-117">Definování typů CLR.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="d5d4d-118">`Contained` Atribut se používá pro zahrnutí navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="d5d4d-119">Generovat model EDM založený na typy CLR.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="d5d4d-120">`ODataConventionModelBuilder` Zpracuje vytváření EDM model, pokud `Contained` přidání atributu do odpovídající navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="d5d4d-121">Pokud je vlastnost typu kolekce `GetCount(string NameContains)` bude vytvořen také funkce.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="d5d4d-122">Vygenerovaný metadata bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d5d4d-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="d5d4d-123">`ContainsTarget` Atribut označuje, že je vlastnost navigace členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="d5d4d-124">Definování řadičem obsahující sadu entit</span><span class="sxs-lookup"><span data-stu-id="d5d4d-124">Define the containing entity set controller</span></span>

<span data-ttu-id="d5d4d-125">Obsažené entity nemají své vlastní řadiče; akce je definované v kontroleru obsahující sadu entit.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="d5d4d-126">V této ukázce je AccountsController, ale žádné PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="d5d4d-127">Pokud cestu OData 4 nebo více segmenty, pouze atribut směrování funguje, jako například `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` ve výše uvedené kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="d5d4d-128">Jinak hodnota atributu a standardní metody směrování funguje: například `GetPayInPIs(int key)` odpovídá `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="d5d4d-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="d5d4d-129">*Díky Leo Hu pro původní obsah tohoto článku.*</span><span class="sxs-lookup"><span data-stu-id="d5d4d-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
