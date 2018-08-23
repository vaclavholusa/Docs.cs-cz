---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Zpracování postbacků extenderu Popupcontrol ovládacím prvkem UpdatePanel (C#) | Dokumentace Microsoftu
author: wenz
description: Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Zvláštní pozornost je třeba vzít...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f886ba0f3e79bc6d5daf193eaedfd627bc9b937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756656"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Zpracování postbacků extenderu Popupcontrol ovládacím prvkem UpdatePanel (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Zvláštní pozornost musí být provedeny, když dojde k takové rozbalené postbacku.


## <a name="overview"></a>Přehled

Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Zvláštní pozornost musí být provedeny, když dojde k takové rozbalené postbacku.

## <a name="steps"></a>Kroky

Při použití `PopupControl` s zpětné volání, `UpdatePanel` může zabránit aktualizace stránky způsobené zpětné volání. Následující kód definuje několik důležité elementy:

- A `ScriptManager` tak, aby funguje technologie ASP.NET AJAX Control Toolkit
- Dvě `TextBox` ovládací prvky, které aktivují obě automaticky otevíraného okna
- A `Panel` ovládací prvek, který bude sloužit jako automaticky otevíraného okna
- V rámci panelu `Calendar` ovládací prvek se vloží v rámci `UpdatePanel` ovládacího prvku
- Dvě `PopupControlExtender` ovládací prvky, které přiřadit do textových polí panelu

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Všimněte si, že `OnSelectionChanged` atribut `Calendar` nastavit ovládací prvek. Takže když uživatel vybere datum v kalendáři, dojde k zpětné volání a metody na straně serveru `c1_SelectionChanged()` provádí. V rámci této metody musí načíst aktuální datum a zapíše zpět do textového pole.

Syntaxe, která je následujícím způsobem: za prvé, proxy pro objekt `PopupControlExtender` na stránce je nutné vygenerovat. ASP.NET AJAX Control Toolkit nabízí `GetProxyForCurrentPopup()` metody. Objekt, vrátí tato metoda podporuje `Commit()` metodu, která odešle hodnotu zpět na ovládací prvek, který aktivuje místní nabídka (ne ovládací prvek, která aktivuje volání metody!). Následující kód obsahuje vybrané datum jako argument pro `Commit()` metody způsobí kódu ke zpětnému zápisu vybraného data do textového pole:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Nyní pokaždé, když kliknete na datum kalendáře přidružené textového pole, se zobrazí vybrané datum vytvoření ovládacího prvku pro výběr data, která aktuálně najdete na počet webů.


[![Když uživatel klikne do textového pole, zobrazí se v kalendáři](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Když uživatel klikne do textového pole, zobrazí se v kalendáři ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Kliknutím na datum umístí ho do textového pole](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Kliknutím na datum umístí ho do textového pole ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Předchozí](using-multiple-popup-controls-cs.md)
> [další](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
