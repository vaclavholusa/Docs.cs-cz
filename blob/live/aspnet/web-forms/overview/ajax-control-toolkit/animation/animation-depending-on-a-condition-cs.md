---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: "Animace v závislosti na podmínce (C#) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Zda je animace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="ff6e0-104">Animace v závislosti na podmínce (C#)</span><span class="sxs-lookup"><span data-stu-id="ff6e0-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="ff6e0-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ff6e0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ff6e0-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ff6e0-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="ff6e0-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff6e0-108">Zda je animace spustit nebo Ne můžete také závisí na podmínce v podobě určitý kód JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="ff6e0-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="ff6e0-109">Overview</span></span>

<span data-ttu-id="ff6e0-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff6e0-111">Zda je animace spustit nebo Ne můžete také závisí na podmínce v podobě určitý kód JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ff6e0-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="ff6e0-112">Steps</span></span>

<span data-ttu-id="ff6e0-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="ff6e0-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="ff6e0-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ff6e0-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="ff6e0-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="ff6e0-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="ff6e0-116">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj`runat="server":`</span><span class="sxs-lookup"><span data-stu-id="ff6e0-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="ff6e0-117">V rámci `<Animations>` uzlu, použijte `<OnLoad>` ke spuštění animací, jakmile byla úplným načtením stránky.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="ff6e0-118">Místo mezi regulární animace `<Condition>` element se stává play.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="ff6e0-119">Kód jazyka JavaScript, který je zadaný jako hodnota `ConditionScript` atribut je provést v době běhu.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="ff6e0-120">Pokud se vyhodnotí jako true, animace se spustí, jinak není.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="ff6e0-121">Následující kód obsahuje dvě animací, každý z nich vykonáván v případech při náhodných 50 %.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="ff6e0-122">Vzhledem k tomu, že může existovat pouze jedna animace v rámci `<OnLoad>`, dva `<Condition>` animací připojeni pomocí `<Sequence>` element:</span><span class="sxs-lookup"><span data-stu-id="ff6e0-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="ff6e0-123">Všimněte si, že menší než přihlašovací (`<`) v `ConditionScript` atribut musí být uvozovacími znaky ().</span><span class="sxs-lookup"><span data-stu-id="ff6e0-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="ff6e0-124">Při spuštění tohoto skriptu, buď žádná animace spuštění, nebo jednu ze dvou nemá nebo obě provést.</span><span class="sxs-lookup"><span data-stu-id="ff6e0-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="ff6e0-125">[![Na panelu je pozvolného vysouvání bez změny velikosti, tak druhý animace spustí první z nich nebyl](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ff6e0-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="ff6e0-126">Na panelu je pozvolného vysouvání bez změny velikosti, tak druhý animace spustí první z nich nebyl ([Kliknutím zobrazit obrázek v plné velikosti](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ff6e0-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ff6e0-127">[Předchozí](executing-several-animations-after-each-other-cs.md)
[další](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ff6e0-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
