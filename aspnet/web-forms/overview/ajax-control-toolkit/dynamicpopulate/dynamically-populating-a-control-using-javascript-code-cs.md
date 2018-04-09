---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Dynamicky naplnění ovládacího prvku s použitím kódu JavaScript (C#) | Microsoft Docs
author: wenz
description: DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c9fd11ea0348eb7fe9a7634f7b26031339146828
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="50645-103">Dynamicky naplnění ovládacího prvku s použitím kódu JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="50645-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>
====================
<span data-ttu-id="50645-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="50645-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="50645-105">[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="50645-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="50645-106">DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="50645-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="50645-107">Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="50645-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="50645-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="50645-108">Overview</span></span>

<span data-ttu-id="50645-109">`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="50645-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="50645-110">Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="50645-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="50645-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="50645-111">Steps</span></span>

<span data-ttu-id="50645-112">Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulateExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="50645-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="50645-113">Webová služba implementuje metodu `getDate()` , očekává jeden argument typu řetězec, nazývá `contextKey`, vzhledem k tomu, `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="50645-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="50645-114">Zde je kód (soubor `DynamicPopulate.cs.asmx`) který načte aktuální datum v jednom ze tří formátů:</span><span class="sxs-lookup"><span data-stu-id="50645-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="50645-115">V dalším kroku vytvoří nový web s ASP.NET a spustit pomocí ovládacího prvku ASP.NET AJAX ScriptManager:</span><span class="sxs-lookup"><span data-stu-id="50645-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="50645-116">Potom přidat ovládací prvek popisek (například pomocí ovládacího prvku HTML se stejným názvem, nebo `<asp:Label />` ovládacího prvku webového) který později zobrazí výsledek volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="50645-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="50645-117">V dalším kroku zahrnout `DynamicPopulateExtender` řídit a poskytnout informace o webové služby, cílový ovládací prvek, ale ne název řídit, která aktivuje naplnění to bude provedeno později na použití vlastní JavaScript!</span><span class="sxs-lookup"><span data-stu-id="50645-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="50645-118">Teď do části JavaScript.</span><span class="sxs-lookup"><span data-stu-id="50645-118">Now to the JavaScript part.</span></span> <span data-ttu-id="50645-119">`$find()` Funkce, které jsou definované knihovnu ASP.NET AJAX, vrátí odkaz na straně serveru objekty sady nástrojů ovládacího prvku ASP.NET AJAX, jako `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="50645-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="50645-120">V aktuální soubor `$find("dpe")` vrátí odkaz na ten `DynamicPopulateExtender` ovládacího prvku stránce.</span><span class="sxs-lookup"><span data-stu-id="50645-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="50645-121">Poskytuje metodu s názvem `populate()` které spustí proces dynamické naplnění.</span><span class="sxs-lookup"><span data-stu-id="50645-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="50645-122">`populate()` Metoda vyžaduje jeden argument: klíč kontext, který bude sloužit jako argument `getDate()` webové metody.</span><span class="sxs-lookup"><span data-stu-id="50645-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="50645-123">Tak například `$find("dpe").populate("format1")` by naplnit štítek s aktuální datum ve formátu měsíc den roku.</span><span class="sxs-lookup"><span data-stu-id="50645-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="50645-124">Aby bylo možné ukázku trochu flexibilnější, uživatel může zvolit teď několik formát data.</span><span class="sxs-lookup"><span data-stu-id="50645-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="50645-125">Pro každou z nich se zobrazí přepínače.</span><span class="sxs-lookup"><span data-stu-id="50645-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="50645-126">Jednou uživatel klikne na na přepínače, kódu jazyka JavaScript dynamicky naplní štítek s formátem vybraným datem.</span><span class="sxs-lookup"><span data-stu-id="50645-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="50645-127">Zde jsou tyto přepínače:</span><span class="sxs-lookup"><span data-stu-id="50645-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="50645-128">Všimněte si, že se v kontextu přepínače, JavaScript výraz `this.value` odkazuje na hodnotu aktuální tlačítka, která se dělá jako přesně stejné informace `getDate()` metoda můžete pracovat.</span><span class="sxs-lookup"><span data-stu-id="50645-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="50645-129">[![Klikněte na tlačítko načte data ze serveru ve formátu určeném](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="50645-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="50645-130">Klikněte na tlačítko načte data ze serveru ve formátu určeném ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="50645-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="50645-131">[Předchozí](dynamically-populating-a-control-cs.md)
> [další](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="50645-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
