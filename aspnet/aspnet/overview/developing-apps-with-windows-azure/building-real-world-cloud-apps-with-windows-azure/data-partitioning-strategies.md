---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: "Data dělení strategie (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: dca016cb6293a346f5622cc272e510b182c86d58
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Data dělení strategie (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o řadě najdete v tématu [první kapitoly](introduction.md).


Dříve jsme viděli, jak je snadné škálování se webová úroveň cloudové aplikace, přidávání a odebírání webových serverů. Ale pokud budou se všechny stiskne stejné úložiště dat, kritické aplikace místo přesune z front-endu na back-end a datovou vrstvou je nejtěžší škálování. V této kapitole podíváme na způsob provádění datové vrstvy škálovatelné dělení dat do několika relačních databází, nebo kombinací relační databáze úložiště s jinými možnostmi datového úložiště.

Nastavení schéma rozdělení oddílů je nejvhodnější done až přední ze stejného důvodu již bylo zmíněno dříve: je velmi obtížné změnit strategie úložiště dat po aplikaci v produkčním prostředí. Pokud si myslíte, pevné předem o různý přístup, nemusíte mít "Twitter chvíli" Pokud vaše aplikace dojde k chybě nebo přestane fungovat po dlouhou dobu, během reorganizovat data a data přístupového kódu vaší aplikace.

## <a name="the-three-vs-of-data-storage"></a>Tři Vs úložiště dat

Aby bylo možné zjistit, zda je nutné strategie dělení a co by měl být, vezměte v úvahu tři otázky týkající se vašich dat:

- Nakonec svazek – kolik dat bude můžete uložit? Několik gigabajtů? Několik set gigabajtů? Terabajtů? Petabajty?
- Rychlosti – co je rychlost, jakou se zvýší data? Je interní aplikace, která není generování velké množství dat? Externí aplikaci, která zákazníků se nahrávání, bitové kopie a videa do?
- Různé – co typ dat bude ukládat? Relační, obrázky, páry klíč hodnota, sociálních grafy?

Pokud si myslíte, že budete mít mnoho svazku, rychlosti nebo různých, budete muset pečlivě zvažte, jaký druh schéma rozdělení oddílů bude nejlépe povolení škálování efektivně a efektivně roste a zajistit nespouští do kritické body, které v aplikaci.

V podstatě existují tři přístupy k vytváření oddílů:

- Vertikální dělení
- vodorovné rozdělení do oddílů
- Hybridní dělení

## <a name="vertical-partitioning"></a>Vertikální dělení

Svislé rozporcování je jako rozdělení tabulky podle sloupců: jednu sadu sloupců přejde do jednoho datového úložiště, a jinou sadu sloupců přejde do jiné datové úložiště.

Předpokládejme například, že Moje aplikace ukládá data o uživatelům, včetně bitové kopie:

![Datová tabulka](data-partitioning-strategies/_static/image1.png)

Pokud představují tato data jako tabulku a podívejte se na různé typy dat, můžete zobrazit mít tři sloupce na levé straně řetězec data, která může být efektivně uložená relační databázi, zatímco dva sloupce na pravé straně jsou v podstatě bajtová pole tohoto jazyka c Domů z bitové kopie souborů. Je možné do úložiště bitové kopie souboru dat v relační databázi, a velké množství uživatelů provést, protože nemusíte chtít uložit data do systému souborů. Nemusejí mít umožňující ukládání požadované objemů dat systému souborů nebo nemusí chtějí spravovat samostatný zálohování a obnovení systému. Tento postup funguje i pro místní databáze a pro malé množství dat v databázích cloudu. V místním prostředí může být snazší právě umožníte postará o všechno, co správce databáze.

Ale v databázi cloudové úložiště je poměrně drahé a k velkému počtu bitové kopie může zkontrolujte velikost databáze růst mimo hranice, na kterých se mohou pracovat efektivně. Tyto problémy můžete vyřešit tak, že dělení dat ve svislém směru, což znamená, že si vyberete nejvhodnější úložiště dat pro každý sloupec v tabulce dat. Co může být nejvhodnější v tomto příkladu je uvést dat řetězců v relační databázi a obrázků v úložišti objektů Blob.

![Datová tabulka svisle na oddíly](data-partitioning-strategies/_static/image2.png)

Ukládání bitových kopií v úložišti objektů Blob místo databáze je praktičtější v cloudu než v místním prostředí, protože nemusíte si dělat starosti o nastavení souborových serverů nebo Správa zálohování a obnovení data uložená mimo relační databáze: všechny který zpracovává pro vás automaticky službu úložiště objektů Blob.

Jedná se o rozdělení postup implementovali jsme v aplikaci opravit a podíváme kód pro tento v [kapitoly úložiště objektů Blob](unstructured-blob-storage.md). Bez této schéma rozdělení oddílů a za předpokladu, že obrázku průměrnou velikost 3 MB aplikace opravit pouze budou moct pro ukládání o 40 000 úloh před stiskne maximální velikost 150 gigabajtů. Po odebrání bitové kopie, můžete ukládat databázi 10 třikrát tolik úlohy; můžete přejít déle, než bude nutné myslíte o implementaci vodorovné schéma rozdělení oddílů. A jako aplikace škáluje, vaše výdaje zvýší pomalejší, protože hromadným požadavky na ukládání budou do velmi nenákladné úložiště objektů Blob.

## <a name="horizontal-partitioning-sharding"></a>Vodorovné rozdělení do oddílů (horizontálního dělení)

Vodorovné rozporcování je jako rozdělení tabulky po řádcích: jednu sadu řádků přejde do jednoho datového úložiště, a jinou sadu řádků, přejde do jiné datové úložiště.

Zadány stejnou sadu dat, bude další možnost pro uložení různých rozsahů názvů zákazníků v různých databází.

![Datová tabulka vodorovně na oddíly](data-partitioning-strategies/_static/image3.png)

Chcete velmi opatrně vašeho schématu horizontálního dělení a ujistěte se, že data jsou rovnoměrně rozloženy aby se zabránilo aktivní body. Tento jednoduchý příklad použití první písmeno příjmení nesplňuje tento požadavek, protože velké množství uživatelů příjmení, které začínají určitým běžné písmena. Omezení velikosti tabulky by setkají dříve, než by se dalo očekávat vzhledem k tomu, že některé databáze by získat velké, zatímco většina by zůstala malé.

Dolů straně vodorovné oddíly je, že může být obtížné provést dotazy napříč všemi dat. V tomto příkladu dotaz musel kreslení z až 26 různých databází, který zjistí všechna data uložená v aplikaci.

## <a name="hybrid-partitioning"></a>Hybridní dělení

Můžete kombinovat svislého a vodorovného rozdělení do oddílů. V příkladu data může například ukládání bitových kopií v úložišti objektů Blob a vodorovně rozdělení dat řetězec.

![Hybridní tabulky dat rozdělena na oddíly](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Vytváření oddílů produkční aplikace

Koncepčně je snadno zjistit, jak by fungují schéma rozdělení oddílů, ale žádné schéma rozdělení oddílů se zvýší složitost kódu a zavádí mnoho nové komplikací, ke které je třeba řešit. Pokud přesouváte bitové kopie do úložiště objektů blob, co udělat po službu úložiště dolů? Jak zpracováváte zabezpečení objektů blob Co se stane, pokud úložiště databáze a objektů blob synchronizované? Pokud jste horizontálního dělení, jak bude zpracovávat dotazování napříč všemi databází?

Komplikace jsou spravovat tak dlouho, dokud se pro ně plánování před přechodem do produkčního prostředí. Spousta lidí, kteří nebyla udělat, které chcete měly později. V průměru náš tým zákazníka poradní tým (CAT) získá panicked telefonní hovory o jednou za měsíc od zákazníků, jejichž aplikace trvá Opravdu veliký způsobem a nebyla tak při tomto plánování. A říká se něco podobného jako: "Pomozte Všechno, co dát do jednoho datového úložiště, a v 45 dní kliknu dostatek místa na něm!" A pokud máte spoustu součástí přístupu data store obchodní logika a máte zákazníkům, kteří používají vaši aplikaci, neexistuje žádný dobrý čas přejděte za den při migraci. Jsme skončili projít herculean úsilí na pomoc oddílu odběratele svá data za chodu bez dolů doby. Je velmi zajímavé a velmi strach, a není něco chcete být účastnící se můžete vyhnout. je-li! Přemýšlíte o to předem a integraci do své aplikace budou život mnohem snazší, pokud aplikace zvětšování později.

## <a name="summary"></a>Souhrn

Efektivní schéma rozdělení oddílů můžete povolit cloudové aplikaci, kterou chcete škálovat petabajty dat v cloudu bez kritická místa. A nemusíte předem platit za masivní počítače nebo širokou infrastrukturu, jak můžete, pokud aplikace byly spuštěny v datovém centru místní. V cloudu můžete postupně přidávat kapacitu podle ji potřebujete, a pouze věnujeme, pro co nejvíce, pokud používáte při použití ho.

V [další kapitoly](unstructured-blob-storage.md) ukážeme, jak aplikaci opravte implementuje vertikální dělení uložením bitové kopie v úložišti objektů Blob.

## <a name="resources"></a>Prostředky

Další informace o vytváření oddílů strategie najdete v následujících zdrojích informací.

Dokumentace:

- [Osvědčené postupy pro návrh rozsáhlých služeb v systému Windows Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper moduly SIMM značky a Michael Thomassy.
- [Microsoft Patterns and Practices - cloudu vzory návrhu](https://msdn.microsoft.com/library/dn568099.aspx). Najdete v části Vytvoření oddílů dat pokyny vzor horizontálního dělení.

Videa:

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Řada devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Uvede základními koncepty a architektury zásady způsobem, velmi přístupné a zajímavé, s scénářů čerpají z prostředí Microsoft zákazníka poradní tým (CAT) s skutečným zákazníkům. Přečtěte si rozdělení diskuzi v díl 7.
- [Vytváření Big: Poučení vyplývající z zákazníky služby Windows Azure – část I](https://channel9.msdn.com/Events/Build/2012/3-029). Moduly SIMM označit popisuje rozdělení schémata horizontálního dělení strategie, jak implementovat horizontálního dělení a federace databáze SQL, počínaje 19:49. Podobně jako u řady bezporuchový ale přejde do další postupy podrobnosti.

Ukázkový kód:

- [Základy služby v systému Windows Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázkovou aplikaci, která zahrnuje horizontálně dělené databáze. Popis schéma horizontálního dělení implementována najdete v tématu [DAL – horizontálního dělení RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) na blogu k systému Windows Azure.

>[!div class="step-by-step"]
[Předchozí](data-storage-options.md)
[další](unstructured-blob-storage.md)
