---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Pomocí TextBoxWatermark s ovládacími prvky ověření (C#) | Microsoft Docs
author: wenz
description: TextBoxWatermark ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je možné...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Pomocí TextBoxWatermark s ovládacími prvky ověření (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> TextBoxWatermark ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je vyprázdnit. Pokud uživatel ponechá bez nutnosti zadávat text do pole, zobrazí se předem vyplněných text znovu. To může dojít ke konfliktu s ovládacími prvky ověřování ASP.NET na stejné stránce, ale mohou být tyto problémy překonat.


## <a name="overview"></a>Přehled

`TextBoxWatermark` Ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je vyprázdnit. Pokud uživatel ponechá bez nutnosti zadávat text do pole, zobrazí se předem vyplněných text znovu. To může dojít ke konfliktu s ovládacími prvky ověřování ASP.NET na stejné stránce, ale mohou být tyto problémy překonat.

## <a name="steps"></a>Kroky

Základní nastavení vzorku je následující: `TextBox` ovládací prvek je vodoznakem pomocí `TextBoxWatermarkExtender` ovládacího prvku. Tlačítko zpětné volání se aktivuje a bude možné později použít k aktivaci ověřovací ovládací prvky na stránce. Navíc `ScriptManager` řízení se vyžaduje k chybě při inicializaci prvku ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Nyní přidejte `RequiredFieldValidator` ovládací prvek, který kontroluje, zda je text v poli při odeslání formuláře. `InitialValue` Validátoru musí být nastavena na stejnou hodnotu, která se používá v `TextBoxWatermarkExtender` ovládacího prvku: po odeslání formuláře hodnota beze změny textové pole je hodnota vodoznaku v něm:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Ale je jedním z problémů s tímto přístupem: Jestliže klient zakáže JavaScript, není pole text předem s vodoznakového textu, proto `RequiredFieldValidator` neaktivuje chybovou zprávu. Proto druhý `RequiredFieldValidator` ovládací prvek je vyžadován, která vyhledává prázdné textové pole (vynechání `InitialValue` atribut).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Vzhledem k tomu použít i validátory `Display` = `"Dynamic"`, koncový uživatel nemůže odlišit od vzhled, které ze dvou validátory byla aktivována; místo toho to vypadá, došlo jenom jeden z nich.

Nakonec přidejte některé kódu na straně serveru k vypsání text v poli, pokud žádné program pro ověření objeví chybová zpráva:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Validátor complains, že neexistuje žádný text v poli](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Validátor complains, že neexistuje žádný text v poli ([Kliknutím zobrazit obrázek v plné velikosti](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-textboxwatermark-in-a-formview-cs.md)
> [další](using-textboxwatermark-in-a-formview-vb.md)
