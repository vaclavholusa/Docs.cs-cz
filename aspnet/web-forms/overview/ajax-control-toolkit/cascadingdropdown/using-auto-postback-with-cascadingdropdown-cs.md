---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Použití funkce Auto-Postback ovládacím prvkem CascadingDropDown (C#) | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: bece4f78b46c44db988e04e0e987450d94c37aab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819377"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="e60aa-103">Použití funkce Auto-Postback ovládacím prvkem CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="e60aa-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="e60aa-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e60aa-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e60aa-105">[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e60aa-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="e60aa-106">Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e60aa-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e60aa-107">Ale při použití ovládacího prvku CascadingDropDown ASP. Prvek DropDownList NET AutoPostBack funkce nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, samotného.</span><span class="sxs-lookup"><span data-stu-id="e60aa-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="e60aa-108">Pomocí kódu jazyka JavaScript se můžete vyhnout tomuto chování.</span><span class="sxs-lookup"><span data-stu-id="e60aa-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="e60aa-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="e60aa-109">Overview</span></span>

<span data-ttu-id="e60aa-110">Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e60aa-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e60aa-111">(Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) Ale při použití ovládacího prvku CascadingDropDown ASP. Prvek DropDownList NET AutoPostBack funkce nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, samotného.</span><span class="sxs-lookup"><span data-stu-id="e60aa-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="e60aa-112">Pomocí kódu jazyka JavaScript se můžete vyhnout tomuto chování.</span><span class="sxs-lookup"><span data-stu-id="e60aa-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="e60aa-113">Kroky</span><span class="sxs-lookup"><span data-stu-id="e60aa-113">Steps</span></span>

<span data-ttu-id="e60aa-114">K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci &lt; `form` &gt; element):</span><span class="sxs-lookup"><span data-stu-id="e60aa-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="e60aa-115">Ovládací prvek DropDownList je pak potřeba:</span><span class="sxs-lookup"><span data-stu-id="e60aa-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="e60aa-116">Pro tento seznam se přidá rozšíření CascadingDropDown poskytuje adresu URL a metoda informace o webových službách:</span><span class="sxs-lookup"><span data-stu-id="e60aa-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="e60aa-117">Zařízení extender CascadingDropDown pak asynchronně volá webové služby s využitím následující podpis metody:</span><span class="sxs-lookup"><span data-stu-id="e60aa-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="e60aa-118">Metoda vrátí pole typu hodnota CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="e60aa-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="e60aa-119">Konstruktor typu očekává, že nejprve titulek položky seznamu a pak hodnotu (HTML `value` atributu).</span><span class="sxs-lookup"><span data-stu-id="e60aa-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="e60aa-120">Načítání stránky v prohlížeči vyplní rozevíracího seznamu, s třemi dodavateli, druhý se předem vybrali.</span><span class="sxs-lookup"><span data-stu-id="e60aa-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="e60aa-121">Také definuje ASP.NET `__doPostBack()` metodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e60aa-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="e60aa-122">Po načtení stránky toto volání JavaScript se přidá do rozevíracího seznamu, pouze pokud je ale prvky v ní.</span><span class="sxs-lookup"><span data-stu-id="e60aa-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="e60aa-123">Pokud v seznamu nejsou žádné elementy, Control Toolkit je aktuálně se načítá, tak, aby kód jazyka JavaScript používá vypršení časového limitu a pokusí se znovu za půl sekundy.</span><span class="sxs-lookup"><span data-stu-id="e60aa-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="e60aa-124">Tímto způsobem, zpětné volání je pouze spuštěn, když v seznamu nejsou ve skutečnosti elementy a uživatel vybere záznam.</span><span class="sxs-lookup"><span data-stu-id="e60aa-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="e60aa-125">[![Výběr prvku seznamu vyvolá zpětné volání](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e60aa-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="e60aa-126">Výběr prvku seznamu vyvolá zpětné volání ([kliknutím ji zobrazíte obrázek v plné velikosti](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e60aa-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e60aa-127">[Předchozí](presetting-list-entries-with-cascadingdropdown-cs.md)
> [další](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e60aa-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
