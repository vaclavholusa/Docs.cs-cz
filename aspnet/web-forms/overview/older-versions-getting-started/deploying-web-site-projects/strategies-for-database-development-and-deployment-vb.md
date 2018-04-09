---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Strategie pro vývoj databází a nasazení (VB) | Microsoft Docs
author: rick-anderson
description: Při nasazení aplikace datové poprvé můžete zkopírovat databázi slepě ve vývojovém prostředí do produkčního prostředí. B...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 26e6537b7cba704d3513a2e4ae32f9266834e6d3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="strategies-for-database-development-and-deployment-vb"></a>Strategie pro vývoj databází a nasazení (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Při nasazení aplikace datové poprvé můžete zkopírovat databázi slepě ve vývojovém prostředí do produkčního prostředí. Ale provádění blind kopírování v následných nasazení přepíše všechna data do provozní databáze. Místo toho nasazení databáze zahrnuje použití změn provedených v databázi vývoj od posledního nasazení do produkční databázi. V tomto kurzu prozkoumá tyto problémy a nabízí různé strategie vám pomůže při chronicling a provádění změn databázi provedeny od posledního nasazení.


## <a name="introduction"></a>Úvod

Jak je popsáno v předchozí kurzy, nasazení aplikace ASP.NET zahrnuje kopírování příslušné obsah z vývojového prostředí do produkčního prostředí. Nasazení není jednorázové události, ale spíš něco, co se stane při každém vydání nové verze softwaru, nebo když chyb nebo z důvodů zabezpečení byl pravděpodobně identifikovat a řešit. Při kopírování stránek ASP.NET, obrázků, soubory JavaScript a dalších tyto soubory k produkčnímu prostředí nepotřebujete sami se týkají jak tyto souborem se změnily od posledního nasazení. Můžete slepě zkopírujte soubor do produkčního prostředí, přepsání existujícího obsahu. Tato jednoduchost bohužel neprodlužuje nasazení databáze.

Při nasazení aplikace datové poprvé můžete zkopírovat databázi slepě ve vývojovém prostředí do produkčního prostředí. Ale provádění blind kopírování v následných nasazení přepíše všechna data do provozní databáze. Místo toho nasazení databáze zahrnuje použití *změny* provedených v databázi vývoj od posledního nasazení do produkční databázi. V tomto kurzu prozkoumá tyto problémy a nabízí různé strategie vám pomůže při chronicling a provádění změn databázi provedeny od posledního nasazení.

## <a name="the-challenges-of-deploying-a-database"></a>Obtíže spojené s nasazení databází

Předtím, než aplikaci na datové nasazený poprvé, je pouze jedna databáze, konkrétně databázi ve vývojovém prostředí, proto při nasazení aplikace datové poprvé slepě můžete kopírovat databázi vývojové prostředí do produkčního prostředí. Jakmile je aplikace nasazená existují dvě kopie databáze, ale: jeden vývoj a jeden v produkčním prostředí.

Mezi nasazeními se může stát synchronizované vývoj a provozní databáze. Při schématu s produkční databáze zůstává beze změny, může změnit schéma vývoj databáze s při přidání nových funkcí. Může přidat nebo odebrat sloupce, tabulky, zobrazení nebo uložené procedury. Také může být důležitá data, která bude přidána do databáze vývoj. Mnoho aplikací řízené daty zahrnují vyhledávací tabulky naplněný daty pevně, specifické pro aplikaci, která se nedají upravovat uživatele. Například webem aukce může mít rozevíracího seznamu s možností, které popisují podmínku položky se aukční režim: nové, jako je nové, funkční a veletrhu. Místo pevně kódováno tyto možnosti přímo v rozevíracím seznamu je obvykle lepší umístit do databázové tabulky. Pokud během vývoje, novou podmínku s názvem nízká se přidá do tabulky pak při nasazování aplikace tento stejný záznam musí být přidán do vyhledávací tabulky v produkční databázi.

Nasazení databáze v ideálním případě by zahrnovat kopírování databáze z vývojového do produkčního prostředí. Ale mějte na paměti, že po nasazení aplikace a obnovit vývoj, provozní databáze je právě naplněný reálná data z reálného uživatele. Proto pokud byste chtěli jednoduše zkopírovat databázi z vývojového do produkčního prostředí na další nasazení by přepsat provozní databáze a ztratit své stávající data. Net výsledkem je, že nasazení databáze boils k provádění změn provedených v databázi vývoj od posledního nasazení.

Vzhledem k tomu, že nasazení databáze zahrnuje použití změn v schéma a případně i data od posledního nasazení, historii změn musí být zachována (nebo v době nasazení určit) tak, aby tyto změny mohou být použity na produkční. Existují různé postupy pro správu a při použití změn do datového modelu.

### <a name="defining-the-baseline"></a>Definování směrného plánu

Chcete-li udržovat změny do databáze s vaší aplikací mít některé počáteční stav, ke kterému se změny použijí pro směrný plán. V jedné extreme počáteční stav může být prázdnou databázi bez tabulek, zobrazení nebo uložené procedury. Tyto standardní hodnoty dochází protokol velké změny, protože musí zahrnovat vytváření všech databáze s tabulek, zobrazení a uložených procedur společně s veškeré změny provedené po počátečním nasazení. Na druhém konci spektra můžete nastavit směrného plánu jako verzi databáze, která je původně nasazena do produkčního prostředí. Tato volba výsledkem mnohem menší změny protokolu, protože obsahuje pouze změny provedené v databázi po prvním nasazení. Toto je přístupů, které nechci. A samozřejmě můžete zvolit další střední tohoto přístupu silniční definování směrný plán s některými bodu mezi počáteční vytváření databáze a kdy je databáze poprvé nasazeno.

Jakmile máte vybrat směrný plán zvažte generování skriptu SQL, které mohou být provedeny znovu vytvořit základní verzi. Takového skriptu umožňuje rychle znovu vytvořit základní verzi databáze. Tato funkce je obzvláště užitečná v větší projekty, kde může být více vývojáře, kteří pracují v projektu nebo další prostředí, například testování nebo pracovní, že každý potřebovat vlastní kopii databáze.

Existuje mnoho různých nástrojů k dispozici vygenerovat skript SQL základní verze. Z SQL Server Management Studio (SSMS) můžete kliknout na databázi pravým tlačítkem, přejděte na dílčí úlohy a vyberte možnost generování skriptů. Spustí se průvodce skript, který můžete určit, aby generovat soubor, který obsahuje příkazy SQL k vytvoření databáze s objekty. Další možností je Průvodce publikováním databáze, který může generovat příkazů SQL pro nejen vytvoření schématu databáze, ale také data v tabulkách databáze. V Průvodci publikování databáze bylo podrobně zpět v *nasazení databáze* kurzu. Bez ohledu na to, jaký nástroj použijete, v části end byste měli mít soubor skriptu, který můžete použít k opětovnému vytvoření základní verzi databáze, měli potřeba nastat.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentace změny databáze v souvislém textu

Nejjednodušší způsob, jak udržovat protokol změn do datového modelu v průběhu fáze vývoje je záznam změn v souvislém textu. Například, pokud během vývoje aplikace, už nasazená přidat nový sloupec na `Employees` tabulky, odeberte sloupce z `Orders` tabulky a přidejte novou tabulku (`ProductCategories`), by spravovat textový soubor nebo dokument aplikace Microsoft Word pomocí následujících historie:

<a id="0.8_table01"></a>


| **Datum změny** | **Změňte podrobnosti** |
| --- | --- |
| 2009-02-03: | Přidání sloupce `DepartmentID` (`int`, není NULL) k `Employees` tabulky. Přidat omezení cizího klíče z `Departments.DepartmentID` k `Employees.DepartmentID`. |
| 2009-02-05: | Odebrané sloupec `TotalWeight` z `Orders` tabulky. Související data již zaznamenaná v `OrderDetails` záznamy. |
| 2009-02-12: | Vytvořit `ProductCategories` tabulky. Existují tři sloupce: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), a `Active` (`bit`, `NOT NULL`). Přidat omezení primárního klíče do `ProductCategoryID`a výchozí hodnota 1 znamená `Active`. |


Existuje několik nevýhod tohoto přístupu. Pro začátek neexistuje žádné Naděje pro automatizaci. Tyto změny kdykoli potřebovat má být použita pro databázi – například když je aplikace nasazená – vývojář musí implementovat ručně každý změnit, po jednom. Kromě toho pokud potřebujete rekonstrukci konkrétní verzi databáze ze směrného plánu pomocí protokol změn, to tak bude trvat více času s růstem velikosti souboru protokolu. Tato metoda jiné nevýhodou je, že přehlednost a úroveň podrobností každé položky protokolu změn je ponechán osobě záznam změn. Ve skupině s více vývojáři některé může být podrobnější, srozumitelnější nebo přesnější položek než jiné. Také je možné překlepy a další data související s lidské chyby položku.

Hlavní výhodou dokumentace změny databáze v souvislém textu je jednoduchost. Neuděláte t nutné znalost syntaxe jazyka SQL pro vytváření a změny databázové objekty. Místo toho můžete záznam změn v souvislém textu a implementovat je pomocí SQL Server Management Studio s grafickým uživatelským rozhraním.

Zachování změnu přihlášení prose není, admittedly, velmi sofistikované a získané t pracovní dobře u určité projekty, jako je ta, která jsou v oboru, velké máte časté změny do datového modelu, nebo zahrnují více vývojářů. Ale viděli jste tento přístup velmi dobře fungovaly v malých one-man projektů kde samotné vývojáře nemá silné pozadí v SQL syntaxi pro vytváření a změny databázové objekty, které mají pouze příležitostně změny do datového modelu.

> [!NOTE]
> Zatímco informace v protokolu změn je technicky potřeba jenom dokud nasazení čas, abyste se udržuje historii změn. Ale namísto zachování jedné, neustále rozšiřující změna souboru protokolu, zvažte, zda soubor jiné změny protokolu pro každou verzi databáze. Obvykle budete chtít verze databáze pokaždé, když je nasazena. Udržováním protokolu protokoly můžete, od standardních hodnot, znovu vytvořit všechny verze databáze spuštěním skriptů protokol změn od verze 1 a budete pokračovat, dokud se nedostanete na verzi, budete muset znovu vytvořit.


## <a name="recording-the-sql-change-statements"></a>Záznam změn příkazy SQL

Primární navracení zachování protokol změn v souvislém textu je nedostatek automatizace. V ideálním případě implementace změn databáze do provozní databáze v době nasazení může být stejně snadná jako kliknutím na tlačítko pro spuštění skriptu, namísto nutnosti ručně provést seznam pokyny. Tato automatizace je možné zachování protokol změn, která obsahuje tyto příkazy SQL, které používají pro úpravu datový model.

Syntaxe SQL obsahuje počet příkazů pro vytvoření a úprava různé objekty databáze. Například [ *příkazu CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), je-li provést, vytvoří novou tabulku s použitím zadaných sloupců a omezení. [ *Příkaz ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) upraví existující tabulky, přidání, odebrání nebo změna jeho sloupce nebo omezení. Existují také příkazy k vytvoření, úpravě a vyřadit indexy, zobrazení, uživatelem definované funkce, uložené procedury, aktivační události a dalších databázové objekty.

Vrácení k naší předchozího příkladu bitové kopie, kterou během vývoje aplikace už nasazená přidat nový sloupec na `Employees` tabulky, odeberte sloupce z `Orders` tabulky a přidejte novou tabulku (`ProductCategories`). Taková akce by způsobilo soubor protokolu změnit pomocí následujících příkazů SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Nabízení tyto změny do provozní databáze v době nasazení je operace jedním kliknutím: Otevřete SQL Server Management Studio, připojení k provozní databázi, otevřete okno Nový dotaz, vložte obsah protokol změn a klikněte na tlačítko spustit pro spuštění skriptu.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Pomocí nástroje porovnání synchronizace datové modely

Dokumentace změny databáze v souvislém textu je jednoduché, ale implementace změny vyžaduje vývojáře, aby každé změně na produkční databázi, jeden najednou. dokumentace příkazy SQL změnu implementace tyto změny na produkční databázi jako snadno a rychle jako při kliknutí na tlačítko se však vyžaduje učení a ovládnutí koncepcí příkazů jazyka SQL a syntaxe pro vytváření a změny databázové objekty. Nástroje pro porovnání databáze využít nejlepší z obou přístupů a zahodit nejhoršího.

Nástroj pro porovnání databáze porovná schématu nebo data z obou databází a zobrazí souhrnná sestava ukazuje, jak se liší databáze. Potom se kliknutím na tlačítko můžete vygenerovat příkazy SQL pro synchronizaci jeden nebo více objektů databáze. Stručně řečeno, nástroj pro porovnání databáze můžete použít k porovnání vývoj a provozní databáze v době nasazení, generování soubor, který obsahuje SQL příkazů, když provést, se použijí změny schématu databáze s provozním proto to odpovídá schématu databáze s vývoj.

Existují různé nástroje třetích stran databáze porovnání, které nabízí mnoho různých výrobců. Jedním z těchto příkladů je [ *porovnat SQL*](http://www.red-gate.com/products/SQL_Compare/), pomocí [ *softwaru brány Red*](http://www.red-gate.com/). Umožní s detailně provede procesem pomocí SQL porovnat porovnat a synchronizaci databáze schémata vývoj a provozní.

> [!NOTE]
> V době psaní tohoto textu byla aktuální verze SQL porovnat verze 7.1, s výpočet nákladů $395 Standard Edition. Můžete absolvovat stažením bezplatnou zkušební verzi 14 dnů.


Při porovnání SQL spuštění otevře se dialogové okno porovnání projekty zobrazující uložených projektů porovnat SQL. Vytvořte nový projekt. Spustí se Průvodce konfigurací projektu, kde informace o databázi k porovnání (viz obrázek 1). Zadejte informace pro vývoj a provozní databáze prostředí.


[![Porovnání vývoj a provozní databáze](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Obrázek 1**: porovnání vývoj a provozních databází ([Kliknutím zobrazit obrázek v plné velikosti](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> Pokud je databáze prostředí vývoj soubor databáze SQL Express Edition `App_Data` složku vašeho webu, budete muset zaregistrovat databázi v databázi serveru SQL Server Express, abyste jej zvolit z dialogových oken na obrázku 1. Nejjednodušší způsob, jak tomu je otevřít SQL Server Management Studio (SSMS), připojení k databázi serveru SQL Server Express a připojte databázi. Pokud nemáte v počítači nainstalována aplikace SSMS můžete stáhnout a nainstalovat bezplatnou [ *SQL Server 2008 Management Studio základní verze*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Kromě vyberete databáze pro porovnání, můžete také zadat řadu porovnání nastavení na kartě Možnosti. Jednou z možností, které chcete zapnout je "Ignorovat omezení a index názvy." Odvolat, že v předchozím kurzu jsme přidali, že aplikace služby databázové objekty pro vývoj a provozní databáze. Pokud jste použili `aspnet_regsql.exe` nástroj k vytváření těchto objektů na produkční databázi pak zjistíte, že se primární klíč a názvy jedinečné omezení liší vývoj a provozní databáze. V důsledku toho porovnat SQL bude příznak všechny tabulky služby aplikace jako liší. Můžete buď ponechat "Ignorovat omezení a index názvy" nezaškrtnuté a synchronizovat názvy omezení, nebo požádejte SQL porovnat ignorovat tyto rozdíly.

Po výběru databáze porovnání (a porovnání možnosti revize), klikněte na tlačítko porovnat teď zahájíte porovnání. Přes několik dalších sekund porovnat SQL prozkoumá schémata ve dvou databázích a vygeneruje zprávu o tom, jak se liší. I sunout záměrně provedeny některé změny databáze vývoj zobrazit, jak tyto rozdíly jsou uvedeny v rozhraní SQL porovnat. Jak je vidět na obrázku 2, I byla přidána `BirthDate` sloupec, který se `Authors` tabulky, odebrat `ISBN` sloupec z `Books` tabulku a přidat nové tabulky, `Ratings`, který je určen tak, aby uživatelé, kteří navštěvují rychlost lokality zkontrolovat knihy.

> [!NOTE]
> Datový model změny provedené v tomto kurzu měla provést pro ilustraci pomocí nástroje porovnání databáze. Tyto změny nebude nalezeno v databázi v budoucnu kurzy.


[![Porovnání SQL jsou uvedeny rozdíly mezi vývoj a provozní databáze](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Obrázek 2**: porovnání SQL jsou uvedeny rozdíly mezi vývoj a provozních databází ([Kliknutím zobrazit obrázek v plné velikosti](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


Porovnání SQL rozpis databázové objekty do skupin, rychle ukazuje, jaké objekty existují v obou databází, ale se liší, které existují objekty v jedné databáze, ale ne na druhou a jaké objekty jsou identické. Jak vidíte, jsou dva objekty, které existují v obou databází, ale se liší: `Authors` tabulky, který měl sloupec přidat, a `Books` tabulku, která obsahovala jeden odebrat. Se jeden objekt, který existuje pouze v databázi vývoj, a to nově vytvořený `Ratings` tabulky. A nejsou 117 objekty, které jsou identické v obou databází.

Výběr objektu databáze se zobrazí okno SQL rozdíly, které ukazuje, jak tyto objekty liší. Klade důraz okno SQL rozdíly, zobrazí v dolní části na obrázku 2, který `Authors` tabulky v databázi vývoj má `BirthDate` sloupec, který nebyl nalezen v `Authors` tabulky na produkční databázi.

Po zkontrolování rozdíly a výběr objektů, které se mají synchronizovat, je dalším krokem generují potřeba aktualizovat schéma produkční databáze s příkazy SQL tak, aby odpovídala databázi vývoj. To lze provést pomocí Průvodce synchronizace. Průvodce synchronizace potvrdí, jaké objekty k synchronizaci a shrnuje akce plánování (viz obrázek 3). Můžete synchronizovat databáze okamžitě nebo vygenerovat skript s příkazy SQL, které lze spustit ve volném čase.


[![Pomocí Průvodce synchronizace synchronizovat vaše databáze schémata](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Obrázek 3**: použijte Průvodce synchronizace synchronizovat vaše databáze schémata ([Kliknutím zobrazit obrázek v plné velikosti](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


Nástroje pro porovnání databáze jako softwaru brány červený SQL porovnat s zkontrolujte aplikuje změny schématu databáze vývoj stejně snadná jako bod a klikněte na provozní databázi.

> [!NOTE]
> Porovnání SQL porovná a synchronizuje dvě databáze *schémata*. Bohužel se nepodporuje porovnání a synchronizaci dat ve dvou databázích tabulky. Software brány Red nabízejí produkt s názvem [ *porovnání dat SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) který porovná a provede synchronizaci dat mezi dvěma databázemi, ale je samostatný produkt z porovnat SQL a jiné $395 stojí.


## <a name="taking-the-application-offline-during-deployment"></a>Pořízení aplikace do režimu Offline během nasazení

Jako jsme jste viděli v rámci tyto kurzy nasazení je proces, který zahrnuje několik kroků: kopírování stránek ASP.NET, hlavní stránky, šablon stylů CSS soubory, JavaScript soubory, Image a další nezbytné obsah z vývojového prostředí do provozního prostředí; kopírování si informace o konfiguraci specifické pro prostředí produkčního, v případě potřeby; a provádění změn do datového modelu od posledního nasazení. V závislosti na počtu souborů a složitost změny databáze tyto kroky může trvat od pár sekund několik minut na dokončení. Během této doby je webová aplikace v toku a uživatelům, kteří navštíví web může zaznamenat chyby nebo neočekávané chování.

Při nasazování webu je nejlepší do offline režimu webové aplikace "" až po dokončení nasazení. Převedení aplikace do režimu offline (a po dokončení procesu nasazení bude zálohování) je stejně snadná jako nahrání souboru a poté ji odstranit. Spuštění s prostředím ASP.NET 2.0, samotná existence soubor s názvem `app_offline.htm` v aplikaci s kořenovým adresářem převede celý web "do režimu offline." Každá žádost o stránku ASP.NET v této lokalitě je automaticky odpověděla s obsah `app_offline.htm` souboru. Odebraný tento soubor aplikace přejde do režimu online.

Převedení aplikace do režimu offline během nasazení, pak je stejně jednoduché jako nahrávání `app_offline.htm` souboru do produkčního prostředí s kořenový adresář. před zahájením procesu nasazení a poté ji odstranit (nebo jeho přejmenováním na něco jiného), po nasazení byla dokončena. Další informace o této technice najdete v článku s Jan Peterson trvá [ *Offline aplikace ASP.NET*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Souhrn

Hlavní těžkostí v nasazení aplikace řízené daty se soustředí kolem nasazení databáze. Vzhledem k tomu, že existují dvě verze databáze: 1 ve vývojovém prostředí - a jeden v provozním prostředí schémata tyto dvě databáze se může stát synchronizován při přidání nových funkcí v vývoj. Jaké s další, protože provozní databáze jako se naplní reálná data z reálného uživatele, nelze přepsat produkční databázi s databází upravené vývoj jako můžete při nasazování soubory, které tvoří aplikace (stránky ASP.NET, soubory obrázků a tak dále). Místo toho nasazení databáze zahrnuje implementace přesné sadu změn provedených v databázi vývoj na produkční databázi od posledního nasazení.

V tomto kurzu prohlédli tři techniky pro údržbu a použití protokolu změny v databázi. Nejjednodušší způsob je záznam změn v souvislém textu. Při této cílem provede implementaci těchto změn na produkční databázi ruční proces, nevyžaduje znalostní báze příkazů SQL pro vytváření a změny databázové objekty. Propracovanější postup a ten, který je mnohem víc palatable větší projekty nebo projekty s více vývojáři, je k zaznamenání změny jako řadu příkazů SQL. To výrazně hastens zavádí tyto změny na cílovou databázi. Nejlepší z obou přístupů lze dosáhnout pomocí nástroje pro porovnání databáze, jako je například Software brány Red s porovnat SQL.

V tomto kurzu se ukončí soustředili na nasazení aplikace řízené daty. Další sadu kurzy vypadá na to, jak reagovat na chyby v provozním prostředí. Postupy: zobrazení popisný chybovou stránku místo místo žlutý obrazovka smrti podíváme. A ukážeme, jak se přihlásit s podrobnosti chyby a jak vás upozorní, když dojde k takové chyby.

Radostí programování!

> [!div class="step-by-step"]
> [Předchozí](configuring-a-website-that-uses-application-services-vb.md)
> [další](displaying-a-custom-error-page-vb.md)
