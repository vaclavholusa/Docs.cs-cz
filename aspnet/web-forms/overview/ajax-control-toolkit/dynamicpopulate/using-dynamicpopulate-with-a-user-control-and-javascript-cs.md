---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: DynamicPopulate pomocí uživatelského ovládacího prvku a JavaScript (C#) | Microsoft Docs
author: wenz
description: DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: cced645733375de7ab6235efa46b8d20ed262e50
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879566"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>DynamicPopulate pomocí uživatelského ovládacího prvku a JavaScript (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky. Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta. Zvláštní pozornost ale musí být provedeny, když rozšiřujícího objektu se nachází v uživatelského ovládacího prvku.


## <a name="overview"></a>Přehled

`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky. Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta. Zvláštní pozornost ale musí být provedeny, když rozšiřujícího objektu se nachází v uživatelského ovládacího prvku.

## <a name="steps"></a>Kroky

Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulateExtender` ovládacího prvku. Webová služba implementuje metodu `getDate()` , očekává jeden argument typu řetězec, nazývá `contextKey`, vzhledem k tomu, `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby. Zde je kód (soubor `DynamicPopulate.cs.asmx`) který načte aktuální datum v jednom ze tří formátů:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

V dalším kroku, vytvořte nový uživatelský ovládací prvek (`.ascx` soubor), označen následující deklarace v jeho první řádek:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt; element se použije k zobrazení dat ze serveru.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Také v řídicím souboru uživatele, budeme používat přepínače tři, každý z nich představující mezi tři možné data formátů podporovaných webovou službu. Když uživatel klikne na jednom z přepínačů, spustí prohlížeč kód JavaScript, který vypadá takto:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Tento kód přistupuje `DynamicPopulateExtender` (Nedělejte si starosti o neobvyklé ID ještě to se budeme později na) a aktivuje dynamické naplnění s daty. V kontextu aktuálního přepínač `this.value` odkazuje na jeho hodnotu, která je `format1`, `format2` nebo `format3` přesně co webové metody očekává.

Jediné, co chybí v ovládacím prvku uživatele ještě není `DynamicPopulateExtender` ovládací prvek, který odkazuje rádiová tlačítka na webovou službu.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Znovu si poznamenejte neobvyklé ID použité v ovládacím prvku: `mcd1$myDate` místo `myDate`. Dříve, kód jazyka JavaScript používá `mcd1_dpe1` k přístupu `DynamicPopulateExtender` místo `dpe1`. Tato strategie vytváření názvů je speciální požadavky při použití `DynamicPopulateExtender` v rámci uživatelského ovládacího prvku. Kromě toho budete muset Vložení kontrolních uživatele konkrétní způsobem, aby bylo fungovat. Vytvořte novou stránku ASP.NET a zaregistrovat předponu značky uživatelského ovládacího prvku, který jste právě implementovali:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Pak musíte uvést prvku ASP.NET AJAX `ScriptManager` ovládacího prvku na novou stránku:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Nakonec přidejte uživatelského ovládacího prvku na stránku. Budete muset nastavit jeho `ID` atribut (a `runat="server"`, samozřejmě), ale máte také nastavit na konkrétní název: `mcd1` vzhledem k tomu, že toto je Předpona použitá v rámci uživatelského ovládacího prvku pro přístup k ní pomocí jazyka JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

A je to! Stránce chová podle očekávání: uživatel klikne na jednom z přepínačů, ovládací prvek v sadě nástrojů volání webové služby a zobrazí aktuální datum v požadovaném formátu.


[![Přepínací tlačítka se nacházejí v uživatelského ovládacího prvku](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Přepínací tlačítka se nacházejí v uživatelského ovládacího prvku ([Kliknutím zobrazit obrázek v plné velikosti](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-populating-a-control-using-javascript-code-cs.md)
> [další](dynamically-populating-a-control-vb.md)
