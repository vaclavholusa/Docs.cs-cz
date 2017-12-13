---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: "Zpracování postback z ovládacího prvku místní nabídka bez UpdatePanel (VB) | Microsoft Docs"
author: wenz
description: "Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek. Zpětné volání při výskytu v su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c4afee37eab33036e5e563e78f873275951700b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="f4ced-104">Zpracování postback z ovládacího prvku místní nabídka bez UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="f4ced-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="f4ced-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f4ced-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f4ced-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f4ced-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="f4ced-107">Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="f4ced-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="f4ced-108">Pokud dojde k zpětné volání takové panelu a existují několik panelů na stránce je obtížné určit klepnutí na panelu.</span><span class="sxs-lookup"><span data-stu-id="f4ced-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="f4ced-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="f4ced-109">Overview</span></span>

<span data-ttu-id="f4ced-110">Rozšiřujícího objektu PopupControl v Toolkitu AJAX nabízí snadný způsob, jak aktivovat místní okno, pokud je aktivován další ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="f4ced-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="f4ced-111">Pokud dojde k zpětné volání takové panelu a existují několik panelů na stránce je obtížné určit klepnutí na panelu.</span><span class="sxs-lookup"><span data-stu-id="f4ced-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="f4ced-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="f4ced-112">Steps</span></span>

<span data-ttu-id="f4ced-113">Při použití `PopupControl` s zpětné volání, ale bez nutnosti `UpdatePanel` na stránce Toolkitu nenabízí způsob, jak určit, který element klienta má spustí automaticky otevírané zase způsobil zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="f4ced-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="f4ced-114">Ale malé efektu nabízí řešení pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="f4ced-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="f4ced-115">První řadě je zde základní nastavení: dvě textová pole, které oba aktivovat stejnou místní kalendáře.</span><span class="sxs-lookup"><span data-stu-id="f4ced-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="f4ced-116">Dva `PopupControlExtenders` společně nabídnou textová pole a místní.</span><span class="sxs-lookup"><span data-stu-id="f4ced-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="f4ced-117">Základní myšlenkou je skryté pole formuláře v přidání &lt; `form` &gt; element, který obsahuje textové pole, které se spustí automaticky otevřeném okně:</span><span class="sxs-lookup"><span data-stu-id="f4ced-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="f4ced-118">Při načtení stránky kódu jazyka JavaScript přidá do obou polí obslužné rutiny události: vždy, když uživatel klikne na textové pole, jeho název je zapsán do skryté pole formuláře:</span><span class="sxs-lookup"><span data-stu-id="f4ced-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="f4ced-119">V kódu na straně serveru musí přečíst hodnotu skrytého pole.</span><span class="sxs-lookup"><span data-stu-id="f4ced-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="f4ced-120">Vzhledem k tomu, že jsou trivial k manipulaci s skrytého pole, seznamu povolených IP adres přístup k ověření skryté hodnoty je požadovaná.</span><span class="sxs-lookup"><span data-stu-id="f4ced-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="f4ced-121">Jakmile byl identifikován správný textového pole, datum z kalendáře se zapisují do ní.</span><span class="sxs-lookup"><span data-stu-id="f4ced-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="f4ced-122">[![Kalendář se zobrazí, když uživatel klikne do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f4ced-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="f4ced-123">Kalendář se zobrazí, když uživatel klikne do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f4ced-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="f4ced-124">[![Kliknutím na datum vloží ho do textového pole](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f4ced-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="f4ced-125">Kliknutím na datum vloží ho do textového pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f4ced-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f4ced-126">Předchozí</span><span class="sxs-lookup"><span data-stu-id="f4ced-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
