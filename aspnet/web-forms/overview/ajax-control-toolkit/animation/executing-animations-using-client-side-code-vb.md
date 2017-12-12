---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "Provádění animace pomocí kódu na straně klienta (VB) | Microsoft Docs"
author: wenz
description: "V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a>Provádění animace pomocí kódu na straně klienta (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

V sadě nástrojů ovládacího prvku ASP.NET AJAX ovládacího prvku animace není právě ovládací prvek ale celé rozhraní pro přidání do ovládacího prvku animace. Spuštění animace mohou být vyvolány také pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

První řadě zahrnují `ScriptManager` na stránce; potom knihovny ASP.NET AJAX je načtena, aby bylo možné použít Toolkitu:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Animace se použijí na panel textu, který vypadá takto:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Související třídy CSS pro panel definovat barvu pozadí dobrý a také nastavit pevnou šířku pro panel:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Poté, přidejte `AnimationExtender` na stránku, poskytuje `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací jednou uživatel klikne na panelu. Přidejte dva animací parallelly spouštění:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Z důvodu ukázku Tato animace (a všechny ostatní animace vytvořili pomocí sady nástrojů pro ovládací prvek) se spustí pomocí kódu jazyka JavaScript, po spuštění stránky. Nejprve všech budeme potřebovat přístup ke `AnimationExtender` ovládacího prvku. Poskytuje knihovnu ASP.NET AJAX `$find()` funkce pro tuto úlohu:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender` Ovládací prvek poskytuje bohaté rozhraní API, včetně metody s názvy identické obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`a tak dále. Volání pro instanci systému `OnClick()` metoda provádí animace v rámci `<OnClick>` element `AnimationExtender` ovládacího prvku:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Tady je dokončení kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile byla úplným načtením stránky Všimněte si, že `pageLoad()` se používá název funkce, kterému se říká pomocí prvku ASP.NET AJAX jednou stránky a zahrnuty všechny byly knihovny JavaScript načíst.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Animace se spustí okamžitě, bez klikněte na tlačítko myši ([Kliknutím zobrazit obrázek v plné velikosti](executing-animations-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](modifying-animations-from-the-server-side-vb.md)
[další](changing-an-animation-using-client-side-code-vb.md)
