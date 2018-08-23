---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Použití extenderu ConfirmButton v Repeateru (C#) | Dokumentace Microsoftu
author: wenz
description: Zařízení extender ConfirmButton v sadou nástrojů AJAX Control Toolkit vytvoří Ano/žádnou místní nabídku, když uživatel klikne na tlačítko (včetně prvek LinkButton). Ano pouze tehdy, pokud je...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 614b5b1edaa164cca30b2142d1e0c02771153403
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755027"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>Použití extenderu ConfirmButton v Repeateru (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> Zařízení extender ConfirmButton v sadou nástrojů AJAX Control Toolkit vytvoří Ano/žádnou místní nabídku, když uživatel klikne na tlačítko (včetně prvek LinkButton). Pouze Ano po kliknutí na tlačítka akce provádí, jinak zrušena. To je možné v repeateru.


## <a name="overview"></a>Přehled

Zařízení extender ConfirmButton v sadou nástrojů AJAX Control Toolkit vytvoří Ano/žádnou místní nabídku, když uživatel klikne na tlačítko (včetně prvek LinkButton). Pouze Ano po kliknutí na tlačítka akce provádí, jinak zrušena. To je možné v repeateru.

## <a name="steps"></a>Kroky

Za prvé zdroj dat je povinný. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalace sady Visual Studio (včetně express edition) a jsou také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databáze AdventureWorks je součástí sad SQL Server 2005 ukázky a Sample Databases (stáhnout na [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databázi je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojit `AdventureWorks.mdf` databázový soubor.

V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení. Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi.

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Potom zdroj dat je povinný. Z důvodu zjednodušení jsou načteny pouze prvních pět položky v tabulce dodavatelů AdventureWorks'. Všimněte si, že při použití průvodce Visual Studio k vytvoření zdroje dat, název tabulky (`Vendors`) není aktuálně správně s předponou `Purchasing`. Následující kód je ten správný:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Tento zdroj dat je pak možné v rámci repeateru. Obvyklým způsobem `DataBinder.Eval()` metoda načte data ze zdroje dat. `ConfirmButtonExtender` Ovládací prvek musí být umístěn pak v rámci `<ItemTemplate>` části opakovače tak, že se zobrazí pro každou položku ve zdroji dat.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat.](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat ([kliknutím ji zobrazíte obrázek v plné velikosti](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-a-confirmbutton-in-a-repeater-vb.md)
