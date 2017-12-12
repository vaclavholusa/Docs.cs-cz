---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "Povolení jenom určitých znaků v textovém poli (VB) | Microsoft Docs"
author: wenz
description: "Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáte neplatný..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Povolení jenom určitých znaků v textovém poli (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.


## <a name="overview"></a>Přehled

Ovládací prvky ASP.NET ověření můžete zajistit, že jsou povolené jenom některé znaky vstup uživatele. Ale to stále nebrání uživatelům z zadáním neplatné znaky a pokusu o odeslání formuláře.

## <a name="steps"></a>Kroky

Obsahuje sadu ovládacího prvku ASP.NET AJAX `FilteredTextBox` řídit, která rozšiřuje textové pole. Po aktivaci, může je třeba zadat pouze určitou sadu znaků do pole.

Tento postup vyžaduje, je nejprve nutné jako obvykle prvku ASP.NET AJAX `ScriptManager` což způsobí načtení knihoven jazyka JavaScript, které jsou také používány Toolkitu ASP.NET AJAX:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Potom potřebujeme textové pole:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Nakonec `FilteredTextBoxExtender` řízení postará znaky má uživatel na typ omezení. Nastavte nejprve, `TargetControlID` atribut `ID` z `TextBox` ovládacího prvku. Potom vyberte jednu z dostupných `FilterType` hodnoty:

- `Custom`Výchozí; je nutné zadat seznam platný znaků
- `LowercaseLetters`jenom malá písmena
- `Numbers`pouze číslice
- `UppercaseLetters`jenom velká písmena

Pokud `Custom FilterType` se používá, `ValidChars` vlastnost musí být nastavené a zadat seznam znaky, které může být typu. Tím: Pokud se pokusíte vložit text do textového pole, se odeberou všechny neplatné znaky.

Zde je kód pro `FilteredTextBoxExtender` ovládací prvek, který umožňuje pouze číslice (něco, co by také bylo umožněno s `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Spustit na stránku a zkuste zadat písmeno, pokud je povolen jazyk JavaScript, nebude fungovat; číslic se ale zobrazí na stránce. Ale Všimněte si, že ochranu `FilteredTextBox` poskytuje není odrážka ověření: Pokud JavaScript je povolena, všechna data lze zadat do textového pole, budete muset použít znamená další ověřování, tj. ASP. Ovládací prvky NET na ověření.


[![Lze zadat pouze číslice](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Lze zadat pouze číslice ([Kliknutím zobrazit obrázek v plné velikosti](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](allowing-only-certain-characters-in-a-text-box-cs.md)
