---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Komplexní dědičnost typů v OData v4 s rozhraním ASP.NET Web API | Dokumentace Microsoftu
author: microsoft
description: Podle specifikace v4 pracovní verze protokolu OData komplexní typ může dědit z jiného komplexního typu. (Komplexní typ je strukturovaného typu bez klíče.) Webové rozhraní API...
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d295a6ae20f5771ae1f4f28166f7e651b6ec5c58
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824027"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Komplexní dědičnost typů v OData v4 s rozhraním ASP.NET Web API
====================
podle [Microsoft](https://github.com/microsoft)

> Podle OData v4 [specifikace](http://www.odata.org/documentation/odata-version-4-0/), komplexní typ může dědit z jiného komplexního typu. (A *komplexní* typ je typ structured bez klíče.) Web API OData 5.3 podporuje komplexní dědičnost typů.
> 
> Toto téma ukazuje, jak vytvářet entity data model (EDM) s komplexní dědičnost typů. Úplný zdrojový kód, naleznete v tématu [OData komplexní typ dědičnosti ukázka](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Hierarchie modelu

Pro ilustraci komplexní dědičnost typů, používáme následující hierarchie tříd.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` je abstraktní komplexního typu. `Rectangle`, `Triangle`, a `Circle` jsou komplexní typy odvozené z `Shape`, a `RoundRectangle` je odvozena z `Rectangle`. `Window` je typ entity a obsahuje `Shape` instance.

Tady jsou třídy CLR, které definují tyto typy.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Sestavení modelu EDM

Chcete-li vytvořit EDM, můžete použít **ODataConventionModelBuilder**, která odvodí vztahy dědičnosti z typů CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Můžete také sestavit EDM explicitně, pomocí **Tvůrce ODataModelBuilder**. To trvá více kódu, ale dává větší kontrolu nad modelu EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Tyto dva příklady vytvoření stejné schéma modelu EDM.

## <a name="metadata-document"></a>Dokument metadat

Tady je dokument metadat OData, zobrazuje komplexní dědičnost typů.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Dokument metadat uvidíte, které:

- `Shape` Komplexní typ je abstraktní.
- `Rectangle`, `Triangle`, A `Circle` komplexního typu mít základní typ `Shape`.
- `RoundRectangle` Typ má základní typ `Rectangle`.

## <a name="casting-complex-types"></a>Přetypování komplexní typy

Přetypování na komplexní typy se teď podporuje. Například následující dotaz přetypování `Shape` k `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Tady je datové části odpovědi:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
