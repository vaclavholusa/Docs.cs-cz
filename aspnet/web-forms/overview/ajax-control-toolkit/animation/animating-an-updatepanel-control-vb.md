---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animace ovládacího prvku UpdatePanel (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Pro obsah...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c508d549c7740079b02da323c655c9f789cfd3b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805298"
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="2a779-104">Animace ovládacího prvku UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="2a779-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="2a779-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2a779-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2a779-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2a779-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="2a779-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="2a779-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2a779-108">Obsah prvku UpdatePanel speciální rozšiřující objekt existuje, která se spoléhá na rozhraní .NET framework animace: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="2a779-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="2a779-109">Tento kurz ukazuje, jak nastavit tyto animace ovládacího prvku UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="2a779-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="2a779-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="2a779-110">Overview</span></span>

<span data-ttu-id="2a779-111">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="2a779-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2a779-112">Pro obsah `UpdatePanel`, speciální rozšiřující objekt existuje, která se spoléhá na rozhraní .NET framework animace: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="2a779-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="2a779-113">Tento kurz ukazuje, jak nastavit tyto animace pro `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="2a779-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="2a779-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="2a779-114">Steps</span></span>

<span data-ttu-id="2a779-115">Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby je načtena knihovna ASP.NET AJAX a Control Toolkit je možné:</span><span class="sxs-lookup"><span data-stu-id="2a779-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2a779-116">Animace v tomto scénáři se použijí pro ASP.NET `Wizard` webový ovládací prvek umístěný v `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="2a779-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="2a779-117">Tři kroky (libovolný) obsahují dostatek možnosti k aktivaci zpětného odeslání:</span><span class="sxs-lookup"><span data-stu-id="2a779-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2a779-118">Pro značky `UpdatePanelAnimationExtender` je velmi podobný značky použité pro ovládací prvek `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2a779-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="2a779-119">V `TargetControlID` atribut zajišťuje `ID` z `UpdatePanel` pro animaci; v rámci `UpdatePanelAnimationExtender` ovládací prvek, `<Animations>` element obsahuje kód XML pro animace.</span><span class="sxs-lookup"><span data-stu-id="2a779-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="2a779-120">Ale není jedním z rozdílů: omezen objem událostí a obslužných rutin událostí ve srovnání s `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2a779-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="2a779-121">Pro `UpdatePanels`, pouze dva z nich neexistuje:</span><span class="sxs-lookup"><span data-stu-id="2a779-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="2a779-122">`<OnUpdated>` Pokud byl aktualizován prvku UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="2a779-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="2a779-123">`<OnUpdating>` Aktualizuje se při spuštění prvku UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="2a779-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="2a779-124">V tomto scénáři, nový obsah `UpdatePanel` (po zpětném volání) se má vyblednout.</span><span class="sxs-lookup"><span data-stu-id="2a779-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="2a779-125">To je nezbytné zápis, který:</span><span class="sxs-lookup"><span data-stu-id="2a779-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="2a779-126">Nyní pokaždé, když se vyvolá se v rámci prvku UpdatePanel zpětné volání, nový obsah na panelu fade plynule.</span><span class="sxs-lookup"><span data-stu-id="2a779-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="2a779-127">[![V dalším kroku průvodce se pozvolného](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2a779-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2a779-128">V dalším kroku průvodce se pozvolného ([kliknutím ji zobrazíte obrázek v plné velikosti](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2a779-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a779-129">[Předchozí](changing-an-animation-using-client-side-code-vb.md)
> [další](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2a779-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
