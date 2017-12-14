---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: "Manipulace s DropShadow vlastnosti z kódu klienta (C#) | Microsoft Docs"
author: wenz
description: "Přizpůsobení DataList úpravy rozhraní"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 59f7d4610ce610ef4357510f0e861f107278b5da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulace s DropShadow vlastnosti z kódu klienta (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.


## <a name="overview"></a>Přehled

DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.

## <a name="steps"></a>Kroky

Kód začíná panelu obsahující některé řádky textu:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Třída CSS, která přidružené dává panelu barvu pozadí dobrý:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender` Se přidá do rozšířit panel s efekt stínu rozevírací krytí nastavena na 50 %:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Potom prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Jiný panel obsahuje dva odkazy JavaScript pro nastavení krytí stín: odkaz znaménkem minus snižuje krytí bude stín, plus odkaz se zvyšuje.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Funkce JavaScript, která `changeOpacity()` musí nejprve vyhledejte `DropShadowExtender` ovládací prvek na stránce. Definuje prvku ASP.NET AJAX `$find()` metoda pro přesně tuto úlohu. Potom, `get_Opacity()` metoda načte aktuální krytí `set_Opacity()` metoda ji nastaví. Kód jazyka JavaScript pak vloží aktuální hodnota neprůhlednosti `<label>` element:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Krytí se změní na straně klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Krytí se změní na straně klienta ([Kliknutím zobrazit obrázek v plné velikosti](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](adjusting-the-z-index-of-a-dropshadow-cs.md)
[další](adjusting-the-z-index-of-a-dropshadow-vb.md)