---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Spuštění okna Modální místní nabídky serverovým kódem (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují tento t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a13915f5748f8ad9b79fb787cc5e0682faa19388
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374088"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Spuštění okna Modální místní nabídky serverovým kódem (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují, že otevírání Modální místní nabídky se aktivuje na straně serveru.


## <a name="overview"></a>Přehled

Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují, že otevírání Modální místní nabídky se aktivuje na straně serveru.

## <a name="steps"></a>Kroky

Webové ovládací prvek technologie ASP.NET je nejprve potřeba ukazují, jak funguje řízení ovládacího prvku ModalPopup. Přidejte tlačítko, v rámci &lt;formuláře&gt; prvek na nové stránce:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Potom budete potřebovat značky pro automaticky otevírané okno, které chcete vytvořit. Definovat jako `<asp:Panel>` řídit a ujistěte se, že obsahuje ovládací prvek tlačítko. Ovládací prvek ovládacího prvku ModalPopup a nabízí funkce, aby automaticky otevírané okno; toto tlačítko Zavřít jinak neexistuje žádný snadný způsob, jak ho zmizí.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

Dále přidejte ovládací prvek ovládacího prvku ModalPopup z technologie ASP.NET AJAX Toolkit na stránku. Nastavit vlastnosti pro tlačítka, který načte ovládací prvek, tlačítko, které umožňuje zmizí a ID aplikace skutečný automaticky otevíraného okna.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Stejně jako u všech webových stránek, které jsou založené na technologii ASP.NET AJAX; Správce skriptů je nutná k načtení nezbytné knihovny jazyka JavaScript pro jiné cílové prohlížeče:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Spusťte v příkladu v prohlížeči. Po kliknutí na tlačítko se zobrazí Modální místní nabídky. Abyste dosáhli stejného účinku pomocí kódu na straně serveru, je potřeba nové tlačítko:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Jak je vidět, klikněte na tlačítko vytvoří zpětné volání a spustí `ServerButton_Click()` metoda na serveru. Tato metoda volá funkci jazyka JavaScript `launchModal()` provádí být přesné, funkce JavaScript, která se spustí po načtení stránky:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

Práce `launchModal()` je zobrazení ovládacího prvku ModalPopup. `launchModal()` Funkce se provede po dokončení stránky HTML se načetl. V daném okamžiku však rozhraní ASP.NET AJAX framework ještě není zavedený plně. Proto `launchModal()` funkce pouze nastaví proměnnou ovládacího prvku ModalPopup ovládacího prvku musí být uvedeny později:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` Funkce JavaScript, která je speciální funkce, která se provede po technologie ASP.NET AJAX bylo plně načteno. Proto jsme přidejte kód pro tuto funkci chcete-li zobrazit ovládací prvek ovládacího prvku ModalPopup, ale pouze v případě `launchModal()` byla volána před:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()` Funkce hledá pojmenovaného elementu na stránce a očekává jako parametr ID na straně serveru. Proto `$find("mpe")` vrátí reprezentaci klienta ovládacího prvku ModalPopup ovládacího prvku; jeho `show()` metoda umožňuje automaticky otevírané okno se zobrazí.


[![Modální místní nabídky se zobrazí po kliknutí na některý z tlačítek](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Modální místní nabídky se zobrazí po kliknutí na některý z tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-modalpopup-with-a-repeater-control-cs.md)
