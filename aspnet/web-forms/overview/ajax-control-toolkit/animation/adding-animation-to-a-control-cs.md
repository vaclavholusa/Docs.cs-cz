---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Přidání animace do ovládacího prvku (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Tento kurz ukazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="54771-104">Přidání animace do ovládacího prvku (C#)</span><span class="sxs-lookup"><span data-stu-id="54771-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="54771-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="54771-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="54771-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="54771-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="54771-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="54771-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="54771-108">Tento kurz ukazuje, jak nastavit takové animace.</span><span class="sxs-lookup"><span data-stu-id="54771-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="54771-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="54771-109">Overview</span></span>

<span data-ttu-id="54771-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="54771-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="54771-111">Tento kurz ukazuje, jak nastavit takové animace.</span><span class="sxs-lookup"><span data-stu-id="54771-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="54771-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="54771-112">Steps</span></span>

<span data-ttu-id="54771-113">Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="54771-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="54771-114">Animace v tomto scénáři se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="54771-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="54771-115">Související třídy CSS pro panel definuje barvu pozadí a šířku:</span><span class="sxs-lookup"><span data-stu-id="54771-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="54771-116">Další až, potřebujeme `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="54771-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="54771-117">Po zadání `ID` a obvykle `runat="server"`, `TargetControlID` musí být nastaven na ovládací prvek pro animaci v našem případě panelu:</span><span class="sxs-lookup"><span data-stu-id="54771-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="54771-118">Použití celou animace deklarativně, pomocí syntaxe XML bohužel momentálně není plně nepodporuje technologii IntelliSense v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54771-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="54771-119">Kořenový uzel je `<Animations>;` v rámci tohoto uzlu, jsou povoleny několik událostí, které určují, kdy animation(s) přezkumném místní:</span><span class="sxs-lookup"><span data-stu-id="54771-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="54771-120">`OnClick` (klikněte na tlačítko myši)</span><span class="sxs-lookup"><span data-stu-id="54771-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="54771-121">`OnHoverOut` (Pokud myši ponechá ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="54771-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="54771-122">`OnHoverOver` (pokud ukazatel myši nachází ovládacího prvku, zastavování `OnHoverOut` animace)</span><span class="sxs-lookup"><span data-stu-id="54771-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="54771-123">`OnLoad` (Pokud stránky byl načten)</span><span class="sxs-lookup"><span data-stu-id="54771-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="54771-124">`OnMouseOut` (Pokud myši ponechá ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="54771-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="54771-125">`OnMouseOver` (pokud ukazatel myši nachází ovládacího prvku, není zastavení `OnMouseOut` animace)</span><span class="sxs-lookup"><span data-stu-id="54771-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="54771-126">Rozhraní framework se dodává se sadou animací, každé z nich reprezentována vlastní – element XML.</span><span class="sxs-lookup"><span data-stu-id="54771-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="54771-127">Tady je výběr:</span><span class="sxs-lookup"><span data-stu-id="54771-127">Here is a selection:</span></span>

- <span data-ttu-id="54771-128">`<Color>` (a změníte barvu)</span><span class="sxs-lookup"><span data-stu-id="54771-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="54771-129">`<FadeIn>` (pozvolného vysouvání)</span><span class="sxs-lookup"><span data-stu-id="54771-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="54771-130">`<FadeOut>` (pozvolného vysouvání)</span><span class="sxs-lookup"><span data-stu-id="54771-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="54771-131">`<Property>` (Změna ovládacího prvku)</span><span class="sxs-lookup"><span data-stu-id="54771-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="54771-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="54771-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="54771-133">`<Resize>` (změně velikosti)</span><span class="sxs-lookup"><span data-stu-id="54771-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="54771-134">`<Scale>` (úměrně změně velikosti)</span><span class="sxs-lookup"><span data-stu-id="54771-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="54771-135">V tomto příkladu se objevovat panelu. Animace přijmou 1,5 sekund (`Duration` atribut), zobrazení 24 snímků (postup animace) za sekundu (`Fps` atributy).</span><span class="sxs-lookup"><span data-stu-id="54771-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="54771-136">Tady je značku dokončení `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="54771-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="54771-137">Při spuštění tohoto skriptu panelu se zobrazí a setmívá v jedné a půl sekundy.</span><span class="sxs-lookup"><span data-stu-id="54771-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="54771-138">[![Na panelu je pozvolného vysouvání](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="54771-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="54771-139">Na panelu je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="54771-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="54771-140">Next</span><span class="sxs-lookup"><span data-stu-id="54771-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
