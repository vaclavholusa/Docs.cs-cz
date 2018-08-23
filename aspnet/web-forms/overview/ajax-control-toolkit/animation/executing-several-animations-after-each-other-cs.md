---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Postupné provedení několika animací (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Umožňuje spustit severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 60fb7dacfe59eaafef71fcc9cde61b7840624343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752925"
---
<a name="executing-several-animations-after-each-other-c"></a>Postupné provedení několika animací (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Umožňuje spuštění několika animací jednu po druhé.


Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Umožňuje spuštění několika animací jednu po druhé.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile úplným načtením stránky. Obecně platí `<OnLoad>` přijímá pouze jeden animace. Animace framework umožňuje připojit k několika animací do jednoho pomocí `<Sequence>` elementu. Všechny animace v rámci `<Sequence>` jsou spuštěný jeden po druhém. Tady je možné značky `AnimationExtender` ovládací prvek, nejprve Příprava panelu širší a zmenšení výšku:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Při spuštění tohoto skriptu panelu první získá širší a potom menší.


[![Nejprve je vyšší šířku](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Nejprve je vyšší šířku ([kliknutím ji zobrazíte obrázek v plné velikosti](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Pak je snížení výšky](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Pak se sníží výška ([kliknutím ji zobrazíte obrázek v plné velikosti](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](executing-several-animations-at-the-same-time-cs.md)
> [další](animation-depending-on-a-condition-cs.md)
