---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Dynamické naplnění ovládacího prvku (C#) | Dokumentace Microsoftu
author: wenz
description: ASP.NET AJAX Control Toolkit ovládacího prvku DynamicPopulate volání webové služby (nebo metodu stránky) a vyplní výsledné hodnoty do cílového ovládacího prvku na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f37a3a41c7b83738f97c0daacd781c52b55abc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756923"
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="22c14-103">Dynamické naplnění ovládacího prvku (C#)</span><span class="sxs-lookup"><span data-stu-id="22c14-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="22c14-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="22c14-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="22c14-105">[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="22c14-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="22c14-106">DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky.</span><span class="sxs-lookup"><span data-stu-id="22c14-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="22c14-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="22c14-107">Overview</span></span>

<span data-ttu-id="22c14-108">`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky.</span><span class="sxs-lookup"><span data-stu-id="22c14-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="22c14-109">Tento kurz ukazuje, jak nastavit tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="22c14-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="22c14-110">Kroky</span><span class="sxs-lookup"><span data-stu-id="22c14-110">Steps</span></span>

<span data-ttu-id="22c14-111">Za prvé, třeba webové služby ASP.NET, která implementuje metodu, které jsou volány `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="22c14-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="22c14-112">Vyžaduje třídu webové služby `ScriptService` atribut, který je definován v rámci `Microsoft.Web.Script.Services`; v opačném případě technologie ASP.NET AJAX nelze vytvořit proxy server JavaScript na straně klienta pro webovou službu, kterou pak vyžaduje `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="22c14-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="22c14-113">Metodu webové musíte očekávat jeden argument typu řetězec, volá `contextKey`, protože `DynamicPopulate` ovládací prvek odešle jednu část informací o kontextu se každé volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="22c14-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="22c14-114">Následující webová služba vrátí aktuální datum ve formátu reprezentována `contextKey` argument:</span><span class="sxs-lookup"><span data-stu-id="22c14-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="22c14-115">Webová služba se pak uloží jako `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="22c14-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="22c14-116">Alternativně je možné implementovat `getDate()` metody jako metody stránky v rámci skutečné stránky technologie ASP.NET s `DynamicPopulate` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="22c14-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="22c14-117">V dalším kroku vytvořte nový soubor technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="22c14-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="22c14-118">Jako vždy, prvním krokem je zahrnout `ScriptManager` na aktuální stránce se načíst knihovnu ASP.NET AJAX a jak zajistit Control Toolkit práce:</span><span class="sxs-lookup"><span data-stu-id="22c14-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="22c14-119">Pak přidejte ovládací prvek popisku (například pomocí ovládacího prvku HTML se stejným názvem, nebo &lt; `asp:Label`  / &gt; ovládací prvek webu) který se později zobrazí výsledek volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="22c14-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="22c14-120">HTML tlačítko (jako ovládací prvek HTML, protože jsme nevyžadují zpětného odeslání na server) se pak použije k aktivaci dynamické naplnění:</span><span class="sxs-lookup"><span data-stu-id="22c14-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="22c14-121">Nakonec potřebujeme `DynamicPopulateExtender` ovládacího prvku na věci při přenosu.</span><span class="sxs-lookup"><span data-stu-id="22c14-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="22c14-122">Následující atributy se nastaví (kromě těch zřejmé, `ID` a `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="22c14-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="22c14-123">`TargetControlID` kam umístit výsledek z volání webové služby</span><span class="sxs-lookup"><span data-stu-id="22c14-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="22c14-124">`ServicePath` Cesta k webové službě (vynechat, pokud chcete použít metodu stránky)</span><span class="sxs-lookup"><span data-stu-id="22c14-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="22c14-125">`ServiceMethod` Název webové metody nebo stránky – metoda</span><span class="sxs-lookup"><span data-stu-id="22c14-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="22c14-126">`ContextKey` informace o kontextu k odeslání do webové služby</span><span class="sxs-lookup"><span data-stu-id="22c14-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="22c14-127">`PopulateTriggerControlID` element, který aktivuje volání webové služby</span><span class="sxs-lookup"><span data-stu-id="22c14-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="22c14-128">`ClearContentsDuringUpdate` jestli se má prázdný target element během volání webové služby</span><span class="sxs-lookup"><span data-stu-id="22c14-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="22c14-129">Jak je vidět, ovládací prvek vyžaduje určité informace, ale umístění všeho na místě je poměrně přímočaré.</span><span class="sxs-lookup"><span data-stu-id="22c14-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="22c14-130">Tady je zápis `DynamicPopulateExtender` ovládacího prvku v aktuální situaci:</span><span class="sxs-lookup"><span data-stu-id="22c14-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="22c14-131">Na stránce ASP.NET spustit v prohlížeči a klikněte na tlačítko; Zobrazí se aktuální datum ve formátu měsíc roku dny.</span><span class="sxs-lookup"><span data-stu-id="22c14-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="22c14-132">[![Klikněte na tlačítko načte data ze serveru](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22c14-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="22c14-133">Klikněte na tlačítko načte data ze serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="22c14-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="22c14-134">Next</span><span class="sxs-lookup"><span data-stu-id="22c14-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
