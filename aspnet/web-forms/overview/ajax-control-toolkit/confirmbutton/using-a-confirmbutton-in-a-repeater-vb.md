---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Použití extenderu ConfirmButton v Repeateru (VB) | Dokumentace Microsoftu
author: wenz
description: Zařízení extender ConfirmButton v sadou nástrojů AJAX Control Toolkit vytvoří Ano/žádnou místní nabídku, když uživatel klikne na tlačítko (včetně prvek LinkButton). Ano pouze tehdy, pokud je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: ce403e84766f586eca36ef6bc513d9fbf7bd1d40
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396416"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Použití extenderu ConfirmButton v Repeateru (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Zařízení extender ConfirmButton v sadou nástrojů AJAX Control Toolkit vytvoří Ano/žádnou místní nabídku, když uživatel klikne na tlačítko (včetně prvek LinkButton). Pouze Ano po kliknutí na tlačítka akce provádí, jinak zrušena. To je možné v repeateru.


## <a name="overview"></a>Přehled

Zařízení extender ConfirmButton v sadou nástrojů AJAX Control Toolkit vytvoří Ano/žádnou místní nabídku, když uživatel klikne na tlačítko (včetně prvek LinkButton). Pouze Ano po kliknutí na tlačítka akce provádí, jinak zrušena. To je možné v repeateru.

## <a name="steps"></a>Kroky

Za prvé zdroj dat je povinný. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalace sady Visual Studio (včetně express edition) a jsou také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databáze AdventureWorks je součástí sad SQL Server 2005 ukázky a Sample Databases (stáhnout na [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databázi je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojit `AdventureWorks.mdf` databázový soubor.

V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení. Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi.

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Potom zdroj dat je povinný. Z důvodu zjednodušení jsou načteny pouze prvních pět položky v tabulce dodavatelů AdventureWorks'. Všimněte si, že při použití průvodce Visual Studio k vytvoření zdroje dat, název tabulky (`Vendors`) není aktuálně správně s předponou `Purchasing`. Následující kód je ten správný:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Tento zdroj dat je pak možné v rámci repeateru. Obvyklým způsobem `DataBinder.Eval()` metoda načte data ze zdroje dat. `ConfirmButtonExtender` Ovládací prvek musí být umístěn pak v rámci `<ItemTemplate>` části opakovače tak, že se zobrazí pro každou položku ve zdroji dat.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat.](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat ([kliknutím ji zobrazíte obrázek v plné velikosti](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-a-confirmbutton-in-a-repeater-cs.md)
