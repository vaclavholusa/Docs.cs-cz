---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Zpracování postbacků extenderu Popupcontrol bez ovládacího prvku UpdatePanel (VB) ovládacího | Dokumentace Microsoftu
author: wenz
description: Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Zpětné volání při výskytu v su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ef1d0300cba42eefb5f6cda77693aeafb93a31a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363841"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Zpracování postbacků extenderu Popupcontrol ovládacím bez ovládacího prvku UpdatePanel (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Když zpětného odeslání dojde k takové panelu a na stránce se několika panely je obtížné určit, se kliklo panelu.


## <a name="overview"></a>Přehled

Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Když zpětného odeslání dojde k takové panelu a na stránce se několika panely je obtížné určit, se kliklo panelu.

## <a name="steps"></a>Kroky

Při použití `PopupControl` s zpětné volání, ale bez nutnosti `UpdatePanel` na stránce Control Toolkit nenabízí způsob, jak určit, který klient prvek spustil automaticky otevírané okno, které pak způsobil zpětné volání. Ale triku poskytuje řešení pro tento scénář.

Za prvé, tady je základní nastavení: dvě textová pole, které obě aktivovat stejné místní nabídky kalendář. Dvě `PopupControlExtenders` textová pole a automaticky otevírané okno pohromadě.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

Základní myšlenka spočívá v přidání skrytého pole v &lt; `form` &gt; element, který obsahuje textové pole, které spustí automaticky otevírané okno:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Při načtení stránky kódu jazyka JavaScript přidá obslužnou rutinu události do obou polí: pokaždé, když uživatel klikne na textové pole, jeho název je zapsán do skryté pole formuláře:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

V kódu na straně serveru musí přečíst hodnotu skrytého pole. Protože jsou triviální k manipulaci s skrytých polí ve formuláři, vyžaduje se seznamu povolených IP adres přístup k ověření skryté hodnoty. Jakmile byl identifikován správný textového pole, se do něj zapíše datum z kalendáře.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Když uživatel klikne do textového pole, zobrazí se v kalendáři](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Když uživatel klikne do textového pole, zobrazí se v kalendáři ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Kliknutím na datum umístí ho do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Kliknutím na datum umístí ho do textového pole ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
