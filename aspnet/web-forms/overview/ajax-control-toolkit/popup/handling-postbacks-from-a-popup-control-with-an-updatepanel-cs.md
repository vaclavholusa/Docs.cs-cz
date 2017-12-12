---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: "Zpracování postback z ovládacího prvku místní okno s UpdatePanel (C#) | Microsoft Docs"
author: wenz
description: "Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Věnovat zvláštní pozornost tomu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 2abcf381aa95ad10d2f36e72d1be64c70fb7d9e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Zpracování postback z ovládacího prvku místní okno s UpdatePanel (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Zvláštní pozornost tomu mají být provedeny, když zpětné volání vyskytuje v takové místní.


## <a name="overview"></a>Přehled

Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Zvláštní pozornost tomu mají být provedeny, když zpětné volání vyskytuje v takové místní.

## <a name="steps"></a>Kroky

Při použití `PopupControl` s zpětné volání, `UpdatePanel` může zabránit aktualizaci stránky způsobené zpětné volání. Následující kód definuje několik důležité elementy:

- A `ScriptManager` řídit tak, aby funguje Toolkitu ASP.NET AJAX
- Dva `TextBox` ovládací prvky, které aktivují i místní okno
- A `Panel` ovládací prvek, který bude sloužit jako automaticky otevřeném okně.
- V panelu `Calendar` vložené ovládací prvek v rámci `UpdatePanel` ovládací prvek
- Dva `PopupControlExtender` ovládacích prvků, které přiřadit do textových polí panelu

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Všimněte si, že `OnSelectionChanged` atribut `Calendar` řízení je nastavené. Takže když uživatel vybere datum v kalendáři, dojde k zpětné volání a metody na straně serveru `c1_SelectionChanged()` se spustí. V rámci této metody musí být aktuální datum načíst a zapisovat zpátky do textového pole.

Syntaxe pro který je následující: objekt nejprve všechny proxy pro `PopupControlExtender` musí být na stránce vygeneroval. Nabízí sadu ovládacího prvku ASP.NET AJAX `GetProxyForCurrentPopup()` metoda. Objekt, vrátí tato metoda podporuje `Commit()` metodu, která odešle hodnotu zpět do ovládacího prvku, který aktivoval místním (ne ovládací prvek, který spouští volání metoda!). Následující kód poskytuje vybraným datem jako argument pro `Commit()` metoda, způsobí kód ke zpětnému zápisu vybraným datem do textového pole:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Nyní vždy, když kliknete na kalendářní datum vybraným datem se zobrazí v přidružené textového pole vytvoření ovládací prvek pro výběr data, která aktuálně naleznete na mnoha webů.


[![Kalendář se zobrazí, když uživatel klikne do textového pole](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Kalendář se zobrazí, když uživatel klikne do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Kliknutím na datum vloží ho do textového pole](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Kliknutím na datum vloží ho do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[Předchozí](using-multiple-popup-controls-cs.md)
[další](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
