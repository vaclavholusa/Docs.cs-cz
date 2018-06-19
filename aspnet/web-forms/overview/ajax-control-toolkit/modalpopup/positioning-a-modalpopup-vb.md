---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Umístění ModalPopup (VB) | Microsoft Docs
author: wenz
description: ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale ovládací prvek nenabízí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874613"
---
<a name="positioning-a-modalpopup-vb"></a>Umístění ModalPopup (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.


## <a name="overview"></a>Přehled

ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.

## <a name="steps"></a>Kroky

Chcete-li aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager`. ovládací prvek musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Dál přidejte panel, který slouží jako modální místní. Tlačítko slouží k zavření automaticky otevřeném okně:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Vždy, když se zobrazí automaticky otevřeném okně, musí být umístěna na určité místo na stránce. Pro tuto úlohu je vytvořen funkce jazyka JavaScript na straně klienta. Nejprve se pokusí získat přístup k panelu. Pokud se aktivace podaří, pozice panelu je nastavena pomocí šablon stylů CSS a JavaScript (změny, které bude pozici v místní nabídce). Ale `ModalPopupExtender` ovládací prvek také pokusí umístit automaticky otevřeném okně. Kód jazyka JavaScript proto umisťuje opakovaně místní, každou desetinu sekundy.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Jak můžete vidět, vrátí hodnotu, která `setTimeout()` metodu JavaScript je uložen v globální proměnné. To umožňuje zastavit opakovaných umístění automaticky otevřeném okně na vyžádání, pomocí `clearTimeout()` metoda:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Nyní již zbývá udělat je ověřte prohlížeč volat tyto funkce, kdykoli je to vhodné. `movePanel()` Funkce JavaScript, která musí být volána, když po kliknutí na tlačítko, která spustí panelu:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

A `stopMoving()` funkce stává play při zavření automaticky otevřeném okně. to může být aktivována v `ModalPopupExtender` ovládacího prvku:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Modální automaticky otevřeném okně se zobrazí na určené pozici](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Modální automaticky otevřeném okně se zobrazí na určené pozici ([Kliknutím zobrazit obrázek v plné velikosti](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](handling-postbacks-from-a-modalpopup-vb.md)
