---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: "Animace v odezvě na interakci s uživatelem (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Můžete hvězdičkami animací..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 3219e9d126b3225bfc78d08fb3ac7ef4cc3dca75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="f2124-104">Animace v odezvě na interakci s uživatelem (VB)</span><span class="sxs-lookup"><span data-stu-id="f2124-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="f2124-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f2124-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f2124-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f2124-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="f2124-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="f2124-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f2124-108">Animací spustit automaticky nebo může být aktivovány interakci s uživatelem, například kliknutím myší.</span><span class="sxs-lookup"><span data-stu-id="f2124-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="f2124-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="f2124-109">Overview</span></span>

<span data-ttu-id="f2124-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="f2124-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f2124-111">Animací spustit automaticky nebo může být aktivovány interakci s uživatelem, například kliknutím myší.</span><span class="sxs-lookup"><span data-stu-id="f2124-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="f2124-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="f2124-112">Steps</span></span>

<span data-ttu-id="f2124-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="f2124-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="f2124-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f2124-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="f2124-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="f2124-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="f2124-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="f2124-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="f2124-117">V rámci `<Animations>` uzlu pět způsobů, jak pro spuštění animace prostřednictvím zásah uživatele (chybí element je `<OnLoad>` který je proveden po celé stránky se načetl plně):</span><span class="sxs-lookup"><span data-stu-id="f2124-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="f2124-118">`<OnClick>`(myši klikněte na ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="f2124-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="f2124-119">`<OnHoverOut>`(ukazatel myši opustí ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="f2124-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="f2124-120">`<OnHoverOver>`(ukazatel myši nachází ovládacího prvku zastavení `<OnHoverOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="f2124-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="f2124-121">`<OnMouseOut>`(myši ponechá ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="f2124-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="f2124-122">`<OnMouseOver>`(ukazatel myši nachází ovládacího prvku není zastavení `<OnMouseOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="f2124-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="f2124-123">V tomto scénáři `<OnClick>` se používá.</span><span class="sxs-lookup"><span data-stu-id="f2124-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="f2124-124">Když uživatel klikne na panelu, se změnila velikost a setmívá ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="f2124-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="f2124-125">[![Animace bude spuštěna, klikněte na tlačítko myši](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f2124-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="f2124-126">Animace bude spuštěna, klikněte na tlačítko myši ([Kliknutím zobrazit obrázek v plné velikosti](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f2124-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f2124-127">[Předchozí](picking-one-animation-out-of-a-list-vb.md)
[další](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f2124-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
