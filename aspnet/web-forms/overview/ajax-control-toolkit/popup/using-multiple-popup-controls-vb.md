---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Použití několika překryvných ovládacích prvků (VB) | Dokumentace Microsoftu
author: wenz
description: Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek. Je také možné použít m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 81b0dbd794d31bc3c55bff4ba8110c590b88b509
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757105"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="0b162-104">Použití několika překryvných ovládacích prvků (VB)</span><span class="sxs-lookup"><span data-stu-id="0b162-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="0b162-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0b162-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0b162-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0b162-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="0b162-107">Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="0b162-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0b162-108">Je také možné použít více než jeden ovládací prvek popup na jedné stránce.</span><span class="sxs-lookup"><span data-stu-id="0b162-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="0b162-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="0b162-109">Overview</span></span>

<span data-ttu-id="0b162-110">Zařízení extender PopupControl v sadou nástrojů AJAX Control Toolkit nabízí snadný způsob, jak aktivovat automaticky otevíraného okna, když se aktivuje jiný ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="0b162-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0b162-111">Je také možné použít více než jeden ovládací prvek popup na jedné stránce.</span><span class="sxs-lookup"><span data-stu-id="0b162-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="0b162-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="0b162-112">Steps</span></span>

<span data-ttu-id="0b162-113">K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="0b162-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="0b162-114">V dalším kroku přidáte panel, který slouží jako automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="0b162-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="0b162-115">V aktuální scénář obsahuje panel `Calendar` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="0b162-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="0b162-116">Pokud se chcete vyhnout aktualizuje stránku způsobené postbacků kalendáře, se zařadí na panelu v rámci `UpdatePanel` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="0b162-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="0b162-117">Stránka také obsahuje dvě textová pole.</span><span class="sxs-lookup"><span data-stu-id="0b162-117">The page also contains two text boxes.</span></span> <span data-ttu-id="0b162-118">Pro každé pole kalendář automaticky otevírané okno se zobrazí po aktivaci do textového pole.</span><span class="sxs-lookup"><span data-stu-id="0b162-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="0b162-119">Rozšiřte každou z tato dvě textová pole se teď `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="0b162-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="0b162-120">`TargetControlID` Atribut obsahuje ID vázané na zařízení extender ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="0b162-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="0b162-121">`PopupControlID` Atribut obsahuje ID panelu automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="0b162-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="0b162-122">V takovém případě obou zařízení Extender zobrazit stejný panel, ale jiné panely je to možné, jsou také.</span><span class="sxs-lookup"><span data-stu-id="0b162-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="0b162-123">Nyní pokaždé, když kliknete do textového pole, kalendář zobrazí pod polem, což vám umožní vybrat datum.</span><span class="sxs-lookup"><span data-stu-id="0b162-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="0b162-124">(Načítání vybraného data zpět do textových polí se budeme v různých kurz.)</span><span class="sxs-lookup"><span data-stu-id="0b162-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="0b162-125">[![Když uživatel klikne do textového pole, zobrazí se v kalendáři](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0b162-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="0b162-126">Když uživatel klikne do textového pole, zobrazí se v kalendáři ([kliknutím ji zobrazíte obrázek v plné velikosti](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0b162-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b162-127">[Předchozí](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [další](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0b162-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
