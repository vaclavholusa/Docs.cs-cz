---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Strategie vývoje databáze a nasazení (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při nasazování aplikace řízené daty poprvé slepě můžete zkopírovat databázi ve vývojovém prostředí do produkčního prostředí. B....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 98ba771f6eafad84303279045e5b5c4a167c0678
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371928"
---
<a name="strategies-for-database-development-and-deployment-c"></a>Strategie vývoje databáze a nasazení (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Při nasazování aplikace řízené daty poprvé slepě můžete zkopírovat databázi ve vývojovém prostředí do produkčního prostředí. Ale provádění blind kopírování v následné nasazení přepíše všechna data do provozní databáze. Místo toho nasazení databáze zahrnuje použití změn provedených v databázi vývoj od posledního nasazení do provozní databáze. Tento kurz zkoumá tyto výzvy a nabízí různé strategie, která vám pomůže s chronicling a aplikováním změny provedené od posledního nasazení do databáze.


## <a name="introduction"></a>Úvod

Jak je popsáno v předchozích kurzech, nasazení aplikace ASP.NET zahrnuje kopírování relevantní obsah z vývojového prostředí do produkčního prostředí. Nasazení je jednorázová, ale spíše něco, co se stane při každém vydání nové verze softwaru nebo když chyby nebo byly identifikovat a řešit otázky zabezpečení. Při kopírování stránek ASP.NET, obrázky, soubory jazyka JavaScript a dalších takových souborů k produkčnímu prostředí nepotřebujete sami se týkají jak tyto souborem se změnily od posledního nasazení. Můžete slepě zkopírujte soubor do produkčního prostředí, přepíše existující obsah. Takto se bohužel nevztahuje na nasazení databáze.

Při nasazování aplikace řízené daty poprvé slepě můžete zkopírovat databázi ve vývojovém prostředí do produkčního prostředí. Ale provádění blind kopírování v následné nasazení přepíše všechna data do provozní databáze. Místo toho nasazení databáze zahrnuje použití *změny* provedených v databázi vývoj od posledního nasazení do provozní databáze. Tento kurz zkoumá tyto výzvy a nabízí různé strategie, která vám pomůže s chronicling a aplikováním změny provedené od posledního nasazení do databáze.

## <a name="the-challenges-of-deploying-a-database"></a>Úkoly nasazení databáze

Před prvním nasazení aplikace s daty, existuje pouze jedna databáze, konkrétně databáze ve vývojovém prostředí, což je důvod, proč při nasazování aplikace řízené daty poprvé slepě můžete zkopírovat databázi v vývojové prostředí do produkčního prostředí. Ale po nasazení aplikace existují dvě kopie databáze: jeden ve fázi vývoje a v produkčním prostředí.

Mezi nasazeními vývoje a provozu databáze se může stát synchronizovaný. Když schéma s produkční databáze zůstane beze změny, schéma vývoje databáze s můžou změnit, protože jsou přidány nové funkce. Může přidat nebo odebrat sloupce, tabulek, zobrazení nebo uložené procedury. Také může být důležitá data, která se přidá k databázi vývoj. Mnoho aplikací řízených daty zahrnují vyhledávacími tabulkami vyplní daty pevně zakódované, specifické pro aplikaci, která se nedají upravovat uživatele. Na webu aukce může mít například rozevíracího seznamu s možnosti, které popisují podmínky položky se aukční režim: nové, jako jsou nové, dobré a veletrhu. Místo pevného kódování tyto možnosti přímo v rozevíracím seznamu je obvykle vhodnější je umístíte v databázové tabulce. Pokud během vývoje, novou podmínku s názvem špatné se přidá do tabulky pak při nasazování aplikace tento stejný záznam musí být přidán do tabulky vyhledávání v provozní databázi.

Nasazení databáze v ideálním případě by vyžadovalo kopírování databáze z vývojového do produkčního prostředí. Nezapomeňte ale, že po nasazení aplikace a obnovit vývoj, provozní databáze se plní reálná data z skutečné uživatele. Proto pokud byste chtěli jednoduše zkopírovat databázi z vývojového do produkčního prostředí v další nasazení by přepsat provozní databáze a ztratit svá stávající data. Net výsledkem je, že nasazení databáze boils k provádění změn provedených v databázi vývoj od posledního nasazení.

Protože nasazení databáze zahrnuje použití změn schématu a případně také data od posledního nasazení, historii změn musí být udržovány (nebo v době nasazení určit) tak, aby tyto změny mohou být použity v produkčním prostředí. Existuje řada různých technik pro správu a provádění změn do datového modelu.

### <a name="defining-the-baseline"></a>Definování směrného plánu

Zachovat změny do databáze s vaší aplikace budete muset mít nějakou počáteční stav, ke které se použijí změny do směrného plánu. Na jeden extreme počáteční stav může být prázdná databáze bez tabulek, zobrazení nebo uložené procedury. Tyto standardní hodnoty za následek velký Změnový protokol vzhledem k tomu, že musí obsahovat vytváření všechny databáze s tabulek, zobrazení a uložených procedur spolu se všechny změny provedené po počátečním nasazení. Na druhém konci spektra můžete nastavit směrného plánu jako verzi databáze, který je původně nasazené do produkčního prostředí. Tato volba výsledkem mnohem menší Změnový protokol, protože zahrnuje pouze změny provedené v databázi po prvním nasazení. Jedná se o postup, který raději. A samozřejmě můžete použít další střední silniční přístupu, definování směrný plán s některými bod mezi počáteční vytváření databáze a při prvním nasazení databáze.

Jakmile budete mít zvolený základ zvažte generování skriptu SQL, který lze spustit znovu vytvořit základní verzi. Takového skriptu umožňuje rychle znovu vytvořit základní verzi databáze. Tato funkce je zvláště užitečné ve větších projektů tam, kde může být více vývojáře, kteří pracují na projektu nebo další prostředí, jako je testování nebo pracovní, že každý potřebovat vlastní kopii databáze.

Existuje řada různých nástrojů vašim službám se vygenerovat skript SQL ze základní verze. Z SQL Server Management Studio (SSMS) můžete kliknout na databázi pravým tlačítkem, přejděte na podnabídky úkoly a zvolte možnost Generovat skripty. Tím se spustí Průvodce skript, který můžete dát pokyn k vygenerování souboru, který obsahuje příkazy SQL k vytvoření databáze s objekty. Další možností je Průvodce publikováním databáze, který může generovat příkazy SQL nejen vytvoření schématu databáze, ale také k datům v databázových tabulkách. Průvodce publikováním databáze byla podrobně zpátky *nasazení databáze* kurzu. Bez ohledu na to, jaký nástroj používáte nakonec byste měli mít soubor skriptu, který vám pomůže se základní verzi databáze, znovu vytvořte potřeba nastat.

## <a name="documenting-the-database-changes-in-prose"></a>Jak dokumentovat změny databáze v souvislém textu

Nejjednodušší způsob, jak udržovat protokolu změn do datového modelu ve vývojové fázi spočívá v zaznamenání změn v souvislém textu. Pokud během vývoje aplikace už nasadili přidáte nový sloupec, třeba `Employees` tabulky, odeberte sloupec z `Orders` tabulku a přidat novou tabulku (`ProductCategories`), by spravovat textový soubor nebo dokument aplikace Microsoft Word pomocí následujících historie:

<a id="0.4_table01"></a>


| **Datum změny** | **Podrobnosti o změně** |
| --- | --- |
| 2009-02-03: | Přidaný sloupec `DepartmentID` (`int`, NOT NULL) do `Employees` tabulky. Přidat omezení cizího klíče z `Departments.DepartmentID` k `Employees.DepartmentID`. |
| 2009-02-05: | Odebrané sloupce `TotalWeight` z `Orders` tabulky. Související data již zaznamenaná v `OrderDetails` záznamy. |
| 2009-02-12: | Vytvoří `ProductCategories` tabulky. Existují tři sloupce: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), a `Active` (`bit`, `NOT NULL`). Přidat omezení primárního klíče na `ProductCategoryID`a výchozí hodnotu 1 `Active`. |


Existuje mnoho z nevýhod tohoto přístupu. Pokud začínáte není k dispozici žádnou naději pro automatizaci. Kdykoli těchto změn je potřeba použít k databázi – například když je aplikace nasazená – vývojář musí implementovat ručně každý změnit, postupně po jednom. Kromě toho pokud budete potřebovat k rekonstrukci konkrétní verzi databáze ze standardních hodnot pomocí protokolu změn, to uděláte tak bude trvat více a více času nárůstu velikosti souboru protokolu. Tato metoda jiné nevýhodou je, že jasné a úroveň podrobností každé položky protokolu změn je ponecháno na osobu záznam změny. V týmu s více vývojářů některé mohou vytvořit podrobnější čitelnější a přesnější položky než jiné. Překlepy a další data související s lidských položka chyby jsou také je to možné.

Hlavní výhodou jak dokumentovat změny databáze v souvislém textu je jednoduché. Zadávat t nutné znalosti syntaxe jazyka SQL pro vytváření a změny databázové objekty. Místo toho záznam změn v souvislém textu a implementovat je pomocí SQL Server Management Studio s grafickým uživatelským rozhraním.

Zachování vašich protokol změn v souvislém textu není, admittedly, velmi propracované a získané nefungují s některých projektů, jako je například těch, které jsou v oboru, velké máte časté změny do datového modelu, nebo zahrnují více vývojářů. Ale viděli jste tento přístup poměrně dobře fungovaly v malých one-man projektů, které mají jenom občasné změny do datového modelu a kde samostatný vývojář, který nemá silné na pozadí v syntaxi SQL pro vytváření a změny databázové objekty.

> [!NOTE]
> Informace v protokolu změn je technicky, stačí do nasazení – čas, můžu doporučují udržovat historii změn. Ale namísto zachování jeden, neustále se rozšiřující soubor protokolu změn, zvažte, jestli by různých změna souboru protokolu pro každou verzi databáze. Obvykle můžete na verzi databáze pokaždé, když je nasazená. Díky udržování protokolu změn protokolů můžete, od výchozího stavu, znovu vytvořit všechny verze databáze spuštěním skripty protokolu změn od verze 1 a budete pokračovat, dokud se nedostanete na verzi, budete muset znovu vytvořit.


## <a name="recording-the-sql-change-statements"></a>Záznam změn příkazy SQL

Hlavní nevýhodou udržování protokolu změn v souvislém textu je nedostatek služby automation. V ideálním případě provádění změn databáze do provozní databáze v době nasazení může být stejně snadné jako kliknutí na tlačítko pro spuštění skriptu, spíše než by bylo nutné ručně provést seznam pokyny. Tato automatizace je možné udržováním Změnový protokol, který obsahuje příkazy SQL, používají pro úpravu datového modelu.

Syntaxe jazyka SQL obsahuje několik příkazů pro vytváření a úpravu různých databázové objekty. Například [ *příkazu CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), při spuštění vytvoří novou tabulku se sloupci zadaného a omezení. [ *Příkaz ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) upraví existující tabulky, přidání, odebrání nebo změna jeho sloupce nebo omezení. Existují také příkazy k vytvoření, úpravě a vyřadit indexy, zobrazení, uživatelsky definovaných funkcí, uložených procedur, triggerů a ostatních databázových objektů.

Vrací naše předchozího příkladu, při vývoji dříve nasazené aplikace přidat nový sloupec, obrázku, který je `Employees` tabulky, odeberte sloupec z `Orders` tabulku a přidat novou tabulku (`ProductCategories`). Výsledkem bude taková akce v souboru protokolu změn pomocí následujících příkazů SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Doručením (push) změny do provozní databáze v době nasazení je operace jedním kliknutím: Otevřete SQL Server Management Studio, připojte se k provozní databázi, otevřete okno Nový dotaz, vložte obsah protokolu změn a klikněte na tlačítko spustit pro spuštění skriptu.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Pomocí nástroje porovnání synchronizace datových modelů

Jak dokumentovat změny databáze v souvislém textu je jednoduché, ale implementace změny vyžaduje vývojář, abyste mohli měnit každou na jeden provozní databáze najednou. dokumentování příkazy SQL change díky implementaci těchto změn na produkční databázi jako snadné a rychlé jako při kliknutí na tlačítko, ale vyžaduje učení a ovládnutí příkazů jazyka SQL a syntaxe pro vytváření a změny databázové objekty. Databázové nástroje porovnání využít to nejlepší z obou metod a zahodit nejhoršího.

Nástroj pro porovnání databáze porovná schématu nebo dat dvě databáze a zobrazuje souhrnnou sestavu zobrazující, jak se liší databází. Pak kliknutím na tlačítko, můžete vygenerovat příkazů SQL pro synchronizaci jeden nebo více databázových objektů. Řečeno v kostce, nástroji pro porovnávání databáze můžete použít k porovnání vývoj a provozních databází během nasazení, generuje se soubor, který obsahuje SQL příkazy, které při spuštění se použít změny schématu databáze s produkčním prostředí tak, že Zrcadlí schéma databáze s vývoje.

Existují různé nástroje třetích stran databáze porovnání nabízí mnoho různých výrobců. Je jedním z takových příkladů [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), [ *Software brány Red*](http://www.red-gate.com/). Umožní s procesem s použitím použití SQL Compare můžete porovnat a synchronizovat schémata databáze vývoje a provozu.

> [!NOTE]
> V době psaní tohoto textu byl aktuální verze SQL Compare verze 7.1, s edicí Standard ocenění 395 $. Můžete absolvovat stáhněte si bezplatnou zkušební verzi 14 dní.


Při spuštění porovnání SQL otevře se dialogové okno porovnání projektů zobrazující uložených projektů SQL Compare. Vytvořte nový projekt. Spustí se Průvodce konfigurací projektu, který vyzve k zadání informací o databázi k porovnání (viz obrázek 1). Zadejte informace pro vývoj a provoz prostředí databáze.


[![Porovnání vývoje a provozních databází](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Obrázek 1**: porovnání vývoje a provozních databází ([kliknutím ji zobrazíte obrázek v plné velikosti](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Pokud vaše vývojové prostředí databázi je v souboru databáze SQL Express Edition `App_Data` složku vašeho webu, budete muset zaregistrovat databáze v databázi serveru SQL Server Express, abyste mohli vybrat z dialogových oken na obrázku 1. Nejjednodušší způsob, jak to provést, je otevřete SQL Server Management Studio (SSMS), připojte se k databázovému serveru SQL Server Express a připojte databázi. Pokud nemáte v počítači nainstalovanou aplikaci SSMS můžete stáhnout a nainstalovat bezplatnou [ *SQL Server 2008 Management Studio základní verze*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Kromě výběru databázi, kterou chcete porovnat, můžete také zadat různá nastavení porovnání na kartě Možnosti. Jednou z možností, které chcete zapnout je "Ignorovat omezení a index názvy." Připomínáme, že v předchozím kurzu jsme přidali aplikace služby databázových objektů databáze vývoje a provozu. Pokud jste použili `aspnet_regsql.exe` nástroj k vytváření těchto objektů na provozní databázi a zjistíte, že primární klíč a názvy jedinečné omezení se liší mezi vývojovou a provozní databáze. V důsledku toho porovnání SQL se označí všechny tabulky aplikace služby jako lišící se. "Ignorovat omezení a index názvy" můžete buď nechat nezaškrtnuté a synchronizovat omezení názvů, nebo dáte pokyn, aby SQL Compare ignorovat tyto rozdíly.

Po výběru databázi, kterou chcete porovnat (a možnosti porovnání revize), klikněte na tlačítko porovnání nyní zahájíte porovnání. V dalších několika sekund SQL Compare zkontroluje schémat ve dvou databázích a generuje sestavu o tom, jak se liší. Můžu odebrat záměrně provedli určité změny pro vývoj databáze, kterou chcete zobrazit, jak tyto rozdíly jsou uvedeny ve rozhraní SQL Compare. Jak je vidět na obrázku 2, můžu byla přidána `BirthDate` sloupec, který se `Authors` tabulky odebrán `ISBN` sloupec z `Books` tabulku a přidat novou tabulku, `Ratings`, který slouží k umožní uživatelům, kteří navštíví rychlost lokality přezkoumání knihy.

> [!NOTE]
> Datový model změny provedené v tomto kurzu jste dokončili pro ilustraci, pomocí nástroje porovnání databáze. Tyto změny v databázi nenajde v budoucích kurzech.


[![Porovnání SQL jsou uvedeny rozdíly mezi vývojovou a provozních databází](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Obrázek 2**: porovnání SQL jsou uvedené rozdíly mezi vývoje a provozních databází ([kliknutím ji zobrazíte obrázek v plné velikosti](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


Porovnání SQL boří databázových objektů do skupin, rychle zobrazí objekty, které existují v databázi i databázi, ale se liší, které existují objekty v jedné databázi, ale nikoli u druhého a jaké objekty jsou identické. Jak vidíte, jsou dva objekty, které existují v databázi i databázi, ale jsou odlišné: `Authors` tabulku, která má sloupec přidali, a `Books` tabulku, která obsahovala jeden odebrat. Existuje jeden objekt, který existuje pouze v databázi vývoj, konkrétně nově vytvořený `Ratings` tabulky. A 117 objekty, které jsou stejné v obou databázích.

Výběr objektu databáze zobrazí v okně SQL ve službě, která ukazuje, jak tyto objekty se liší. V okně SQL ve službě, zobrazí v dolní části na obrázku 2 se zvýrazní, který `Authors` tabulky v databázi vývoj má `BirthDate` sloupec, který se nachází v `Authors` tabulku v provozní databázi.

Po kontrole rozdíly a výběr objektů, které chcete synchronizovat, je dalším krokem je ke generování potřeba aktualizovat schéma produkční databáze s příkazy SQL tak, aby odpovídala databázi vývoj. To lze provést pomocí Průvodce synchronizace. Průvodce synchronizace potvrdí, které objekty k synchronizaci a shrnuje akce plánování (viz obrázek 3). Můžete synchronizovat databáze hned nebo generovat skript SQL příkazy, které lze spustit ve volném čase.


[![Pomocí Průvodce synchronizace můžete synchronizovat vaše schémata databáze](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Obrázek 3**: použijte Průvodce synchronizace synchronizovat vaše schémata databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Porovnání nástroje databáze, jako je Red Software brány s porovnání SQL Zkontrolujte použití změn schématu databáze vývoje do provozní databáze, tak i pomocí ukázání a kliknutí.

> [!NOTE]
> Porovnání SQL porovná a synchronizuje dvě databáze *schémata*. Bohužel to neobsahuje porovnání a synchronizovat data v tabulkách dvě databáze. Software brány Red nabízejí produkt s názvem [ *porovnání dat SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) , který porovnává a provede synchronizaci dat mezi dvěma databázemi, ale je samostatný produkt z porovnání SQL a jiné $395 náklady.


## <a name="taking-the-application-offline-during-deployment"></a>Přepnutí aplikace do offline režimu během nasazení

Jako jsme viděli v těchto kurzech ve nasazení je proces, který zahrnuje několik kroků: kopírování stránek ASP.NET, stránky předlohy, šablon stylů CSS soubory, JavaScript soubory, obrázky a další nezbytné obsah z vývojového prostředí na produkční prostředí; kopírování informací specifických pro prostředí konfigurace produkčního prostředí, v případě potřeby; a použití změn do datového modelu od posledního nasazení. V závislosti na počtu souborů a složitosti databáze změny těchto kroků může trvat pár sekund několik minut, než k dokončení. Během tohoto časového intervalu pro webové aplikace je v toku a návštěvníků webu může docházet, chyby nebo neočekávané chování.

Při nasazování webu je nejvhodnější pro webovou aplikaci "do režimu offline" trvat, dokud se nedokončí nasazení. Přepínat aplikace do offline režimu (a spustit ho zpátky až po dokončení procesu nasazení) je stejně jednoduché jako nahrání souboru a její následné odstranění. Spouštění pomocí technologie ASP.NET 2.0, pouhé přítomnost souboru s názvem `app_offline.htm` v aplikaci s kořenovým adresářem přebírá celý web "do režimu offline." Jakákoli žádost stránky ASP.NET na daném webu je automaticky odpověděl s obsahem `app_offline.htm` souboru. Po odebrání tohoto souboru aplikace přejde do režimu online.

Převedení aplikace do offline režimu během nasazení, pak je stejně jednoduché jako nahrávání `app_offline.htm` souboru do produkčního prostředí s kořenový adresář. před zahájením procesu nasazení a poté odstraní (nebo přejmenování na něco jiného) po nasazení byla dokončena. Další informace o této techniky najdete v článku s Jan Peterson, přičemž [ *Offline aplikace ASP.NET*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Souhrn

Hlavní těžkostí v nasazení aplikace s daty se soustředí kolem nasazení databáze. Vzhledem k tomu, že existují dvě verze databáze: 1 ve vývojovém prostředí – a v produkčním prostředí může být schémata těchto dvou databází synchronizován při přidání nové funkce ve vývoji. Jaké další, protože provozní databáze jako se vyplní reálná data z reálného uživatele, se nedá přepsat provozní databáze s upravenou vývoje databází jako při nasazování souborů, které tvoří aplikaci (stránky ASP.NET, s soubory obrázků a tak dále). Místo toho nasazení databáze zahrnuje implementace přesnou sadu změn do vývoje databáze na produkční databáze od posledního nasazení.

V tomto kurzu se podívali na tři metody pro údržbu a použití protokolu změn databází. Nejjednodušším způsobem je záznam změn v souvislém textu. Přestože tento postup díky implementaci těchto změn v provozní databázi ruční proces, nevyžaduje znalost příkazů SQL pro vytváření a změny databázové objekty. Složitější přístup a ten, který je mnohem více palatable ve větší projekty a projekty s více vývojářů, spočívá v zaznamenání změny jako posloupnost příkazů SQL. To výrazně hastens zavádí tyto změny do cílové databáze. To nejlepší z obou metod lze dosáhnout pomocí nástroje, porovnání databáze, jako je Red Software brány s SQL Compare.

V tomto kurzu dokončíte soustředili na nasazení aplikace řízené daty. Zjistí další sadu kurzy týkající se reakce na chyby v provozním prostředí. Podíváme se na tom, jak zobrazit popisná chybová stránka spíše místo žlutý obrazovky smrti. A uvidíme, jak protokolovat podrobnosti o chybě s a tom, jak vás upozornit, když dojde k takové chybě.

Všechno nejlepší programování!

> [!div class="step-by-step"]
> [Předchozí](configuring-a-website-that-uses-application-services-cs.md)
> [další](displaying-a-custom-error-page-cs.md)
