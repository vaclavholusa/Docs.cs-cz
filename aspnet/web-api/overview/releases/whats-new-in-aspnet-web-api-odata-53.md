---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: "Co je nového v technologii ASP.NET Web API OData 5.3 | Microsoft Docs"
author: microsoft
description: 
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
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Co je nového v technologii ASP.NET Web API OData 5.3
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového pro ASP.NET Web API OData 5.3.

- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Základní OData knihovny](#corelib)
- [Nové funkce](#newf)
- [Známé problémy a nejnovější změny](#known-issues)
- [Opravy chyb](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet. Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další dokumentaci o ASP.NET Web API OData na můžete najít [webové stránky ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Základní OData knihovny

Webové rozhraní API pro OData v4, teď používá ODataLib verze 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Nové funkce technologie ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Podpora pro $levels v $rozbalte

Můžete použít $levels dotazu v $rozbalte dotazy. Příklad:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Tento dotaz je ekvivalentní:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Podpora pro typy otevřete entit

*Otevřete typ* je stuctured typu, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarované v definici typu. Otevřete typy umožňují zvýšit flexibilitu datové modely. Další informace najdete v tématu xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Podpora pro dynamické kolekce vlastností v otevřené typy

Dříve dynamických vlastností musel být jedna hodnota. V 5.3 může mít dynamické vlastnosti kolekce hodnoty. Například v následujícím datové části JSON `Emails` vlastnost je dynamická vlastnost a je kolekce typu řetězec:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Podpora pro dědičnosti pro komplexní typy

Komplexní typy lze nyní dědit ze základního typu. Službě OData třeba definovat následující komplexní typy:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Tady je EDM pro tento příklad:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Další informace najdete v tématu [OData komplexní typ dědičnosti ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

Tato část popisuje známé problémy a nejnovějších změn v ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Možnosti dotazu

Problém: Použití vnořených $rozbalte s $levels = maximálního počtu výsledků v hloubku nesprávné rozšíření.

Například uděleno následující požadavek:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Pokud `MaxExpansionDepth` je 5, tento dotaz povedou hloubku rozšíření 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Opravy chyb a aktualizací menší funkce

Tato verze rovněž obsahuje několik oprav chyb a menší funkce aktualizace. Úplný seznam najdete:

- [Opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

V této verzi jsme provedli [oprava chyby](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) k některým AllowedFunctions výčty. Tato verze nemá ostatní opravy chyb nebo nové funkce.
