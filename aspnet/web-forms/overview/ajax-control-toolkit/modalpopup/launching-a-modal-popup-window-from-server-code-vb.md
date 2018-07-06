---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Spuštění okna Modální místní nabídky serverovým kódem (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují tento t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 52eec262a9241aa7b11cbb892bdf2142c7a2b83a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822765"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Spuštění okna Modální místní nabídky serverovým kódem (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují, že otevírání Modální místní nabídky se aktivuje na straně serveru.


## <a name="overview"></a>Přehled

Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují, že otevírání Modální místní nabídky se aktivuje na straně serveru.

## <a name="steps"></a>Kroky

Webové ovládací prvek technologie ASP.NET je nejprve potřeba ukazují, jak funguje řízení ovládacího prvku ModalPopup. Přidejte tlačítko, v rámci &lt;formuláře&gt; prvek na nové stránce:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Potom budete potřebovat značky pro automaticky otevírané okno, které chcete vytvořit. Definovat jako `<asp:Panel>` řídit a ujistěte se, že obsahuje ovládací prvek tlačítko. Ovládací prvek ovládacího prvku ModalPopup a nabízí funkce, aby automaticky otevírané okno; toto tlačítko Zavřít jinak neexistuje žádný snadný způsob, jak ho zmizí.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Dále přidejte ovládací prvek ovládacího prvku ModalPopup z technologie ASP.NET AJAX Toolkit na stránku. Nastavit vlastnosti pro tlačítka, který načte ovládací prvek, tlačítko, které umožňuje zmizí a ID aplikace skutečný automaticky otevíraného okna.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Stejně jako u všech webových stránek, které jsou založené na technologii ASP.NET AJAX; Správce skriptů je nutná k načtení nezbytné knihovny jazyka JavaScript pro jiné cílové prohlížeče:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Spusťte v příkladu v prohlížeči. Po kliknutí na tlačítko se zobrazí Modální místní nabídky. Abyste dosáhli stejného účinku pomocí kódu na straně serveru, je potřeba nové tlačítko:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Jak je vidět, klikněte na tlačítko vytvoří zpětné volání a spustí `ServerButton_Click()` metoda na serveru. Tato metoda volá funkci jazyka JavaScript `launchModal()` provádí být přesné, funkce JavaScript, která se spustí po načtení stránky:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Práce `launchModal()` je zobrazení ovládacího prvku ModalPopup. `launchModal()` Funkce se provede po dokončení stránky HTML se načetl. V daném okamžiku však rozhraní ASP.NET AJAX framework ještě není zavedený plně. Proto `launchModal()` funkce pouze nastaví proměnnou ovládacího prvku ModalPopup ovládacího prvku musí být uvedeny později:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` Funkce JavaScript, která je speciální funkce, která se provede po technologie ASP.NET AJAX bylo plně načteno. Proto jsme přidejte kód pro tuto funkci chcete-li zobrazit ovládací prvek ovládacího prvku ModalPopup, ale pouze v případě `launchModal()` byla volána před:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()` Funkce hledá pojmenovaného elementu na stránce a očekává jako parametr ID na straně serveru. Proto `$find("mpe")` vrátí reprezentaci klienta ovládacího prvku ModalPopup ovládacího prvku; jeho `show()` metoda umožňuje automaticky otevírané okno se zobrazí.


[![Modální místní nabídky se zobrazí po kliknutí na některý z tlačítek](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Modální místní nabídky se zobrazí po kliknutí na některý z tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](positioning-a-modalpopup-cs.md)
> [další](using-modalpopup-with-a-repeater-control-vb.md)
