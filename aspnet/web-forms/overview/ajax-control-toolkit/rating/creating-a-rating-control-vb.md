---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Vytvoření ovládacího prvku Rating (VB) | Dokumentace Microsoftu
author: wenz
description: Mnoho webů z elektronického obchodování na komunitní weby, nabízí svým uživatelům míra články nebo položky. To obvykle vyžaduje některé kódování úsilí, ale pracujeme...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b229d0155fbab0437c644b41424164e4c655f5e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755013"
---
<a name="creating-a-rating-control-vb"></a>Vytvoření ovládacího prvku Rating (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Mnoho webů z elektronického obchodování na komunitní weby, nabízí svým uživatelům míra články nebo položky. To obvykle vyžaduje některé kódování úsilí, ale musíme Control Toolkit naše vyřazení.


## <a name="overview"></a>Přehled

Mnoho webů z elektronického obchodování na komunitní weby, nabízí svým uživatelům míra články nebo položky. To obvykle vyžaduje některé kódování úsilí, ale musíme Control Toolkit naše vyřazení.

## <a name="steps"></a>Kroky

Za prvé, budete potřebovat (minimálně) dva druhy bitové kopie: jednu pro plného hodnocení položky a z nich je pro položku prázdný hodnocení. Hodnocení položky je obvykle hvězdička nebo ikonu Veselého. V tomto scénáři najít tři soubory, smiley.png a empty.png a obličej done.png jako součást stažené soubory zdrojového kódu pro účely tohoto kurzu.

Potom vytvořte nový soubor ASP.NET a začít s přidáváním `ScriptManager` ovládacího prvku do ní:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Pak přidejte `Rating` ovládacího prvku z technologie ASP.NET AJAX Control Toolkit. Následující atributy je potřeba nastavit v tomto příkladu:

- `CurrentRating` Počáteční hodnocení, který se má použít
- `MaxRating` maximální hodnocení
- `EmptyStarCssClass` nic neobsahuje třídu šablony stylů CSS pro použití při hodnocení položky (star)
- `FilledStarCssClass` vyplnění třídu šablony stylů CSS pro použití při hodnocení položky (star)
- `StarCssClass` Třída CSS použitá pro viditelné stat
- `WaitingStarCssClass` Při hodnocení hvězdičkami budou odeslána zpět do serveru, použijte třídu šablony stylů CSS

A tady je kód, který vytvoří ovládacího prvku rating s pěti položek (Veselé obličeje), z nichž žádná vyplnění zpočátku:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Tři odkazované třídy šablony stylů CSS teď třeba zobrazit příslušné obrázkové soubory, které lze snadno udělat pomocí šablon stylů CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Ujistěte se, že zadáte šířku a výšku tři Image, jinak může vypadat trochu messed do zobrazení.

Nakonec výsledek hodnocení by měl být uživateli zobrazí (nebo aspoň uložený v databázi). Proto přidejte popisek pro výstup textovou zprávu a tlačítka Odeslat zpět post formuláře hodnocení k serveru:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

V kódu na straně serveru, přístup k ovládacímu prvku hodnocení prostřednictvím jeho `ID` a přejděte k jeho `CurrentRating` vlastnost, která je počet položek vybraných hodnocení, v našem příkladu hodnotu od 0 do 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Uložit na stránku a jejich načtení do prohlížeče. Když najedete myší položky (původně prázdným) hodnocení, JavaScript účinek se vyskytuje: změny hodnocení. Když kliknete na sadu hvězdiček, se uchovávají aktuální hodnocení. Nakonec po odeslání formuláře kód na straně serveru vypíše vybrané hodnocení.


[![Vytvoření systému hodnocení s minimem kódování](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Vytvoření systému hodnocení s minimem kódu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Předchozí](creating-a-rating-control-cs.md)
