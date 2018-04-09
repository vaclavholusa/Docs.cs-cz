---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Umístění ModalPopup (VB) | Microsoft Docs
author: wenz
description: ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale ovládací prvek nenabízí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="078e8-104">Umístění ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="078e8-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="078e8-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="078e8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="078e8-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="078e8-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="078e8-107">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="078e8-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="078e8-108">Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="078e8-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="078e8-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="078e8-109">Overview</span></span>

<span data-ttu-id="078e8-110">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="078e8-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="078e8-111">Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="078e8-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="078e8-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="078e8-112">Steps</span></span>

<span data-ttu-id="078e8-113">Chcete-li aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="078e8-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="078e8-114">ovládací prvek musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="078e8-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="078e8-115">Dál přidejte panel, který slouží jako modální místní.</span><span class="sxs-lookup"><span data-stu-id="078e8-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="078e8-116">Tlačítko slouží k zavření automaticky otevřeném okně:</span><span class="sxs-lookup"><span data-stu-id="078e8-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="078e8-117">Vždy, když se zobrazí automaticky otevřeném okně, musí být umístěna na určité místo na stránce.</span><span class="sxs-lookup"><span data-stu-id="078e8-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="078e8-118">Pro tuto úlohu je vytvořen funkce jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="078e8-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="078e8-119">Nejprve se pokusí získat přístup k panelu.</span><span class="sxs-lookup"><span data-stu-id="078e8-119">It first tries to access the panel.</span></span> <span data-ttu-id="078e8-120">Pokud se aktivace podaří, pozice panelu je nastavena pomocí šablon stylů CSS a JavaScript (změny, které bude pozici v místní nabídce).</span><span class="sxs-lookup"><span data-stu-id="078e8-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="078e8-121">Ale `ModalPopupExtender` ovládací prvek také pokusí umístit automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="078e8-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="078e8-122">Kód jazyka JavaScript proto umisťuje opakovaně místní, každou desetinu sekundy.</span><span class="sxs-lookup"><span data-stu-id="078e8-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="078e8-123">Jak můžete vidět, vrátí hodnotu, která `setTimeout()` metodu JavaScript je uložen v globální proměnné.</span><span class="sxs-lookup"><span data-stu-id="078e8-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="078e8-124">To umožňuje zastavit opakovaných umístění automaticky otevřeném okně na vyžádání, pomocí `clearTimeout()` metoda:</span><span class="sxs-lookup"><span data-stu-id="078e8-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="078e8-125">Nyní již zbývá udělat je ověřte prohlížeč volat tyto funkce, kdykoli je to vhodné.</span><span class="sxs-lookup"><span data-stu-id="078e8-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="078e8-126">`movePanel()` Funkce JavaScript, která musí být volána, když po kliknutí na tlačítko, která spustí panelu:</span><span class="sxs-lookup"><span data-stu-id="078e8-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="078e8-127">A `stopMoving()` funkce stává play při zavření automaticky otevřeném okně. to může být aktivována v `ModalPopupExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="078e8-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="078e8-128">[![Modální automaticky otevřeném okně se zobrazí na určené pozici](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="078e8-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="078e8-129">Modální automaticky otevřeném okně se zobrazí na určené pozici ([Kliknutím zobrazit obrázek v plné velikosti](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="078e8-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="078e8-130">Předchozí</span><span class="sxs-lookup"><span data-stu-id="078e8-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
