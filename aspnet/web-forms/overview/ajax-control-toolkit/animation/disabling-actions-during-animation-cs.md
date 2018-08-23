---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Zakázání akcí během animace (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Podporuje také akce...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754988"
---
<a name="disabling-actions-during-animation-c"></a>Zakázání akcí během animace (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Podporuje také akce, jako je kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Podporuje také akce, jako je kliknutí myší. Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Uplatní se animace k tlačítku HTML tímto způsobem:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Mějte na paměti, že ovládací prvek je použít místo webový ovládací prvek, protože jsme nechcete, aby na tlačítko Vytvořit zpětné volání; právě zahájí animací na straně klienta pro nás.

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

V rámci `<Animations>` uzlu `<OnClick>` je správný prvek a zpracování kliknutí myší. Však může být stisknuto během animace, stejně. `<EnableAction>` Element zařídit, který. Nastavení `Enabled="false"` zakáže tlačítko jako součást animace. Protože používáme několika jednotlivých animací (zakázání tlačítka a skutečné animací), `<Parallel>` je vyžadován prvek připevněte jedné animace dohromady do jedné. Tady je kompletní kód pro `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Také je možné znovu povolit tlačítko po animaci pomocí následujícího elementu XML na konci seznamu:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Ale v daném scénáři to může být zbytečné od tlačítko setmívá a není viditelný na konci animace.


[![Poté, co běží animaci je tlačítko neaktivní.](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Je tlačítko neaktivní co nejdříve po spuštění animace ([kliknutím ji zobrazíte obrázek v plné velikosti](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](animating-in-response-to-user-interaction-cs.md)
> [další](triggering-an-animation-in-another-control-cs.md)
