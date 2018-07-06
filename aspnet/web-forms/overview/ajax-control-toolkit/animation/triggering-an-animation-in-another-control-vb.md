---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Aktivace animace v jiného ovládacího prvku (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Obecně platí, spouští se...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d9ef7d35e34bd4f2b1433f7fb02d0c48834b2357
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804889"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="efc8f-104">Aktivace animace v jiného ovládacího prvku (VB)</span><span class="sxs-lookup"><span data-stu-id="efc8f-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="efc8f-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="efc8f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="efc8f-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="efc8f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="efc8f-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="efc8f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="efc8f-108">Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="efc8f-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="efc8f-109">Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="efc8f-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="efc8f-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="efc8f-110">Overview</span></span>

<span data-ttu-id="efc8f-111">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="efc8f-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="efc8f-112">Obecně platí spuštění animace se aktivuje interakce uživatele s stejný ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="efc8f-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="efc8f-113">Je však také možné pracovat s jeden ovládací prvek a potom animace jiného ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="efc8f-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="efc8f-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="efc8f-114">Steps</span></span>

<span data-ttu-id="efc8f-115">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="efc8f-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="efc8f-116">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="efc8f-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="efc8f-117">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="efc8f-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="efc8f-118">Pokud chcete začít animace panelu, je HTML tlačítko použít.</span><span class="sxs-lookup"><span data-stu-id="efc8f-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="efc8f-119">Všimněte si, že `<input type="button" />` nejvyšších přes `<asp:Button />` od tudy zpětné volání, když uživatel klikne na toto tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efc8f-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="efc8f-120">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="efc8f-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="efc8f-121">Je důležité nastavit `TargetControlID` ID tlačítko (elementu animace aktivace), aby Identifikátor panelu (elementu animované)</span><span class="sxs-lookup"><span data-stu-id="efc8f-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="efc8f-122">V rámci `<Animations>` uzlu, místo animace jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="efc8f-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="efc8f-123">Aby bylo možné je měnit panelu, není toto tlačítko, nastavte `AnimationTarget` atribut pro každý prvek animace v rámci `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="efc8f-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="efc8f-124">Hodnota pro `AnimationTarget` je samozřejmě ID panelu.</span><span class="sxs-lookup"><span data-stu-id="efc8f-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="efc8f-125">Tímto způsobem animací dojít s panelem, nikoli zpětným na spouštěcí tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efc8f-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="efc8f-126">Tady je `AnimationExtender` kód pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="efc8f-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="efc8f-127">Poznámka: zvláštní pořadí, ve kterém se zobrazují jednotlivé animace.</span><span class="sxs-lookup"><span data-stu-id="efc8f-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="efc8f-128">Za prvé získá tlačítko Deaktivovat po spuštění animace.</span><span class="sxs-lookup"><span data-stu-id="efc8f-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="efc8f-129">Protože neexistuje žádné `AnimationTarget` atribut v `<EnableAction>` element, tato animace se použije pro původní ovládacího prvku: tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efc8f-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="efc8f-130">Animace další dva kroky se provádějí parallelly (`<Parallel>` element).</span><span class="sxs-lookup"><span data-stu-id="efc8f-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="efc8f-131">Obě mají jejich `AnimationTarget` nastavte atributy na `"Panel1"`, tedy animace panelu, ne na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efc8f-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="efc8f-132">[![Panel animace spuštěna, kliknutí myší na tlačítko](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="efc8f-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="efc8f-133">Panel animace spuštěna, kliknutí myší na tlačítko ([kliknutím ji zobrazíte obrázek v plné velikosti](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="efc8f-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="efc8f-134">[Předchozí](disabling-actions-during-animation-vb.md)
> [další](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="efc8f-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
