---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Použití ovládacího prvku TextBoxWatermark s validačních ovládacích prvků (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládacího prvku TextBoxWatermark v sadou nástrojů AJAX Control Toolkit rozšiřuje textové pole tak, aby text se zobrazí v poli. Když uživatel klikne do pole, to jsem...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 144e4ee8f8096076b4e498844429ebf96e6ea402
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836186"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Použití ovládacího prvku TextBoxWatermark s validačních ovládacích prvků (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Ovládacího prvku TextBoxWatermark v sadou nástrojů AJAX Control Toolkit rozšiřuje textové pole tak, aby text se zobrazí v poli. Když uživatel klikne do pole, je prázdný. Pokud uživatel ponechá pole bez nutnosti zadávat text, zobrazí se předem vyplněných text. To může kolidovat s ASP.NET validačních ovládacích prvků na stejné stránce, ale může tyto problémy překonat.


## <a name="overview"></a>Přehled

`TextBoxWatermark` Ovládacího prvku AJAX Control Toolkit rozšiřuje textové pole tak, aby text se zobrazí v poli. Když uživatel klikne do pole, je prázdný. Pokud uživatel ponechá pole bez nutnosti zadávat text, zobrazí se předem vyplněných text. To může kolidovat s ASP.NET validačních ovládacích prvků na stejné stránce, ale může tyto problémy překonat.

## <a name="steps"></a>Kroky

Základní nastavení vzorku je následující: `TextBox` ovládací prvek je horní mezí, použití `TextBoxWatermarkExtender` ovládacího prvku. Tlačítko aktivuje zpětného odeslání a bude možné později použít k aktivaci validačních ovládacích prvků na stránce. Navíc `ScriptManager` ovládací prvek je potřebné k inicializaci technologie ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Teď přidejte `RequiredFieldValidator` ovládací prvek, který kontroluje, zda je text v poli při odeslání formuláře. `InitialValue` Validátoru musí být nastavena na stejnou hodnotu, která se používá v `TextBoxWatermarkExtender` ovládacího prvku: když se odešle formulář, beze změny textové pole hodnotu hodnoty meze v rámci něj:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Nicméně je jeden problém s tímto přístupem: Jestliže klient zakáže jazyka JavaScript, pole textu není proto předem s vodoznakového textu `RequiredFieldValidator` neaktivuje chybovou zprávu. Proto sekundy `RequiredFieldValidator` ovládací prvek je vyžadován, který vyhledává prázdné textové pole (vynechání `InitialValue` atributu).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Jelikož obě validátory používat `Display` = `"Dynamic"`, koncovému uživateli nelze rozlišit z vizuálního vzhledu, která dvě validátorů se vyvolala; místo toho to vypadá, došlo jenom jeden z nich.

Nakonec přidejte nějaký kód na straně serveru do výstupního text v poli, pokud nebyl nalezen žádný validátor vydané chybová zpráva:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Program pro ověření, že neexistuje žádný text v poli si bude stěžovat.](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Validátor si bude stěžovat na, že neexistuje žádný text v poli ([kliknutím ji zobrazíte obrázek v plné velikosti](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-textboxwatermark-in-a-formview-cs.md)
> [další](using-textboxwatermark-in-a-formview-vb.md)
