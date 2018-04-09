---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Povolení jenom určitých znaků v textovém poli (C#) | Microsoft Docs
author: wenz
description: Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáte neplatný...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d2ffc4b741bd0c7f9c456b6e76017f5350ab6378
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Povolení jenom určitých znaků v textovém poli (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.


## <a name="overview"></a>Přehled

Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.

## <a name="steps"></a>Kroky

Obsahuje sadu ovládacího prvku ASP.NET AJAX `FilteredTextBox` řídit, která rozšiřuje textové pole. Po aktivaci, může je třeba zadat pouze určitou sadu znaků do pole.

Tento postup vyžaduje, je nejprve nutné jako obvykle prvku ASP.NET AJAX `ScriptManager` což způsobí načtení knihoven jazyka JavaScript, které jsou také používány Toolkitu ASP.NET AJAX:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Potom potřebujeme textové pole:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Nakonec `FilteredTextBoxExtender` řízení postará znaky má uživatel na typ omezení. Nastavte nejprve, `TargetControlID` atribut `ID` z `TextBox` ovládacího prvku. Potom vyberte jednu z dostupných `FilterType` hodnoty:

- `Custom` Výchozí; je nutné zadat seznam platný znaků
- `LowercaseLetters` jenom malá písmena
- `Numbers` pouze číslice
- `UppercaseLetters` jenom velká písmena

Pokud `Custom FilterType` se používá, `ValidChars` vlastnost musí být nastavené a zadat seznam znaky, které může být typu. Tím: Pokud se pokusíte vložit text do textového pole, se odeberou všechny neplatné znaky.

Zde je kód pro `FilteredTextBoxExtender` ovládací prvek, který umožňuje pouze číslice (něco, co by také bylo umožněno s `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Spustit na stránku a zkuste zadat písmeno, pokud je povolen jazyk JavaScript, nebude fungovat; číslic se ale zobrazí na stránce. Ale Všimněte si, že ochranu `FilteredTextBox` poskytuje není odrážka ověření: Pokud JavaScript je povolena, všechna data lze zadat do textového pole, budete muset použít znamená další ověřování, tj. ASP. Ovládací prvky NET na ověření.


[![Lze zadat pouze číslice](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Lze zadat pouze číslice ([Kliknutím zobrazit obrázek v plné velikosti](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](allowing-only-certain-characters-in-a-text-box-vb.md)
