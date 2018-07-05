---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Co je nového v rozhraní ASP.NET Web Pages 3.2 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: eb5698b2928c1f2d1016ec74c18a63e2df5b3225
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390430"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Co je nového v rozhraní ASP.NET Web Pages 3.2
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového v ASP.NET Web Pages 3.2, Web Pages 3.2.2 a [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>Rozhraní ASP.NET Web Pages 3.2

Tato verze opravuje chybu a zavádí jednu novou funkci.

## <a name="download"></a>Stáhnout

Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet. Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace. Balíček ASP.NET Web Pages 3.2 má následující verzi: &ldquo;3.2.0&rdquo;. Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.

Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nové funkce a opravy chyb

Oprava chyby jeden jsme provedli jeden dílčí funkce vylepšení v této verzi. Můžete najít dotaz pro stejný [tady](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>Rozhraní ASP.NET Web Pages 3.2.2

Tato verze zobrazí nahoru změny [rozhraní ASP.NET Web Pages 3.2.1 betaverze](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) poskytující zlepšení výkonu při vykreslování stránky razor velké. Zobrazit[Codeplex problém 585](https://aspnetwebstack.codeplex.com/workitem/585). Tato verze v souladu s MVC 5.2.2 balíčky, které teď bude záviset na tuto verzi.

Tým služby MSN jsme pracovali na vykreslování velkých stránek. Pokud stránky vykreslit více než 80 kB dat, můžeme skončit s objekty na velkých objektových haldách. Pokud se používá několik vrstev rozložení můžete vynásobené tohoto efektu.

Výsledek na serveru je další využití procesoru, paměti a dokonce i dlouho delší dobu uchování pozastaví při [2. generace vyčištění](https://msdn.microsoft.com/en-us/library/ms973837.aspx) v systému uvolňování paměti.

Tady je tabulka demonstrace výsledky analýzy [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) pro spuštění. Procesoru se nachází konstantní 68 %, zatímco jsou vykreslované velkých stránek. Tabulka ukazuje, že číslo 2. generace kolekce téměř zcela odstraněn a výsledkem je vyšší frekvence požadavků a značné snížení možné je kvůli uvolňování paměti.

| **Oblast** | **Před (3.2)** | **Po (3.2.1)** | **% Rozdílů** |
| --- | --- | --- | --- |
| Celkový požadavek (počet) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Sledovat trvání (v sekundách) | 196.20 | 198.60 | 1.20% |
| Žádosti za sekundu | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Zatížení procesoru | 68.80% | 68.50% |  -0.40% |
| Vzorky procesoru uvolňování paměti | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Celkový počet přidělení (počet) | 55,357,146 | 57,222,949 | 3.40% |
| Celkový počet pozastavení uvolňování paměti (Ukázky) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (počet) | 403 | 1,216 | 201.70% |
| Gen1 GC (počet) | 290 | 367 | 26.60% |
| Uvolňování paměti Gen2 (počet) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| Využití procesoru / žádosti (vzorků/no) | 19.73 | 16.47 | -16.50% |

| Barevné kódování: | <font style="background-color: #00ff00">Vylepšení jádra</font> | <font style="background-color: #4bacc6">Pozitivní dopad na výkon</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Tato verze obsahuje pouze oprav chyb. Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam problémů, které jsou opravené v této verzi.
