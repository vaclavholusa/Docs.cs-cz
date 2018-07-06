---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Změna animace klientským kódem (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 9711e93ee6f119ec1825e32bdad64535435970c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808953"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Změna animace klientským kódem (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Skutečné animace spustí HTML tlačítko:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Všimněte si, že neexistuje žádná `<Animations>` uzlu v rámci `AnimationExtender` ovládacího prvku. Vlastní kód jazyka JavaScript se používá k poskytování animace, který se má použít s ovládacím prvkem.

Stejně jako u serveru rozhraní API `AnimationExtender`, neexistuje žádný snadný způsob, jak přiřadit animace zařízení extender ještě. Ale zařízení extender zpřístupní ke čtení a zápisu animací několik metod zaregistrovaného na různé události (`OnClick`, `OnLoad`, a tak dále). Následuje několik příkladů:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Formát vrácené hodnoty `get_*()` funkce a formát argumentu pro `set_*()` funkce je řetězec formátu JSON, poskytuje reprezentaci objektu toho, co by být kód XML. V současné době neexistuje žádný způsob předat objekt, ale je možné číst objekt z dané animace (`get_OnXXXBehavior()` metody).

Tady je řetězec formátu JSON (bez oddělovací uvozovky a hezky formátovaný) představující animace aktivuje tlačítko, ale animace panelu podle jejich velikosti a mizení ve stejnou dobu:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Následující kód jazyka JavaScript přiřadí JSON descripting k `OnClick` animace aktuální zařízení extender a spustí ho:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky) ([kliknutím ji zobrazíte obrázek v plné velikosti](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](executing-animations-using-client-side-code-cs.md)
> [další](animating-an-updatepanel-control-cs.md)
