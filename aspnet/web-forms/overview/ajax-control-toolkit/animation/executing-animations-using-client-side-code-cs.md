---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Provádění animace pomocí kódu na straně klienta (C#) | Microsoft Docs
author: wenz
description: V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-c"></a>Provádění animace pomocí kódu na straně klienta (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací jednou uživatel klikne na panelu. Přidejte dva animací parallelly spouštění:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Z důvodu ukázku Tato animace (a všechny ostatní animace vytvořili pomocí sady nástrojů pro ovládací prvek) se spustí pomocí kódu jazyka JavaScript, po spuštění stránky. Nejprve všech budeme potřebovat přístup ke `AnimationExtender` ovládacího prvku. Poskytuje knihovnu ASP.NET AJAX `$find()` funkce pro tuto úlohu:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender` Ovládací prvek poskytuje bohaté rozhraní API, včetně metody s názvy identické obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`a tak dále. Volání pro instanci systému `OnClick()` metoda provádí animace v rámci `<OnClick>` element `AnimationExtender` ovládacího prvku:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Tady je dokončení kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile byla úplným načtením stránky Všimněte si, že `pageLoad()` se používá název funkce, kterému se říká pomocí prvku ASP.NET AJAX jednou stránky a zahrnuty všechny byly knihovny JavaScript načíst.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Animace se spustí okamžitě, bez klikněte na tlačítko myši ([Kliknutím zobrazit obrázek v plné velikosti](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](modifying-animations-from-the-server-side-cs.md)
> [další](changing-an-animation-using-client-side-code-cs.md)
