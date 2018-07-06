---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Vytvoření omezení trasy (VB) | Dokumentace Microsoftu
author: StephenWalther
description: V tomto kurzu Stephen Walther ukazuje, jak můžete řídit, jak prohlížeč požaduje shoda tras tak, že vytvoříte omezení trasy s regulárními výrazy.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 4be9b3c26fe456ae429160766b3366fef54ef1cc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806691"
---
<a name="creating-a-route-constraint-vb"></a>Vytvoření omezení trasy (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak můžete řídit, jak prohlížeč požaduje shoda tras tak, že vytvoříte omezení trasy s regulárními výrazy.


Pomocí omezení trasy omezte požadavků prohlížeče, které odpovídají konkrétní trasy. Regulární výraz můžete použít k určení omezení trasy.

Představte si například, kterou jste definovali trasy v informacích 1 v souboru Global.asax.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Výpis 1 obsahuje trasa s názvem produktu. Trasy produktu můžete použít k mapování požadavků prohlížeče ProductController součástí výpis 2.

**Výpis 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Všimněte si, že akce Details() vystavené kontroler produktu přijímá jeden parametr s názvem productId. Tento parametr je parametr celé číslo.

Trasy definované v informacích 1 se shodují s některým z následujících adres URL:

- / Produktu/23
- / Produktu/7

Bohužel trasy se taky shodovat následující adresy URL:

- / Produktu/Bla
- / Produktu/apple

Protože akce Details() očekává jako parametr celé číslo, požadavku, který obsahuje něco jiného než celočíselnou hodnotu způsobí chybu. Například pokud do prohlížeče zadáte adresu URL /Product/apple pak zobrazí se chybová stránka na obrázku 1.


[![Dialogové okno Nový projekt](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Obrázek 01**: nezobrazí stránka Rozbalit ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-route-constraint-vb/_static/image2.png))


Co vlastně chcete udělat, je odpovídá pouze adresy URL, které obsahují productId správné celé číslo. Omezení můžete použít při definování trasy omezovat adresy URL, které odpovídají trasy. Upravené trasy produktu ve výpisu 3 obsahuje omezení regulárního výrazu, který odpovídá jen celá čísla.

**Listing 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

\D+ regulárního výrazu odpovídá jedné nebo více celých čísel. Vlivem tohoto omezení trasy produkt tak, aby odpovídala následující adresy URL:

- / Produkt/3
- / Produktu/8999

Ale ne následující adresy URL:

- / Produktu/apple
- / Produktu

Tyto požadavky prohlížeče bude zpracován adresou jiné cestě nebo, pokud nejsou žádné odpovídající trasy *prostředek se nenašel* se vrátí chyba.

> [!div class="step-by-step"]
> [Předchozí](creating-custom-routes-vb.md)
> [další](creating-a-custom-route-constraint-vb.md)
