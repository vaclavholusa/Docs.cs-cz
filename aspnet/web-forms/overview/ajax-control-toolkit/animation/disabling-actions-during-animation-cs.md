---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: "Zakázání akcí během animace (C#) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Také podporuje akce..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 875c6be5e190c31fac030fc17ef040a934233f16
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-c"></a>Zakázání akcí během animace (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Podporuje také akce, jako jsou kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Podporuje také akce, jako jsou kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Animace se použijí k HTML tlačítko takto:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Upozorňujeme, že ovládací prvek jazyka HTML je místo ovládací prvek webu vzhledem k tomu, že jsme nechcete, aby tlačítko pro vytvoření zpětné volání; právě zahájí animace straně klienta pro nás.

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

V rámci `<Animations>` uzlu `<OnClick>` je element správné zpracování kliknutí myší. Však může být stisknuto během animace také. `<EnableAction>` Element můžou starat o který. Nastavení `Enabled="false"` zakáže tlačítko jako součást animace. Vzhledem k tomu, že se používá několik jednotlivých animace (zakázání tlačítko a skutečný animace), `<Parallel>` element je potřeba dohledání jeden animací společně do jednoho. Tady je kompletní kód pro `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Také je možné znovu zapnout. pro tlačítko po animace, pomocí následujícího elementu XML na konci tohoto seznamu:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Ale v této situaci by to byl nemá od tlačítko setmívá a není viditelný na konci animace.


[![Tlačítko vypnutá při spuštění animace](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Tlačítko vypnutá při spuštění animace ([Kliknutím zobrazit obrázek v plné velikosti](disabling-actions-during-animation-cs/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](animating-in-response-to-user-interaction-cs.md)
[další](triggering-an-animation-in-another-control-cs.md)
