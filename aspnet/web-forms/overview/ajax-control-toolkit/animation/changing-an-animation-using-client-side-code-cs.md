---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Změna animace pomocí kódu na straně klienta (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animace může taky...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f572efeb012d88ab15740bab7b0e4383572f3f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870645"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Změna animace pomocí kódu na straně klienta (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animace se změní taky pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Animace se změní taky pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Skutečné animace je spuštěn HTML tlačítko:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Všimněte si, že neexistuje žádná `<Animations>` uzel v rámci `AnimationExtender` ovládacího prvku. Vlastní kód JavaScript slouží k poskytování animací, který se má použít s ovládacím prvkem.

Stejně jako u rozhraní API serveru z `AnimationExtender`, neexistuje žádný snadný způsob, jak přiřadit animace rozšiřujícího objektu ještě. Ale rozšiřujícího objektu vystavit několik metod, jak číst a zapisovat animací zaregistrována různé události (`OnClick`, `OnLoad`a tak dále). Následuje několik příkladů:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Formát vrácenou hodnotu `get_*()` funkce a formát argument pro `set_*()` funkce je řetězec formátu JSON, poskytuje reprezentaci objektu z co by být značek XML. V současné době neexistuje žádný způsob, jak předat objekt v, ale je možné načíst objekt z daného animace (`get_OnXXXBehavior()` metody).

Tady je řetězec formátu JSON (bez uvozovek rozdělujících a přehledně naformátovaná) představující animace aktivuje tlačítko, ale animace panelu změny velikosti a roztmívání ve stejnou dobu:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Následující kód v JavaScriptu přiřadí JSON descripting k `OnClick` animace aktuální rozšiřujícího objektu a spustí ho:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![Animace se spustí okamžitě, bez kliknutí myši (a s velmi malé značek)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Animace se spustí okamžitě, bez klikněte na tlačítko myši (a s velmi malé značek) ([Kliknutím zobrazit obrázek v plné velikosti](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](executing-animations-using-client-side-code-cs.md)
> [další](animating-an-updatepanel-control-cs.md)
