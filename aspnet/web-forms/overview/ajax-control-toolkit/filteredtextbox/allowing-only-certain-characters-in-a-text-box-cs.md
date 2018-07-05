---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Povolení určitých znaků v textovém poli (C#) | Dokumentace Microsoftu
author: wenz
description: Validačních ovládacích prvků technologie ASP.NET můžete zajistit, že jsou povolené jenom některé znaky ve vstupu uživatele. Ale to stále nezabrání uživatelům zadáte neplatný...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7203d7f367f275d2d80c86119edc9645c9d24c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400727"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="4f01f-104">Povolení určitých znaků v textovém poli (C#)</span><span class="sxs-lookup"><span data-stu-id="4f01f-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="4f01f-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4f01f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4f01f-106">[Stáhněte si kód](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4f01f-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="4f01f-107">Validačních ovládacích prvků technologie ASP.NET můžete zajistit, že jsou povolené jenom některé znaky ve vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f01f-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="4f01f-108">Ale to stále nezabrání uživatelům zadáte neplatné znaky pokus o odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="4f01f-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="4f01f-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="4f01f-109">Overview</span></span>

<span data-ttu-id="4f01f-110">Validačních ovládacích prvků technologie ASP.NET můžete zajistit, že jsou povolené jenom některé znaky ve vstupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f01f-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="4f01f-111">Ale to stále nezabrání uživatelům zadáte neplatné znaky pokus o odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="4f01f-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="4f01f-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="4f01f-112">Steps</span></span>

<span data-ttu-id="4f01f-113">ASP.NET AJAX Control Toolkit obsahuje `FilteredTextBox` ovládací prvek, který rozšiřuje textové pole.</span><span class="sxs-lookup"><span data-stu-id="4f01f-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="4f01f-114">Po aktivaci, můžete do pole zadat pouze určitou sadu znaků.</span><span class="sxs-lookup"><span data-stu-id="4f01f-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="4f01f-115">Aby to fungovalo, nejprve musíme obvyklým technologie ASP.NET AJAX `ScriptManager` což způsobí načtení knihovny JavaScript, které jsou také používány ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="4f01f-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="4f01f-116">Pak potřebujeme textové pole:</span><span class="sxs-lookup"><span data-stu-id="4f01f-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="4f01f-117">Nakonec `FilteredTextBoxExtender` postará o omezení znaků, které uživatel může na typ ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="4f01f-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="4f01f-118">Nejprve nastavte `TargetControlID` atribut `ID` z `TextBox` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="4f01f-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="4f01f-119">Potom vyberte jednu z dostupných `FilterType` hodnoty:</span><span class="sxs-lookup"><span data-stu-id="4f01f-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="4f01f-120">`Custom` výchozí. je nutné zadat seznam platné znaky</span><span class="sxs-lookup"><span data-stu-id="4f01f-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="4f01f-121">`LowercaseLetters` jenom malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4f01f-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="4f01f-122">`Numbers` pouze číslice</span><span class="sxs-lookup"><span data-stu-id="4f01f-122">`Numbers` digits only</span></span>
- <span data-ttu-id="4f01f-123">`UppercaseLetters` jenom velkými písmeny</span><span class="sxs-lookup"><span data-stu-id="4f01f-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="4f01f-124">Pokud `Custom FilterType` se používá, `ValidChars` vlastnost musí být nastavena a zadat seznam znaků, které může být zadán.</span><span class="sxs-lookup"><span data-stu-id="4f01f-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="4f01f-125">Mimochodem: Pokud se pokusíte vložit text do textového pole, se odeberou všechny neplatné znaky.</span><span class="sxs-lookup"><span data-stu-id="4f01f-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="4f01f-126">Tady je zápis `FilteredTextBoxExtender` ovládací prvek, který umožňuje pouze číslice (něco, co by také bylo možné s `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="4f01f-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="4f01f-127">Spustit na stránku a zkuste zadat písmeno, pokud je povolen jazyk JavaScript, nebude fungovat; na stránce se ale zobrazí číslic.</span><span class="sxs-lookup"><span data-stu-id="4f01f-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="4f01f-128">Ale Všimněte si, že ochranu `FilteredTextBox` poskytuje není odrážky testování: Pokud jazyk JavaScript je povolený, žádná data můžete zadat do textového pole, proto budete muset použít další ověřovací prostředky, například ASP. Ovládací prvky ověřování vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="4f01f-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="4f01f-129">[![Můžete zadat pouze číslice](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f01f-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="4f01f-130">Můžete zadat pouze číslice ([kliknutím ji zobrazíte obrázek v plné velikosti](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4f01f-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4f01f-131">Next</span><span class="sxs-lookup"><span data-stu-id="4f01f-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
