---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamicky řízení UpdatePanel animace (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 78401ee35027040dffea50c295c752dd182e75a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868604"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a>Dynamicky řízení UpdatePanel animace (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah ovládacího prvku UpdatePanel speciální rozšiřujícího objektu existuje, která je založena na rozhraní animace: UpdatePanelAnimation. Může spolupracovat taky společně s UpdatePanel aktivační události.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah `UpdatePanel`, existuje speciální rozšiřujícího objektu, který založena na rozhraní animace: `UpdatePanelAnimation`. Můžete také pracovat společně s `UpdatePanel` aktivační události.

## <a name="steps"></a>Kroky

Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Animace v tomto scénáři se použijí k zobrazení aktuálního času. Tyto informace je možné zapsat do popisek pomocí `Page_Load()` metoda, nebo (z důvodu zjednodušení) se používá následující vloženého kódu:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Také se vytvoří tlačítko pro aktivaci aktualizací času:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Tento kód je pak přesuňte do `<ContentTemplate>` části `UpdatePanel` elementu. Na panelu `UpdateMode` musí být nastaven `"Conditional"`, protože pouze aktivační události může aktualizovat obsah panelu. V `<Triggers>` části `UpdatePanel`, asynchronní postback aktivační událost se vytvoří a vázáno `Click` události tlačítka. Proto v případě, že uživatel klikne na tlačítko, `UpdatePanel` je aktualizovat. Zde je kód pro `UpdatePanel` ovládacího prvku:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Nakonec `UpdatePanelAnimationExtender` musí být nakonfigurované: nastavte `TargetControlID` atribut ID panelu a definujte animace v rámci rozšiřujícího objektu. Pozvolného vysouvání v díky smysl, který vytvoří dobrý visual důraz na čas poslední aktualizace. Váš kód rozšiřujícího objektu může pak vypadat takto:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Spusťte soubor v prohlížeči. Vždy, když kliknete na tlačítko se zobrazí aktuální čas v panelech vždy pozvolného vysouvání po dobu trvání jednu sekundu.


[![Aktuální čas je pozvolného vysouvání](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Aktuální čas je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animating-an-updatepanel-control-cs.md)
> [další](adding-animation-to-a-control-vb.md)
