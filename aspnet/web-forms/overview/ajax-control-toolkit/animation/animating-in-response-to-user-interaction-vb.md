---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animace v reakci na interakci uživatele (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze hvězdičkami...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b774a058e715e4a98e767daf92886f24e627822
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815522"
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="c23fa-104">Animace v reakci na interakci uživatele (VB)</span><span class="sxs-lookup"><span data-stu-id="c23fa-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="c23fa-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c23fa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c23fa-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c23fa-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="c23fa-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="c23fa-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c23fa-108">Animace lze spustit automaticky nebo může být aktivované interakci s uživatelem, například po kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="c23fa-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="c23fa-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="c23fa-109">Overview</span></span>

<span data-ttu-id="c23fa-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="c23fa-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c23fa-111">Animace lze spustit automaticky nebo může být aktivované interakci s uživatelem, například po kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="c23fa-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="c23fa-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="c23fa-112">Steps</span></span>

<span data-ttu-id="c23fa-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="c23fa-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="c23fa-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c23fa-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="c23fa-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="c23fa-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="c23fa-116">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="c23fa-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="c23fa-117">V rámci `<Animations>` uzlu, jsou pět jeden ze způsobů spuštění animace prostřednictvím interakce uživatele (chybí element je `<OnLoad>` který je proveden, jakmile úplným načtením celé stránky):</span><span class="sxs-lookup"><span data-stu-id="c23fa-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="c23fa-118">`<OnClick>` (kliknutí myší na ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="c23fa-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="c23fa-119">`<OnHoverOut>` (ukazatel myši opustí ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="c23fa-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="c23fa-120">`<OnHoverOver>` (ovládací prvek, zastavuje se ukazatel myši nachází `<OnHoverOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="c23fa-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="c23fa-121">`<OnMouseOut>` (ukazatel myši opustí ovládací prvek)</span><span class="sxs-lookup"><span data-stu-id="c23fa-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="c23fa-122">`<OnMouseOver>` (myší na ovládací prvek není zastavení `<OnMouseOut>` animace)</span><span class="sxs-lookup"><span data-stu-id="c23fa-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="c23fa-123">V tomto scénáři `<OnClick>` se používá.</span><span class="sxs-lookup"><span data-stu-id="c23fa-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="c23fa-124">Když uživatel klikne na panelu, je velikost a setmívá ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="c23fa-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="c23fa-125">[![Animace bude spuštěna kliknutí myší](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c23fa-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="c23fa-126">Animace bude spuštěna kliknutí myší ([kliknutím ji zobrazíte obrázek v plné velikosti](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c23fa-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c23fa-127">[Předchozí](picking-one-animation-out-of-a-list-vb.md)
> [další](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c23fa-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
