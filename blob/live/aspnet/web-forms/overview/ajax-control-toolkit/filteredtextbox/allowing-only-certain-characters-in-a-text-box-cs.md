---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: "Povolení jenom určitých znaků v textovém poli (C#) | Microsoft Docs"
author: wenz
description: "Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáte neplatný..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 246c3b5dd55ceb0f47ad1f4982ae5b3bf855e747
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="f8453-104">Povolení jenom určitých znaků v textovém poli (C#)</span><span class="sxs-lookup"><span data-stu-id="f8453-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="f8453-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f8453-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f8453-106">[Stáhněte si kód](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f8453-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="f8453-107">Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="f8453-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="f8453-108">Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="f8453-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="f8453-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="f8453-109">Overview</span></span>

<span data-ttu-id="f8453-110">Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="f8453-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="f8453-111">Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="f8453-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="f8453-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="f8453-112">Steps</span></span>

<span data-ttu-id="f8453-113">Obsahuje sadu ovládacího prvku ASP.NET AJAX `FilteredTextBox` řídit, která rozšiřuje textové pole.</span><span class="sxs-lookup"><span data-stu-id="f8453-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="f8453-114">Po aktivaci, může je třeba zadat pouze určitou sadu znaků do pole.</span><span class="sxs-lookup"><span data-stu-id="f8453-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="f8453-115">Tento postup vyžaduje, je nejprve nutné jako obvykle prvku ASP.NET AJAX `ScriptManager` což způsobí načtení knihoven jazyka JavaScript, které jsou také používány Toolkitu ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="f8453-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="f8453-116">Potom potřebujeme textové pole:</span><span class="sxs-lookup"><span data-stu-id="f8453-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="f8453-117">Nakonec `FilteredTextBoxExtender` řízení postará znaky má uživatel na typ omezení.</span><span class="sxs-lookup"><span data-stu-id="f8453-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="f8453-118">Nastavte nejprve, `TargetControlID` atribut `ID` z `TextBox` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="f8453-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="f8453-119">Potom vyberte jednu z dostupných `FilterType` hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f8453-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="f8453-120">`Custom`Výchozí; je nutné zadat seznam platný znaků</span><span class="sxs-lookup"><span data-stu-id="f8453-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="f8453-121">`LowercaseLetters`jenom malá písmena</span><span class="sxs-lookup"><span data-stu-id="f8453-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="f8453-122">`Numbers`pouze číslice</span><span class="sxs-lookup"><span data-stu-id="f8453-122">`Numbers` digits only</span></span>
- <span data-ttu-id="f8453-123">`UppercaseLetters`jenom velká písmena</span><span class="sxs-lookup"><span data-stu-id="f8453-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="f8453-124">Pokud `Custom FilterType` se používá, `ValidChars` vlastnost musí být nastavené a zadat seznam znaky, které může být typu.</span><span class="sxs-lookup"><span data-stu-id="f8453-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="f8453-125">Tím: Pokud se pokusíte vložit text do textového pole, se odeberou všechny neplatné znaky.</span><span class="sxs-lookup"><span data-stu-id="f8453-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="f8453-126">Zde je kód pro `FilteredTextBoxExtender` ovládací prvek, který umožňuje pouze číslice (něco, co by také bylo umožněno s `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="f8453-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="f8453-127">Spustit na stránku a zkuste zadat písmeno, pokud je povolen jazyk JavaScript, nebude fungovat; číslic se ale zobrazí na stránce.</span><span class="sxs-lookup"><span data-stu-id="f8453-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="f8453-128">Ale Všimněte si, že ochranu `FilteredTextBox` poskytuje není odrážka ověření: Pokud JavaScript je povolena, všechna data lze zadat do textového pole, budete muset použít znamená další ověřování, tj. ASP. Ovládací prvky NET na ověření.</span><span class="sxs-lookup"><span data-stu-id="f8453-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="f8453-129">[![Lze zadat pouze číslice](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f8453-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="f8453-130">Lze zadat pouze číslice ([Kliknutím zobrazit obrázek v plné velikosti](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f8453-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f8453-131">Další</span><span class="sxs-lookup"><span data-stu-id="f8453-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
