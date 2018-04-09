---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Vytvoření číselný nahoru/dolů ovládacího prvku pomocí webové služby back-end (C#) | Microsoft Docs
author: wenz
description: Místo uživatele zadejte hodnotu do zaškrtávacího políčka, by mohly být číselný nahoru/dolů ovládací prvek (který existuje ve Windows a jinými operačními systémy) jako další c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 942902bdba93fe4fef8a9122403c6d5c62e6123c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="ca1e0-103">Vytvoření číselný nahoru/dolů ovládacího prvku pomocí webové služby back-end (C#)</span><span class="sxs-lookup"><span data-stu-id="ca1e0-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>
====================
<span data-ttu-id="ca1e0-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ca1e0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ca1e0-105">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ca1e0-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="ca1e0-106">Místo uživatele zadejte hodnotu do zaškrtávacího políčka, by mohly být číselný nahoru/dolů ovládací prvek (který existuje ve Windows a jinými operačními systémy) jako další možnost.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="ca1e0-107">Ve výchozím nastavení ovládacího prvku NumericUpDown vždy zvýší nebo sníží hodnotu 1, ale webová služba prokáže větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="ca1e0-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="ca1e0-108">Overview</span></span>

<span data-ttu-id="ca1e0-109">Místo uživatele zadejte hodnotu do zaškrtávacího políčka, by mohly být číselný nahoru/dolů ovládací prvek (který existuje ve Windows a jinými operačními systémy) jako další možnost.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="ca1e0-110">Ve výchozím nastavení `NumericUpDown` řízení vždy zvýší nebo sníží hodnotu 1, ale webová služba prokáže větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="ca1e0-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="ca1e0-111">Steps</span></span>

<span data-ttu-id="ca1e0-112">Obsahuje sadu ovládacího prvku ASP.NET AJAX `NumericUpDown` rozšiřujícího objektu, který automaticky přidá dvě tlačítka do textového pole: jeden pro zvýšení jeho hodnoty, jeden pro snížení ho.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="ca1e0-113">Ovládací prvek ale také podporuje volání webové služby (nebo volání metody stránky).</span><span class="sxs-lookup"><span data-stu-id="ca1e0-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="ca1e0-114">Vždy, když nahoru nebo dolů tlačítko po kliknutí na, JavaScript, kód se připojí k webovému serveru a provede metodu existuje.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="ca1e0-115">Podpis metody je následující:</span><span class="sxs-lookup"><span data-stu-id="ca1e0-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="ca1e0-116">`current` Argument je aktuální hodnota v textovém poli; `tag` atribut je další kontext data, která lze nastavit jako vlastnost `NumericUpDown` rozšiřujícího objektu (ale není povinen).</span><span class="sxs-lookup"><span data-stu-id="ca1e0-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="ca1e0-117">Tato ukázka číselný nahoru/dolů řízení pouze povolit hodnoty, které jsou zajišťuje dva: 1, 2, 4, 8, 16, 32, 64 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="ca1e0-118">Proto musí metoda spustit, když chce uživatel zvýšit hodnotu double původní hodnoty; jiné metody musí dělení hodnoty ve dvou.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="ca1e0-119">Tady je proto dokončení webové služby:</span><span class="sxs-lookup"><span data-stu-id="ca1e0-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="ca1e0-120">Nakonec vytvořte novou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="ca1e0-121">Obvyklým způsobem, budete potřebovat `ScriptManager` řízení, `TextBox` řízení a `NumericUpDownExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="ca1e0-122">K tomu je nutné zadat informace o webové služby:</span><span class="sxs-lookup"><span data-stu-id="ca1e0-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="ca1e0-123">`ServiceDownMethod` název na nabídku webové metody nebo stránky – metoda</span><span class="sxs-lookup"><span data-stu-id="ca1e0-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="ca1e0-124">`ServiceDownPath` Cesta k webové službě s dolů metody služby; vynechat, pokud používáte metodu stránky</span><span class="sxs-lookup"><span data-stu-id="ca1e0-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="ca1e0-125">`ServiceUpMethod` Název nahoru webové metody nebo stránky – metoda</span><span class="sxs-lookup"><span data-stu-id="ca1e0-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="ca1e0-126">`ServiceUpPath` Cesta k webové službě s aktuálním metody služby; vynechat, pokud používáte metodu stránky</span><span class="sxs-lookup"><span data-stu-id="ca1e0-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="ca1e0-127">Tady je kompletní kód pro stránky:</span><span class="sxs-lookup"><span data-stu-id="ca1e0-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="ca1e0-128">Pokud spustíte stránky, Všimněte si, jak se hodnota v textovém poli vždy zdvojnásobí, klikněte na tlačítko horní, a je oproti jiným poloviční po kliknutí na tlačítko nižší.</span><span class="sxs-lookup"><span data-stu-id="ca1e0-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="ca1e0-129">[![Zobrazí pouze čísla, které jsou power 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ca1e0-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span></span>

<span data-ttu-id="ca1e0-130">Zobrazí pouze čísla, které jsou power 2 ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ca1e0-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ca1e0-131">Next</span><span class="sxs-lookup"><span data-stu-id="ca1e0-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
