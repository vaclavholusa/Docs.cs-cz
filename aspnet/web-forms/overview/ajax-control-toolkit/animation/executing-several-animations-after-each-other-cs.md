---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: "Provádění několik animací po sobě navzájem (C#) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit severa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d4322690132fe3829e3454f0aa7ff38acd8eb04
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-c"></a>Provádění několik animací po sobě navzájem (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit několik animací jedna po druhé.


V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit několik animací jedna po druhé.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj`runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky. Obecně platí `<OnLoad>` přijímá pouze jeden animace. Rozhraní framework animace umožňuje připojení k několika animace do jednoho pomocí `<Sequence>` elementu. Všechny animace v rámci `<Sequence>` jsou prováděny jedna po druhé. Tady je je možné kód pro `AnimationExtender` řízení, nejprve Příprava širší panelu a pak snížení jeho výšku:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Při spuštění tohoto skriptu panelu první získá širší a pak menší.


[![Nejprve je vyšší šířku](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Nejprve je vyšší šířku ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Pak je zmenšit výšku](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Pak je zmenšit výšku ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-after-each-other-cs/_static/image6.png))

>[!div class="step-by-step"]
[Předchozí](executing-several-animations-at-the-same-time-cs.md)
[další](animation-depending-on-a-condition-cs.md)
