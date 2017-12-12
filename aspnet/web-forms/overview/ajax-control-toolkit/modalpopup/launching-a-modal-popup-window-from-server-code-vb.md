---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "Spuštění modální automaticky otevíraném okně z kódu serveru (VB) | Microsoft Docs"
author: wenz
description: "ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale některé scénáře vyžadují tento t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="a4d50-104">Spuštění modální automaticky otevíraném okně z kódu serveru (VB)</span><span class="sxs-lookup"><span data-stu-id="a4d50-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="a4d50-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a4d50-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a4d50-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a4d50-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="a4d50-107">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="a4d50-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="a4d50-108">Ale některé scénáře vyžadují, aby otevření Modální překryvné okno se aktivuje na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="a4d50-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="a4d50-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="a4d50-109">Overview</span></span>

<span data-ttu-id="a4d50-110">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="a4d50-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="a4d50-111">Ale některé scénáře vyžadují, aby otevření Modální překryvné okno se aktivuje na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="a4d50-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="a4d50-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="a4d50-112">Steps</span></span>

<span data-ttu-id="a4d50-113">První řadě prvku tlačítko ASP.NET je potřeba ukazují, jak funguje řízení ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="a4d50-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="a4d50-114">Přidání tlačítka v rámci &lt;formuláře&gt; element na novou stránku:</span><span class="sxs-lookup"><span data-stu-id="a4d50-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="a4d50-115">Pak musíte kód pro místní, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a4d50-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="a4d50-116">Definovat jako `<asp:Panel>` řízení a ujistěte se, že zahrnuje ovládací prvek tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4d50-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="a4d50-117">Ovládací prvek ModalPopup nabízí funkci, aby toto tlačítko Zavřít místní; v opačném případě neexistuje žádný snadný způsob, jak jej zmizí.</span><span class="sxs-lookup"><span data-stu-id="a4d50-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="a4d50-118">Přidáte další ovládací prvek ModalPopup z ASP.NET AJAX Toolkit na stránku.</span><span class="sxs-lookup"><span data-stu-id="a4d50-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="a4d50-119">Nastavit vlastnosti pro tlačítka, který načte ovládacího prvku tlačítko, takže je zmizí a ID skutečného místní nabídky.</span><span class="sxs-lookup"><span data-stu-id="a4d50-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="a4d50-120">Stejně jako u všech webových stránek, které jsou založené na ASP.NET AJAX; Správce skriptu je potřeba načíst potřebné knihovny JavaScript pro jiný cíl prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="a4d50-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="a4d50-121">Spouštění v příkladu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a4d50-121">Run the example in the browser.</span></span> <span data-ttu-id="a4d50-122">Po kliknutí na tlačítko se zobrazí Modální místní okno.</span><span class="sxs-lookup"><span data-stu-id="a4d50-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="a4d50-123">K dosažení stejného efektu pomocí kódu na straně serveru, je nutné zadat nové tlačítko:</span><span class="sxs-lookup"><span data-stu-id="a4d50-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="a4d50-124">Jak můžete vidět, a klikněte na tlačítko generuje zpětné volání a provede `ServerButton_Click()` metoda na serveru.</span><span class="sxs-lookup"><span data-stu-id="a4d50-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="a4d50-125">Tato metoda volá funkci jazyka JavaScript `launchModal()` je provést, aby byl přesný, bude proveden funkce JavaScript, která po načtení stránky:</span><span class="sxs-lookup"><span data-stu-id="a4d50-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="a4d50-126">Úkolem `launchModal()` je zobrazit ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="a4d50-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="a4d50-127">`launchModal()` Funkce se spustí po dokončení stránky HTML se načetl.</span><span class="sxs-lookup"><span data-stu-id="a4d50-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="a4d50-128">V tuto chvíli však rozhraní ASP.NET AJAX nebyl úplným načtením ještě.</span><span class="sxs-lookup"><span data-stu-id="a4d50-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="a4d50-129">Proto `launchModal()` funkce právě nastaví proměnné, která ovládacího prvku ModalPopup musí zobrazit později na:</span><span class="sxs-lookup"><span data-stu-id="a4d50-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="a4d50-130">`pageLoad()` Funkce JavaScript, která je speciální funkce, která provede se jednou prvku ASP.NET AJAX byla plně načtena.</span><span class="sxs-lookup"><span data-stu-id="a4d50-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="a4d50-131">Proto přidáme kód k této funkci můžete zobrazit ModalPopup řízení, ale jenom v případě `launchModal()` bylo voláno před:</span><span class="sxs-lookup"><span data-stu-id="a4d50-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="a4d50-132">`$find()` Hledá pojmenovaného elementu na stránce funkce a očekává ID na straně serveru jako parametr.</span><span class="sxs-lookup"><span data-stu-id="a4d50-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="a4d50-133">Proto `$find("mpe")` vrátí reprezentaci klienta ovládacího prvku ModalPopup; jeho `show()` metoda umožňuje automaticky otevřeném okně se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a4d50-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="a4d50-134">[![Modální automaticky otevřeném okně se zobrazí, když po kliknutí na buď tlačítek](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a4d50-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="a4d50-135">Modální automaticky otevřeném okně se zobrazí, když po kliknutí na buď tlačítek ([Kliknutím zobrazit obrázek v plné velikosti](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a4d50-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a4d50-136">[Předchozí](positioning-a-modalpopup-cs.md)
[další](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a4d50-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
