---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Spuštění animací klientským kódem (C#) | Dokumentace Microsoftu
author: wenz
description: Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5dc5c0b49a3530988bf42d6d632a061622f0a217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753062"
---
<a name="executing-animations-using-client-side-code-c"></a>Spuštění animací klientským kódem (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace může také aktivovat pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

Animace ovládacího prvku ASP.NET AJAX Control Toolkit je právě ovládacího prvku, ale celé rozhraní pro přidání animace k ovládacímu prvku. Spuštění animace může také aktivovat pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

Za prvé, zahrnout `ScriptManager` na stránce; potom technologie ASP.NET AJAX je načíst knihovnu, což umožňuje použití Control Toolkit:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animace se použijí pro panel text, který vypadá takto:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

V přidružené třídy šablony stylů CSS pro panel definovat barvu pozadí nice a také nastavit Pevná šířka panelu:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Pak přidejte `AnimationExtender` na stránku, poskytování `ID`, `TargetControlID` atribut a povinný údaj `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

V rámci `<Animations>` uzlu, použijte `<OnClick>` spuštění animací uživatel klikne na tlačítko na panelu. Přidejte dva animace parallelly provádět:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Představu Tato animace (a jakékoli jiné animace vytvořené pomocí Control Toolkit) je provedeno pomocí kódu jazyka JavaScript, po spuštění stránky. Za prvé jsme potřebují přístup k `AnimationExtender` ovládacího prvku. Poskytuje knihovna ASP.NET AJAX `$find()` funkce pro tuto úlohu:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender` Ovládací prvek poskytuje plnohodnotné rozhraní API, včetně metod s názvy shodné s obslužné rutiny událostí používá v kódu XML: `OnClick()`, `OnLoad()`, a tak dále. Například volání `OnClick()` spuštění animace v rámci metody `<OnClick>` elementu `AnimationExtender` ovládacího prvku:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Tady je kompletní kód JavaScript na straně klienta, který emuluje klikněte na panelu, jakmile úplným načtením stránky, Všimněte si, že `pageLoad()` se používá název funkce, které je voláno rozhraním ASP.NET AJAX jednou na stránce a zahrnuty všechny byly knihoven jazyka JavaScript načíst.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Animace se spustí okamžitě, bez kliknutí myší](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Animace se spustí okamžitě, bez kliknutí myší ([kliknutím ji zobrazíte obrázek v plné velikosti](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](modifying-animations-from-the-server-side-cs.md)
> [další](changing-an-animation-using-client-side-code-cs.md)
