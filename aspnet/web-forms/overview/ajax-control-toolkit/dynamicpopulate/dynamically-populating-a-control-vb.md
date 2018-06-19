---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamicky naplnění ovládacího prvku (VB) | Microsoft Docs
author: wenz
description: DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879735"
---
<a name="dynamically-populating-a-control-vb"></a>Dynamicky naplnění ovládacího prvku (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.


## <a name="overview"></a>Přehled

`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky. Tento kurz ukazuje, jak nastavit tuto možnost.

## <a name="steps"></a>Kroky

Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulate`. Třída webové služby vyžaduje `ScriptService` atribut, který je definován v rámci `Microsoft.Web.Script.Services`; v opačném případě prvku ASP.NET AJAX, nelze vytvořit proxy server JavaScript na straně klienta pro webovou službu, který naopak je požadován `DynamicPopulate`.

Webové metody měli očekávat jeden argument typu řetězec, s názvem `contextKey`, protože `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby. Webovou službu následující vrátí aktuální datum ve formátu reprezentována `contextKey` argument:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Webová služba pak je uložena jako `DynamicPopulate.vb.asmx`. Alternativně může implementovat `getDate()` metoda jako metodu stránky v rámci skutečné stránky ASP.NET s `DynamicPopulate` ovládacího prvku.

V dalším kroku vytvořte nový soubor technologie ASP.NET. Jako vždy, prvním krokem je zahrnout `ScriptManager` na aktuální stránce se načíst knihovnu ASP.NET AJAX a aby pracovní sada nástrojů ovládacího prvku:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Potom přidat ovládací prvek popisek (například pomocí ovládacího prvku HTML se stejným názvem, nebo &lt; `asp:Label`  / &gt; ovládacího prvku webového) který později zobrazí výsledek volání webové služby.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

K aktivaci dynamické plnění pak bude sloužit HTML tlačítko (jako ovládací prvek jazyka HTML, protože jsme nevyžadují zpětné volání k serveru):

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Nakonec potřebujeme `DynamicPopulateExtender` řízení přenosu věcí nahoru. Následující atributy se nastaví (kromě těch zřejmé, `ID` a `runat` = `"server"`):

- `TargetControlID` umístění výsledek z volání webové služby
- `ServicePath` Cesta k webové službě (vynechat, pokud chcete použít metodu stránky)
- `ServiceMethod` Název webové metody nebo stránce – metoda
- `ContextKey` informace o kontextu k odeslání do webové služby
- `PopulateTriggerControlID` element, který aktivuje volání webové služby
- `ClearContentsDuringUpdate` jestli se má prázdný target element během volání webové služby

Jak můžete vidět, ovládacího prvku vyžaduje některé informace, ale vytvoření všechno, co je poměrně jednoduché. Zde je kód pro `DynamicPopulateExtender` ovládací prvek v aktuální scénář:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

V prohlížeči spuštění stránky ASP.NET a klikněte na tlačítko; Zobrazí se aktuální datum ve formátu měsíc den roku.


[![Klikněte na tlačítko načte data ze serveru](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Klikněte na tlačítko načte data ze serveru ([Kliknutím zobrazit obrázek v plné velikosti](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [další](dynamically-populating-a-control-using-javascript-code-vb.md)
