---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Úprava animace z na straně serveru (VB) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animací může také...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="77e38-104">Úprava animace z na straně serveru (VB)</span><span class="sxs-lookup"><span data-stu-id="77e38-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="77e38-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="77e38-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="77e38-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="77e38-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="77e38-107">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="77e38-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="77e38-108">Animací pravděpodobně se změní také na straně serveru</span><span class="sxs-lookup"><span data-stu-id="77e38-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="77e38-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="77e38-109">Overview</span></span>

<span data-ttu-id="77e38-110">V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace.</span><span class="sxs-lookup"><span data-stu-id="77e38-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="77e38-111">Animací pravděpodobně se změní také na straně serveru</span><span class="sxs-lookup"><span data-stu-id="77e38-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="77e38-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="77e38-112">Steps</span></span>

<span data-ttu-id="77e38-113">První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="77e38-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="77e38-114">Animace se použijí na panel textu, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="77e38-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="77e38-115">Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:</span><span class="sxs-lookup"><span data-stu-id="77e38-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="77e38-116">Zbytek kód běží na straně serveru a nepoužívá značek; Místo toho použije k vytvoření kódu `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="77e38-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="77e38-117">Ale Toolkitu aktuálně neposkytuje rozhraní API pro přístup k vytvoření jednotlivých animace.</span><span class="sxs-lookup"><span data-stu-id="77e38-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="77e38-118">Je však možné nastavit `AnimationExtender`vlastnost animací na řetězec obsahující kód XML používá při přiřazování animací deklarativně.</span><span class="sxs-lookup"><span data-stu-id="77e38-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="77e38-119">Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít rozhraní .NET Framework XML podporují, nebo jako v následujícím kódu, stačí zadat řetězec:</span><span class="sxs-lookup"><span data-stu-id="77e38-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="77e38-120">Nakonec přidejte `AnimationExtender` v řízení na aktuální stránku, `<form runat="server">` elementu, a ujistěte se, že animace je součástí a spustí:</span><span class="sxs-lookup"><span data-stu-id="77e38-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="77e38-121">[![Animace je vytvořený pomocí kódu na straně serveru C# / VB.](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="77e38-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="77e38-122">Animace je vytvořený pomocí kódu na straně serveru C# / VB. ([Kliknutím zobrazit obrázek v plné velikosti](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="77e38-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77e38-123">[Předchozí](triggering-an-animation-in-another-control-vb.md)
> [další](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="77e38-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
