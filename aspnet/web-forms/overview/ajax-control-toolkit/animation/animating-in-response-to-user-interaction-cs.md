---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animace v reakci na interakci uživatele (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze hvězdičkami...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de99b3194a5517047e40922d89c28e341ba8e20
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752863"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="bf2fe-104">Animace v reakci na interakci uživatele (C#)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="bf2fe-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bf2fe-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="bf2fe-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="bf2fe-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bf2fe-108">Animace lze spustit automaticky nebo může být aktivované interakci s uživatelem, například po kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="bf2fe-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="bf2fe-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="bf2fe-109">Overview</span></span>

<span data-ttu-id="bf2fe-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="bf2fe-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bf2fe-111">Animace lze spustit automaticky nebo může být aktivované interakci s uživatelem, například po kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="bf2fe-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="bf2fe-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="bf2fe-112">Steps</span></span>

<span data-ttu-id="bf2fe-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="bf2fe-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="bf2fe-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="bf2fe-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="bf2fe-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="bf2fe-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="bf2fe-116">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="bf2fe-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="bf2fe-117">V rámci `<Animations>` uzlu, jsou pět jeden ze způsobů spuštění animace prostřednictvím interakce uživatele (chybí element je `<OnLoad>` který je proveden, jakmile úplným načtením celé stránky):</span><span class="sxs-lookup"><span data-stu-id="bf2fe-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="bf2fe-118">`<OnClick>` (kliknutí myší na ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="bf2fe-119">`<OnHoverOut>` (ukazatel myši opustí ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="bf2fe-120">`<OnHoverOver>` (ovládací prvek, zastavuje se ukazatel myši nachází `<OnHoverOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="bf2fe-121">`<OnMouseOut>` (ukazatel myši opustí ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="bf2fe-122">`<OnMouseOver>` (myší na ovládací prvek není zastavení `<OnMouseOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="bf2fe-123">V tomto scénáři `<OnClick>` se používá.</span><span class="sxs-lookup"><span data-stu-id="bf2fe-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="bf2fe-124">Když uživatel klikne na panelu, je velikost a setmívá ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="bf2fe-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="bf2fe-125">[![Animace bude spuštěna kliknutí myší](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="bf2fe-126">Animace bude spuštěna kliknutí myší ([kliknutím ji zobrazíte obrázek v plné velikosti](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bf2fe-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf2fe-127">[Předchozí](picking-one-animation-out-of-a-list-cs.md)
> [další](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bf2fe-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
