---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Datové vazby k typu Accordion (VB) | Microsoft Docs
author: wenz
description: Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarovány w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: 0739e4ad263eb83f844a937eae4aa845df2f2593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="databinding-to-an-accordion-vb"></a>Datové vazby k typu Accordion (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarované v rámci vlastní stránky, ale vazby ke zdroji dat nabízí větší flexibilitu.


## <a name="overview"></a>Přehled

Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarované v rámci vlastní stránky, ale vazby ke zdroji dat nabízí větší flexibilitu.

## <a name="steps"></a>Kroky

První řadě zdroj dat je vyžadován. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.

Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení. Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

Pak přidejte zdroje dat na stránku. Chcete-li použít omezené množství dat, jsme vybrat pouze prvních pět položek v tabulce dodavatele databázi AdventureWorks. Pokud používáte pomocníkem pro Visual Studio k vytvoření zdroje dat, paměti, že chyby v aktuální verzi není předpony názvu tabulky (`Vendor`) s `Purchasing`. Následující kód ukazuje správnou syntaxi:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Pamatujte název zdroje dat (ID). Tato velmi identifikace pak použije v `DataSourceID` vlastností ovládacího prvku typu Accordion:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

V ovládacím prvku typu Accordion, můžete zadat šablony pro různé části ovládacího prvku, včetně hlavičky (`<HeaderTemplate>`) a obsah (`<ContentTemplate>`). V rámci těchto prvků právě výstupní data z dat zdroje, pomocí `DataBinder.Eval()` metoda:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

Při načtení stránky zdroje dat musí být vázána na accordion s tímto kódem na straně serveru:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Dokončete tuto ukázku, musíte definovat dvě třídy CSS, které se odkazuje v ovládacím prvku typu Accordion (v jeho vlastnosti `HeaderCssClass` a `ContentCssClass`). Uveďte následující kód do `<head>` části stránky:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Data v prvku typu accordion pochází přímo ze zdroje dat.](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Data v prvku typu accordion pochází přímo ze zdroje dat ([Kliknutím zobrazit obrázek v plné velikosti](databinding-to-an-accordion-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-adding-an-accordion-pane-cs.md)
> [další](dynamically-adding-an-accordion-pane-vb.md)
