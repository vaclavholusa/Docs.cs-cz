---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Co je nového v rozhraní ASP.NET Web Pages 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896391"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Co je nového v rozhraní ASP.NET Web Pages 3.2
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového u 3.2 webových stránek ASP.NET, webových stránek 3.2.2 a [webové stránky 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>Rozhraní ASP.NET Web Pages 3.2

Tato verze opravy chyby a zavádí nová funkce.

## <a name="download"></a>Stáhnout

Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet. Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace. Balíček ASP.NET Web Pages 3.2 má následující verzi: &ldquo;3.2.0&rdquo;. Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.

Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nové funkce a opravy chyb

Jsme pevné jeden chyb a k vylepšení jeden dílčí funkce v této verzi. Můžete vyhledat dotaz pro stejné [zde](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Tato verze zobrazí souhrn up změn [rozhraní ASP.NET Web Pages 3.2.1 betaverze](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) který poskytuje zlepšování významně zvýšit výkon při vykreslování stránky velké razor. V tématu[webu Codeplex problém 585](https://aspnetwebstack.codeplex.com/workitem/585). Toto vydání zarovnaná s MVC 5.2.2 balíčky, které teď bude záviset na tuto verzi.

Jsme pracovali s týmem MSN vykreslování velkých stránek. Při vykreslení stránky více než 80 kB dat, můžeme skončili s objekty v haldě velkého objektu. Pokud se používá několik vrstev rozložení můžete násobí tento vliv.

Výsledek na serveru je navíc využití procesoru, delší uchování paměti a to i dlouho pozastaví během [2. generace čištění](https://msdn.microsoft.com/en-us/library/ms973837.aspx) v systému uvolňování paměti.

Níže je tabulka demonstraci výsledky analýzy [nástroje perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) spustit. Procesor trvá konstantní v 68 % při vykreslení velkých stránek. V tabulce jsou téměř úplně eliminace počet kolekcí, 2. generace, a výsledkem je vyšší rychlost požadavků a významnou snížení pozastaví kvůli uvolnění paměti.

| **Oblasti** | **Před (3.2)** | **Po (3.2.1)** | **Delta %** |
| --- | --- | --- | --- |
| Celkový počet požadavku (počet) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Doba trvání trasování (sekundy) | 196.20 | 198.60 | 1.20% |
| Žádost o za sekundu | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Zatížení procesoru | 68.80% | 68.50% |  -0.40% |
| Ukázky procesoru uvolňování paměti | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Celkový počet přidělených (počet) | 55,357,146 | 57,222,949 | 3.40% |
| Celkový počet GC pozastavení (Ukázky) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (počet) | 403 | 1,216 | 201.70% |
| Gen1 GC (počet) | 290 | 367 | 26.60% |
| Gen2 GC (počet) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| Využití procesoru / požadavku (ukázky/req) | 19.73 | 16.47 | -16.50% |

| Barevné kódování: | <font style="background-color: #00ff00">Zlepšování jádra</font> | <font style="background-color: #4bacc6">Kladné dopad na výkon</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Tato verze obsahuje pouze opravy chyb. Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam chyby v této verzi.
