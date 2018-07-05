---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamické naplnění ovládacího prvku (VB) | Dokumentace Microsoftu
author: wenz
description: ASP.NET AJAX Control Toolkit ovládacího prvku DynamicPopulate volání webové služby (nebo metodu stránky) a vyplní výsledné hodnoty do cílového ovládacího prvku na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fd55b59f9375eb320711ffea8d971a8d86179c43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368657"
---
<a name="dynamically-populating-a-control-vb"></a>Dynamické naplnění ovládacího prvku (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky.


## <a name="overview"></a>Přehled

`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a zkopíruje výslednou hodnotu na cílový ovládací prvek na stránce bez aktualizace stránky. Tento kurz ukazuje, jak nastavit tuto možnost.

## <a name="steps"></a>Kroky

Za prvé, třeba webové služby ASP.NET, která implementuje metodu, které jsou volány `DynamicPopulate`. Vyžaduje třídu webové služby `ScriptService` atribut, který je definován v rámci `Microsoft.Web.Script.Services`; v opačném případě technologie ASP.NET AJAX nelze vytvořit proxy server JavaScript na straně klienta pro webovou službu, kterou pak vyžaduje `DynamicPopulate`.

Metodu webové musíte očekávat jeden argument typu řetězec, volá `contextKey`, protože `DynamicPopulate` ovládací prvek odešle jednu část informací o kontextu se každé volání webové služby. Následující webová služba vrátí aktuální datum ve formátu reprezentována `contextKey` argument:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Webová služba se pak uloží jako `DynamicPopulate.vb.asmx`. Alternativně je možné implementovat `getDate()` metody jako metody stránky v rámci skutečné stránky technologie ASP.NET s `DynamicPopulate` ovládacího prvku.

V dalším kroku vytvořte nový soubor technologie ASP.NET. Jako vždy, prvním krokem je zahrnout `ScriptManager` na aktuální stránce se načíst knihovnu ASP.NET AJAX a jak zajistit Control Toolkit práce:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Pak přidejte ovládací prvek popisku (například pomocí ovládacího prvku HTML se stejným názvem, nebo &lt; `asp:Label`  / &gt; ovládací prvek webu) který se později zobrazí výsledek volání webové služby.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

HTML tlačítko (jako ovládací prvek HTML, protože jsme nevyžadují zpětného odeslání na server) se pak použije k aktivaci dynamické naplnění:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Nakonec potřebujeme `DynamicPopulateExtender` ovládacího prvku na věci při přenosu. Následující atributy se nastaví (kromě těch zřejmé, `ID` a `runat` = `"server"`):

- `TargetControlID` kam umístit výsledek z volání webové služby
- `ServicePath` Cesta k webové službě (vynechat, pokud chcete použít metodu stránky)
- `ServiceMethod` Název webové metody nebo stránky – metoda
- `ContextKey` informace o kontextu k odeslání do webové služby
- `PopulateTriggerControlID` element, který aktivuje volání webové služby
- `ClearContentsDuringUpdate` jestli se má prázdný target element během volání webové služby

Jak je vidět, ovládací prvek vyžaduje určité informace, ale umístění všeho na místě je poměrně přímočaré. Tady je zápis `DynamicPopulateExtender` ovládacího prvku v aktuální situaci:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Na stránce ASP.NET spustit v prohlížeči a klikněte na tlačítko; Zobrazí se aktuální datum ve formátu měsíc roku dny.


[![Klikněte na tlačítko načte data ze serveru](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Klikněte na tlačítko načte data ze serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [další](dynamically-populating-a-control-using-javascript-code-vb.md)
