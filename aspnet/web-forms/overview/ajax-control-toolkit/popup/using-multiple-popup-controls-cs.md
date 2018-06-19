---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Použití více ovládacích prvků místní (C#) | Microsoft Docs
author: wenz
description: Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Je také možné použít m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 7acd1b53e1b3e3e0d09d248b68941b166da3e81e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869683"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="65a85-104">Použití více ovládacích prvků místní (C#)</span><span class="sxs-lookup"><span data-stu-id="65a85-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="65a85-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="65a85-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="65a85-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="65a85-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="65a85-107">Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="65a85-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="65a85-108">Je také možné použít více než jeden prvek automaticky otevřeném okně na jedné stránce.</span><span class="sxs-lookup"><span data-stu-id="65a85-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="65a85-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="65a85-109">Overview</span></span>

<span data-ttu-id="65a85-110">Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="65a85-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="65a85-111">Je také možné použít více než jeden prvek automaticky otevřeném okně na jedné stránce.</span><span class="sxs-lookup"><span data-stu-id="65a85-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="65a85-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="65a85-112">Steps</span></span>

<span data-ttu-id="65a85-113">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="65a85-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="65a85-114">Dál přidejte panel, který slouží jako místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="65a85-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="65a85-115">Scénář, aktuální, obsahuje panel `Calendar` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="65a85-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="65a85-116">Aby se zabránilo aktualizace stránky způsobené postback v kalendáři, panelu zprovozněn v rámci `UpdatePanel` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="65a85-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="65a85-117">Tato stránka také obsahuje dvě textová pole.</span><span class="sxs-lookup"><span data-stu-id="65a85-117">The page also contains two text boxes.</span></span> <span data-ttu-id="65a85-118">Pro každého textového pole se zobrazí místní nabídka kalendáře po aktivaci textového pole.</span><span class="sxs-lookup"><span data-stu-id="65a85-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="65a85-119">Nyní rozšiřte každou z do dvou textových polí s `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="65a85-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="65a85-120">`TargetControlID` Atribut poskytuje ID ovládacího prvku vázáno rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="65a85-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="65a85-121">`PopupControlID` Atribut obsahuje ID místní panelu.</span><span class="sxs-lookup"><span data-stu-id="65a85-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="65a85-122">V takovém případě obou Extender zobrazit panel stejné, ale jiné panelů je také možné.</span><span class="sxs-lookup"><span data-stu-id="65a85-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="65a85-123">Nyní vždy, když kliknete na tlačítko v textovém poli, kalendář se objeví pod polem, což vám umožní vybrat datum.</span><span class="sxs-lookup"><span data-stu-id="65a85-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="65a85-124">(Získávání vybrané data zpět do textových polí se budeme v různých kurzu.)</span><span class="sxs-lookup"><span data-stu-id="65a85-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="65a85-125">[![Kalendář se zobrazí, když uživatel klikne do textového pole](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="65a85-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="65a85-126">Kalendář se zobrazí, když uživatel klikne do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="65a85-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="65a85-127">Next</span><span class="sxs-lookup"><span data-stu-id="65a85-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
