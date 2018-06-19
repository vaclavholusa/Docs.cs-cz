---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Pomocí automatického provedení operace CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 04e0914dd1057f9ce490f68ae3fa9c56766beafb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872465"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Pomocí automatického provedení operace CascadingDropDown (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. Ale při použití ovládacího prvku CascadingDropDown ASP. Funkce Automatické odeslání NET na rozevírací seznam řízení nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, sám sebe. S určitý kód JavaScript se vyhnout této vliv.


## <a name="overview"></a>Přehled

Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. (Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) Ale při použití ovládacího prvku CascadingDropDown ASP. Funkce Automatické odeslání NET na rozevírací seznam řízení nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, sám sebe. S určitý kód JavaScript se vyhnout této vliv.

## <a name="steps"></a>Kroky

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř &lt; `form` &gt; element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Ovládací prvek rozevírací seznam je pak potřeba:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Pro tento seznam je přidána rozšiřujícího objektu CascadingDropDown poskytuje informace adresy URL a metoda webové služby:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Rozšiřujícího objektu CascadingDropDown pak asynchronní volání webové služby pomocí následující podpis metody:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda vrátí pole typu CascadingDropDown hodnotu. Konstruktor typu očekává první popisek položky seznamu a potom hodnotu (HTML `value` atributu).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Při načítání stránky v prohlížeči naplní rozevíracího seznamu s tři dodavateli, druhý probíhá předem vybrali. Také definuje ASP.NET `__doPostBack()` metodu JavaScript. Po načtení stránky toto volání jazyka JavaScript se přidá do rozevíracího seznamu, pouze pokud je ale elementy v ní. Pokud v seznamu nejsou žádné elementy, Toolkitu je aktuálně načítání, tak, aby kód jazyka JavaScript používá vypršení časového limitu a pokusí se znovu za půl sekundy.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Tímto způsobem zpětné volání je spustit pouze když existuje ve skutečnosti prvků v seznamu a uživatel vybere položku.


[![Když vyberete prvek seznamu, budou zpětné volání](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Výběr prvku seznamu způsobí, že zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](presetting-list-entries-with-cascadingdropdown-cs.md)
> [další](filling-a-list-using-cascadingdropdown-vb.md)
