---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animace v reakci na interakci uživatele (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze hvězdičkami...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b774a058e715e4a98e767daf92886f24e627822
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815522"
---
<a name="animating-in-response-to-user-interaction-vb"></a>Animace v reakci na interakci uživatele (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze spustit automaticky nebo může být aktivované interakci s uživatelem, například po kliknutí myší.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze spustit automaticky nebo může být aktivované interakci s uživatelem, například po kliknutí myší.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, jsou pět jeden ze způsobů spuštění animace prostřednictvím interakce uživatele (chybí element je `<OnLoad>` který je proveden, jakmile úplným načtením celé stránky):

- `<OnClick>` (kliknutí myší na ovládací prvek)
- `<OnHoverOut>` (ukazatel myši opustí ovládací prvek)
- `<OnHoverOver>` (ovládací prvek, zastavuje se ukazatel myši nachází `<OnHoverOut>` animace)
- `<OnMouseOut>` (ukazatel myši opustí ovládací prvek)
- `<OnMouseOver>` (myší na ovládací prvek není zastavení `<OnMouseOut>` animace)

V tomto scénáři `<OnClick>` se používá. Když uživatel klikne na panelu, je velikost a setmívá ve stejnou dobu.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![Animace bude spuštěna kliknutí myší](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Animace bude spuštěna kliknutí myší ([kliknutím ji zobrazíte obrázek v plné velikosti](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](picking-one-animation-out-of-a-list-vb.md)
> [další](disabling-actions-during-animation-vb.md)
