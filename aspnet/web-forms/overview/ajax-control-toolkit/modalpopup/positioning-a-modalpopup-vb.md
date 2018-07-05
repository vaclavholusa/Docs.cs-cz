---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Umístění ovládacího prvku ModalPopup (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Ale ovládací prvek nenabízí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1578a1bd6d5e3b595eba526552b8723daefb2ba1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387829"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="e3d70-104">Umístění ovládacího prvku ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="e3d70-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="e3d70-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e3d70-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e3d70-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e3d70-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="e3d70-107">Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e3d70-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e3d70-108">Ovládací prvek ale nenabízí vestavěnou funkci na pozici automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="e3d70-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="e3d70-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="e3d70-109">Overview</span></span>

<span data-ttu-id="e3d70-110">Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e3d70-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e3d70-111">Ovládací prvek ale nenabízí vestavěnou funkci na pozici automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="e3d70-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="e3d70-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="e3d70-112">Steps</span></span>

<span data-ttu-id="e3d70-113">K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="e3d70-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="e3d70-114">ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="e3d70-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="e3d70-115">V dalším kroku přidáte panel, který slouží jako modální místní nabídky.</span><span class="sxs-lookup"><span data-stu-id="e3d70-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="e3d70-116">Tlačítko slouží k zavření automaticky otevírané okno:</span><span class="sxs-lookup"><span data-stu-id="e3d70-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="e3d70-117">Pokaždé, když se zobrazí automaticky otevírané okno, musí být umístěn na určité místo na stránce.</span><span class="sxs-lookup"><span data-stu-id="e3d70-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="e3d70-118">Pro tuto úlohu se vytvoří funkce jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e3d70-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="e3d70-119">Nejprve se pokusí přistupovat k panelu.</span><span class="sxs-lookup"><span data-stu-id="e3d70-119">It first tries to access the panel.</span></span> <span data-ttu-id="e3d70-120">Pokud se aktivace podaří, pozici panelu se nastavuje pomocí šablon stylů CSS a JavaScriptu (změnit pozici automaticky otevíraného okna na budou).</span><span class="sxs-lookup"><span data-stu-id="e3d70-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="e3d70-121">Ale `ModalPopupExtender` ovládací prvek se rovněž snaží pozici automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="e3d70-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="e3d70-122">Proto kód jazyka JavaScript opakovaně umístí automaticky otevírané okno, každou desetinu sekundy.</span><span class="sxs-lookup"><span data-stu-id="e3d70-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="e3d70-123">Jak je vidět, návratová hodnota `setTimeout()` metodu JavaScript se uloží do globální proměnné.</span><span class="sxs-lookup"><span data-stu-id="e3d70-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="e3d70-124">Zastavit opakované umístění automaticky otevíraného okna na vyžádání, díky tomu pomocí `clearTimeout()` metody:</span><span class="sxs-lookup"><span data-stu-id="e3d70-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="e3d70-125">Nyní vše, co už zbývá jen se v prohlížeči volání těchto funkcí, kdykoli je to vhodné.</span><span class="sxs-lookup"><span data-stu-id="e3d70-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="e3d70-126">`movePanel()` Funkce JavaScript, která musí být volána, když po kliknutí na tlačítko, která aktivuje panelu:</span><span class="sxs-lookup"><span data-stu-id="e3d70-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="e3d70-127">A `stopMoving()` funkce vstupu do play při zavření to může automaticky otevírané okno se aktivuje v `ModalPopupExtender` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="e3d70-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="e3d70-128">[![Modální místní nabídky se zobrazí na určené pozici](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e3d70-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="e3d70-129">Modální místní nabídky se zobrazí na určené pozici ([kliknutím ji zobrazíte obrázek v plné velikosti](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e3d70-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e3d70-130">Předchozí</span><span class="sxs-lookup"><span data-stu-id="e3d70-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
