---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Návrh při selhání (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 01883cb0be3e7c7b5dc8d32b784ccb3a28652f1e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Návrh při selhání (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Jednou z věcí, budete muset vezměte v úvahu při vytváření jakéhokoli typu aplikace, ale hlavně jeden, který se spustí v cloudu, kde mnoha lidmi budete používat, je návrhu aplikace, aby mohli pohodlné zpracování chyb a nadále získávat hodnota co nejvíc možné. Dispozici dostatek času co se chystáte dojít k chybě v jakémkoli prostředí nebo jakéhokoli softwaru systému. Jak vaše aplikace zpracovává těchto situacích Určuje, jak nespokojený získají vašich zákazníků a kolik čas strávený analýza a odstraňování potíží.

## <a name="types-of-failures"></a>Typy chyb

Existují dvě kategorie základní chyb, které budete chtít jinak zpracovat:

- Dočasná samoopravení selhání například přerušované problémů s připojením.
- Trvalé selhání, které vyžadují zásah.

Pro přechodných chyb můžete implementovat zásady opakování zajistit, že ve většině případů aplikace obnoví rychle a automaticky. Vaši zákazníci mohou Všimněte si, trochu delší doby odezvy, ale jinak nebude mít vliv. Ukážeme některé způsoby, jak zpracovávat tyto chyby ve [přechodných chyb kapitoly](transient-fault-handling.md).

Pro trvalé selhání, můžete implementovat, sledování a protokolování funkce, které rychle vás upozorní, jakmile se vyskytnout potíže a které usnadňuje analýza hlavní příčiny. Ukážeme některé způsoby, jak si udržíte nad těchto druhů chyb v [monitorování a Telemetrie kapitoly](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Selhání oboru

Máte také myslet selhání oboru – jestli má vliv jeden počítač, celé služby, jako je SQL Database nebo úložiště nebo celou oblast.

![Selhání oboru](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Selhání počítače

V Azure novou automaticky nahrazuje server, který selhal a dobře navrženou cloudové aplikace zotavení z tento druh selhání rychle a automaticky. Dříve jsme zdůraznit škálovatelnost výhod bezstavové webovou vrstvu a snadné zotavení ze selhání serveru je Další výhodou statelessness. Usnadnění obnovení je také jednou z výhod platforma jako služba (PaaS) funkce jako je SQL Database a Azure App Service Web Apps. Selhání hardwaru vyskytují jen vzácně, ale pokud k nim dojde, že tyto služby zpracovat je automaticky. Nemáte i napsat kód pro zpracování chyby počítače, když používáte jednu z těchto služeb.

### <a name="service-failures"></a>Selhání služby

Cloudové aplikace se obvykle používají více služeb. Například aplikace opravte ji používá službu SQL Database, službu úložiště a webové aplikace je nasazená do služby Azure App Service. Co bude vaše aplikace dělat, když dojde k selhání jednoho služby, které závisí na? Pro některé služby selhání popisný "je nám líto, zkuste to znovu později" zpráva může být nejvhodnější můžete provést. Ale v mnoha případech můžete lépe provést. Například když back-end data store je vypnutý, můžete přijímají vstup uživatele, zobrazení, "vaše žádost byla přijata" a úložiště musí být zhruba else vstup dočasně; a pokud je služba potřebujete provozní znovu, můžete načíst vstupu a zpracovat.

[Zaměřené na fronty práci vzor](queue-centric-work-pattern.md) kapitoly ukazuje jeden ze způsobů tohoto scénáře. Aplikace opravte ukládá úkoly do databáze SQL, ale nemá ukončete pracovat, když databáze SQL je mimo provoz. V této kapitole vidíte ukládání uživatelský vstup pro úlohu ve frontě a používání pracovní proces fronty číst a aktualizovat úlohu. Pokud SQL je vypnutý, je schopnost vytvářet úlohy opravte ho neovlivní; pracovní proces můžete počkat a zpracovat nové úkoly, pokud databáze SQL je k dispozici.

### <a name="region-failures"></a>Selhání oblast

Celý oblastech mohou selhat. Přírodní katastrofě mohl poškodit datového centra, může získat průmětu podle meteor, trunk řádku do datového centra může vyjmout zemědělec burying krávy s rýpadlového atd. Pokud vaše aplikace je hostována v datovém centru stricken co můžete udělat? Je možné nastavit aplikaci v Azure můžou běžet v několika oblastech současně, takže pokud dojde k havárii v jednom, můžete pokračovat ve spouštění v jiné oblasti. Taková selhání jsou velmi výjimečných výskytů a většinu aplikací není přejít prostřednictvím obruče nezbytné, aby byla zajištěna nepřerušená činnost služby prostřednictvím selhání toto řazení. Najdete v části prostředky na konci kapitoly informace o tom, jak udržovat aplikace k dispozici i prostřednictvím selhání oblast.

Cílem Azure se provést všechny tyto druhy selhání bylo mnohem snazší zpracování a budete zde naleznete několik příkladů jak jsme se učinit v následujících kapitolách.

## <a name="slas"></a>SLA

Lidé často uslyšíme smlouvy o úrovni služeb (SLA) v prostředí cloudu. V podstatě jedná se o tom, jak spolehlivé své služby je lišící které společností. 99,9 %, které SLA znamená, že byste měli očekávat službu pravděpodobně nepracuje správně 99,9 % času. Který je docela typické hodnota pro SLA a vypadá jako velmi vysoké číslo ale nemusí zjistíte, jak dlouhá doba dolů. 1 % ve skutečnosti částek. Zde je tabulku, která ukazuje, kolik výpadek různých procent SLA částka přes rok, měsíc a týden.

![Tabulka SLA](design-to-survive-failures/_static/image2.png)

Proto 99,9 % SLA znamená služby může být mimo provoz 8.76 hodin v roce nebo 43,2 minut za měsíc. Který je více výpadek než většina lidí mějte na paměti. Takže jako vývojář chcete mějte na paměti, že je možné určité množství výpadek a zpracování řádně způsobem. V určitém okamžiku někdo se bude používat vaše aplikace a služby bude mimo provoz a chcete minimalizovat negativní dopad této na zákazníka.

Jednou z věcí byste měli vědět o SLA je jakém časovém horizontu odkazuje na: nemá hodiny získat resetovat, každý týden, každý měsíc nebo každý rok? V Azure můžeme resetovat hodiny každý měsíc, což je pro vás lépe než roční SLA, protože roční SLA může skrýt chybný měsíců tak, že je odsazení s řadou dobrý měsíců.

Samozřejmě jsme vždy aspire udělat lépe než SLA; Obvykle budete dolů mnohem méně než. Potenciálu je, že pokud se nám někdy dolů po dobu delší než maximální výpadek požádejte pro vrácení peněz. Množství peníze, kterou získáte zpět pravděpodobně nebude kompenzovat plně obchodní dopad nad výpadek, ale tento aspekt smlouvě SLA funguje jako vynucení zásad a vás informuje, že jsme vzít velmi vážně.

### <a name="composite-slas"></a>Složené SLA

Důležité myslet, když se díváte na SLA je dopad používání více služeb v aplikaci s každou službou nutnosti samostatné SLA. Například aplikace opravte používá Azure App Service Web Apps, úložiště Azure a SQL Database. Zde jsou čísla svých SLA od data, kdy tato e kniha probíhá zápis v prosinci 2013:

![Databáze SQL webový server, úložiště, SLA](design-to-survive-failures/_static/image3.png)

Jaký je maximální dolů čas, které očekáváte pro aplikace založené na tyto služby SLA? Může si myslíte, že vaše výpadkům budou rovna nejhorší procento SLA System nebo 99,9 % v tomto případě. Který by mít hodnotu true, pokud všechny tři služby se nezdařilo vždy ve stejnou dobu, ale který není nutně ve skutečnosti stane. Každá služba může selhat nezávisle v různou dobu, proto je nutné vypočítat složený SLA vynásobením jednotlivých čísel smlouvy o úrovni služeb.

![Složené SLA](design-to-survive-failures/_static/image4.png)

Tak, aby vaše aplikace může být mimo provoz nejen 43,2 minut za měsíc ale 3krát tato šířka, 108 minut za měsíc – a přesto být v rámci smlouvy SLA pro Azure.

Tento problém není jedinečný do Azure. Ve skutečnosti poskytujeme nejlepší cloudu SLA všechny cloudové služby, které jsou k dispozici, a budete mít podobný problémy řešení Pokud použití jakéhokoli dodavatele cloudových služeb. To ukazuje je význam přemýšlení o jak můžete navrhnout vaší aplikace pro zpracování selhání nevyhnutelné služby řádně ukončena, protože by se mohlo stát dostatečně často k mít vliv na vaše zákazníky nebo uživatele.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>SLA cloudu ve srovnání s enterprise době prostředí

Uživatelé někdy Řekněme, "V mé aplikace enterprise nikdy máte tyto problémy." Pokud požádáte kolik výpadkům na měsíc ve skutečnosti mají, obvykle říká, "Dobře, odehrává se příležitostně." A pokud požádejte jak často budou proud, "Někdy musíme zálohovat nebo instalaci nového serveru nebo aktualizace softwaru." Samozřejmě, který se zkracuje jako čas. Většinu firemních aplikací dokud je obzvláště důležité jsou ve skutečnosti dolů déle než dobu povolenou našich smluv SLA služby. Ale pokud je váš server a vaše infrastruktura a vy budete pro něj a v ovládacím prvku je odpovědný, zpravidla myslíte, že menší angst dobu mimo provoz. V cloudovém prostředí budete závisí na někdo jiný a nevíte, co se děje, takže mívají získat více obavy o něm.

Pokud organizace lze dosáhnout větší procento doby provozu než můžete získat z cloudu SLA, budou se provádí mnohem víc peníze výdaje na hardware. Cloudové služby to udělat, ale se účtují další pro jeho služby. Místo toho můžete využít finančně efektivní služby a navrhnout softwaru tak, aby nevyhnutelné selhání způsobit přerušení minimální zákazníkům. Úlohu jako návrháře cloudové aplikace není mnoho aby se zabránilo selhání, aby se zabránilo katastrofická a můžete udělat zaměřit na software, nikoli na hardwaru. Zatímco se maximalizovat střední čas mezi selhání zajistit podnikové aplikace, cloudových aplikací snažit minimalizovat střední čas obnovení.

### <a name="not-all-cloud-services-have-slas"></a>Ne všechny cloudové služby mají SLA

Uvědomte si také, Ne každé cloudové služby i má SLA. Pokud je aplikace závislá na službě se žádná záruka, doby provozu, může být mimo provoz daleko déle, než může Představte si. Například pokud povolíte přihlásit k webu pomocí zprostředkovatele sociálních třeba Facebook nebo Twitter, zkontrolujte u poskytovatele služeb a zjistěte, pokud je SLA, takže vám možná odhlašování došlo není. Ale pokud ověřovací služby ocitne mimo provoz nebo nemůže podporovat objem požadavků, jež throw na to, vaši zákazníci jsou zamčené z vaší aplikace. Může být mimo provoz, dnů nebo déle. Tvůrci jeden novou aplikaci stovky milionů souborů ke stažení, očekává a závislá na ověřování Facebook – ale nebyla komunikují s Facebook před přechodem za provozu a zjištěných příliš pozdní které došlo žádné SLA pro tuto službu.

### <a name="not-all-downtime-counts-toward-slas"></a>Ne všechny počty výpadek směrem k SLA

Některé cloudové služby může úmyslně odepření služby, pokud vaše aplikace používá přepsání je. To se označuje jako *omezení*. Pokud služba má SLA, má být uvedeno podmínky, za kterých může být omezené, a měli vyhnout tyto podmínky a náležitě reagovat na omezení, pokud se stane návrhu aplikace. Například pokud dojde k chybě při překročení určitého počtu za sekundu žádosti o službu, spusťte chcete Ujistěte se, že automatické opakované pokusy nenastávají tak rychle, že způsobí omezení pokračujte. Budeme mít více tedy o omezení v [přechodných chyb kapitoly](transient-fault-handling.md).

## <a name="summary"></a>Souhrn

Tato kapitola se pokusila můžete Uvědomte si, proč cloudové aplikace skutečných má být navržena k selhání řádně. Od verze [další kapitoly](monitoring-and-telemetry.md), zbývající vzory této série přejděte do více podrobností o několik strategií, můžete to udělat:

- Vhodné mít [monitorování a telemetrie](monitoring-and-telemetry.md), takže získáte informace rychle o selhání, které vyžadují zásah, a máte dostatek informací k jejich řešení.
- [Zpracování přechodných](transient-fault-handling.md) implementací inteligentního opakování logiku, tak, aby vaše aplikace obnoví automaticky při může a spadne zpět na [jistič](transient-fault-handling.md#circuitbreakers) logiku, když ji nelze.
- Použití [distribuované ukládání do mezipaměti](distributed-caching.md) aby se minimalizoval propustnosti, latenci a připojení problémy s přístupem k databázi.
- Implementace přijít spojovacích prostřednictvím [fronty způsob práce zaměřený](queue-centric-work-pattern.md)tak, aby vaše aplikace front-endu pokračovat v práci při back-end je vypnutý.

## <a name="resources"></a>Prostředky

Další informace najdete v dalších kapitolách v této e knihy a následující prostředky.

Dokumentace:

- [Bezporuchový: Pokyny pro odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper matolin Mercuri, Ulrich Homann a Andrew Townhill. Webová stránka verze série videí bezporuchový.
- [Osvědčené postupy pro návrh rozsáhlých služeb v cloudu Azure služeb](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper moduly SIMM značky a Michael Thomassy.
- [Azure obchodní kontinuity technické pokyny](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Dokument White paper Patrik Wickline a Jason Roth.
- [Zotavení po havárii a vysoká dostupnost pro aplikace Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Dokument White paper Michael McKeown, Hanu Kommalapati a Jason Roth.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/dn568099.aspx). Viz příručka nasazení více datového centra, jistič vzor.
- [Podpora Azure - smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
- [Kontinuita podnikových procesů v Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentaci k SQL Database vysokou dostupnost a po havárii funkce obnovení.
- [Vysoká dostupnost a zotavení po havárii pro SQL Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videa:

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Řada devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Uvede základními koncepty a architektury zásady způsobem, velmi přístupné a zajímavé, s scénářů čerpají z prostředí Microsoft zákazníka poradní tým (CAT) s skutečným zákazníkům. Díly 1 a 8 zabývají podrobněji důvody pro navrhování cloudové aplikace při selhání. Další informace najdete v části následné diskusní omezování v díl 2 počínaje 49:57, diskuzi o selhání body a selhání režimy v díl 2 počínaje 56:05 a diskuzi o moduly dělení na okruh v díl 3 počínaje 40:55.
- [Vytváření Big: Poučení vyplývající z Azure zákazníků – část II](https://channel9.msdn.com/Events/Build/2012/3-030). Moduly SIMM označit zmíněn návrhu pro selhání a instrumentace vše. Podobně jako u řady bezporuchový ale přejde do další postupy podrobnosti.

> [!div class="step-by-step"]
> [Předchozí](unstructured-blob-storage.md)
> [další](monitoring-and-telemetry.md)
