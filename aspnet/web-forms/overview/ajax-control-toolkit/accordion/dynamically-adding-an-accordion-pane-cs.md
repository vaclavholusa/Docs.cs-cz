---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: "Dynamické přidávání podokně aplikace Accordion (C#) | Microsoft Docs"
author: wenz
description: "Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarovány w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c6af79186ca21082647beec500c47974a47c35a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="ecd8a-104">Dynamické přidávání podokně aplikace Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="ecd8a-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="ecd8a-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ecd8a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ecd8a-106">[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ecd8a-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="ecd8a-107">Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="ecd8a-108">Panely jsou obvykle deklarované v rámci vlastní stránky, ale kódu na straně serveru můžete použít k dosažení stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="ecd8a-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="ecd8a-109">Overview</span></span>

<span data-ttu-id="ecd8a-110">Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="ecd8a-111">Panely jsou obvykle deklarované v rámci vlastní stránky, ale kódu na straně serveru můžete použít k dosažení stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="ecd8a-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="ecd8a-112">Steps</span></span>

<span data-ttu-id="ecd8a-113">Ovládací prvek Accordion poskytuje všechny důležité vlastnosti do kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="ecd8a-114">Kromě jiných věcí `Panes` vlastnost uděluje přístup ke kolekci podokna, které tvoří prvku typu Accordion.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="ecd8a-115">Každý podokně není typu `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="ecd8a-116">Je proto trivial vytvořit takové podokno:</span><span class="sxs-lookup"><span data-stu-id="ecd8a-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="ecd8a-117">`HeaderContainer` Vlastnost `AccordionPane` poskytuje přístup k ovládacím prvkům ASP.NET v záhlaví části podokna; `ContentContainer` vlastnost `AccordionPane` nemá stejný pro obsah části podokna.</span><span class="sxs-lookup"><span data-stu-id="ecd8a-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="ecd8a-118">To umožňuje kódu ASP.NET přidání obsahu do podokna:</span><span class="sxs-lookup"><span data-stu-id="ecd8a-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="ecd8a-119">Nakonec pane(s) musí být přidán do `Panes` kolekce prvku typu Accordion:</span><span class="sxs-lookup"><span data-stu-id="ecd8a-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="ecd8a-120">Tady je kompletní serverový kód, který přidá dvě podokna na ovládací prvek Accordion:</span><span class="sxs-lookup"><span data-stu-id="ecd8a-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="ecd8a-121">Pouze chybí element je Accordion sebe, což závisí na přítomnost ASP.NET `ScriptManager` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="ecd8a-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="ecd8a-122">Dokončete příklad dvou tříd CSS, kterou se odkazuje v ovládacím prvku typu Accordion poskytují informace o stylu pro prohlížeč:</span><span class="sxs-lookup"><span data-stu-id="ecd8a-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="ecd8a-123">[![Data v prvku typu accordion, byl přidán dynamicky kódu na straně serveru](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ecd8a-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="ecd8a-124">Data v prvku typu accordion, byl přidán dynamicky kódu na straně serveru ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ecd8a-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ecd8a-125">[Předchozí](databinding-to-an-accordion-cs.md)
[další](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ecd8a-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
