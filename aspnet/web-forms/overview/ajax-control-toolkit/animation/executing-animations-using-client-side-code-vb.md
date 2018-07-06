---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Spuštění animací klientským kódem (VB) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b180ad17e0e3d2dffa6262d10a83a8353e0a1d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811883"
---
<a name="executing-animations-using-client-side-code-vb"></a>Spuštění animací klientským kódem (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace může také aktivovat pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace může také aktivovat pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací uživatel klikne na tlačítko na panelu. Přidejte dva animace parallelly provádět:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Představu Tato animace (a jakékoli jiné animace vytvořené pomocí Control Toolkit) je provedeno pomocí kódu jazyka JavaScript, po spuštění stránky. Za prvé jsme potřebují přístup k `AnimationExtender` ovládacího prvku. Poskytuje knihovna ASP.NET AJAX `$find()` funkce pro tuto úlohu:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender` Ovládací prvek poskytuje plnohodnotné rozhraní API, včetně metod s názvy shodné s obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`, a tak dále. Například volání `OnClick()` spuštění animace v rámci metody `<OnClick>` elementu `AnimationExtender` ovládacího prvku:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Tady je kompletní kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile úplným načtením stránky, Všimněte si, že `pageLoad()` se používá název funkce, které je voláno rozhraním ASP.NET AJAX jednou na stránce a zahrnuty všechny byly knihoven jazyka JavaScript načíst.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Animace se spustí okamžitě, bez kliknutí myší ([kliknutím ji zobrazíte obrázek v plné velikosti](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](modifying-animations-from-the-server-side-vb.md)
> [další](changing-an-animation-using-client-side-code-vb.md)
