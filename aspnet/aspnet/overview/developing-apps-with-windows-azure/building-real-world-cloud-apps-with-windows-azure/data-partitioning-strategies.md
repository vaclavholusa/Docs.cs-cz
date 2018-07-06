---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: (Vytváření skutečných cloudových aplikací s Azure) strategie dělení dat | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 89921df4f84b86ef7c222f8e8c871f510856b4f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819796"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>(Vytváření skutečných cloudových aplikací s Azure) strategie dělení dat
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o této sérii, naleznete v tématu [první kapitoly](introduction.md).


Dříve jsme viděli, jak je snadné škálování webové vrstvy cloudové aplikace, přidáváním a odebíráním webových serverů. Ale pokud jsou všechny vyskytuje stejného úložiště dat, kritickým bodem aplikace přesune z front-endu do back endu a datovou vrstvou je nejtěžší možností škálování. V této kapitole podíváme jak můžete provést datovou vrstvu, škálovatelné dělení dat do více relačních databází, nebo kombinací relační databáze úložiště u jiných možností úložiště dat.

Nastavení schéma rozdělení oddílů je vhodné si přední ze stejného důvodu již bylo zmíněno dříve Hotovo: je velmi obtížné, chcete-li změnit vaší strategie úložiště dat po aplikaci v produkčním prostředí. "Twitter chvíli" když vaše aplikace dojde k chybě nebo ocitne mimo provoz delší dobu, během reorganizovat data a kód přístupu k datům vaší aplikace můžete vyhnout názor pevné ještě před zahájením různé přístupy.

## <a name="the-three-vs-of-data-storage"></a>Tři Vs úložiště dat

Aby bylo možné určit, jestli potřebujete strategie dělení a jaké by měly být, vezměte v úvahu tři otázky týkající se vašich dat:

- Takže v konečném důsledku svazek – kolik dat budete ukládat? Několik gigabajtů? Několik stovek GB? Terabajtů? Petabajty?
- Rychlost – co je rychlost, jakou se zvýší vaše data? Je interní aplikace, které se generuje velké množství dat? Externí aplikaci, která zákazníkům se nahrávání, obrázky a videa do?
- Uložení různých – jaký typ dat se vám? Relační, obrázky, páry klíč hodnota, sociální grafy?

Pokud se domníváte, že budete mít spoustu objem, rychlost nebo celé řady, budete muset pečlivě zvažte, jaký druh schéma rozdělení oddílů nejlépe umožní vaší aplikace určené ke škálování efektivně a účinně jak roste a zajistit není narazíte na případné kritické body.

V podstatě existují tři přístupy k vytváření oddílů:

- Vertikální dělení
- Horizontální dělení
- Hybridní dělení

## <a name="vertical-partitioning"></a>Vertikální dělení

Svislé rozporcování je jako rozdělení tabulku podle sloupce: jednu sadu sloupců přejde do jednoho úložiště dat, a jinou sadu sloupců přejde do jiné datové úložiště.

Předpokládejme například, že Moje aplikace ukládá data o uživatelích, včetně imagí:

![Tabulka dat](data-partitioning-strategies/_static/image1.png)

Když představují tato data jako tabulku a podívejte se na různé typy dat, zobrazí se mají tři sloupce na levé straně řetězcová data, která můžete efektivně ukládat relační databázi, dva sloupce na pravé straně jsou v podstatě bajtová pole c pár z obrazových souborů. Je možné úložiště bitové kopie souboru data do relační databáze a velké množství lidí to, protože nebudete chtít uložit data do systému souborů. Nemusí mít systém souborů umožňující ukládání požadované objemy dat nebo nebudete chtít spravovat samostatnou zálohování a obnovení systému. Tento přístup funguje dobře u místních databází a pro malé množství dat v cloudových databázích. V místním prostředí může být jednodušší stačí nechat DBA postará o všechno.

Ale v databázi cloud storage je poměrně nákladné a velký počet imagí dokonce vytvářet velikosti databáze nad rámec, za které se mohou pracovat efektivně rozšiřovat. Řešení těchto problémů tím dělení dat svisle, což znamená, že vyberete nejvhodnější úložiště dat pro každý sloupec v tabulce dat. Co může být nejvhodnější pro účely tohoto příkladu je umístění řetězcová data do relační databáze a obrázků v úložišti objektů Blob.

![Vertikálně dělené tabulky dat](data-partitioning-strategies/_static/image2.png)

Ukládání imagí v úložišti objektů Blob místo databáze je praktičtější v cloudu než v místním prostředí, protože není nutné se starat o nastavování souborové servery a Správa zálohování a obnovení dat uložených mimo relační databáze: všechny který se udělá za vás automaticky pomocí služby Blob storage.

Jedná se o dělení postup implementovali jsme v aplikace Fix It, a podíváme se na kód pro, který v [kapitoly úložiště objektů Blob](unstructured-blob-storage.md). Bez této schéma vytváření oddílů a za předpokladu, že průměrná image množství 3 MB aplikace Fix It by pouze moci ukládat asi 40 000 úloh před tím databáze maximální velikosti 150 GB. Po odebrání Image, můžete ukládat databázi 10 třikrát tolik úloh; můžete přejít mnohem déle dříve, než máte přemýšlet o implementaci vodorovné schéma vytváření oddílů. A jako škálování aplikace, vaše výdaje růst pomaleji, protože hromadné požadavky na ukládání se přicházející do velmi cenově dostupné úložiště objektů Blob.

## <a name="horizontal-partitioning-sharding"></a>Horizontální dělení (sharding)

Vodorovné rozporcování je jako rozdělení tabulky po řádcích: jednu sadu řádků přejde do jednoho úložiště dat, a jinou sadu řádků přejde do jiné datové úložiště.

S ohledem stejnou sadu dat, Další možností je ukládat různé rozsahy názvů zákazníků v různých databázích.

![Horizontálně dělené do oddílů tabulky dat](data-partitioning-strategies/_static/image3.png)

Chcete buďte velmi opatrní vaše schéma horizontálního dělení, abyste měli jistotu, že data jsou distribuovaná rovnoměrně předejdete tak aktivní body. Tento jednoduchý příklad použití první písmeno příjmení nesplňuje tento požadavek, protože poslední s názvy začínajícími některých běžných písmena mají velký počet uživatelů. Omezení velikosti tabulky by přístupů dříve, než byste očekávali, protože některé databáze by být velmi rozsáhlé, zatímco většina by zůstala malé.

Dolů na straně horizontální dělení je, že může být obtížně provádět dotazy napříč všechna data. V tomto příkladu dotaz musel čerpat až 26 různými databázemi. Chcete-li získat všechna data ukládaná aplikaci.

## <a name="hybrid-partitioning"></a>Hybridní dělení

Můžete kombinovat vertikálním a horizontálním dělení. V příkladu s daty můžete například uložit obrázky v úložišti objektů Blob a horizontálně dělit data řetězec.

![Tabulka hybridní data rozdělit na oddíly](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Dělení produkční aplikace

Obecně je snadno zjistit, jak by fungovalo schéma rozdělení oddílů, ale jakékoli schéma dělení zvyšuje složitost kódu a přináší mnoho nových komplikace, které budete muset řešit. Pokud přesouváte obrázky do úložiště objektů blob, co můžete dělat při služba storage je mimo provoz? Jak zpracujete zabezpečení objektů blob Co se stane, když databáze a blob storage se rozsynchronizovat? Pokud jste horizontálního dělení, jak bude zpracovávat dotazování napříč všechny databáze?

Komplikace jsou spravovatelné tak dlouho, dokud máte v plánu pro ně před přechodem do produkčního prostředí. Mnoho uživatelů, kteří neprovedli, které chcete měli později. V průměru náš tým zákazníka poradní tým (CAT) získá panicked telefonní hovory o jednou za měsíc od zákazníků, jejichž aplikace je možné tak Opravdu velký a nedělalo toto plánování. A něco jako říká: "Nápověda! Můžu dát všechno v jednom datovém úložišti, a během 45 dnů si vyberu vyčerpala volné místo na to!" A pokud máte velké množství obchodní logiky, které jsou součástí jak získat přístup k úložišti dat a máte zákazníky, kteří používají vaši aplikaci, neexistuje žádná vhodná doba za den během migrace. Nakonec jsme projít herculean úsilí, abychom oddílu odběratele svá data v reálném čase s žádné výpadkům. Je Úžasná a velmi scary a nemít něco chcete být součástí pokud ho se můžete vyhnout! Uvažujete o to ještě před zahájením a integraci do vaší aplikace vám usnadní život mnohem Pokud aplikace poroste později.

## <a name="summary"></a>Souhrn

Efektivní schéma rozdělení oddílů můžete povolit škálování na petabajty dat v cloudu bez kritické body cloudové aplikace. A není nutné ještě před zahájením pro masivní počítače nebo rozsáhlou infrastrukturu platit podle využití může aplikaci spustili v místním datacentru. V cloudu můžete inkrementálně přidávat kapacity ji budete potřebovat a budete platit pouze pro tak, jak používáte když ji použijete.

V [další kapitolu](unstructured-blob-storage.md) uvidíme, jak aplikace Fix It implementuje vertikální dělení ukládáním imagí ve službě Blob storage.

## <a name="resources"></a>Prostředky

Další informace o strategie dělení najdete v následující prostředky.

Dokumentace ke službě:

- [Osvědčené postupy pro navrhování rozsáhlých služeb v systému Windows Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper Mark Simms a Michael Thomassy.
- [Microsoft Patterns and Practices - způsoby návrhu v cloudu](https://msdn.microsoft.com/library/dn568099.aspx). Zobrazit Data pokyny k dělení, model horizontálního dělení.

Videa:

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri a Mark Simms devět částí série. Nabídne základními koncepty a Principy architektury tak vysoce dostupné a zajímavé příběhy z prostředí Microsoft zákazníka poradní tým (CAT) se skutečným zákazníkům. Viz dělení diskuze v epizodě 7.
- [Vytváření Big: Získané od zákazníků Windows Azure – část I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms schémata dělení, strategií horizontálního dělení, tento článek popisuje způsob implementace horizontálního dělení a federace databáze SQL, od 19:49. Podobně jako řady bezporuchový ale přejde do další postupy podrobnosti.

Ukázkový kód:

- [Cloud Service Fundamentals v Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázková aplikace, která zahrnuje horizontálně dělené databáze. Popis schéma horizontálního dělení implementovat, najdete v části [DAL – horizontální dělení RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) na blogu Windows Azure.

> [!div class="step-by-step"]
> [Předchozí](data-storage-options.md)
> [další](unstructured-blob-storage.md)
