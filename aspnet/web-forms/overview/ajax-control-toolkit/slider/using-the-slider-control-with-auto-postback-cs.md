---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Použití ovládacího prvku posuvník s automatického provedení operace (C#) | Microsoft Docs
author: wenz
description: Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné, aby automaticky zaúčtovat posuvníku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="8d8e6-104">Použití ovládacího prvku posuvník s automatického provedení operace (C#)</span><span class="sxs-lookup"><span data-stu-id="8d8e6-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="8d8e6-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8d8e6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8d8e6-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8d8e6-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="8d8e6-107">Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8d8e6-108">Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="8d8e6-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="8d8e6-109">Overview</span></span>

<span data-ttu-id="8d8e6-110">Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8d8e6-111">Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="8d8e6-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="8d8e6-112">Steps</span></span>

<span data-ttu-id="8d8e6-113">Aby bylo možné posuvník automaticky zpětného volání při změně, třeba obou polí atribut `AutoPostBack="true"`: textové pole, které bude posuvník sám a textové pole, které obsahuje pozici posuvníku.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="8d8e6-114">Zde je kód požadované pro tento:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="8d8e6-115">`SliderExtender` Řízení z ovládacího prvku ASP.NET AJAX Toolkit přiřadí funkci posuvník do dvou textových polí:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="8d8e6-116">Element label další se později použije informovat uživatele zpětné volání:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="8d8e6-117">Nakonec `ScriptManager` ovládacího prvku ASP.NET AJAX načte požadované JavaScript pro Toolkitu postup:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="8d8e6-118">Nyní je jezdec publikování zpět; Tato událost může na straně serveru, místo zachycení a reagovali na ni:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="8d8e6-119">[![Posunutím jezdce aktivuje zpětné volání](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8d8e6-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="8d8e6-120">Posunutím jezdce aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8d8e6-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="8d8e6-121">[![Později datum této změny je napsána v popisku](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8d8e6-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="8d8e6-122">Později, datum této změny je napsána v popisku ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8d8e6-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8d8e6-123">Next</span><span class="sxs-lookup"><span data-stu-id="8d8e6-123">Next</span></span>](databinding-the-slider-control-cs.md)
