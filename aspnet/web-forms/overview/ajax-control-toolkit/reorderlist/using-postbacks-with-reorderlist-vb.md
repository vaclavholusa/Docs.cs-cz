---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: "Pomocí postback ReorderList (VB) | Microsoft Docs"
author: wenz
description: "ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, objednávky a..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a>Pomocí postback ReorderList (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.


## <a name="overview"></a>Přehled

`ReorderList` Ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.

## <a name="steps"></a>Kroky

Existuje několik možných datových zdrojů pro `ReorderList` ovládacího prvku. Jedna je používat `XmlDataSource` ovládacího prvku:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Chcete-li vytvořit vazbu tento XML tak, aby `ReorderList` musí být nastavena řízení a povolit postback, následující atributy:

- `DataSourceID`: ID zdroje dat
- `SortOrderField`: Vlastnost řadit podle
- `AllowReorder`: Zda chcete povolit uživatelům změnit pořadí prvky seznamu
- `PostBackOnReorder`: Zda dojde k automatickému seznamu je přeskupení zpětné volání

Tady je vhodné značky ovládacího prvku:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

V rámci `ReorderList` , konkrétní data ze zdroje dat může být vázán pomocí `Eval()` metoda:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

Na libovolné pozici na stránce štítek bude obsahovat informace při poslední změna došlo k chybě:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Tento popisek, naplní se text v kódu na straně serveru, zpracování zpětného volání:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Nakonec, aby bylo možné aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit na stránce:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Každý změna aktivuje zpětné volání](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Každý změna aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-postbacks-with-reorderlist-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](drag-and-drop-via-reorderlist-cs.md)
[další](drag-and-drop-via-reorderlist-vb.md)
