---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Zpracování postback z ModalPopup (VB) | Microsoft Docs
author: wenz
description: ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Zvláštní pozornost musí být provedeny, když pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874207"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="93a81-104">Zpracování postback z ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="93a81-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="93a81-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="93a81-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="93a81-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="93a81-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="93a81-107">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="93a81-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="93a81-108">Musí dát zvláštní pozor při zpětné volání z v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="93a81-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="93a81-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="93a81-109">Overview</span></span>

<span data-ttu-id="93a81-110">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="93a81-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="93a81-111">Musí dát zvláštní pozor při zpětné volání z v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="93a81-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="93a81-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="93a81-112">Steps</span></span>

<span data-ttu-id="93a81-113">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="93a81-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="93a81-114">Dál přidejte panel, který slouží jako modální místní.</span><span class="sxs-lookup"><span data-stu-id="93a81-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="93a81-115">Existuje může uživatel zadat název a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="93a81-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="93a81-116">Tlačítko se používá automaticky otevřeném okně zavřete a uložte informace.</span><span class="sxs-lookup"><span data-stu-id="93a81-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="93a81-117">Všimněte si, že `OnClick` atribut je nastaven tak, aby zpětné volání nastane, když po kliknutí na toto tlačítko:</span><span class="sxs-lookup"><span data-stu-id="93a81-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="93a81-118">Vlastní stránky se skládá ze dvou popisky přesně stejné informace: jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="93a81-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="93a81-119">Tlačítko se používá k aktivaci Modální překryvné okno:</span><span class="sxs-lookup"><span data-stu-id="93a81-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="93a81-120">Aby bylo možné automaticky otevřeném okně se zobrazí, přidejte `ModalPopupExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="93a81-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="93a81-121">Nastavte `PopupControlID` atribut ID panelu a `TargetControlID` ID tlačítka:</span><span class="sxs-lookup"><span data-stu-id="93a81-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="93a81-122">Nyní vždy, když `Save` v překryvném okně modální po kliknutí na tlačítko, na straně serveru `SaveData()` metoda spuštěna.</span><span class="sxs-lookup"><span data-stu-id="93a81-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="93a81-123">Zde můžete uložit zadané údaje v úložišti.</span><span class="sxs-lookup"><span data-stu-id="93a81-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="93a81-124">Z důvodu zjednodušení je nová data právě výstup v popisku:</span><span class="sxs-lookup"><span data-stu-id="93a81-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="93a81-125">Ovládacích prvcích textbox v překryvném okně modální by měl být navíc vyplněn aktuální název a e-mailu.</span><span class="sxs-lookup"><span data-stu-id="93a81-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="93a81-126">Ale to je nutné pouze v případě žádný postback.</span><span class="sxs-lookup"><span data-stu-id="93a81-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="93a81-127">Pokud je zpětné volání, funkci ASP.NET viewstate automaticky vyplní textových polí s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="93a81-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="93a81-128">[![Modální místní způsobí, že zpětné volání](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="93a81-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="93a81-129">Modální místní způsobí postback ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="93a81-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="93a81-130">[Předchozí](using-modalpopup-with-a-repeater-control-vb.md)
> [další](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="93a81-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
