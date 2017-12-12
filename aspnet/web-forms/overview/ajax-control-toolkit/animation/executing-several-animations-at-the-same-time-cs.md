---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: "Provádění několik animací ve stejnou dobu (C#) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit severa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: b7ee9c2da392bed512e259b44237e5ff997dca23
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="fb4e9-104">Provádění několik animací ve stejnou dobu (C#)</span><span class="sxs-lookup"><span data-stu-id="fb4e9-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="fb4e9-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fb4e9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fb4e9-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fb4e9-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="fb4e9-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fb4e9-108">Umožňuje spustit několik animací paralelní způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="fb4e9-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="fb4e9-109">Overview</span></span>

<span data-ttu-id="fb4e9-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fb4e9-111">Umožňuje spustit několik animací paralelní způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="fb4e9-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="fb4e9-112">Steps</span></span>

<span data-ttu-id="fb4e9-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="fb4e9-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="fb4e9-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fb4e9-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="fb4e9-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="fb4e9-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="fb4e9-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="fb4e9-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="fb4e9-117">V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="fb4e9-118">Obecně platí `<OnLoad>` přijímá pouze jeden animace.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="fb4e9-119">Rozhraní framework animace umožňuje připojení k několika animace do jednoho pomocí `<Parallel>` elementu.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="fb4e9-120">Všechny animace v rámci `<Parallel>` jsou spouštěny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="fb4e9-121">Tady je je možné kód pro `AnimationExtender` řízení, pozvolného vysouvání a změna velikosti panelu ve stejnou dobu:</span><span class="sxs-lookup"><span data-stu-id="fb4e9-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="fb4e9-122">A skutečně: při spuštění tohoto skriptu se zobrazí na panelu a potom změní (více než threefolding svou šířku a halfing výšku) a setmívá ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="fb4e9-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="fb4e9-123">[![Na panelu je pozvolného vysouvání a změna velikosti (včetně jeho obsahu, díky modul vykreslování prohlížeče)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fb4e9-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="fb4e9-124">Na panelu je pozvolného vysouvání a změna velikosti (včetně jeho obsahu, díky modul vykreslování prohlížeče) ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fb4e9-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fb4e9-125">[Předchozí](adding-animation-to-a-control-cs.md)
[další](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fb4e9-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
