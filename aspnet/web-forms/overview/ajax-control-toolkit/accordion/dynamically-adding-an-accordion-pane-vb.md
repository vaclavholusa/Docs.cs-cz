---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamické přidání podokna ovládacího prvku Accordion (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fdb95ee1ee93bc011a257e4e21c876dbbc7d2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820023"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamické přidání podokna ovládacího prvku Accordion (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované v rámci samotné stránky, ale kód na straně serveru slouží k dosažení stejného výsledku.


## <a name="overview"></a>Přehled

Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované v rámci samotné stránky, ale kód na straně serveru slouží k dosažení stejného výsledku.

## <a name="steps"></a>Kroky

Ovládací prvek Accordion poskytuje všechny důležité vlastnosti do kódu na straně serveru. Mimo jiné `Panes` vlastnost uděluje přístup ke kolekci podoken, které tvoří prvku typu Accordion. Každé podokno je typu `AccordionPane`. Proto je jednoduché vytvořit takové podokno:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Vlastnost `AccordionPane` poskytuje přístup k ovládacím prvkům technologie ASP.NET v záhlaví podokna; `ContentContainer` vlastnost `AccordionPane` dělá to samé pro části obsahu podokna. To umožňuje kódu ASP.NET k přidání obsahu do podokna:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Nakonec pane(s) musí být přidané do `Panes` kolekce prvku typu Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Tady je kompletní kód na straně serveru, který přidá dvě podokna ovládacího prvku Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Pouze chybí element je prvku typu Accordion samostatně, což závisí na přítomnosti technologie ASP.NET `ScriptManager` ovládacího prvku:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

K dokončení příkladu, dvě šablony stylů CSS třídy odkazuje v ovládacím prvku typu Accordion poskytují informace o stylu prohlížeče:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Data prvku typu accordion dynamicky přidal kód na straně serveru](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Data prvku typu accordion dynamicky přidal kód na straně serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](databinding-to-an-accordion-vb.md)
