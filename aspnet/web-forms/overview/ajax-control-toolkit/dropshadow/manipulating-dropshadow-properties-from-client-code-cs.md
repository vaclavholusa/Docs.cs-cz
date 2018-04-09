---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulace s DropShadow vlastnosti z kódu klienta (C#) | Microsoft Docs
author: wenz
description: Přizpůsobení DataList úpravy rozhraní
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="0353c-103">Manipulace s DropShadow vlastnosti z kódu klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="0353c-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="0353c-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0353c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0353c-105">[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0353c-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="0353c-106">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="0353c-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="0353c-107">Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="0353c-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="0353c-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="0353c-108">Overview</span></span>

<span data-ttu-id="0353c-109">DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu.</span><span class="sxs-lookup"><span data-stu-id="0353c-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="0353c-110">Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="0353c-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="0353c-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="0353c-111">Steps</span></span>

<span data-ttu-id="0353c-112">Kód začíná panelu obsahující některé řádky textu:</span><span class="sxs-lookup"><span data-stu-id="0353c-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="0353c-113">Třída CSS, která přidružené dává panelu barvu pozadí dobrý:</span><span class="sxs-lookup"><span data-stu-id="0353c-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="0353c-114">`DropShadowExtender` Se přidá do rozšířit panel s efekt stínu rozevírací krytí nastavena na 50 %:</span><span class="sxs-lookup"><span data-stu-id="0353c-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="0353c-115">Potom prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:</span><span class="sxs-lookup"><span data-stu-id="0353c-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="0353c-116">Jiný panel obsahuje dva odkazy JavaScript pro nastavení krytí stín: odkaz znaménkem minus snižuje krytí bude stín, plus odkaz se zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="0353c-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="0353c-117">Funkce JavaScript, která `changeOpacity()` musí nejprve vyhledejte `DropShadowExtender` ovládací prvek na stránce.</span><span class="sxs-lookup"><span data-stu-id="0353c-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="0353c-118">Definuje prvku ASP.NET AJAX `$find()` metoda pro přesně tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="0353c-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="0353c-119">Potom, `get_Opacity()` metoda načte aktuální krytí `set_Opacity()` metoda ji nastaví.</span><span class="sxs-lookup"><span data-stu-id="0353c-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="0353c-120">Kód jazyka JavaScript pak vloží aktuální hodnota neprůhlednosti `<label>` element:</span><span class="sxs-lookup"><span data-stu-id="0353c-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="0353c-121">[![Krytí se změní na straně klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0353c-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="0353c-122">Krytí se změní na straně klienta ([Kliknutím zobrazit obrázek v plné velikosti](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0353c-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0353c-123">[Předchozí](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [další](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0353c-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
