---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Úpravy animací na straně serveru (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace může také...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: d39a40f2776d5b87bf82d1a6c6282ce920f4a907
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401951"
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="6e4d4-104">Úpravy animací na straně serveru (VB)</span><span class="sxs-lookup"><span data-stu-id="6e4d4-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="6e4d4-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6e4d4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6e4d4-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6e4d4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="6e4d4-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="6e4d4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6e4d4-108">Animace nejspíš se změní taky na straně serveru</span><span class="sxs-lookup"><span data-stu-id="6e4d4-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="6e4d4-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="6e4d4-109">Overview</span></span>

<span data-ttu-id="6e4d4-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="6e4d4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6e4d4-111">Animace nejspíš se změní taky na straně serveru</span><span class="sxs-lookup"><span data-stu-id="6e4d4-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="6e4d4-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="6e4d4-112">Steps</span></span>

<span data-ttu-id="6e4d4-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="6e4d4-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="6e4d4-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6e4d4-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="6e4d4-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="6e4d4-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="6e4d4-116">Zbytek kódu běží na straně serveru a nepoužívá značky Místo toho používá kód k vytvoření `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="6e4d4-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="6e4d4-117">Ale Control Toolkit aktuálně neposkytuje přístup rozhraní API k vytvoření jednotlivých animace.</span><span class="sxs-lookup"><span data-stu-id="6e4d4-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="6e4d4-118">Je však možné nastavit `AnimationExtender`animace vlastností na řetězec obsahující kód XML, použít při přiřazování animací deklarativně.</span><span class="sxs-lookup"><span data-stu-id="6e4d4-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="6e4d4-119">Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít XML rozhraní .NET Framework podporují, nebo jako v následujícím kódu, stačí zadat řetězec:</span><span class="sxs-lookup"><span data-stu-id="6e4d4-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="6e4d4-120">Nakonec přidejte `AnimationExtender` ovládací prvek na aktuální stránku v rámci `<form runat="server">` elementu, ujistěte se, že animaci je součástí a spustí:</span><span class="sxs-lookup"><span data-stu-id="6e4d4-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="6e4d4-121">[![Animace se vytvoří pomocí kódu na straně serveru C# /VB](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e4d4-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="6e4d4-122">Animace se vytvoří pomocí kódu na straně serveru C# /VB ([kliknutím ji zobrazíte obrázek v plné velikosti](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6e4d4-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6e4d4-123">[Předchozí](triggering-an-animation-in-another-control-vb.md)
> [další](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6e4d4-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
