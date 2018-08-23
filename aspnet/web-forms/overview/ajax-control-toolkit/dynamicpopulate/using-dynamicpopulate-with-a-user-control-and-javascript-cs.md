---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Použití ovládacího prvku DynamicPopulate s uživatelského ovládacího prvku a JavaScriptem (C#) | Dokumentace Microsoftu
author: wenz
description: ASP.NET AJAX Control Toolkit ovládacího prvku DynamicPopulate volání webové služby (nebo metodu stránky) a vyplní výsledné hodnoty do cílového ovládacího prvku na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 110f6dd05d038438bc061d3ee907a5e2da8968c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756734"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Použití ovládacího prvku DynamicPopulate s uživatelského ovládacího prvku a JavaScriptem (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky. Je také možné aktivovat naplnění psát vlastní kód JavaScript na straně klienta. Zvláštní pozornost má ale mají být provedeny, když zařízení extender se nachází v uživatelském ovládacím prvku.


## <a name="overview"></a>Přehled

`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky. Je také možné aktivovat naplnění psát vlastní kód JavaScript na straně klienta. Zvláštní pozornost má ale mají být provedeny, když zařízení extender se nachází v uživatelském ovládacím prvku.

## <a name="steps"></a>Kroky

Za prvé, třeba webové služby ASP.NET, která implementuje metodu, které jsou volány `DynamicPopulateExtender` ovládacího prvku. Webová služba implementuje metodu `getDate()` , která očekává jeden argument typu řetězec, volá `contextKey`, protože `DynamicPopulate` ovládací prvek odešle jednu část informací o kontextu se každé volání webové služby. Zde je kód (soubor `DynamicPopulate.cs.asmx`) načte aktuální datum v jednom ze tří formátů:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

V dalším kroku vytvoření nového uživatelského ovládacího prvku (`.ascx` souboru), označen následující deklarace v jeho první řádek:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt; element se použije k zobrazení dat pocházejících ze serveru.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

V souboru uživatelského ovládacího prvku, jsme také pomocí přepínačů tři, každý z nich představuje jednu ze tří možných datum formátů podporovaných webovou službu. Když uživatel klikne na jednom z přepínačů, prohlížeč provede kód jazyka JavaScript, který vypadá takto:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Tento kód přistupuje k `DynamicPopulateExtender` (Nedělejte si starosti o neobvyklé ID ale to se budeme dále) a aktivuje dynamické naplnění daty. V rámci aktuální přepínač `this.value` odkazuje na jeho hodnotu, která je `format1`, `format2` nebo `format3` přesně co metodu webové očekává.

Jediné, co chybí v prvku uživatel zatím je `DynamicPopulateExtender` ovládací prvek, který odkazuje přepínačů k webové službě.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Může znovu nezapomeňte strangeová ID používané v ovládacím prvku: `mcd1$myDate` místo `myDate`. Dříve, kód jazyka JavaScript používaný `mcd1_dpe1` přístup `DynamicPopulateExtender` místo `dpe1`. Tato strategie vytváření názvů je zvláštní požadavek při použití `DynamicPopulateExtender` v rámci uživatelského ovládacího prvku. Kromě toho budete muset vložit kontrolních uživatele určitým způsobem, aby to fungovalo. Vytvoření nové stránky technologie ASP.NET a zaregistrujte předponu značky uživatelského ovládacího prvku, který jste právě implementovali:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Pak musíte uvést technologie ASP.NET AJAX `ScriptManager` ovládací prvek na nové stránce:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Nakonec přidejte uživatelský ovládací prvek na stránce. Budete muset nastavit jeho `ID` atribut (a `runat="server"`, samozřejmě), ale budete muset nastavit na konkrétní název: `mcd1` vzhledem k tomu, že toto je předpona používaná pro přístup k ní pomocí jazyka JavaScript v rámci uživatelského ovládacího prvku.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

A to je všechno! Na stránce se chová podle očekávání: uživatel klikne na jednom z přepínačů, ovládací prvek v sadě nástrojů volat webovou službu a zobrazí aktuální datum v požadovaném formátu.


[![Přepínací tlačítka jsou umístěny do uživatelského ovládacího prvku](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Přepínací tlačítka jsou umístěny do uživatelského ovládacího prvku ([kliknutím ji zobrazíte obrázek v plné velikosti](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](dynamically-populating-a-control-using-javascript-code-cs.md)
> [další](dynamically-populating-a-control-vb.md)
