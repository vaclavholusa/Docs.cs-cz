---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: "Dynamicky naplnění ovládacího prvku (C#) | Microsoft Docs"
author: wenz
description: "DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="7559c-103">Dynamicky naplnění ovládacího prvku (C#)</span><span class="sxs-lookup"><span data-stu-id="7559c-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="7559c-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7559c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7559c-105">[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7559c-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="7559c-106">DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="7559c-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="7559c-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="7559c-107">Overview</span></span>

<span data-ttu-id="7559c-108">`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="7559c-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="7559c-109">Tento kurz ukazuje, jak nastavit tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="7559c-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="7559c-110">Kroky</span><span class="sxs-lookup"><span data-stu-id="7559c-110">Steps</span></span>

<span data-ttu-id="7559c-111">Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="7559c-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="7559c-112">Třída webové služby vyžaduje `ScriptService` atribut, který je definován v rámci `Microsoft.Web.Script.Services`; v opačném případě prvku ASP.NET AJAX, nelze vytvořit proxy server JavaScript na straně klienta pro webovou službu, který naopak je požadován `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="7559c-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="7559c-113">Webové metody měli očekávat jeden argument typu řetězec, s názvem `contextKey`, protože `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="7559c-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="7559c-114">Webovou službu následující vrátí aktuální datum ve formátu reprezentována `contextKey` argument:</span><span class="sxs-lookup"><span data-stu-id="7559c-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="7559c-115">Webová služba pak je uložena jako `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="7559c-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="7559c-116">Alternativně může implementovat `getDate()` metoda jako metodu stránky v rámci skutečné stránky ASP.NET s `DynamicPopulate` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7559c-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="7559c-117">V dalším kroku vytvořte nový soubor technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7559c-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="7559c-118">Jako vždy, prvním krokem je zahrnout `ScriptManager` na aktuální stránce se načíst knihovnu ASP.NET AJAX a aby pracovní sada nástrojů ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="7559c-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="7559c-119">Potom přidat ovládací prvek popisek (například pomocí ovládacího prvku HTML se stejným názvem, nebo &lt; `asp:Label`  / &gt; ovládacího prvku webového) který později zobrazí výsledek volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="7559c-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="7559c-120">K aktivaci dynamické plnění pak bude sloužit HTML tlačítko (jako ovládací prvek jazyka HTML, protože jsme nevyžadují zpětné volání k serveru):</span><span class="sxs-lookup"><span data-stu-id="7559c-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="7559c-121">Nakonec potřebujeme `DynamicPopulateExtender` řízení přenosu věcí nahoru.</span><span class="sxs-lookup"><span data-stu-id="7559c-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="7559c-122">Následující atributy se nastaví (kromě těch zřejmé, `ID` a `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="7559c-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="7559c-123">`TargetControlID`umístění výsledek z volání webové služby</span><span class="sxs-lookup"><span data-stu-id="7559c-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="7559c-124">`ServicePath`Cesta k webové službě (vynechat, pokud chcete použít metodu stránky)</span><span class="sxs-lookup"><span data-stu-id="7559c-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="7559c-125">`ServiceMethod`Název webové metody nebo stránce – metoda</span><span class="sxs-lookup"><span data-stu-id="7559c-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="7559c-126">`ContextKey`informace o kontextu k odeslání do webové služby</span><span class="sxs-lookup"><span data-stu-id="7559c-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="7559c-127">`PopulateTriggerControlID`element, který aktivuje volání webové služby</span><span class="sxs-lookup"><span data-stu-id="7559c-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="7559c-128">`ClearContentsDuringUpdate`jestli se má prázdný target element během volání webové služby</span><span class="sxs-lookup"><span data-stu-id="7559c-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="7559c-129">Jak můžete vidět, ovládacího prvku vyžaduje některé informace, ale vytvoření všechno, co je poměrně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="7559c-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="7559c-130">Zde je kód pro `DynamicPopulateExtender` ovládací prvek v aktuální scénář:</span><span class="sxs-lookup"><span data-stu-id="7559c-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="7559c-131">V prohlížeči spuštění stránky ASP.NET a klikněte na tlačítko; Zobrazí se aktuální datum ve formátu měsíc den roku.</span><span class="sxs-lookup"><span data-stu-id="7559c-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="7559c-132">[![Klikněte na tlačítko načte data ze serveru](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7559c-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="7559c-133">Klikněte na tlačítko načte data ze serveru ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7559c-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7559c-134">Další</span><span class="sxs-lookup"><span data-stu-id="7559c-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
