---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Přidání animace do ovládacího prvku (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Tento kurz ukazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-c"></a>Přidání animace do ovládacího prvku (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Tento kurz ukazuje, jak nastavit takové animace.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Tento kurz ukazuje, jak nastavit takové animace.

## <a name="steps"></a>Kroky

Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Animace v tomto scénáři se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definuje barvu pozadí a šířku:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Další až, potřebujeme `AnimationExtender`. Po zadání `ID` a obvykle `runat="server"`, `TargetControlID` musí být nastaven na ovládací prvek pro animaci v našem případě panelu:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Použití celou animace deklarativně, pomocí syntaxe XML bohužel momentálně není plně nepodporuje technologii IntelliSense v sadě Visual Studio. Kořenový uzel je `<Animations>;` v rámci tohoto uzlu, jsou povoleny několik událostí, které určují, kdy animation(s) přezkumném místní:

- `OnClick` (klikněte na tlačítko myši)
- `OnHoverOut` (Pokud myši ponechá ovládací prvek)
- `OnHoverOver` (pokud ukazatel myši nachází ovládacího prvku, zastavování `OnHoverOut` animace)
- `OnLoad` (Pokud stránky byl načten)
- `OnMouseOut` (Pokud myši ponechá ovládací prvek)
- `OnMouseOver` (pokud ukazatel myši nachází ovládacího prvku, není zastavení `OnMouseOut` animace)

Rozhraní framework se dodává se sadou animací, každé z nich reprezentována vlastní – element XML. Tady je výběr:

- `<Color>` (a změníte barvu)
- `<FadeIn>` (pozvolného vysouvání)
- `<FadeOut>` (pozvolného vysouvání)
- `<Property>` (Změna ovládacího prvku)
- `<Pulse>` (pulsating)
- `<Resize>` (změně velikosti)
- `<Scale>` (úměrně změně velikosti)

V tomto příkladu se objevovat panelu. Animace přijmou 1,5 sekund (`Duration` atribut), zobrazení 24 snímků (postup animace) za sekundu (`Fps` atributy). Tady je značku dokončení `AnimationExtender` ovládacího prvku:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Při spuštění tohoto skriptu panelu se zobrazí a setmívá v jedné a půl sekundy.


[![Na panelu je pozvolného vysouvání](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Na panelu je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](executing-several-animations-at-the-same-time-cs.md)
