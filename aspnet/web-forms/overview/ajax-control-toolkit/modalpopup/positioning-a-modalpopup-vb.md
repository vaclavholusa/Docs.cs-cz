---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Umístění ovládacího prvku ModalPopup (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Ale ovládací prvek nenabízí...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: e36d22b5c2f503ed849f373153e263bdd9546452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821758"
---
<a name="positioning-a-modalpopup-vb"></a>Umístění ovládacího prvku ModalPopup (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Ovládací prvek ale nenabízí vestavěnou funkci na pozici automaticky otevíraného okna.


## <a name="overview"></a>Přehled

Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Ovládací prvek ale nenabízí vestavěnou funkci na pozici automaticky otevíraného okna.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager`. ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

V dalším kroku přidáte panel, který slouží jako modální místní nabídky. Tlačítko slouží k zavření automaticky otevírané okno:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Pokaždé, když se zobrazí automaticky otevírané okno, musí být umístěn na určité místo na stránce. Pro tuto úlohu se vytvoří funkce jazyka JavaScript na straně klienta. Nejprve se pokusí přistupovat k panelu. Pokud se aktivace podaří, pozici panelu se nastavuje pomocí šablon stylů CSS a JavaScriptu (změnit pozici automaticky otevíraného okna na budou). Ale `ModalPopupExtender` ovládací prvek se rovněž snaží pozici automaticky otevíraného okna. Proto kód jazyka JavaScript opakovaně umístí automaticky otevírané okno, každou desetinu sekundy.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Jak je vidět, návratová hodnota `setTimeout()` metodu JavaScript se uloží do globální proměnné. Zastavit opakované umístění automaticky otevíraného okna na vyžádání, díky tomu pomocí `clearTimeout()` metody:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Nyní vše, co už zbývá jen se v prohlížeči volání těchto funkcí, kdykoli je to vhodné. `movePanel()` Funkce JavaScript, která musí být volána, když po kliknutí na tlačítko, která aktivuje panelu:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

A `stopMoving()` funkce vstupu do play při zavření to může automaticky otevírané okno se aktivuje v `ModalPopupExtender` ovládacího prvku:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Modální místní nabídky se zobrazí na určené pozici](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Modální místní nabídky se zobrazí na určené pozici ([kliknutím ji zobrazíte obrázek v plné velikosti](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](handling-postbacks-from-a-modalpopup-vb.md)
