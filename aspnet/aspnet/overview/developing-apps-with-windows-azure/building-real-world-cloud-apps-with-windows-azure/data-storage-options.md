---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Možnosti úložiště dat (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 16388bf00c291ee21eec7bc72a9c01c33cec9371
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820331"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Možnosti úložiště dat (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Většina lidí se používají k relačním databázím a mají tendenci přehlédnout jiných možností úložiště dat při jejich návrhu cloudové aplikace. Výsledkem může být neoptimální výkon, vysokou výdaje a nebo ještě hůř, protože [NoSQL](http://en.wikipedia.org/wiki/NoSQL) některé úlohy může zpracovávat efektivněji než relační databáze (nerelační) databáze. Když zákazníci nás požádat o pomoc s řešením problému úložiště důležitých dat, je to často protože mají relační databáze, kde některé z možností NoSQL by fungovat lépe. V takových situacích zákazník bylo lepší vzorec, pokud se má implementovat řešení NoSQL před nasazením aplikace do produkčního prostředí.

Na druhé straně bylo by také chybu, aby předpokládal, že databázi NoSQL můžete provést všechno dobře nebo dostatečně dobře. Neexistuje žádné jeden data správy ideální pro všechny úlohy úložiště dat; řešení pro správu různých datových jsou optimalizovány pro různé úlohy. Většina skutečných cloudových aplikací mají různé požadavky na úložiště dat a často obsluhuje nejvhodnější kombinace více řešení datového úložiště.

Účelem této kapitole je získali širší představu řada možností úložišť dat, která je k dispozici cloudové aplikace a některé základní informace o tom, jak vybrat ty, které vyhovují vašemu scénáři. Je vhodné vědět o dostupných možnostech a uvažovat o jejich silné a slabé stránky před při vývoji aplikace. Změna možnosti ukládání dat v produkční aplikaci může být velmi obtížné, jako byste měli změnit leteckého motoru během letu plochy.

## <a name="data-storage-options-on-azure"></a>Možnosti ukládání dat v Azure

Díky cloudu je poměrně snadné ho použít celou řadu relačních a datových úložišť typu NoSQL. Tady jsou některé platformy úložiště dat, které můžete použít v Azure.

![](data-storage-options/_static/image1.png)

V tabulce jsou uvedeny čtyři typy databází NoSQL:

- [Klíč/hodnota databází](https://msdn.microsoft.com/library/dn313285.aspx#sec7) uložení objektu jedné serializované pro každou hodnotu klíče. Jsou vhodné pro ukládání velkých objemů dat, ve kterém chcete přijímat jednu položku pro danou hodnotu klíče a nemáte k dispozici pro dotazy podle jiné vlastnosti položky.

    [Azure Blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) je databáze klíč/hodnota, která funguje jako úložiště souborů v cloudu pomocí hodnoty klíče, které odpovídají názvy souborů a složek. Načtení souboru tak, že jeho složku a název souboru, ne tak, že hodnoty v obsahu souboru.

    [Azure Table storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) je také databázi klíč/hodnota. Každá hodnota se nazývá *entity* (podobně jako na řádek určený klíč oddílu a klíč řádku) a obsahuje více *vlastnosti* (podobně jako sloupce, ale ne všechny entity v tabulce muset sdílet stejný sloupce). Při dotazování u sloupce mimo klíč je velmi neefektivní a mělo by se vyhnout. Například může ukládat data uživatelského profilu, s jedním oddílem ukládání informací o jenom jednoho konkrétního uživatele. V samostatné vlastnosti jedné entitě nebo v samostatné entity do stejného oddílu, může uchovávat data, například uživatelské jméno, hodnoty hash hesla, datum narození a tak dále. Ale není vhodné k vyhledání všech uživatelů s zadaný rozsah data narození a nelze spustit dotaz spojení mezi vaší tabulce profilů a jinou tabulku. Table storage je škálovat a jsou míň nákladné než relační databáze, ale neumožňuje složitých dotazů nebo spojení.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) databáze klíč/hodnota, ve kterých jsou hodnoty *dokumenty*. "Dokument" zde není použit v tom smyslu dokument aplikace Word nebo Excel, ale znamená, že kolekce pojmenovaných polí a hodnoty, každá z nich může být podřízená dokumentu. Například v tabulce historie pořadí může mít dokument pořadí pořadové číslo, datum objednávky a polí zákazníka. a pole Zákazník může mít název a adresu pole. Databáze kóduje pole data ve formátu, jako jsou XML, YAML, JSON nebo BSON; nebo může použít jako prostý text. Jednu funkci, která nastavuje dokumentové databáze kromě databází klíč/hodnota je schopnost dotazovat na neklíčovým pole a definovat sekundárních indexů, aby dotazování efektivnější. Díky schopnosti je vhodnější pro aplikace, které potřebují k načtení dat na základě kritérií složitější než hodnota klíč dokumentu databáze dokumentů zajistit. Například v databázi dokumentů historie prodejní objednávky může dotaz na různá pole, jako je například ID produktu, ID zákazníka, jméno zákazníka a tak dále. [MongoDB](http://www.mongodb.org/) je databáze dokumentů.
- [Databáze rodin sloupců](https://msdn.microsoft.com/library/dn313285.aspx#sec9) klíč/hodnota jsou úložiště dat, která vám umožní a struktura ukládání dat do kolekcí související sloupce s názvy rodin sloupců. Například sčítání databáze může mít jednu skupinu sloupců pro jméno osoby (první, střední, poslední), jedna skupina pro adresu osoby a jednu skupinu pro informace o profilu osoby (DOB, pohlaví, atd.). Databázi můžete pak ukládat každá rodina sloupců v samostatném oddílu při zachování všech dat pro jednu osobu, která související se stejným klíčem. Bez nutnosti přečíst všechny informace název a adresu a pak můžete přečíst všechny informace o profilu. [Cassandra](http://cassandra.apache.org/) je Oblíbené databáze rodin sloupců.
- [Databáze grafu](https://msdn.microsoft.com/library/dn313285.aspx#sec10) ukládat informace jako kolekce objekty a vztahy. Účelem databáze grafu je umožnit aplikaci efektivní provádění dotazů, které procházejí sítí objekty a vztahy mezi nimi. Například objekty mohou být zaměstnanci v databázi lidských zdrojů, a může být vhodné pro usnadnění dotazů jako například "najít všechny zaměstnance, kteří pracují přímo nebo nepřímo pro Scott." [Neo4j](http://www.neo4j.org/) je Oblíbené grafová databáze.

Porovnání s relačními databázemi, možnosti NoSQL nabízí mnohem větší škálovatelnosti a nákladové efektivity pro ukládání a analýze nestrukturovaných dat. Výměnou za to je, že není poskytuje bohaté queryability a robustní datové integritní schopnosti relačních databází. NoSQL funguje dobře pro data protokolu služby IIS, včetně vysoké objemy bez nutnosti pro připojení k dotazy. NoSQL nebude tak dobře fungovat pro bankovní transakce, který vyžaduje integritu dat absolutní a zahrnuje mnoho vztahy k jiným datům souvisejícím s účtem.

K dispozici je také novější kategorie platformy databáze názvem [NewSQL](http://en.wikipedia.org/wiki/NewSQL) , který kombinuje škálovatelnost databáze NoSQL s queryability a transakční integrity relační databáze. NewSQL databáze jsou navržené pro distribuované ukládání a zpracování dotazů, což je často obtížné implementovat v databázích "OldSQL". [NuoDB](http://www.nuodb.com/) je příkladem NewSQL databázi, která je možné v Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop a MapReduce

Velké objemy dat, který můžete uložit do databáze NoSQL, může být obtížné analyzovat efektivně včas. Provedete to, které můžete použít architekturu jako třeba [Hadoop](http://hadoop.apache.org/) která implementuje [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funkce. V podstatě MapReduce proces nemá je následující:

- Limit množství dat, kterou je potřeba zpracovat tak, že vyberete mimo data ukládat jenom data, která skutečně potřebujete analyzovat. Je třeba znát strukturu vaší uživatelské základně podle roku narození, takže vybrat pouze narození let mimo vaše úložiště dat profilu uživatele.
- Rozdělit data do částí a odesílat do jiných počítačů pro zpracování. Počítači A vypočítá počet lidí s daty 1950 1959, počítač B nepodporuje 1960 1969, atd. Tato skupina počítačů se nazývá *Hadoop cluster*.
- Vložte výsledky jednotlivých částí zpět společně po dokončení zpracování na části. Teď máte seznam kolik lidí pro každý rok narození poměrně krátké a úloh výpočtu procenta v tomto seznamu celkové se dají spravovat.

V Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/) umožňuje zpracovávat, analyzovat a získání nových poznatků z velkých objemů dat s využitím výkonu hadoopu. Například může použít k analýze protokolů webového serveru:

- Povolení protokolování webového serveru do vašeho účtu úložiště. Tím se nastaví Azure pro zápis protokolů do služby Blob Service pro každý požadavek HTTP do vaší aplikace. Služba objektů Blob je v podstatě Cloudová úložiště souborů a hezky integruje s HDInsight. 

    ![Protokoly do úložiště objektů Blob](data-storage-options/_static/image2.png)
- Protože aplikace získá provoz, protokolů webového serveru IIS se zapisují do úložiště objektů Blob. 

    ![Protokoly webového serveru](data-storage-options/_static/image3.png)
- Na portálu klikněte na tlačítko **nový** - **datové služby** - **HDInsight** - **rychlé vytvoření**, a zadejte název clusteru HDInsight, velikost clusteru (počet datových uzlů clusteru HDInsight) a uživatelské jméno a heslo pro HDInsight cluster. 

    ![HDInsight](data-storage-options/_static/image4.png)

Nyní můžete nastavit úlohy mapreduce je možné k analýze protokolů a získejte odpovědi na otázky jako:

- Kterou denní dobu Moje aplikace získáte nejvyšší či nejnižší provoz?
- Jaké zemích se Moje provoz přicházející z?
- Co je průměrná okolí příjem Moje provoz pochází z oblasti. (Není veřejné datové sady, které vám detekovaná sousední výnosy podle IP adresy a shodovat s proti IP adresu v protokolů webového serveru.)
- Jak detekovaná sousední příjem korelovat na konkrétní stránky nebo produkty v lokalitě?

Můžete pak použít odpovědi na otázky, jako jsou tyto cílové reklamy podle pravděpodobnosti, které by zákazník zájem nebo pravděpodobnost, že si konkrétního produktu.

Jak je vysvětleno v [automatizovat všechno kapitoly](automate-everything.md), je možné automatizovat většinu funkcí, které můžete udělat na portálu a který obsahuje nastavení a spouštění analytických úloh pro HDInsight. Typické HDInsight skript může obsahovat následující kroky:

- Zřídit HDInsight cluster a připojit ho k účtu úložiště pro vstup úložiště objektů Blob.
- Spustitelné soubory úlohy MapReduce (soubory .jar nebo .exe) nahrajte do clusteru HDInsight.
- Odeslání MapReduce, který uloží výstupní data do úložiště objektů Blob.
- Počkejte na dokončení úlohy.
- Odstranění clusteru HDInsight.
- Přístup k výstupu z úložiště objektů Blob.

Spuštěním skriptu, který to vše dělá minimalizovat množství času, který je zřízení clusteru HDInsight, což minimalizuje náklady.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platforma jako služba (PaaS) a infrastruktura jako služba (IaaS)

Možnosti úložiště dat výše uvedených zahrnují Platform-as-a-Service (PaaS) a řešení Infrastructure-as--Service (IaaS). PaaS znamená, že budeme ji spravovat infrastrukturu hardware a software a stačí použít službu. SQL Database je funkce Azure PaaS. Požádat o databáze, a na pozadí Azure nastaví a konfiguruje virtuální počítače a nastaví databází na ně. Nemají přímý přístup k virtuálním počítačům a nemusíte spravovat. IaaS znamená, že nastavení, konfigurace a správa virtuálních počítačů, které běží v naší infrastruktuře Datacenter a vložíte cokoliv, co chcete s nimi. Poskytujeme galerii předem nakonfigurovaných imagí virtuálních počítačů pro společné konfigurace virtuálního počítače. Můžete například nainstalovat předem nakonfigurovaných imagí virtuálních počítačů pro Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database, atd.

Data řešení PaaS, které Azure nabízí, patří:

- Azure SQL Database (dřív označovanou jako SQL Azure). Cloudová relační databáze založená na SQL serveru.
- Azure Table storage. Klíč/hodnota, databáze NoSQL.
- Azure Blob storage. Úložiště souborů v cloudu.

Pro IaaS můžete spustit všechno, co můžete načíst do virtuálního počítače, například:

- Relačními databázemi jako SQL Server, Oracle, MySQL, SQL Compact, SQLite nebo Postgres.
- Například protokolem Memcache, Redis, Cassandra a Riak úložiště dat klíč/hodnota.
- Sloupec data ukládá jako je například HBase.
- Databáze dokumentů, jako je například CouchDB, MongoDB a RavenDB.
- Databáze grafů, jako je například Neo4j.

![Možnosti ukládání dat v Azure](data-storage-options/_static/image5.png)

Možnost IaaS poskytuje téměř neomezená data na možnosti úložiště a mnoho z nich jsou obzvláště snadno používá, protože můžete vytvořit pomocí předem nakonfigurovaných imagí virtuálních počítačů. Například v portálu pro správu přejděte na **virtuálních počítačů**, klikněte na tlačítko **image** kartu a klikněte na tlačítko **procházet VM Depot**.

![Procházet VM Depot](data-storage-options/_static/image6.png)

Zobrazí seznam [stovky předkonfigurovaných imagí virtuálních počítačů](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), a můžete vytvořit virtuální počítač z bitové kopie, která má systém správy databáze a předinstalovaným, například MongoDB, Neo4J, Redis, Cassandra nebo CouchDB:

![MongoDB v úložišti VM Depot po](data-storage-options/_static/image7.png)

S Azure můžete použít stejně snadno jako možné možnosti ukládání dat v IaaS, ale nabídek PaaS mají celou řadu výhod, které je nákladově efektivní a praktické pro řadu scénářů:

- Není nutné vytvářet virtuální počítače, jenom pomocí portálu nebo skript nastavení datového úložiště. Pokud chcete úložiště dat 200 terabajt, jen klikněte na tlačítko nebo spouští příkazy a během několika sekund je připraven k použití.
- Nemusíte spravovat ani aktualizovat virtuální počítače použité službou. Společnost Microsoft, který pro vás automaticky.-není nutné se starat o nastavení infrastruktury pro škálování a vysoké dostupnosti; Microsoft všechno, zpracuje za vás.
- Není potřeba koupit licence; licenční poplatky jsou zahrnuté v poplatků za služby.
- Platíte jenom za využité.

Možnosti úložiště dat PaaS v Azure zahrnují nabídek jiných poskytovatelů. Například můžete použít [doplňku MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) z Azure Store ke zřízení databáze MongoDB jako služba.

## <a name="choosing-a-data-storage-option"></a>Pokud vyberete možnost úložiště dat

Žádné jedním z přístupů je nejvhodnější pro všechny scénáře. Pokud kdokoli říká, že tato technologie je odpověď, je první věc, kterou požádat "Jaký je dotaz?", protože různá řešení, které jsou optimalizovány pro různé věci. Jednoznačného výhod relačního modelu; To je důvod, proč se stále týká tak dlouho. Existuje ale také dolů strany do SQL, které lze řešit pomocí řešení NoSQL.

Často co vidíme výborně fungují je složení přístup, pokud používáte SQL a NoSQL v jediném řešení. I když lidé Řekněme, že se už středu NoSQL, pokud můžete přejít k podrobnostem co dělají je často hledání, používá několik různých rozhraní NoSQL: používá [CouchDB](http://wiki.apache.org/couchdb/Introduction), a [Redis](http://redis.io/)a [ Riak](http://basho.com/riak/) pro různé věci. Dokonce i Facebooku, který značně používá NoSQL, používá různá rozhraní NoSQL pro různé části služby. Možnost kombinovat a párovat přístupy úložiště dat je jednou z věcí, které je dobré o cloudu, protože je snadné použití řešení pro více dat a jejich integrace v jediné aplikaci.

Tady je pár otázek zamyslet, když se výběr přístupu:

| Sémantické dat | -Co je základní úložiště a data přístupu k datům sémantického (jsou můžete ukládání relačních i nestrukturovanými daty)? Nejlépe nestrukturovaných dat, jako je například mediální soubory v úložišti objektů blob; kolekce související data, jako jsou produkty, inventáře, Dodavatelé, objednávek zákazníků, atd., nejlépe do relační databáze. |
| --- | --- |
| Podpora dotazů | – Jak snadné je dotazy na data? -Co typů otázek mohou být efektivně dotaz? Úložiště dat klíč/hodnota jsou velmi vhodné v získání jednoho řádku zadaný s hodnotou klíče, ale ne tak dobré pro složité dotazy. Pro úložiště dat profilu v uživatele ve kterém se vždy zobrazuje data pro jeden konkrétní uživatel úložiště dvojic klíč/hodnota data může fungovat. pro katalog produktů, ve které chcete získat různé seskupení na základě různých atributů produktu může být vhodnější relační databáze. Databáze NoSQL můžete efektivně ukládat velké objemy dat, ale budete muset struktury databáze po tom, jak aplikace dotazuje data a díky obtížnější provádět dotazy ad hoc. S relační databázi můžete vytvořit téměř jakýkoli druh dotazu. |
| Funkční projekce | -Může otázky, agregace, atd., být provedeno na straně serveru? Pokud mám vyberte počet spuštění (\*) z tabulky v SQL, bude velmi efektivně udělal všechnu práci na serveru a vrátí číslo hledám. Pokud chci, aby stejný výpočet z úložiště dat typu NoSQL, která nepodporuje agregace, to je neefektivní "bez vazby dotazu" a pravděpodobně vyprší časový limit. I v případě, že dotaz bude úspěšné, budu muset načíst všechna data ze serveru do klienta a počet řádků na straně klienta. – Jaké jazyky nebo typy výrazů můžete použít? Relační databáze I pomocí SQL. Některé databáze NoSQL jako je Azure Table storage, můžu používat [OData](http://www.odata.org/), a vše můžu udělat, je filtrovat na primární klíč a získejte projekce (vybrat podmnožinu pole k dispozici). |
| Snadná škálovatelnost | – Jak často a množství budou data muset škálovat? -Je platforma nativně implementuje horizontální navýšení kapacity? – Jak snadné je přidání nebo odebrání kapacity (velikosti a propustnosti)? Relační databáze a tabulky nejsou rozděleny do oddílů automaticky tak, aby byly škálovatelné, takže jsou složité jejich rozšiřování nad rámec určitá omezení. Ze své podstaty oddílů datových úložišť typu NoSQL, jako je Azure Table storage, všechno, co a skoro neomezený přidání oddílů. Table Storage je možné snadno škálovat až 200 TB, ale maximální velikosti databáze pro službu Azure SQL Database je 500 GB. Dělení do více databází můžete škálovat relační data, ale nastavení aplikace pro podporu tohoto modelu zahrnuje spoustu práce programování. |
| Instrumentace a možností správy | – Jak snadné je platforma pro instrumentaci, monitorovat a spravovat? Budete muset Udržujte si přehled o stavu a výkonu úložiště dat, proto je potřeba vědět před jeho zahájením jaké metriky na platformě poskytuje zdarma, a co musíte vypracovat sami. |
| Operace | – Jak snadné je platforma pro nasazení a spuštění v Azure? PaaS? IaaS? Linux? Table Storage a SQL Database se snadno nastavit v Azure. Platformy, které nejsou integrovanými řešeními Azure PaaS vyžadovat další úsilí. |
| Podpora rozhraní API | -Je k dispozici rozhraní API, která usnadňuje práci s platformou? Pro službu Azure Table Service je sada SDK pomocí rozhraní API .NET, která podporuje asynchronní programovací model rozhraní .NET 4.5. Pokud píšete aplikaci .NET, budete moci mnohem snadněji psaní a testování kódu pro službu Azure Table Service ve srovnání s jinou klíč/hodnota sloupce datové úložiště platformě, která nemá žádné rozhraní API nebo některou méně komplexní. |
| Transakční konzistence integrity a data | -Je důležité, že platformu podporu transakcí kvůli zaručení konzistence dat? Pro udržování přehledu o hromadné e-mailů, výkon a náklady na úložiště málo dat může být mnohem důležitější než Automatická podpora funkce pro transakce nebo referenční integritu v datové platformy, aby službu Azure Table Service dobrou volbou. Pro sledování bankovním účtu vyrovnává nebo nákupních objednávek platformu relační databáze, která poskytuje silné záruky transakční by bylo vhodnější použít. |
| Kontinuita podnikových procesů | – Jak snadné je zálohování, obnovení a zotavení po havárii? Dříve či později bude získat poškozený provozních dat a budete potřebovat funkci zpět. Relační databáze často mají další funkce jemně odstupňovaná restore, například možnost obnovit do bodu v čase. Vysvětlení, jaké funkce obnovení jsou u různých platforem, které zvažujete dostupné je důležitým faktorem, který vezměte v úvahu. |
| Náklady | – Pokud data úlohy může podporovat více než jedné platformě, jejich porovnání nákladů? Například pokud používáte ASP.NET Identity, můžete ukládat data uživatelského profilu ve službě Azure Table Service nebo Azure SQL Database. Pokud nepotřebujete široké možnosti dotazování zařízení služby SQL Database, můžete se rozhodnout Azure Tables zčásti vzhledem k tomu, že to stojí většina pro danou velikost úložiště. |

Co obecně se doporučuje znát odpovědi na otázky v každé z těchto kategorií zvolte řešení úložiště dat.

Kromě toho vaše zatížení může mít specifické požadavky, které mohou některé platformy podporují lépe než jiné. Příklad:

- Vaše aplikace vyžadovat auditu možnosti?
- Jaké jsou požadavky na vaše data životnost – nevyžadují automatické možnosti archivace nebo mazání?
- Máte požadavky na zabezpečení specializované? Například data obsahují osobní údaje (identifikovatelné osobní údaje), ale budete muset být schopni Ujistěte se, že identifikovatelné osobní údaje je vyloučený z výsledků dotazu.
- Pokud máte nějaká data, která nemůže být uložena v cloudu pro regulační nebo technologické důvodů může být nutné Cloudová datová platforma úložiště, která usnadňuje integrování místního úložiště.

## <a name="demo--using-sql-database-in-azure"></a>Ukázka – využívající službu SQL Database v Azure

Aplikace Fix It používá relační databázi k ukládání úkolů. Skript prostředí Windows PowerShell vytváření prostředí je znázorněno [kapitoly automatizovat všechno, co](automate-everything.md) vytvoří dvě instance SQL Database. V portálu můžete zobrazit kliknutím **databází SQL** kartu.

![Databáze SQL na portálu](data-storage-options/_static/image8.png)

Je také snadné vytváření databáze pomocí portálu.

Klikněte na tlačítko **nový – Data Services –** -- **SQL Database** -- **rychlé vytvoření**, zadejte název databáze, zvolte server, který už máte ve svém účtu nebo vytvořte novou a klikněte na tlačítko **vytvořit databázi SQL**.

![Nová databáze SQL](data-storage-options/_static/image9.png)

Počkejte několik sekund, a mít databázi v Azure, které jsou připravené k použití.

![Vytvoření nové databáze SQL](data-storage-options/_static/image10.png)

Tak, že Azure v několika sekund, co může trvat denně nebo týdně nebo i delší dobu provádět v místním prostředí. A protože databáze stejně snadno můžete ve skriptu nebo pomocí rozhraní API pro správu vytvořit automaticky, můžete dynamicky horizontálně navýšit tím, že rozprostírá vaše data napříč několika databázemi < o:p > tak dlouho, dokud aplikace naprogramování pro daný. < /o : p >

Toto je příkladem našeho modelu platforma jako služba. Není nutné ke správě serverů, Uděláme to. Nemusíte se obávat o zálohování, Uděláme to. Běží v prostředí s vysokou dostupností – data v databázi se replikují napříč tři servery automaticky. Pokud počítač přestane být funkční, můžeme automaticky převzetí služeb při selhání a přijít o žádná data. Server je pravidelně opravit, nemusíte zabývat.

Klikněte na tlačítko a získat přesné připojovací řetězec, potřebujete a můžete hned začít používat nové databáze.

![Připojovací řetězce](data-storage-options/_static/image11.png)

Řídicí panel zobrazuje historii připojení a velikost úložiště využitá.

![Řídicí panel SQL Database](data-storage-options/_static/image12.png)

Můžete spravovat databází na portálu nebo pomocí nástrojů SQL Server je už znáte, včetně SQL Server Management Studio (SSMS) a nástroje sady Visual Studio, SQL Server Object Explorer (SSOX) a Průzkumník serveru.

![SSOX](data-storage-options/_static/image13.png)

Další skvělé na tom je cenový model. Vývoj můžete začít s bezplatné 20 MB databázi a produkční databáze začíná přibližně 5 USD za měsíc. Platíte objem dat, které ukládáte ve skutečnosti v databázi není maximální kapacity. Není nutné si koupit licenci.

SQL Database je jednoduché škálování. Pro aplikace Fix It databáze, kterou jsme vytvořili v našich automatizační skript omezené na 1 GB. Pokud chcete škálovat až 150 GB, můžete pouze na portál a změnit toto nastavení nebo spusťte příkaz REST API, a během několika sekund máte 150 GB databáze, kterou můžete nasadit do data.

![Edice SQL Database a velikosti](data-storage-options/_static/image14.png)

To je síla cloudu a zprovozněte infrastrukturu, snadno a rychle začít používat okamžitě.

Aplikace Fix It používá dvě databáze SQL, jeden pro členství (ověřování a autorizace) a jeden pro data, a to je všechno, co musíte udělat, abyste ho zajišťovat a škálovat ho. Jste viděli již dříve jak zřídit databáze prostřednictvím skriptů prostředí Windows PowerShell, a teď jste viděli jsme také, jak snadné je udělat na portálu.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Srovnání s přímým přístupem k databázi pomocí ADO.NET Entity Framework

Aplikace Fix It nemá přístup k těmto databázím s použitím Entity Framework, Microsoft doporučuje ORM (objektově relační Mapovač) pro aplikace .NET. ORM je skvělý nástroj, který usnadňuje produktivitu vývojářů, ale produktivita spočívá za cenu snížení výkonu v některých scénářích. Ve skutečných cloudových aplikací nebudou provádět výběr mezi využitím EF, nebo přímo pomocí technologie ADO.NET – použijete obě. Ve většině případů při psaní kódu, který spolupracuje s databází, získání maximální výkon není důležité, a můžete využít výhod zjednodušené kódování a testování, získáte s rozhraním Entity Framework. V situacích, kde EF režie způsobí nepřijatelné výkonu můžete napsat a spouštění vlastních dotazů pomocí technologie ADO.NET, ideálně pomocí volání uložené procedury.

Libovolné metody, které používáte pro přístup k databázi, kterou chcete minimalizovat "upovídanost" co největší míře. Jinými slovy Pokud se zobrazí všechna data, které potřebujete v jedné větší výsledek dotazu nastavte místo jen pár nebo stovky menších, který je obvykle vhodnější. Například pokud potřebujete seznam studentů a kurzy, které se registrují, je obvykle vhodnější zobrazíte všechna data v dotazu jedno spojení místo získávání studenty v jednom dotazu a provádění samostatné dotazy pro každý student kurzy.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Databáze SQL a Entity Framework v aplikaci Fix It

V aplikaci Fix It `FixItContext` třída, která je odvozena z rozhraní Entity Framework `DbContext` třídy, určuje databázi a určuje tabulek v databázi. Kontext určuje sadu entit (tabulka) pro úlohy, a kód předá v kontextu název připojovacího řetězce. Tento název se odkazuje na připojovací řetězec, který je definován v souboru Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Připojovací řetězec *Web.config* soubor má název appdb (zde odkazující na databázi místního vývojového):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Vytvoří rozhraní Entity Framework *FixItTasks* tabulky na základě vlastností součástí `FixItTask` třída entity. Toto je jednoduchá třída POCO (Plain staré objektu CLR), což znamená, že nelze dědit z nebo mít všechny závislosti na rozhraní Entity Framework. Ale Entity Framework ví, jak vytvořit na jejím základě tabulku a provádění operací CRUD (vytváření čtení aktualizace odstraňování) s ním.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks tabulky](data-storage-options/_static/image15.png)

Tato aplikace Fix It obsahuje rozhraní úložiště, které používá pro práci s úložištěm dat. operace CRUD.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Všimněte si, že metody úložiště všechny asynchronní, tak všechny přístup k datům lze provést zcela asynchronním způsobem.

Volání implementace úložiště Entity Framework asynchronní metody pro práci s daty, včetně LINQ dotazy stejně jako u vložení, aktualizace a odstranění operace. Tady je příklad kódu pro vyhledání úkolů Fix It.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Můžete si všimnout, je také některé načasování a protokolování kód chyby, podíváme, který později v [monitorování a Telemetrie kapitoly](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Výběr databáze SQL (PaaS) versus SQL Server na virtuálním počítači (IaaS) v Azure

O vhodnou položku o SQL serveru a Azure SQL Database je stejné programovací model core u obou z nich. Většina stejné dovednosti, které můžete použít v obou prostředích. Dokonce můžete použít databázi serveru SQL Server ve vývoji a instance SQL Database v cloudu, což je nastavení aplikace Fix It.

Alternativně můžete spustit stejný Server SQL v cloudu spouštět místně pomocí instalace na virtuální počítače IaaS. U některých starších aplikací s SQL serverem na virtuálním počítači může být lepší řešení. Protože databáze serveru SQL Server běží na vyhrazených virtuálních počítačů, má k dispozici více prostředků, než databázi SQL Database, která běží na sdílený server. To znamená do databáze SQL serveru může být větší a i nadále provádět. Obecně platí Čím menší velikost databáze a velikost tabulky, tím lépe případ použití funguje pro SQL Database (PaaS).

Tady jsou některé pokyny o tom, jak zvolit mezi těmito dvěma modely.

| Azure SQL Database (PaaS) | SQL Server na virtuálním počítači (IaaS) |
| --- | --- |
| **Profesionálové** – není nutné vytvořit nebo spravovat virtuální počítače, aktualizace nebo opravy operačního systému nebo SQL; Azure, které udělá za vás. -Integrovanou vysokou dostupnost, se smlouvou SLA na úrovni databáze. -Nízké celkové náklady na vlastnictví (TCO), protože platíte jenom za využité (vyžaduje žádné licence). -Vhodný pro zpracování velkého množství menších databází (&lt;= 500 GB). -Snadno dynamicky se vytvářejí nové databázích tak, aby horizontální navýšení kapacity. | ***Profesionálové*** – funkce kompatibilní s místním SQL serveru. -Může implementovat systém SQL Server [vysokou dostupností AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) ve více než 2 virtuální počítače s SLA na úrovni virtuálního počítače. -Máte plnou kontrolu nad způsobu správy SQL. -Můžete znovu použít licence SQL už vlastníte, nebo můžete platit podle počtu hodin pro jeden. -Vhodná pro menší počet zpracování, ale větší (1 TB +) databáze. |
| **Nevýhody** – některé funkce mezery ve srovnání s místním SQL serveru (nedostatku [integrace modulu CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [podpory komprese](https://technet.microsoft.com/library/cc280449.aspx), [systému SQL Server Služby Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)atd)-limitu velikosti 500 GB. | ***Nevýhody*** – aktualizace nebo opravy (operační systém a SQL) jsou vaší odpovědností – vytváření a správu databází jsou vaší odpovědností - Disk IOPS (vstupně výstupní operace za sekundu) omezené na přibližně 8000 (prostřednictvím 16 datových jednotek). |

Pokud chcete použít systém SQL Server na virtuálním počítači, můžete použít svoji vlastní licenci systému SQL Server nebo můžete platit jednu po hodinách. Na portálu nebo přes rozhraní REST API můžete například vytvořit nový virtuální počítač pomocí image SQL serveru.

![Vytvoření virtuálního počítače s SQL serverem](data-storage-options/_static/image16.png)

![Seznam imagí virtuálního počítače s SQL serverem](data-storage-options/_static/image17.png)

Při vytváření virtuálního počítače s bitovou kopii systému SQL Server, jsme poměrné míra náklady na licence systému SQL Server po hodinách na základě vašeho využití virtuálního počítače. Pokud už máte projekt, který bude jenom zobrazovat běžel několik měsíců, je levnější platit podle počtu hodin. Pokud se domníváte, že váš projekt bude poslední roky, je levnější koupit licenci způsob, jakým obvyklým způsobem.

## <a name="summary"></a>Souhrn

Cloud computingu díky praktické kombinovat a porovnání dat úložiště přístupy k nejlépe vyhovuje potřebám vaší aplikace. Pokud už vytváříte nové aplikace, pečlivě zvážit otázky zde uvedeny, aby bylo možné vybrat přístupů, které budou fungovat dobře, když se vaše aplikace poroste. [Další kapitolu](data-partitioning-strategies.md) popisují některé strategie dělení, které vám umožní kombinovat několik přístupů datového úložiště.

## <a name="resources"></a>Prostředky

Další informace najdete v tématu následující prostředky.

Volba databázové platformě:

- [Přístup k datům pro vysoce škálovatelné řešení: použití SQL, NoSQL a Polyglotické trvalosti](http://aka.ms/dag-doc). E-kniha od Microsoft Patterns and Practices, která přejdou do hloubky do různých typů dat ukládá k dispozici pro cloudové aplikace.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/ff898430.aspx). Najdete v úvodu ke konzistenci dat, replikace dat a pokyny k synchronizaci, model tabulky indexů, modelu Materializovaného zobrazení.
- [Základní: Odpovídající zásadám Acid alternativu](http://queue.acm.org/detail.cfm?id=1394128). Článek o kompromisy mezi konzistencí data a škálovatelnost.
- [Sedm databází v sedmi týdnů: Tento průvodce moderní databází a přesun NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Kniha od Eric Redmond a Jim R. Wilson. Důrazně doporučujeme zavedení sami oblasti datové platformy úložiště, které jsou dnes k dispozici.

Volba mezi SQL serverem a SQL Database:

- [Premium ve verzi Preview pro SQL Database pokyny](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Úvod do SQL Database úrovně Premium a pokyny, kdy zvolit přes edice SQL Database Web a Business.
- [Pokyny a omezení (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Stránka portálu, který obsahuje odkazy na dokumentaci k omezení služby SQL Database, včetně ten, který se zaměřuje na funkce produktu SQL Server databáze SQL nepodporuje.
- [SQL Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Stránka portálu, který obsahuje odkazy na dokumentaci k systému SQL Server běžící v Azure.
- [Scott Guthrie vysvětluje databáze SQL v Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). úvodní video 6 minut do služby SQL Database, Autor: Scott Guthrie
- [Modely aplikací a vývojové strategie pro SQL Server na virtuálních počítačích Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Použití rozhraní Entity Framework a SQL Database ve webové aplikace v ASP.NET

- [Začínáme s EF 6 pomocí MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Devět částí série, která vás provede postupem vytvoření aplikace MVC, která používá EF a nasadí databáze do Azure a SQL Database.
- [Nasazení webu ASP.NET pomocí sady Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Část 12 kurzů, které obsahuje podrobnosti o tom, jak nasadit databázi s využitím platforem EF Code First.
- [Nasaďte zabezpečené aplikace ASP.NET MVC 5 s Membership, OAuth a SQL Database na Azure web](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Podrobný kurz, který vás provede vytvořením webové aplikace, která používá ověřování, uloží aplikaci tabulek v databázi členství, upraví schéma databáze a nasadí aplikaci do Azure.
- [Mapa obsahu přístupu k datům v technologii ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Obsahuje odkazy na prostředky pro práci s EF a SQL Database.

Pomocí databáze MongoDB v Azure:

- [MongoLab – MongoDB v Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Stránka portálu pro dokumentaci o spuštění MongoDB v Azure.
- [Webový server, který se připojuje k MongoDB na virtuálním počítači v Azure vytvořit](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Podrobný kurz, který ukazuje, jak používat databázi MongoDB ve webové aplikaci ASP.NET.

HDInsight (Hadoop v Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). HDInsight dokumentaci na portálu [Azure](https://azure.microsoft.com/) webu.
- [Hadoop a HDInsight: velké objemy dat v Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Zpravodaj MSDN Magazine článku Bruno Terkaly a Richard Villalobos, úvod Hadoop v Azure.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Viz MapReduce vzor.

> [!div class="step-by-step"]
> [Předchozí](single-sign-on.md)
> [další](data-partitioning-strategies.md)
