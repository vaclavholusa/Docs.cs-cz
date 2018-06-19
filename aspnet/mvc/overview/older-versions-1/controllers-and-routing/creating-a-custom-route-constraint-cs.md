---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Vytváření vlastní trasy omezení (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Implementaci jednoduchou vlastní omezení, které brání v dodržení trasa odpovídá w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874340"
---
<a name="creating-a-custom-route-constraint-c"></a>Vytváření vlastní trasy omezení (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Můžeme implementovat jednoduchý vlastní omezení, které brání trasa se shoduje se, když prohlížeč požadavku ze vzdáleného počítače.


Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní trasy omezení. Omezení vlastní trasy umožňuje zabránit trasy probíhá shodná, pokud je nalezena shoda některé vlastní podmínky.

V tomto kurzu vytvoříme Localhost omezení trasy. Omezení trasy Localhost pouze odpovídá požadavkům z místního počítače. Vzdálené žádosti z přes Internet není nalezena shoda.

Implementací rozhraní IRouteConstraint implementujete omezení vlastní trasy. To je velmi jednoduché rozhraní, který je popsán způsob:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Metoda vrací logickou hodnotu. Pokud jste vrátí hodnotu false, trasy přidružených k omezení nebude odpovídat požadavku prohlížeče.

Výpis 1 je součástí omezení Localhost.

**Listing 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Omezení v výpis 1 využívá vlastnost IsLocal vystavené třídě požadavku HTTP. Tato vlastnost vrací hodnotu true, pokud se IP adresa požadavku je buď 127.0.0.1 nebo pokud IP adresa požadavku je stejný jako IP adresa serveru.

Můžete použít vlastní omezení v rámci trasy definované v souboru Global.asax. Soubor Global.asax výpis 2 používá omezení Localhost zabránit uživatelům v požadavku stránky pro správu, pokud provádění požadavku z místního serveru. Žádost o /Admin/DeleteAll se například nezdaří, když ze vzdáleného serveru.

**Listing 2 - Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Omezení Localhost se používá v definici trasy, která správce. Tato trasa nebude odpovídat požadavkem vzdáleného prohlížeče. Uvědomte si ale, že další trasy definované v souboru Global.asax může odpovídat stejném požadavku. Je důležité si uvědomit, že omezení zabrání odpovídající žádost o konkrétní trasy a ne všechny trasy definované v souboru Global.asax.

Všimněte si, že výchozí trasu byla změněna na komentář ze souboru Global.asax v výpis 2. Pokud zahrnete výchozí trasu, výchozí trasa odpovídá požadavky řadiče pro správu. V takovém případě vzdálených uživatelů může stále vyvolání akce správce řadiče i v případě, že jejich požadavky nebude odpovídají správce trasy.

> [!div class="step-by-step"]
> [Předchozí](creating-a-route-constraint-cs.md)
> [další](asp-net-mvc-controller-overview-vb.md)
