---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Výběr jedné animace ze seznamu (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Rozhraní také povolit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 561f05e96888962cfe576963ce3905b171203939
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387738"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Výběr jedné animace ze seznamu (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Rozhraní framework umožňuje také programátora pro výběr jedné animace ze seznamu animace, v závislosti na hodnocení kódu jazyka JavaScript.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Rozhraní framework umožňuje také programátora pro výběr jedné animace ze seznamu animace, v závislosti na hodnocení kódu jazyka JavaScript.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile úplným načtením stránky. Místo jednoho regulární animací `<Case>` element vstupu do play. Hodnota atributu jeho SelectScript je vyhodnocen; Vrácená hodnota musí být číselná. V závislosti na toto číslo, jeden z subanimations v rámci &lt;případ&gt; provádí. Například pokud SelectScript vyhodnotí na 2, Control Toolkit se spustí třetí animace v rámci &lt;případ&gt; (počítání začíná 0).

Následující kód definuje tři subanimations: Změna velikosti šířka, výška změny velikosti a mizení. Kód jazyka JavaScript (`Math.floor(3 * Math.random())`) pak vybere číslo mezi 0 a 2, tak, aby jeden ze tří animace spustit:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Jednou z možných tři animací: získá širší panelu](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Jednou z možných tři animací: panelu získá širší ([kliknutím ji zobrazíte obrázek v plné velikosti](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animation-depending-on-a-condition-vb.md)
> [další](animating-in-response-to-user-interaction-vb.md)
