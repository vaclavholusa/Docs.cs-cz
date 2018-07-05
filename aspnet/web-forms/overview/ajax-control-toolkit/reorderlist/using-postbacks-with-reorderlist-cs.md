---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Použití postbacků s ovládacím prvkem ReorderList (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládacím prvkem ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí přetažení. Pokaždé, když je pořadí v seznamu změníte, po...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 71e806b7915c010cec66931d87bd8c1f3b6d1fb3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365630"
---
<a name="using-postbacks-with-reorderlist-c"></a>Použití postbacků s ovládacím prvkem ReorderList (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Ovládacím prvkem ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí přetažení. Pokaždé, když je pořadí v seznamu změníte, zpětné volání informuje server změny.


## <a name="overview"></a>Přehled

`ReorderList` Ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí přetažení. Pokaždé, když je pořadí v seznamu změníte, zpětné volání informuje server změny.

## <a name="steps"></a>Kroky

Existuje několik datových zdrojů pro `ReorderList` ovládacího prvku. Jeden má používat `XmlDataSource` ovládacího prvku:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Vázat tento XML tak, aby `ReorderList` postbacků ovládacího prvku a povolit, následující atributy musí být nastavena:

- `DataSourceID`: ID zdroje dat
- `SortOrderField`Seřadit podle: vlastnost
- `AllowReorder`: Zda chcete povolit uživatelům změnit uspořádání seznamu elementů
- `PostBackOnReorder`: Jestli chcete vytvořit zpětné volání pokaždé, když je změnit jejich uspořádání seznamu

Tady je odpovídající značky ovládacího prvku:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

V rámci `ReorderList` ovládacího prvku, konkrétní data ze zdroje dat, může být vázaný pomocí `Eval()` metody:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

Na libovolné pozici na stránce popisek bude obsahovat informace, kdy poslední změny pořadí došlo k chybě:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Tento popisek je vyplněna text v kódu na straně serveru, zpracování zpětného volání:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Nakonec, aby bylo možné aktivovat funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit na stránce:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Každá změna uspořádání aktivuje zpětné volání](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Každá změna uspořádání aktivuje zpětné volání ([kliknutím ji zobrazíte obrázek v plné velikosti](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](drag-and-drop-via-reorderlist-cs.md)
