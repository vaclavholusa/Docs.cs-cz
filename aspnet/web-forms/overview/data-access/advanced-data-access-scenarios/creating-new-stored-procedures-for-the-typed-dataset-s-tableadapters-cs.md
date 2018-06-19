---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Vytváření nových uložené procedury pro TableAdapters typové datové sady (C#) | Microsoft Docs
author: rick-anderson
description: V dřívějších kurzech jsme vytvořili příkazů SQL v našem kódu a předán příkazy k databázi a provést. Alternativní způsob se má používat s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 3baff73d63cbc96a1a9f2222d077035cdd9a68f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879293"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Vytváření nových uložené procedury pro TableAdapters typové datové sady (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) nebo [stáhnout PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> V dřívějších kurzech jsme vytvořili příkazů SQL v našem kódu a předán příkazy k databázi a provést. Alternativní způsob je pomocí uložených procedur, kde jsou příkazy SQL předem definovaných v databázi. V tomto kurzu jsme zjistěte, jak má průvodce nastavením TableAdapter generovat nové uložené procedury pro nás.


## <a name="introduction"></a>Úvod

Data přístup Layer (DAL) pro tyto kurzy používá typové datové sady. Jak je popsáno v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-cs.md) kurzu zadaný datové sady se skládají z silného typu DataTables a TableAdapters. DataTables představují logických entit v systému během TableAdapters rozhraní s základní databáze pro přístup k práci data. To zahrnuje naplnění DataTables s daty, provádění dotazů, které vrátí skalární data, a vkládání, aktualizaci a odstraňování záznamů z databáze.

Příkazy SQL, které jsou spuštěny TableAdapters může být buď příkazů SQL ad-hoc, jako například `SELECT columnList FROM TableName`, nebo uložené procedury. Prvků TableAdapters ve Naše architektura používat příkazy SQL ad hoc. Mnoho vývojáři a správci databází, ale upřednostňovat uložené procedury přes ad-hoc příkazů SQL z důvodů zabezpečení, udržovatelnosti a aktualizovatelnosti. Ostatní ardently přednost ad-hoc příkazů SQL pro jejich flexibilitě. Vlastní pracovního I upřednostnit uložené procedury přes ad-hoc příkazy SQL, ale rozhodli používat příkazy SQL ad-hoc ke zjednodušení starší kurzy.

Při definování TableAdapter či přidávání nových metod, Průvodce nastavením TableAdapter s je stejně jako snadno vytvořit nové uložené procedury nebo použijte existující uložené procedury, jako tomu je používat příkazy SQL ad hoc. V tomto kurzu podíváme, jak vám má průvodce nastavením TableAdapter s automaticky generovat uložené procedury. V dalším kurzu se podíváme na postup konfigurace metod TableAdapter s používat existující nebo ručně vytvořené uložené procedury.

> [!NOTE]
> Najdete v položkách blogu Rob Howard [nemáte procedury uložené pomocí položky ještě?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) a [Frans Bouma](https://weblogs.asp.net/fbouma/) položku blogu s [uložené procedury jsou chybný, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) pro živá diskuse o výhody a nevýhody uložené procedury a ad-hoc SQL.


## <a name="stored-procedure-basics"></a>Základy uložené procedury

Funkce jsou společné pro všechny programovací jazyky konstrukt. Funkce je kolekce příkazů, které jsou spouštěny, když je tato funkce volána. Funkce může přijmout vstupní parametry a volitelně může vrátit hodnotu. *[Uložené procedury](http://en.wikipedia.org/wiki/Stored_procedure)*  jsou konstrukce databáze, které sdílejí mnoho společného s funkcí v programovacích jazyků. Uložená procedura se skládá ze sady příkazů T-SQL, které jsou spouštěny při volání uložené procedury. Uložené procedury mohou použít nulu mnoho vstupní parametry a může vrátit skalárních hodnot, výstupní parametry, nebo, nejčastěji ze sady výsledků `SELECT` dotazy.

> [!NOTE]
> Uložené procedury jsou často označovány jako sprocs nebo aktualizace Service Pack.


Uložené procedury lze vytvořit pomocí [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) příkaz T-SQL. Například následující skriptu T-SQL vytvoří uložené procedury s názvem `GetProductsByCategoryID` , která má jeden parametr s názvem `@CategoryID` a vrátí `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` pole těchto sloupců `Products` tabulky, které mají odpovídající `CategoryID` hodnotu:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Po vytvoření tuto uloženou proceduru lze volat, použijte tuto syntaxi:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> V dalším kurzu vyzkoušíme vytváření uložené procedury pomocí prostředí Visual Studio IDE. V tomto kurzu ale přidáme umožníte Průvodce nastavením TableAdapter automaticky generovat uložené procedury pro nás.


Kromě jednoduše vracející data, se často uložené procedury používají k provedení více příkazů databáze v rámci oboru jedné transakci. Uložené procedury s názvem `DeleteCategory`, například může trvat `@CategoryID` parametr a provádět dva `DELETE` příkazy: první, z nich k odstranění souvisejících produktů a druhý odstraňování Zadaná kategorie. Více příkazů v rámci uložené procedury jsou *není* automaticky zabalené v rámci transakce. Další příkazy T-SQL musí být vydaný zajistit uložené procedury s, který více příkazů, které jsou považovány za atomická operace. Postup při zabalení uložené procedury s příkazy v oboru transakce v následných kurzu uvidíme.

Při použití uložených procedur v rámci architekturu, vyvolání metody Data Access Layer s konkrétní uložené procedury místo vydání příkazu SQL ad hoc. Toto místo je definován v architektuře aplikace s centralizuje umístění příkazy SQL spustit (na databázi). Taková centralizace pravděpodobně je snazší najít, analýzu a ladit dotazy a poskytuje mnohem lepší přehled o tom, jak a kde se používá databázi.

Další informace o uložené proceduře základy najdete na konci tohoto kurzu prostředky v části Další čtení.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Krok 1: Vytvoření pokročilé Data přístup vrstvy scénáře webových stránek

Než začneme naše diskuzi o vytváření DAL pomocí uložené procedury, umožní s nejprve vytváření stránek ASP.NET v našem webu projekt, který je nutné zadat v tomto a další kurzy několik chvíli trvat. Nejprve přidejte novou složku s názvem `AdvancedDAL`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Přidání stránky ASP.NET pro scénáře kurzy pokročilé Data přístup vrstvy](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Obrázek 1**: Přidání stránky ASP.NET pro scénáře kurzy pokročilé Data přístup vrstvy


V jiných složkách, jako `Default.aspx` v `AdvancedDAL` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Nakonec přidejte tyto stránek jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po práci s daty zpracovat v dávce `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně nyní obsahuje položky pro pokročilé scénáře kurzy DAL.


![Mapy webu nyní obsahuje položky pro kurzy scénáře pokročilé DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Obrázek 3**: mapy webu nyní obsahuje položky pro kurzy scénáře pokročilé DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Krok 2: Konfigurace TableAdapter pro vytváření nových uložené procedury

Chcete-li ukazují, vytvoření Data Access Layer místo příkazů SQL ad-hoc používá uložené procedury, umožňují s vytvořit nové typové datové sady v `~/App_Code/DAL` složku s názvem `NorthwindWithSprocs.xsd`. Vzhledem k tomu, že tento proces podrobně jsme již prošli v předchozí kurzy, jsme bude pokračovat rychle provede kroky v tomto poli. Pokud máte zablokuje nebo potřebujete další podrobné pokyny v vytváření a konfiguraci typové datové sady, přečtěte si téma zpět [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-cs.md) kurzu.

Do projektu přidejte kliknutím pravým tlačítkem na nová datová sada `DAL` složky, výběr přidat novou položku a výběrem šablony datovou sadu, jak je znázorněno na obrázku 4.


[![Přidat nové typové datové sady do projektu s názvem NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Obrázek 4**: Přidat novou datovou sadu zadali do projektu s názvem `NorthwindWithSprocs.xsd` ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Tato se vytvoří nová datová sada zadali, otevřete jeho Designer, vytvořte nové TableAdapter a spusťte Průvodce nastavením TableAdapter. Prvním krokem Průvodce nastavením TableAdapter s dotazem, nám vyberte databázi pro práci s. Připojovací řetězec k databázi Northwind by měl být uvedený v rozevíracím seznamu. Tuto možnost vyberte a klikněte na tlačítko Další.

Prostřednictvím této obrazovky další jsme můžete zvolit, jak TableAdapter by měl přístup k databázi. V předchozích kurzy jsme vybrali první možnosti, pomocí SQL dotazů. V tomto kurzu vyberte druhou možnost, vytvořit nové uložené procedury a klikněte na tlačítko Další.


[![Vyzvat TableAdpater k vytvoření nové uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Obrázek 5**: dá pokyn TableAdpater k vytvoření nové uložené postupy ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Stejně jako s použitím příkazů SQL ad-hoc, v dalším kroku jsme vyzváni k zadání `SELECT` příkaz pro hlavní dotazu TableAdapter s. Ale místo použití `SELECT` příkaz v zadaném přímo provést dotaz ad-hoc, vytvoří Průvodce nastavením TableAdapter s uložené procedury, která obsahuje toto `SELECT` dotazu.

Použijte následující `SELECT` dotazu pro tuto TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Zadejte dotaz SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Obrázek 6**: Zadejte `SELECT` dotazu ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> Výše uvedené dotazu se mírně liší od hlavní dotazování `ProductsTableAdapter` v `Northwind` typové datové sady. Odvolat, který `ProductsTableAdapter` v `Northwind` zadali datová sada obsahuje dvě korelační poddotazy tak, aby název kategorie a název společnosti pro každou kategorii produktu s a dodavatele. V nadcházející akce [aktualizace TableAdapter na použití spojí](updating-the-tableadapter-to-use-joins-cs.md) se podíváme na přidání tohoto kurzu související data se tento TableAdapter.


Klikněte na tlačítko Upřesnit možnosti chvíli trvat. Zde jsme můžete určit, zda průvodce by měl také generovat insert, update a delete příkazy TableAdapter, jestli se má použít optimistickou metodu souběžného a jestli je třeba aktualizovat tabulky dat po vložení a aktualizace. Ve výchozím nastavení je zaškrtnutá možnost příkazy Generovat Insert, Update a Delete. Ponechte zaškrtnuté. V tomto kurzu nechejte použití možností optimistickou metodu souběžného nezaškrtnuté.

Uložené procedury automaticky vytvořený pomocí Průvodce nastavením TableAdapter s, zdá, že obnovení možnost tabulky dat je ignorováno. Bez ohledu na to, jestli je toto políčko zaškrtnuté, výsledná insert a update načíst uložené procedury právě vložit nebo aktualizovat právě záznam, protože jsme se zobrazí v kroku 3.


![Nechte generovat Insert, Update a Delete příkazy zaškrtnutá možnost](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Obrázek 7**: nechat vygenerovat Insert, Update a Delete příkazy zaškrtnutá možnost


> [!NOTE]
> Pokud je možnost Použít optimistickou metodu souběžného zaškrtnuto, průvodce přidá dodatečné podmínky, které `WHERE` klauzuli, která data zabránit aktualizaci Pokud byly nějaké změny v ostatních polích. Odkazovat zpět [implementace optimistickou metodu souběžného](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) kurz pro další informace o použití funkce TableAdapter s integrovanou optimistickou metodu souběžného řízení.


Po zadání `SELECT` dotazování a potvrzení, že je zaškrtnuté možnosti příkazy Generovat Insert, Update a Delete, klikněte na tlačítko Další. Tato další obrazovka ukazuje obrázek 8, vyzve k zadání názvy uložené procedury, které má průvodce vytvořit pro výběr, vkládání, aktualizaci a odstraňování data. Změna tyto uložené procedury názvy `Products_Select`, `Products_Insert`, `Products_Update`, a `Products_Delete`.


[![Přejmenování uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Obrázek 8**: Přejmenovat uložené procedury ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Průvodce nastavením TableAdapter použije k vytvoření čtyři uložené procedury T-SQL najdete kliknutím na tlačítko skript SQL Preview. Z dialogového okna Skript SQL Preview můžete uložit do souboru skriptu nebo ho zkopírujte do schránky.


![Náhled skript SQL sloužící ke generování uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Obrázek 9**: Preview skript SQL sloužící ke generování uložené procedury


Po pojmenování uložené procedury, klikněte na tlačítko Další název TableAdapter s odpovídající metody. Stejně jako při použití ad-hoc příkazy SQL vytvoříme metody, které vyplnění existující DataTable, nebo může vracet novou. Jsme můžete také určit, zda TableAdapter by měla obsahovat DB-Direct vzor pro vložení, aktualizaci a odstraňování záznamů. Ponechejte všechna tři políčka zaškrtnuto, ale přejmenovat návratový DataTable metodu pro `GetProducts` (jak je znázorněno na obrázku 10).


[![Název metody výplň a GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Obrázek 10**: název metody `Fill` a `GetProducts` ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Klikněte na tlačítko Další se zobrazí souhrn kroky, které průvodce provede. Dokončete průvodce kliknutím na tlačítko Dokončit. Po dokončení průvodce se vrátíte na datovou sadu s Designer, která by měla obsahovat nyní `ProductsDataTable`.


[![Návrháři DataSet s ukazuje nově přidané ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Obrázek 11**: datové sady s Designer zobrazí nově přidaná `ProductsDataTable` ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Krok 3: Prozkoumání nově vytvořený uložené procedury

Průvodce nastavením TableAdapter použít v kroku 2 automaticky vytvořit uložené procedury pro výběr, vkládání, aktualizaci a odstraňování data. Tyto uložené procedury můžete zobrazit nebo upravit pomocí sady Visual Studio přejděte do Průzkumníka serveru a procházení k podrobnostem do složky, uložené procedury s databáze. Jak ukazuje obrázek 12, databázi Northwind obsahuje čtyři nové uložené procedury: `Products_Delete`, `Products_Insert`, `Products_Select`, a `Products_Update`.


![Čtyři uložené procedury vytvořili v kroku 2 naleznete ve složce uložené procedury databáze s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Obrázek 12**: čtyři uložené procedury vytvořili v kroku 2 naleznete ve složce uložené procedury databáze s


> [!NOTE]
> Pokud nevidíte Průzkumníka serveru, přejděte do nabídky zobrazení a vyberte možnost Průzkumníka serveru. Pokud nevidíte týkající se produktu uložené procedury přidat z kroku 2, zkuste pravým tlačítkem na složku uložené procedury a výběr aktualizovat.


Chcete-li zobrazit nebo upravit uložené procedury, klikněte dvakrát na jeho název v Průzkumníku serveru nebo, nebo klikněte pravým tlačítkem na uložené procedury a zvolte otevřete. Obrázek 13 ukazuje `Products_Delete` uložené procedury, při otevření.


[![Uložené procedury lze otevřít a změnit z Visual Studia](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Obrázek 13**: uložené procedury můžete otevřít a změnit z v sadě Visual Studio ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


Obsah obou `Products_Delete` a `Products_Select` uložené procedury jsou poměrně jednoduché. `Products_Insert` a `Products_Update` uložené procedury na druhé straně vlastní blíže kontroly obě provádět `SELECT` příkaz po jejich `INSERT` a `UPDATE` příkazy. Například následující příkaz SQL tvoří `Products_Insert` uložené procedury:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Uložená procedura akceptuje jako vstupní parametry `Products` sloupce, které byly vráceny `SELECT` dotaz zadaný v Průvodci TableAdapter s a tyto hodnoty se používají v `INSERT` příkaz. Následující `INSERT` příkaz, `SELECT` dotazu se používá k vrácení `Products` hodnoty ve sloupcích (včetně `ProductID`) nově přidaného záznamu. Tato aktualizace funkce je užitečná při přidávání nového záznamu pomocí vzoru dávková aktualizace jako se automaticky aktualizuje nově přidaný `ProductRow` instance `ProductID` vlastnosti hodnotami automaticky zvýší přiřadila databáze.

Následující kód ukazuje tuto funkci. Obsahuje `ProductsTableAdapter` a `ProductsDataTable` pro vytvořit `NorthwindWithSprocs` typové datové sady. Nového produktu přidá do databáze se při vytváření `ProductsRow` instance, zadávání jeho hodnoty a volání TableAdapter s `Update` předávání v případě metody `ProductsDataTable`. Interně TableAdapter s `Update` metoda výčet `ProductsRow` instance v DataTable předané (v tomto příkladu je pouze – ta, kterou jsme právě přidali) a provede odpovídající vložit, aktualizovat nebo odstranit příkaz. V takovém případě `Products_Insert` uloženou proceduru spustit, který přidá nový záznam pro `Products` tabulce a vrátí podrobnosti o nově přidané záznamu. `ProductsRow` Instance s `ProductID` hodnota je pak aktualizovat. Po `Update` metoda byl dokončen, jsme má přístup k záznamům nově přidané s `ProductID` hodnotu prostřednictvím `ProductsRow` s `ProductID` vlastnost.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update` Podobně zahrnuje uložené procedury `SELECT` příkaz po jeho `UPDATE` příkaz.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Všimněte si, že to uložené procedury obsahuje dvě vstupní parametry pro `ProductID`: `@Original_ProductID` a `@ProductID`. Tato funkce umožňuje pro scénáře, kde může být změněn primární klíč. Například v databáze, může každý záznam zaměstnance použít číslo sociálního pojištění zaměstnance s jako jejich primární klíč. Chcete-li změnit existující zaměstnanec s číslo sociálního pojištění, je nutné zadat nové číslo sociálního pojištění a původní jeden. Pro `Products` tabulky, tato funkce není potřeba, protože `ProductID` sloupec je `IDENTITY` sloupce a nikdy by mělo být změněno. Ve skutečnosti `UPDATE` příkaz v `Products_Update` t nemá uložené procedury patří `ProductID` sloupec ve svém seznamu sloupců. Ano, při `@Original_ProductID` se používá v `UPDATE` příkaz s `WHERE` klauzule, je pro nadbytečné `Products` tabulky a může být nahrazen `@ProductID` parametr. Při úpravě s parametry uložené procedury je důležité také aktualizovat TableAdapter metodu nebo metody, které používají tuto uloženou proceduru.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Krok 4: Úprava s parametry uložené procedury a aktualizace TableAdapter

Vzhledem k tomu `@Original_ProductID` parametr je nadbytečné, umožňují s odeberte ji z `Products_Update` zcela uložené procedury. Otevřete `Products_Update` uložené procedury, odstranit `@Original_ProductID` parametr a v `WHERE` klauzuli `UPDATE` prohlášení, změnit název parametru použití z `@Original_ProductID` k `@ProductID`. Po provedení těchto změn, T-SQL v rámci uložené procedury by měl vypadat následovně:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Chcete-li tyto změny uložit do databáze, klikněte na ikonu Uložit na panelu nástrojů nebo stiskněte kombinaci kláves Ctrl + S. V tomto okamžiku `Products_Update` uložené procedury neočekává `@Original_ProductID` vstupní parametr, ale TableAdapter nakonfigurovaný tak, aby předat takový parametr. Zobrazí parametry TableAdapter odešle `Products_Update` uložené procedury výběrem TableAdapter v Návrháři DataSet, přejdete do okna vlastností a kliknutím na symbol tří teček v `UpdateCommand` s `Parameters` kolekce. Otevře dialogové okno Editor kolekce parametrů ukazuje obrázek 14.


![Editor kolekce parametrů seznamy použité parametry předaný Products_Update uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Obrázek 14**: seznamy Editor kolekce parametrů použité parametry předaný `Products_Update` uložené procedury


Tento parametr můžete odebrat z tohoto místa jednoduše výběrem `@Original_ProductID` parametr ze seznamu členů a kliknutím na tlačítko Odebrat.

Alternativně můžete obnovit použité pro všechny metody pravým tlačítkem myši na TableAdapter v návrháři a zvolením konfigurovat parametry. Tím se otevře Průvodce konfigurací TableAdapter, výpis uložené procedury používané pro výběr, vkládání, aktualizaci, a odstranění, spolu s parametry uložené procedury očekávat. Pokud kliknete na aktualizace rozevíracím seznamu se zobrazí `Products_Update` uložené procedury očekávána vstupní parametry, které teď už obsahuje `@Original_ProductID` (viz obrázek 15). Jednoduše kliknutím na tlačítko Dokončit automaticky aktualizovat kolekci parametrů používaných TableAdapter.


[![Můžete také použít Průvodce nastavením TableAdapter s konfigurací aktualizujte jeho kolekce parametru metody](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Obrázek 15**: můžete případně použít TableAdapter s Průvodce konfigurací a obnovit její metody parametr kolekcí ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Krok 5: Přidání další TableAdapter metod

Jako zobrazené kroku 2 při vytváření nové TableAdapter je snadné mít odpovídající uložené procedury, které jsou automaticky generovány. Totéž platí při přidávání dalších metod TableAdapter. Pro znázornění je umožnit s přidat `GetProductByProductID(productID)` metodu `ProductsTableAdapter` vytvořili v kroku 2. Tato metoda bude trvat jako vstup `ProductID` hodnotu a vrátí podrobnosti o zadaného produktu.

Start pravým tlačítkem myši na TableAdapter a zvolením přidat dotazu v místní nabídce.


![Přidat nový dotaz TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Obrázek 16**: přidejte nový dotaz do TableAdapter


Tím se spustí Průvodce konfigurací dotazu TableAdapter, nejprve vyzve k zadání jak TableAdapter by měl přístup k databázi. Chcete-li vytvořit nové uložené procedury, zvolte vytvořením novou možnost uložené procedury a klikněte na tlačítko Další.


[![Zvolte vytvořením nové uložené procedury možnost](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Obrázek 17**: Zvolte vytvořením nové uložené procedury možnost ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Na další obrazovce zobrazí výzva k identifikaci typ dotazu provést, ať už se vrátí sadu řádků nebo jednu skalární hodnotu nebo provést `UPDATE`, `INSERT`, nebo `DELETE` příkaz. Vzhledem k tomu `GetProductByProductID(productID)` bude metoda vrátí řádek, nechte SELECT, který vrátí řádek možnost vybrána a stiskněte tlačítko Další.


[![Zvolte SELECT, který vrátí řádek možnost](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Obrázek 18**: Zvolte SELECT, který vrátí řádek možnost ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


Na další obrazovce zobrazí TableAdapter s hlavní dotaz, který právě zobrazuje název uložené procedury (`dbo.Products_Select`). Název uložené procedury nahraďte následujícím `SELECT` příkaz, který vrací všechna pole produktu pro zadaný produkt:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Nahraďte název uložené procedury dotaz SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Obrázek 19**: nahraďte název uložené procedury s `SELECT` dotazu ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


Na další obrazovce požádá o název uložené procedury, která bude vytvořena. Zadejte název `Products_SelectByProductID` a klikněte na tlačítko Další.


[![Název nové Products_SelectByProductID uložené procedury](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Obrázek 20**: název nové uloženou proceduru `Products_SelectByProductID` ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Poslední krok průvodce umožňuje změnit metodu názvy generované a také označuje, zda chcete použít výplň DataTable vzor, vraťte se DataTable vzor, nebo obojí. Tuto metodu, nechte obě možnosti zaškrtnuto, ale přejmenovat metody k `FillByProductID` a `GetProductByProductID`. Kliknutím na tlačítko Další zobrazení souhrnu kroků Průvodce provede a pak klikněte na tlačítko Dokončit dokončete průvodce.


[![Přejmenujte metod TableAdapter s FillByProductID a GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Obrázek 21**: Přejmenovat TableAdapter s metody k `FillByProductID` a `GetProductByProductID` ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Po dokončení průvodce, TableAdapter má k dispozici nová metoda `GetProductByProductID(productID)` , při vyvolání, spustí `Products_SelectByProductID` uložené procedury, kterou jste právě vytvořili. Za chvíli zobrazit tuto novou uloženou proceduru z Průzkumníka serveru procházení do složky, uložené procedury a otevírání `Products_SelectByProductID` (pokud ho nevidíte, klikněte pravým tlačítkem na složku, uložené procedury a zvolte aktualizace).

Všimněte si, že `SelectByProductID` uložené procedury trvá `@ProductID` jako vstupní parametr a spouští `SELECT` příkaz, který jsme zadali v průvodci.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Krok 6: Vytvoření třídy vrstvu obchodní logiky

V průběhu kurzu budeme mít strived udržovat vrstveného architekturu, ve kterém prezentační vrstvy provedeny všechny jeho volání do vrstvy obchodní logiky (BLL). Aby bylo možné řídit tomuto rozhodnutí o návrhu, musíme nejprve vytvořit třídu BLL pro nová datová sada zadali jsme mohli dostat k produktu data od prezentační vrstvy.

Vytvořte soubor novou třídu s názvem `ProductsBLLWithSprocs.cs` v `~/App_Code/BLL` složky a přidejte do ní následující kód:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Tato třída napodobuje `ProductsBLL` třídy sémantiku z dřívějších kurzy, ale používá `ProductsTableAdapter` a `ProductsDataTable` objekty z `NorthwindWithSprocs` datovou sadu. Například místo `using NorthwindTableAdapters` příkaz na začátku souboru třídy jako `ProductsBLL` nemá, `ProductsBLLWithSprocs` třídy používá `using NorthwindWithSprocsTableAdapters`. Podobně `ProductsDataTable` a `ProductsRow` objekty použité v této třídě jsou předponou `NorthwindWithSprocs` oboru názvů. `ProductsBLLWithSprocs` Třída poskytuje metody pro přístup k datům dvě `GetProducts` a `GetProductByProductID`, a metody pro přidání, aktualizace a odstranění instance jednoho produktu.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Krok 7: Práce s`NorthwindWithSprocs`datovou sadu od prezentační vrstvy

V tuto chvíli jsme vytvořili DAL, pomocí uložené procedury, která přístup a měnit základní data databáze. Elementární BLL jsme také jste vytvořili pomocí metod k načtení všech produktů nebo konkrétní produkt společně s metody pro přidání, aktualizaci a odstraňování produkty. Zaokrouhlení v tomto kurzu, umožňují s vytvořit stránku ASP.NET, která používá BLL s `ProductsBLLWithSprocs` třídu pro zobrazení, aktualizace a odstranění záznamů.

Otevřete `NewSprocs.aspx` stránku `AdvancedDAL` složku a přetáhněte GridView z panelu nástrojů na návrháře pojmenování ho `Products`. Z GridView s inteligentním vyberte pro vytvoření vazby na nové ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLLWithSprocs` třídy, jak ukazuje obrázek 22.


[![Konfigurace ObjectDataSource použití třídy ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Obrázek 22**: Konfigurace ObjectDataSource pro použití `ProductsBLLWithSprocs` – třída ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


Rozevíracím seznamu v kartě vyberte má dvě možnosti, `GetProducts` a `GetProductByProductID`. Vzhledem k tomu, že chceme zobrazit všechny produkty v GridView, vyberte `GetProducts` metoda. Rozevírací seznamy na kartách UPDATE, INSERT a DELETE mít pouze jednu metodu. Zajistěte, aby každý z těchto rozevíracích seznamech jeho odpovídající metodu vybrané a pak klikněte na tlačítko Dokončit.

Po dokončení Průvodce ObjectDataSource přidá Visual Studio BoundFields a třídy CheckBoxField do GridView pro produkt datová pole. Zapněte GridView s integrovanou úpravy a odstraňování funkce kontrolou povolit úpravy a Povolit odstraňování možnosti v inteligentní značky.


[![Tato stránka obsahuje GridView s úpravy a odstraňování povolenou podporou mezipaměti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Obrázek 23**: Tato stránka obsahuje GridView s úpravy a odstraňování povolena podpora ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Jako jsme sunout popsané v předchozí kurzy, po dokončení průvodce s ObjectDataSource sady Visual Studio `OldValuesParameterFormatString` vlastnost, která má původní\_{0}. To je potřeba se vrátí zpátky na jeho výchozí hodnotu {0} v pořadí pro data úpravy funkce fungovat správně zadané parametry očekávanou metody v našem BLL. Proto nezapomeňte nastavit `OldValuesParameterFormatString` vlastnost {0} nebo úplně odeberte vlastnost z deklarativní syntaxi.

Po dokončení průvodce Konfigurace zdroje dat, zapnout úpravy a odstraňování podpory v GridView a vrácení ObjectDataSource s `OldValuesParameterFormatString` vlastnost na výchozí hodnotu, stránku s deklarativní by měl vypadat takto:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

V tuto chvíli jsme může přehledné až GridView přizpůsobením úpravy rozhraní, které chcete zahrnout ověřování, má `CategoryID` a `SupplierID` sloupce vykreslují jako DropDownLists a tak dále. Přidáme potvrzení na straně klienta může také na tlačítko odstranit, a I doporučujeme trvat dobu implementace těchto vylepšení. Vzhledem k tomu, že tato témata popsaná v předchozích kurzy, ale nebude nabídneme je znovu sem.

Bez ohledu na to, jestli je zvýšit GridView, nebo ne otestování stránky s klíčových funkcí v prohlížeči. Jak ukazuje obrázek 24, na stránce uvedeny produkty v GridView, která poskytuje na řádek úpravy a odstraňování možnosti.


[![Produkty můžete prohlížet, upravovat a odstranění z GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Obrázek 24**: The produkty lze zobrazit, Upravovaný a odstraněných z GridView ([Kliknutím zobrazit obrázek v plné velikosti](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Souhrn

Prvků TableAdapters ve typové datové sady můžete přistupovat k datům z databáze pomocí příkazů SQL ad-hoc nebo prostřednictvím uložené procedury. Při práci s uloženými procedurami, lze použít buď existující uložené procedury nebo Průvodce nastavením TableAdapter můžete pokyn k vytvoření nových uložené procedury na základě `SELECT` dotazu. V tomto kurzu jsme prozkoumali postup mít uložené procedury automaticky vytvoří pro nás.

Při s uložené procedury, které automaticky generovaný pomáhá ušetřit čas, jsou určitých případech, kde jsou uložené procedury vytvořené t nemá průvodce zarovnané s co by byla vytvořena v našem vlastní. Jedním z příkladů je `Products_Update` uložené procedury, která očekává obě `@Original_ProductID` a `@ProductID` vstupní parametry, i když `@Original_ProductID` parametr bylo nadbytečné.

V mnoha scénářích uložené procedury již byla pravděpodobně vytvořena nebo chceme sestavení je ručně tak, aby byl jemnějšího stupeň kontroly nad uložené procedury s příkazy. V obou případech by chceme pokyn TableAdapter použití existující uložené procedury pro její metody. Vidíme jak to provést v dalším kurzu.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vytvoření a údržbu uložené procedury](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Načítání skalární dat z uložené procedury](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Systému SQL Server uložené procedury základy](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Uložené procedury: Přehled](http://www.sqlteam.com/item.asp?ItemID=563)
- [Zápis uložené procedury](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Geisenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
