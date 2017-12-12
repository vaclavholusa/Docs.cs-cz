---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: "Datové vazby v ovládacím prvku posuvník (C#) | Microsoft Docs"
author: wenz
description: "Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné svázat aktuální pozice..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2aa770bce5969a4ab57893d5ceecc127cf7f7872
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="databinding-the-slider-control-c"></a>Datové vazby v ovládacím prvku posuvník (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné k vytvoření vazby aktuální pozici jezdec na další ovládací prvek ASP.NET.


## <a name="overview"></a>Přehled

Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné k vytvoření vazby aktuální pozici jezdec na další ovládací prvek ASP.NET.

## <a name="steps"></a>Kroky

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Dál přidejte dva `TextBox` ovládací prvky na stránku. Jeden se převede na grafické jezdce a dalších jedna bude obsahovat pozice posuvníku.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Dalším krokem je již v posledním kroku. `SliderExtender` Řízení z ovládacího prvku ASP.NET AJAX Toolkit díky jezdce mimo první textového pole a automaticky aktualizuje druhého textového pole, když posuvník umístit změny. V pořadí, pro který chcete pracovat `SliderExtender`na `TargetControlID` musí být nastaven na ID první textového pole; `BoundControlID` musí být nastaven na ID druhého textového pole.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Jak vidíte v prohlížeči, datové vazby funguje v obou směrech: zadáním nové hodnoty v textovém poli aktualizace pozici posuvníku. Pokud provedete druhé textové pole jen pro čtení, můžete přidat slabé ochrany pro textové pole tak, aby se pro uživatele ručně aktualizovat hodnotu zde složitější.


[![Posuvník a textové pole, které jsou synchronizované](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Posuvník a textové pole, které jsou synchronizované ([Kliknutím zobrazit obrázek v plné velikosti](databinding-the-slider-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](using-the-slider-control-with-auto-postback-cs.md)
[další](using-the-slider-control-with-auto-postback-vb.md)
