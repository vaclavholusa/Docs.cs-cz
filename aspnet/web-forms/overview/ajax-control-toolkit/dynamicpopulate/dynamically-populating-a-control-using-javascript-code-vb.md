---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamické naplnění ovládacího prvku Javascriptovým kódem (VB) | Dokumentace Microsoftu
author: wenz
description: ASP.NET AJAX Control Toolkit ovládacího prvku DynamicPopulate volání webové služby (nebo metodu stránky) a vyplní výsledné hodnoty do cílového ovládacího prvku na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9df2a66bd49ecba52b0dd8b1d52a65b36c38a5dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366845"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>Dynamické naplnění ovládacího prvku Javascriptovým kódem (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky. Je také možné aktivovat naplnění psát vlastní kód JavaScript na straně klienta.


## <a name="overview"></a>Přehled

`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky. Je také možné aktivovat naplnění psát vlastní kód JavaScript na straně klienta.

## <a name="steps"></a>Kroky

Za prvé, třeba webové služby ASP.NET, která implementuje metodu, které jsou volány `DynamicPopulateExtender` ovládacího prvku. Webová služba implementuje metodu `getDate()` , která očekává jeden argument typu řetězec, volá `contextKey`, protože `DynamicPopulate` ovládací prvek odešle jednu část informací o kontextu se každé volání webové služby. Zde je kód (soubor `DynamicPopulate.vb.asmx`) načte aktuální datum v jednom ze tří formátů:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

V dalším kroku vytvoření nového webu technologie ASP.NET a začít pomocí ovládacího prvku ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

Pak přidejte ovládací prvek popisku (například pomocí ovládacího prvku HTML se stejným názvem, nebo `<asp:Label />` ovládací prvek webu) který se později zobrazí výsledek volání webové služby.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

V dalším kroku zahrnout `DynamicPopulateExtender` řídit a poskytnout informace o webových službách, cílový ovládací prvek, ale nikoli název ovládacího prvku, které aktivuje naplnění to se provede později pomocí vlastního jazyka JavaScript!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

Teď do části jazyka JavaScript. `$find()` Funkce definované knihovnou ASP.NET AJAX, jako například vrátí odkaz na serverové objekty technologie ASP.NET AJAX Control Toolkit `DynamicPopulateExtender`. V aktuálním souboru `$find("dpe")` vrátí odkaz na ten `DynamicPopulateExtender` ovládací prvek na stránce. Poskytuje metodu s názvem `populate()` který spustí proces dynamické naplnění. `populate()` Metoda vyžaduje jeden argument: kontext klíče, který bude sloužit jako argument `getDate()` webovou metodu. Takže například `$find("dpe").populate("format1")` by naplnit popisek s aktuální datum ve formátu měsíc roku dny.

Aby ukázku trochu více flexibilní, může uživatel teď vybrat mezi několika formátů data. Pro každou z nich zobrazí se přepínač. Jednou uživatel klikne na přepínač, kód jazyka JavaScript dynamicky naplní popisek s vybraným datem formátu. Tady jsou tyto přepínače:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

Všimněte si, že v rámci kontextu přepínací tlačítko, výraz jazyka JavaScript `this.value` odkazuje na hodnotu aktuální tlačítko, které je přesně stejné informace `getDate()` metoda můžete pracovat.


[![Klikněte na tlačítko načte data ze serveru ve formátu určeném](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

Klikněte na tlačítko načte data ze serveru ve formátu určeném ([kliknutím ji zobrazíte obrázek v plné velikosti](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-populating-a-control-vb.md)
> [další](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
