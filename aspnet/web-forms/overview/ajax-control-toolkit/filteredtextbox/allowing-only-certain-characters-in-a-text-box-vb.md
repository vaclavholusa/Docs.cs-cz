---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Povolení jenom určitých znaků v textovém poli (VB) | Microsoft Docs
author: wenz
description: Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáte neplatný...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b63a3582c09e08310c97d4adfc7b8273458a723
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870151"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="cb8fa-104">Povolení jenom určitých znaků v textovém poli (VB)</span><span class="sxs-lookup"><span data-stu-id="cb8fa-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="cb8fa-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cb8fa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cb8fa-106">[Stáhněte si kód](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cb8fa-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="cb8fa-107">Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="cb8fa-108">Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="cb8fa-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="cb8fa-109">Overview</span></span>

<span data-ttu-id="cb8fa-110">Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="cb8fa-111">Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="cb8fa-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="cb8fa-112">Steps</span></span>

<span data-ttu-id="cb8fa-113">Obsahuje sadu ovládacího prvku ASP.NET AJAX `FilteredTextBox` řídit, která rozšiřuje textové pole.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="cb8fa-114">Po aktivaci, může je třeba zadat pouze určitou sadu znaků do pole.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="cb8fa-115">Tento postup vyžaduje, je nejprve nutné jako obvykle prvku ASP.NET AJAX `ScriptManager` což způsobí načtení knihoven jazyka JavaScript, které jsou také používány Toolkitu ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="cb8fa-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="cb8fa-116">Potom potřebujeme textové pole:</span><span class="sxs-lookup"><span data-stu-id="cb8fa-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="cb8fa-117">Nakonec `FilteredTextBoxExtender` řízení postará znaky má uživatel na typ omezení.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="cb8fa-118">Nastavte nejprve, `TargetControlID` atribut `ID` z `TextBox` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="cb8fa-119">Potom vyberte jednu z dostupných `FilterType` hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cb8fa-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="cb8fa-120">`Custom` Výchozí; je nutné zadat seznam platný znaků</span><span class="sxs-lookup"><span data-stu-id="cb8fa-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="cb8fa-121">`LowercaseLetters` jenom malá písmena</span><span class="sxs-lookup"><span data-stu-id="cb8fa-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="cb8fa-122">`Numbers` pouze číslice</span><span class="sxs-lookup"><span data-stu-id="cb8fa-122">`Numbers` digits only</span></span>
- <span data-ttu-id="cb8fa-123">`UppercaseLetters` jenom velká písmena</span><span class="sxs-lookup"><span data-stu-id="cb8fa-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="cb8fa-124">Pokud `Custom FilterType` se používá, `ValidChars` vlastnost musí být nastavené a zadat seznam znaky, které může být typu.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="cb8fa-125">Tím: Pokud se pokusíte vložit text do textového pole, se odeberou všechny neplatné znaky.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="cb8fa-126">Zde je kód pro `FilteredTextBoxExtender` ovládací prvek, který umožňuje pouze číslice (něco, co by také bylo umožněno s `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="cb8fa-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="cb8fa-127">Spustit na stránku a zkuste zadat písmeno, pokud je povolen jazyk JavaScript, nebude fungovat; číslic se ale zobrazí na stránce.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="cb8fa-128">Ale Všimněte si, že ochranu `FilteredTextBox` poskytuje není odrážka ověření: Pokud JavaScript je povolena, všechna data lze zadat do textového pole, budete muset použít znamená další ověřování, tj. ASP. Ovládací prvky NET na ověření.</span><span class="sxs-lookup"><span data-stu-id="cb8fa-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="cb8fa-129">[![Lze zadat pouze číslice](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb8fa-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="cb8fa-130">Lze zadat pouze číslice ([Kliknutím zobrazit obrázek v plné velikosti](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cb8fa-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cb8fa-131">Předchozí</span><span class="sxs-lookup"><span data-stu-id="cb8fa-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
