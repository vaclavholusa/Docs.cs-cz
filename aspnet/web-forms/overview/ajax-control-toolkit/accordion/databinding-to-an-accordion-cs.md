---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: "Datové vazby k typu Accordion (C#) | Microsoft Docs"
author: wenz
description: "Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarovány w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a>Datové vazby k typu Accordion (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarované v rámci vlastní stránky, ale vazby ke zdroji dat nabízí větší flexibilitu.


## <a name="overview"></a>Přehled

Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarované v rámci vlastní stránky, ale vazby ke zdroji dat nabízí větší flexibilitu.

## <a name="steps"></a>Kroky

První řadě zdroj dat je vyžadován. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.

Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení. Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Pak přidejte zdroje dat na stránku. Chcete-li použít omezené množství dat, jsme vybrat pouze prvních pět položek v tabulce dodavatele databázi AdventureWorks. Pokud používáte pomocníkem pro Visual Studio k vytvoření zdroje dat, paměti, že chyby v aktuální verzi není předpony názvu tabulky (`Vendor`) s `Purchasing`. Následující kód ukazuje správnou syntaxi:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Pamatujte název zdroje dat (ID). Tato velmi identifikace pak použije v `DataSourceID` vlastností ovládacího prvku typu Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

V ovládacím prvku typu Accordion, můžete zadat šablony pro různé části ovládacího prvku, včetně hlavičky (`<HeaderTemplate>`) a obsah (`<ContentTemplate>`). V rámci těchto prvků právě výstupní data z dat zdroje, pomocí `DataBinder.Eval()` metoda:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Při načtení stránky zdroje dat musí být vázána na accordion s tímto kódem na straně serveru:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Dokončete tuto ukázku, musíte definovat dvě třídy CSS, které se odkazuje v ovládacím prvku typu Accordion (v jeho vlastnosti `HeaderCssClass` a `ContentCssClass`). Uveďte následující kód do `<head>` části stránky:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Data v prvku typu accordion pochází přímo ze zdroje dat.](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Data v prvku typu accordion pochází přímo ze zdroje dat ([Kliknutím zobrazit obrázek v plné velikosti](databinding-to-an-accordion-cs/_static/image3.png))

>[!div class="step-by-step"]
[Další](dynamically-adding-an-accordion-pane-cs.md)
