---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Zpracování postback z ovládacího prvku místní okno s UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Věnovat zvláštní pozornost tomu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868474"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="5366e-104">Zpracování postback z ovládacího prvku místní okno s UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="5366e-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="5366e-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5366e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5366e-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5366e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="5366e-107">Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="5366e-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5366e-108">Zvláštní pozornost tomu mají být provedeny, když zpětné volání vyskytuje v takové místní.</span><span class="sxs-lookup"><span data-stu-id="5366e-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="5366e-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="5366e-109">Overview</span></span>

<span data-ttu-id="5366e-110">Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="5366e-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5366e-111">Zvláštní pozornost tomu mají být provedeny, když zpětné volání vyskytuje v takové místní.</span><span class="sxs-lookup"><span data-stu-id="5366e-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="5366e-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="5366e-112">Steps</span></span>

<span data-ttu-id="5366e-113">Při použití `PopupControl` s zpětné volání, `UpdatePanel` může zabránit aktualizaci stránky způsobené zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="5366e-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="5366e-114">Následující kód definuje několik důležité elementy:</span><span class="sxs-lookup"><span data-stu-id="5366e-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="5366e-115">A `ScriptManager` řídit tak, aby funguje Toolkitu ASP.NET AJAX</span><span class="sxs-lookup"><span data-stu-id="5366e-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="5366e-116">Dva `TextBox` ovládací prvky, které aktivují i místní okno</span><span class="sxs-lookup"><span data-stu-id="5366e-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="5366e-117">A `Panel` ovládací prvek, který bude sloužit jako automaticky otevřeném okně.</span><span class="sxs-lookup"><span data-stu-id="5366e-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="5366e-118">V panelu `Calendar` vložené ovládací prvek v rámci `UpdatePanel` ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="5366e-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="5366e-119">Dva `PopupControlExtender` ovládacích prvků, které přiřadit do textových polí panelu</span><span class="sxs-lookup"><span data-stu-id="5366e-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="5366e-120">Všimněte si, že `OnSelectionChanged` atribut `Calendar` řízení je nastavené.</span><span class="sxs-lookup"><span data-stu-id="5366e-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="5366e-121">Takže když uživatel vybere datum v kalendáři, dojde k zpětné volání a metody na straně serveru `c1_SelectionChanged()` se spustí.</span><span class="sxs-lookup"><span data-stu-id="5366e-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="5366e-122">V rámci této metody musí být aktuální datum načíst a zapisovat zpátky do textového pole.</span><span class="sxs-lookup"><span data-stu-id="5366e-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="5366e-123">Syntaxe pro který je následující: objekt nejprve všechny proxy pro `PopupControlExtender` musí být na stránce vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="5366e-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="5366e-124">Nabízí sadu ovládacího prvku ASP.NET AJAX `GetProxyForCurrentPopup()` metoda.</span><span class="sxs-lookup"><span data-stu-id="5366e-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="5366e-125">Objekt, vrátí tato metoda podporuje `Commit()` metodu, která odešle hodnotu zpět do ovládacího prvku, který aktivoval místním (ne ovládací prvek, který spouští volání metoda!).</span><span class="sxs-lookup"><span data-stu-id="5366e-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="5366e-126">Následující kód poskytuje vybraným datem jako argument pro `Commit()` metoda, způsobí kód ke zpětnému zápisu vybraným datem do textového pole:</span><span class="sxs-lookup"><span data-stu-id="5366e-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="5366e-127">Nyní vždy, když kliknete na kalendářní datum vybraným datem se zobrazí v přidružené textového pole vytvoření ovládací prvek pro výběr data, která aktuálně naleznete na mnoha webů.</span><span class="sxs-lookup"><span data-stu-id="5366e-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="5366e-128">[![Kalendář se zobrazí, když uživatel klikne do textového pole](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5366e-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="5366e-129">Kalendář se zobrazí, když uživatel klikne do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5366e-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="5366e-130">[![Kliknutím na datum vloží ho do textového pole](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="5366e-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="5366e-131">Kliknutím na datum vloží ho do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5366e-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5366e-132">[Předchozí](using-multiple-popup-controls-vb.md)
> [další](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5366e-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
