---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animace v závislosti na podmínce (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Zda je animace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: b530239e76654bc68a8fa6ac900a20df1d5699b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878565"
---
<a name="animation-depending-on-a-condition-c"></a>Animace v závislosti na podmínce (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Zda je animace spustit nebo Ne můžete také závisí na podmínce v podobě určitý kód JavaScript.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Zda je animace spustit nebo Ne můžete také závisí na podmínce v podobě určitý kód JavaScript.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky. Místo mezi regulární animace `<Condition>` element se stává play. Kód jazyka JavaScript, který je zadaný jako hodnota `ConditionScript` atribut je provést v době běhu. Pokud se vyhodnotí jako true, animace se spustí, jinak není. Následující kód obsahuje dvě animací, každý z nich vykonáván v případech při náhodných 50 %. Vzhledem k tomu, že může existovat pouze jedna animace v rámci `<OnLoad>`, dva `<Condition>` animací připojeni pomocí `<Sequence>` element:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Všimněte si, že menší než přihlašovací (`<`) v `ConditionScript` atribut musí být uvozovacími znaky (). Při spuštění tohoto skriptu, buď žádná animace spuštění, nebo jednu ze dvou nemá nebo obě provést.


[![Na panelu je pozvolného vysouvání bez změny velikosti, tak druhý animace spustí první z nich nebyl](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Na panelu je pozvolného vysouvání bez změny velikosti, tak druhý animace spustí první z nich nebyl ([Kliknutím zobrazit obrázek v plné velikosti](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](executing-several-animations-after-each-other-cs.md)
> [další](picking-one-animation-out-of-a-list-cs.md)
