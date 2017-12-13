---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: "Umístění ModalPopup (C#) | Microsoft Docs"
author: wenz
description: "ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale ovládací prvek nenabízí..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8dcc4e20ac98cbbad1ea3e86b7f895d32c853d4a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="d7758-104">Umístění ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="d7758-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="d7758-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d7758-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d7758-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d7758-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="d7758-107">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="d7758-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d7758-108">Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="d7758-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="d7758-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="d7758-109">Overview</span></span>

<span data-ttu-id="d7758-110">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="d7758-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d7758-111">Ovládací prvek ale nenabízí integrovanou funkci na pozici automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="d7758-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="d7758-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="d7758-112">Steps</span></span>

<span data-ttu-id="d7758-113">Chcete-li aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="d7758-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="d7758-114">ovládací prvek musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="d7758-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="d7758-115">Dál přidejte panel, který slouží jako modální místní.</span><span class="sxs-lookup"><span data-stu-id="d7758-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="d7758-116">Tlačítko slouží k zavření automaticky otevřeném okně:</span><span class="sxs-lookup"><span data-stu-id="d7758-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="d7758-117">Vždy, když se zobrazí automaticky otevřeném okně, musí být umístěna na určité místo na stránce.</span><span class="sxs-lookup"><span data-stu-id="d7758-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="d7758-118">Pro tuto úlohu je vytvořen funkce jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d7758-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="d7758-119">Nejprve se pokusí získat přístup k panelu.</span><span class="sxs-lookup"><span data-stu-id="d7758-119">It first tries to access the panel.</span></span> <span data-ttu-id="d7758-120">Pokud se aktivace podaří, pozice panelu je nastavena pomocí šablon stylů CSS a JavaScript (změny, které bude pozici v místní nabídce).</span><span class="sxs-lookup"><span data-stu-id="d7758-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="d7758-121">Ale `ModalPopupExtender` ovládací prvek také pokusí umístit automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="d7758-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="d7758-122">Kód jazyka JavaScript proto umisťuje opakovaně místní, každou desetinu sekundy.</span><span class="sxs-lookup"><span data-stu-id="d7758-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="d7758-123">Jak můžete vidět, vrátí hodnotu, která `setTimeout()` metodu JavaScript je uložen v globální proměnné.</span><span class="sxs-lookup"><span data-stu-id="d7758-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="d7758-124">To umožňuje zastavit opakovaných umístění automaticky otevřeném okně na vyžádání, pomocí `clearTimeout()` metoda:</span><span class="sxs-lookup"><span data-stu-id="d7758-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="d7758-125">Nyní již zbývá udělat je ověřte prohlížeč volat tyto funkce, kdykoli je to vhodné.</span><span class="sxs-lookup"><span data-stu-id="d7758-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="d7758-126">`movePanel()` Funkce JavaScript, která musí být volána, když po kliknutí na tlačítko, která spustí panelu:</span><span class="sxs-lookup"><span data-stu-id="d7758-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="d7758-127">A `stopMoving()` funkce stává play při zavření automaticky otevřeném okně. to může být aktivována v `ModalPopupExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="d7758-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="d7758-128">[![Modální automaticky otevřeném okně se zobrazí na určené pozici](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d7758-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="d7758-129">Modální automaticky otevřeném okně se zobrazí na určené pozici ([Kliknutím zobrazit obrázek v plné velikosti](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d7758-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d7758-130">[Předchozí](handling-postbacks-from-a-modalpopup-cs.md)
[další](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d7758-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
