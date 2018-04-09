---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animace ovládacího prvku UpdatePanel (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5d8d5b9c3f15b39045b5e01b455bdddfc9443a24
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-c"></a>Animace ovládacího prvku UpdatePanel (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah ovládacího prvku UpdatePanel speciální rozšiřujícího objektu existuje, která je založena na rozhraní animace: UpdatePanelAnimation. Tento kurz ukazuje, jak nastavit takové animace UpdatePanel.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah `UpdatePanel`, existuje speciální rozšiřujícího objektu, který založena na rozhraní animace: `UpdatePanelAnimation`. Tento kurz ukazuje, jak nastavit takové animace pro `UpdatePanel`.

## <a name="steps"></a>Kroky

Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

Animace v tomto scénáři se použijí pro technologie ASP.NET `Wizard` umístěných v ovládací prvek webu `UpdatePanel`. Tři kroky (libovolný) poskytují dostatek možností pro aktivaci postback:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

Kód potřebné pro `UpdatePanelAnimationExtender` je velmi podobný kód používá pro ovládací prvek `AnimationExtender`. V `TargetControlID` atribut poskytujeme `ID` z `UpdatePanel` pro animaci; v rámci `UpdatePanelAnimationExtender` ovládací prvek, `<Animations>` element obsahuje kód XML pro animation(s). Je však jeden rozdíl: velikost události a obslužné rutiny událostí je omezená ve srovnání s `AnimationExtender`. Pro `UpdatePanels`, pouze dvě z nich neexistuje:

- `<OnUpdated>` Když se aktualizovala UpdatePanel
- `<OnUpdating>` Při spuštění UpdatePanel aktualizace

V tomto scénáři nový obsah `UpdatePanel` (po postback) se objevovat se. Toto je nezbytné značek pro tento:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Nyní vždy, když v rámci prvku UpdatePanel dojde k zpětné volání, nový obsah panelu objevovat se bez problémů.


[![V dalším kroku průvodce je pozvolného vysouvání](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

V dalším kroku průvodce je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](changing-an-animation-using-client-side-code-cs.md)
> [další](dynamically-controlling-updatepanel-animations-cs.md)
