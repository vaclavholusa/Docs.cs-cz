---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Umístění ModalPopup (C#) | Microsoft Docs
author: wenz
description: ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale ovládací prvek nenabízí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874145"
---
<a name="positioning-a-modalpopup-c"></a>Umístění ModalPopup (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.


## <a name="overview"></a>Přehled

ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.

## <a name="steps"></a>Kroky

Chcete-li aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager`. ovládací prvek musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Dál přidejte panel, který slouží jako modální místní. Tlačítko slouží k zavření automaticky otevřeném okně:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Vždy, když se zobrazí automaticky otevřeném okně, musí být umístěna na určité místo na stránce. Pro tuto úlohu je vytvořen funkce jazyka JavaScript na straně klienta. Nejprve se pokusí získat přístup k panelu. Pokud se aktivace podaří, pozice panelu je nastavena pomocí šablon stylů CSS a JavaScript (změny, které bude pozici v místní nabídce). Ale `ModalPopupExtender` ovládací prvek také pokusí umístit automaticky otevřeném okně. Kód jazyka JavaScript proto umisťuje opakovaně místní, každou desetinu sekundy.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Jak můžete vidět, vrátí hodnotu, která `setTimeout()` metodu JavaScript je uložen v globální proměnné. To umožňuje zastavit opakovaných umístění automaticky otevřeném okně na vyžádání, pomocí `clearTimeout()` metoda:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Nyní již zbývá udělat je ověřte prohlížeč volat tyto funkce, kdykoli je to vhodné. `movePanel()` Funkce JavaScript, která musí být volána, když po kliknutí na tlačítko, která spustí panelu:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

A `stopMoving()` funkce stává play při zavření automaticky otevřeném okně. to může být aktivována v `ModalPopupExtender` ovládacího prvku:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![Modální automaticky otevřeném okně se zobrazí na určené pozici](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Modální automaticky otevřeném okně se zobrazí na určené pozici ([Kliknutím zobrazit obrázek v plné velikosti](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](handling-postbacks-from-a-modalpopup-cs.md)
> [další](launching-a-modal-popup-window-from-server-code-vb.md)
