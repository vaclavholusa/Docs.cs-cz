---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: "Použití ConfirmButton v prvku Repeater (VB) | Microsoft Docs"
author: wenz
description: "Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně). Pouze v případě Ano je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 5da8491c157ad6f35c2c25803680f262a35ce6e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Použití ConfirmButton v prvku Repeater (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně). Pouze Ano po kliknutí na tlačítka provedení akce, jinak zrušena. To je možné v prvku repeater.


## <a name="overview"></a>Přehled

Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně). Pouze Ano po kliknutí na tlačítka provedení akce, jinak zrušena. To je možné v prvku repeater.

## <a name="steps"></a>Kroky

První řadě zdroj dat je vyžadován. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.

Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení. Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Potom zdroj dat je vyžadován. Z důvodu zjednodušení jsou načteny pouze prvních pět položky v tabulce Dodavatelé AdventureWorks'. Všimněte si, že při použití průvodce Visual Studio k vytvoření zdroje dat, název tabulky (`Vendors`) není aktuálně předponu správně `Purchasing`. Následující kód je tu správnou:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Tento zdroj dat lze potom použít v rámci prvku repeater. Obvyklým způsobem `DataBinder.Eval()` metoda načítá data z datového zdroje. `ConfirmButtonExtender` Ovládací prvek musí být umístěn pak v rámci `<ItemTemplate>` části opakovače, které se zobrazí se pro každou položku ve zdroji dat.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat.](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

U každé položky ze zdroje dat se zobrazí tlačítko Potvrdit ([Kliknutím zobrazit obrázek v plné velikosti](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](using-a-confirmbutton-in-a-repeater-cs.md)
