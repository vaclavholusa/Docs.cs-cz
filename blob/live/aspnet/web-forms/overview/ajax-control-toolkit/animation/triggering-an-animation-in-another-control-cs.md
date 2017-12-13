---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: "Spuštění animace v další ovládací prvek (C#) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Obecně platí, spouštění..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d243eebc42b66f1e86b38a1b7531e527144ea7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="e0cee-104">Spuštění animace v další ovládací prvek (C#)</span><span class="sxs-lookup"><span data-stu-id="e0cee-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="e0cee-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e0cee-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e0cee-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e0cee-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="e0cee-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="e0cee-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e0cee-108">Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="e0cee-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="e0cee-109">Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e0cee-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="e0cee-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="e0cee-110">Overview</span></span>

<span data-ttu-id="e0cee-111">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="e0cee-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e0cee-112">Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="e0cee-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="e0cee-113">Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e0cee-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="e0cee-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="e0cee-114">Steps</span></span>

<span data-ttu-id="e0cee-115">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="e0cee-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="e0cee-116">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e0cee-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="e0cee-117">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="e0cee-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="e0cee-118">Pokud chcete začít animace panelu, je HTML tlačítko použít.</span><span class="sxs-lookup"><span data-stu-id="e0cee-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="e0cee-119">Všimněte si, že `<input type="button" />` je nejvyšších přes `<asp:Button />` vzhledem k tomu, že jsme nechcete, aby zpětné volání po kliknutí na toto tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0cee-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="e0cee-120">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="e0cee-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="e0cee-121">Je důležité nastavit `TargetControlID` na ID tlačítko (elementu spuštění animace), ne na ID panelu (elementu se animovaný)</span><span class="sxs-lookup"><span data-stu-id="e0cee-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="e0cee-122">V rámci `<Animations>` uzlu, místní animací jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="e0cee-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="e0cee-123">Aby bylo možné je změnit panelu, ne na tlačítko nastavit `AnimationTarget` atribut pro každý element animace, v rámci `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="e0cee-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="e0cee-124">Hodnota `AnimationTarget` samozřejmě je ID panelu.</span><span class="sxs-lookup"><span data-stu-id="e0cee-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="e0cee-125">Tímto způsobem animací dojít s panelem, ne při spouštěcí tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0cee-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="e0cee-126">Tady je `AnimationExtender` kód pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="e0cee-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="e0cee-127">Všimněte si, speciální pořadí, ve kterém se zobrazují jednotlivé animace.</span><span class="sxs-lookup"><span data-stu-id="e0cee-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="e0cee-128">Tlačítko získá první řadě deaktivovat po spuštění animace.</span><span class="sxs-lookup"><span data-stu-id="e0cee-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="e0cee-129">Vzhledem k tomu, že je žádné `AnimationTarget` atribut `<EnableAction>` elementu, tato animace se použije pro původní ovládacího prvku: tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0cee-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="e0cee-130">Animace následující dva kroky se provádějí parallelly (`<Parallel>` element).</span><span class="sxs-lookup"><span data-stu-id="e0cee-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="e0cee-131">Mají obě jejich `AnimationTarget` atributy nastavit na `"Panel1"`, proto animace panelu, není tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0cee-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="e0cee-132">[![Panel animace spuštěna, myši klikněte na tlačítko](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e0cee-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="e0cee-133">Panel animace spuštěna, myši klikněte na tlačítko ([Kliknutím zobrazit obrázek v plné velikosti](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e0cee-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e0cee-134">[Předchozí](disabling-actions-during-animation-cs.md)
[další](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e0cee-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
