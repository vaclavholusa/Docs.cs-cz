---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Testování síla hesla (C#) | Microsoft Docs
author: wenz
description: Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit. PasswordStrength ovládacího prvku ASP. N....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f4a7128f2edbef4fbe95faf9de19bdae5f436e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869215"
---
<a name="testing-the-strength-of-a-password-c"></a>Testování síla hesla (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit. PasswordStrength ovládacího prvku ASP.NET AJAX Control Toolkit můžete zkontrolovat, jak dobrý je heslo.


## <a name="overview"></a>Přehled

Hesla je nutná prakticky odkudkoli tak, aby opožděné uživatelé zpravidla zvolte jednoduchá hesla, které se dají snadno rozdělit. `PasswordStrength` Ovládací prvek v sadě nástrojů ovládacího prvku ASP.NET AJAX, můžete zkontrolovat, jak dobrý je heslo.

## <a name="steps"></a>Kroky

`PasswordStrength` Řízení rozšiřuje textové pole a zkontroluje, zda je heslo v ní dostatečně funkční. Nabízí širokou řadu možností prostřednictvím atributy; Zde jsou jen některé z nich:

- `MinimumNumericCharacters` minimální počet číselné znaků požadovaných v hesle
- `MinimumSymbolCharacters` minimální počet znaků symbolu (ne písmen a číslic) v hesle
- `PreferredPasswordLength` Minimální délka hesla
- `RequiresUpperAndLowerCaseCharacters` jestli heslo musí používat velká a malá písmena

`StrengthIndicatorType` Poskytuje informace o tom, jak k dispozici síly hesla, jako text (hodnota `"Text"`) nebo jako druh indikátor průběhu (hodnota `"BarIndicator"`). V `DisplayPosition` atribut, můžete konfigurovat umístění zobrazení informací. Tady je kompletní příklad, včetně prvku ASP.NET AJAX `ScriptManager` ovládací prvek, `PasswordStrength` řízení a samozřejmě textového pole, které může uživatel zadat heslo. Pole pozdější formuláře je z důvodu ukázce běžného textového pole a pole hesla, aby mohli zobrazit během vývoje zadávané.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Spuštění stránky a zadejte ji okamžitě: pouze po malá písmena, velká písmena, číslice a symboly, které jste zadali, se považuje jako nedělitelné heslo.


[![Teď heslo je vhodný pro (celkem)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Teď je heslo (celkem) vhodný ([Kliknutím zobrazit obrázek v plné velikosti](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](testing-the-strength-of-a-password-vb.md)
