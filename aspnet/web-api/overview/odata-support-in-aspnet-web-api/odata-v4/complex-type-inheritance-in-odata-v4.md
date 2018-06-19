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
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Komplexní typ dědičnosti v OData v4 s rozhraním ASP.NET Web API
====================
podle [Microsoft](https://github.com/microsoft)

> Podle OData v4 [specifikace](http://www.odata.org/documentation/odata-version-4-0/), komplexního typu. může dědit vlastnosti z jiného komplexního typu. (A *komplexní* typ je strukturovaného typu bez klíče.) Web API OData 5.3 podporuje dědičnosti komplexního typu.
> 
> Toto téma ukazuje, jak k vytvoření datového modelu entity (EDM) s dědičnosti komplexní typy. Úplný zdrojový kód, najdete v části [OData komplexní typ dědičnosti ukázka](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Model hierarchie

Pro ilustraci dědičnosti komplexní typ, použijeme následující hierarchie tříd.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`je abstraktní komplexního typu. `Rectangle`, `Triangle`, a `Circle` komplexní typy jsou odvozeny od `Shape`, a `RoundRectangle` je odvozena z `Rectangle`. `Window`Typ entity a obsahuje `Shape` instance.

Tady jsou třídy CLR, které definují tyto typy.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Vytvoření modelu EDM

Pokud chcete vytvořit EDM, můžete použít **ODataConventionModelBuilder**, který odvodí vztahy dědičnost z typů CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Můžete také vytvořit EDM explicitně, pomocí **Tvůrce ODataModelBuilder**. To provede další kód, ale vám dává větší kontrolu nad EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Tyto dva příklady vytvořit stejné schéma EDM.

## <a name="metadata-document"></a>Dokument metadat

Zde je dokument metadat OData, zobrazení dědičnosti komplexního typu.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Z dokumentu metadat můžete zjistit, který:

- `Shape` Komplexní typ je abstraktní.
- `Rectangle`, `Triangle`, A `Circle` komplexní typ mít základní typ. `Shape`.
- `RoundRectangle` Typ má základní typ `Rectangle`.

## <a name="casting-complex-types"></a>Přetypování komplexní typy

Přetypování na komplexní typy se teď podporuje. Například následující dotaz přetypování `Shape` k `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Tady je datové části odpovědi:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
