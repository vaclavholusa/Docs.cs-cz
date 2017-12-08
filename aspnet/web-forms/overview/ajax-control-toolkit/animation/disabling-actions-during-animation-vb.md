---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: "Zakázání akcí během animace (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Také podporuje akce..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 0f68d27f60e9b224a7cc0d598962553afeb3f28f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="3ec3a-104">Zakázání akcí během animace (VB)</span><span class="sxs-lookup"><span data-stu-id="3ec3a-104">Disabling Actions during Animation (VB)</span></span>
====================
<span data-ttu-id="3ec3a-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3ec3a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3ec3a-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3ec3a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="3ec3a-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3ec3a-108">Podporuje také akce, jako jsou kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="3ec3a-109">Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="3ec3a-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="3ec3a-110">Overview</span></span>

<span data-ttu-id="3ec3a-111">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3ec3a-112">Podporuje také akce, jako jsou kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="3ec3a-113">Ale při kliknutí myší spuštění animace, je třeba zakázat kliknutí myší během animace.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="3ec3a-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="3ec3a-114">Steps</span></span>

<span data-ttu-id="3ec3a-115">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="3ec3a-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="3ec3a-116">Animace se použijí k HTML tlačítko takto:</span><span class="sxs-lookup"><span data-stu-id="3ec3a-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="3ec3a-117">Upozorňujeme, že ovládací prvek jazyka HTML je místo ovládací prvek webu vzhledem k tomu, že jsme nechcete, aby tlačítko pro vytvoření zpětné volání; právě zahájí animace straně klienta pro nás.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="3ec3a-118">Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="3ec3a-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="3ec3a-119">V rámci `<Animations>` uzlu `<OnClick>` je element správné zpracování kliknutí myší.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="3ec3a-120">Však může být stisknuto během animace také.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="3ec3a-121">`<EnableAction>` Element můžou starat o který.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="3ec3a-122">Nastavení `Enabled="false"` zakáže tlačítko jako součást animace.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="3ec3a-123">Vzhledem k tomu, že se používá několik jednotlivých animace (zakázání tlačítko a skutečný animace), `<Parallel>` element je potřeba dohledání jeden animací společně do jednoho.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="3ec3a-124">Tady je kompletní kód pro `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="3ec3a-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="3ec3a-125">Také je možné znovu zapnout. pro tlačítko po animace, pomocí následujícího elementu XML na konci tohoto seznamu:</span><span class="sxs-lookup"><span data-stu-id="3ec3a-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="3ec3a-126">Ale v této situaci by to byl nemá od tlačítko setmívá a není viditelný na konci animace.</span><span class="sxs-lookup"><span data-stu-id="3ec3a-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="3ec3a-127">[![Tlačítko vypnutá při spuštění animace](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3ec3a-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="3ec3a-128">Tlačítko vypnutá při spuštění animace ([Kliknutím zobrazit obrázek v plné velikosti](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3ec3a-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3ec3a-129">[Předchozí](animating-in-response-to-user-interaction-vb.md)
[další](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3ec3a-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
