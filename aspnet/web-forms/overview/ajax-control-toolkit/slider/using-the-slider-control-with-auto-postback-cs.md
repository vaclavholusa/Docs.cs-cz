---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Použití ovládacího prvku posuvník s funkcí Auto-Postback (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši. Je možné provádět automaticky zaúčtovat posuvníku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 8e855bebf557536b5f9e2b2730f828e54c63221e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366818"
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="757f0-104">Použití ovládacího prvku posuvník s funkcí Auto-Postback (C#)</span><span class="sxs-lookup"><span data-stu-id="757f0-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="757f0-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="757f0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="757f0-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="757f0-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="757f0-107">Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="757f0-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="757f0-108">Je možné měnit autopostback posuvník po jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="757f0-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="757f0-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="757f0-109">Overview</span></span>

<span data-ttu-id="757f0-110">Ovládací prvek posuvník v sadou nástrojů AJAX Control Toolkit poskytuje grafické posuvníku, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="757f0-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="757f0-111">Je možné měnit autopostback posuvník po jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="757f0-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="757f0-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="757f0-112">Steps</span></span>

<span data-ttu-id="757f0-113">Pokud chcete mít posuvník automaticky postback na změnu, potřebujete obou polí atribut `AutoPostBack="true"`: textové pole, které se stanou posuvník samotného a textové pole, která obsahuje pozice posuvníku.</span><span class="sxs-lookup"><span data-stu-id="757f0-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="757f0-114">Tady je požadované značky, které:</span><span class="sxs-lookup"><span data-stu-id="757f0-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="757f0-115">`SliderExtender` Ovládacího prvku z technologie ASP.NET AJAX Control Toolkit přiřadí funkce pro posuvník tato dvě textová pole:</span><span class="sxs-lookup"><span data-stu-id="757f0-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="757f0-116">Element další popisek se později použije k uživatel informován o zpětném odeslání:</span><span class="sxs-lookup"><span data-stu-id="757f0-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="757f0-117">Nakonec `ScriptManager` ovládací prvek technologie ASP.NET AJAX načte požadované jazyka JavaScript pro ovládací prvek Toolkit pracovat:</span><span class="sxs-lookup"><span data-stu-id="757f0-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="757f0-118">Nyní je posuvník účtování; zpět na straně serveru může tato událost zachycena a reagovali na ni:</span><span class="sxs-lookup"><span data-stu-id="757f0-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="757f0-119">[![Posunutím jezdce aktivuje zpětné volání](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="757f0-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="757f0-120">Posunutím jezdce aktivuje zpětné volání ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="757f0-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="757f0-121">[![Potom data této změny je napsána v popisku](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="757f0-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="757f0-122">Potom data této změny je napsána v popisku ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="757f0-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="757f0-123">Next</span><span class="sxs-lookup"><span data-stu-id="757f0-123">Next</span></span>](databinding-the-slider-control-cs.md)
