---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamické naplnění ovládacího prvku Javascriptovým kódem (VB) | Dokumentace Microsoftu
author: wenz
description: ASP.NET AJAX Control Toolkit ovládacího prvku DynamicPopulate volání webové služby (nebo metodu stránky) a vyplní výsledné hodnoty do cílového ovládacího prvku na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9df2a66bd49ecba52b0dd8b1d52a65b36c38a5dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366845"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="ca0a0-103">Dynamické naplnění ovládacího prvku Javascriptovým kódem (VB)</span><span class="sxs-lookup"><span data-stu-id="ca0a0-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="ca0a0-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ca0a0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ca0a0-105">[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ca0a0-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="ca0a0-106">DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="ca0a0-107">Je také možné aktivovat naplnění psát vlastní kód JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="ca0a0-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="ca0a0-108">Overview</span></span>

<span data-ttu-id="ca0a0-109">`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="ca0a0-110">Je také možné aktivovat naplnění psát vlastní kód JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ca0a0-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="ca0a0-111">Steps</span></span>

<span data-ttu-id="ca0a0-112">Za prvé, třeba webové služby ASP.NET, která implementuje metodu, které jsou volány `DynamicPopulateExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="ca0a0-113">Webová služba implementuje metodu `getDate()` , která očekává jeden argument typu řetězec, volá `contextKey`, protože `DynamicPopulate` ovládací prvek odešle jednu část informací o kontextu se každé volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="ca0a0-114">Zde je kód (soubor `DynamicPopulate.vb.asmx`) načte aktuální datum v jednom ze tří formátů:</span><span class="sxs-lookup"><span data-stu-id="ca0a0-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="ca0a0-115">V dalším kroku vytvoření nového webu technologie ASP.NET a začít pomocí ovládacího prvku ASP.NET AJAX ScriptManager:</span><span class="sxs-lookup"><span data-stu-id="ca0a0-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="ca0a0-116">Pak přidejte ovládací prvek popisku (například pomocí ovládacího prvku HTML se stejným názvem, nebo `<asp:Label />` ovládací prvek webu) který se později zobrazí výsledek volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="ca0a0-117">V dalším kroku zahrnout `DynamicPopulateExtender` řídit a poskytnout informace o webových službách, cílový ovládací prvek, ale nikoli název ovládacího prvku, které aktivuje naplnění to se provede později pomocí vlastního jazyka JavaScript!</span><span class="sxs-lookup"><span data-stu-id="ca0a0-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="ca0a0-118">Teď do části jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-118">Now to the JavaScript part.</span></span> <span data-ttu-id="ca0a0-119">`$find()` Funkce definované knihovnou ASP.NET AJAX, jako například vrátí odkaz na serverové objekty technologie ASP.NET AJAX Control Toolkit `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="ca0a0-120">V aktuálním souboru `$find("dpe")` vrátí odkaz na ten `DynamicPopulateExtender` ovládací prvek na stránce.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="ca0a0-121">Poskytuje metodu s názvem `populate()` který spustí proces dynamické naplnění.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="ca0a0-122">`populate()` Metoda vyžaduje jeden argument: kontext klíče, který bude sloužit jako argument `getDate()` webovou metodu.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="ca0a0-123">Takže například `$find("dpe").populate("format1")` by naplnit popisek s aktuální datum ve formátu měsíc roku dny.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="ca0a0-124">Aby ukázku trochu více flexibilní, může uživatel teď vybrat mezi několika formátů data.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="ca0a0-125">Pro každou z nich zobrazí se přepínač.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="ca0a0-126">Jednou uživatel klikne na přepínač, kód jazyka JavaScript dynamicky naplní popisek s vybraným datem formátu.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="ca0a0-127">Tady jsou tyto přepínače:</span><span class="sxs-lookup"><span data-stu-id="ca0a0-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="ca0a0-128">Všimněte si, že v rámci kontextu přepínací tlačítko, výraz jazyka JavaScript `this.value` odkazuje na hodnotu aktuální tlačítko, které je přesně stejné informace `getDate()` metoda můžete pracovat.</span><span class="sxs-lookup"><span data-stu-id="ca0a0-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="ca0a0-129">[![Klikněte na tlačítko načte data ze serveru ve formátu určeném](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ca0a0-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="ca0a0-130">Klikněte na tlačítko načte data ze serveru ve formátu určeném ([kliknutím ji zobrazíte obrázek v plné velikosti](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ca0a0-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca0a0-131">[Předchozí](dynamically-populating-a-control-vb.md)
> [další](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ca0a0-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
