---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animace v reakci na zásah uživatele (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Můžete hvězdičkami animací...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 783563f4e33087e99a071cf829ca6bab246ba3b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870944"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="255cc-104">Animace v reakci na zásah uživatele (C#)</span><span class="sxs-lookup"><span data-stu-id="255cc-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="255cc-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="255cc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="255cc-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="255cc-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="255cc-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="255cc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="255cc-108">Animací spustit automaticky nebo může být aktivovány interakci s uživatelem, například kliknutím myší.</span><span class="sxs-lookup"><span data-stu-id="255cc-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="255cc-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="255cc-109">Overview</span></span>

<span data-ttu-id="255cc-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="255cc-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="255cc-111">Animací spustit automaticky nebo může být aktivovány interakci s uživatelem, například kliknutím myší.</span><span class="sxs-lookup"><span data-stu-id="255cc-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="255cc-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="255cc-112">Steps</span></span>

<span data-ttu-id="255cc-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="255cc-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="255cc-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="255cc-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="255cc-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="255cc-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="255cc-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="255cc-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="255cc-117">V rámci `<Animations>` uzlu pět způsobů, jak pro spuštění animace prostřednictvím zásah uživatele (chybí element je `<OnLoad>` který je proveden po celé stránky se načetl plně):</span><span class="sxs-lookup"><span data-stu-id="255cc-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="255cc-118">`<OnClick>` (myši klikněte na ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="255cc-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="255cc-119">`<OnHoverOut>` (ukazatel myši opustí ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="255cc-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="255cc-120">`<OnHoverOver>` (ukazatel myši nachází ovládacího prvku zastavení `<OnHoverOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="255cc-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="255cc-121">`<OnMouseOut>` (myši ponechá ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="255cc-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="255cc-122">`<OnMouseOver>` (ukazatel myši nachází ovládacího prvku není zastavení `<OnMouseOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="255cc-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="255cc-123">V tomto scénáři `<OnClick>` se používá.</span><span class="sxs-lookup"><span data-stu-id="255cc-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="255cc-124">Když uživatel klikne na panelu, se změnila velikost a setmívá ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="255cc-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="255cc-125">[![Animace bude spuštěna, klikněte na tlačítko myši](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="255cc-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="255cc-126">Animace bude spuštěna, klikněte na tlačítko myši ([Kliknutím zobrazit obrázek v plné velikosti](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="255cc-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="255cc-127">[Předchozí](picking-one-animation-out-of-a-list-cs.md)
> [další](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="255cc-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
