---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulace s DropShadow vlastnosti z kódu klienta (VB) | Microsoft Docs
author: wenz
description: DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Vlastnosti této rozšiřujícího objektu se změní taky pomocí klienta JavaScrip...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870905"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipulace s DropShadow vlastnosti z kódu klienta (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.


## <a name="overview"></a>Přehled

DropShadow ovládacího prvku Toolkitu AJAX rozšiřuje panelu s stínu. Vlastnosti této rozšiřujícího objektu můžete změnit také pomocí kód JavaScript klienta.

## <a name="steps"></a>Kroky

Kód začíná panelu obsahující některé řádky textu:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Třída CSS, která přidružené dává panelu barvu pozadí dobrý:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender` Se přidá do rozšířit panel s efekt stínu rozevírací krytí nastavena na 50 %:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Potom prvku ASP.NET AJAX `ScriptManager` řízení umožňuje Toolkitu postup:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Jiný panel obsahuje dva odkazy JavaScript pro nastavení krytí stín: odkaz znaménkem minus snižuje krytí bude stín, plus odkaz se zvyšuje.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Funkce JavaScript, která `changeOpacity()` musí nejprve vyhledejte `DropShadowExtender` ovládací prvek na stránce. Definuje prvku ASP.NET AJAX `$find()` metoda pro přesně tuto úlohu. Potom, `get_Opacity()` metoda načte aktuální krytí `set_Opacity()` metoda ji nastaví. Kód jazyka JavaScript pak vloží aktuální hodnota neprůhlednosti `<label>` element:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Krytí se změní na straně klienta](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Krytí se změní na straně klienta ([Kliknutím zobrazit obrázek v plné velikosti](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](adjusting-the-z-index-of-a-dropshadow-vb.md)
