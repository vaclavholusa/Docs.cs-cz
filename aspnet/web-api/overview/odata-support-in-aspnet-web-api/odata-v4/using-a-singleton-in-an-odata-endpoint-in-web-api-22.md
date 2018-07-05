---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Vytvoření jednoho prvku v OData v4 použití rozhraní Web API 2.2 | Dokumentace Microsoftu
author: rick-anderson
description: Toto téma ukazuje, jak definovat typ singleton v koncový bod OData v Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3ae9b23fae356e387a011a190119d760dc46d022
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390924"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="36001-103">Vytvoření jednoho prvku v OData v4 použití rozhraní Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="36001-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="36001-104">podle Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="36001-104">by Zoe Luo</span></span>

> <span data-ttu-id="36001-105">Tradičně entita může přistupovat pouze pokud byly zapouzdřený v sadu entit.</span><span class="sxs-lookup"><span data-stu-id="36001-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="36001-106">Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a členství ve skupině, které podporuje WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="36001-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="36001-107">Tento článek ukazuje, jak definovat typ singleton v koncový bod OData v Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="36001-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="36001-108">Informace o jaký typ singleton je a jak vám může přinést použití, naleznete v tématu [použití typu singleton k definování zvláštní entita](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="36001-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="36001-109">Vytvoření koncového bodu OData V4 v rozhraní Web API najdete v tématu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="36001-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="36001-110">Vytvoříme jednotlivý prvek v projektu webového rozhraní API pomocí modelu následující:</span><span class="sxs-lookup"><span data-stu-id="36001-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Datový Model](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="36001-112">Jednotlivý prvek s názvem `Umbrella` se určí na základě typu `Company`a sadu pojmenovaných entit `Employees` se určí na základě typu `Employee`.</span><span class="sxs-lookup"><span data-stu-id="36001-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="36001-113">Řešení použité v tomto kurzu si můžete stáhnout z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="36001-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="36001-114">Definování datového modelu</span><span class="sxs-lookup"><span data-stu-id="36001-114">Define the data model</span></span>

1. <span data-ttu-id="36001-115">Definování typů CLR.</span><span class="sxs-lookup"><span data-stu-id="36001-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="36001-116">Generovat model EDM založený na typy CLR.</span><span class="sxs-lookup"><span data-stu-id="36001-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="36001-117">Tady `builder.Singleton<Company>("Umbrella")` říká Tvůrce modelu vytvořit jednotlivý prvek s názvem `Umbrella` v modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="36001-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="36001-118">Vygenerovaný metadata bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="36001-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="36001-119">Z metadat můžeme vidět, který navigační vlastnost `Company` v `Employees` sady entit je vázán na jednotlivý prvek `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="36001-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="36001-120">Vazba se provádí automaticky `ODataConventionModelBuilder`, protože pouze `Umbrella` má `Company` typu.</span><span class="sxs-lookup"><span data-stu-id="36001-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="36001-121">Pokud je model veškerou nejednoznačnost, můžete použít `HasSingletonBinding` explicitně svázat vlastnost navigace typu singleton; `HasSingletonBinding` má stejný účinek jako použití `Singleton` atributu v definici typu CLR:</span><span class="sxs-lookup"><span data-stu-id="36001-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="36001-122">Definování typu singleton kontroleru</span><span class="sxs-lookup"><span data-stu-id="36001-122">Define the singleton controller</span></span>

<span data-ttu-id="36001-123">Jako řadič EntitySet kontroleru typu singleton dědí z `ODataController`, a názvu kontroleru jednotlivý prvek by měl být `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="36001-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="36001-124">Aby bylo možné zpracovávat různé druhy žádostí, akce musí být předem definované v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="36001-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="36001-125">**Atribut směrování** povolena ve výchozím nastavení WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="36001-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="36001-126">Například chcete-li definovat akci, kterou chcete zpracovat dotazování `Revenue` z `Company` používající směrování na atribut, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="36001-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="36001-127">Pokud nepřijmete definují atributy pro každou akci, stačí definovat akce po [konvencí směrování protokolu OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="36001-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="36001-128">Protože klíč není vyžadována pro dotazování typu singleton, se mírně liší z akcí, které jsou definované v kontroleru entityset akce definované v kontroleru typu singleton.</span><span class="sxs-lookup"><span data-stu-id="36001-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="36001-129">Pro srovnání podpisy metod pro každou definici akce v kontroleru typu singleton jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="36001-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="36001-130">V podstatě to je všechno, co musíte udělat na straně služby.</span><span class="sxs-lookup"><span data-stu-id="36001-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="36001-131">[Ukázkový projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) obsahuje kód pro řešení a klient OData, který ukazuje způsob použití typu singleton.</span><span class="sxs-lookup"><span data-stu-id="36001-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="36001-132">Klient je sestavena podle kroků v [vytvoření klientské aplikace OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="36001-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="36001-133">.</span><span class="sxs-lookup"><span data-stu-id="36001-133">.</span></span> 

<span data-ttu-id="36001-134">*Děkujeme, že k leo by patřil velice Hu původní obsah tohoto článku.*</span><span class="sxs-lookup"><span data-stu-id="36001-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
