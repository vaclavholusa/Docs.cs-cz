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
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="943a2-104">Dynamické přidávání podokně aplikace Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="943a2-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="943a2-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="943a2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="943a2-106">[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="943a2-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="943a2-107">Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy.</span><span class="sxs-lookup"><span data-stu-id="943a2-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="943a2-108">Panely jsou obvykle deklarované v rámci vlastní stránky, ale kódu na straně serveru můžete použít k dosažení stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="943a2-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="943a2-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="943a2-109">Overview</span></span>

<span data-ttu-id="943a2-110">Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy.</span><span class="sxs-lookup"><span data-stu-id="943a2-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="943a2-111">Panely jsou obvykle deklarované v rámci vlastní stránky, ale kódu na straně serveru můžete použít k dosažení stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="943a2-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="943a2-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="943a2-112">Steps</span></span>

<span data-ttu-id="943a2-113">Ovládací prvek Accordion poskytuje všechny důležité vlastnosti do kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="943a2-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="943a2-114">Kromě jiných věcí `Panes` vlastnost uděluje přístup ke kolekci podokna, které tvoří prvku typu Accordion.</span><span class="sxs-lookup"><span data-stu-id="943a2-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="943a2-115">Každý podokně není typu `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="943a2-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="943a2-116">Je proto trivial vytvořit takové podokno:</span><span class="sxs-lookup"><span data-stu-id="943a2-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="943a2-117">`HeaderContainer` Vlastnost `AccordionPane` poskytuje přístup k ovládacím prvkům ASP.NET v záhlaví části podokna; `ContentContainer` vlastnost `AccordionPane` nemá stejný pro obsah části podokna.</span><span class="sxs-lookup"><span data-stu-id="943a2-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="943a2-118">To umožňuje kódu ASP.NET přidání obsahu do podokna:</span><span class="sxs-lookup"><span data-stu-id="943a2-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="943a2-119">Nakonec pane(s) musí být přidán do `Panes` kolekce prvku typu Accordion:</span><span class="sxs-lookup"><span data-stu-id="943a2-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="943a2-120">Tady je kompletní serverový kód, který přidá dvě podokna na ovládací prvek Accordion:</span><span class="sxs-lookup"><span data-stu-id="943a2-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="943a2-121">Pouze chybí element je Accordion sebe, což závisí na přítomnost ASP.NET `ScriptManager` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="943a2-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="943a2-122">Dokončete příklad dvou tříd CSS, kterou se odkazuje v ovládacím prvku typu Accordion poskytují informace o stylu pro prohlížeč:</span><span class="sxs-lookup"><span data-stu-id="943a2-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="943a2-123">[![Data v prvku typu accordion, byl přidán dynamicky kódu na straně serveru](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="943a2-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="943a2-124">Data v prvku typu accordion, byl přidán dynamicky kódu na straně serveru ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="943a2-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="943a2-125">Předchozí</span><span class="sxs-lookup"><span data-stu-id="943a2-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
