---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: "Animace ovládacího prvku UpdatePanel (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d1056fc798e22254e94e5cad54436576a297f7d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-vb"></a>Animace ovládacího prvku UpdatePanel (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah ovládacího prvku UpdatePanel speciální rozšiřujícího objektu existuje, která je založena na rozhraní animace: UpdatePanelAnimation. Tento kurz ukazuje, jak nastavit takové animace UpdatePanel.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah `UpdatePanel`, existuje speciální rozšiřujícího objektu, který založena na rozhraní animace: `UpdatePanelAnimation`. Tento kurz ukazuje, jak nastavit takové animace pro `UpdatePanel`.

## <a name="steps"></a>Kroky

Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Animace v tomto scénáři se použijí pro technologie ASP.NET `Wizard` umístěných v ovládací prvek webu `UpdatePanel`. Tři kroky (libovolný) poskytují dostatek možností pro aktivaci postback:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Kód potřebné pro `UpdatePanelAnimationExtender` je velmi podobný kód používá pro ovládací prvek `AnimationExtender`. V `TargetControlID` atribut poskytujeme `ID` z `UpdatePanel` pro animaci; v rámci `UpdatePanelAnimationExtender` ovládací prvek, `<Animations>` element obsahuje kód XML pro animation(s). Je však jeden rozdíl: velikost události a obslužné rutiny událostí je omezená ve srovnání s `AnimationExtender`. Pro `UpdatePanels`, pouze dvě z nich neexistuje:

- `<OnUpdated>`Když se aktualizovala UpdatePanel
- `<OnUpdating>`Při spuštění UpdatePanel aktualizace

V tomto scénáři nový obsah `UpdatePanel` (po postback) se objevovat se. Toto je nezbytné značek pro tento:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Nyní vždy, když v rámci prvku UpdatePanel dojde k zpětné volání, nový obsah panelu objevovat se bez problémů.


[![V dalším kroku průvodce je pozvolného vysouvání](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

V dalším kroku průvodce je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](animating-an-updatepanel-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](changing-an-animation-using-client-side-code-vb.md)
[další](dynamically-controlling-updatepanel-animations-vb.md)
