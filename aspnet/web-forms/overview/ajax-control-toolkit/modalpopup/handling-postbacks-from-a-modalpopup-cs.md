---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Zpracování postbacků z ovládacího prvku ModalPopup (C#) | Dokumentace Microsoftu
author: wenz
description: Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Zvláštní pozornost musí být provedeny, když pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c5c3b573b62d779ab09caad22b0c0e3a6995634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399065"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Zpracování postbacků z ovládacího prvku ModalPopup (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Při zpětném odeslání z rozbalené, musí být přijata zvláštní pozornost.


## <a name="overview"></a>Přehled

Ovládací prvek ovládacího prvku ModalPopup v sadou nástrojů AJAX Control Toolkit nabízí jednoduchý způsob, jak vytvořit modální místní nabídky pomocí znamená, že na straně klienta. Při zpětném odeslání z rozbalené, musí být přijata zvláštní pozornost.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

V dalším kroku přidáte panel, který slouží jako modální místní nabídky. Uživatel může existuje, zadejte název a e-mailovou adresu. Tlačítko slouží k automaticky otevírané okno zavřete a uložte informace. Všimněte si, `OnClick` atribut je nastaven tak, aby zpětné odeslání vyvolá se při kliknutí na toto tlačítko:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Samotná stránka se skládá ze dvou popisků stejné informace: jméno a e-mailovou adresu. Tlačítko se používá k aktivaci Modální místní nabídky:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Aby bylo možné automaticky otevírané okno se zobrazí, přidejte `ModalPopupExtender` ovládacího prvku. Nastavte `PopupControlID` atribut ID v panelu a `TargetControlID` ID tlačítka:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Nyní pokaždé, když `Save` je stisknuto tlačítko Modální místní nabídky, server-side `SaveData()` provedení metody. V úložišti dat, můžete ušetřit zadané údaje. Z důvodu zjednodušení nová data jenom výstup v popisku:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

TextBox – ovládací prvky v rámci Modální místní nabídky by měl být také, zaplněny aktuální jméno a e-mailu. Ale to je nutné pouze dojde bez zpětného odeslání. Pokud je zpětné volání, funkce stav zobrazení ASP.NET automaticky vyplnit textová pole s příslušnými hodnotami.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![Vyvolá Modální místní nabídky zpětné volání](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Vyvolá Modální místní nabídky zpětné volání ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-modalpopup-with-a-repeater-control-cs.md)
> [další](positioning-a-modalpopup-cs.md)
