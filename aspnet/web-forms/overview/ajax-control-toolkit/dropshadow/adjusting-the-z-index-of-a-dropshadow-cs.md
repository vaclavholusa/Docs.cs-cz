---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: "Úprava Z-Index DropShadow (C#) | Microsoft Docs"
author: wenz
description: "DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Ale tento stínové někdy je v konfliktu s další ovládací prvky pro Nainstalo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Úprava Z-Index DropShadow (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Ale tento stínové někdy je v konfliktu s další ovládací prvky pro instanci ovládacího prvku Menu technologie ASP.NET. Pokud položku nabídky se zobrazí, se objeví za stín.


## <a name="overview"></a>Přehled

DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Ale tento stínové někdy je v konfliktu s další ovládací prvky pro instanci ovládacího prvku Menu technologie ASP.NET. Pokud položku nabídky se zobrazí, se objeví za stín.

## <a name="steps"></a>Kroky

Kód zahájí s panelem samostatně, obsahující dostatek text tak, aby panelu obsahuje dostatek text pro účinek viditelné:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Jiný panel je umístěno přímo před `panelShadow` panelu. Obsahuje nabídku s vodorovné orientaci tak, aby položky nabídky se zobrazují nad (nebo spíše: v části) `dropShadow` panely):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Potom, `DropShadowExtender` se přidá do rozšířit `panelShadow` panelu s efekt stínu rozevírací:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Nakonec prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Po spuštění tohoto skriptu položky nabídky se zobrazí pod panelu. Ale v nabídce používá třídu CSS `panel` kde právě musíte definovat dvě věci, aby elementy, které se zobrazují před jiné panelu:

- Relativní umístění
- Kladné z-index

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Potom, `DropShadowExtender` ovládací prvek není v konfliktu už pomocí ovládacího prvku nabídky.


[![Před: Položka nabídky není viditelná](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Předtím: Položka nabídky není viditelné ([Kliknutím zobrazit obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Po: Zobrazí se položka nabídky](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Po: Zobrazí se položka nabídky ([Kliknutím zobrazit obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

>[!div class="step-by-step"]
[Další](manipulating-dropshadow-properties-from-client-code-cs.md)
