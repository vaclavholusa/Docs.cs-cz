---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Vytváří číselnou nahoru/dolů ovládacího prvku s back-end webové služby (VB) | Dokumentace Microsoftu
author: wenz
description: Místo uživatele zadejte hodnotu do zaškrtávací políčko, by mohly být číselné směrem nahoru nebo dolů ovládací prvek (který existuje ve Windows a jiné operační systémy) jako další c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e63c76a69002e7cc66f7a8a1bc28e36796eb42c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375483"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="4a1bc-103">Vytváření Numeric Up/Down ovládacího prvku s back-end webové služby (VB)</span><span class="sxs-lookup"><span data-stu-id="4a1bc-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="4a1bc-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4a1bc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4a1bc-105">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4a1bc-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="4a1bc-106">Místo uživatele zadejte hodnotu do zaškrtávací políčko, by mohly být číselné nahoru/dolů ovládací prvek (který existuje ve Windows a jiné operační systémy) jako další příjemné.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="4a1bc-107">Ve výchozím nastavení ovládací prvek NumericUpDown vždy zvýší nebo sníží hodnotu o 1, ale webová služba ukáže větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="4a1bc-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="4a1bc-108">Overview</span></span>

<span data-ttu-id="4a1bc-109">Místo uživatele zadejte hodnotu do zaškrtávací políčko, by mohly být číselné nahoru/dolů ovládací prvek (který existuje ve Windows a jiné operační systémy) jako další příjemné.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="4a1bc-110">Ve výchozím nastavení `NumericUpDown` ovládací prvek vždy zvýší nebo sníží hodnotu o 1, ale webová služba ukáže větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="4a1bc-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="4a1bc-111">Steps</span></span>

<span data-ttu-id="4a1bc-112">ASP.NET AJAX Control Toolkit obsahuje `NumericUpDown` rozšiřujícího objektu, která automaticky přidá dvě tlačítka do textového pole: jeden pro zvýšení jeho hodnoty, jeden pro snížení ho.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="4a1bc-113">Ovládací prvek ale také podporuje volání webové služby (nebo volání metody stránky).</span><span class="sxs-lookup"><span data-stu-id="4a1bc-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="4a1bc-114">Pokaždé, když směrem nahoru nebo dolů tlačítko dojde ke kliknutí na, JavaScript, kód se připojí k webovému serveru a provede metodu existuje.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="4a1bc-115">Podpis metody je následující:</span><span class="sxs-lookup"><span data-stu-id="4a1bc-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="4a1bc-116">`current` Argument je aktuální hodnota v textovém poli; `tag` atribut je další kontext dat, která můžete nastavit jako vlastnost `NumericUpDown` zařízení extender (ale není potřeba).</span><span class="sxs-lookup"><span data-stu-id="4a1bc-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="4a1bc-117">V tomto příkladu se číselné nahoru/dolů ovládací prvek povolit pouze hodnoty, které jsou pro mocniny dvou: 1, 2, 4, 8, 16, 32, 64 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="4a1bc-118">Proto musí metoda spouštěny, když uživatel chce zvýšit hodnotu double stará hodnota; jiné metody musíte dělit hodnotu dvou.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="4a1bc-119">Tady je úplný webové služby:</span><span class="sxs-lookup"><span data-stu-id="4a1bc-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="4a1bc-120">Nakonec vytvořte novou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="4a1bc-121">Obvyklým způsobem, je nutné `ScriptManager` ovládací prvek, `TextBox` ovládacího prvku a `NumericUpDownExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="4a1bc-122">K tomu je nutné zadat informace o webových službách:</span><span class="sxs-lookup"><span data-stu-id="4a1bc-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="4a1bc-123">`ServiceDownMethod` název v seznamu webovou metodu nebo stránky – metoda</span><span class="sxs-lookup"><span data-stu-id="4a1bc-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="4a1bc-124">`ServiceDownPath` Cesta k webové službě s dolů metodu služby; vynechat, pokud používáte metodu stránky</span><span class="sxs-lookup"><span data-stu-id="4a1bc-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="4a1bc-125">`ServiceUpMethod` Název nahoru webovou metodu nebo stránky – metoda</span><span class="sxs-lookup"><span data-stu-id="4a1bc-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="4a1bc-126">`ServiceUpPath` Cesta k webové službě s aktuálním metodu služby; vynechat, pokud používáte metodu stránky</span><span class="sxs-lookup"><span data-stu-id="4a1bc-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="4a1bc-127">Tady je kompletní kód pro stránky:</span><span class="sxs-lookup"><span data-stu-id="4a1bc-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="4a1bc-128">Při spuštění stránky, Všimněte si, jak se hodnota v textovém poli vždy zdvojnásobí, klikněte na tlačítko nahoře, a je zkrátili na polovinu po kliknutí na tlačítko nižší.</span><span class="sxs-lookup"><span data-stu-id="4a1bc-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="4a1bc-129">[![Zobrazí jenom čísla, které jsou mocninou čísla 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a1bc-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="4a1bc-130">Zobrazí jenom čísla, které jsou mocninou čísla 2 ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4a1bc-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4a1bc-131">Předchozí</span><span class="sxs-lookup"><span data-stu-id="4a1bc-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
