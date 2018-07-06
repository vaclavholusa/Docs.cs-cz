---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Úpravy animací na straně serveru (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace může také...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 8954f025873ea553fde26c6e2330ce6e5be2b539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804051"
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="67ca2-104">Úpravy animací na straně serveru (C#)</span><span class="sxs-lookup"><span data-stu-id="67ca2-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="67ca2-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="67ca2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="67ca2-106">[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="67ca2-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="67ca2-107">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="67ca2-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="67ca2-108">Animace nejspíš se změní taky na straně serveru</span><span class="sxs-lookup"><span data-stu-id="67ca2-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="67ca2-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="67ca2-109">Overview</span></span>

<span data-ttu-id="67ca2-110">Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="67ca2-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="67ca2-111">Animace nejspíš se změní taky na straně serveru</span><span class="sxs-lookup"><span data-stu-id="67ca2-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="67ca2-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="67ca2-112">Steps</span></span>

<span data-ttu-id="67ca2-113">Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="67ca2-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="67ca2-114">Animace se použijí pro panel text, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="67ca2-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="67ca2-115">V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:</span><span class="sxs-lookup"><span data-stu-id="67ca2-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="67ca2-116">Zbytek kódu běží na straně serveru a nepoužívá značky Místo toho používá kód k vytvoření `AnimationExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="67ca2-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="67ca2-117">Ale Control Toolkit aktuálně neposkytuje přístup rozhraní API k vytvoření jednotlivých animace.</span><span class="sxs-lookup"><span data-stu-id="67ca2-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="67ca2-118">Je však možné nastavit `AnimationExtender`animace vlastností na řetězec obsahující kód XML, použít při přiřazování animací deklarativně.</span><span class="sxs-lookup"><span data-stu-id="67ca2-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="67ca2-119">Chcete-li vytvořit XML, který nesmí obsahovat `<Animations>` element můžete použít XML rozhraní .NET Framework podporují, nebo jako v následujícím kódu, stačí zadat řetězec:</span><span class="sxs-lookup"><span data-stu-id="67ca2-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="67ca2-120">Nakonec přidejte `AnimationExtender` ovládací prvek na aktuální stránku v rámci `<form runat="server">` elementu, ujistěte se, že animaci je součástí a spustí:</span><span class="sxs-lookup"><span data-stu-id="67ca2-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="67ca2-121">[![Animace se vytvoří pomocí kódu na straně serveru C# /VB](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="67ca2-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="67ca2-122">Animace se vytvoří pomocí kódu na straně serveru C# /VB ([kliknutím ji zobrazíte obrázek v plné velikosti](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="67ca2-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="67ca2-123">[Předchozí](triggering-an-animation-in-another-control-cs.md)
> [další](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="67ca2-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
