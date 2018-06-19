---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Dynamicky řízení UpdatePanel animace (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Pro obsah...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871542"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="72138-104">Dynamicky řízení UpdatePanel animace (VB)</span><span class="sxs-lookup"><span data-stu-id="72138-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="72138-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="72138-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="72138-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="72138-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="72138-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="72138-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="72138-108">Pro obsah ovládacího prvku UpdatePanel speciální rozšiřujícího objektu existuje, která je založena na rozhraní animace: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="72138-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="72138-109">Může spolupracovat taky společně s UpdatePanel aktivační události.</span><span class="sxs-lookup"><span data-stu-id="72138-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="72138-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="72138-110">Overview</span></span>

<span data-ttu-id="72138-111">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="72138-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="72138-112">Pro obsah `UpdatePanel`, existuje speciální rozšiřujícího objektu, který založena na rozhraní animace: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="72138-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="72138-113">Můžete také pracovat společně s `UpdatePanel` aktivační události.</span><span class="sxs-lookup"><span data-stu-id="72138-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="72138-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="72138-114">Steps</span></span>

<span data-ttu-id="72138-115">Prvním krokem je jako obvykle zahrnují `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="72138-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="72138-116">Animace v tomto scénáři se použijí k zobrazení aktuálního času.</span><span class="sxs-lookup"><span data-stu-id="72138-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="72138-117">Tyto informace je možné zapsat do popisek pomocí `Page_Load()` metoda, nebo (z důvodu zjednodušení) se používá následující vloženého kódu:</span><span class="sxs-lookup"><span data-stu-id="72138-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="72138-118">Také se vytvoří tlačítko pro aktivaci aktualizací času:</span><span class="sxs-lookup"><span data-stu-id="72138-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="72138-119">Tento kód je pak přesuňte do `<ContentTemplate>` části `UpdatePanel` elementu.</span><span class="sxs-lookup"><span data-stu-id="72138-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="72138-120">Na panelu `UpdateMode` musí být nastaven `"Conditional"`, protože pouze aktivační události může aktualizovat obsah panelu.</span><span class="sxs-lookup"><span data-stu-id="72138-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="72138-121">V `<Triggers>` části `UpdatePanel`, asynchronní postback aktivační událost se vytvoří a vázáno `Click` události tlačítka.</span><span class="sxs-lookup"><span data-stu-id="72138-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="72138-122">Proto v případě, že uživatel klikne na tlačítko, `UpdatePanel` je aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="72138-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="72138-123">Zde je kód pro `UpdatePanel` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="72138-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="72138-124">Nakonec `UpdatePanelAnimationExtender` musí být nakonfigurované: nastavte `TargetControlID` atribut ID panelu a definujte animace v rámci rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="72138-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="72138-125">Pozvolného vysouvání v díky smysl, který vytvoří dobrý visual důraz na čas poslední aktualizace.</span><span class="sxs-lookup"><span data-stu-id="72138-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="72138-126">Váš kód rozšiřujícího objektu může pak vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="72138-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="72138-127">Spusťte soubor v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="72138-127">Run the file in the browser.</span></span> <span data-ttu-id="72138-128">Vždy, když kliknete na tlačítko se zobrazí aktuální čas v panelech vždy pozvolného vysouvání po dobu trvání jednu sekundu.</span><span class="sxs-lookup"><span data-stu-id="72138-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="72138-129">[![Aktuální čas je pozvolného vysouvání](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="72138-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="72138-130">Aktuální čas je pozvolného vysouvání ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="72138-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="72138-131">Předchozí</span><span class="sxs-lookup"><span data-stu-id="72138-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
