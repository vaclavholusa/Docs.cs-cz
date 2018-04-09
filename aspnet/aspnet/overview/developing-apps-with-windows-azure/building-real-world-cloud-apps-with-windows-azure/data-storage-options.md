---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Možnosti ukládání dat (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: d638dca331cb24c340a4471e5964a00b75bb608a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Možnosti ukládání dat (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Většina lidí se používají k relačních databází a jejich mívají přehlédnout jiné možnosti ukládání dat při jejich navrhování cloudové aplikace. Výsledkem mohou být zhoršené výkon, vysokou výdaje nebo programy, protože [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (nerelačních) databáze může zpracovat některé úlohy efektivnější než relačních databází. Když zákazníci nám požádat o pomoc při řešení problému úložiště důležitých dat, je to často protože mají relační databázi, kde jednu z možností NoSQL by již dříve pracovali lépe. V těchto situacích Zákazník by byl lepší vypnout, pokud měl implementují řešení NoSQL před nasazením aplikace do produkčního prostředí.

Na druhé straně je chyba předpokládají, že databáze NoSQL může dělat všechno, dobře nebo dostatečně dobře. Neexistuje žádný jeden nejlepší dat správy výběr pro všechny úlohy úložiště dat; různé datové řešení pro správu jsou optimalizované pro různé úlohy. Většina reálných cloudových aplikací mít různé požadavky na úložiště dat a jsou často obsloužených nejlepší kombinaci více řešení úložiště pro data.

Účelem této kapitoly je poskytnout širší představu o dostupných možnostech úložiště dat pro cloudové aplikace a některé základní informace o tom, jak vybrat ty, které vyhovovat vašemu scénáři. Je nejlepší mít přehled o dostupných možností a myslíte o jejich silné a slabé stránky před vyvíjíte aplikaci. Změna možnosti ukládání dat v produkční aplikace může být velmi náročné, jako je nutnosti změny modul jet, když rovině přecházejí.

## <a name="data-storage-options-on-azure"></a>Možnosti ukládání dat v Azure

Cloudu je poměrně snadné ho použít celou řadu relační a úložiště dat typu NoSQL. Zde jsou některé platformy úložiště dat, které můžete použít v Azure.

![](data-storage-options/_static/image1.png)

V tabulce jsou uvedeny čtyři typy databáze NoSQL:

- [Klíč/hodnota databáze](https://msdn.microsoft.com/library/dn313285.aspx#sec7) ukládání jedné serializovaný objekt pro každou hodnotu klíče. Jsou vhodné pro ukládání velkých objemů dat, kde chcete získat jednu položku pro dané hodnoty klíče a nemáte k dotaz založený na jiné vlastnosti položky.

    [Azure Blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) je databáze klíč/hodnota, která funguje jako soubor úložiště v cloudu, pomocí hodnoty klíče, které odpovídají názvů složek a souborů. Můžete načíst soubor svým složku a název souboru, ne podle hledání podle hodnot v souboru obsahu.

    [Azure Table storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) je také databázi klíč/hodnota. Je každá hodnota volána *entity* (podobně jako na řádek, určený klíč oddílu a klíč řádku) a obsahuje více *vlastnosti* (podobná sloupců, ale ne všechny entity v tabulce mají sdílet stejné sloupce). Dotazování na sloupce mimo klíč je velmi neefektivní a je nutno. Můžete například uložit data uživatelského profilu, s jedním oddílem ukládání informací o jednoho uživatele. Data, jako jsou uživatelské jméno, hodnoty hash hesla, datum narození a tak dále, může uložit v samostatných vlastnosti jednu entitu nebo v samostatné entity ve stejném oddílu. Ale byste neměli chtít dotazu pro všechny uživatele s zadaný rozsah data narození a nelze spustit dotaz spojení mezi tabulku profil a jinou tabulkou. Table storage je větší škálovatelnost a levnější než relační databázi, ale neumožňuje složitých dotazů nebo spojení.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) jsou databáze klíč/hodnota, ve kterých jsou hodnoty *dokumenty*. "Dokumentu" zde nepoužívá v tom smyslu, dokumentu Word či Excel, ale znamená kolekci s názvem pole a hodnoty, každá z nich může být podřízené dokumentu. Například v tabulce historie pořadí může mít dokument pořadí pořadové číslo, datum pořadí a pole zákazníka; a pole může obsahovat pole název a adresu. Databáze kóduje pole data ve formátu například JSON, XML, YAML nebo BSON; nebo můžete použít prostý text. Jeden funkce, která nastavuje dokumentové databáze kromě databáze klíč/hodnota je možnost zadat dotaz na pole neklíčový a definovat sekundární aby dotazování efektivnější. Tato možnost je nejvhodnější pro aplikace, které potřebují k načtení dat na základě kritérií složitější než hodnota klíč dokumentu databázi dokumentů. V databázi dokumentů historie prodejní objednávky můžete například dotazovat na různá pole, jako je například ID produktu, ID zákazníka, jméno zákazníka a tak dále. [MongoDB](http://www.mongodb.org/) je databáze oblíbených dokumentu.
- [Rodin sloupců databáze](https://msdn.microsoft.com/library/dn313285.aspx#sec9) jsou klíč/hodnota úložiště dat, která umožňují struktura ukládání dat do kolekcí související sloupce názvem rodin sloupců. Například databáze úplné zjišťování může mít jednu skupinu sloupců pro jméno osoby (první, střední, poslední), jedna skupina pro adresu osoby a jednu skupinu pro informace o profilu osoby (DOB, pohlaví, atd.). Databáze pak můžete uložit rodiny každý sloupec v samostatném oddílu při zachování všech dat pro jedna osoba související se stejným klíčem. Pak si můžete přečíst informace o profilech všech bez nutnosti pročtěte všechny také informace název a adresu. [Cassandra](http://cassandra.apache.org/) je Oblíbené rodin sloupců databáze.
- [Graf databáze](https://msdn.microsoft.com/library/dn313285.aspx#sec10) ukládání informací jako kolekce objekty a vztahy. Účelem databáze grafu je povolení aplikace k efektivnímu provádění dotazů, které překračují sítě objekty a vztahy mezi nimi. Například objekty může být zaměstnanci v databázi lidských zdrojů, a může být vhodné pro usnadnění dotazy, jako "najít všechny zaměstnance, kteří pracují přímo nebo nepřímo pro Scott." [Neo4j](http://www.neo4j.org/) je databáze oblíbených grafu.

Ve srovnání s relačních databází, možnosti NoSQL nabízí mnohem větší škálovatelnost a efektivity nákladů pro úložiště a analýzu Nestrukturovaná data. Kompromis je, že nemáte poskytovat bohaté queryability a robustní data integritní schopnosti relačních databází. NoSQL funguje dobře pro data protokolu služby IIS, který zahrnuje velkému s není nutné pro připojení k dotazy. NoSQL nebude fungovat tak dobře pro bankovnictví transakce, který vyžaduje absolutní data integrity a zahrnuje mnoho relace s jinými daty souvisejícím s účtem.

Je také novější kategorie databáze platforma názvem [NewSQL](http://en.wikipedia.org/wiki/NewSQL) který kombinuje škálovatelnost databáze NoSQL s queryability a transakční integritu relační databáze. NewSQL databáze jsou navrženy pro distribuované úložiště a zpracování dotazů, což je často obtížné implementovat v databázích "OldSQL". [NuoDB](http://www.nuodb.com/) je příkladem NewSQL databázi, která lze použít v Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop a MapReduce

Velký objem dat, která můžete ukládat do databáze NoSQL může být obtížně analyzovat efektivně včas. Uděláte to, které můžete použít rozhraní jako [Hadoop](http://hadoop.apache.org/) které implementuje [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funkce. V podstatě nemá MapReduce proces je následující:

- Limit velikost dat, která je potřeba zpracovat tak, že vyberete mimo data uložit pouze data, která skutečně potřebujete k analýze. Například chcete vědět způsob vytvoření vaše uživatele základní roku narození, takže vybrat pouze narození roků mimo vaší úložiště dat profilu uživatele.
- Rozdělení dat do části a jejich odeslání pro zpracování různých počítačích. Počítač A vypočítá počet lidí s daty 1950 1959, počítač B nemá 1960-1969, atd. Tato skupina počítačů se nazývá *clusteru Hadoop*.
- Po dokončení zpracování na části, uveďte výsledky každé části zpět společně. Nyní máte poměrně krátké seznam kolik uživatelé pro každý rok narození a úlohy Výpočet procenta v tomto seznamu celkové se dá spravovat.

V Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/) umožňuje zpracovat, analyzovat a distribučních z velkých objemů dat pomocí power systému Hadoop. Například může použít k analýze protokolů webového serveru:

- Povolení protokolování webového serveru k účtu úložiště. Toto nastaví Azure tak, aby zápis protokolů služby objektů Blob pro každý požadavek HTTP do vaší aplikace. Služba objektů Blob je v podstatě souborového úložiště v cloudu a vhodně integruje s HDInsight. 

    ![Protokoly do úložiště objektů Blob](data-storage-options/_static/image2.png)
- Jak aplikace získá provoz, protokoly webového serveru IIS se zapisují do úložiště objektů Blob. 

    ![Protokoly webového serveru](data-storage-options/_static/image3.png)
- Na portálu, klikněte na tlačítko **nový** - **datové služby** - **HDInsight** - **rychle vytvořit**, a zadejte název clusteru HDInsight, velikost clusteru (počet dat uzlech clusteru HDInsight) a uživatelské jméno a heslo pro HDInsight cluster. 

    ![HDInsight](data-storage-options/_static/image4.png)

Nyní můžete nastavit úloh MapReduce k analýze protokolů a získejte odpovědi na otázky, jako například:

- Jaké denní dobu mé aplikace získat nejvyšší nebo nejnižší provozu?
- Jaké zemích je můj provoz přicházející ze?
- Co je průměrná okolí příjmy Moje provoz pochází z oblasti. (Je veřejný datovou sadu, která umožňuje příjem Okolní počítače podle IP adresy a který může odpovídat na IP adresu do protokolů webového serveru.)
- Jak okolí příjmu korelující na konkrétní stránky nebo produkty v lokalitě?

Pak můžete použít odpovědi na otázky jako do cílové reklamy podle pravděpodobnost, kterou zákazník by zajímat nebo nakoupit určitého produktu.

Jak je popsáno v [automatizovat všechno kapitoly](automate-everything.md), je možné automatizovat většinu funkcí, které můžete provést na portálu a který zahrnuje nastavení a provádění úloh analysis HDInsight. Typického skriptu HDInsight může obsahovat následující kroky:

- Zřízení clusteru služby HDInsight a tu propojit na účtu úložiště pro vstup úložiště objektů Blob.
- Nahrajte spustitelné soubory úlohy MapReduce (soubory .exe nebo .jar) do clusteru HDInsight.
- Odešlete MapReduce, který uloží výstupní data do úložiště objektů Blob.
- Počkejte na dokončení úlohy.
- Odstranění clusteru HDInsight.
- Přístup k výstupu z úložiště objektů Blob.

Spuštěním skriptu, který nemá všechny to minimalizujete množství času, který je zajištěný clusteru HDInsight, což minimalizuje náklady.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platforma jako služba (PaaS) a infrastruktury jako služby (IaaS)

Výše uvedených možností úložiště dat obsahovat platformy jako služba (PaaS) i řešení infrastruktury jako služba (IaaS). PaaS znamená, že jsme Správa hardwaru a softwaru infrastruktury a právě používáte službu. Databáze SQL je PaaS funkce Azure. Požádejte pro databáze, a na pozadí Azure nastavuje a nakonfiguruje virtuálních počítačů a nastaví databáze je. Nemají přímý přístup k virtuálním počítačům a nemusíte spravovat. IaaS znamená, že nastavení, konfigurovat a spravovat virtuální počítače, které běží v našem infrastruktuře datového centra a vložíte všechno na ně. Poskytujeme galerie předem nakonfigurovaná Image virtuálních počítačů pro běžné konfigurace virtuálního počítače. Můžete například nainstalovat předem nakonfigurovaná Image virtuálních počítačů pro Windows Server 2008, Windows Server 2012, BizTalk serveru, Oracle WebLogic Server, databáze Oracle, atd.

Řešení pro data PaaS, které Azure nabízí, patří:

- Azure SQL Database (dříve označované jako SQL Azure). Cloud relační databáze založené na systému SQL Server.
- Azure Table storage. Klíč/hodnota, databáze NoSQL.
- Úložiště objektů Blob Azure. Úložiště souborů v cloudu.

Pro IaaS můžete spustit nic, co můžete načíst do virtuálního počítače, například:

- Relační databáze jako je například SQL Server, Oracle, MySQL, SQL Compact, SQLite nebo Postgres.
- Úložiště dat klíč/hodnota, například protokolem Memcache, Redis, Cassandra a Riak.
- Úložiště sloupce dat, jako je například HBase.
- Například MongoDB, RavenDB a CouchDB dokumentové databáze.
- Graf databáze například Neo4j.

![Možnosti ukládání dat v Azure](data-storage-options/_static/image5.png)

Možnost IaaS nabízí možnosti úložiště téměř neomezená data na úrovni a většina z nich je obzvláště snadno použitelný, vzhledem k tomu, že můžete vytvořit pomocí předem nakonfigurovaných bitové kopie virtuálních počítačů. Například v portálu pro správu přejděte do **virtuální počítače**, klikněte na tlačítko **bitové kopie** a klikněte na **procházet skladu virtuálních počítačů**.

![Procházet skladu virtuálních počítačů](data-storage-options/_static/image6.png)

Zobrazí seznam [stovky předkonfigurované Image virtuálních počítačů](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), a můžete vytvořit virtuální počítač z obrázku, který má systém správy databáze předinstalována, jako jsou například MongoDB, Neo4J, Redis, Cassandra nebo CouchDB:

![MongoDB ve skladu virtuálních počítačů](data-storage-options/_static/image7.png)

Azure umožňuje snadno použít stejně jako možné IaaS možnosti ukládání dat, ale nabídky PaaS mít celou řadu výhod, díky kterým více nákladově efektivní a praktické pro mnoho scénářů:

- Nemáte k vytvoření virtuálních počítačů, stačí použít na portálu nebo skript k nastavení úložiště dat. Pokud chcete 200 terabajt úložiště dat, jste právě klikněte na tlačítko nebo spustit příkaz, a v sekundách je připraven k použití.
- Nemusíte spravovat ani opravovat virtuální počítače, používá služba; Microsoft nemá který pro vás automaticky.-nemusíte si dělat starosti o nastavení infrastruktury pro škálování nebo vysokou dostupnost; Microsoft zpracovává všechno, co pro vás.
- Nemáte zakoupit licence; licenční poplatky jsou součástí služby poplatky.
- Platíte jenom se používá.

Mezi možnosti úložiště dat PaaS v Azure patří nabídky zprostředkovatelé třetí strany. Například můžete [MongoLab rozšíření](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) z úložiště Azure, které chcete zřídit databázi MongoDB jako služba.

## <a name="choosing-a-data-storage-option"></a>Volba možnosti úložiště dat

Žádné jeden ze způsobů je nejvhodnější pro všechny scénáře. Pokud každý, kdo říká, že tato technologie je odpověď, je nejprve si požádejte "Jaké jsou otázka?", protože různá řešení jsou optimalizované pro různých věcí. Určit přesně výhod relační modelu; To je důvod, proč je k dispozici tak dlouho. Ale existují také nižší stranách do SQL, které lze řešit s řešením NoSQL.

Často co vidíte pracovní nejlepší je složení přístup, kde používáte SQL a NoSQL v jediném řešení. I když obecně se říká, budou se osvojují NoSQL, pokud přejdete do jednotlivé kroky můžete často najít, že používají několik různých rozhraní NoSQL: používají [CouchDB](http://wiki.apache.org/couchdb/Introduction), a [Redis](http://redis.io/)a [ Riak](http://basho.com/riak/) pro různých věcí. I Facebook, který je hojně používá NoSQL, používá jiný architektury NoSQL pro různé části služby. Možnost kombinovat a párovat přístupy úložiště dat je jednou z věcí, které je dobrý o cloudu, protože se snadno používat více dat řešení a integrovat je v jediné aplikaci.

Tady je pár otázek myslet, když zvolíte přístup:

| Sémantické dat | -Co je základní úložiště a dat. přístupu k datům sémantického (jsou se ukládat relační nebo nestrukturovaných dat)? Nestrukturovaných dat, jako jsou například soubory médií nejlépe vyhovuje v úložišti objektů blob; kolekce související data, jako jsou produkty, inventáře, dodavatelů, objednávek zákazníků, atd., nejlépe vyhovuje v relační databázi. |
| --- | --- |
| Podporu dotazů | -Jak snadné je k dotazování dat? -Typy dotazů možné efektivně výzva? Úložiště dat klíč/hodnota jsou velmi dobře při získání jednoho řádku zadaný s hodnotou klíče, ale není proto vhodný pro složité dotazy. Pro úložiště dat profilu v uživatele kde se vždy zobrazuje data pro jeden konkrétní uživatel úložiště dvojic klíč/hodnota dat může fungovat i; pro katalog produktů, ve které chcete získat různé seskupení na základě různých produktů atributů může lépe pracovat relační databáze. Databáze NoSQL může ukládat velké objemy dat efektivní, ale budete muset struktury databáze kolem jak aplikace dotazuje data a díky tomu ad hoc dotazy těžší udělat. S relační databázi můžete vytvořit téměř jakýkoli druh dotazu. |
| Funkční projekce | -Můžete otázky, agregace atd., být prováděny serverovou? Pokud spuštění vyberte počet (\*) z tabulky v SQL, bude velmi efektivně udělají tuto práci na serveru a vrátí číslo hledám. Pokud chci stejný výpočet z úložiště dat typu NoSQL, která nepodporuje agregace, to je neefektivní "bez vazby dotazu" a pravděpodobně bude časový limit. I v případě, že dotaz je úspěšné nutné načíst všechna data ze serveru do klienta a počet řádků na straně klienta. -Jaké jazyky nebo typy výrazů můžete používat? Relační databáze I pomocí SQL. U některých NoSQL databází, jako je například Azure Table storage, I budete používat [OData](http://www.odata.org/), a všechny můžete udělat, je filtrovat podle primární klíč a získat projekce (vybrat podmnožinu dostupných polí). |
| Snadná škálovatelnost | – Jak často a mnoho budou data muset škálovat? -Platformou nativně implementovat Škálováním na více systémů? -Jak snadné je přidání nebo odebrání kapacity (velikost a propustnost)? Relační databáze a tabulky nejsou rozděleny do oddílů automaticky tak, aby byly škálovatelné, takže jsou k obtížné škálování nad rámec některými omezeními. Úložiště dat typu NoSQL jako Azure Table storage ze své podstaty oddílu všechno, co a přidání oddílů téměř žádné omezení. Table Storage je možné snadno škálovat až 200 terabajtů, ale maximální velikost databáze pro databázi SQL Azure je 500 GB. Je možné škálovat relačních dat při rozdělení do více databází, ale nastavení aplikace pro podporu tohoto modelu zahrnuje mnoho programování práci. |
| Instrumentace a možnosti správy | -Jak snadné je platforma pro instrumentace, sledovat a spravovat? Budete muset Udržujte si přehled o stavu a výkonu úložiště dat., je třeba vědět předem jaké metriky platformu vám zdarma a co musíte vypracovat sami. |
| Operace | -Jak snadné je platforma nasadit a spustit v Azure? PaaS? IaaS? Linux? Úložiště TABLE a SQL Database lze snadno nastavit v Azure. Platformy, které nejsou integrované řešení Azure PaaS komplikovanější. |
| Podpora rozhraní API | -Je k dispozici rozhraní API, které usnadňuje práci s platformou? Pro službu tabulky Azure je sada SDK s .NET API, která podporuje asynchronní programovací model rozhraní .NET 4.5. Pokud píšete aplikace .NET, bude k psaní a testování kódu pro službu tabulky Azure ve srovnání s jinou klíč/hodnota sloupce dat úložiště platforma, která nemá žádné rozhraní API nebo méně rozsáhlé jeden mnohem jednodušší. |
| Transakční konzistence integrity a dat | -Je důležité, aby platformy podporují transakce Chcete-li zaručit konzistenci dat? Pro udržování přehledu o hromadné e-maily odesílané, výkon a nízkou data náklady na úložiště může být důležitější než Automatická podpora pro transakce nebo referenční integrity dat platformy, vytváření služby Azure Table dobrou volbou. Pro sledování bankovní účet vyrovnává nebo objednávek platformu relační databázi, která poskytuje silné záruky transakcí by bylo vhodnější volbou. |
| Kontinuita podnikových procesů | -Jak snadno jsou, zálohování, obnovení a zotavení po havárii? Dřív nebo později bude získat provozními daty poškozen a budete potřebovat funkci zpět. Relační databáze často mají další možnosti podrobné obnovení, jako je například umožňuje obnovit k určitému bodu v čase. Pochopení, jaké funkce obnovení jsou k dispozici v každou platformu, kterou byste chtěli je důležitým faktorem vzít v úvahu. |
| Náklady | – Pokud je vaše data úlohy může podporovat více než jedné platformě, jak se srovnání náklady? Například pokud použijete ASP.NET Identity, můžete uložit data uživatelského profilu služby Table Azure nebo Azure SQL Database. Pokud nepotřebujete bohaté dotazování zařízení SQL Database, může tabulky Azure v části vybrat, protože náklady na něj mnohem menší pro dané množství úložiště. |

Co obecně se doporučuje zvolte řešení úložiště dat znát odpovědi na otázky v každé z těchto kategorií.

Kromě toho vaše zatížení může mít specifické požadavky, které mohou některé platformy podporovat lépe než jiné. Příklad:

- Vaše aplikace potřebují auditovat možnosti?
- Jaké jsou vaše data dlouhověkosti požadavky – vyžadujete automatizované archivace nebo mazání možností?
- Máte potřeby specializované zabezpečení? Například data obsahují PII (identifikovatelné osobní údaje), ale budete muset mít a ujistěte se, že PII je vyloučen z výsledků dotazu.
- Pokud máte data, která nelze uložit v cloudu charakterem předpisů nebo technologické z důvodů, bude pravděpodobně nutné cloudové platformy úložiště dat, která usnadňuje integraci s místní úložiště.

## <a name="demo--using-sql-database-in-azure"></a>Ukázka – používání databáze SQL v Azure

Aplikace opravte používá relační databázi k ukládání úlohy. Prostředí skriptu prostředí Windows PowerShell pro vytváření ukazuje [automatizovat všechno kapitoly](automate-everything.md) vytvoří dvě instance databáze SQL. Ty na portálu se zobrazí kliknutím **databází SQL** kartě.

![Databází SQL v portálu](data-storage-options/_static/image8.png)

Je také usnadňuje vytváření databází pomocí portálu.

Klikněte na tlačítko **nové – datové služby** -- **SQL Database** -- **rychle vytvořit**, zadejte název databáze, zvolte server, které už máte v účtu nebo vytvořte novou a klikněte na tlačítko **vytvořit databázi SQL**.

![Novou databázi SQL.](data-storage-options/_static/image9.png)

Počkejte několik sekund a máte databázi v Azure, které jsou připravené k použití.

![Vytvořit novou databázi SQL.](data-storage-options/_static/image10.png)

Takže Azure v pár sekund co může vám trvat den nebo v týdnu nebo déle, než se provést v místním prostředí. A vzhledem k tomu, že databáze stejně snadno můžete ve skriptu nebo pomocí rozhraní API pro správu vytvářet automaticky, můžete dynamicky vertikální navýšení kapacity tak, že se vaše data napříč více databází: < p >, tak dlouho, dokud vaše aplikace naprogramování pro tento. < /o : p >

Toto je příklad našeho modelu platforma jako služba. Nemáte ke správě serverů, můžeme provést. Nemusíte si dělat starosti o zálohování, můžeme provést. Je spuštěna v vysoká dostupnost – data v databázi se automaticky replikují mezi třemi servery. Pokud je počítač kterou, můžeme automaticky převzetí služeb při selhání a ztratit žádná data. Server je pravidelně opravit, nemusíte si dělat starosti, o který.

Klikněte na tlačítko a získat přesné připojovací řetězec, potřebujete a můžete okamžitě začít využívat nové databáze.

![Připojovací řetězce](data-storage-options/_static/image11.png)

Řídicí panel se dozvíte, historie připojení a velikost úložiště, které používá.

![Řídicí panel databáze SQL](data-storage-options/_static/image12.png)

Můžete spravovat databáze na portálu nebo pomocí nástroje SQL Server jste již obeznámeni s, včetně služby SQL Server Management Studio (SSMS) a nástroje sady Visual Studio, SQL Server Object Explorer (SSOX) a Průzkumníka serveru.

![SSOX](data-storage-options/_static/image13.png)

Jiné skvělé na tom je cenový model. Vývoj můžete začít s bezplatnou databázi 20 MB a provozní databáze začíná na přibližně 5 $ za měsíc. Platí pouze pro množství dat, která je ve skutečnosti ukládá do databáze není maximální kapacity. Nemusíte si jeho licenci zakoupili.

Databáze SQL je snadné škálování. Pro aplikaci opravte ji databázi, kterou se nám vytvořit v našem skriptu pro automatizaci limitován 1 GB. Pokud chcete škálovat až 150 4gigový, můžete právě přejděte na portál a změna tohoto nastavení nebo spusťte příkaz REST API a v sekundách, kterou můžete nasadit data do databáze 150 4gigový máte.

![Edice SQL Database a velikosti](data-storage-options/_static/image14.png)

To je výkon cloudu a rychle a snadno stát infrastruktury jej začít používat okamžitě.

Aplikace opravte ji používá dvě databáze SQL, jeden pro členství (ověřování a autorizace) a jeden pro data, a to je všechno, co stačí zřídit a škálovat ho. Už dříve jste viděli jak zřídit databáze prostřednictvím skriptů prostředí Windows PowerShell a nyní také jste viděli, jak je snadné provést na portálu.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Rozhraní Entity Framework versus přímým přístupem k databázi pomocí ADO.NET

Aplikace opravte ji přístup k těmto databázím s použitím rozhraní Entity Framework, společnosti Microsoft doporučuje ORM (objekt relační mapper) pro aplikace .NET. ORM je skvělý nástroj, který usnadňuje produktivita vývojářů, ale za cenu snížení výkonu v některých scénářích se dodává produktivitu. V reálných cloudové aplikace nebude provádět volba mezi využitím EF, nebo přímo pomocí ADO.NET – budete používat obě. Ve většině případů při psaní kódu, který funguje s databází, získávání maximální výkon není příliš důležitý a můžete využít výhod zjednodušené kódování a testování můžete získat pomocí rozhraní Entity Framework. V situacích, kdy EF režie by způsobilo nepřijatelný výkon můžete napsat a spouštění vlastních dotazů pomocí ADO.NET, ideálně volání uložené procedury.

Libovolné metody, které používáte pro přístup k databázi, kterou chcete minimalizovat "chattiness" co nejvíc. Jinými slovy Pokud se v jedné větší výsledek dotazu nastavit místo desítky nebo stovky menším můžete všechna data, která budete potřebovat, který je obvykle vhodnější. Například pokud potřebujete seznam studenty a kurzy, které se registrují, je obvykle lepší získat všechna data v dotazu jedno připojení spíš než získávání na studentů v jednom dotazu a provádění samostatné dotazy pro každou Studentova kurzy.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Databáze SQL a Entity Framework v aplikaci opravit

V aplikaci opravit `FixItContext` třída, která je odvozena z rozhraní Entity Framework `DbContext` třídy, identifikuje databázi a určuje tabulky v databázi. Kontext určuje sadu entit (tabulky) pro úlohy a kód předá v kontextu název připojovacího řetězce. Tento název se odkazuje na připojovací řetězec, který je definován v souboru Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Připojovací řetězec *Web.config* název souboru je appdb (zde odkazující na databázi místní vývoj):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Rozhraní Entity Framework vytvoří *FixItTasks* tabulky podle vlastnosti, které jsou součástí `FixItTask` třídy entita. Toto je jednoduchý třída objektů POCO (prostý původní objekt CLR), což znamená, není dědí nebo jsou všechny závislé na rozhraní Entity Framework. Ale umí vytvořit tabulku založených na rozhraní Entity Framework a provádět operace CRUD (vytvoření čtení aktualizace odstranění) s ním.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabulka FixItTasks](data-storage-options/_static/image15.png)

Aplikace opravte zahrnuje rozhraní úložiště, který používá pro práci s úložištěm dat operace CRUD.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Všimněte si, že úložiště metody jsou všechny asynchronní, takže všechny přístup k datům lze provést zcela asynchronním způsobem.

Implementace volá úložiště Entity Framework asynchronní metody pro práci s daty, včetně LINQ dotazuje stejně jako u vložit, aktualizovat a operace odstranění. Tady je příklad kódu pro vyhledávání úlohu opravte ji.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Můžete si všimnout k dispozici je také některé načasování a protokolování kód chyby, podíváme, později v [monitorování a Telemetrie kapitoly](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Výběr databáze SQL (PaaS) a serveru SQL Server ve virtuálním počítači (IaaS) v Azure

O vhodnou položku o systému SQL Server a databáze SQL Azure je stejné základní programovací model pro oba dva. Většina stejné dovednosti můžete použít v obou prostředích. Dokonce můžete databázi systému SQL Server v vývoj a instanci databáze SQL v cloudu, což je, jak nastavit aplikace opravit.

Alternativně můžete spustit stejný Server SQL v cloudu spustit místně nainstalováním na virtuální počítače IaaS. Pro některé starší aplikace systémem SQL Server ve virtuálním počítači může být lepším řešením. Vzhledem k databázi systému SQL Server běží na vyhrazených virtuálních počítačů, je dostupné další prostředky než databáze SQL Database, která běží na sdílených serverů. To znamená, může být větší a i nadále provádět i databázi systému SQL Server. Obecně platí tím menší velikost databáze a velikost tabulky, tím lépe případ použití funguje pro databáze SQL (PaaS).

Zde jsou uvedeny pokyny o tom, jak zvolit dva modely.

| Databáze Azure SQL (PaaS) | Systému SQL Server ve virtuálním počítači (IaaS) |
| --- | --- |
| **Specialisté** -nemáte k vytvoření nebo spravovat virtuální počítače, aktualizace nebo opravy operačního systému nebo SQL; Provede Azure. -Integrovanou vysokou dostupnost, s SLA úrovni databáze. -Nedostatek celkové náklady na vlastnictví (TCO), protože platíte jenom za použijete (žádná licence vyžaduje). -Vhodné pro zpracování velkého počtu menší databáze (&lt;= 500 GB). -Snadno vytvořit dynamicky nové databáze a umožnit tak škálování. | ***Specialisté*** – funkce kompatibilní s místním SQL serveru. -Můžete implementovat systému SQL Server [vysokou dostupnost prostřednictvím AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) ve virtuálních počítačích 2 +, s SLA úroveň virtuálního počítače. -Máte plnou kontrolu nad spravováni SQL. -Můžete znovu použít SQL licence už vlastníte, nebo se platit za hodinu pro jednu. -Vhodné pro zpracování méně ale větší (1 TB +) databází. |
| **Cons** -některé funkce mezery ve srovnání s místním SQL serveru (nedostatku [integrace modulu CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [podporu komprese](https://technet.microsoft.com/library/cc280449.aspx), [systému SQL Server Služba Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)atd)-limitu velikosti databáze 500 GB. | ***Cons*** – aktualizace nebo opravy (operačního systému a SQL) jsou zodpovědní za – vytváření a správu databází jsou zodpovědní za - IOPS disku (vstupně výstupních operací za sekundu) nesmí být o 8000 (prostřednictvím 16 datových jednotek). |

Pokud chcete používat systém SQL Server ve virtuálním počítači, můžete použít vlastní licenci na SQL Server, nebo může platit pro jednu podle hodin. Například na portálu nebo pomocí rozhraní REST API můžete vytvořit nový virtuální počítač pomocí bitové kopie systému SQL Server.

![Vytvoření virtuálního počítače se systémem SQL Server](data-storage-options/_static/image16.png)

![Seznam obrázků virtuální počítač s SQL serverem](data-storage-options/_static/image17.png)

Při vytváření virtuálního počítače s bitovou kopii systému SQL Server, jsme poměrně náklady na licence systému SQL Server za hodinu na základě vašeho využití virtuálního počítače. Pokud máte projekt, který bude jenom pro několik měsíců, je levnější věnovat podle hodin. Pokud se domníváte, že váš projekt bude poslední let, je levnější koupit licenci způsob, jakým obvyklým způsobem.

## <a name="summary"></a>Souhrn

Cloud computing je praktický kombinovat a shody dat úložiště přístupy k nejlépe vyhovovaly potřebám vaší aplikace. Pokud vytváříte novou aplikaci, byste pečlivě zvážit otázky zde uvedeny, aby bylo možné vybrat přístupy, které budou fungovat i při zvětšování vaší aplikace. [Další kapitoly](data-partitioning-strategies.md) bude vysvětlují několik rozdělení strategií, které můžete kombinovat více dat úložiště přístupy.

## <a name="resources"></a>Prostředky

Další informace najdete v následujících zdrojích informací.

Výběr databáze platforma:

- [Přístup k datům pro vysoce škálovatelné řešení: pomocí SQL, NoSQL a Polyglot trvalost](http://aka.ms/dag-doc). Elektronická kniha podle Microsoft Patterns and Practices, je třeba do hloubky do různých typů dat ukládá k dispozici pro cloudové aplikace.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/ff898430.aspx). Zobrazit Úvod do konzistence dat, Data replikace a synchronizace pokyny, tabulky indexu vzor, vzor Materializována zobrazení.
- [Základní: Acid alternativu](http://queue.acm.org/detail.cfm?id=1394128). Článek o kompromisy mezi konzistenci dat a škálovatelnost.
- [Sedm databází v sedmi týdnů: Průvodce moderní databáze a přesun NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Kniha Erica Redmond a Jima R. Wilsona. Důrazně doporučujeme pro sami Úvod do rozsahu platformy na úložiště dat, které jsou dnes k dispozici.

Volba mezi systému SQL Server a SQL Database:

- [Premium Preview pokyny databáze SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Úvod do databáze SQL ve verzi Premium a pokyny, když chcete-li zvolit přes edice SQL databáze Web a Business.
- [Pokyny a omezení (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Stránky portálu, který obsahuje odkazy na dokumentaci o omezeních databáze SQL, včetně ten, který se zaměřuje na funkce systému SQL Server databáze SQL nepodporuje.
- [Systému SQL Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Stránky portálu, který obsahuje odkazy na dokumentaci o systémem SQL Server v Azure.
- [Scott Guthrie vysvětluje databází SQL v Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 dvouminutové video Úvod do databáze SQL pomocí Scott Guthrie.
- [Vzory aplikace a vývoj strategie pro SQL Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Použitím rozhraní Entity Framework a databáze SQL v aplikaci ASP.NET Web

- [Začínáme s EF 6 pomocí MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řada devět částí, který vás provede procesem vytváření aplikace MVC, která používá EF a nasadí databázi a databázi SQL Azure.
- [Nasazení webu ASP.NET pomocí sady Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Kurz série dvanácti část, která přejde do větší hloubky o tom, jak nasadit databázi s využitím platforem EF Code First.
- [Nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a SQL Database k webovému serveru Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Podrobný kurz, který vás provede procesem vytvoření webové aplikace, která používá ověřování, ukládá aplikace tabulky v databázi členství, změní schéma databáze a nasadí aplikaci do Azure.
- [Mapa obsahu přístupu k Data technologie ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Odkazy na zdroje pro práci s EF a databáze SQL.

Na platformě Azure pomocí MongoDB:

- [MongoLab - MongoDB v Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Stránky portálu pro dokumentaci o spuštění MongoDB v Azure.
- [Vytvoření Azure webu, který se připojuje k MongoDB, které jsou spuštěny na virtuálním počítači v Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Podrobný návod, jak používat databázi MongoDB ve webové aplikaci ASP.NET.

HDInsight (Hadoop v Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Na HDInsight dokumentaci na portálu [Azure](https://azure.microsoft.com/) webu.
- [Hadoop a prostředí HDInsight: velkých objemů dat v Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Článku MSDN časopis Bruno Terkaly a Richard Villalobos, představení Hadoop v Azure.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/dn568099.aspx). Vzor MapReduce najdete v tématu.

> [!div class="step-by-step"]
> [Předchozí](single-sign-on.md)
> [další](data-partitioning-strategies.md)
