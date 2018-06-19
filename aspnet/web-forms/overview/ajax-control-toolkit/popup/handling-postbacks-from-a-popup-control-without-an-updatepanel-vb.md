---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Zpracování postback z ovládacího prvku místní nabídka bez UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Zpětné volání při výskytu v su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871360"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Zpracování postback z ovládacího prvku místní nabídka bez UpdatePanel (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Pokud dojde k zpětné volání takové panelu a existují několik panelů na stránce je obtížné určit klepnutí na panelu.


## <a name="overview"></a>Přehled

Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Pokud dojde k zpětné volání takové panelu a existují několik panelů na stránce je obtížné určit klepnutí na panelu.

## <a name="steps"></a>Kroky

Při použití `PopupControl` s zpětné volání, ale bez nutnosti `UpdatePanel` na stránce Toolkitu nenabízí způsob, jak určit, který element klienta má spustí automaticky otevírané zase způsobil zpětné volání. Ale malé efektu nabízí řešení pro tento scénář.

První řadě je zde základní nastavení: dvě textová pole, které oba aktivovat stejnou místní kalendáře. Dva `PopupControlExtenders` společně nabídnou textová pole a místní.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

Základní myšlenkou je skryté pole formuláře v přidání &lt; `form` &gt; element, který obsahuje textové pole, které se spustí automaticky otevřeném okně:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Při načtení stránky kódu jazyka JavaScript přidá do obou polí obslužné rutiny události: vždy, když uživatel klikne na textové pole, jeho název je zapsán do skryté pole formuláře:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

V kódu na straně serveru musí přečíst hodnotu skrytého pole. Vzhledem k tomu, že jsou trivial k manipulaci s skrytého pole, seznamu povolených IP adres přístup k ověření skryté hodnoty je požadovaná. Jakmile byl identifikován správný textového pole, datum z kalendáře se zapisují do ní.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Kalendář se zobrazí, když uživatel klikne do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Kalendář se zobrazí, když uživatel klikne do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Kliknutím na datum vloží ho do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Kliknutím na datum vloží ho do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
