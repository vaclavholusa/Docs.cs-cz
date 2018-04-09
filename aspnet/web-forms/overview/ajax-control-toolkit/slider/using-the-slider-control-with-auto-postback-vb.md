---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Použití ovládacího prvku posuvník s Auto-Postback (VB) | Microsoft Docs
author: wenz
description: Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné, aby automaticky zaúčtovat posuvníku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="9a6a8-104">Použití ovládacího prvku posuvník s Auto-Postback (VB)</span><span class="sxs-lookup"><span data-stu-id="9a6a8-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="9a6a8-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9a6a8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9a6a8-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9a6a8-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="9a6a8-107">Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="9a6a8-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="9a6a8-108">Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9a6a8-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="9a6a8-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="9a6a8-109">Overview</span></span>

<span data-ttu-id="9a6a8-110">Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši.</span><span class="sxs-lookup"><span data-stu-id="9a6a8-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="9a6a8-111">Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9a6a8-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="9a6a8-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="9a6a8-112">Steps</span></span>

<span data-ttu-id="9a6a8-113">Aby bylo možné posuvník automaticky zpětného volání při změně, třeba obou polí atribut `AutoPostBack="true"`: textové pole, které bude posuvník sám a textové pole, které obsahuje pozici posuvníku.</span><span class="sxs-lookup"><span data-stu-id="9a6a8-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="9a6a8-114">Zde je kód požadované pro tento:</span><span class="sxs-lookup"><span data-stu-id="9a6a8-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="9a6a8-115">`SliderExtender` Řízení z ovládacího prvku ASP.NET AJAX Toolkit přiřadí funkci posuvník do dvou textových polí:</span><span class="sxs-lookup"><span data-stu-id="9a6a8-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="9a6a8-116">Element label další se později použije informovat uživatele zpětné volání:</span><span class="sxs-lookup"><span data-stu-id="9a6a8-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="9a6a8-117">Nakonec `ScriptManager` ovládacího prvku ASP.NET AJAX načte požadované JavaScript pro Toolkitu postup:</span><span class="sxs-lookup"><span data-stu-id="9a6a8-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="9a6a8-118">Nyní je jezdec publikování zpět; Tato událost může na straně serveru, místo zachycení a reagovali na ni:</span><span class="sxs-lookup"><span data-stu-id="9a6a8-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="9a6a8-119">[![Posunutím jezdce aktivuje zpětné volání](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9a6a8-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="9a6a8-120">Posunutím jezdce aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9a6a8-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="9a6a8-121">[![Později datum této změny je napsána v popisku](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9a6a8-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="9a6a8-122">Později, datum této změny je napsána v popisku ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9a6a8-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9a6a8-123">[Předchozí](databinding-the-slider-control-cs.md)
> [další](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9a6a8-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
