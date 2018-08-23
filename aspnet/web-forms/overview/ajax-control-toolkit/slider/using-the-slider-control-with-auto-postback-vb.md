---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Použití ovládacího prvku posuvník s funkcí Auto-Postback (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné provádět automaticky zaúčtovat posuvníku...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 403e8fa6e54d84eb091769cbdb6ef1003ad4245c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756649"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Použití ovládacího prvku posuvník s funkcí Auto-Postback (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné měnit autopostback posuvník po jeho hodnotu.


## <a name="overview"></a>Přehled

Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné měnit autopostback posuvník po jeho hodnotu.

## <a name="steps"></a>Kroky

Pokud chcete mít posuvník automaticky postback na změnu, potřebujete obou polí atribut `AutoPostBack="true"`: textové pole, které se stanou posuvník samotného a textové pole, která obsahuje pozice posuvníku. Tady je požadované značky, které:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` Ovládacího prvku z technologie ASP.NET AJAX Control Toolkit přiřadí funkce pro posuvník tato dvě textová pole:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Element další popisek se později použije k uživatel informován o zpětném odeslání:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Nakonec `ScriptManager` ovládací prvek technologie ASP.NET AJAX načte požadované jazyka JavaScript pro ovládací prvek Toolkit pracovat:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Nyní je posuvník účtování; zpět na straně serveru může tato událost zachycena a reagovali na ni:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Posunutím jezdce aktivuje zpětné volání](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Posunutím jezdce aktivuje zpětné volání ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Potom data této změny je napsána v popisku](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Potom data této změny je napsána v popisku ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](databinding-the-slider-control-cs.md)
> [další](databinding-the-slider-control-vb.md)
