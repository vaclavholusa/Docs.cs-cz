---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Distribuované ukládání do mezipaměti (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 3600200f9bb705ccf66c859547668bdf8e89d97a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868669"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Distribuované ukládání do mezipaměti (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Předchozích kapitol hledá v přechodná chyba zpracování a ukládání do mezipaměti jako strategie jistič uvedených. Tato kapitola obsahuje další informace o ukládání do mezipaměti, kdy ji obecné vzory pro používání, použít včetně a implementaci v Azure.

## <a name="what-is-distributed-caching"></a>Co je distribuován ukládání do mezipaměti

Mezipaměť poskytuje vysokou propustnost, nízkou latencí přístup k datům nejčastěji používanou aplikací, tím, že ukládání dat v paměti. Pro cloudové aplikace je velmi užitečné typ mezipaměti distribuované mezipaměti, což znamená, že data nejsou uložená v paměti jednotlivých webový server, ale na jiné cloudové prostředky, a data uložená v mezipaměti, která je k dispozici pro všechny servery, webové aplikace (nebo jiné cloudové virtuální počítače této ar e používá aplikace).

![Diagram zobrazující více webových serverů přístup ke stejné servery mezipaměti](distributed-caching/_static/image1.png)

Když aplikace škáluje přidáním nebo odebráním serverů, nebo když jsou servery nahradit z důvodu chyb nebo upgradů, data uložená v mezipaměti zůstává přístupný pro každý server, který spouští aplikace.

Vyhnout přístup k datům vysokou latencí úložiště dat, může ukládání do mezipaměti výrazně zlepšit odezvu aplikací. Načítání dat z mezipaměti je například mnohem rychlejší než načítání z relační databáze.

Tím se snižuje straně výhody ukládání do mezipaměti, provoz do úložiště dat, což může vést k nižší náklady, pokud jsou sazbách za odchozí data poplatky za úložiště dat trvalé.

## <a name="when-to-use-distributed-caching"></a>Kdy použít distribuované ukládání do mezipaměti

Ukládání do mezipaměti funguje nejvhodnější pro aplikace úlohy, proveďte další čtení než zápis dat, a když datový model podporuje organizace klíč/hodnota, která používáte k ukládání a načítání dat v mezipaměti. Je také užitečnější při aplikace uživatelé sdílet spoustu dalších běžných dat; v případě, že každý uživatel většinou načte data jedinečné pro tohoto uživatele například mezipaměti nebude zadejte jako řadu výhod. Příklad, kdy ukládání do mezipaměti může být velmi užitečné je katalog produktů, protože data nemění příliš často a všem zákazníkům před sebou stejná data.

Výhodou ukládání do mezipaměti se stane stále měřitelné více škáluje aplikace, omezení propustnosti a latence zpoždění trvalé úložiště jsou více omezení na aplikace celkový výkon. Však může implementovat, ukládání do mezipaměti z jiných důvodů než i výkon. Pro data, která nemusí být perfektně aktuální, když se zobrazí uživateli přístup do mezipaměti může sloužit jako jistič pro v případě je úložiště dat neodpovídá nebo není k dispozici.

## <a name="popular-cache-population-strategies"></a>Strategie naplnění oblíbených mezipaměti

Aby bylo možné načíst data z mezipaměti, budete muset uložit existuje nejdřív. Existuje několik strategií pro získávání dat, který je nutné do mezipaměti:

- Na vyžádání / vyhraďte do mezipaměti

    Aplikace se pokusí načíst data z mezipaměti a když mezipaměti nemá data ("nenalezeno"), aplikace ukládá data do mezipaměti, tak, že bude k dispozici další čas. Nalezne dalším spuštění aplikace se pokusí získat stejná data, co hledají v mezipaměti ("nalezený"). Pokud chcete zabránit načítání data uložená v mezipaměti, který se změnil na databázi, platnost mezipaměti při provádění změn do úložiště dat.
- Datová oznámení pozadí

    Pozadí služby nabízet data do mezipaměti v pravidelných intervalech a aplikace, vždycky si z mezipaměti. Tento postup funguje dobře s vysokou latencí zdroje dat, které nevyžadují vždy vrátí nejnovější data.
- Jistič

    Aplikace obvykle komunikuje přímo s úložištěm dat, ale pokud je úložiště dat trvalé dostupnosti problémy, aplikace získá data z mezipaměti. Data může byly uvedeny v mezipaměti pomocí vyhrazené mezipaměti nebo pozadí data nabízené strategie. Toto je chybu zpracování strategie spíše než strategie pro zlepšení výkonu.

Chcete-li zachovat data v mezipaměti aktuální, můžete odstranit položky související mezipaměti, pokud vaše aplikace vytvoří, aktualizace, nebo odstraní data. Pokud pro vaši aplikaci se někdy získat data, která jsou mírně zastaralá se nic neděje, můžete spolehnout na čas vypršení platnosti konfigurovat lze nastavit omezení na stáří mezipaměti data mohou být.

Můžete nakonfigurovat absolutní vypršení platnosti (množství času, protože položku mezipaměti, která byla vytvořena) nebo klouzavé vypršení platnosti (množství čas od posledního přístupu k položce mezipaměti). Absolutní vypršení platnosti se používá, když jsou v závislosti na mechanismu vypršení platnosti mezipaměti zabránit vzniku příliš zastaralá data. V aplikaci opravte ji a budeme budete ručně vyřazení mezipaměti zastaralé položky a použijeme klouzavé vypršení platnosti zachovat aktuální data v mezipaměti. Bez ohledu na zásady vypršení platnosti, který zvolíte bude do mezipaměti automaticky vyřazení nejstarší položky (minimálně nedávno použité nebo hodnoty nejdelšího Nepoužití.), po dosažení limitu paměti do mezipaměti.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Ukázkový kód mezipaměti vyhraďte opravte ji aplikace

V následující vzorový kód jsme mezipaměti proveďte nejprve kontrolu při načítání úlohy opravte ji. Pokud je úloha nalezených v mezipaměti, vrátíme. Pokud není nalezen, jsme získat z databáze a uloží jej v mezipaměti. By provedené změny k přidání ukládání do mezipaměti na `FindTaskByIdAsync` jsou vyznačené metoda.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Při aktualizaci nebo odstranění úlohy opravte ji, budete muset zrušit platnost úloh (odebrat) uložené v mezipaměti. V opačném budoucí pokusí přečíst, že tento úkol bude nadále získat stará data z mezipaměti.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Toto jsou ukázky pro ilustraci jednoduché ukládání do mezipaměti kódu; ukládání do mezipaměti nebyla implementována v ke stažení projektu opravte ji.

## <a name="azure-caching-services"></a>Ukládání do mezipaměti služby Azure

Azure nabízí následující služby ukládání do mezipaměti: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) a [Azure spravované Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis cache je založená na oblíbených [otevřít zdroj Redis Cache](http://redis.io/) a je první volbou pro většinu ukládání do mezipaměti scénáře.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Pomocí zprostředkovatele mezipaměti stavu relace ASP.NET

Jak je uvedeno v [webové vývoj osvědčené postupy kapitoly](web-development-best-practices.md), osvědčeným postupem je vyhýbat se používání stavu relace. Pokud vaše aplikace vyžaduje stav relace, další osvědčený postup se vyhnete výchozího zprostředkovatele v paměti vzhledem k tomu, který neumožňuje horizontální navýšení kapacity (více instancí webového serveru). Umožňuje poskytovatele stavu relace ASP.NET SQL Server lokality, která běží na více webových serverů k používání stavu relace, ale jeho vznikly náklady vysokou latencí ve srovnání s poskytovatele v paměti. Nejlepším řešením, pokud máte se používání stavu relace je pro použití poskytovatele mezipaměti, například [poskytovatele stavu relace pro Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Souhrn

Už víte, jak může aplikace opravte implementovat ukládání do mezipaměti za účelem zlepšení doba odezvy a škálovatelnost a umožňuje aplikaci pokračovat jako odpovídající pro operace čtení, když databáze není k dispozici. V [další kapitoly](queue-centric-work-pattern.md) ukážeme, jak dále zlepšit škálovatelnost a vytvoření aplikace pokračovat jako odpovídající pro operace zápisu.

## <a name="resources"></a>Prostředky

Další informace o ukládání do mezipaměti najdete v následujících materiálech.

Dokumentace

- [Mezipaměť Azure](https://msdn.microsoft.com/library/gg278356.aspx). Oficiální dokumentace MSDN na ukládání do mezipaměti v Azure.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/dn568099.aspx). Najdete pokyny ukládání do mezipaměti a doplňováním mezipaměti.
- [Bezporuchový: Pokyny pro odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper matolin Mercuri, Ulrich Homann a Andrew Townhill. Najdete v části na ukládání do mezipaměti.
- [Osvědčené postupy pro návrh rozsáhlých služeb v cloudu Azure služeb](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Dokument White paper moduly SIMM značky a Michael Thomassy. Najdete v části na distribuované ukládání do mezipaměti.
- [Distribuované ukládání do mezipaměti na cestě k škálovatelnost](https://msdn.microsoft.com/magazine/dd942840.aspx). Starší článek Časopis MSDN (2009), ale jasně napsané Úvod k distribuované ukládání do mezipaměti obecně; Klient se přepne do větší hloubky než na ukládání do mezipaměti části Dokumenty white paper bezporuchový a osvědčené postupy.

Videa

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Řada devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Představuje 400 úroveň zobrazení o tom, jak architektury cloudových aplikací. Tato řada se zaměřuje na teoreticky a důvody, proč; Další podrobnosti postupy najdete v tématu sestavování velkých řadu podle moduly SIMM značky. Přečtěte si diskuzi ukládání do mezipaměti v díl 3 počínaje 1:24:14.
- [Vytváření Big: Poučení vyplývající z Azure zákazníků – část I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies popisuje distribuované ukládání do mezipaměti začínající na 46:00. Podobně jako u řady bezporuchový ale přejde do další postupy podrobnosti. Prezentaci byl zadán 31. října 2012, takže nepokrývá ukládání do mezipaměti služby Web Apps v Azure App Service, která byla zavedena v 2013.

Ukázka kódu

- [Základy služby ve službě Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázkovou aplikaci, která implementuje distribuované ukládání do mezipaměti. Naleznete v příspěvku blogu doprovodné [cloudové služby základy – základní informace o ukládání do mezipaměti](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Předchozí](transient-fault-handling.md)
> [další](queue-centric-work-pattern.md)
