---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Zakázání akcí během animace (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Podporuje také akce...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 811e1d75f79885f3f4c561d9211fec625fcf1807
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754308"
---
<a name="disabling-actions-during-animation-vb"></a>Zakázání akcí během animace (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Podporuje také akce, jako je kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Podporuje také akce, jako je kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Uplatní se animace k tlačítku HTML tímto způsobem:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Mějte na paměti, že ovládací prvek je použít místo webový ovládací prvek, protože jsme nechcete, aby na tlačítko Vytvořit zpětné volání; právě zahájí animací na straně klienta pro nás.

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

V rámci `<Animations>` uzlu `<OnClick>` je správný prvek a zpracování kliknutí myší. Však může být stisknuto během animace, stejně. `<EnableAction>` Element zařídit, který. Nastavení `Enabled="false"` zakáže tlačítko jako součást animace. Protože používáme několika jednotlivých animací (zakázání tlačítka a skutečné animací), `<Parallel>` je vyžadován prvek připevněte jedné animace dohromady do jedné. Tady je kompletní kód pro `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Také je možné znovu povolit tlačítko po animaci pomocí následujícího elementu XML na konci seznamu:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Ale v daném scénáři to může být zbytečné od tlačítko setmívá a není viditelný na konci animace.


[![Poté, co běží animaci je tlačítko neaktivní.](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Je tlačítko neaktivní co nejdříve po spuštění animace ([kliknutím ji zobrazíte obrázek v plné velikosti](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animating-in-response-to-user-interaction-vb.md)
> [další](triggering-an-animation-in-another-control-vb.md)
