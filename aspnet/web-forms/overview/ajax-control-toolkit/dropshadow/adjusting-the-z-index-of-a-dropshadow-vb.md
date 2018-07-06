---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Úprava indexu z ovládacího prvku DropShadow (VB) | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku DropShadow rozšiřuje panel s vrhá stín. Ale stín někdy je v konfliktu s jinými ovládacími prvky pro insta...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 78697f51a09dfaad315255efa23120d4c456bfea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829541"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Úprava indexu z ovládacího prvku DropShadow (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> Sada nástrojů AJAX Control Toolkit ovládacího prvku DropShadow rozšiřuje panel s vrhá stín. Ale stín někdy je v konfliktu s jinými ovládacími prvky, například ovládací prvek ASP.NET nabídky. Pokud položka nabídky se zobrazí, se zobrazí za vrhá stín.


## <a name="overview"></a>Přehled

Sada nástrojů AJAX Control Toolkit ovládacího prvku DropShadow rozšiřuje panel s vrhá stín. Ale stín někdy je v konfliktu s jinými ovládacími prvky, například ovládací prvek ASP.NET nabídky. Pokud položka nabídky se zobrazí, se zobrazí za vrhá stín.

## <a name="steps"></a>Kroky

Kód začíná samostatně, Panel obsahující dostatečné množství textu, tak, aby panel obsahuje dostatek text pro efekt uvidí:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Jiného panelu je umístěna přímo před `panelShadow` panel. Obsahuje nabídku s vodorovné orientaci, tak, aby položky nabídek zobrazují nad (nebo spíše: v části) `dropShadow` panel):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Pak, bude `DropShadowExtender` se přidá k rozšíření `panelShadow` panelu s efektem stínu:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Nakonec technologie ASP.NET AJAX `ScriptManager` ovládací prvek umožňuje Control Toolkit pracovat:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Při spuštění tohoto skriptu se zobrazí pod panelu položky nabídky. Ale používá třídu šablony stylů CSS v nabídce `panel` kde stačí definovat dvě věci, aby se elementy zobrazí před na panelu:

- Relativní umístění
- Kladné z-index

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Pak, bude `DropShadowExtender` ovládací prvek není v konfliktu už pomocí ovládacího prvku nabídky.


[![Před: Položka nabídky se nezobrazuje](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Předtím: Položka nabídky není viditelný ([kliknutím ji zobrazíte obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Po: Zobrazí se položka nabídky](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Po: Zobrazí se položka nabídky ([kliknutím ji zobrazíte obrázek v plné velikosti](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](manipulating-dropshadow-properties-from-client-code-cs.md)
> [další](manipulating-dropshadow-properties-from-client-code-vb.md)
