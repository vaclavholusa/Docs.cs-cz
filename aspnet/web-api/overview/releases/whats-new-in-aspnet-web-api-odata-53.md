---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Co je nového v technologii ASP.NET Web API OData 5.3 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566737"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="31186-102">Co je nového v technologii ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="31186-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="31186-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="31186-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="31186-104">Toto téma popisuje, co je nového pro ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="31186-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="31186-105">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="31186-105">Download</span></span>](#download)
- [<span data-ttu-id="31186-106">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="31186-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="31186-107">Základní OData knihovny</span><span class="sxs-lookup"><span data-stu-id="31186-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="31186-108">Nové funkce</span><span class="sxs-lookup"><span data-stu-id="31186-108">New Features</span></span>](#newf)
- [<span data-ttu-id="31186-109">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="31186-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="31186-110">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="31186-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="31186-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="31186-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="31186-112">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="31186-112">Download</span></span>

<span data-ttu-id="31186-113">Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="31186-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="31186-114">Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="31186-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="31186-115">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="31186-115">Documentation</span></span>

<span data-ttu-id="31186-116">Kurzy a další dokumentaci o ASP.NET Web API OData na můžete najít [webové stránky ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="31186-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="31186-117">Základní OData knihovny</span><span class="sxs-lookup"><span data-stu-id="31186-117">OData Core Libraries</span></span>

<span data-ttu-id="31186-118">Webové rozhraní API pro OData v4, teď používá ODataLib verze 6.5.0</span><span class="sxs-lookup"><span data-stu-id="31186-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="31186-119">Nové funkce technologie ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="31186-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="31186-120">Podpora pro $levels v $rozbalte</span><span class="sxs-lookup"><span data-stu-id="31186-120">Support for $levels in $expand</span></span>

<span data-ttu-id="31186-121">Můžete použít $levels dotazu v $rozbalte dotazy.</span><span class="sxs-lookup"><span data-stu-id="31186-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="31186-122">Příklad:</span><span class="sxs-lookup"><span data-stu-id="31186-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="31186-123">Tento dotaz je ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="31186-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="31186-124">Podpora pro typy otevřete entit</span><span class="sxs-lookup"><span data-stu-id="31186-124">Support for Open Entity Types</span></span>

<span data-ttu-id="31186-125">*Otevřete typ* je stuctured typu, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarované v definici typu.</span><span class="sxs-lookup"><span data-stu-id="31186-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="31186-126">Otevřete typy umožňují zvýšit flexibilitu datové modely.</span><span class="sxs-lookup"><span data-stu-id="31186-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="31186-127">Další informace najdete v tématu xxxx.</span><span class="sxs-lookup"><span data-stu-id="31186-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="31186-128">Podpora pro dynamické kolekce vlastností v otevřené typy</span><span class="sxs-lookup"><span data-stu-id="31186-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="31186-129">Dříve dynamických vlastností musel být jedna hodnota.</span><span class="sxs-lookup"><span data-stu-id="31186-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="31186-130">V 5.3 může mít dynamické vlastnosti kolekce hodnoty.</span><span class="sxs-lookup"><span data-stu-id="31186-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="31186-131">Například v následujícím datové části JSON `Emails` vlastnost je dynamická vlastnost a je kolekce typu řetězec:</span><span class="sxs-lookup"><span data-stu-id="31186-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="31186-132">Podpora pro dědičnosti pro komplexní typy</span><span class="sxs-lookup"><span data-stu-id="31186-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="31186-133">Komplexní typy lze nyní dědit ze základního typu.</span><span class="sxs-lookup"><span data-stu-id="31186-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="31186-134">Službě OData třeba definovat následující komplexní typy:</span><span class="sxs-lookup"><span data-stu-id="31186-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="31186-135">Tady je EDM pro tento příklad:</span><span class="sxs-lookup"><span data-stu-id="31186-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="31186-136">Další informace najdete v tématu [OData komplexní typ dědičnosti ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="31186-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="31186-137">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="31186-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="31186-138">Tato část popisuje známé problémy a nejnovějších změn v ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="31186-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="31186-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="31186-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="31186-140">Možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="31186-140">Query Options</span></span>

<span data-ttu-id="31186-141">Problém: Použití vnořených $rozbalte s $levels = maximálního počtu výsledků v hloubku nesprávné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="31186-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="31186-142">Například uděleno následující požadavek:</span><span class="sxs-lookup"><span data-stu-id="31186-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="31186-143">Pokud `MaxExpansionDepth` je 5, tento dotaz povedou hloubku rozšíření 6.</span><span class="sxs-lookup"><span data-stu-id="31186-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="31186-144">Opravy chyb a aktualizací menší funkce</span><span class="sxs-lookup"><span data-stu-id="31186-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="31186-145">Tato verze rovněž obsahuje několik oprav chyb a menší funkce aktualizace.</span><span class="sxs-lookup"><span data-stu-id="31186-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="31186-146">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="31186-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="31186-147">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="31186-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="31186-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="31186-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="31186-149">V této verzi jsme provedli [oprava chyby](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) k některým AllowedFunctions výčty.</span><span class="sxs-lookup"><span data-stu-id="31186-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="31186-150">Tato verze nemá ostatní opravy chyb nebo nové funkce.</span><span class="sxs-lookup"><span data-stu-id="31186-150">This release doesn't have any other bug fixes or new features.</span></span>
