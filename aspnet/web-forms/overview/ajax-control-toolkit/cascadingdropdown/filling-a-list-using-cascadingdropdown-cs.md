---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Naplnění seznamu pomocí CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870970"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Naplnění seznamu pomocí CascadingDropDown (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. (Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) V prvním kroku k vyřešení je ve skutečnosti vyplnění pomocí tento ovládací prvek rozevírací seznam.


## <a name="overview"></a>Přehled

Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam. (Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) V prvním kroku k vyřešení je ve skutečnosti vyplnění pomocí tento ovládací prvek rozevírací seznam.

## <a name="steps"></a>Kroky

Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Ovládací prvek rozevírací seznam je pak potřeba:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Pro tento seznam se přidá rozšiřujícího objektu CascadingDropDown. K webové službě, která pak vrátí seznam položek, který se má zobrazit v seznamu, kterou bude odesílat Asynchronní požadavek. Tento postup vyžaduje je nutné následující atributy CascadingDropDown nastavit:

- `ServicePath`: Adresa URL webové služby doručování položky seznamu
- `ServiceMethod`: Webové metody doručování položky seznamu
- `TargetControlID`: ID rozevíracího seznamu
- `Category`: Informace o kategoriích, které je odeslána do webové metody při volání
- `PromptText`: Text zobrazí v případě, že asynchronní načítání seznamu dat ze serveru

Zde je kód pro `CascadingDropDown` elementu. Jediným rozdílem mezi C# a VB je název přidružené webové služby:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Kód jazyka JavaScript pocházejících z `CascadingDropDown` rozšiřujícího objektu volá metody webové služby s podpisem následující:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Proto je důležitým aspektem, že metoda musí vrátit pole typu `CascadingDropDownNameValue` (definovanou sadu ovládacího prvku ASP.NET AJAX). V `CascadingDropDownNameValue` contructor, je třeba zadat text položky seznamu a pak jeho hodnoty, první stejně jako `<option value="VALUE">NAME</option>` by se ve formátu HTML. Tady je několik ukázkových dat:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Při načítání stránky v prohlížeči aktivuje se v seznamu pro vyplnění pomocí tří dodavatelů.


[![V seznamu se vyplní automaticky](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

V seznamu se vyplní automaticky ([Kliknutím zobrazit obrázek v plné velikosti](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-cascadingdropdown-with-a-database-cs.md)
