---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Předvolba seznamu položek s CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869891"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Předvolba seznamu položek s CascadingDropDown (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. Díky chvilku kódu je možné, že element seznamu je předem vybrali po dynamicky načíst data.


## <a name="overview"></a>Přehled

Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. (Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) Díky chvilku kódu je možné, že element seznamu je předem vybrali po dynamicky načíst data.

## <a name="steps"></a>Kroky

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Ovládací prvek rozevírací seznam je pak potřeba:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Pro tento seznam je přidána rozšiřujícího objektu CascadingDropDown poskytuje informace adresy URL a metoda webové služby:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Rozšiřujícího objektu CascadingDropDown pak asynchronní volání webové služby pomocí následující podpis metody:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Metoda vrátí pole typu CascadingDropDown hodnotu. Konstruktor typu očekává první popisek položky seznamu a potom hodnotu (HTML `value` atributu). Pokud třetí argument je nastaven na hodnotu true, v seznamu je element automaticky vybrán v prohlížeči.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Při načítání stránky v prohlížeči naplní rozevíracího seznamu s tři dodavateli, druhý probíhá předem vybrali.


[![Seznam se naplní a předem vybrali automaticky](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

Seznam se naplní a předem vybrali automaticky ([Kliknutím zobrazit obrázek v plné velikosti](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-cascadingdropdown-with-a-database-vb.md)
> [další](using-auto-postback-with-cascadingdropdown-vb.md)
