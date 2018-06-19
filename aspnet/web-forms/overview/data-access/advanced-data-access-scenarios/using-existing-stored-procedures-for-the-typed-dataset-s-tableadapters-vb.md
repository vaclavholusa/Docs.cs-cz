---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Použití existující uložené procedury pro typové datové sady TableAdapters (VB) | Microsoft Docs
author: rick-anderson
description: V předchozích kurzu jsme zjistili, jak používat Průvodce nastavením TableAdapter generovat nové uložené procedury. V tomto kurzu jsme dozvíte, jak stejné TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: ac319b67c9215c5dde8e7507076ed45a1f7825c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877369"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Použití existující uložené procedury pro typové datové sady TableAdapters (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) nebo [stáhnout PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> V předchozích kurzu jsme zjistili, jak používat Průvodce nastavením TableAdapter generovat nové uložené procedury. V tomto kurzu jsme zjistěte, jak můžete stejné Průvodce nastavením TableAdapter pracovat existující uložené procedury. Můžeme také zjistěte, jak ručně přidat nové uložených procedur do databáze hotový.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) jsme viděli, jak může typové datové sady s TableAdapters nakonfigurovat tak, aby pomocí uložených procedur se příkazy SQL dat než ad-hoc přístup. Konkrétně jsme se zaměřili na tom, jak průvodce nastavením TableAdapter automaticky vytvořit tyto uložené procedury. Při portování starší verze aplikace na technologii ASP.NET 2.0 nebo při vytváření webu k technologii ASP.NET 2.0 kolem existující datový model, je pravděpodobné, že databáze již obsahuje uložené procedury, potřebujeme. Alternativně může ale chcete vytvořit uložené procedury ručně nebo pomocí některé nástroje, než Průvodce nastavením TableAdapter, které automaticky generuje uložené procedury.

V tomto kurzu se podíváme na tom, jak nakonfigurovat TableAdapter použití existující uložené procedury. Vzhledem k tomu, že databáze Northwind má jenom malou sadu předdefinovaných uložené procedury, podíváme se také na kroky potřebné k nové uložené procedury ručně přidat do databáze pomocí prostředí Visual Studio. Umožňují s začít!

> [!NOTE]
> V [zabalení změny databáze v rámci transakce](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) kurzu jsme přidali metody k TableAdapter podpory transakcí (`BeginTransaction`, `CommitTransaction`a tak dále). Alternativně lze provádět zcela v rámci uložené procedury, která nevyžaduje žádné úpravy kód Data Access Layer transakce. V tomto kurzu jsme budete prozkoumejte příkazů T-SQL použít ke spuštění příkazů s uloženou proceduru v rámci oboru transakce.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Krok 1: Přidání uložených procedur do databáze Northwind

Visual Studio snadno přidat nové uložené procedury k databázi. Umožňují s přidat nové uložené procedury k databázi Northwind, která vrací všechny sloupce z `Products` tabulku pro ty, které mají určitý `CategoryID` hodnotu. Z okna Průzkumníka serveru rozbalte databázi Northwind, aby jeho složky - databázových schématech, tabulek, zobrazení a tak dále - zobrazí. Jak jsme viděli v předchozím kurzu, uložené procedury složka obsahuje databáze s existující uložené procedury. Pokud chcete přidat nové uložené procedury, jednoduše klikněte pravým tlačítkem na složku uložené procedury a zvolte možnost Přidat novou uloženou proceduru v místní nabídce.


[![Klikněte pravým tlačítkem na složku uložené procedury a přidejte nové uložené procedury](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Obrázek 1**: klikněte pravým tlačítkem na složku uložené procedury a přidejte nový uloženou proceduru ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Jak ukazuje obrázek 1, zvolte možnost Přidat novou uloženou proceduru otevře okno skript v sadě Visual Studio s obrysem skript SQL, které jsou nutné k vytvoření uložené procedury. Je naše úlohy k realizaci tento skript a spusťte ho v tomto okamžiku uložená procedura se zařadí do databáze.

Zadejte následující skript:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Tento skript, a to po provedení přidá nové uložené procedury k databázi Northwind s názvem `Products_SelectByCategoryID`. Tuto uloženou proceduru přijímá jeden vstupní parametr (`@CategoryID`, typu `int`) a vrátí všechna pole pro tyto produkty s odpovídající `CategoryID` hodnotu.

Chcete-li to provést `CREATE PROCEDURE` skript a přidat uloženou proceduru do databáze, klikněte na ikonu Uložit na panelu nástrojů nebo stiskněte kombinaci kláves Ctrl + S. Až to uděláte, aktualizuje složku uložené procedury, zobrazující nově vytvořený uložené procedury. Skript v okně se také změní subtlety z `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` k `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Přidá nové uložené procedury do databáze, zatímco `ALTER PROCEDURE` aktualizuje stávající. Vzhledem k tomu, že spuštění skriptu se změnila na `ALTER PROCEDURE`, změna uložené procedury vstupní parametry nebo příkazů SQL a kliknutím na ikonu uložit tyto změny aktualizuje uložené procedury.

Obrázek 2 ukazuje Visual Studio po `Products_SelectByCategoryID` uložená procedura se uložila.


[![Uložená procedura Products_SelectByCategoryID byla přidána do databáze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Obrázek 2**: uloženou proceduru `Products_SelectByCategoryID` byla přidána do databáze ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Krok 2: Konfigurace TableAdapter pro použití existující uložené procedury

Teď, když `Products_SelectByCategoryID` uložené procedury byla přidána do databáze, nakonfigurujeme naše Data Access Layer používat tuto uloženou proceduru, pokud jeden z jeho metody vyvolání. Konkrétně přidáme `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` metodu `ProductsTableAdapter` v `NorthwindWithSprocs` datovou sadu typu, který volá `Products_SelectByCategoryID` uložené procedury jsme právě vytvořili.

Začněte otevřením `NorthwindWithSprocs` datovou sadu. Klikněte pravým tlačítkem na `ProductsTableAdapter` a zvolte Přidat dotaz spustíte Průvodce konfigurací dotazu TableAdapter. V [předchozí kurzu](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) jsme vyjádřit výslovný souhlas tak, aby měl TableAdapter vytvořit nové uložené procedury pro nás. V tomto kurzu ale chceme propojit nová metoda TableAdapter ke stávající `Products_SelectByCategoryID` uložené procedury. Proto zvolte možnost použití existující uložené procedury v prvním kroku průvodce s a pak klikněte na tlačítko Další.


[![Zvolte použití existující uložené procedury možnost](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Obrázek 3**: Zvolte použití existující uložené procedury možnost ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Následující obrazovku poskytuje že rozevíracího seznamu naplní databázi s uložené procedury. Vyberte uloženou proceduru uvádí jeho vstupních parametrů na levé straně a pole dat, která je vrácena (pokud existuje) na pravé straně. Vyberte `Products_SelectByCategoryID` uloženou proceduru ze seznamu a klikněte na tlačítko Další.


[![Vyberte Products_SelectByCategoryID uložené procedury](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Obrázek 4**: vyberte `Products_SelectByCategoryID` uloženou proceduru ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Na další obrazovce vás nám jaký typ dat je vrácený uložené procedury a zde naše odpověď určí typ vrácený metodou TableAdapter s. Například pokud jsme indikuje, že se vrátí tabulková data, metoda vrátí `ProductsDataTable` instanci naplněnou záznamy vrácené uloženou proceduru. Naproti tomu Pokud jsme znamenat, že tuto uloženou proceduru vrátí jednu hodnotu TableAdapter se vrátí `Object` přiřazené hodnoty z prvního sloupce na první záznam vrácený uložené procedury.

Vzhledem k tomu `Products_SelectByCategoryID` uložené procedury vrátí všechny produkty, které patří do určité kategorie, zvolte první odpověď - tabulková data - a klikněte na tlačítko Další.


[![Označuje, že uložené procedury vrátí tabulková Data](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Obrázek 5**: znamenat, že uložený postup vrátí Tabular Data ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Zbývá označující, co metoda vzory použít následuje názvy pro tyto metody. Nechte obě výplně DataTable a vraťte se možnosti DataTable zaškrtnuto, ale přejmenovat metody k `FillByCategoryID` a `GetProductsByCategoryID`. Klikněte na tlačítko Další Zkontrolujte souhrn úlohy, které průvodce provede. Pokud se vše vypadá v pořádku, klikněte na tlačítko Dokončit.


[![Název metody FillByCategoryID a GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Obrázek 6**: název metody `FillByCategoryID` a `GetProductsByCategoryID` ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Metod TableAdapter jsme právě vytvořili, `FillByCategoryID` a `GetProductsByCategoryID`, očekávat vstupní parametr typu `Integer`. Je tato hodnota vstupní parametr předaný do uložené procedury prostřednictvím jeho `@CategoryID` parametr. Pokud změníte `Products_SelectByCategory` uložené procedury s parametry, budete muset taky aktualizovat parametry pro tyto metody TableAdapter. Jak je popsáno v [předchozí kurzu](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), to lze provést jedním ze dvou způsobů: ručně přidáním nebo odebráním parametrů v kolekci parametrů nebo opětovným spuštěním Průvodce nastavením TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Krok 3: Přidání`GetProductsByCategoryID(categoryID)`BLL – metoda

Pomocí `GetProductsByCategoryID` metoda DAL dokončení, dalším krokem je poskytnout přístup k této metody v vrstvu obchodní logiky. Otevřete `ProductsBLLWithSprocs` třídy souboru a přidejte následující metodu:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Tato metoda BLL jednoduše vrátí `ProductsDataTable` vrácená z `ProductsTableAdapter` s `GetProductsByCategoryID` metoda. `DataObjectMethodAttribute` Atribut poskytuje metadata používá Průvodce konfigurace zdroje dat s ObjectDataSource. Konkrétně tato metoda se zobrazí v rozevíracím seznamu vyberte kartu s.

## <a name="step-4-displaying-products-by-category"></a>Krok 4: Zobrazení produkty podle kategorie

K testování nově přidaný `Products_SelectByCategoryID` uložené procedury a odpovídající DAL a BLL metody, umožní vytvořit stránku ASP.NET, který obsahuje rozevírací seznam a GridView s. Rozevírací seznam se zobrazí seznam všech kategorií v databázi při GridView zobrazí produkty, které patří do vybrané kategorie.

> [!NOTE]
> Jsme rozhraní a podrobností jste vytvořili v předchozí kurzy DropDownLists. Podrobnější pohled na implementaci a podrobností sestavy, najdete v části [a podrobností filtrování s rozevírací seznam](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) kurzu.


Otevřete `ExistingSprocs.aspx` stránku `AdvancedDAL` složku a přetáhněte rozevírací seznam z panelu nástrojů na návrháře. Nastavte rozevírací seznam s `ID` vlastnost `Categories` a jeho `AutoPostBack` vlastnost `True`. V dalším kroku z inteligentních značek, vytvořit vazbu rozevírací seznam nové ObjectDataSource s názvem `CategoriesDataSource`. Nakonfigurovat ObjectDataSource tak, aby ho načte data z `CategoriesBLL` třídu s `GetCategories` metoda. Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Načtení dat z metody třídy CategoriesBLL s GetCategories](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Obrázek 7**: načtení dat z `CategoriesBLL` třídu s `GetCategories` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Obrázek 8**: rozevíracího seznamu jsou uvedeny ve UPDATE, INSERT a odstranit karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Po dokončení Průvodce ObjectDataSource, nakonfigurujte rozevírací seznam pro zobrazení `CategoryName` datové pole a použít `CategoryID` pole jako `Value` pro každou `ListItem`.

V tomto okamžiku by rozevírací seznam a ObjectDataSource s deklarativní podobný následujícímu:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

V dalším kroku přetažením GridView na návrháře jeho umístění pod rozevírací seznam. Nastavit GridView s `ID` k `ProductsByCategory` a z jeho inteligentních značek navázat jej na nové ObjectDataSource s názvem `ProductsByCategoryDataSource`. Konfigurace `ProductsByCategoryDataSource` ObjectDataSource používat `ProductsBLLWithSprocs` třídu, má ji načíst jeho data pomocí `GetProductsByCategoryID(categoryID)` metoda. Vzhledem k tomu, že tato rutina GridView se použije pouze pro zobrazení dat, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný) a klikněte na tlačítko Další.


[![Konfigurace ObjectDataSource použití třídy ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Obrázek 9**: Konfigurace ObjectDataSource pro použití `ProductsBLLWithSprocs` – třída ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Načtení dat z metody GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Obrázek 10**: načtení dat z `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Metodě vybrané na kartě vyberte očekává parametr, takže posledním kroku průvodce nám vyzve ke zdroji s parametr. Nastavte parametr zdrojového rozevíracího seznamu k řízení a zvolte `Categories` ovládací prvek z rozevíracího seznamu ControlID. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Použijte rozevírací seznam kategorií jako zdroj categoryID parametr](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Obrázek 11**: použití `Categories` rozevírací seznam jako zdroj `categoryID` parametr ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Po dokončení průvodce bude ObjectDataSource, Visual Studio přidá BoundFields a třídy CheckBoxField pro každé pole data produktu. Nebojte se, že přizpůsobení těchto polí podle svých potřeb.

Navštivte stránku prostřednictvím prohlížeče. Při návštěvě stránky zvolené kategorii Nápoje a odpovídající produkty, které jsou uvedeny v mřížce. Změna rozevíracího seznamu do alternativní kategorie, jako obrázek 12 znázorňuje, způsobí, že zpětné volání a znovu načte mřížky s produkty nově vybrané kategorie.


[![Produkty v kategorii vytvořit jsou zobrazeny.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Obrázek 12**: se zobrazují produktů v kategorii vytvořit ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Krok 5: Zabalení uložené procedury s příkazy v oboru transakce.

V [zabalení změny databáze v rámci transakce](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) kurzu jsme probrali techniky pro provádění řady příkazů změny databáze v rámci oboru transakce. Stažení, který změny provést pod pojmem transakce buď všechny úspěšné nebo všechny selhání, která zaručí nedělitelnost. Mezi dostupné techniky pro použití transakcí patří:

- Pomocí třídy v `System.Transactions` oboru názvů
- Data Access Layer nutnosti použít třídy ADO.NET jako `SqlTransaction`, a
- Přidání příkazů transakce T-SQL přímo v rámci uložené procedury

*Zabalení změny databáze v rámci transakce* kurzu používané třídy ADO.NET v DAL. Zbývající část tohoto kurzu hledá Správa transakce pomocí příkazů T-SQL z v rámci uložené procedury.

Jsou tři příkazy SQL klíče pro ruční spuštění, potvrzení a vrácení transakce zpět `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, a `ROLLBACK TRANSACTION`, v uvedeném pořadí. S tímto přístupem ADO.NET, jako při použití transakce z v rámci uložené procedury je potřeba použít následující vzorek:

1. Upozornění na zahájení transakce.
2. Spouštění příkazů jazyka SQL, které tvoří transakce.
3. Pokud dojde k chybě v některém z příkazy z kroku 2, vrácení transakce.
4. Pokud všechny příkazy z kroku 2 dokončeno bez chyb, potvrzení transakce.

Tento vzor, můžou se implementovat v syntaxi T-SQL pomocí následující šablony:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Šablona spustí definováním `TRY...CATCH` blok konstrukt nový SQL Server 2005. Jako s `Try...Catch` bloků v jazyce Visual Basic SQL `TRY...CATCH` spouští příkazy v bloku `TRY` bloku. Pokud žádné příkaz vyvolá chybu, je ovládací prvek okamžitě přenesen na `CATCH` bloku.

Pokud nejsou žádné chyby provádění příkazů SQL tento způsob vytvoření transakce, `COMMIT TRANSACTION` příkaz potvrdí změny a dokončení transakce. Pokud však mezi příkazy skončí chybou, `ROLLBACK TRANSACTION` v `CATCH` bloku vrátí databázi do stavu před zahájení transakce. Uložená procedura také dojde k chybě pomocí [příkaz RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), který spustí `SqlException` má být aktivována v aplikaci.

> [!NOTE]
> Vzhledem k tomu `TRY...CATCH` blok je nový systém SQL Server 2005, výše uvedené šablony nebude fungovat, pokud používáte starší verze systému Microsoft SQL Server. Pokud se nepoužívá systém SQL Server 2005, projděte si [Správa transakcí v SQL serveru uložené procedury](http://www.4guysfromrolla.com/webtech/080305-1.shtml) pro šablonu, která bude fungovat s jiné verze systému SQL Server.


Umožní s, podívejte se na konkrétní příklady. Mezi existuje omezení cizího klíče `Categories` a `Products` tabulky, což znamená, že se každý `CategoryID` pole v `Products` tabulky musí být namapována na `CategoryID` hodnotu `Categories` tabulky. Všechny akce, která by způsobila porušení toto omezení, jako je například pokusu o odstranění kategorie, která má přidružené produkty, výsledkem je porušení omezení pro cizí klíč. Chcete-li to ověřit pokroku příklad aktualizace a odstranění existující binární Data v práci s části binárních dat (`~/BinaryData/UpdatingAndDeleting.aspx`). Tato stránka obsahuje seznam každou kategorii v systému společně s tlačítka Upravit a odstranit (viz obrázek 13), ale pokud se pokusíte odstranit kategorii, která má přidružené produkty – například nápoje - odstranění nezdaří z důvodu narušení omezení cizího klíče (viz obrázek 14).


[![Každá kategorie se zobrazí v GridView s upravit a odstranit tlačítka](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Obrázek 13**: Každá kategorie se zobrazí v GridView s upravit a odstranit tlačítka ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Nelze odstranit kategorii, která má existující produkty](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Obrázek 14**: nelze odstranit kategorii, která má existující produkty ([Kliknutím zobrazit obrázek v plné velikosti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Představte si ale, že chceme povolit kategorie má být odstraněn bez ohledu na to, zda mají přiřazené produkty. Měla by být odstraněna kategorie s produkty, představte si, že chceme také odstranit její existující produkty (i když jinou možnost může být jednoduše nastavit jeho produktů `CategoryID` hodnoty k `NULL`). Tato funkce může implementovat prostřednictvím pravidel cascade omezení cizího klíče. Alternativně může vytvoříme uložené procedury, která přijímá `@CategoryID` vstupní parametr a po vyvolání explicitně odstraní všechna přidružená produktů a potom Zadaná kategorie.

Naše o první pokus o uložené proceduře může vypadat následovně:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Při výborný tato akce odstraní přidružené produktů a kategorii, se neprovádí tak pod pojmem transakce. Představte si, že je na některé další omezení cizího klíče `Categories` , by zakázat odstranění konkrétní `@CategoryID` hodnotu. Problém je, že v takovém případě všechny produkty se odstraní před jsme pokusu o odstranění kategorii. Net výsledkem je, že tuto třídu tuto uloženou proceduru by odebrat všechny produkty zatímco kategorii zůstala vzhledem k tomu, že je stále existují související záznamy v jiné tabulce.

Pokud uložená procedura byly zabalené v rámci oboru transakce, ale při odstraní pro `Products` tabulky by se při krátkodobém Neúspěšné odstranění vrátit zpět `Categories`. Následující skript uložená procedura používá transakce, aby zajistil nedělitelnost mezi těmito dvěma `DELETE` příkazy:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Za chvíli přidat `Categories_Delete` uložené procedury k databázi Northwind. Odkazovat zpět ke kroku 1 postup pro přidání uložené procedury k databázi.

## <a name="step-6-updating-thecategoriestableadapter"></a>Krok 6: Aktualizace`CategoriesTableAdapter`

Při jsme přidali jste `Categories_Delete` uloženou proceduru v databázi, DAL je nyní nakonfigurován, aby používat příkazy SQL ad-hoc k odstranění. Je potřeba aktualizovat `CategoriesTableAdapter` a dostane pokyn, aby použít `Categories_Delete` místo uložené procedury.

> [!NOTE]
> V tomto kurzu jsme pracovali `NorthwindWithSprocs` datovou sadu. Ale, že se datová sada má jenom jedna entita, `ProductsDataTable`, a potřebujeme pro práci s kategorií. Proto pro zbývající část tohoto kurzu při I mluvit o m Data Access Layer I, který se odkazuje na `Northwind` datovou sadu, ten, který jsme vytvořili nejprve v [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-vb.md) kurzu.


Otevřete datovou sadu Northwind, vyberte `CategoriesTableAdapter`a přejděte do okna vlastností. Seznamy vlastností okna `InsertCommand`, `UpdateCommand`, `DeleteCommand`, a `SelectCommand` používané TableAdapter, jakož i jeho název a informace o připojení. Rozbalte `DeleteCommand` vlastnost zobrazíte její podrobnosti. Jak je vidět na obrázku 15, `DeleteCommand` s `ComamndType` je nastavena na hodnotu Text, který nastaví ho odeslat text v `CommandText` vlastnost jako příkazu jazyka SQL ad hoc.


![Vyberte CategoriesTableAdapter v návrháři a zobrazte její vlastnosti v okně vlastností](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Obrázek 15**: vyberte `CategoriesTableAdapter` v návrháři a zobrazte její vlastnosti v okně vlastností


Chcete-li změnit tato nastavení, vyberte text (DeleteCommand) v okně vlastností a zvolte (Nový) z rozevíracího seznamu. Tímto dojde k vymazání se nastavení `CommandText`, `CommandType`, a `Parameters` vlastnosti. Dále nastavte `CommandType` vlastnost `StoredProcedure` a pak zadejte název uložené procedury pro `CommandText` (`dbo.Categories_Delete`). Pokud jste si nezapomeňte nejprve zadejte vlastnosti v tomto pořadí: `CommandType` a potom `CommandText` -Visual Studio budou automaticky vyplnit kolekce parametrů. Pokud nezadáte tyto vlastnosti v tomto pořadí, je nutné ručně přidat parametry prostřednictvím Editor kolekce parametrů. V obou případech ho s doporučeno klikněte na symbol tří teček ve vlastnosti parametry, aby si Editor kolekce parametrů k ověření, zda správný parametr nastavení byly provedeny změny (viz obrázek 16). Pokud se nezobrazí žádné parametry v dialogovém okně, přidejte `@CategoryID` parametr ručně (není potřeba přidat `@RETURN_VALUE` parametr).


![Ujistěte se, zda jsou nastavení parametrů opravte](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Obrázek 16**: Zkontrolujte, zda jsou nastavení parametrů opravte


Po DAL se aktualizovala, kategorie se automaticky odstraní všechny jeho přidružené produktů a učinit pod pojmem transakce. Chcete-li to ověřit, návrat na stránku aktualizace a odstranění existující binární Data a klikněte na tlačítko Odstranit pro některou z kategorií. Pomocí jednoho jedním kliknutím myší kategorie a všechny její přidružené produkty se odstraní.

> [!NOTE]
> Před testováním `Categories_Delete` uložené procedury, která se odstranit některé produkty spolu s vybranou kategorii, může být vhodné vytvořit záložní kopii databáze. Pokud používáte `NORTHWND.MDF` databáze v `App_Data`, jednoduše zavřete Visual Studio a zkopírujte soubory MDF a LDF v `App_Data` některé jiné složky. Po otestování funkci, můžete obnovit databázi ukončením sady Visual Studio a nahraďte aktuální MDF a LDF soubory v `App_Data` pomocí záložní kopie.


## <a name="summary"></a>Souhrn

Během Průvodce nastavením TableAdapter s bude automaticky generovat uložené procedury pro nás, existují situace, když jsme může už máte tyto uložené procedury vytvořit nebo chcete vytvořit je ručně nebo pomocí jiných nástrojů místo. Abychom vyhověli takových scénářů, TableAdapter můžete také nakonfigurovat tak, aby existující uložené procedury. V tomto kurzu jsme se podívali na postup ručního přidání uložených procedur do databáze pomocí prostředí Visual Studio a postup propojit s metod TableAdapter na tyto uložené procedury. Můžeme také posouzení příkazů T-SQL a skriptu způsobem používaným pro počáteční, potvrzení a vracení změn transakce z v rámci uložené procedury.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Hilton Geisenow, S ren Jakub Lauritsen a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [další](updating-the-tableadapter-to-use-joins-vb.md)
