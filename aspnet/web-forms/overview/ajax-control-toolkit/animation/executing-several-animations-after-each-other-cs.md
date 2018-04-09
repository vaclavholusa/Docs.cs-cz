---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Provádění několik animací po sobě navzájem (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Umožňuje spustit severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 836f0bba890a03e74ae62c2df029b7525b34275c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="0d144-104">Provádění několik animací po sobě navzájem (C#)</span><span class="sxs-lookup"><span data-stu-id="0d144-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="0d144-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0d144-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0d144-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0d144-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="0d144-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="0d144-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0d144-108">Umožňuje spustit několik animací jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="0d144-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="0d144-109">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="0d144-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0d144-110">Umožňuje spustit několik animací jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="0d144-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="0d144-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="0d144-111">Steps</span></span>

<span data-ttu-id="0d144-112">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="0d144-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="0d144-113">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0d144-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="0d144-114">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="0d144-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="0d144-115">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="0d144-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="0d144-116">V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky.</span><span class="sxs-lookup"><span data-stu-id="0d144-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="0d144-117">Obecně platí `<OnLoad>` přijímá pouze jeden animace.</span><span class="sxs-lookup"><span data-stu-id="0d144-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="0d144-118">Rozhraní framework animace umožňuje připojení k několika animace do jednoho pomocí `<Sequence>` elementu.</span><span class="sxs-lookup"><span data-stu-id="0d144-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="0d144-119">Všechny animace v rámci `<Sequence>` jsou prováděny jedna po druhé.</span><span class="sxs-lookup"><span data-stu-id="0d144-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="0d144-120">Tady je je možné kód pro `AnimationExtender` řízení, nejprve Příprava širší panelu a pak snížení jeho výšku:</span><span class="sxs-lookup"><span data-stu-id="0d144-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="0d144-121">Při spuštění tohoto skriptu panelu první získá širší a pak menší.</span><span class="sxs-lookup"><span data-stu-id="0d144-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="0d144-122">[![Nejprve je vyšší šířku](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0d144-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="0d144-123">Nejprve je vyšší šířku ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0d144-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="0d144-124">[![Pak je zmenšit výšku](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0d144-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="0d144-125">Pak je zmenšit výšku ([Kliknutím zobrazit obrázek v plné velikosti](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0d144-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0d144-126">[Předchozí](executing-several-animations-at-the-same-time-cs.md)
> [další](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0d144-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
