---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Zakázání akcí během animace (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Také podporuje akce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-vb"></a>Zakázání akcí během animace (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Podporuje také akce, jako jsou kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Podporuje také akce, jako jsou kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Animace se použijí k HTML tlačítko takto:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Upozorňujeme, že ovládací prvek jazyka HTML je místo ovládací prvek webu vzhledem k tomu, že jsme nechcete, aby tlačítko pro vytvoření zpětné volání; právě zahájí animace straně klienta pro nás.

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

V rámci `<Animations>` uzlu `<OnClick>` je element správné zpracování kliknutí myší. Však může být stisknuto během animace také. `<EnableAction>` Element můžou starat o který. Nastavení `Enabled="false"` zakáže tlačítko jako součást animace. Vzhledem k tomu, že se používá několik jednotlivých animace (zakázání tlačítko a skutečný animace), `<Parallel>` element je potřeba dohledání jeden animací společně do jednoho. Tady je kompletní kód pro `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Také je možné znovu zapnout. pro tlačítko po animace, pomocí následujícího elementu XML na konci tohoto seznamu:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Ale v této situaci by to byl nemá od tlačítko setmívá a není viditelný na konci animace.


[![Tlačítko vypnutá při spuštění animace](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Tlačítko vypnutá při spuštění animace ([Kliknutím zobrazit obrázek v plné velikosti](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animating-in-response-to-user-interaction-vb.md)
> [další](triggering-an-animation-in-another-control-vb.md)
