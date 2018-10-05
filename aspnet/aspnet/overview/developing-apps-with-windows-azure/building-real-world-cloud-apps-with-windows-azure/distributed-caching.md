---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Distribuované ukládání do mezipaměti (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 26866ef9d13a198aab627ccf0f5e1ff3d2304427
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577538"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Distribuované ukládání do mezipaměti (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Předchozí kapitoly zvažovali zpracování přechodných chyb a uvedených ukládání do mezipaměti jako strategii jističe. Tato kapitola obsahuje další informace o ukládání do mezipaměti, kdy se má použít, běžných vzorů pro jeho použití, včetně a jak implementovat v Azure.

## <a name="what-is-distributed-caching"></a>Co je distribuované ukládání do mezipaměti

Mezipaměť poskytuje vysokou propustnost, nízkou latenci přístupu k datům běžně používanou aplikací, ukládání dat v paměti. Pro cloudové aplikace je nejužitečnější typ mezipaměti distribuované mezipaměti, což znamená, že data nejsou uložená v paměti jednotlivých webový server, ale na další cloudové prostředky a data uložená v mezipaměti je k dispozici pro všechny servery pro webové aplikace (nebo jiné cloudové virtuální počítače této ar Elektronická aplikace používá).

![Diagram zobrazující více webových serverů, které nepřistupují na servery stejné mezipaměti](distributed-caching/_static/image1.png)

Při škálování aplikace přidáním nebo odebráním servery nebo servery se nahradí kvůli upgradu nebo chyby, data uložená v mezipaměti zůstává přístupný pro každý server, na kterém běží aplikace.

Přístup k vysoké latenci data do trvalého úložiště dat se vyhnout, můžete ukládání do mezipaměti výrazně zlepšit odezvu aplikace. Načítání dat z mezipaměti je například mnohem rychlejší než jejich načtení z relační databáze.

Na straně výhody ukládání do mezipaměti snižuje náklady přenosy do úložiště trvalých dat, to ale může způsobit snížení nákladů při odchozí přenos dat do trvalého úložiště dat.

## <a name="when-to-use-distributed-caching"></a>Kdy použít distribuované ukládání do mezipaměti

Ukládání do mezipaměti funguje nejlépe pro úlohy aplikací, udělat více čtení než zápisu dat, a pokud datový model podporuje organizace klíč/hodnota, který použijete k ukládání a načítání dat v mezipaměti. Je také užitečnější, pokud aplikace uživatelů sdílí mnoho společných dat; například mezipaměť neposkytuje tolik výhody, pokud každý uživatel obvykle načítá data, které jsou jedinečné pro daného uživatele. Příklad, kdy ukládání do mezipaměti může být velmi užitečné je katalog produktů, protože se data nemění příliš často a všem zákazníkům je vaším cílem stejná data.

Výhody ukládání do mezipaměti bude stále měřitelné Čím aplikaci škáluje, více omezení na výpočetní výkon jsou omezení propustnosti a latence zpoždění do trvalého úložiště dat. Však může implementovat, ukládání do mezipaměti z jiných důvodů než i výkon. Pro data, která nemusí být zcela aktuální, když se zobrazí uživateli přístup do mezipaměti může sloužit jako jističe pro při do trvalého úložiště dat. neodpovídá nebo není k dispozici.

## <a name="popular-cache-population-strategies"></a>Oblíbené mezipaměti naplňování strategií

Aby bylo možné načíst data z mezipaměti, budete muset uložit existuje nejprve. Existuje několik strategií, jak získat data, která je nutné do mezipaměti:

- Na vyžádání / doplňováním mezipaměti

    Aplikace se pokusí načíst data z mezipaměti a když mezipaměti nemá data ("miss"), ale aplikace ukládá data v mezipaměti tak, že bude k dispozici příštím. Příště se aplikace pokusí získat stejná data, zjistí, co hledají v mezipaměti ("zasáhnout"). Pokud chcete zabránit, načítají se data uložená v mezipaměti, která se změnila na databázi, zneplatnění mezipaměti při provádění změn do úložiště dat.
- Datová oznámení na pozadí

    Služby na pozadí nabízení dat do mezipaměti v pravidelných intervalech a aplikace vždy si z mezipaměti. Tento přístup skvěle funguje s vysokou latencí zdroje dat, které nevyžadují vždy vrátí nejnovější data.
- Jističe

    Aplikace obvykle komunikuje přímo s do trvalého úložiště dat. ale problémů s dostupností po do trvalého úložiště dat. aplikace načítá data z mezipaměti. Data mohou byla pozastavena do mezipaměti pomocí vyhrazené mezipaměti nebo strategie nabízených dat na pozadí. Toto je chyba zpracování strategie spíše než strategii pro zlepšení výkonu.

Aby bylo možné zachovat data v mezipaměti aktuální, můžete odstranit položky související mezipaměti, pokud vaše aplikace vytvoří, aktualizuje, nebo odstraní data. Pokud je v pořádku pro vaši aplikaci někdy získat data, která je mírně zastaralá, můžete se spolehnout na čas konfigurovatelné vypršení platnosti lze nastavit omezení na data můžou být stáří mezipaměti.

Můžete nakonfigurovat absolutní vypršení platnosti (množství času, od vytvoření položky v mezipaměti) nebo klouzavé vypršení platnosti (množství času od posledního přístupu k položce mezipaměti). Absolutní vypršení platnosti se používá, když jsou v závislosti na mechanismu vypršení platnosti mezipaměti, chcete-li zabránit přílišnému zastarávání. V aplikaci Fix It jsme budete ručně vyčistit položky v mezipaměti zastaralá a použijeme klouzavé vypršení platnosti pro nejaktuálnější data uchovávat v mezipaměti. Bez ohledu na to, že zásada vypršení platnosti, kterou zvolíte bude do mezipaměti automaticky vyřazovat nejstarší položky (nejméně naposledy použité nebo LRU), při dosažení limitu paměti do mezipaměti.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Vzorový kód s doplňováním mezipaměti pro aplikace Fix It

V následujícím ukázkovém kódu jsme mezipaměti proveďte nejprve kontrolu při načítání úloh Fix It. Pokud se úloha nenajde v mezipaměti, se vrací. Pokud není nalezen, můžeme ho získat z databáze a ukládání do mezipaměti. Změny žádným přidání ukládání do mezipaměti `FindTaskByIdAsync` metody jsou zvýrazněné.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Při aktualizaci nebo odstranění úkolu Fix It, budete muset zneplatnit úkolů (odebrat) v mezipaměti. V opačném případě budoucnosti se pokusí přečíst, že tuto úlohu nadále dostávat stará data z mezipaměti.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Toto jsou ukázky, které ukazují jednoduchý kód ukládání do mezipaměti; ukládání do mezipaměti není implementovaná v ke stažení projektu opravit ho.

## <a name="azure-caching-services"></a>Ukládání do mezipaměti služby Azure

Azure nabízí následující služby ukládání do mezipaměti: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) a [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis cache je založená na oblíbené [projektu open source mezipaměti redis cache](http://redis.io/) a je první volbu pro většinu ukládání do mezipaměti scénáře.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Pomocí mezipaměť zprostředkovatele stavu relace ASP.NET

Jak je uvedeno v [webového vývoje osvědčené postupy kapitoly](web-development-best-practices.md), osvědčeným postupem je vyhýbat se použití stavu relace. Pokud vaše aplikace vyžaduje stav relace, další osvědčeným postupem je se vyhnout výchozího zprostředkovatele v paměti, protože, který neumožňuje škálování na více systémů (více instancí webového serveru). Zprostředkovatel stavu relací ASP.NET SQL serveru umožňuje lokalitu, která běží na více webových serverů k používání stavu relace, ale budou vám účtovány s vysokou latencí náklady v porovnání s poskytovatelem služby v paměti. Nejlepší řešení, pokud máte používání stavu relace je použití zprostředkovatele mezipaměti, jako [zprostředkovatel stavu relací pro Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Souhrn

Už víte, aplikace Fix It implementace ukládání do mezipaměti za účelem zlepšení doby odezvy a škálovatelnost a k tomu, aby aplikace, aby mohli zůstat responzivní pro operace čtení, pokud je databáze není k dispozici. V [další kapitolu](queue-centric-work-pattern.md) vám ukážeme, jak dál vylepšit škálovatelnost a aby se aplikace nadále reagovat pro operace zápisu.

## <a name="resources"></a>Prostředky

Další informace o ukládání do mezipaměti najdete v následující prostředky.

Dokumentace

- [Mezipaměť Azure](https://msdn.microsoft.com/library/gg278356.aspx). Oficiální dokumentaci MSDN na ukládání do mezipaměti v Azure.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobrazit pokyny k ukládání do mezipaměti a model s doplňováním mezipaměti.
- [Bezporuchový: Pokyny, odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper Marc Mercuri, Ulrich Homann a Andrew Townhill. V části na ukládání do mezipaměti.
- [Osvědčené postupy pro navrhování rozsáhlých služeb v Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Dokument White paper Mark Simms a Michael Thomassy. V části na distribuované ukládání do mezipaměti.
- [Distribuované ukládání do mezipaměti na cestě k škálovatelnost](https://msdn.microsoft.com/magazine/dd942840.aspx). Starší článku MSDN Magazine (2009), ale obsahuje jasně písemné Úvod do distribuované ukládání do mezipaměti. Přejde do větší hloubky, než na ukládání do mezipaměti části Dokumenty white paper bezporuchový a osvědčené postupy.

Videa

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri a Mark Simms devět částí série. Představuje 400 úroveň zobrazení o tom, jak navrhovat cloudových aplikací. Tato série se zaměřuje na teorií a důvody, proč; Další postupy podrobnosti najdete v tématu Vytváření velký řady podle Mark Simms. Viz ukládání do mezipaměti diskuze v epizodě 3 počínaje 1:24:14.
- [Vytváření Big: Získané od zákazníků Azure – část I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies distribuované ukládání do mezipaměti počínaje 46:00 Tento článek popisuje. Podobně jako řady bezporuchový ale přejde do další postupy podrobnosti. Prezentace byl zadán 31. října 2012, takže nezahrnuje službu ukládání do mezipaměti služby Web Apps ve službě Azure App Service, která byla zavedena v 2013.

Ukázka kódu

- [Cloud Service Fundamentals v Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázkovou aplikaci, která implementuje distribuované ukládání do mezipaměti. Najdete v doprovodných blogovém příspěvku [Cloud Service Fundamentals – základní informace o ukládání do mezipaměti](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Předchozí](transient-fault-handling.md)
> [další](queue-centric-work-pattern.md)
