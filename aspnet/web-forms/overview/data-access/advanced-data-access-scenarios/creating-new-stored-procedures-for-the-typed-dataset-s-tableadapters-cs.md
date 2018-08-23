---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Vytvoření nových uložených procedur komponentami TableAdapter typové datové sady (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozích kurzech jsme vytvořili SQL příkazy v našem kódu a předat příkazy databáze, kterou chcete provést. Alternativním přístupem je použití s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 0932749d6cf1665eedd5f452ab5dd63ed8678962
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754282"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Vytvoření nových uložených procedur komponentami TableAdapter typové datové sady (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) nebo [stahovat PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> V předchozích kurzech jsme vytvořili SQL příkazy v našem kódu a předat příkazy databáze, kterou chcete provést. Alternativním přístupem je použití uložené procedury, kde jsou příkazy SQL předem definovaných v databázi. V tomto kurzu jsme zjistěte, jak mají generovat nové uložené procedury pro nás Průvodci vytvořením objektu TableAdapter.


## <a name="introduction"></a>Úvod

Vrstva přístupu dat (DAL) pro tyto kurzy typované datové sady používá. Jak je popsáno v [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu typy datových sad se skládají z DataTables silného typu a objekty TableAdapter. Datové tabulky představují logické entity v systému během rozhraní objekty TableAdapter s podkladové databázi a provést na datový přístup do práce. Jedná se o naplnění datové tabulky s daty, spouštěním dotazů, které vracejí skalární data, vkládání, aktualizace a odstranění záznamů z databáze.

SQL příkazy provedl objekty TableAdapter může být buď SQL příkazy ad-hoc, například `SELECT columnList FROM TableName`, nebo uložených procedur komponentami TableAdapter. Objekty TableAdapter v naší architektury pomocí ad-hoc příkazů jazyka SQL. Mnoho vývojáři a správci databází, ale dáváte přednost uložených procedur SQL příkazy ad-hoc z důvodů zabezpečení, udržovatelnost a aktualizovatelnosti. Ostatní ardently raději ad-hoc příkazů SQL pro jejich flexibilitu. V vlastní pracovní můžu upřednostnit uložených procedur SQL příkazy ad-hoc, ale rozhodli používat příkazy SQL ad-hoc ke zjednodušení z předchozích kurzů.

Při definování TableAdapter nebo přidání nové metody, třídy TableAdapter s průvodce usnadňuje stejně snadno vytvořit nové uložené procedury nebo použití existující uložené procedury, stejně jako na použití ad-hoc SQL. V tomto kurzu prozkoumáme how má TableAdapter s průvodce automaticky generovat uložené procedury. V dalším kurzu se podíváme na konfigurace můžete použít existující nebo ručně vytvořené uložené procedury s metody třídy TableAdapter.

> [!NOTE]
> Najdete v blogovém příspěvku Rob Howard [není postupy ukládají pomocí položky ještě?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) a [Frans Bouma](https://weblogs.asp.net/fbouma/) s blogu [uložené procedury jsou chybná, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) pro živá diskuse o výhody a nevýhody uložené procedury a ad-hoc SQL.


## <a name="stored-procedure-basics"></a>Základy uložené procedury

Funkce jsou společné pro všechny programovací jazyky konstrukce. Funkce je kolekce příkazů, které jsou spouštěny, když je tato funkce volána. Funkce může přijmout vstupní parametry a může volitelně vrátit hodnotu. *[Uložené procedury](http://en.wikipedia.org/wiki/Stored_procedure)*  jsou databáze konstrukce, které řada podobností s funkcí v programovacích jazycích. Uloženou proceduru se skládá ze sady příkazů T-SQL, které jsou spouštěny, když se volá uloženou proceduru. Uloženou proceduru mohou použít nulu pro velký počet vstupních parametrů a může vrátit skalárních hodnot, výstupní parametry nebo nejčastěji sady výsledků z `SELECT` dotazy.

> [!NOTE]
> Uložené procedury jsou často označovány jako sprocs nebo aktualizace Service packu.


Uložené procedury lze vytvořit pomocí [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) příkazu T-SQL. Například následující skript T-SQL vytvoří uloženou proceduru s názvem `GetProductsByCategoryID` , která přijímá jeden parametr s názvem `@CategoryID` a vrátí `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` pole těchto sloupců `Products` tabulky, které mají odpovídající `CategoryID` hodnotu:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Po vytvoření tuto uloženou proceduru lze volat, použijte tuto syntaxi:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> V dalším kurzu prozkoumáme vytváření uložených procedur prostřednictvím integrovaného vývojového prostředí sady Visual Studio. V tomto kurzu ale budeme nechat automaticky vytvořit úložné procedury pro nás Průvodci vytvořením objektu TableAdapter.


Kromě jednoduše vrací data, jsou uložené procedury často používány k provádění více příkazů databáze v rámci jedné transakce. Uloženou proceduru s názvem `DeleteCategory`, například může trvat, než `@CategoryID` parametr a provedení dvou `DELETE` příkazy: první, z nich k odstranění souvisejících produktů a druhá odstranění zadané kategorie. Více příkazů v rámci uložené procedury jsou *není* automaticky zabalené v rámci transakce. Další příkazy jazyka T-SQL musí být vystaveno pro zajištění uložená procedura s, který více příkazů zachází jako atomickou operaci. Uvidíme, jak zabalit pomocí uložené procedury s příkazů v rámci oboru transakcí v následujícím kurzu.

Při použití uložených procedur v rámci architekturu, volání metod vrstvy přístupu k datům s konkrétní uložené procedury místo vydání příkazu SQL ad-hoc. Toto místo je definována v rámci architektura aplikací s centralizuje umístění příkazů jazyka SQL proveden (v databázi). Tato centralizace pravděpodobně usnadňuje jeho hledání, analyzovat a optimalizovat dotazy a poskytuje mnoho dostáváme postupně jasnější představu, kde a jak se používá databázi.

Další informace o základní informace uložené procedury vyhledejte prostředky v části Další čtení na konci tohoto kurzu.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Krok 1: Vytvoření pokročilé datové přístup vrstvy scénáře webových stránek

Než začneme naše diskuse o vytvoření DAL pomocí uložených procedur, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, kterou potřebujeme pro toto a další kurzy několik. Začněte přidáním novou složku s názvem `AdvancedDAL`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Přidání stránky technologie ASP.NET pro pokročilé datové přístup vrstvy scénáře kurzy](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Obrázek 1**: Přidání stránky technologie ASP.NET pro pokročilé datové přístup vrstvy scénáře kurzy


V jiných složkách, jako jsou `Default.aspx` v `AdvancedDAL` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


A konečně, přidejte tyto stránky jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za práce s daty v dávce `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro pokročilé scénáře kurzy vrstvy DAL.


![Mapa webu nyní obsahuje položky pro kurzy DAL pokročilé scénáře](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Obrázek 3**: mapy webu nyní obsahuje záznamy pro kurzy DAL pokročilé scénáře


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Krok 2: Konfigurace TableAdapter k vytvoření nových uložených procedur

Abychom si předvedli, vytvoření vrstvy přístupu k datům, která používá uložené procedury namísto SQL příkazy ad-hoc, umožňují s vytvořit nové zadané datové sady v `~/App_Code/DAL` složku s názvem `NorthwindWithSprocs.xsd`. Protože tento proces podrobně jsme již prošli v předchozích kurzech, jsme bude pokračovat rychle provede postupem tady. Pokud se zablokuje nebo potřebujete další podrobné pokyny při vytváření a konfigurace datové sady typu, vraťte se do [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) kurzu.

Přidejte novou datovou sadu do projektu kliknutím pravým tlačítkem na `DAL` složky, výběrem přidat novou položku a výběr šablony datové sady, jak je znázorněno na obrázku 4.


[![Přidat nový typové datové sady do projektu s názvem NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Obrázek 4**: Přidání nové datové sady typu do projektu s názvem `NorthwindWithSprocs.xsd` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


To bude vytvářet nová datová sada typu, otevřete návrhář, vytvoření nového TableAdapter a spusťte Průvodce nastavením TableAdapter. První krok průvodce nastavením TableAdapter s dotazem, abychom vyberte databázi pro práci s. Připojovací řetězec k databázi Northwind by měly být uvedeny v rozevíracím seznamu. Tuto možnost vyberte a klikněte na tlačítko Další.

Na této obrazovce Další jsme můžete zvolit, jak by měl TableAdapter přistupovat k databázi. V předchozích kurzech jsme vybrali první možnost, použít SQL příkazy. Pro účely tohoto kurzu druhou možnost vyberte, vytvořit nové uložené procedury a klikněte na tlačítko Další.


[![Dáte pokyn, aby TableAdpater k vytvoření nových uložených procedur](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Obrázek 5**: dáte pokyn, aby TableAdpater k vytvoření nové uložené procedury ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Stejně jako s použitím příkazů jazyka SQL ad-hoc, v následujícím kroku jsme vyzváni k zadání `SELECT` příkazu pro TableAdapter s hlavním dotazu. Ale namísto použití `SELECT` příkaz dotazu ad-hoc provést přímo tady zadáte, TableAdapter s Průvodce vytvoří uloženou proceduru, která obsahuje tato `SELECT` dotazu.

Pomocí následujících `SELECT` dotazu pro tuto TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Zadejte dotaz SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Obrázek 6**: Zadejte `SELECT` dotazu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> Výše uvedený dotaz se mírně liší od hlavní dotaz `ProductsTableAdapter` v `Northwind` typované datové sady. Vzpomeňte si, že `ProductsTableAdapter` v `Northwind` zadali datová sada obsahuje dvě korelační poddotazy tak, aby název kategorie a název společnosti pro každou kategorii produktů s a dodavateli. V nadcházející [aktualizace komponenty TableAdapter na použití připojí](updating-the-tableadapter-to-use-joins-cs.md) kurzu se podíváme na to přidáním související data do tohoto objektu TableAdapter.


Klikněte na tlačítko Upřesnit možnosti chvíli trvat. Odsud můžete určíme, zda průvodce by měl také generovat insert, update a delete příkazy pro TableAdapter, jestli se má použít optimistické řízení souběžnosti a určuje, zda by měla být tabulka dat aktualizují po provedení vkládání a aktualizace. Ve výchozím nastavení je zaškrtnutá možnost příkazy Generovat Insert, Update a Delete. Ponechte zaškrtnuté políčko. Pro účely tohoto kurzu ponechte použijte možnosti optimistického řízení souběžnosti není zaškrtnuto.

Když máte uložené procedury automaticky vytvořené průvodcem TableAdapter, zdá se, že aktualizace dat možnost tabulky se ignoruje. Bez ohledu na to, jestli je toto políčko zaškrtnuté, výsledný insert, update a načíst uložené procedury just vložit nebo aktualizovat jenom záznam, jak uvidíme v kroku 3.


![Ponechte zaškrtnutá možnost příkazy Generovat Insert, Update a Delete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Obrázek 7**: ponechte zaškrtnutá možnost příkazy Generovat Insert, Update a Delete


> [!NOTE]
> Pokud je možnost použít optimistické řízení souběžnosti je zaškrtnuto, průvodce přidá dodatečné podmínky, které `WHERE` klauzuli, která brání data aktualizaci Pokud došlo ke změně v jiných polích. Vraťte se do [implementace optimistického řízení souběžnosti](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) kurz pro další informace o použití funkce řízení TableAdapter s integrovanou optimistického řízení souběžnosti.


Po zadání `SELECT` dotazování a potvrdí, že je zaškrtnuté políčko příkazy možnost Generovat Insert, Update a Delete, klikněte na tlačítko Další. Tato další obrazovka ukazuje obrázek 8, vyzve k zadání jména uložené procedury, které průvodce vytvoří pro výběr, vkládání, aktualizaci a odstraňování dat. Změnit tyto uložené procedury názvy `Products_Select`, `Products_Insert`, `Products_Update`, a `Products_Delete`.


[![Přejmenovat uložených procedur](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Obrázek 8**: Přejmenování uložené procedury ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Na T-SQL najdete v Průvodci vytvořením objektu TableAdapter pomocí vytvořit čtyři uložené procedury, klikněte na tlačítko Náhled skriptu SQL. Z dialogového okna Náhled skriptu SQL může uložit skript do souboru nebo ho zkopírujte do schránky.


![Zobrazit náhled SQL skriptu používaného pro generování uložených procedur](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Obrázek 9**: náhled SQL skriptu sloužící ke generování uložených procedur


Po pojmenování uložené procedury, klikněte na tlačítko vedle názvu třídy TableAdapter s odpovídající metody. Stejně jako při použití příkazů jazyka SQL ad-hoc můžeme vytvořit metody, které vyplnit existující objekt DataTable nebo vrátí nový. Můžeme také určit, zda TableAdapter by měl obsahovat vzor DB přímo pro vkládání, aktualizaci a odstraňování záznamů. Ponechat všechny tři zaškrtnutých políček, ale přejmenovat metodu DataTable pro vrácení `GetProducts` (jak je znázorněno na obrázku 10).


[![Název metody Fill a GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Obrázek 10**: název metody `Fill` a `GetProducts` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Kliknutím na tlačítko Další prohlédnout souhrnné informace o krocích, které průvodce provede. Dokončete průvodce kliknutím na tlačítko Dokončit. Po dokončení průvodce budete přesměrováni zpět do datové sady s návrháře, který by teď měl obsahovat `ProductsDataTable`.


[![V Návrháři datové sady s se zobrazí nově přidané ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Obrázek 11**: datové sady s návrhář zobrazí nově přidaná `ProductsDataTable` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Krok 3: Prozkoumání nově vytvořený uložených procedur

Průvodci vytvořením objektu TableAdapter použít v kroku 2 automaticky vytvořeny uložené procedury pro výběr, vkládání, aktualizaci a odstraňování dat. Těchto uložených procedurách můžete zobrazit nebo upravit pomocí sady Visual Studio přejděte do Průzkumníka serveru a procházení k podrobnostem složka databáze s uložené procedury. Jak ukazuje obrázek 12, databázi Northwind obsahuje čtyři nové uložené procedury: `Products_Delete`, `Products_Insert`, `Products_Select`, a `Products_Update`.


![Čtyři uložené procedury, které jsou vytvořené v kroku 2 najdete ve složce databáze s uloženými procedurami](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Obrázek 12**: čtyři uložené procedury, které jsou vytvořené v kroku 2 najdete ve složce databáze s uloženými procedurami


> [!NOTE]
> Pokud se nezobrazí v Průzkumníku serveru, přejděte do zobrazení nabídky a zvolte možnost Průzkumníka serveru. Pokud nevidíte týkající se produktu uložené procedury přidali v kroku 2, zkuste pravým tlačítkem na složku uložené procedury a volbou aktualizovat.


Lze zobrazit nebo upravit uložené procedury, klikněte dvakrát na jeho název v Průzkumníku serveru nebo, nebo klikněte pravým tlačítkem na uložené procedury a zvolte Otevřít. Obrázek 13 ukazuje `Products_Delete` uložené procedury, při otevření.


[![Uložené procedury lze otevřít a upravit z Visual Studia](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Obrázek 13**: uložené procedury se dají otevřít a upravit z v rámci sady Visual Studio ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


Obsah obou `Products_Delete` a `Products_Select` uložené procedury jsou poměrně jednoduché. `Products_Insert` a `Products_Update` uložené procedury, na druhé straně zaručujete blíže kontroly jak obě pracují `SELECT` příkaz po jejich `INSERT` a `UPDATE` příkazy. Například následující příkaz SQL tvoří `Products_Insert` uložené procedury:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Uložené procedury přijímá jako vstupní parametry `Products` sloupce, které byly vráceny `SELECT` dotaz zadaný v Průvodci vytvořením objektu TableAdapter s a tyto hodnoty se používají v `INSERT` příkazu. Následující `INSERT` příkaz, `SELECT` dotazu se používá k vrácení `Products` hodnoty ve sloupcích (včetně `ProductID`) nově přidaného záznamu. Tato aktualizace možnost je užitečná při přidání nového záznamu pomocí vzoru aktualizace služby Batch jako se automaticky aktualizuje nově přidaný `ProductRow` instance `ProductID` vlastností se automaticky navyšovaná hodnoty přiřazené v databázi.

Následující kód ukazuje tuto funkci. Obsahuje `ProductsTableAdapter` a `ProductsDataTable` vytvořené pro `NorthwindWithSprocs` typované datové sady. Přidání nového produktu do databáze tak, že vytvoříte `ProductsRow` instance, zadávání hodnot a volání TableAdapter s `Update` metodu `ProductsDataTable`. Interně, s TableAdapter `Update` metoda výčet `ProductsRow` instance v objektu DataTable předaným (v tomto příkladu je pouze – ten jsme právě přidali) a provede odpovídající vložení, aktualizace nebo odstranění příkazu. V takovém případě `Products_Insert` uloženou proceduru spustit, který přidá nový záznam do `Products` tabulky a vrátí podrobnosti o záznamu nově přidané. `ProductsRow` Instance s `ProductID` hodnota se pak aktualizuje. Po `Update` metoda dokončí, budeme má přístup k záznamům nově přidané s `ProductID` hodnotu prostřednictvím `ProductsRow` s `ProductID` vlastnost.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update` Podobně zahrnuje uložené procedury `SELECT` příkaz po jeho `UPDATE` příkazu.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Všimněte si, že tato uložená procedura obsahuje dva vstupní parametry pro `ProductID`: `@Original_ProductID` a `@ProductID`. Tato funkce umožňuje scénáře, ve kterém může být změněn primární klíč. Například v databázi zaměstnanců může každý zaměstnanec záznam použít číslo sociálního pojištění zaměstnance s jako svůj primární klíč. Chcete-li změnit existující zaměstnance s číslo sociálního pojištění, je nutné zadat nové číslo sociálního pojištění a původní. Pro `Products` tabulky, tato funkce není nutné, protože `ProductID` sloupec je `IDENTITY` sloupce a nikdy by měla být změněna. Ve skutečnosti `UPDATE` příkaz v `Products_Update` uloženou proceduru kódu t zahrnout `ProductID` sloupec v seznamu sloupců. Ano, zatímco `@Original_ProductID` se používá v `UPDATE` příkaz s `WHERE` klauzule je nadbytečný pro `Products` tabulky a může být nahrazen `@ProductID` parametru. Při úpravě s parametry uložené procedury je důležité, že jsou také aktualizované metody TableAdapter, které používají tuto uloženou proceduru.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Krok 4: Úprava uloženou proceduru s parametry a aktualizace komponenty TableAdapter

Protože `@Original_ProductID` parametrů je nadbytečný, umožňují s odeberte ho z `Products_Update` úplně uložené procedury. Otevřít `Products_Update` uložené procedury, odstranit `@Original_ProductID` parametr a v `WHERE` klauzuli `UPDATE` prohlášení, změnit název parametru použití z `@Original_ProductID` k `@ProductID`. Po provedení těchto změn, T-SQL v rámci uložené procedury by měl vypadat nějak takto:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Chcete-li tyto změny uložit do databáze, klikněte na ikonu Uložit na panelu nástrojů nebo stiskněte kombinaci kláves Ctrl + S. V tuto chvíli `Products_Update` neočekávají uložené procedury `@Original_ProductID` vstupní parametr, ale objektu TableAdapter je nakonfigurován k předání těchto parametrů. Zobrazí parametry TableAdapter bude odesílat do `Products_Update` uloženou proceduru výběru objektu TableAdapter v návrháři datových sad, že přejdete do okna Vlastnosti a kliknutím na symbol tří teček v `UpdateCommand` s `Parameters` kolekce. Otevře dialogové okno Editor kolekce parametrů znázorňuje obrázek 14.


![Seznamy parametrů Editor kolekce parametrů použitých předán Products_Update uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Obrázek 14**: předat parametry seznamy Editor kolekce parametrů použitých `Products_Update` uložené procedury


Tento parametr odsud můžete odebrat tak, že jednoduše vyberete `@Original_ProductID` parametr ze seznamu členů a kliknete na tlačítko Odebrat.

Alternativně můžete aktualizovat parametry používané pravým tlačítkem myši na TableAdapter v návrháři a zvolíte konfigurace pro všechny metody. Tím se otevře Průvodce konfigurací TableAdapter, výpis uložené procedury používané pro výběr, vkládání, aktualizace a odstranění, spolu s parametry uložené procedury by se měl zobrazit. Pokud kliknete na Aktualizovat rozevírací seznam uvidíte `Products_Update` uložené procedury očekává vstupní parametry, které teď už obsahuje `@Original_ProductID` (viz obrázek 15). Jednoduše klikněte na tlačítko Dokončit k automatické aktualizaci parametru kolekce používané TableAdapter.


[![Můžete také použít Průvodce konfigurací s TableAdapter aktualizovat jeho kolekce parametrů metody](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Obrázek 15**: můžete také použít s TableAdapter Průvodce konfigurací a aktualizujte jeho kolekce parametrů metody ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Krok 5: Přidání metody další TableAdapter

Jako znázorněno kroku 2 při vytváření nového objektu TableAdapter je snadné mít odpovídající uložené procedury, které jsou automaticky generovány. Totéž platí při přidávání další metody objektu typu TableAdapter. Pro znázornění je nechat s přidat `GetProductByProductID(productID)` metodu `ProductsTableAdapter` vytvořili v kroku 2. Tato metoda vezme jako vstup `ProductID` hodnotu a vrátí podrobnosti o zadaný produkt.

Začněte tím, že pravým tlačítkem myši na TableAdapter a zvolením přidat dotaz v místní nabídce.


![Přidat nový dotaz TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Obrázek 16**: Přidat nový dotaz TableAdapter


Tím spustíte Průvodce konfigurací dotazu TableAdapter, který nejprve zobrazí výzvu k zadání jak by měl TableAdapter přistupovat k databázi. Pokud chcete, aby vytvoří novou úložnou proceduru vytvořen, zvolte možnost vytvořit novou úložnou proceduru možnost a klikněte na tlačítko Další.


[![Zvolte možnost vytvořit novou úložnou proceduru možnost](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Obrázek 17**: Zvolte možnost vytvořit novou úložnou proceduru možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Na další obrazovce zobrazí výzva k identifikaci typu dotazu provést, ať už se vrátit sadu řádků nebo jednu skalární hodnotu nebo provést `UPDATE`, `INSERT`, nebo `DELETE` příkazu. Vzhledem k tomu, `GetProductByProductID(productID)` metoda se vrátí řádek, ponechte SELECT, který vrátí řádek možnost vybraná kliknu na další.


[![Zvolte, které vrátí řádek možnost](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Obrázek 18**: Zvolte, které vrátí řádek možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


Na další obrazovce se zobrazí na TableAdapter s hlavní dotaz, který právě uvádí název uložené procedury (`dbo.Products_Select`). Nahraďte názvem uložené procedury následujícím `SELECT` příkaz, který vrátí všechna pole produktu pro konkrétní produkt:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Nahraďte názvem uložené procedury výběrový dotaz](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Obrázek 19**: nahradit název uložené procedury s `SELECT` dotazu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


Na další obrazovce vás požádá, abyste název uložené procedury, která bude vytvořena. Zadejte název `Products_SelectByProductID` a klikněte na tlačítko Další.


[![Název nové Products_SelectByProductID uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Obrázek 20**: pojmenujte novou uloženou proceduru `Products_SelectByProductID` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Posledním krokem Průvodce umožňuje změnit způsob názvy generované také určit, jestli se má použít výplně vzor objektu DataTable, vrátit objekt DataTable vzoru, nebo obojí. Tato metoda nechat obě možnosti zaškrtnuté, ale přejmenovat metody k `FillByProductID` a `GetProductByProductID`. Klikněte na tlačítko Další k zobrazení souhrnu kroky Průvodce provede a potom klikněte na tlačítko Dokončit dokončete průvodce.


[![Přejmenovat metod TableAdapter s FillByProductID a GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Obrázek 21**: přejmenování metody třídy TableAdapter s `FillByProductID` a `GetProductByProductID` ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Po dokončení průvodce se má TableAdapter novou metodu k dispozici, `GetProductByProductID(productID)` , která při vyvolání, spustí `Products_SelectByProductID` uložené procedury, která byla právě vytvořena. Za chvíli chcete-li zobrazit tuto novou úložnou proceduru z Průzkumníka serveru při bližším pohledu na složku uložené procedury a otevřením `Products_SelectByProductID` (pokud ho nevidíte, klikněte pravým tlačítkem na složku uložené procedury a vyberte příkaz Aktualizovat).

Všimněte si, že `SelectByProductID` uložené procedury přijímá `@ProductID` jako vstupní parametr a spustí `SELECT` příkaz, který jsme zadali v průvodci.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Krok 6: Vytvoření třídy vrstvy obchodní logiky

V celé řadě kurzů jsme mají strived udržovat vrstvené architektury, ve kterém prezentační vrstvy provedli všechny její volání do vrstvy obchodní logiky (BLL). Aby bylo možné splnit toto rozhodnutí o návrhu, musíme nejprve vytvořit třídu BLL pro nová datová sada typu, než jsme přístup k datům produktů od prezentační vrstvy.

Vytvořte nový soubor třídy `ProductsBLLWithSprocs.cs` v `~/App_Code/BLL` složky a přidejte do ní následující kód:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Tato třída napodobuje `ProductsBLL` sémantiku z předchozích kurzů, ale používá třídu `ProductsTableAdapter` a `ProductsDataTable` objekty z `NorthwindWithSprocs` datové sady. Například místo `using NorthwindTableAdapters` příkaz na začátek souboru třídy jako `ProductsBLL` nemá, `ProductsBLLWithSprocs` třídy používá `using NorthwindWithSprocsTableAdapters`. Podobně `ProductsDataTable` a `ProductsRow` objekty používané v této třídě jsou s předponou `NorthwindWithSprocs` oboru názvů. `ProductsBLLWithSprocs` Třída poskytuje dva datové přístup k metodám, `GetProducts` a `GetProductByProductID`, a metody pro přidání, aktualizace a odstranění instance jednoho produktu.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Krok 7: Práce s`NorthwindWithSprocs`datové sady od prezentační vrstvy

V tuto chvíli jsme vytvořili vrstvy DAL, který používá uložené procedury pro přístup a úpravy podkladová data databáze. Základní BLL jsme také vytvořili pomocí metody pro načtení všech produktů nebo konkrétní produkt spolu s metody pro přidání, aktualizaci a odstraňování produktů. Zaokrouhlení v tomto kurzu, umožňují s vytvoření stránky ASP.NET, která používá BLL s `ProductsBLLWithSprocs` třídy pro zobrazení, aktualizaci a odstraňování záznamů.

Otevřít `NewSprocs.aspx` stránku `AdvancedDAL` složky a GridView přetáhněte z panelu nástrojů na Návrhář, jeho pojmenování `Products`. V prvku GridView s inteligentním zvolte a vytvořte jeho vazbu nového prvku ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLLWithSprocs` třídy, jak ukazuje obrázek 22.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Obrázek 22**: Konfigurace ObjectDataSource k použití `ProductsBLLWithSprocs` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


Nabízí dvě možnosti, rozevíracího seznamu vyberte kartě `GetProducts` a `GetProductByProductID`. Protože chceme zobrazit všechny produkty v prvku GridView, zvolte `GetProducts` metody. Rozevírací seznamy na kartách UPDATE, INSERT a DELETE mít pouze jednu metodu. Zajistěte, aby každá z těchto rozevíracích seznamech jeho odpovídající metodu vybrána a potom klikněte na tlačítko Dokončit.

Po dokončení Průvodce ObjectDataSource sady Visual Studio bude přidán BoundFields a třídě CheckBoxField do prvku GridView pro datová pole produktu. Zapněte GridView s integrovanou úpravy a odstraňování funkce zaškrtnutím možnosti Povolit úpravy a Povolit odstranění k dispozici v inteligentních značek.


[![Tato stránka obsahuje prvek GridView s úpravy a odstranění je povolená podpora](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Obrázek 23**: Tato stránka obsahuje prvek GridView s úpravy a odstranění povolená podpora ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Uložit jako jsme probírali v předchozích kurzech se po dokončení průvodce s ObjectDataSource sady Visual Studio `OldValuesParameterFormatString` vlastnost s původní\_{0}. Tato hodnota musí být nastavení bylo vráceno na výchozí hodnotu {0} v pořadí funkcí změny dat fungovala správně zadané parametry očekává pomocí metod v našich knihoven BLL. Proto je potřeba nastavit `OldValuesParameterFormatString` vlastnost {0} nebo úplně odeberte vlastnost z deklarativní syntaxe.

Po dokončení Průvodce nakonfigurovat zdroj dat, zapnutí úpravy a odstranění podpory v prvku GridView a vrácení prvku ObjectDataSource s `OldValuesParameterFormatString` vlastnost na svou výchozí hodnotu, stránka s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

V tuto chvíli jsme mohli pořádek prvku GridView přizpůsobením úpravy rozhraní zahrnout ověřování, máte `CategoryID` a `SupplierID` sloupce takto DropDownLists a tak dále. Přidáme potvrzení na straně klienta může také na tlačítku pro odstranění a neváhejte se čekat na k implementaci těchto rozšíření. Protože tato témata popsaná v předchozích kurzech, ale nebude probereme je znovu tady.

Bez ohledu na to, zda prvku GridView vylepšila nebo ne otestujte na stránku s základní funkce v prohlížeči. Jak ukazuje obrázek 24, na stránce uvedeny produkty v prvku GridView, která poskytuje jeden řádek úpravy a odstranění funkce.


[![Produkty lze zobrazit, upravit a odstranit z prvku GridView.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Obrázek 24**: The produktů je možné zobrazit, Upravovaný a odstraněných z prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Souhrn

Objekty TableAdapter v datové sadě zadán můžou k datům z databáze pomocí ad-hoc SQL nebo prostřednictvím uložené procedury. Při práci s uloženými procedurami, lze použít buď existující uložené procedury nebo Průvodci vytvořením objektu TableAdapter lze pokyn k vytvoření nových uložených procedur na základě `SELECT` dotazu. V tomto kurzu jsme vyzkoušeli jste automaticky vytváří uložené procedury.

Přitom má uložené procedury, které automaticky generované pomáhá šetřit čas, existují určité případy, kde úložná procedura vytvořila pomocí t Průvodce kódu bylo v souladu s co byste vytvořili jsme v naší vlastní. Jedním z příkladů je `Products_Update` uloženou proceduru, která očekává i `@Original_ProductID` a `@ProductID` vstupní parametry, i když `@Original_ProductID` parametrů je nadbytečný.

V mnoha případech úložné procedury již byla pravděpodobně vytvořena nebo může být vhodné sestavení je ručně tak, aby byl jemnější stupeň kontroly nad s příkazy uloženou proceduru. V obou případech by chcete dát pokyn TableAdapter používat existující uložené procedury pro její metody. Můžeme se zjistit, jak to provést v dalším kurzu.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vytváření a Správa uložených procedur](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Načítání dat skalární z uložené procedury](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server uložené procedury základy](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Uložené procedury: Přehled](http://www.sqlteam.com/item.asp?ItemID=563)
- [Zápis uložené procedury](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Geisenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
