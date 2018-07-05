---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Vyplnění seznamu ovládacím prvkem CascadingDropDown (VB) pomocí | Dokumentace Microsoftu
author: wenz
description: Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: edd6c330d21a7875b8f90fcf9bbde75978b6d08d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389596"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>Vyplnění seznamu ovládacím prvkem použití ovládacího prvku CascadingDropDown (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. (Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) První výzva k řešení je pro skutečné vyplnění rozevíracího seznamu pomocí tohoto ovládacího prvku.


## <a name="overview"></a>Přehled

Sada nástrojů AJAX Control Toolkit ovládacího prvku CascadingDropDown rozšiřuje ovládací prvek DropDownList tak, aby se změny v jedné DropDownList zatížení související hodnoty v jiném DropDownList. (Například jeden seznam obsahuje seznam nám stavy a dalším seznamu je pak vyplněna hlavních měst v tomto stavu.) První výzva k řešení je pro skutečné vyplnění rozevíracího seznamu pomocí tohoto ovládacího prvku.

## <a name="steps"></a>Kroky

K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Ovládací prvek DropDownList je pak potřeba:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Pro tento seznam je přidán CascadingDropDown rozšíření. Asynchronní požadavek se odešle do webové služby, která pak vrátí seznam položek, které lze zobrazit v seznamu. Aby to fungovalo nutné nastavit následující atributy CascadingDropDown:

- `ServicePath`: Adresa URL webová služba doručování položky seznamu
- `ServiceMethod`: Metoda webové zajištění položky seznamu
- `TargetControlID`: ID z rozevíracího seznamu
- `Category`: Informace o kategoriích, které je odeslána do webové metody při volání
- `PromptText`: Text zobrazovaný v případě asynchronní načítání seznamu data ze serveru

Tady je zápis `CascadingDropDown` elementu. Jediným rozdílem mezi C# a VB je název přidružené webové služby:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Z kódu jazyka JavaScript `CascadingDropDown` volání rozšiřující metody webové služby s následující signaturou:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Proto je důležitým aspektem je, že metoda musí vracet pole typu `CascadingDropDownNameValue` (definované technologie ASP.NET AJAX Control Toolkit). V `CascadingDropDownNameValue` konstruktoru, první text položky seznamu a pak její hodnotu musí být zadaná, stejně jako `<option value="VALUE">NAME</option>` byste udělali ve formátu HTML. Tady je ukázková data:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Načítání stránky v prohlížeči se aktivuje seznamu tankujeme tři dodavatelů.


[![V seznamu se vyplní automaticky](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

V seznamu se vyplní automaticky ([kliknutím ji zobrazíte obrázek v plné velikosti](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-auto-postback-with-cascadingdropdown-cs.md)
> [další](using-cascadingdropdown-with-a-database-vb.md)
