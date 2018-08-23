---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animace ovládacího prvku UpdatePanel (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Pro obsah...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 797ee37eb440bed261403aa0e1b68f38d3cd8ef9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752435"
---
<a name="animating-an-updatepanel-control-vb"></a>Animace ovládacího prvku UpdatePanel (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Obsah prvku UpdatePanel speciální rozšiřující objekt existuje, která se spoléhá na rozhraní .NET framework animace: UpdatePanelAnimation. Tento kurz ukazuje, jak nastavit tyto animace ovládacího prvku UpdatePanel.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Pro obsah `UpdatePanel`, speciální rozšiřující objekt existuje, která se spoléhá na rozhraní .NET framework animace: `UpdatePanelAnimation`. Tento kurz ukazuje, jak nastavit tyto animace pro `UpdatePanel`.

## <a name="steps"></a>Kroky

Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby je načtena knihovna ASP.NET AJAX a Control Toolkit je možné:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Animace v tomto scénáři se použijí pro ASP.NET `Wizard` webový ovládací prvek umístěný v `UpdatePanel`. Tři kroky (libovolný) obsahují dostatek možnosti k aktivaci zpětného odeslání:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Pro značky `UpdatePanelAnimationExtender` je velmi podobný značky použité pro ovládací prvek `AnimationExtender`. V `TargetControlID` atribut zajišťuje `ID` z `UpdatePanel` pro animaci; v rámci `UpdatePanelAnimationExtender` ovládací prvek, `<Animations>` element obsahuje kód XML pro animace. Ale není jedním z rozdílů: omezen objem událostí a obslužných rutin událostí ve srovnání s `AnimationExtender`. Pro `UpdatePanels`, pouze dva z nich neexistuje:

- `<OnUpdated>` Pokud byl aktualizován prvku UpdatePanel
- `<OnUpdating>` Aktualizuje se při spuštění prvku UpdatePanel

V tomto scénáři, nový obsah `UpdatePanel` (po zpětném volání) se má vyblednout. To je nezbytné zápis, který:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Nyní pokaždé, když se vyvolá se v rámci prvku UpdatePanel zpětné volání, nový obsah na panelu fade plynule.


[![V dalším kroku průvodce se pozvolného](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

V dalším kroku průvodce se pozvolného ([kliknutím ji zobrazíte obrázek v plné velikosti](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](changing-an-animation-using-client-side-code-vb.md)
> [další](dynamically-controlling-updatepanel-animations-vb.md)
