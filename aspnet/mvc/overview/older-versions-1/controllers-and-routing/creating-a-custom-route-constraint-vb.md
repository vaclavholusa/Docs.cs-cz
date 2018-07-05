---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Vytvoření vlastního omezení trasy (VB) | Dokumentace Microsoftu
author: StephenWalther
description: Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Můžeme implementovat jednoduché vlastního omezení trasy brání odpovídající w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: cc30cfd49402b10274e22eb27abba73329811b7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385492"
---
<a name="creating-a-custom-route-constraint-vb"></a>Vytvoření vlastního omezení trasy (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Implementujeme jednoduchý vlastní omezení, které brání trasy je nalezena shoda, po provedení požadavku prohlížeče ze vzdáleného počítače.


Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní trasy omezení. Omezení vlastní trasy umožňuje zabránit trasy je nalezena shoda, pokud je nalezena shoda s některé vlastní podmínky.

V tomto kurzu vytvoříme Localhost omezení trasy. Dané omezení trasy Localhost odpovídá pouze požadavky z místního počítače. Vzdálené žádosti z přes Internet není nalezena shoda.

Implementace omezení vlastní trasy implementací rozhraní IRouteConstraint. To je velmi jednoduché rozhraní, které popisuje jedinou metodu:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Metoda vrátí hodnotu typu Boolean. Pokud se vrátit hodnotu False, nebude odpovídat přidružené k dané omezení trasy požadavek prohlížeče.

Výpis 1 je součástí omezení Localhost.

**Výpis 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Omezení v informacích 1 využívá výhod vlastnost IsLocal vystavené třídy HttpRequest. Tato vlastnost vrací hodnotu true, pokud IP adresa požadavku je buď 127.0.0.1 nebo pokud IP adresa požadavku je stejné jako IP adresa serveru.

Můžete použít vlastní omezení v rámci trasy definované v souboru Global.asax. Soubor Global.asax výpis 2 používá Localhost omezení a zabránit uživatelům požadování stránky pro správu, není-li využívají žádost z místního serveru. Žádost o /Admin/DeleteAll se například nezdaří při provedení ze vzdáleného serveru.

**Listing 2 - Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Omezení Localhost se používá v definici trasy, která správce. Tato trasa nebude odpovídat podvýrazu žádost vzdálené prohlížeče. Uvědomte si však, že stejný požadavek může shodovat s další trasy definované v souboru Global.asax. Je důležité pochopit, že omezení zabrání konkrétní trasu odpovídající požadavek a ne všechny trasy definované v souboru Global.asax.

Všimněte si, že výchozí trasa byla zakomentované ze souboru Global.asax ve výpisu 2. Pokud zahrnete výchozí trasu, výchozí trasu odpovídají požadavky řadiče pro správu. V takovém případě vzdáleným uživatelům může stále vyvolání akce kontroleru pro správce i když jejich žádosti o shodě správce trasy.

> [!div class="step-by-step"]
> [Předchozí](creating-a-route-constraint-vb.md)
