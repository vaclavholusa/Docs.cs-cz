---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Datová vazba ovládacího prvku Accordion (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: e21eeacf776111656eb539b1fef8203db1b761c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805885"
---
<a name="databinding-to-an-accordion-vb"></a>Datová vazba ovládacího prvku Accordion (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované v rámci samotné stránky, ale vazba ke zdroji dat nabízí větší flexibilitu.


## <a name="overview"></a>Přehled

Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované v rámci samotné stránky, ale vazba ke zdroji dat nabízí větší flexibilitu.

## <a name="steps"></a>Kroky

Za prvé zdroj dat je povinný. Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition. Databáze je volitelná součást instalace sady Visual Studio (včetně express edition) a jsou také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Databáze AdventureWorks je součástí sad SQL Server 2005 ukázky a Sample Databases (stáhnout na [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Nejjednodušší způsob, jak nastavit databázi je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojit `AdventureWorks.mdf` databázový soubor.

V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení. Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi.

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

Zadejte zdroj dat na stránku. Chcete-li použít omezené množství dat, vybereme pouze prvních pět záznamů v tabulce dodavatele databáze AdventureWorks. Pokud používáte Pomocníka s nastavením Visual Studio k vytvoření zdroje dat, mějte na paměti, že chyby v aktuální verzi předponu názvu tabulky (`Vendor`) s `Purchasing`. Následující kód ukazuje správná syntaxe:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Pamatujte název zdroje dat (ID). Tato velmi identifikace musí použít ve `DataSourceID` vlastnost ovládacího prvku typu Accordion:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

V ovládacím prvku typu Accordion může poskytnout šablony pro různé části ovládacího prvku, včetně hlavičky (`<HeaderTemplate>`) a obsah (`<ContentTemplate>`). V rámci těchto prvků právě výstupu zobrazí data z dat zdroj, pomocí `DataBinder.Eval()` metody:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

Když je stránka načtená, zdroj dat musí být vázán na prvku typu accordion s tímto kódem na straně serveru:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Dokončete tento příklad, musíte definovat dvě třídy CSS, které jsou odkazovány v ovládacím prvku typu Accordion (ve vlastnostech `HeaderCssClass` a `ContentCssClass`). Vložte následující kód `<head>` části stránky:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Data v prvku typu accordion pocházejí přímo ze zdroje dat.](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Data v prvku typu accordion pocházejí přímo ze zdroje dat ([kliknutím ji zobrazíte obrázek v plné velikosti](databinding-to-an-accordion-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-adding-an-accordion-pane-cs.md)
> [další](dynamically-adding-an-accordion-pane-vb.md)
