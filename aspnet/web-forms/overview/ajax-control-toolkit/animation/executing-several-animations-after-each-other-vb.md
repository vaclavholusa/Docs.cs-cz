---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Provádění několik animací po sobě navzájem (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="42610-104">Provádění několik animací po sobě navzájem (VB)</span><span class="sxs-lookup"><span data-stu-id="42610-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="42610-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="42610-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="42610-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="42610-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="42610-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="42610-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="42610-108">Umožňuje spustit několik animací jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="42610-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="42610-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="42610-109">Overview</span></span>

<span data-ttu-id="42610-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="42610-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="42610-111">Umožňuje spustit několik animací jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="42610-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="42610-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="42610-112">Steps</span></span>

<span data-ttu-id="42610-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="42610-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="42610-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="42610-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="42610-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="42610-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="42610-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="42610-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="42610-117">V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky.</span><span class="sxs-lookup"><span data-stu-id="42610-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="42610-118">Obecně platí `<OnLoad>` přijímá pouze jeden animace.</span><span class="sxs-lookup"><span data-stu-id="42610-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="42610-119">Rozhraní framework animace umožňuje připojení k několika animace do jednoho pomocí `<Sequence>` elementu.</span><span class="sxs-lookup"><span data-stu-id="42610-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="42610-120">Všechny animace v rámci `<Sequence>` jsou prováděny jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="42610-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="42610-121">Tady je je možné kód pro `AnimationExtender` řízení, nejprve Příprava širší panelu a pak snížení jeho výšku:</span><span class="sxs-lookup"><span data-stu-id="42610-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="42610-122">Při spuštění tohoto skriptu panelu první získá širší a pak menší.</span><span class="sxs-lookup"><span data-stu-id="42610-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="42610-123">[![Nejprve je vyšší šířku](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="42610-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="42610-124">Nejprve je vyšší šířku ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="42610-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="42610-125">[![Pak je zmenšit výšku](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="42610-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="42610-126">Pak je zmenšit výšku ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="42610-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="42610-127">[Předchozí](executing-several-animations-at-the-same-time-vb.md)
> [další](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="42610-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
