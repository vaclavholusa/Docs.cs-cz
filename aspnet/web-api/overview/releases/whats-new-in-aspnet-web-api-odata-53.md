---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Co je nového v ASP.NET Web API OData 5.3 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753119"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Co je nového v ASP.NET Web API OData 5.3
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového v ASP.NET Web API OData 5.3.

- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [OData základní knihovny](#corelib)
- [Nové funkce](#newf)
- [Známé problémy a změny způsobující chyby](#known-issues)
- [Opravy chyb](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet. Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Můžete najít kurzy a další dokumentace o ASP.NET Web API OData na [Web ASP.NET s](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData základní knihovny

Pro OData v4 webové rozhraní API teď používá ODataLib verze 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Novinky v ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Podpora rozbalte $levels $

Můžete použít $levels dotazu v $rozbalte dotazy. Příklad:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Tento dotaz je ekvivalentní:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Podpora pro typy Open entit

*Otevřený typ.* stuctured typ, který obsahuje dynamické vlastnosti, kromě všechny vlastnosti, které jsou deklarovány v definici typu. Otevřené typy umožňují zvýšit flexibilitu datových modelů. Další informace najdete v tématu xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Podpora pro dynamické kolekce vlastností v otevřené typy

Dříve musely být jedinou hodnotu dynamických vlastností. V 5.3 může mít dynamické vlastnosti kolekce hodnot. Například v následujícím datové části JSON `Emails` vlastnost dynamických vlastností a je kolekce typu řetězec:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Podpora dědičnosti pro komplexní typy

Komplexní typy nyní může dědit ze základního typu. Služby OData může například definovat následující komplexní typy:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Tady je EDM pro tento příklad:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Další informace najdete v tématu [OData komplexní typ dědičnosti ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

Tato část popisuje známé problémy a novinkách v ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Možnosti dotazu

Problém: Použití vnořených $ s $levels rozšiřovat = maximální výsledky do hloubky nesprávné rozšíření.

Mějme například následující žádosti:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Pokud `MaxExpansionDepth` je 5, tento dotaz by vést hloubku rozšíření 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Opravy chyb a aktualizace podverze funkcí

Tato verze zahrnuje také několik oprav chyb a dílčí funkce aktualizace. Úplný seznam najdete:

- [Opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

V této verzi jsme provedli [oprava chyby](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) k některým AllowedFunctions výčtů. Tato verze neobsahuje žádné opravy chyb a nové funkce.
