---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Vytvoření vlastního omezení trasy (C#) | Dokumentace Microsoftu
author: StephenWalther
description: Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Můžeme implementovat jednoduché vlastního omezení trasy brání odpovídající w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: a77a25672c94d6b706af0cc36807b11297bc573a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400000"
---
<a name="creating-a-custom-route-constraint-c"></a>Vytvoření vlastního omezení trasy (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther ukazuje, jak můžete vytvořit vlastní trasy omezení. Implementujeme jednoduchý vlastní omezení, které brání trasy je nalezena shoda, po provedení požadavku prohlížeče ze vzdáleného počítače.


Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní trasy omezení. Omezení vlastní trasy umožňuje zabránit trasy je nalezena shoda, pokud je nalezena shoda s některé vlastní podmínky.

V tomto kurzu vytvoříme Localhost omezení trasy. Dané omezení trasy Localhost odpovídá pouze požadavky z místního počítače. Vzdálené žádosti z přes Internet není nalezena shoda.

Implementace omezení vlastní trasy implementací rozhraní IRouteConstraint. To je velmi jednoduché rozhraní, které popisuje jedinou metodu:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Metoda vrátí hodnotu typu Boolean. Pokud se vrátit hodnotu false, nebude odpovídat přidružené k dané omezení trasy požadavek prohlížeče.

Výpis 1 je součástí omezení Localhost.

**Výpis 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Omezení v informacích 1 využívá výhod vlastnost IsLocal vystavené třídy HttpRequest. Tato vlastnost vrací hodnotu true, pokud IP adresa požadavku je buď 127.0.0.1 nebo pokud IP adresa požadavku je stejné jako IP adresa serveru.

Můžete použít vlastní omezení v rámci trasy definované v souboru Global.asax. Soubor Global.asax výpis 2 používá Localhost omezení a zabránit uživatelům požadování stránky pro správu, není-li využívají žádost z místního serveru. Žádost o /Admin/DeleteAll se například nezdaří při provedení ze vzdáleného serveru.

**Listing 2 - Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Omezení Localhost se používá v definici trasy, která správce. Tato trasa nebude odpovídat podvýrazu žádost vzdálené prohlížeče. Uvědomte si však, že stejný požadavek může shodovat s další trasy definované v souboru Global.asax. Je důležité pochopit, že omezení zabrání konkrétní trasu odpovídající požadavek a ne všechny trasy definované v souboru Global.asax.

Všimněte si, že výchozí trasa byla zakomentované ze souboru Global.asax ve výpisu 2. Pokud zahrnete výchozí trasu, výchozí trasu odpovídají požadavky řadiče pro správu. V takovém případě vzdáleným uživatelům může stále vyvolání akce kontroleru pro správce i když jejich žádosti o shodě správce trasy.

> [!div class="step-by-step"]
> [Předchozí](creating-a-route-constraint-cs.md)
> [další](asp-net-mvc-controller-overview-vb.md)
