---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Svázání posuvníku (VB) s daty | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné vytvořit vazbu aktuální pozice...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 284a650d1b63ceed887deb3ddd639fe9fd27019f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753326"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="92d9d-104">Datová vazba ovládacího prvku jezdec (VB)</span><span class="sxs-lookup"><span data-stu-id="92d9d-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="92d9d-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="92d9d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="92d9d-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="92d9d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="92d9d-107">Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="92d9d-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="92d9d-108">Je možné vytvořit vazbu aktuální pozice posuvníku na jiný ovládací prvek technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="92d9d-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="92d9d-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="92d9d-109">Overview</span></span>

<span data-ttu-id="92d9d-110">Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="92d9d-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="92d9d-111">Je možné vytvořit vazbu aktuální pozice posuvníku na jiný ovládací prvek technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="92d9d-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="92d9d-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="92d9d-112">Steps</span></span>

<span data-ttu-id="92d9d-113">K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="92d9d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="92d9d-114">V dalším kroku přidejte dva `TextBox` ovládacích prvků na stránce.</span><span class="sxs-lookup"><span data-stu-id="92d9d-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="92d9d-115">Jedna se transformuje na grafické posuvník a druhá bude obsahovat pozice posuvníku.</span><span class="sxs-lookup"><span data-stu-id="92d9d-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="92d9d-116">Dalším krokem je již v posledním kroku.</span><span class="sxs-lookup"><span data-stu-id="92d9d-116">The next step is already the final step.</span></span> <span data-ttu-id="92d9d-117">`SliderExtender` Ovládacího prvku z technologie ASP.NET AJAX Control Toolkit umožňuje posuvníku z prvního textového pole a automaticky aktualizuje do druhého textového pole, když změny pozice posuvníku.</span><span class="sxs-lookup"><span data-stu-id="92d9d-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="92d9d-118">Který chcete, aby `SliderExtender`společnosti `TargetControlID` atribut musí být nastaven na ID prvního textového pole; `BoundControlID` atribut musí být nastaven na ID do druhého textového pole.</span><span class="sxs-lookup"><span data-stu-id="92d9d-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="92d9d-119">Jak je vidět v prohlížeči, datové vazby funguje v obou směrech: zadáte novou hodnotu v textovém poli aktualizuje pozice posuvníku.</span><span class="sxs-lookup"><span data-stu-id="92d9d-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="92d9d-120">Pokud provedete do druhého textového pole jen pro čtení, můžete přidat slabé ochranu do textového pole tak, aby se pro uživatele k ruční aktualizaci hodnoty tady složitější.</span><span class="sxs-lookup"><span data-stu-id="92d9d-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="92d9d-121">[![Posuvníku a textového pole, které jsou synchronizované](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="92d9d-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="92d9d-122">Posuvníku a textového pole, které jsou synchronizované ([kliknutím ji zobrazíte obrázek v plné velikosti](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="92d9d-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="92d9d-123">Předchozí</span><span class="sxs-lookup"><span data-stu-id="92d9d-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
