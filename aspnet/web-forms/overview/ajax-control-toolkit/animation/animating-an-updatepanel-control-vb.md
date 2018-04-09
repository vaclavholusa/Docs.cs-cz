---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animace ovládacího prvku UpdatePanel (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="e0c60-104">Animace ovládacího prvku UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="e0c60-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="e0c60-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e0c60-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e0c60-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e0c60-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="e0c60-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="e0c60-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e0c60-108">Pro obsah ovládacího prvku UpdatePanel speciální rozšiřujícího objektu existuje, která je založena na rozhraní animace: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="e0c60-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="e0c60-109">Tento kurz ukazuje, jak nastavit takové animace UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="e0c60-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="e0c60-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="e0c60-110">Overview</span></span>

<span data-ttu-id="e0c60-111">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="e0c60-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e0c60-112">Pro obsah `UpdatePanel`, existuje speciální rozšiřujícího objektu, který založena na rozhraní animace: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="e0c60-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="e0c60-113">Tento kurz ukazuje, jak nastavit takové animace pro `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="e0c60-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="e0c60-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="e0c60-114">Steps</span></span>

<span data-ttu-id="e0c60-115">Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="e0c60-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="e0c60-116">Animace v tomto scénáři se použijí pro technologie ASP.NET `Wizard` umístěných v ovládací prvek webu `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="e0c60-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="e0c60-117">Tři kroky (libovolný) poskytují dostatek možností pro aktivaci postback:</span><span class="sxs-lookup"><span data-stu-id="e0c60-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="e0c60-118">Kód potřebné pro `UpdatePanelAnimationExtender` je velmi podobný kód používá pro ovládací prvek `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="e0c60-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="e0c60-119">V `TargetControlID` atribut poskytujeme `ID` z `UpdatePanel` pro animaci; v rámci `UpdatePanelAnimationExtender` ovládací prvek, `<Animations>` element obsahuje kód XML pro animation(s).</span><span class="sxs-lookup"><span data-stu-id="e0c60-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="e0c60-120">Je však jeden rozdíl: velikost události a obslužné rutiny událostí je omezená ve srovnání s `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="e0c60-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="e0c60-121">Pro `UpdatePanels`, pouze dvě z nich neexistuje:</span><span class="sxs-lookup"><span data-stu-id="e0c60-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="e0c60-122">`<OnUpdated>` Když se aktualizovala UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="e0c60-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="e0c60-123">`<OnUpdating>` Při spuštění UpdatePanel aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0c60-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="e0c60-124">V tomto scénáři nový obsah `UpdatePanel` (po postback) se objevovat se.</span><span class="sxs-lookup"><span data-stu-id="e0c60-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="e0c60-125">Toto je nezbytné značek pro tento:</span><span class="sxs-lookup"><span data-stu-id="e0c60-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="e0c60-126">Nyní vždy, když v rámci prvku UpdatePanel dojde k zpětné volání, nový obsah panelu objevovat se bez problémů.</span><span class="sxs-lookup"><span data-stu-id="e0c60-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="e0c60-127">[![V dalším kroku průvodce je pozvolného vysouvání](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e0c60-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="e0c60-128">V dalším kroku průvodce je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e0c60-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0c60-129">[Předchozí](changing-an-animation-using-client-side-code-vb.md)
> [další](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e0c60-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
