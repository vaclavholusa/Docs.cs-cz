---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Vytváření vlastní trasy omezení (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Implementaci jednoduchou vlastní omezení, které brání v dodržení trasa odpovídá w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-vb"></a>Vytváření vlastní trasy omezení (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Můžeme implementovat jednoduchý vlastní omezení, které brání trasa se shoduje se, když prohlížeč požadavku ze vzdáleného počítače.


Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní trasy omezení. Omezení vlastní trasy umožňuje zabránit trasy probíhá shodná, pokud je nalezena shoda některé vlastní podmínky.

V tomto kurzu vytvoříme Localhost omezení trasy. Omezení trasy Localhost pouze odpovídá požadavkům z místního počítače. Vzdálené žádosti z přes Internet není nalezena shoda.

Implementací rozhraní IRouteConstraint implementujete omezení vlastní trasy. To je velmi jednoduché rozhraní, který je popsán způsob:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Metoda vrací logickou hodnotu. Pokud jste vrátí hodnotu False, trasy přidružených k omezení nebude odpovídat požadavku prohlížeče.

Výpis 1 je součástí omezení Localhost.

**Výpis 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Omezení v výpis 1 využívá vlastnost IsLocal vystavené třídě požadavku HTTP. Tato vlastnost vrací hodnotu true, pokud se IP adresa požadavku je buď 127.0.0.1 nebo pokud IP adresa požadavku je stejný jako IP adresa serveru.

Můžete použít vlastní omezení v rámci trasy definované v souboru Global.asax. Soubor Global.asax výpis 2 používá omezení Localhost zabránit uživatelům v požadavku stránky pro správu, pokud provádění požadavku z místního serveru. Žádost o /Admin/DeleteAll se například nezdaří, když ze vzdáleného serveru.

**Listing 2 - Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Omezení Localhost se používá v definici trasy, která správce. Tato trasa nebude odpovídat požadavkem vzdáleného prohlížeče. Uvědomte si ale, že další trasy definované v souboru Global.asax může odpovídat stejném požadavku. Je důležité si uvědomit, že omezení zabrání odpovídající žádost o konkrétní trasy a ne všechny trasy definované v souboru Global.asax.

Všimněte si, že výchozí trasu byla změněna na komentář ze souboru Global.asax v výpis 2. Pokud zahrnete výchozí trasu, výchozí trasa odpovídá požadavky řadiče pro správu. V takovém případě vzdálených uživatelů může stále vyvolání akce správce řadiče i v případě, že jejich požadavky nebude odpovídají správce trasy.

> [!div class="step-by-step"]
> [Předchozí](creating-a-route-constraint-vb.md)
