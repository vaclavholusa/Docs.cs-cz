---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Spuštění okna Modální místní nabídky serverovým kódem (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Některé scénáře však vyžadují tento t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 52eec262a9241aa7b11cbb892bdf2142c7a2b83a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822765"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="353f8-104">Spuštění okna Modální místní nabídky serverovým kódem (VB)</span><span class="sxs-lookup"><span data-stu-id="353f8-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="353f8-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="353f8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="353f8-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="353f8-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="353f8-107">Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="353f8-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="353f8-108">Některé scénáře však vyžadují, že otevírání Modální místní nabídky se aktivuje na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="353f8-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="353f8-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="353f8-109">Overview</span></span>

<span data-ttu-id="353f8-110">Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="353f8-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="353f8-111">Některé scénáře však vyžadují, že otevírání Modální místní nabídky se aktivuje na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="353f8-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="353f8-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="353f8-112">Steps</span></span>

<span data-ttu-id="353f8-113">Webové ovládací prvek technologie ASP.NET je nejprve potřeba ukazují, jak funguje řízení ovládacího prvku ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="353f8-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="353f8-114">Přidejte tlačítko, v rámci &lt;formuláře&gt; prvek na nové stránce:</span><span class="sxs-lookup"><span data-stu-id="353f8-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="353f8-115">Potom budete potřebovat značky pro automaticky otevírané okno, které chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="353f8-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="353f8-116">Definovat jako `<asp:Panel>` řídit a ujistěte se, že obsahuje ovládací prvek tlačítko.</span><span class="sxs-lookup"><span data-stu-id="353f8-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="353f8-117">Ovládací prvek ovládacího prvku ModalPopup a nabízí funkce, aby automaticky otevírané okno; toto tlačítko Zavřít jinak neexistuje žádný snadný způsob, jak ho zmizí.</span><span class="sxs-lookup"><span data-stu-id="353f8-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="353f8-118">Dále přidejte ovládací prvek ovládacího prvku ModalPopup z technologie ASP.NET AJAX Toolkit na stránku.</span><span class="sxs-lookup"><span data-stu-id="353f8-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="353f8-119">Nastavit vlastnosti pro tlačítka, který načte ovládací prvek, tlačítko, které umožňuje zmizí a ID aplikace skutečný automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="353f8-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="353f8-120">Stejně jako u všech webových stránek, které jsou založené na technologii ASP.NET AJAX; Správce skriptů je nutná k načtení nezbytné knihovny jazyka JavaScript pro jiné cílové prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="353f8-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="353f8-121">Spusťte v příkladu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="353f8-121">Run the example in the browser.</span></span> <span data-ttu-id="353f8-122">Po kliknutí na tlačítko se zobrazí Modální místní nabídky.</span><span class="sxs-lookup"><span data-stu-id="353f8-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="353f8-123">Abyste dosáhli stejného účinku pomocí kódu na straně serveru, je potřeba nové tlačítko:</span><span class="sxs-lookup"><span data-stu-id="353f8-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="353f8-124">Jak je vidět, klikněte na tlačítko vytvoří zpětné volání a spustí `ServerButton_Click()` metoda na serveru.</span><span class="sxs-lookup"><span data-stu-id="353f8-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="353f8-125">Tato metoda volá funkci jazyka JavaScript `launchModal()` provádí být přesné, funkce JavaScript, která se spustí po načtení stránky:</span><span class="sxs-lookup"><span data-stu-id="353f8-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="353f8-126">Práce `launchModal()` je zobrazení ovládacího prvku ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="353f8-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="353f8-127">`launchModal()` Funkce se provede po dokončení stránky HTML se načetl.</span><span class="sxs-lookup"><span data-stu-id="353f8-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="353f8-128">V daném okamžiku však rozhraní ASP.NET AJAX framework ještě není zavedený plně.</span><span class="sxs-lookup"><span data-stu-id="353f8-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="353f8-129">Proto `launchModal()` funkce pouze nastaví proměnnou ovládacího prvku ModalPopup ovládacího prvku musí být uvedeny později:</span><span class="sxs-lookup"><span data-stu-id="353f8-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="353f8-130">`pageLoad()` Funkce JavaScript, která je speciální funkce, která se provede po technologie ASP.NET AJAX bylo plně načteno.</span><span class="sxs-lookup"><span data-stu-id="353f8-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="353f8-131">Proto jsme přidejte kód pro tuto funkci chcete-li zobrazit ovládací prvek ovládacího prvku ModalPopup, ale pouze v případě `launchModal()` byla volána před:</span><span class="sxs-lookup"><span data-stu-id="353f8-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="353f8-132">`$find()` Funkce hledá pojmenovaného elementu na stránce a očekává jako parametr ID na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="353f8-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="353f8-133">Proto `$find("mpe")` vrátí reprezentaci klienta ovládacího prvku ModalPopup ovládacího prvku; jeho `show()` metoda umožňuje automaticky otevírané okno se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="353f8-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="353f8-134">[![Modální místní nabídky se zobrazí po kliknutí na některý z tlačítek](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="353f8-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="353f8-135">Modální místní nabídky se zobrazí po kliknutí na některý z tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="353f8-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="353f8-136">[Předchozí](positioning-a-modalpopup-cs.md)
> [další](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="353f8-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
