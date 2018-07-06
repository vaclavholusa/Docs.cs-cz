---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Aktivace animace jiného ovládacího prvku (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Obecně platí, spouští se...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a7eeb522ae982cd5a84aa9c8e4228871c8c78440
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810799"
---
<a name="triggering-an-animation-in-another-control-c"></a>Aktivace animace jiného ovládacího prvku (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek. Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek. Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Pokud chcete začít animace panelu, je HTML tlačítko použít. Všimněte si, že `<input type="button" />` nejvyšších přes `<asp:Button />` od tudy zpětné volání, když uživatel klikne na toto tlačítko.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`. Je důležité nastavit `TargetControlID` ID tlačítko (elementu animace aktivace), aby Identifikátor panelu (elementu animované)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

V rámci `<Animations>` uzlu, místo animace jako obvykle. Aby bylo možné je měnit panelu, není toto tlačítko, nastavte `AnimationTarget` atribut pro každý prvek animace v rámci `AnimationExtender`. Hodnota pro `AnimationTarget` je samozřejmě ID panelu. Tímto způsobem animací dojít s panelem, nikoli zpětným na spouštěcí tlačítko. Tady je `AnimationExtender` kód pro tento scénář:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Poznámka: zvláštní pořadí, ve kterém se zobrazují jednotlivé animace. Za prvé získá tlačítko Deaktivovat po spuštění animace. Protože neexistuje žádné `AnimationTarget` atribut v `<EnableAction>` element, tato animace se použije pro původní ovládacího prvku: tlačítko. Animace další dva kroky se provádějí parallelly (`<Parallel>` element). Obě mají jejich `AnimationTarget` nastavte atributy na `"Panel1"`, tedy animace panelu, ne na tlačítko.


[![Panel animace spuštěna, kliknutí myší na tlačítko](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Panel animace spuštěna, kliknutí myší na tlačítko ([kliknutím ji zobrazíte obrázek v plné velikosti](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](disabling-actions-during-animation-cs.md)
> [další](modifying-animations-from-the-server-side-cs.md)
