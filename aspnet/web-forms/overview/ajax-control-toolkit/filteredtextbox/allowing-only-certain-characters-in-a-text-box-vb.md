---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Povolení určitých znaků v textovém poli (VB) | Dokumentace Microsoftu
author: wenz
description: Validačních ovládacích prvků technologie ASP.NET můžete zajistit, že jsou povolené jenom některé znaky ve vstupu uživatele. Ale to stále nezabrání uživatelům zadáte neplatný...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: e53d7282237c94b88c712e53bf607dcb94ccf1f2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830743"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Povolení určitých znaků v textovém poli (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Validačních ovládacích prvků technologie ASP.NET můžete zajistit, že jsou povolené jenom některé znaky ve vstupu uživatele. Ale to stále nezabrání uživatelům zadáte neplatné znaky pokus o odeslání formuláře.


## <a name="overview"></a>Přehled

Validačních ovládacích prvků technologie ASP.NET můžete zajistit, že jsou povolené jenom některé znaky ve vstupu uživatele. Ale to stále nezabrání uživatelům zadáte neplatné znaky pokus o odeslání formuláře.

## <a name="steps"></a>Kroky

ASP.NET AJAX Control Toolkit obsahuje `FilteredTextBox` ovládací prvek, který rozšiřuje textové pole. Po aktivaci, můžete do pole zadat pouze určitou sadu znaků.

Aby to fungovalo, nejprve musíme obvyklým technologie ASP.NET AJAX `ScriptManager` což způsobí načtení knihovny JavaScript, které jsou také používány ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Pak potřebujeme textové pole:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Nakonec `FilteredTextBoxExtender` postará o omezení znaků, které uživatel může na typ ovládacího prvku. Nejprve nastavte `TargetControlID` atribut `ID` z `TextBox` ovládacího prvku. Potom vyberte jednu z dostupných `FilterType` hodnoty:

- `Custom` výchozí. je nutné zadat seznam platné znaky
- `LowercaseLetters` jenom malá písmena.
- `Numbers` pouze číslice
- `UppercaseLetters` jenom velkými písmeny

Pokud `Custom FilterType` se používá, `ValidChars` vlastnost musí být nastavena a zadat seznam znaků, které může být zadán. Mimochodem: Pokud se pokusíte vložit text do textového pole, se odeberou všechny neplatné znaky.

Tady je zápis `FilteredTextBoxExtender` ovládací prvek, který umožňuje pouze číslice (něco, co by také bylo možné s `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Spustit na stránku a zkuste zadat písmeno, pokud je povolen jazyk JavaScript, nebude fungovat; na stránce se ale zobrazí číslic. Ale Všimněte si, že ochranu `FilteredTextBox` poskytuje není odrážky testování: Pokud jazyk JavaScript je povolený, žádná data můžete zadat do textového pole, proto budete muset použít další ověřovací prostředky, například ASP. Ovládací prvky ověřování vaší sítě.


[![Můžete zadat pouze číslice](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Můžete zadat pouze číslice ([kliknutím ji zobrazíte obrázek v plné velikosti](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](allowing-only-certain-characters-in-a-text-box-cs.md)
