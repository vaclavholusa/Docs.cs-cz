---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Výběr jednoho animace mimo seznam (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Rozhraní framework také povolit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872062"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Výběr jednoho animace mimo seznam (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Rozhraní framework také umožňuje programátorů vybrat jeden animace mimo seznam animace, v závislosti na vyhodnocení určitý kód JavaScript.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Rozhraní framework také umožňuje programátorů vybrat jeden animace mimo seznam animace, v závislosti na vyhodnocení určitý kód JavaScript.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky. Místo mezi regulární animace `<Case>` element se stává play. Hodnota atributu jeho SelectScript vyhodnotí; Návratová hodnota musí být číselná. V závislosti na toto číslo, jeden z subanimations v rámci &lt;případ&gt; se spustí. Například pokud je výsledkem SelectScript 2, Toolkitu spouští třetí animace v rámci &lt;případ&gt; (počítání spustí 0).

Následující kód definuje tři subanimations: Změna velikosti šířku, změny velikosti výšku a pozvolného vysouvání. Kód jazyka JavaScript (`Math.floor(3 * Math.random())`) potom vybere číslo mezi 0 a 2, tak, aby mezi tři animace se spustí:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Jedním z možných tři animací: panelu získá širší](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Jedním z možných tři animací: panelu získá širší ([Kliknutím zobrazit obrázek v plné velikosti](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animation-depending-on-a-condition-vb.md)
> [další](animating-in-response-to-user-interaction-vb.md)
