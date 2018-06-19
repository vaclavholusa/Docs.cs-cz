---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Sbalení a rozšiřování panely z jazyka JavaScript (VB) | Microsoft Docs
author: wenz
description: CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf61cd0d8204a5405ba62cd3884d66ccb21968b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873115"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="72ce4-103">Sbalení a rozšiřování panely z jazyka JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="72ce4-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="72ce4-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="72ce4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="72ce4-105">[Stáhněte si kód](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="72ce4-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="72ce4-106">CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho znovu.</span><span class="sxs-lookup"><span data-stu-id="72ce4-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="72ce4-107">Tyto dvě akce se dá taky spustit z vlastního kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72ce4-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="72ce4-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="72ce4-108">Overview</span></span>

<span data-ttu-id="72ce4-109">CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho znovu.</span><span class="sxs-lookup"><span data-stu-id="72ce4-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="72ce4-110">Tyto dvě akce se dá taky spustit z vlastního kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72ce4-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="72ce4-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="72ce4-111">Steps</span></span>

<span data-ttu-id="72ce4-112">Nejdřív všech, vytvořte novou stránku ASP.NET a zahrnout `ScriptManager` v rámci ten `<form>` elementu.</span><span class="sxs-lookup"><span data-stu-id="72ce4-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="72ce4-113">Tento kód načte knihovny ASP.NET AJAX, který je požadován Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="72ce4-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="72ce4-114">Pak vytvořte panelu s nějaký text tak, aby si můžete prohlédnout účinek rozbalit nebo sbalit:</span><span class="sxs-lookup"><span data-stu-id="72ce4-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="72ce4-115">Jak je vidět na panelu odkazuje na třídu CSS, která se zobrazí zde (a v podstatě definuje barvu pozadí a šířka panelu):</span><span class="sxs-lookup"><span data-stu-id="72ce4-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="72ce4-116">`CollapsiblePanelExtender` Vyžaduje ovládací prvek `TargetControlID` atributů tak, aby sada nástrojů zná panelu sbalit nebo rozšířit na žádost:</span><span class="sxs-lookup"><span data-stu-id="72ce4-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="72ce4-117">Bohužel rozšiřujícího objektu aktuálně nevystavuje konkrétní rozhraní API pro sbalení nebo rozbalení panelu, ale některé nedokumentovanými metody provede.</span><span class="sxs-lookup"><span data-stu-id="72ce4-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="72ce4-118">Nejprve přidejte tři tlačítka HTML na stránku, který se potom aktivuje JavaScript na straně klienta sbalit nebo rozbalte obsah panelu:</span><span class="sxs-lookup"><span data-stu-id="72ce4-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="72ce4-119">V kódu jazyka JavaScript na straně klienta (práce s `<script type="text/javascript">`), `$find()` metoda se musí použít pro přístup k `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="72ce4-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="72ce4-120">`$find("cpe")` Vrátí odkaz na něj.</span><span class="sxs-lookup"><span data-stu-id="72ce4-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="72ce4-121">Odtud na konkrétní metody vyřeší na prováděné úloze.</span><span class="sxs-lookup"><span data-stu-id="72ce4-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="72ce4-122">Metoda pro otevření (rozšíření) se nazývá panelu `_doOpen()`; následující kód implementuje `doOpen()` funkce volána při kliknutí na první tlačítko:</span><span class="sxs-lookup"><span data-stu-id="72ce4-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="72ce4-123">Zavření nebo sbalení panelu, `_doClose()` metoda je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="72ce4-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="72ce4-124">Proto když uživatel klikne na tlačítko druhé, se nazývá následující kód v JavaScriptu:</span><span class="sxs-lookup"><span data-stu-id="72ce4-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="72ce4-125">Tlačítko třetí přepne stav panelu: z sbaleny do rozšiřovat a naopak.</span><span class="sxs-lookup"><span data-stu-id="72ce4-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="72ce4-126">`CollapsiblePanelExtender` Zpřístupní `toggle()` metoda, která zajišťuje přesně který: obrátí stav panelu.</span><span class="sxs-lookup"><span data-stu-id="72ce4-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="72ce4-127">Je však také jiné přístup (která vnitřně používá `toggle()` metoda): `get_Collapsed()` metodu `CollapsiblePanelExtender()` nám oznamuje, zda je panel sbalený nebo ne.</span><span class="sxs-lookup"><span data-stu-id="72ce4-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="72ce4-128">V závislosti na návratovou hodnotu této funkce, panelu je pak buď rozšířit (`_doOpen()` metoda) nebo sbalené (`_doClose()`) metoda:</span><span class="sxs-lookup"><span data-stu-id="72ce4-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="72ce4-129">[![Tlačítko třetí změní stav panelu: z sbalené rozšířené a zpět](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="72ce4-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="72ce4-130">Tlačítko třetí změní stav panelu: z sbalené rozšířené a pozadí ([Kliknutím zobrazit obrázek v plné velikosti](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="72ce4-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="72ce4-131">Předchozí</span><span class="sxs-lookup"><span data-stu-id="72ce4-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
