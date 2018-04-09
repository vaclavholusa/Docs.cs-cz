---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Pomocí postback ReorderList (C#) | Microsoft Docs
author: wenz
description: ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, objednávky a...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed01c30c0721c8f1cd8ccb3fea0735ea8fa4f0a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-c"></a>Pomocí postback ReorderList (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.


## <a name="overview"></a>Přehled

`ReorderList` Ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.

## <a name="steps"></a>Kroky

Existuje několik možných datových zdrojů pro `ReorderList` ovládacího prvku. Jedna je používat `XmlDataSource` ovládacího prvku:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Chcete-li vytvořit vazbu tento XML tak, aby `ReorderList` musí být nastavena řízení a povolit postback, následující atributy:

- `DataSourceID`: ID zdroje dat
- `SortOrderField`: Vlastnost řadit podle
- `AllowReorder`: Zda chcete povolit uživatelům změnit pořadí prvky seznamu
- `PostBackOnReorder`: Zda dojde k automatickému seznamu je přeskupení zpětné volání

Tady je vhodné značky ovládacího prvku:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

V rámci `ReorderList` , konkrétní data ze zdroje dat může být vázán pomocí `Eval()` metoda:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

Na libovolné pozici na stránce štítek bude obsahovat informace při poslední změna došlo k chybě:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Tento popisek, naplní se text v kódu na straně serveru, zpracování zpětného volání:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Nakonec, aby bylo možné aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit na stránce:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Každý změna aktivuje zpětné volání](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Každý změna aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](drag-and-drop-via-reorderlist-cs.md)
