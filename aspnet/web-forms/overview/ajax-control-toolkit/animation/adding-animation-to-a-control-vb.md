---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Přidání animace k ovládacímu prvku (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Tento kurz ukazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3909422dc5d261b39f3efd7d7eaeb5cfb1976f2b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364923"
---
<a name="adding-animation-to-a-control-vb"></a>Přidání animace k ovládacímu prvku (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Tento kurz ukazuje, jak nastavit tyto animace.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Tento kurz ukazuje, jak nastavit tyto animace.

## <a name="steps"></a>Kroky

Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby je načtena knihovna ASP.NET AJAX a Control Toolkit je možné:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Animace v tomto scénáři se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

Přidružené třídy šablony stylů CSS pro panel definuje barvu pozadí a šířku:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Dále až, potřebujeme `AnimationExtender`. Po zadání `ID` a obvyklého `runat="server"`, `TargetControlID` atribut musí být nastaven na ovládací prvek pro animaci v našem případě panelu:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Celý animace použije deklarativně pomocí syntaxe jazyka XML, bohužel momentálně nejsou plně podporovány v sadě Visual Studio IntelliSense. Kořenový uzel je `<Animations>;` v rámci tohoto uzlu, jsou povoleny několik událostí, které určují, kdy animace přezkumném místě:

- `OnClick` (kliknutí myší)
- `OnHoverOut` (když ukazatel myši opustí ovládací prvek)
- `OnHoverOver` (po umístění ukazatele myši nad ovládací prvek, zastavuje `OnHoverOut` animace)
- `OnLoad` (Pokud na stránce se načetl)
- `OnMouseOut` (když ukazatel myši opustí ovládací prvek)
- `OnMouseOver` (po umístění ukazatele myši nad ovládací prvek, ne zastavení `OnMouseOut` animace)

Rozhraní framework obsahuje sadu animace, každý z nich představované vlastní – element XML. Tady je výběr:

- `<Color>` (a změníte barvu)
- `<FadeIn>` (pozvolného)
- `<FadeOut>` (mizení)
- `<Property>` (při změně hodnoty vlastnosti ovládacího prvku)
- `<Pulse>` (pulsating)
- `<Resize>` (Změna velikosti)
- `<Scale>` (proporcionálně změnou velikosti)

V tomto příkladu se zesvětlit panelu. Animace přijmou půl sekundy (`Duration` atribut), zobrazení 24 snímků (kroky animace) za sekundu (`Fps` atributy). Tady je kompletní kód pro `AnimationExtender` ovládacího prvku:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Při spuštění tohoto skriptu na panelu se zobrazí a setmívá v jedné a půl sekundy.


[![Je mizení panelu](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Na panelu je mizení ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-controlling-updatepanel-animations-cs.md)
> [další](executing-several-animations-at-the-same-time-vb.md)
