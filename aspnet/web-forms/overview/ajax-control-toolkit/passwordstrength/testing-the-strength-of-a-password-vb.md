---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testování síla hesla (VB) | Microsoft Docs
author: wenz
description: Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit. PasswordStrength ovládacího prvku ASP. N....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="5ea4d-104">Testování síla hesla (VB)</span><span class="sxs-lookup"><span data-stu-id="5ea4d-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="5ea4d-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5ea4d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5ea4d-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5ea4d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="5ea4d-107">Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="5ea4d-108">PasswordStrength ovládacího prvku ASP.NET AJAX Control Toolkit můžete zkontrolovat, jak dobrý je heslo.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="5ea4d-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="5ea4d-109">Overview</span></span>

<span data-ttu-id="5ea4d-110">Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="5ea4d-111">`PasswordStrength` Ovládací prvek v sadě nástrojů ovládacího prvku ASP.NET AJAX, můžete zkontrolovat, jak dobrý je heslo.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="5ea4d-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="5ea4d-112">Steps</span></span>

<span data-ttu-id="5ea4d-113">`PasswordStrength` Řízení rozšiřuje textové pole a zkontroluje, zda je heslo v ní dostatečně funkční.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="5ea4d-114">Nabízí širokou řadu možností prostřednictvím atributy; Zde jsou jen některé z nich:</span><span class="sxs-lookup"><span data-stu-id="5ea4d-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="5ea4d-115">`MinimumNumericCharacters` minimální počet číselné znaků požadovaných v hesle</span><span class="sxs-lookup"><span data-stu-id="5ea4d-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="5ea4d-116">`MinimumSymbolCharacters` minimální počet znaků symbolu (ne písmen a číslic) v hesle</span><span class="sxs-lookup"><span data-stu-id="5ea4d-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="5ea4d-117">`PreferredPasswordLength` Minimální délka hesla</span><span class="sxs-lookup"><span data-stu-id="5ea4d-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="5ea4d-118">`RequiresUpperAndLowerCaseCharacters` jestli heslo musí používat velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="5ea4d-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="5ea4d-119">`StrengthIndicatorType` Poskytuje informace o tom, jak k dispozici síly hesla, jako text (hodnota `"Text"`) nebo jako druh indikátor průběhu (hodnota `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="5ea4d-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="5ea4d-120">V `DisplayPosition` atribut, můžete konfigurovat umístění zobrazení informací.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="5ea4d-121">Tady je kompletní příklad, včetně prvku ASP.NET AJAX `ScriptManager` ovládací prvek, `PasswordStrength` řízení a samozřejmě textového pole, které může uživatel zadat heslo.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="5ea4d-122">Pole pozdější formuláře je z důvodu ukázce běžného textového pole a pole hesla, aby mohli zobrazit během vývoje zadávané.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="5ea4d-123">Spuštění stránky a zadejte ji okamžitě: pouze po malá písmena, velká písmena, číslice a symboly, které jste zadali, se považuje jako nedělitelné heslo.</span><span class="sxs-lookup"><span data-stu-id="5ea4d-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="5ea4d-124">[![Teď heslo je vhodný pro (celkem)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5ea4d-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="5ea4d-125">Teď je heslo (celkem) vhodný ([Kliknutím zobrazit obrázek v plné velikosti](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5ea4d-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5ea4d-126">Předchozí</span><span class="sxs-lookup"><span data-stu-id="5ea4d-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
