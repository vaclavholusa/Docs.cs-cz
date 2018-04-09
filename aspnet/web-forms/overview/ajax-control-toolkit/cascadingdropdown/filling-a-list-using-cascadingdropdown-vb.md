---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Naplnění seznamu pomocí CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e488a30443970d5e2ce825abd96d8e4a027585d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="87af0-103">Naplnění seznamu pomocí CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="87af0-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="87af0-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="87af0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="87af0-105">[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="87af0-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="87af0-106">Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="87af0-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="87af0-107">(Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) V prvním kroku k vyřešení je ve skutečnosti vyplnění pomocí tento ovládací prvek rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="87af0-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="87af0-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="87af0-108">Overview</span></span>

<span data-ttu-id="87af0-109">Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="87af0-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="87af0-110">(Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) V prvním kroku k vyřešení je ve skutečnosti vyplnění pomocí tento ovládací prvek rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="87af0-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="87af0-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="87af0-111">Steps</span></span>

<span data-ttu-id="87af0-112">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="87af0-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="87af0-113">Ovládací prvek rozevírací seznam je pak potřeba:</span><span class="sxs-lookup"><span data-stu-id="87af0-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="87af0-114">Pro tento seznam se přidá rozšiřujícího objektu CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="87af0-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="87af0-115">K webové službě, která pak vrátí seznam položek, který se má zobrazit v seznamu, kterou bude odesílat Asynchronní požadavek.</span><span class="sxs-lookup"><span data-stu-id="87af0-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="87af0-116">Tento postup vyžaduje je nutné následující atributy CascadingDropDown nastavit:</span><span class="sxs-lookup"><span data-stu-id="87af0-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="87af0-117">`ServicePath`: Adresa URL webové služby doručování položky seznamu</span><span class="sxs-lookup"><span data-stu-id="87af0-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="87af0-118">`ServiceMethod`: Webové metody doručování položky seznamu</span><span class="sxs-lookup"><span data-stu-id="87af0-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="87af0-119">`TargetControlID`: ID rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="87af0-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="87af0-120">`Category`: Informace o kategoriích, které je odeslána do webové metody při volání</span><span class="sxs-lookup"><span data-stu-id="87af0-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="87af0-121">`PromptText`: Text zobrazí v případě, že asynchronní načítání seznamu dat ze serveru</span><span class="sxs-lookup"><span data-stu-id="87af0-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="87af0-122">Zde je kód pro `CascadingDropDown` elementu.</span><span class="sxs-lookup"><span data-stu-id="87af0-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="87af0-123">Jediným rozdílem mezi C# a VB je název přidružené webové služby:</span><span class="sxs-lookup"><span data-stu-id="87af0-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="87af0-124">Kód jazyka JavaScript pocházejících z `CascadingDropDown` rozšiřujícího objektu volá metody webové služby s podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="87af0-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="87af0-125">Proto je důležitým aspektem, že metoda musí vrátit pole typu `CascadingDropDownNameValue` (definovanou sadu ovládacího prvku ASP.NET AJAX).</span><span class="sxs-lookup"><span data-stu-id="87af0-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="87af0-126">V `CascadingDropDownNameValue` contructor, je třeba zadat text položky seznamu a pak jeho hodnoty, první stejně jako `<option value="VALUE">NAME</option>` by se ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="87af0-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="87af0-127">Tady je několik ukázkových dat:</span><span class="sxs-lookup"><span data-stu-id="87af0-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="87af0-128">Při načítání stránky v prohlížeči aktivuje se v seznamu pro vyplnění pomocí tří dodavatelů.</span><span class="sxs-lookup"><span data-stu-id="87af0-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="87af0-129">[![V seznamu se vyplní automaticky](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="87af0-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="87af0-130">V seznamu se vyplní automaticky ([Kliknutím zobrazit obrázek v plné velikosti](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="87af0-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87af0-131">[Předchozí](using-auto-postback-with-cascadingdropdown-cs.md)
> [další](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="87af0-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
