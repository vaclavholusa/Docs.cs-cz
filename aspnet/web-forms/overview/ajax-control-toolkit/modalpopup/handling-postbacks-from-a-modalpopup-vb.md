---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: "Zpracování postback z ModalPopup (VB) | Microsoft Docs"
author: wenz
description: "ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Zvláštní pozornost musí být provedeny, když pos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 7915d555fef363f41aa51bd77f0183c97c2f3769
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Zpracování postback z ModalPopup (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Musí dát zvláštní pozor při zpětné volání z v místní nabídce.


## <a name="overview"></a>Přehled

ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Musí dát zvláštní pozor při zpětné volání z v místní nabídce.

## <a name="steps"></a>Kroky

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Dál přidejte panel, který slouží jako modální místní. Existuje může uživatel zadat název a e-mailovou adresu. Tlačítko se používá automaticky otevřeném okně zavřete a uložte informace. Všimněte si, že `OnClick` atribut je nastaven tak, aby zpětné volání nastane, když po kliknutí na toto tlačítko:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

Vlastní stránky se skládá ze dvou popisky přesně stejné informace: jméno a e-mailovou adresu. Tlačítko se používá k aktivaci Modální překryvné okno:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Aby bylo možné automaticky otevřeném okně se zobrazí, přidejte `ModalPopupExtender` ovládacího prvku. Nastavte `PopupControlID` atribut ID panelu a `TargetControlID` ID tlačítka:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Nyní vždy, když `Save` v překryvném okně modální po kliknutí na tlačítko, na straně serveru `SaveData()` metoda spuštěna. Zde můžete uložit zadané údaje v úložišti. Z důvodu zjednodušení je nová data právě výstup v popisku:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Ovládacích prvcích textbox v překryvném okně modální by měl být navíc vyplněn aktuální název a e-mailu. Ale to je nutné pouze v případě žádný postback. Pokud je zpětné volání, funkci ASP.NET viewstate automaticky vyplní textových polí s příslušnými hodnotami.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![Modální místní způsobí, že zpětné volání](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Modální místní způsobí postback ([Kliknutím zobrazit obrázek v plné velikosti](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](using-modalpopup-with-a-repeater-control-vb.md)
[další](positioning-a-modalpopup-vb.md)
