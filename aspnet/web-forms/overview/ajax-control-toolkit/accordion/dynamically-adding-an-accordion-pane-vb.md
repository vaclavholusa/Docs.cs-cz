---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamické přidávání podokně aplikace Accordion (VB) | Microsoft Docs
author: wenz
description: Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarovány w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamické přidávání podokně aplikace Accordion (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarované v rámci vlastní stránky, ale kódu na straně serveru můžete použít k dosažení stejného výsledku.


## <a name="overview"></a>Přehled

Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarované v rámci vlastní stránky, ale kódu na straně serveru můžete použít k dosažení stejného výsledku.

## <a name="steps"></a>Kroky

Ovládací prvek Accordion poskytuje všechny důležité vlastnosti do kódu na straně serveru. Kromě jiných věcí `Panes` vlastnost uděluje přístup ke kolekci podokna, které tvoří prvku typu Accordion. Každý podokně není typu `AccordionPane`. Je proto trivial vytvořit takové podokno:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Vlastnost `AccordionPane` poskytuje přístup k ovládacím prvkům ASP.NET v záhlaví části podokna; `ContentContainer` vlastnost `AccordionPane` nemá stejný pro obsah části podokna. To umožňuje kódu ASP.NET přidání obsahu do podokna:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Nakonec pane(s) musí být přidán do `Panes` kolekce prvku typu Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Tady je kompletní serverový kód, který přidá dvě podokna na ovládací prvek Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Pouze chybí element je Accordion sebe, což závisí na přítomnost ASP.NET `ScriptManager` ovládacího prvku:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Dokončete příklad dvou tříd CSS, kterou se odkazuje v ovládacím prvku typu Accordion poskytují informace o stylu pro prohlížeč:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Data v prvku typu accordion, byl přidán dynamicky kódu na straně serveru](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Data v prvku typu accordion, byl přidán dynamicky kódu na straně serveru ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](databinding-to-an-accordion-vb.md)
