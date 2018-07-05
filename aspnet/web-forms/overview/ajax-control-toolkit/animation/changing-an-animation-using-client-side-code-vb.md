---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Změna animace klientským kódem (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8ca2c962c5ebe5e0c45d5b575031ada3e64acd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386744"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Změna animace klientským kódem (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Animace lze také změnit pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Skutečné animace spustí HTML tlačítko:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Všimněte si, že neexistuje žádná `<Animations>` uzlu v rámci `AnimationExtender` ovládacího prvku. Vlastní kód jazyka JavaScript se používá k poskytování animace, který se má použít s ovládacím prvkem.

Stejně jako u serveru rozhraní API `AnimationExtender`, neexistuje žádný snadný způsob, jak přiřadit animace zařízení extender ještě. Ale zařízení extender zpřístupní ke čtení a zápisu animací několik metod zaregistrovaného na různé události (`OnClick`, `OnLoad`, a tak dále). Následuje několik příkladů:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Formát vrácené hodnoty `get_*()` funkce a formát argumentu pro `set_*()` funkce je řetězec formátu JSON, poskytuje reprezentaci objektu toho, co by být kód XML. V současné době neexistuje žádný způsob předat objekt, ale je možné číst objekt z dané animace (`get_OnXXXBehavior()` metody).

Tady je řetězec formátu JSON (bez oddělovací uvozovky a hezky formátovaný) představující animace aktivuje tlačítko, ale animace panelu podle jejich velikosti a mizení ve stejnou dobu:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Následující kód jazyka JavaScript přiřadí JSON descripting k `OnClick` animace aktuální zařízení extender a spustí ho:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Animace se spustí okamžitě, bez kliknutí myší (a s velmi málo značky) ([kliknutím ji zobrazíte obrázek v plné velikosti](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](executing-animations-using-client-side-code-vb.md)
> [další](animating-an-updatepanel-control-vb.md)
