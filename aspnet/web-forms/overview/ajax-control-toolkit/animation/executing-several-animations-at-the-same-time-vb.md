---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Postupné provedení několika animací ve stejnou dobu (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Umožňuje spustit severa...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: b748f7f3f8344ff3c87230e26887c761737d4489
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814685"
---
<a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="6a627-104">Postupné provedení několika animací ve stejnou dobu (VB)</span><span class="sxs-lookup"><span data-stu-id="6a627-104">Executing Several Animations at The Same Time (VB)</span></span>
====================
<span data-ttu-id="6a627-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6a627-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6a627-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6a627-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="6a627-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="6a627-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6a627-108">To umožňuje spuštění několika animací paralelní způsobem.</span><span class="sxs-lookup"><span data-stu-id="6a627-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="6a627-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="6a627-109">Overview</span></span>

<span data-ttu-id="6a627-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="6a627-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6a627-111">To umožňuje spuštění několika animací paralelní způsobem.</span><span class="sxs-lookup"><span data-stu-id="6a627-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="6a627-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="6a627-112">Steps</span></span>

<span data-ttu-id="6a627-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="6a627-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="6a627-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6a627-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="6a627-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="6a627-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="6a627-116">Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="6a627-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="6a627-117">V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile úplným načtením stránky.</span><span class="sxs-lookup"><span data-stu-id="6a627-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="6a627-118">Obecně platí `<OnLoad>` přijímá pouze jeden animace.</span><span class="sxs-lookup"><span data-stu-id="6a627-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="6a627-119">Animace framework umožňuje připojit k několika animací do jednoho pomocí `<Parallel>` elementu.</span><span class="sxs-lookup"><span data-stu-id="6a627-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="6a627-120">Všechny animace v rámci `<Parallel>` jsou spouštěny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6a627-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="6a627-121">Tady je možné značky `AnimationExtender` ovládacího prvku, mizení a změnu velikosti panelu ve stejnou dobu:</span><span class="sxs-lookup"><span data-stu-id="6a627-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="6a627-122">A skutečně: při spuštění tohoto skriptu se zobrazí na panelu a pak změní velikost (více než threefolding svou šířku a halfing jeho výška) a setmívá ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6a627-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="6a627-123">[![Na panelu je mizení a změna velikosti (včetně jeho obsahu díky prohlížeče vykreslovací modul)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6a627-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="6a627-124">Na panelu je mizení a změna velikosti (včetně jeho obsahu díky prohlížeče vykreslovací modul) ([kliknutím ji zobrazíte obrázek v plné velikosti](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6a627-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6a627-125">[Předchozí](adding-animation-to-a-control-vb.md)
> [další](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6a627-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
