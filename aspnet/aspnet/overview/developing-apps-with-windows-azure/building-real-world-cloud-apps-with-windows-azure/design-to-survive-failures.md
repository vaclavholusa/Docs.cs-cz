---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Návrh odolný proti chybám (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 1098892c621b8add624b18788076be2fac1cc158
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751811"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Návrh pro selhání (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Jednou z věcí, které mají zvážit při vytváření jakéhokoli typu aplikace, ale hlavně jeden, který se spustí v cloudu, ve kterém velké množství lidí použití, jak navrhovat aplikace tak, aby můžete elegantně zpracovat selhání a dál přinášet nemalé hodnoty je co nejvíce je to možné. Zadaný dostatek času, co se chystáte dojde k problémům v jakémkoli prostředí nebo v každém softwarovém systému. Způsob, jakým aplikace zpracovává tyto situace Určuje, jak nespokojený vaši zákazníci můžou získat a jak dlouho budete muset věnovat analýza a řešení problémů.

## <a name="types-of-failures"></a>Typy chyb

Existují dvě základní kategorie chyb, které budete chtít zpracovat jinak:

- Přechodná, opravy chyb, jako jsou problémy s připojením sítě.
- Trvalou chyby, které vyžadují zásah.

Pro přechodná selhání můžete implementovat zásady opakování zajistit, že ve většině případů obnoví aplikace rychle a automaticky. Může si vaši zákazníci o něco delší dobu odezvy, ale jinak nebude mít vliv. Vám ukážeme některé způsobů zpracování těchto chyb v [zpracování přechodných chyb kapitoly](transient-fault-handling.md).

Pro enduring selhání, můžete implementovat monitorování a protokolování funkci, která okamžitě vás upozorní, když vzniku a, která usnadňuje analýzu původní příčiny. Vám ukážeme některé způsoby, jak vám připněte si tyto druhy chyb v [monitorování a Telemetrie kapitoly](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Rozsah selhání

Taky je potřeba uvažovat o selhání oboru – ať už je vliv na jeden počítač, celou službu jako je SQL Database nebo úložiště nebo celou oblast.

![Rozsah selhání](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Selhání počítače

V Azure selhání serveru je automaticky nahrazena novou a dobře navržené cloudové aplikace zotavení z tohoto typu selhání automaticky a rychle. Dříve jsme zdůraznit výhod škálovatelnosti bezstavové webové vrstvy a Další výhodou statelessness je snadné zotavení ze selhání serveru. Snadné obnovení je také jednou z výhod platforma jako služba (PaaS) funkce, jako je SQL Database a Azure App Service Web Apps. Selhání hardwaru, které se vyskytují jen vzácně, ale pokud k nim dojde, že tyto služby je umožňují automaticky zpracovat; dokonce nemusíte psát kód pro zpracování chyb počítače, když pracujete s některou z těchto služeb.

### <a name="service-failures"></a>Chyby služby

Cloudové aplikace obvykle používají více služeb. Například aplikace Fix It používá službu SQL Database, službu úložiště a webové aplikace se nasadí do služby Azure App Service. Co se vaše aplikace dělat, když dojde k selhání jednoho služby, které závisí na? Pro některé služby selhání popisný "je nám líto, zkuste to znovu později" zpráva může být nejlepší, vám pomůžou. Ale v mnoha scénářích vám pomůžou lépe. Například když váš back endovým datům úložiště je vypnutý, můžete přijímají vstup uživatele, zobrazit "vaše žádost byla přijata" a dočasně; ukládání vstup musí být zhruba else potom když službu, potřebujete opět v provozu, můžete načíst vstupní a zpracovat je.

[Vzor založený na frontě pracovní](queue-centric-work-pattern.md) kapitoly ukazuje jeden ze způsobů tímto scénářem poradit. Aplikace Fix It ukládá úkoly do databáze SQL, ale nemusí to přestane pracovat po SQL Database je mimo provoz. V této kapitole uvidíme, jak uložit uživatelský vstup pro úlohu ve frontě a používat pracovní proces frontě číst a aktualizovat úlohu. Pokud SQL je vypnutý, není ovlivněn; schopnost vytvářet úkoly Fix It pracovní proces může čekat a zpracovat nové úkoly v případě, že SQL Database je dostupná.

### <a name="region-failures"></a>Selhání oblasti

Celé oblasti může selhat. Přírodní katastrofě může zničit datového centra, může získat sloučí podle meteor službě, může být snížili řádku trunk do datového centra dobytka burying krávy s rýpadlového atd. Pokud je vaše aplikace hostovaná v stricken datacenter co dělat? Je možné nastavit vaši aplikaci v Azure ke spuštění v několika oblastech současně, takže pokud nedojde k havárii v jednom, bude možné i nadále spuštěna v jiné oblasti. Tyto chyby jsou velmi vzácné výskyty a většina aplikací není přejít přes obruče nezbytné k zajištění nepřerušovaného službě přes tento typ selhání. V části prostředky na konci kapitoly informace o tom, jak vaše aplikace, klidně i prostřednictvím selhání oblasti k dispozici.

Cílem Azure je zpracování všem těmto typům selhání mnohem jednodušší a uvidíte několik příkladů, jak vám to jde, který v následujících kapitolách.

## <a name="slas"></a>Smlouvy o úrovni služeb

Lidé často poslechněte si o smlouvy o úrovni služeb (SLA) v cloudovém prostředí. V podstatě jde o příslibů, které usnadňují společností o tom, jak spolehlivé příslušnou službu. 99,9 % smlouva SLA znamená, že jste byste očekávat, že služba funguje správně 99,9 % času. To je poměrně Typická hodnota pro smlouvu SLA a vypadá podobně jako velmi velký počet, ale možná zjistíte, kolik času dolů. 1 % ve skutečnosti částek. Tady je tabulka, která ukazuje, jak velká Doba výpadku různých procentní smlouvy SLA částka za rok, měsíc a za týden.

![Smlouva SLA tabulky](design-to-survive-failures/_static/image2.png)

Proto 99,9 % smlouva SLA znamená, že vaše služba může být mimo provoz 8.76 hodin za rok nebo 43,2 minut za měsíc. To je více časové prodlevy než většina lidí realizovat. Tak jako vývojář chcete mějte na paměti, že je možné určité množství časové prodlevy a bezproblémové způsobem ji zpracovat. V určitém okamžiku někdo se bude používat vaše aplikace a služby se to být mimo provoz a chcete minimalizovat negativní dopad, který na zákazníka.

Je jedna věc, kterou byste měli vědět o SLA jakém časovém horizontu, na který odkazuje: funkci hodin získat resetování, každý týden, každý měsíc nebo každý rok? V Azure jsme obnovit platit každý měsíc, což je pro vás lepší než roční SLA, protože roční SLA může skrýt slabé měsíce započtení s řadou dobré měsíců.

Samozřejmě vždy držet udělat lépe než SLA; Obvykle budete dolů mnohem menší než. Uskutečnění je, že pokud jsme pro vás někdy dolů po dobu delší než maximální časové prodlevy můžete požádat o vrácení peněz. Množství peníze, které získáte zpět pravděpodobně nebude kompenzovat plně dopad na chod firmy překročení časové prodlevy, ale tento aspekt smlouvy SLA funguje jako penále za a oznámí vám, že jsme jít velmi vážně.

### <a name="composite-slas"></a>Složené smlouvy SLA

Důležité zamyslet, když se díváte na smlouvy SLA je dopad pomocí několika služeb v aplikaci, přičemž každá služba má samostatné smlouvy SLA. Například aplikace Fix It používá Azure App Service Web Apps služby Azure Storage a SQL Database. Tady jsou jejich čísla SLA k datu, které tato elektronická kniha se zápisem v prosinci, 2013:

![Smlouva SLA webové stránky, Storage, SQL Database](design-to-survive-failures/_static/image3.png)

Jaký je maximální výpadek podle vašeho očekávání pro aplikace založené na těchto smluv SLA služby? Si možná myslíte, že vaše dolů by se rovná nejhorší procento SLA nebo 99,9 % v tomto případě. Který bude mít hodnotu true, pokud všechny tři služby se nezdařilo vždy ve stejnou dobu, ale to není nutně co přesně se stane. Každá služba může selhat nezávisle na sobě v různou dobu, abyste měli k výpočtu Složená hodnota SLA vynásobením jednotlivých čísel smlouvu SLA.

![Složená hodnota SLA](design-to-survive-failures/_static/image4.png)

Aby vaše aplikace může být mimo provoz nejen 43,2 minut za měsíc, ale 3násobek velikosti, 108 minut za měsíc – a v rámci smlouvy SLA pro Azure i nadále.

Tento problém není jedinečný pro Azure. Ve skutečnosti zajišťuje nejlepší cloud SLA kteroukoli cloudovou službu, která je k dispozici a bude nutné podobné problémy řešit, pokud používáte libovolného dodavatele cloudových služeb. To ukazuje je důležitost přemýšlet o tom, jak můžete navrhnout vaše aplikace pro zpracování selhání nevyhnutelné služby bez výpadku, protože může dojít dostatečně často dopad na vaše zákazníky nebo uživatele.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Cloud, smlouvy o úrovni služeb ve srovnání s enterprise výpadkům prostředí

Uživatelé někdy říct "Ve své aplikaci enterprise mám nikdy k dispozici tyto problémy." Po zadání otázky kolik výpadkům v měsíci ve skutečnosti mají, jsou obvykle například "Dobře, to se stane, čas od času." A po zadání otázky jak často se uznat, který "Někdy potřebujeme zálohovat nebo instalaci nového serveru nebo aktualizace softwaru." Samozřejmě, které se počítá jako časové prodlevy. Většina podnikových aplikací Pokud jsou obzvláště důležité jsou ve skutečnosti dolů déle než dovolují naše smlouvy SLA služby časového intervalu. Ale když je váš server a infrastrukturu, budete pro něj a kontrolu nad jeho budete cítit, že menší angst dolů časy. V cloudovém prostředí budete závisí na někdo jiný a nevíte co se děje, takže může vést k získání více obavy o něm.

Organizace dosahuje větší procento doby provozu než získáte z cloudu smlouvy SLA, dělají to podle mnohem víc peníze výdaje na hardware. Cloudovou službu to udělat, ale byste museli mnoho dalšího pro jeho služby účtovat. Místo toho využít nákladově efektivní služby a návrh vašeho softwaru tak, aby nevyhnutelné selhání způsobit přerušení minimální vašim zákazníkům. Úlohy při návrhu aplikace cloud není tak velká, aby selhání, aby se zabránilo katastrofická a můžete to udělat díky zaměření na softwaru, ne na hardware. Vzhledem k tomu, přitom se snaží maximalizovat střední doby mezi poruchami podnikové aplikace, cloudové aplikace snažit minimalizovat průměrný čas potřebný k obnovení.

### <a name="not-all-cloud-services-have-slas"></a>Ne všechny cloudové služby mají smlouvy o úrovni služeb

Upozorňujeme také, že ne každé cloudové službě ještě nemá smlouvu SLA. Pokud je vaše aplikace závislá na službě s neposkytujeme záruku doby provozu, může být mimo provoz mnohem déle, než může být imagine. Například pokud povolíte přihlašování k webu pomocí poskytovatele sociálních sítí, jako je Facebook nebo Twitter, obraťte se na poskytovatele služeb, které chcete zjistit, pokud je SLA a může pro vás tam není. Ale pokud ověřovací službu ocitne mimo provoz nebo je schopen množství žádostí, abyste vyvolali na to, které podporují, jsou zamknuté vaši zákazníci z vaší aplikace. Dnů nebo i delší dobu, může být mimo provoz. Autoři jednu novou aplikaci očekával stovky milionů souborů ke stažení a závislá na ověřování sítě Facebook –, ale nepovedlo komunikovat s Facebooku nepřejdou za provozu a zjištěných příliš pozdě, které se žádná smlouva SLA pro tuto službu.

### <a name="not-all-downtime-counts-toward-slas"></a>Ne všechny počty výpadek směrem k smlouvy o úrovni služeb

Některé cloudové služby může záměrně odepřít služby v případě, že aplikace nadměrně používá. Tento postup se nazývá *omezování*. Pokud služba má smlouvu SLA, by mělo být uvedeno podmínky, za kterých může být omezený a návrhu vaší aplikace měli vyhnout tyto podmínky a náležitě reagovat na omezení, pokud se to stane. Například pokud požadavky na službu začínají selhávat po překročení určitého počtu za sekundu, chcete zajistit, aby automatické opakování pokusů nedojde tak rychle, mohou způsobit omezení pokračujte. Budeme mít více říkají o omezování v [zpracování přechodných chyb kapitoly](transient-fault-handling.md).

## <a name="summary"></a>Souhrn

Tato kapitola nepokusí umožňují Uvědomte si, proč reálného světa cloudové aplikace má být navržena k překonání selhání bez výpadku. Počínaje [další kapitolu](monitoring-and-telemetry.md), zbývající vzory v této sérii přejít na podrobnější informace o několik strategií, můžete to udělat:

- Mít kvalitní [monitorování a telemetrie](monitoring-and-telemetry.md), kde zjistíte, rychle o selhání, které vyžadují zásah, a máte dostatek informací k jejich řešení.
- [Zpracování přechodných chyb](transient-fault-handling.md) implementací logiku opakování inteligentní, aby vaše aplikace obnoví automaticky při může a spadne zpět na [jistič](transient-fault-handling.md#circuitbreakers) logiku, když ji nelze.
- Použití [distribuované ukládání do mezipaměti](distributed-caching.md) za účelem minimalizace připojení, latence a propustnosti problémy s přístupem k databázi.
- Volné párování prostřednictvím implementace [pracovní postup založený na frontě](queue-centric-work-pattern.md)tak, aby front-endu vaší aplikace můžete i nadále fungovat, když back-endu je mimo provoz.

## <a name="resources"></a>Prostředky

Další informace najdete v dalších kapitolách tuto elektronickou příručku a následující prostředky.

Dokumentace ke službě:

- [Bezporuchový: Pokyny, odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper Marc Mercuri, Ulrich Homann a Andrew Townhill. Webová stránka verze bezporuchový videu z řady.
- [Osvědčené postupy pro navrhování rozsáhlých služeb v Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper Mark Simms a Michael Thomassy.
- [Technické pokyny Azure obchodní kontinuity podnikových procesů](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Dokument White paper Patrick Wickline a Jason Roth.
- [Zotavení po havárii a vysoká dostupnost pro aplikace Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Dokument White paper Michael McKeown, Hanu Kommalapati a Jason Roth.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobrazit pokyny k nasazení s více datového centra, jističe.
- [Podpora Azure – smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
- [Kontinuita podnikových procesů ve službě Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentaci ke službě SQL Database vysokou dostupnost a funkce pro obnovení.
- [Vysoká dostupnost a zotavení po havárii pro SQL Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videa:

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri a Mark Simms devět částí série. Nabídne základními koncepty a Principy architektury tak vysoce dostupné a zajímavé příběhy z prostředí Microsoft zákazníka poradní tým (CAT) se skutečným zákazníkům. Epizody 1 a 8 pohledu do hloubky důvody pro vývoj cloudových aplikací při selhání. Další informace najdete v článku následné diskuzi o omezování v epizoda 2 počínaje 49:57 diskuzi o bodů selhání a režimů selhání v epizoda 2 počínaje 56:05 a diskuzi o jističe v epizodě 3 počínaje 40:55.
- [Vytváření Big: Získané od zákazníků Azure – část II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms se hovoří o návrh pro selhání a všechno, co instrumentace. Podobně jako řady bezporuchový ale přejde do další postupy podrobnosti.

> [!div class="step-by-step"]
> [Předchozí](unstructured-blob-storage.md)
> [další](monitoring-and-telemetry.md)
