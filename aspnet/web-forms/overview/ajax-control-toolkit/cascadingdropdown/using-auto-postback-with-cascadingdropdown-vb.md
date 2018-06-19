---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Pomocí automatického provedení operace CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871373"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="c8f09-103">Pomocí automatického provedení operace CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="c8f09-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="c8f09-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c8f09-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c8f09-105">[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c8f09-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="c8f09-106">Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="c8f09-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c8f09-107">Ale při použití ovládacího prvku CascadingDropDown ASP. Funkce Automatické odeslání NET na rozevírací seznam řízení nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c8f09-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c8f09-108">S určitý kód JavaScript se vyhnout této vliv.</span><span class="sxs-lookup"><span data-stu-id="c8f09-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="c8f09-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="c8f09-109">Overview</span></span>

<span data-ttu-id="c8f09-110">Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="c8f09-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c8f09-111">(Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) Ale při použití ovládacího prvku CascadingDropDown ASP. Funkce Automatické odeslání NET na rozevírací seznam řízení nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c8f09-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c8f09-112">S určitý kód JavaScript se vyhnout této vliv.</span><span class="sxs-lookup"><span data-stu-id="c8f09-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="c8f09-113">Kroky</span><span class="sxs-lookup"><span data-stu-id="c8f09-113">Steps</span></span>

<span data-ttu-id="c8f09-114">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř &lt; `form` &gt; element):</span><span class="sxs-lookup"><span data-stu-id="c8f09-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="c8f09-115">Ovládací prvek rozevírací seznam je pak potřeba:</span><span class="sxs-lookup"><span data-stu-id="c8f09-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="c8f09-116">Pro tento seznam je přidána rozšiřujícího objektu CascadingDropDown poskytuje informace adresy URL a metoda webové služby:</span><span class="sxs-lookup"><span data-stu-id="c8f09-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="c8f09-117">Rozšiřujícího objektu CascadingDropDown pak asynchronní volání webové služby pomocí následující podpis metody:</span><span class="sxs-lookup"><span data-stu-id="c8f09-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="c8f09-118">Metoda vrátí pole typu CascadingDropDown hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c8f09-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="c8f09-119">Konstruktor typu očekává první popisek položky seznamu a potom hodnotu (HTML `value` atributu).</span><span class="sxs-lookup"><span data-stu-id="c8f09-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="c8f09-120">Při načítání stránky v prohlížeči naplní rozevíracího seznamu s tři dodavateli, druhý probíhá předem vybrali.</span><span class="sxs-lookup"><span data-stu-id="c8f09-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="c8f09-121">Také definuje ASP.NET `__doPostBack()` metodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f09-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="c8f09-122">Po načtení stránky toto volání jazyka JavaScript se přidá do rozevíracího seznamu, pouze pokud je ale elementy v ní.</span><span class="sxs-lookup"><span data-stu-id="c8f09-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="c8f09-123">Pokud v seznamu nejsou žádné elementy, Toolkitu je aktuálně načítání, tak, aby kód jazyka JavaScript používá vypršení časového limitu a pokusí se znovu za půl sekundy.</span><span class="sxs-lookup"><span data-stu-id="c8f09-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="c8f09-124">Tímto způsobem zpětné volání je spustit pouze když existuje ve skutečnosti prvků v seznamu a uživatel vybere položku.</span><span class="sxs-lookup"><span data-stu-id="c8f09-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="c8f09-125">[![Když vyberete prvek seznamu, budou zpětné volání](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c8f09-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="c8f09-126">Výběr prvku seznamu způsobí, že zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c8f09-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8f09-127">Předchozí</span><span class="sxs-lookup"><span data-stu-id="c8f09-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
