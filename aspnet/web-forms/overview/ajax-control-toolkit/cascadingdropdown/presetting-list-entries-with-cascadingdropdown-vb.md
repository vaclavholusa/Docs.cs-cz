---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Předvedení položek seznamu ovládacím prvkem CascadingDropDown (VB) | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5133516311478d0a4faab45721c6b1d0a251b4b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817749"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="2fc8b-103">Předvedení položek seznamu ovládacím prvkem CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="2fc8b-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="2fc8b-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2fc8b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2fc8b-105">[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2fc8b-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="2fc8b-106">Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="2fc8b-107">Stačí nepatrné kódu je možné, že prvek seznamu je předem vybrali po dynamicky načtení dat.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="2fc8b-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="2fc8b-108">Overview</span></span>

<span data-ttu-id="2fc8b-109">Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="2fc8b-110">(Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) Stačí nepatrné kódu je možné, že prvek seznamu je předem vybrali po dynamicky načtení dat.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="2fc8b-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="2fc8b-111">Steps</span></span>

<span data-ttu-id="2fc8b-112">K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="2fc8b-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="2fc8b-113">Ovládací prvek DropDownList je pak potřeba:</span><span class="sxs-lookup"><span data-stu-id="2fc8b-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="2fc8b-114">Pro tento seznam se přidá rozšíření CascadingDropDown poskytuje adresu URL a metoda informace o webových službách:</span><span class="sxs-lookup"><span data-stu-id="2fc8b-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="2fc8b-115">Zařízení extender CascadingDropDown pak asynchronně volá webové služby s využitím následující podpis metody:</span><span class="sxs-lookup"><span data-stu-id="2fc8b-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="2fc8b-116">Metoda vrátí pole typu hodnota CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="2fc8b-117">Konstruktor typu očekává, že nejprve titulek položky seznamu a pak hodnotu (HTML `value` atributu).</span><span class="sxs-lookup"><span data-stu-id="2fc8b-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="2fc8b-118">Pokud třetí argument je nastaven na hodnotu true, v seznamu je element automaticky vybrán v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="2fc8b-119">Načítání stránky v prohlížeči vyplní rozevíracího seznamu, s třemi dodavateli, druhý se předem vybrali.</span><span class="sxs-lookup"><span data-stu-id="2fc8b-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="2fc8b-120">[![V seznamu je vyplněný a předem vybrali automaticky](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2fc8b-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="2fc8b-121">V seznamu je vyplněný a předem vybrali automaticky ([kliknutím ji zobrazíte obrázek v plné velikosti](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2fc8b-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2fc8b-122">[Předchozí](using-cascadingdropdown-with-a-database-vb.md)
> [další](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2fc8b-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
