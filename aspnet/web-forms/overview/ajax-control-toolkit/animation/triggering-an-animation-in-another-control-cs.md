---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Spuštění animace v další ovládací prvek (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Obecně platí, spouštění...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-c"></a>Spuštění animace v další ovládací prvek (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek. Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek. Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Pokud chcete začít animace panelu, je HTML tlačítko použít. Všimněte si, že `<input type="button" />` je nejvyšších přes `<asp:Button />` vzhledem k tomu, že jsme nechcete, aby zpětné volání po kliknutí na toto tlačítko.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`. Je důležité nastavit `TargetControlID` na ID tlačítko (elementu spuštění animace), ne na ID panelu (elementu se animovaný)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

V rámci `<Animations>` uzlu, místní animací jako obvykle. Aby bylo možné je změnit panelu, ne na tlačítko nastavit `AnimationTarget` atribut pro každý element animace, v rámci `AnimationExtender`. Hodnota `AnimationTarget` samozřejmě je ID panelu. Tímto způsobem animací dojít s panelem, ne při spouštěcí tlačítko. Tady je `AnimationExtender` kód pro tento scénář:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Všimněte si, speciální pořadí, ve kterém se zobrazují jednotlivé animace. Tlačítko získá první řadě deaktivovat po spuštění animace. Vzhledem k tomu, že je žádné `AnimationTarget` atribut `<EnableAction>` elementu, tato animace se použije pro původní ovládacího prvku: tlačítko. Animace následující dva kroky se provádějí parallelly (`<Parallel>` element). Mají obě jejich `AnimationTarget` atributy nastavit na `"Panel1"`, proto animace panelu, není tlačítko.


[![Panel animace spuštěna, myši klikněte na tlačítko](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Panel animace spuštěna, myši klikněte na tlačítko ([Kliknutím zobrazit obrázek v plné velikosti](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](disabling-actions-during-animation-cs.md)
> [další](modifying-animations-from-the-server-side-cs.md)
