---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Přetáhnout myší reorderlist (VB) | Dokumentace Microsoftu
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 78cdb53ce6cbf32ccdf913f30439734f94922e8f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814207"
---
<a name="drag-and-drop-via-reorderlist-vb"></a>Přetáhnout myší reorderlist (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Ovládacím prvkem ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí přetažení. Aktuální pořadí položek v seznamu se na serveru zachován.


## <a name="overview"></a>Přehled

`ReorderList` Ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí přetažení. Aktuální pořadí položek v seznamu se na serveru zachován.

## <a name="steps"></a>Kroky

`ReorderList` Ovládací prvek podporuje vazby dat z databáze do seznamu. Nejlepší na tom také podporuje zápis změn pracovního element seznamu zpět do úložiště dat.

Tato ukázka používá jako úložiště dat Microsoft SQL Server 2005 Express Edition. Databáze je součástí instalace sady Visual Studio, včetně express edition volitelné (a free). Je také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení. Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi.

Nejjednodušší způsob, jak nastavení databáze je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Připojení k serveru, dvakrát klikněte na `Databases` a vytvořit novou databázi (klikněte pravým tlačítkem a zvolte `New Database`) volá `Tutorials`.

V této databázi, vytvořte novou tabulku s názvem `AJAX` s těmito čtyřmi sloupci:

- `id` (primární klíč, celé číslo, identitu, not NULL)
- `char` (char(1), NULL)
- `description` (varchar(50), NULL)
- `position` (int, NULL).


[![Rozložení tabulky AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Rozložení tabulky AJAX ([kliknutím ji zobrazíte obrázek v plné velikosti](drag-and-drop-via-reorderlist-vb/_static/image3.png))


V dalším kroku vyplnění tabulky pomocí několika hodnot. Všimněte si, že `position` sloupec obsahuje pořadí řazení elementů.


[![Počáteční data v tabulce AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Počáteční data v tabulce AJAX ([kliknutím ji zobrazíte obrázek v plné velikosti](drag-and-drop-via-reorderlist-vb/_static/image6.png))


Další krok se vyžaduje k vygenerování `SqlDataSource` řízení ke komunikaci s novou databázi a její tabulky. Zdroj dat musí podporovat `SELECT` a `UPDATE` příkazy jazyka SQL. Pokud později změníte pořadí prvků seznamu, `ReorderList` ovládací prvek automaticky odešle dvě hodnoty ke zdroji dat `Update` příkaz: novou pozici a ID elementu. Proto se zdroj dat, musí `<UpdateParameters>` části tyto dvě hodnoty:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList` Ovládací prvek je potřeba nastavit následující atributy:

- `AllowReorder`: Určuje, zda může být přeskupení položek seznamu
- `DataSourceID`: ID zdroje dat
- `DataKeyField`: Název sloupec primárního klíče ve zdroji dat.
- `SortOrderField`: Sloupci zdroje dat, která poskytuje pořadí řazení položek seznamu

V `<DragHandleTemplate>` a `<ItemTemplate>` oddíly, rozložení seznamu možné podrobně nastavit. Datová vazba je také možné pomocí `Eval()` způsob, jak je vidět tady:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Následující šablony stylů CSS stylu informace (odkazované v `<DragHandleTemplate>` část `ReorderList` ovládací prvek) zajišťuje, že když najede myší úchyt pro přetažení změní umístění ukazatele myši odpovídajícím způsobem:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

A konečně `ScriptManager` inicializuje ovládací prvek ASP.NET AJAX stránky:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Spuštění tohoto příkladu v prohlížeči a trochu Změna uspořádání položek seznamu. Potom načtěte tuto stránku a podívejte se na databázi. Upravený pozice pravděpodobně nebyla zachována a odrážejí hodnoty v `position` sloupec v databázi a že všechno bez jakékoli kódu pouze s použitím značek.


[![Data v databázi se změní podle nového pořadí položek seznamu](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Položku dat v databázi se změní podle nového seznamu pořadí ([kliknutím ji zobrazíte obrázek v plné velikosti](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Předchozí](using-postbacks-with-reorderlist-vb.md)
