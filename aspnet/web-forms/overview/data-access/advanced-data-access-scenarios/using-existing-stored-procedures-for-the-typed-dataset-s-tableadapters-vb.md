---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Použití stávajících uložených procedur komponentami TableAdapter typové datové sady (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozím kurzu jsme zjistili, jak použít Průvodce vytvořením objektu TableAdapter na nové úložné procedury. V tomto kurzu jsme dozvíte, jak stejný TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a478b52387a6bbf726872978a0ad4a83c22c9911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382893"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Použití stávajících uložených procedur komponentami TableAdapter typové datové sady (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) nebo [stahovat PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> V předchozím kurzu jsme zjistili, jak použít Průvodce vytvořením objektu TableAdapter na nové úložné procedury. V tomto kurzu jsme dozvíte, jak pracovat stejným Průvodce TableAdapter s existující uložené procedury. Můžeme také zjistěte, jak ručně přidat nové uložené procedury do databáze.


## <a name="introduction"></a>Úvod

V [předchozím kurzu](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) jsme viděli, jak typované datové sady s objekty TableAdapter je možné nakonfigurovat pouze pomocí uložených procedur na SQL příkazy dat než ad-hoc přístup. Konkrétně jsme se zaměřili na tom, jak má TableAdapter průvodce automaticky vytvoří těchto uložených procedurách. Pokud přenášíte starší verze aplikace pro technologii ASP.NET 2.0 nebo při vytváření na webu technologie ASP.NET 2.0 kolem existující datový model, je pravděpodobné, že databáze již obsahuje uložené procedury, které potřebujeme. Také budete chtít vytvořit uložené procedury, ručně nebo pomocí některé nástroje, než Průvodce TableAdapter, který automaticky vygeneruje uložené procedury.

V tomto kurzu se podíváme na tom, jak konfigurovat TableAdapter používat existující uložené procedury. Vzhledem k tomu, že databáze Northwind obsahuje jenom malou sadu předdefinovaných uložené procedury, také podíváme na kroky potřebné pro ruční přidání nové uložené procedury do databáze pomocí prostředí Visual Studio. Začínáme s let!

> [!NOTE]
> V [zabalení úprav databáze do transakce](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu jsme přidali metody do třídy TableAdapter pro podporu transakcí (`BeginTransaction`, `CommitTransaction`, a tak dále). Alternativně je možné spravovat transakce v rámci uložené procedury, která nevyžaduje žádné úpravy kódu vrstvy přístupu k datům. V tomto kurzu se podíváme příkazů T-SQL ke spuštění příkazů s uloženou proceduru v rámci oboru transakce.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Krok 1: Přidání uložených procedur k databázi Northwind

Visual Studio umožňuje snadno přidat nové uložené procedury do databáze. Umožňují s přidat novou úložnou proceduru k databázi Northwind, která vrací všechny sloupce z `Products` tabulky pro ty, které mají určitý `CategoryID` hodnotu. Z okna Průzkumníka serveru rozbalte databázi Northwind, aby jeho složky - databázových diagramů, tabulek, zobrazení a tak dále - zobrazí. Jak jsme viděli v předchozím kurzu, uložené procedury složka obsahuje databáze s existující uložené procedury. Pokud chcete přidat novou úložnou proceduru, jednoduše klikněte pravým tlačítkem na složku uložené procedury a zvolte možnost Přidat novou uloženou proceduru v místní nabídce.


[![Klikněte pravým tlačítkem na složku uložené procedury a přidejte novou úložnou proceduru](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Obrázek 1**: klikněte pravým tlačítkem na složku uložené procedury a přidat novou uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Jak ukazuje obrázek 1, výběrem možnosti Přidat novou uloženou proceduru otevře okno skriptu v sadě Visual Studio s obrysem skriptu SQL, které jsou potřeba k vytvoření uložené procedury. To je náš úloh podpořili tento skript a spustit ho v tomto okamžiku uloženou proceduru se přidají do databáze.

Zadejte následující skript:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Tento skript při spuštění, přidá novou úložnou proceduru k databázi Northwind s názvem `Products_SelectByCategoryID`. Tuto uloženou proceduru přijímá jeden vstupní parametr (`@CategoryID`, typu `int`) a vrátí všechna pole pro tyto produkty k odpovídajícímu `CategoryID` hodnotu.

Chcete-li to provést `CREATE PROCEDURE` skriptu a přidat uložené procedury do databáze, klikněte na ikonu Uložit na panelu nástrojů nebo stiskněte kombinaci kláves Ctrl + S. Až to uděláte, aktualizuje složku uložené procedury, zobrazující nově vytvořený uložené procedury. Skript v okně se navíc změní subtlety z `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` k `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Přidá novou úložnou proceduru do databáze, zatímco `ALTER PROCEDURE` aktualizuje nějakou existující. Od začátku skriptu se změnila na `ALTER PROCEDURE`, změna uložených procedur vstupní parametry nebo příkazy SQL, a kliknutím na ikonu uložení se aktualizovat uložené procedury s těmito změnami.

Obrázek 2 ukazuje Visual Studio po `Products_SelectByCategoryID` uložená procedura byla uložena.


[![Uložená procedura Products_SelectByCategoryID byla přidána do databáze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Obrázek 2**: The uložená procedura `Products_SelectByCategoryID` byla přidána do databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Krok 2: Konfigurace TableAdapter používat stávající úložnou proceduru

Teď, když `Products_SelectByCategoryID` uložené procedury byla přidána do databáze, můžeme nakonfigurovat naše vrstvy přístupu k datům při jedné z jeho metod vyvolání používat tuto uloženou proceduru. Zejména, přidáme `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` metodu `ProductsTableAdapter` v `NorthwindWithSprocs` typová, která volá `Products_SelectByCategoryID` uložené procedury, které jsme právě vytvořili.

Začněte otevřením `NorthwindWithSprocs` datové sady. Klikněte pravým tlačítkem na `ProductsTableAdapter` a zvolte Přidat dotaz spustíte Průvodce konfigurací dotazu TableAdapter. V [předchozím kurzu](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) jsme se rozhodli mají TableAdapter vytvořit novou úložnou proceduru pro nás. Pro účely tohoto kurzu, ale chceme propojí nové metody třídy TableAdapter ke stávající `Products_SelectByCategoryID` uložené procedury. Proto možnost použití existující uložené procedury z prvního kroku průvodce s a pak klikněte na tlačítko Další.


[![Zvolte možnost použít existující uložené procedury možnost](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Obrázek 3**: Zvolte možnost použít existující uložené procedury možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Na tomto obrázku poskytuje že rozevíracího seznamu naplní databázi s uložené procedury. Výběr uložené procedury jsou uvedeny na levé straně a datová pole, která vrátí (pokud existuje) na pravé straně její vstupní parametry. Zvolte `Products_SelectByCategoryID` uloženou proceduru ze seznamu a klikněte na tlačítko Další.


[![Vyberte Products_SelectByCategoryID uložené procedury](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Obrázek 4**: výběr `Products_SelectByCategoryID` uloženou proceduru ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Na další obrazovce zeptá nám jaký druh dat je vrácený uložené procedury a naše odpověď určí typ vrácený metodou s TableAdapter. Například pokud Udáváme, že se vrátí tabulková data, metoda vrátí `ProductsDataTable` instanci naplněnou záznamů vrácených uložené procedury. Naopak pokud Udáváme, že tuto uloženou proceduru vrací jedinou hodnotu objektu TableAdapter se vrátí `Object` , která je přiřazena hodnota v prvním sloupci první záznam, vrátí uloženou proceduru.

Vzhledem k tomu, `Products_SelectByCategoryID` uložené procedury jsou vráceny všechny produkty, které patří do určité kategorie, zvolte první odpověď – tabulkových dat – a klikněte na tlačítko Další.


[![Označuje, že bude procedura vracet tabulková Data](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Obrázek 5**: označuje, že uložené procedury vrátí tabulková Data ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Už jen zbývá k označení metody vzorů, které za nímž následuje názvy pro tyto metody. Ponechte obou výplně DataTable a vrátit DataTable možnosti zaškrtnuté, ale přejmenovat metody k `FillByCategoryID` a `GetProductsByCategoryID`. Klikněte na další Zkontrolujte souhrn úlohy, které průvodce provede. Pokud vše vypadá v pořádku, klikněte na tlačítko Dokončit.


[![Název metody FillByCategoryID a GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Obrázek 6**: název metody `FillByCategoryID` a `GetProductsByCategoryID` ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Metody TableAdapter, který jsme právě vytvořili, `FillByCategoryID` a `GetProductsByCategoryID`, očekávat vstupní parametr typu `Integer`. Tato hodnota vstupního parametru je předán do uložené procedury prostřednictvím jeho `@CategoryID` parametru. Pokud změníte `Products_SelectByCategory` parametry uložené procedury s, bude nutné také aktualizovat parametry pro tyto metody třídy TableAdapter. Jak je popsáno v [předchozí kurz o službě](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), to můžete udělat v jednom ze dvou způsobů: ruční přidáním nebo odebráním parametrů z kolekce parametrů nebo opětovným spuštěním Průvodce TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Krok 3: Přidání`GetProductsByCategoryID(categoryID)`metodu BLL

S `GetProductsByCategoryID` metoda DAL kompletní, dalším krokem je poskytnout přístup k této metodě v vrstvy obchodní logiky. Otevřít `ProductsBLLWithSprocs` třídy soubor a přidejte následující metodu:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Tato metoda BLL jednoduše vrací `ProductsDataTable` vrácená z `ProductsTableAdapter` s `GetProductsByCategoryID` metody. `DataObjectMethodAttribute` Atribut poskytuje metadata používá Průvodce konfigurace zdroje dat s ObjectDataSource. Konkrétně tato metoda se zobrazí v rozevíracím seznamu vyberte kartu s.

## <a name="step-4-displaying-products-by-category"></a>Krok 4: Zobrazení produktů podle kategorie

K otestování nově přidaný `Products_SelectByCategoryID` uložené procedury a odpovídající vrstvy DAL a BLL metody, umožní vytvořit stránku ASP.NET, která obsahuje DropDownList a GridView s. DropDownList zobrazí seznam všech kategorií v databázi, zatímco prvku GridView zobrazí produkty, které patří do vybrané kategorie.

> [!NOTE]
> Jsme vytvořili záznamů master/detail rozhraní pomocí DropDownLists v předchozích kurzech. Podrobnější rozbor implementace záznamů master/detail sestavu, najdete [filtrování záznamů Master/Detail s DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) kurzu.


Otevřít `ExistingSprocs.aspx` stránku `AdvancedDAL` složky a DropDownList přetáhněte z panelu nástrojů do návrháře. Nastavte DropDownList s `ID` vlastnost `Categories` a jeho `AutoPostBack` vlastnost `True`. V dalším kroku z inteligentních značek, připojení k nové ObjectDataSource s názvem DropDownList `CategoriesDataSource`. Nakonfigurujte prvku ObjectDataSource tak, aby ho načte data z `CategoriesBLL` třída s `GetCategories` metody. Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Načtení dat z metody GetCategories CategoriesBLL třída s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Obrázek 7**: načtení dat z `CategoriesBLL` třída s `GetCategories` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Obrázek 8**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit záložky (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Po dokončení Průvodce prvek ObjectDataSource, nakonfigurujte DropDownList zobrazíte `CategoryName` pole data a použít `CategoryID` pole jako `Value` pro každou `ListItem`.

V tomto okamžiku by DropDownList a prvku ObjectDataSource s deklarativní podobný následujícímu:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

V dalším kroku přetáhněte GridView na Návrhář, že ho umístíte pod DropDownList. Nastavit prvek GridView s `ID` k `ProductsByCategory` a z inteligentních značek, jeho vazbu na nového prvku ObjectDataSource s názvem `ProductsByCategoryDataSource`. Konfigurace `ProductsByCategoryDataSource` ObjectDataSource používat `ProductsBLLWithSprocs` načtení třídy, minimu měl svoje data pomocí `GetProductsByCategoryID(categoryID)` – metoda. Protože tato GridView budou použity pouze k zobrazení dat, nastavte rozevírací seznamy v UPDATE, INSERT a odstraňovat karty na (žádný) a klikněte na tlačítko Další.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Obrázek 9**: Konfigurace ObjectDataSource k použití `ProductsBLLWithSprocs` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Načtení dat z GetProductsByCategoryID(categoryID) – metoda](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Obrázek 10**: načtení dat z `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Metoda zvolit na kartě vyberte očekává parametr, takže poslední krok v průvodci zobrazí výzvu, nám pro zdroj s parametrem. Nastavte parametr zdroj rozevíracího seznamu ovládacího prvku a zvolte `Categories` ovládacího prvku z rozevíracího seznamu ControlID. Kliknutím na Dokončit dokončíte průvodce.


[![Pomocí kategorií DropDownList jako zdroj categoryID parametr](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Obrázek 11**: použijte `Categories` DropDownList jako zdroj `categoryID` parametr ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Po dokončení průvodce bude prvek ObjectDataSource, Visual Studio přidá BoundFields a třídě CheckBoxField pro každé pole data produktu. Teď můžete k přizpůsobení těchto polí podle svých potřeb.

Na stránce prostřednictvím prohlížeče. Při návštěvě stránky vybrané kategorie Nápoje a odpovídající produkty uvedenými v mřížce. Změny rozevíracího seznamu do alternativní kategorie, jako obrázek 12 znázorňuje, vyvolá zpětné volání a znovu načte mřížky s produkty nově vybranou kategorii.


[![Produkty v kategorii vytvořit jsou zobrazeny.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Obrázek 12**: The produktů v kategorii vytvoření se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Krok 5: Zabalení uložené procedury s příkazy v rámci oboru transakcí

V [zabalení úprav databáze do transakce](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) kurzu jsme probírali techniky pro provádění řadu příkazů změny databáze v rámci oboru transakce. Vzpomínáte, změny v zastřešující transakce provést buď všechny úspěšné nebo převzetí služeb při všech, uzavřené atomicitu. Mezi dostupné techniky pro použití transakcí patří:

- Použití tříd v `System.Transactions` oboru názvů
- Vrstva přístupu k datům s pomocí ADO.NET třídy typu `SqlTransaction`, a
- Přidání příkazů T-SQL transakce přímo v rámci uložené procedury

*Zabalení úprav databáze do transakce* kurz v DAL použít třídy rozhraní ADO.NET. Zbývající část tohoto kurzu zkoumá, jak spravovat transakci pomocí příkazů T-SQL z v rámci uložené procedury.

Jsou tři příkazy SQL klíče pro ruční spuštění, potvrzení a vrácení zpět transakcí `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, a `ROLLBACK TRANSACTION`v uvedeném pořadí. Jako s přístupem ADO.NET, při použití transakcí v rámci uložené procedury, musíme použít následujícímu vzoru:

1. Označení začátku transakce.
2. Spouštění příkazů jazyka SQL, které tvoří příslušnou transakci.
3. Pokud dojde k chybě v některém z příkazů z kroku 2, vrácení transakce.
4. Pokud všechny příkazy z kroku 2 dokončena bez chyb, potvrzení transakce.

Tento model je implementovat v syntaxi T-SQL pomocí následující šablony:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Šablona spustí definováním `TRY...CATCH` zablokovat, použít konstrukce nový SQL Server 2005. Jak je `Try...Catch` blokuje v jazyce Visual Basic, SQL `TRY...CATCH` spouští příkazy v bloku `TRY` bloku. Pokud libovolný příkaz vyvolá chybu, ovládací prvek bude převeden okamžitě na `CATCH` bloku.

Pokud zde nejsou žádné chyby provádění příkazů SQL tuto strukturu transakce, `COMMIT TRANSACTION` příkazu potvrdí změny a dokončení transakce. Pokud jeden z příkazů však způsobí chybu, `ROLLBACK TRANSACTION` v `CATCH` bloku se vrátí do stavu před zahájením transakce databáze. Uložená procedura také vyvolává chybu pomocí [příkaz RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), které způsobí, že `SqlException` vyvolání v aplikaci.

> [!NOTE]
> Vzhledem k tomu, `TRY...CATCH` blok je nový SQL Server 2005, výše uvedené šablony nebude fungovat, pokud používáte starší verze systému Microsoft SQL Server. Pokud nepoužíváte SQL Server 2005, projděte si [Správa transakcí v SQL serveru uložené procedury](http://www.4guysfromrolla.com/webtech/080305-1.shtml) šablony, která bude fungovat s jinými verzemi SQL serveru.


Podívejte se na konkrétní příklad s let. Existuje omezení cizího klíče mezi `Categories` a `Products` tabulek, což znamená, že každá `CategoryID` pole `Products` tabulky musí být namapovaný na `CategoryID` hodnota v `Categories` tabulky. Jakoukoli akci, která by mohla narušit omezení, jako je například pokus o odstranění kategorii, která má související produkty, výsledkem je porušení omezení pro cizí klíč. Chcete-li to ověřit návštěvě příklad aktualizace a odstranění stávajících binárních dat ve spolupráci s oddílem binárních dat (`~/BinaryData/UpdatingAndDeleting.aspx`). Tato stránka obsahuje seznam jednotlivých kategorií v systému spolu s tlačítka pro úpravy a odstranění (viz obrázek 13), ale pokud se pokusíte odstranit kategorii, která má související produkty – například nápoje - odstranění nezdaří z důvodu narušení omezení pro cizí klíč (viz obrázek 14).


[![Každá kategorie se zobrazí v prvku GridView s upravit a odstranit tlačítka](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Obrázek 13**: Každá kategorie se zobrazí v prvku GridView s upravit a odstranit tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Nelze odstranit kategorii, která má existující produkty](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Obrázek 14**: nelze odstranit kategorii, která má existující produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Představte si však, že chceme povolit kategorie má být odstraněn bez ohledu na to, zda mají přiřazené produktů. Odstranit kategorii s produkty, představte si, že chceme také odstranit existující produkty (i když další možností je jednoduše nastavte své produkty `CategoryID` hodnoty `NULL`). Tato funkce může implementované pomocí pravidel cascade omezení cizího klíče. Můžeme také vytvořit uloženou proceduru, která přijímá `@CategoryID` vstupní parametr a při vyvolání, explicitně odstraní všechna související produkty a potom zadané kategorie.

Naše první pokus o uloženou proceduru mohou vypadat nějak takto:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Zatímco jednoznačně tato akce odstraní přidružené produktů a kategorii, neučiní tak v části zastřešující transakce. Představte si, že se na některé další omezení pro cizí klíč `Categories` nebude odstranění konkrétní `@CategoryID` hodnotu. Problém je, že v takovém případě všech produktů, které se odstraní předtím, než jsme pokus o odstranění kategorii. Net výsledkem je, že pro tyto kategorie tuto uloženou proceduru by odebrat všechny její produkty zatímco zůstala kategorie, protože s ním stále související záznamy v jiné tabulce.

Pokud uložená procedura byly zabalené v rámci oboru transakce, ale při odstraní pro `Products` tabulka bude vrácena zpět i v případě se nepovedlo odstranit `Categories`. Následující skript uložené procedury používá transakce, aby zajistil atomicitu mezi těmito dvěma `DELETE` příkazy:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Za chvíli přidat `Categories_Delete` uložené procedury k databázi Northwind. Vraťte se ke kroku 1 pokyny k přidání uložených procedur do databáze.

## <a name="step-6-updating-thecategoriestableadapter"></a>Krok 6: Aktualizace`CategoriesTableAdapter`

Při jsme přidali `Categories_Delete` uložené procedury do databáze, DAL je aktuálně nakonfigurován pro použití ad-hoc SQL k provedení odstranit. Potřebujeme aktualizovat `CategoriesTableAdapter` a dostane pokyn, aby používat `Categories_Delete` místo toho uložená procedura.

> [!NOTE]
> Dříve v tomto kurzu jsme pracovali `NorthwindWithSprocs` datové sady. Ale, že datová sada obsahuje pouze jednu entitu, `ProductsDataTable`, a My potřebujeme práce s kategoriemi. Proto pro zbývající část tohoto kurzu, když mám mluvit o m Data Access Layer I, který se odkazuje na `Northwind` datové sady, který jsme vytvořili první v [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-vb.md) kurzu.


Otevřete datovou sadu Northwind, vyberte `CategoriesTableAdapter`a přejděte do okna Vlastnosti. Seznamy vlastností okna `InsertCommand`, `UpdateCommand`, `DeleteCommand`, a `SelectCommand` používané TableAdapter, jakož i její název a informace o připojení. Rozbalte `DeleteCommand` vlastnost zobrazíte její podrobnosti. Jak ukazuje obrázek 15 `DeleteCommand` s `ComamndType` je nastavena na Text, který se má poslat v textu nastaví `CommandText` vlastnosti jako datový typ dotazu SQL ad-hoc.


![V návrháři a zobrazte její vlastnosti v okně Vlastnosti vyberte CategoriesTableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Obrázek 15**: vyberte `CategoriesTableAdapter` v návrháři a zobrazte její vlastnosti v okně Vlastnosti


Chcete-li změnit tato nastavení, v okně Vlastnosti vyberte text (událost DeleteCommand) a z rozevíracího seznamu zvolte (nové). Touto akcí vymažete si nastavení `CommandText`, `CommandType`, a `Parameters` vlastnosti. Dále nastavte `CommandType` vlastnost `StoredProcedure` a pak zadejte název uložené procedury pro `CommandText` (`dbo.Categories_Delete`). Pokud jste nezapomeňte napřed zadejte vlastnosti v uvedeném pořadí – `CommandType` a pak `CommandText` – Visual Studio automaticky naplní kolekci parametrů. Pokud nezadáte tyto vlastnosti v tomto pořadí, budete muset ručně přidat parametry prostřednictvím Editor kolekce parametrů. V obou případech je třeba ji zkontrolovat kliknout na symbol tří teček v vlastnost Parameters zpřístupnit nahoru Editor kolekce parametrů pro ověření, že správný parametr nastavení byly provedeny změny (viz obrázek 16) s. Pokud se nezobrazí žádné parametry v dialogovém okně, přidejte `@CategoryID` parametr ručně (není potřeba přidat `@RETURN_VALUE` parametr).


![Ujistěte se, že parametry nastavení jsou správně](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Obrázek 16**: Ujistěte se, že parametry nastavení jsou správně


Po DAL se aktualizovala, odstraňuje se kategorie bude automaticky odstraní všechny jeho související produkty a učinit v rámci zastřešující transakce. Chcete-li to ověřit, vraťte se na stránku aktualizace a odstranění stávajících binárních dat a klikněte na tlačítko Odstranit pro některou z kategorií. Jeden jediným kliknutím myší kategorie a všechny jeho přidružené produkty se odstraní.

> [!NOTE]
> Před testováním `Categories_Delete` uložené procedury, které se odstraní celou řadou produkty spolu s vybranou kategorii, může být vhodné vytvořit záložní kopii databáze. Pokud používáte `NORTHWND.MDF` databáze v `App_Data`, jednoduše zavřete sadu Visual Studio a zkopírujte soubory MDF a LDF v `App_Data` do nějaké složky. Po otestování funkce, můžete obnovit databázi zavření sady Visual Studio a nahradí aktuální MDF a LDF soubory `App_Data` pomocí záložní kopie.


## <a name="summary"></a>Souhrn

Průvodci vytvořením objektu TableAdapter s budou automaticky generovat uložené procedury pro nás, existují situace, kdy jsme může již takové vytvářet úložné procedury nebo chcete je vytvořit ručně nebo pomocí jiných nástrojů místo. Takových scénářů TableAdapter lze také nastavit tak, aby odkazoval na stávající úložnou proceduru. V tomto kurzu jsme se podívali na tom, jak ručně přidat uložené procedury do databáze prostřednictvím prostředí sady Visual Studio a jak propojí metod TableAdapter s těchto uložených procedurách. Můžeme také prozkoumat příkazů T-SQL a skript používaným pro počáteční potvrzení a vrácení zpět transakcí z v rámci uložené procedury.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Hilton Geisenow, S ren Jakub Lauritsen a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [další](updating-the-tableadapter-to-use-joins-vb.md)
