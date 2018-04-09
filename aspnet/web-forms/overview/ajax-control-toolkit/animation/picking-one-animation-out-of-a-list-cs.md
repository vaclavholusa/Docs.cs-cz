---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Výběr jednoho animace mimo seznam (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Rozhraní framework také povolit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d4aac447fcdfbf296560091cfcdf5eb51997a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="picking-one-animation-out-of-a-list-c"></a>Výběr jednoho animace mimo seznam (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Rozhraní framework také umožňuje programátorů vybrat jeden animace mimo seznam animace, v závislosti na vyhodnocení určitý kód JavaScript.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Rozhraní framework také umožňuje programátorů vybrat jeden animace mimo seznam animace, v závislosti na vyhodnocení určitý kód JavaScript.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky. Místo mezi regulární animace `<Case>` element se stává play. Hodnota atributu jeho SelectScript vyhodnotí; Návratová hodnota musí být číselná. V závislosti na toto číslo, jeden z subanimations v rámci &lt;případ&gt; se spustí. Například pokud je výsledkem SelectScript 2, Toolkitu spouští třetí animace v rámci &lt;případ&gt; (počítání spustí 0).

Následující kód definuje tři subanimations: Změna velikosti šířku, změny velikosti výšku a pozvolného vysouvání. Kód jazyka JavaScript (`Math.floor(3 * Math.random())`) potom vybere číslo mezi 0 a 2, tak, aby mezi tři animace se spustí:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![Jedním z možných tři animací: panelu získá širší](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Jedním z možných tři animací: panelu získá širší ([Kliknutím zobrazit obrázek v plné velikosti](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animation-depending-on-a-condition-cs.md)
> [další](animating-in-response-to-user-interaction-cs.md)
