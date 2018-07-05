---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Datová vazba ovládacího prvku jezdec (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné vytvořit vazbu aktuální pozice...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cbb53309ccde9ed6be67a977a56cf2942bbe7f8c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400652"
---
<a name="databinding-the-slider-control-c"></a>Datová vazba ovládacího prvku jezdec (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné vytvořit vazbu aktuální pozice posuvníku na jiný ovládací prvek technologie ASP.NET.


## <a name="overview"></a>Přehled

Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné vytvořit vazbu aktuální pozice posuvníku na jiný ovládací prvek technologie ASP.NET.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

V dalším kroku přidejte dva `TextBox` ovládacích prvků na stránce. Jedna se transformuje na grafické posuvník a druhá bude obsahovat pozice posuvníku.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Dalším krokem je již v posledním kroku. `SliderExtender` Ovládacího prvku z technologie ASP.NET AJAX Control Toolkit umožňuje posuvníku z prvního textového pole a automaticky aktualizuje do druhého textového pole, když změny pozice posuvníku. Který chcete, aby `SliderExtender`společnosti `TargetControlID` atribut musí být nastaven na ID prvního textového pole; `BoundControlID` atribut musí být nastaven na ID do druhého textového pole.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Jak je vidět v prohlížeči, datové vazby funguje v obou směrech: zadáte novou hodnotu v textovém poli aktualizuje pozice posuvníku. Pokud provedete do druhého textového pole jen pro čtení, můžete přidat slabé ochranu do textového pole tak, aby se pro uživatele k ruční aktualizaci hodnoty tady složitější.


[![Posuvníku a textového pole, které jsou synchronizované](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Posuvníku a textového pole, které jsou synchronizované ([kliknutím ji zobrazíte obrázek v plné velikosti](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-the-slider-control-with-auto-postback-cs.md)
> [další](using-the-slider-control-with-auto-postback-vb.md)
