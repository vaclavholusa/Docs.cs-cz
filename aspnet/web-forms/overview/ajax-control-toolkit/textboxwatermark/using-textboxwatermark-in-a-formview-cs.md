---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: "Použití TextBoxWatermark v FormView (C#) | Microsoft Docs"
author: wenz
description: "TextBoxWatermark ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je možné..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 805ba26720fa7faed82101076ff84e576f4cfcf2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-in-a-formview-c"></a>Použití TextBoxWatermark v FormView (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> TextBoxWatermark ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je vyprázdnit. Pokud uživatel ponechá bez nutnosti zadávat text do pole, zobrazí se předem vyplněných text znovu. To je možné v rámci ovládacího prvku FormView.


## <a name="overview"></a>Přehled

`TextBoxWatermark` Ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je vyprázdnit. Pokud uživatel ponechá bez nutnosti zadávat text do pole, zobrazí se předem vyplněných text znovu. To je také možné v rámci `FormView` ovládacího prvku.

## <a name="steps"></a>Kroky

První řadě zdroj dat je vyžadován. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.

Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení. Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Pak přidejte zdroje dat na stránku, která podporuje `DELETE`, `INSERT` a `UPDATE` příkazů SQL. Pokud používáte pomocníkem pro Visual Studio k vytvoření zdroje dat, paměti, že chyby v aktuální verzi není předpony názvu tabulky (`Vendor`) s `Purchasing`. Následující kód ukazuje správnou syntaxi:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Pamatovat název (`ID`) zdroje dat, protože se používá v `DataSourceID` vlastnost `FormView` ovládacího prvku. `<InsertItemTemplate>` Části `FormView` obsahuje textové pole, která je rozšířit pomocí `TextBoxWatermarkExtender` ovládacího prvku. Ujistěte se, že rozšiřujícího objektu se nachází v rámci šablony a ne mimo.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

Nyní když uživatel změní do režimu vložení `FormView` řídit, pole textu pro nové dodavatele je předem Poděkování `TextBoxWatermarkExtender` ovládacího prvku. Klikněte do textového pole umožňuje výplň text zmizí.


[![Vodoznak v poli pochází z rozšiřujícího objektu](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

Vodoznak v poli pochází z rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

>[!div class="step-by-step"]
[Další](using-textboxwatermark-with-validation-controls-cs.md)
