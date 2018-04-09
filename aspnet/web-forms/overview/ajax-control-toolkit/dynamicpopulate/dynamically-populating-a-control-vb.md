---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamicky naplnění ovládacího prvku (VB) | Microsoft Docs
author: wenz
description: DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="2f7dd-103">Dynamicky naplnění ovládacího prvku (VB)</span><span class="sxs-lookup"><span data-stu-id="2f7dd-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="2f7dd-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2f7dd-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2f7dd-105">[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2f7dd-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="2f7dd-106">DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="2f7dd-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="2f7dd-107">Overview</span></span>

<span data-ttu-id="2f7dd-108">`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="2f7dd-109">Tento kurz ukazuje, jak nastavit tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="2f7dd-110">Kroky</span><span class="sxs-lookup"><span data-stu-id="2f7dd-110">Steps</span></span>

<span data-ttu-id="2f7dd-111">Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="2f7dd-112">Třída webové služby vyžaduje `ScriptService` atribut, který je definován v rámci `Microsoft.Web.Script.Services`; v opačném případě prvku ASP.NET AJAX, nelze vytvořit proxy server JavaScript na straně klienta pro webovou službu, který naopak je požadován `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="2f7dd-113">Webové metody měli očekávat jeden argument typu řetězec, s názvem `contextKey`, protože `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="2f7dd-114">Webovou službu následující vrátí aktuální datum ve formátu reprezentována `contextKey` argument:</span><span class="sxs-lookup"><span data-stu-id="2f7dd-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2f7dd-115">Webová služba pak je uložena jako `DynamicPopulate.vb.asmx`.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="2f7dd-116">Alternativně může implementovat `getDate()` metoda jako metodu stránky v rámci skutečné stránky ASP.NET s `DynamicPopulate` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="2f7dd-117">V dalším kroku vytvořte nový soubor technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="2f7dd-118">Jako vždy, prvním krokem je zahrnout `ScriptManager` na aktuální stránce se načíst knihovnu ASP.NET AJAX a aby pracovní sada nástrojů ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="2f7dd-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2f7dd-119">Potom přidat ovládací prvek popisek (například pomocí ovládacího prvku HTML se stejným názvem, nebo &lt; `asp:Label`  / &gt; ovládacího prvku webového) který později zobrazí výsledek volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="2f7dd-120">K aktivaci dynamické plnění pak bude sloužit HTML tlačítko (jako ovládací prvek jazyka HTML, protože jsme nevyžadují zpětné volání k serveru):</span><span class="sxs-lookup"><span data-stu-id="2f7dd-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="2f7dd-121">Nakonec potřebujeme `DynamicPopulateExtender` řízení přenosu věcí nahoru.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="2f7dd-122">Následující atributy se nastaví (kromě těch zřejmé, `ID` a `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="2f7dd-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="2f7dd-123">`TargetControlID` umístění výsledek z volání webové služby</span><span class="sxs-lookup"><span data-stu-id="2f7dd-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="2f7dd-124">`ServicePath` Cesta k webové službě (vynechat, pokud chcete použít metodu stránky)</span><span class="sxs-lookup"><span data-stu-id="2f7dd-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="2f7dd-125">`ServiceMethod` Název webové metody nebo stránce – metoda</span><span class="sxs-lookup"><span data-stu-id="2f7dd-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="2f7dd-126">`ContextKey` informace o kontextu k odeslání do webové služby</span><span class="sxs-lookup"><span data-stu-id="2f7dd-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="2f7dd-127">`PopulateTriggerControlID` element, který aktivuje volání webové služby</span><span class="sxs-lookup"><span data-stu-id="2f7dd-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="2f7dd-128">`ClearContentsDuringUpdate` jestli se má prázdný target element během volání webové služby</span><span class="sxs-lookup"><span data-stu-id="2f7dd-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="2f7dd-129">Jak můžete vidět, ovládacího prvku vyžaduje některé informace, ale vytvoření všechno, co je poměrně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="2f7dd-130">Zde je kód pro `DynamicPopulateExtender` ovládací prvek v aktuální scénář:</span><span class="sxs-lookup"><span data-stu-id="2f7dd-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="2f7dd-131">V prohlížeči spuštění stránky ASP.NET a klikněte na tlačítko; Zobrazí se aktuální datum ve formátu měsíc den roku.</span><span class="sxs-lookup"><span data-stu-id="2f7dd-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="2f7dd-132">[![Klikněte na tlačítko načte data ze serveru](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2f7dd-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2f7dd-133">Klikněte na tlačítko načte data ze serveru ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2f7dd-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f7dd-134">[Předchozí](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [další](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2f7dd-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
