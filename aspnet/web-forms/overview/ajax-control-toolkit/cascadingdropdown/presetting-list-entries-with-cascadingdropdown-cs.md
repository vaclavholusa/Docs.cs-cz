---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Předvedení položek seznamu ovládacím prvkem CascadingDropDown (C#) | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 56093ad098d69039aafa9b4e6ba4e4c2ad91e49b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361996"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Předvedení položek seznamu ovládacím prvkem CascadingDropDown (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. Stačí nepatrné kódu je možné, že prvek seznamu je předem vybrali po dynamicky načtení dat.


## <a name="overview"></a>Přehled

Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. (Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) Stačí nepatrné kódu je možné, že prvek seznamu je předem vybrali po dynamicky načtení dat.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Ovládací prvek DropDownList je pak potřeba:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Pro tento seznam se přidá rozšíření CascadingDropDown poskytuje adresu URL a metoda informace o webových službách:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

Zařízení extender CascadingDropDown pak asynchronně volá webové služby s využitím následující podpis metody:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda vrátí pole typu hodnota CascadingDropDown. Konstruktor typu očekává, že nejprve titulek položky seznamu a pak hodnotu (HTML `value` atributu). Pokud třetí argument je nastaven na hodnotu true, v seznamu je element automaticky vybrán v prohlížeči.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Načítání stránky v prohlížeči vyplní rozevíracího seznamu, s třemi dodavateli, druhý se předem vybrali.


[![V seznamu je vyplněný a předem vybrali automaticky](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

V seznamu je vyplněný a předem vybrali automaticky ([kliknutím ji zobrazíte obrázek v plné velikosti](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-cascadingdropdown-with-a-database-cs.md)
> [další](using-auto-postback-with-cascadingdropdown-cs.md)
