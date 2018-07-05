---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Použití funkce Auto-Postback ovládacím prvkem CascadingDropDown (C#) | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: f1e0416a9c45cc0478a0179a4eef1788b7e2a8a5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384745"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Použití funkce Auto-Postback ovládacím prvkem CascadingDropDown (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. Ale při použití ovládacího prvku CascadingDropDown ASP. Prvek DropDownList NET AutoPostBack funkce nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, samotného. Pomocí kódu jazyka JavaScript se můžete vyhnout tomuto chování.


## <a name="overview"></a>Přehled

Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. (Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) Ale při použití ovládacího prvku CascadingDropDown ASP. Prvek DropDownList NET AutoPostBack funkce nefunguje, protože asynchronní načítání dat do seznamu generuje (nepotřebné) zpětné volání, samotného. Pomocí kódu jazyka JavaScript se můžete vyhnout tomuto chování.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci &lt; `form` &gt; element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Ovládací prvek DropDownList je pak potřeba:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Pro tento seznam se přidá rozšíření CascadingDropDown poskytuje adresu URL a metoda informace o webových službách:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Zařízení extender CascadingDropDown pak asynchronně volá webové služby s využitím následující podpis metody:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda vrátí pole typu hodnota CascadingDropDown. Konstruktor typu očekává, že nejprve titulek položky seznamu a pak hodnotu (HTML `value` atributu).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Načítání stránky v prohlížeči vyplní rozevíracího seznamu, s třemi dodavateli, druhý se předem vybrali. Také definuje ASP.NET `__doPostBack()` metodu JavaScript. Po načtení stránky toto volání JavaScript se přidá do rozevíracího seznamu, pouze pokud je ale prvky v ní. Pokud v seznamu nejsou žádné elementy, Control Toolkit je aktuálně se načítá, tak, aby kód jazyka JavaScript používá vypršení časového limitu a pokusí se znovu za půl sekundy.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Tímto způsobem, zpětné volání je pouze spuštěn, když v seznamu nejsou ve skutečnosti elementy a uživatel vybere záznam.


[![Výběr prvku seznamu vyvolá zpětné volání](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Výběr prvku seznamu vyvolá zpětné volání ([kliknutím ji zobrazíte obrázek v plné velikosti](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](presetting-list-entries-with-cascadingdropdown-cs.md)
> [další](filling-a-list-using-cascadingdropdown-vb.md)
