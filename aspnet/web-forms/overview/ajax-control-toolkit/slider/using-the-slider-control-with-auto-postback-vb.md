---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Použití ovládacího prvku posuvník s Auto-Postback (VB) | Microsoft Docs
author: wenz
description: Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné, aby automaticky zaúčtovat posuvníku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Použití ovládacího prvku posuvník s Auto-Postback (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.


## <a name="overview"></a>Přehled

Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.

## <a name="steps"></a>Kroky

Aby bylo možné posuvník automaticky zpětného volání při změně, třeba obou polí atribut `AutoPostBack="true"`: textové pole, které bude posuvník sám a textové pole, které obsahuje pozici posuvníku. Zde je kód požadované pro tento:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` Řízení z ovládacího prvku ASP.NET AJAX Toolkit přiřadí funkci posuvník do dvou textových polí:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Element label další se později použije informovat uživatele zpětné volání:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Nakonec `ScriptManager` ovládacího prvku ASP.NET AJAX načte požadované JavaScript pro Toolkitu postup:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Nyní je jezdec publikování zpět; Tato událost může na straně serveru, místo zachycení a reagovali na ni:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Posunutím jezdce aktivuje zpětné volání](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Posunutím jezdce aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Později datum této změny je napsána v popisku](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Později, datum této změny je napsána v popisku ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](databinding-the-slider-control-cs.md)
> [další](databinding-the-slider-control-vb.md)
