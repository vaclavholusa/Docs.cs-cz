---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "Zpracování postback z ovládacího prvku místní nabídka bez UpdatePanel (C#) | Microsoft Docs"
author: wenz
description: "Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Zpětné volání při výskytu v su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Zpracování postback z ovládacího prvku místní nabídka bez UpdatePanel (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Pokud dojde k zpětné volání takové panelu a existují několik panelů na stránce je obtížné určit klepnutí na panelu.


## <a name="overview"></a>Přehled

Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Pokud dojde k zpětné volání takové panelu a existují několik panelů na stránce je obtížné určit klepnutí na panelu.

## <a name="steps"></a>Kroky

Při použití `PopupControl` s zpětné volání, ale bez nutnosti `UpdatePanel` na stránce Toolkitu nenabízí způsob, jak určit, který element klienta má spustí automaticky otevírané zase způsobil zpětné volání. Ale malé efektu nabízí řešení pro tento scénář.

První řadě je zde základní nastavení: dvě textová pole, které oba aktivovat stejnou místní kalendáře. Dva `PopupControlExtenders` společně nabídnou textová pole a místní.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Základní myšlenkou je skryté pole formuláře v přidání &lt; `form` &gt; element, který obsahuje textové pole, které se spustí automaticky otevřeném okně:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Při načtení stránky kódu jazyka JavaScript přidá do obou polí obslužné rutiny události: vždy, když uživatel klikne na textové pole, jeho název je zapsán do skryté pole formuláře:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

V kódu na straně serveru musí přečíst hodnotu skrytého pole. Vzhledem k tomu, že jsou trivial k manipulaci s skrytého pole, seznamu povolených IP adres přístup k ověření skryté hodnoty je požadovaná. Jakmile byl identifikován správný textového pole, datum z kalendáře se zapisují do ní.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Kalendář se zobrazí, když uživatel klikne do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Kalendář se zobrazí, když uživatel klikne do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Kliknutím na datum vloží ho do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Kliknutím na datum vloží ho do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[Předchozí](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[další](using-multiple-popup-controls-vb.md)
