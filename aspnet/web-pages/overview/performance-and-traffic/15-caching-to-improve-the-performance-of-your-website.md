---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "Ukládání dat v rozhraní ASP.NET Web Pages (Razor) lokality pro dosažení vyššího výkonu | Microsoft Docs"
author: tfitzmac
description: "Můžete urychlit web tak, že ho uložit – to znamená, mezipaměti - výsledky data, která by obvykle trvat poměrně dlouho načíst nebo zpracovat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Ukládání dat v Web Pages (Razor) technologie ASP.NET do mezipaměti pro dosažení vyššího výkonu
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak používat pomocná rutina pro informace o mezipaměti pro dosažení vyššího výkonu na webu technologie ASP.NET Web Pages (Razor). Můžete zrychlení webu tak, že se úložiště &#8212; To znamená, mezipaměti &#8212; výsledky data, která normálně by trvat velmi dlouho načíst nebo zpracování a který se nemění často.
> 
> **Získáte informace:** 
> 
> - Jak používat ukládání do mezipaměti zlepšit odezvu vašeho webu.
> 
> Toto jsou nové v článku funkce ASP.NET:
> 
> - `WebCache` Pomocné rutiny.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


Pokaždé, když se někdo požádá o stránku z vaší lokality, webový server má nějakou práci za účelem splnění požadavku. Některé z vašich stránek server může být potřeba provádět úlohy, které trvat (poměrně) dlouhou dobu, jako je například načítání dat z databáze. I v případě, že tyto úlohy nejsou trvat dlouho v absolutních číslech, pokud váš web vyskytne velký objem provozu, celou řadu jednotlivých požadavků, které způsobí webový server k provedení úlohy složitý nebo pomalé můžete přidat do značné úsilí. Nakonec to může mít vliv na výkon webu.

Jeden způsob, jak zlepšit výkon vašeho webu v případech, jako je to je ukládat data do mezipaměti. Pokud váš web získá opakované žádosti pro stejné informace a není potřeba upravit pro každou osobu, informace a není čas citlivé místo znovu načítání nebo přepočítání, můžete po načtení dat a potom uložte výsledky. Při příštím přijde žádost pro tento informace, jenom získáte ji z mezipaměti.

Obecně platí mezipaměti informace, které nemění příliš často. Když vložíte informace v mezipaměti, uloží se do paměti na webovém serveru. Můžete určit, jak dlouho se do mezipaměti, z sekund dnů. Při ukládání do mezipaměti období vyprší, informace automaticky odebrány z mezipaměti.

> [!NOTE]
> Položky v mezipaměti může odebrat z důvodů než jejich platnost vyprší. Například webový server může dočasně nedostatek paměti a jedním ze způsobů, můžete ho získat paměť je aktivována položky z mezipaměti. Jak zjistíte, i když jste vložíte informace do mezipaměti, budete muset Přesvědčte se, zda je stále existuje v případě potřeby.


Představte si, že má daný web stránku, která zobrazuje aktuální teploty a Prognóza počasí. Chcete-li získat tento typ informací, může odeslat požadavek na externí služby. Vzhledem k tomu, že tyto informace nemění příliš hodně (v rámci dvou hodin časové období, např.) a vzhledem k tomu, že externí volání vyžadovat čas a šířku pásma, je vhodným kandidátem pro ukládání do mezipaměti.

## <a name="adding-caching-to-a-page"></a>Přidání ukládání do mezipaměti na stránku

Technologie ASP.NET obsahuje `WebCache` pomocné rutiny, která umožňuje snadno přidat ukládání do mezipaměti na váš web a přidejte data do mezipaměti. V tomto postupu vytvoříte stránky, který ukládá do mezipaměti aktuální čas. Tato akce není v příkladu reálného vzhledem k tomu, že aktuální čas je něco, často mění a která kromě toho není komplexní k výpočtu. Je však vhodné pro ilustraci ukládání do mezipaměti v akci.

1. Přidat novou stránku s názvem *WebCache.cshtml* na web.
2. Přidejte následující kód a kód na stránku:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Když budete ukládat data do mezipaměti, které jste ji vložili do mezipaměti pomocí názvu to je v rámci webu jedinečný. V takovém případě použijete položky mezipaměti s názvem `CachedTime`. Toto je `cacheItemKey` uvedeno v příkladu kódu.

    Nejprve čte kód `CachedTime` položky mezipaměti. Pokud je vrácena hodnota (Pokud položka mezipaměti není null), kód právě nastaví hodnotu proměnné čas na data v mezipaměti.

    Ale pokud neexistuje položky mezipaměti (to znamená, je null), kód nastaví hodnotu čas, přidá do mezipaměti a nastaví hodnotu vypršení platnosti na jednu minutu. Za minutu budou zahozeny položky mezipaměti. (Výchozí hodnota pro vypršení platnosti položky v mezipaměti je 20 minut). Příkaz `WebCache.Set(cacheItemKey, time, 1, false)` ukazuje, jak přidat aktuální hodnota času do mezipaměti a jeho vypršení platnosti nastavené na 1 minutu. Nastavení *parametr slidingExpiration* parametru `false` znamená čas vypršení platnosti neobnovíte pokaždé, když se požaduje. Vypršení platnosti přesně 1 minutu po původně byl přidán do mezipaměti. Pokud nastavíte tuto hodnotu na `true` čas vypršení platnosti 1 minuta se vynuluje pokaždé, když je požadována hodnota z mezipaměti.

    Tento kód ukazuje vzor, který byste měli vždycky používat při mezipaměti data. Než získáte něco z mezipaměti, vždy proveďte nejprve kontrolu jestli `WebCache.Get` Metoda vrátila hodnotu null. Mějte na paměti, že položky mezipaměti vypršela nebo mohl být odebrán z jiného důvodu, takže všechny položky daného nikdy zaručena bezpečnost pro přístup do mezipaměti.
3. Spustit *WebCache.cshtml* v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Při prvním požadavku na stránce časových dat není v mezipaměti a kód má pro přidání hodnoty time do mezipaměti.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aktualizujte *WebCache.cshtml* v prohlížeči. Tentokrát časových dat je v mezipaměti. Všimněte si, že se od posledního zobrazit stránku nezměnil čas.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Počkejte minutu mezipaměti vyprázdnit a pak aktualizujte stránku. Stránky znovu označuje, že časových dat nebyl nalezen v mezipaměti, a čas poslední aktualizace je přidán do mezipaměti.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


- [Zobrazení dat v grafu](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referenční dokumentace rozhraní API WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
