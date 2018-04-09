---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Dynamicky naplnění ovládacího prvku s použitím kódu JavaScript (C#) | Microsoft Docs
author: wenz
description: DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c9fd11ea0348eb7fe9a7634f7b26031339146828
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Dynamicky naplnění ovládacího prvku s použitím kódu JavaScript (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky. Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta.


## <a name="overview"></a>Přehled

`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky. Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta.

## <a name="steps"></a>Kroky

Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulateExtender` ovládacího prvku. Webová služba implementuje metodu `getDate()` , očekává jeden argument typu řetězec, nazývá `contextKey`, vzhledem k tomu, `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby. Zde je kód (soubor `DynamicPopulate.cs.asmx`) který načte aktuální datum v jednom ze tří formátů:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

V dalším kroku vytvoří nový web s ASP.NET a spustit pomocí ovládacího prvku ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Potom přidat ovládací prvek popisek (například pomocí ovládacího prvku HTML se stejným názvem, nebo `<asp:Label />` ovládacího prvku webového) který později zobrazí výsledek volání webové služby.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

V dalším kroku zahrnout `DynamicPopulateExtender` řídit a poskytnout informace o webové služby, cílový ovládací prvek, ale ne název řídit, která aktivuje naplnění to bude provedeno později na použití vlastní JavaScript!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Teď do části JavaScript. `$find()` Funkce, které jsou definované knihovnu ASP.NET AJAX, vrátí odkaz na straně serveru objekty sady nástrojů ovládacího prvku ASP.NET AJAX, jako `DynamicPopulateExtender`. V aktuální soubor `$find("dpe")` vrátí odkaz na ten `DynamicPopulateExtender` ovládacího prvku stránce. Poskytuje metodu s názvem `populate()` které spustí proces dynamické naplnění. `populate()` Metoda vyžaduje jeden argument: klíč kontext, který bude sloužit jako argument `getDate()` webové metody. Tak například `$find("dpe").populate("format1")` by naplnit štítek s aktuální datum ve formátu měsíc den roku.

Aby bylo možné ukázku trochu flexibilnější, uživatel může zvolit teď několik formát data. Pro každou z nich se zobrazí přepínače. Jednou uživatel klikne na na přepínače, kódu jazyka JavaScript dynamicky naplní štítek s formátem vybraným datem. Zde jsou tyto přepínače:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Všimněte si, že se v kontextu přepínače, JavaScript výraz `this.value` odkazuje na hodnotu aktuální tlačítka, která se dělá jako přesně stejné informace `getDate()` metoda můžete pracovat.


[![Klikněte na tlačítko načte data ze serveru ve formátu určeném](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Klikněte na tlačítko načte data ze serveru ve formátu určeném ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-populating-a-control-cs.md)
> [další](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
