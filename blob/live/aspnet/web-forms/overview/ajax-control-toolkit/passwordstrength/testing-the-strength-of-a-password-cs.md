---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: "Testování síla hesla (C#) | Microsoft Docs"
author: wenz
description: "Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit. PasswordStrength ovládacího prvku ASP. N...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: eda7baae1833b074ba34d8f10fa434df14cc592e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="6d4b1-104">Testování síla hesla (C#)</span><span class="sxs-lookup"><span data-stu-id="6d4b1-104">Testing the Strength of a Password (C#)</span></span>
====================
<span data-ttu-id="6d4b1-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6d4b1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6d4b1-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6d4b1-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="6d4b1-107">Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="6d4b1-108">PasswordStrength ovládacího prvku ASP.NET AJAX Control Toolkit můžete zkontrolovat, jak dobrý je heslo.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="6d4b1-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="6d4b1-109">Overview</span></span>

<span data-ttu-id="6d4b1-110">Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="6d4b1-111">`PasswordStrength` Ovládací prvek v sadě nástrojů ovládacího prvku ASP.NET AJAX, můžete zkontrolovat, jak dobrý je heslo.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="6d4b1-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="6d4b1-112">Steps</span></span>

<span data-ttu-id="6d4b1-113">`PasswordStrength` Řízení rozšiřuje textové pole a zkontroluje, zda je heslo v ní dostatečně funkční.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="6d4b1-114">Nabízí širokou řadu možností prostřednictvím atributy; Zde jsou jen některé z nich:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="6d4b1-115">`MinimumNumericCharacters`minimální počet číselné znaků požadovaných v hesle</span><span class="sxs-lookup"><span data-stu-id="6d4b1-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="6d4b1-116">`MinimumSymbolCharacters`minimální počet znaků symbolu (ne písmen a číslic) v hesle</span><span class="sxs-lookup"><span data-stu-id="6d4b1-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="6d4b1-117">`PreferredPasswordLength`Minimální délka hesla</span><span class="sxs-lookup"><span data-stu-id="6d4b1-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="6d4b1-118">`RequiresUpperAndLowerCaseCharacters`jestli heslo musí používat velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="6d4b1-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="6d4b1-119">`StrengthIndicatorType` Poskytuje informace o tom, jak k dispozici síly hesla, jako text (hodnota `"Text"`) nebo jako druh indikátor průběhu (hodnota `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="6d4b1-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="6d4b1-120">V `DisplayPosition` atribut, můžete konfigurovat umístění zobrazení informací.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="6d4b1-121">Tady je kompletní příklad, včetně prvku ASP.NET AJAX `ScriptManager` ovládací prvek, `PasswordStrength` řízení a samozřejmě textového pole, které může uživatel zadat heslo.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="6d4b1-122">Pole pozdější formuláře je z důvodu ukázce běžného textového pole a pole hesla, aby mohli zobrazit během vývoje zadávané.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="6d4b1-123">Spuštění stránky a zadejte ji okamžitě: pouze po malá písmena, velká písmena, číslice a symboly, které jste zadali, se považuje jako nedělitelné heslo.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="6d4b1-124">[![Teď heslo je vhodný pro (celkem)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6d4b1-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="6d4b1-125">Teď je heslo (celkem) vhodný ([Kliknutím zobrazit obrázek v plné velikosti](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6d4b1-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6d4b1-126">Další</span><span class="sxs-lookup"><span data-stu-id="6d4b1-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
