---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Zabalení úprav databáze do transakce (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz je první ze čtyř, která srovnává aktualizace, odstraňování a vkládání dat dávek. V tomto kurzu jsme dozvíte, jak povolit databázové transakce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bddf8aaee20b072e703ecc907aedfd5bb58450d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363925"
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Zabalení úprav databáze do transakce (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) nebo [stahovat PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Tento kurz je první ze čtyř, která srovnává aktualizace, odstraňování a vkládání dat dávek. V tomto kurzu jsme dozvíte, jak povolit batch změny prováděné jako atomickou operaci, což zajistí, že všechny kroky úspěšné nebo neúspěšné všechny kroky, databázové transakce.


## <a name="introduction"></a>Úvod

Jak jsme viděli, počínaje [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) kurzu prvku GridView obsahuje integrovanou podporu pro úpravy na úrovni řádků a odstranění. Pomocí několika kliknutí myši je možné vytvořit rozhraní úpravy velké množství dat bez nutnosti psát kód, tak dlouho, dokud se obsah s úpravy a odstranění na základě na řádek. Ale v některých případech to nestačí a My potřebujeme uživatelům poskytnout možnost upravit nebo odstranit dávky záznamů.

Například nejvíce webové e-mailoví klienti pomocí mřížky do seznamu každá zpráva kde každý řádek obsahuje zaškrtávací políčko společně s informacemi s e-mailu (předmět, odesílatele a tak dále). Toto rozhraní umožňuje uživateli odstranit více zpráv tak, že je a potom klikněte na tlačítko Odstranit vybrané zprávy. Úpravy rozhraní batch je ideální v situacích, kde uživatelé běžně upravovat mnoho různých záznamů. Místo vynucení uživatel klepne na tlačítko Upravit, proveďte jejich změnu a klepněte na tlačítko Aktualizovat pro každý záznam, který musí být upravena, batch úpravy rozhraní vykreslí každý řádek s jeho úpravy rozhraní. Uživatel můžete rychle změnit sadu řádků, které je potřeba změnit a tyto změny uložit kliknutím na tlačítko Aktualizovat vše. V této sérii kurzů prozkoumáme vytvoření rozhraní pro vkládání, úpravy a odstranění dat dávek.

Při provádění dávkových operací se nezdaří s důležité určit, zda by mělo být možné u některých operací ve službě batch k úspěšnému while ostatním uživatelům. Vezměte v úvahu dávkové odstranění rozhraní – co se stane při splnění první vybraný záznam byl úspěšně odstraněn, ale druhá selže, například z důvodu narušení omezení pro cizí klíč? By měl být první odstranění záznamu s vrátila zpět nebo je přijatelné pro první záznam zůstat odstraněné?

Pokud chcete, aby dávkové operace zacházeno jako [atomické operace](http://en.wikipedia.org/wiki/Atomic_operation), jeden kde buď všechny kroky proběhly úspěšně nebo všechny kroky nezdaří, pak vrstvy přístupu k datům musí být rozšíření zahrnují podporu pro [databáze transakce](http://en.wikipedia.org/wiki/Database_transaction). Databázové transakce zaručit atomicitu pro sadu `INSERT`, `UPDATE`, a `DELETE` příkazy spouští pod zastřešující transakce a jsou funkcí podporovaných většina všechny moderní databázovými systémy.

V tomto kurzu podíváme na tom, jak rozšířit DAL použít databázové transakce. Dalších kurzech se zaměřuje implementující webové stránky pro dávkové vložení, aktualizace nebo odstranění rozhraní. Začínáme s let!

> [!NOTE]
> Při změně dat v rámci dávkové transakce, není vždy nutné atomicitu. V některých případech může být přijatelný mít některé změny dat, které jsou úspěšné a dalším uživatelům ve stejné dávkové nezdaří, třeba při odstranění sady e-mailů z klienta webového e-mailu. Pokud je zde s zpracování databáze chyba, polovině odstranění, ho s pravděpodobně přijatelný, zůstaly odstraněné záznamy zpracovat bez chyb. V takových případech není potřeba DAL upravit tak, aby podporu transakcí databáze. Existují další dávkové operace scénáře, však kde atomicitu je důležité. Pokud zákazník přesune její prostředků z jednoho účtu bank do jiného, musí provést dvě operace: prostředků musí být odečtena od první účet a pak přidá do druhé. Banka nemusí vadit s prvním krokem úspěšné, ale druhý krok nezdaří, může být svým zákazníkům understandably nespokojený. Můžu vám doporučujeme projít tento kurz a implementaci rozšíření pro podporu transakcí databáze i v případě, že nemáte v plánu na jejich použití v dávkové vložení, aktualizace nebo odstranění rozhraní, které nám budete vytvářet v následujících kurzech tři vrstvy Dal.


## <a name="an-overview-of-transactions"></a>Přehled transakcí

Většina databází zahrnují podporu pro *transakce*, které povolují více příkazů databáze seskupeny do jedné logické jednotky práce. Příkazy databáze, které tvoří transakci je zaručena atomické, což znamená, že buď všechny příkazy se nezdaří, nebo všechny proběhne úspěšně.

Obecně platí transakce jsou implementované pomocí příkazy SQL s použitím následujícího vzorce:

1. Označení začátku transakce.
2. Spouštění příkazů jazyka SQL, které tvoří příslušnou transakci.
3. Pokud dojde k chybě v některém z příkazů z kroku 2, vrácení transakce.
4. Pokud všechny příkazy z kroku 2 dokončena bez chyb, potvrzení transakce.

Příkazy SQL použitý k vytvoření, potvrzení a vrácení transakce se dají zadat ručně při psaní skriptů SQL nebo vytváření uložených procedur nebo prostřednictvím programový znamená, že pomocí ADO.NET nebo tříd v [ `System.Transactions` obor názvů](https://msdn.microsoft.com/library/system.transactions.aspx). V tomto kurzu, který pouze prozkoumáme Správa transakce pomocí technologie ADO.NET. V budoucích kurzech se podíváme na tom, jak pomocí uložených procedur v vrstvy přístupu k datům, po kterém se podíváme příkazů SQL pro vytváření, vrácení zpět a potvrzování transakcí. Do té doby najdete [Správa transakcí v SQL serveru uložené procedury](http://www.4guysfromrolla.com/webtech/080305-1.shtml) Další informace.

> [!NOTE]
> [ `TransactionScope` Třídy](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) v `System.Transactions` obor názvů umožňuje vývojářům programově zabalení řadu příkazů v rámci oboru transakce a zahrnuje podporu pro složité transakce, které se týkají více zdroje, jako jsou dva různé databáze nebo dokonce heterogenních typů úložišť dat, jako je například databáze Microsoft SQL Server, Oracle database a webové služby. Můžu uložit se rozhodli použít transakce ADO.NET pro účely tohoto kurzu místo `TransactionScope` třídy, protože je určené pro databázové transakce a v mnoha případech ADO.NET je mnohem méně náročná. Kromě toho v určitých případech `TransactionScope` třída používá Microsoft distribuované transakce koordinátor (MSDTC). Problémy s konfigurací, implementaci a výkonu okolního MSDTC umožňuje místo specializované a pokročilé tématu a nad rámec těchto kurzů.


Při práci se zprostředkovatelem SqlClient v ADO.NET, transakce inicializována prostřednictvím volání [ `SqlConnection` třídy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), který vrátí hodnotu [ `SqlTransaction` objekt](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Příkazy změny dat, které strukturu transakce jsou umístěny v rámci `try...catch` bloku. Pokud dojde k chybě v příkazu v `try` blokovat spuštění přenosy do `catch` bloku, ve kterém můžete transakce vrácena zpět prostřednictvím `SqlTransaction` objektu s [ `Rollback` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Pokud všechny příkazy úspěšně dokončil, volání `SqlTransaction` objektu s [ `Commit` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) na konci `try` bloku potvrzení transakce. Následující fragment kódu ukazuje tento model. Zobrazit [zachování konzistence databáze s transakcemi](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) další syntaxe a příklady použití transakcí pomocí ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Ve výchozím nastavení nepoužívejte objekty TableAdapter v datové sadě zadán transakce. Poskytuje podporu pro transakce potřebujeme k posílení třídy TableAdapter, které chcete zahrnout další metody, které používají vyšší vzor provádět řadu příkazů úpravy dat v rámci oboru transakce. V kroku 2 uvidíme, jak používat částečné třídy pro přidání těchto metod.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Krok 1: Vytvoření práce s daty uspořádanými do dávek webové stránky

Než začneme zkoumat, jak rozšířit vrstvy DAL k podpoře databázové transakce, umožní s nejdřív využít k vytvoření webových stránek ASP.NET, které potřebujeme pro účely tohoto kurzu a tři, které následují. Začněte přidáním novou složku s názvem `BatchData` a pak přidejte následující stránky technologie ASP.NET, přidruží s každou stránku `Site.master` stránky předlohy.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Přidání stránky technologie ASP.NET pro SqlDataSource související kurzy](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Obrázek 1**: Přidání stránky technologie ASP.NET pro SqlDataSource související kurzy


Stejně jako u jiných složkách `Default.aspx` použije `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku seznam kurzů v rámci jeho části. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


A konečně, přidejte tyto čtyři stránky jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za vlastní nastavení mapy webu `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro práci s daty uspořádanými do dávek kurzy.


![Mapa webu nyní obsahuje záznamy pro práci s daty uspořádanými do dávek kurzy](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje záznamy pro práci s daty uspořádanými do dávek kurzy


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Krok 2: Aktualizuje se vrstva přístupu k datům pro podporu databázové transakce

Jak jsme probírali v prvním kurzu [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md), typová v našich DAL se skládá z datové tabulky a objekty TableAdapter. Datové tabulky obsahují data, zatímco objekty TableAdapter poskytují funkce pro čtení dat z databáze do datové tabulky k aktualizaci databáze se změnami provedenými datové tabulky a tak dále. Připomínáme, že objekty TableAdapter poskytnout dva modely pro aktualizaci dat, který uvedené jako aktualizace služby Batch a DB přímo. Se vzorkem aktualizace služby Batch je předán TableAdapter datové sady, datové tabulky nebo kolekce DataRows. Tato data je vypočten a pro každou vložit, změnit nebo odstranit řádek, `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` provádí. Se vzorkem DB přímo TableAdapter místo toho předána hodnoty sloupce, které jsou nezbytné pro vkládání, aktualizaci nebo odstranění jednoho záznamu. Tyto hodnoty předané DB přímé metody vzor pak používá ke spouštění odpovídající `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` příkazu.

Objekty TableAdapter automaticky generované metody bez ohledu na aktualizace vzor používá, nepoužívejte transakce. Ve výchozím nastavení každý insert, update nebo delete provedené objektu TableAdapter je považován za jedné diskrétní operace. Představte si například, že vzor DB přímým používá nějaký kód ve službě BLL k vložení deset záznamů do databáze. Tento kód by volat TableAdapter s `Insert` metoda desetkrát. Prvních pět vloží úspěšné, ale ten šestého způsobila výjimku, by prvních pět záznamů vložené zůstanou v databázi. Podobně, pokud vzor aktualizace služby Batch se používá k provedení operace vložení, aktualizace a odstranění do vloženého, upravit a odstranit řádky v objektu DataTable, pokud první několik změn bylo úspěšné, ale novější verzi došlo k chybě, ty dřívější změny, které dokončení by zůstat v databázi.

V některých scénářích chceme zajistit nedělitelnost napříč řadou změny. K tomu TableAdapter jsme musí ručně rozšířit přidáním nových metod, které jsou spouštěny `InsertCommand`, `UpdateCommand`, a `DeleteCommand` s pod zastřešující transakce. V [vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-cs.md) zvažovali jsme i pomocí [částečné třídy](http://en.wikipedia.org/wiki/Partial_type) rozšíření funkcí datové tabulky v datové sadě zadán. Tento postup můžete použít také pomocí instancí TableAdapter.

Datová sada typu `Northwind.xsd` se nachází v `App_Code` složku s `DAL` podsložky. Vytvořte podsložku v `DAL` složku s názvem `TransactionSupport` a přidejte nový soubor třídy s názvem `ProductsTableAdapter.TransactionSupport.cs` (viz obrázek 4). Tento soubor bude obsahovat částečnou implementaci `ProductsTableAdapter` , který obsahuje metody pro provádění změny dat pomocí transakce.


![Přidat složku s názvem TransactionSupport a soubor třídy s názvem ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Obrázek 4**: přidejte složku s názvem `TransactionSupport` a soubor třídy s názvem `ProductsTableAdapter.TransactionSupport.cs`


Zadejte následující kód do `ProductsTableAdapter.TransactionSupport.cs` souboru:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

`partial` Označuje klíčové slovo v deklaraci třídy, které jsou členy přidanými v rámci přidávaného do kompilátoru `ProductsTableAdapter` třídy v `NorthwindTableAdapters` oboru názvů. Poznámka: `using System.Data.SqlClient` příkazu v horní části souboru. Protože TableAdapter byl konfigurován pro použití zprostředkovatelem SqlClient, interně používá [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objekt vydat jeho příkazy do databáze. V důsledku toho je třeba použít `SqlTransaction` třída spustit transakci a potom ho potvrzení nebo vrácení zpět. Pokud používáte úložiště dat než Microsoft SQL Server, budete muset použít odpovídající zprostředkovatele.

Tyto metody poskytují stavební bloky potřebné ke spuštění, vrácení zpět a potvrdit transakci. Nejsou označeny jako `public`, což jim umožňuje použít v rámci `ProductsTableAdapter`z jiné třídy v vrstvy DAL a z jiné vrstvy v architektuře, jako je například BLL. `BeginTransaction` Otevře interní TableAdapter s `SqlConnection` (v případě potřeby), začíná transakce a přiřadí ji k `Transaction` vlastnost a připojí k interní transakce `SqlDataAdapter` s `SqlCommand` objekty. `CommitTransaction` a `RollbackTransaction` volání `Transaction` objektu s `Commit` a `Rollback` metody, resp. před jeho zavřením interní `Connection` objektu.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Krok 3: Přidání metody k aktualizaci a odstraňování dat v rámci zastřešující transakce

S těmito metodami kompletní jsme re jste připravení začít přidejte metody do `ProductsDataTable` nebo BLL, který vykonávat řadu příkazů v rámci zastřešující transakce. Následující metoda používá vzor aktualizace služby Batch k aktualizaci `ProductsDataTable` instance pomocí transakce. Spustí transakci voláním `BeginTransaction` metody a poté ji používá `try...catch` bloku k vydávání příkazů změny data. Pokud volání `Adapter` objektu s `Update` metodu výsledkem výjimky, provádění přenese na `catch` bloku, ve kterém transakce bude vrácena zpět a znovu vyvolána výjimka. Vzpomeňte si, že `Update` metoda implementuje vzor dávkové aktualizace vytyčením řádky zadané `ProductsDataTable` a provádění potřebných `InsertCommand`, `UpdateCommand`, a `DeleteCommand` s. Pokud některá z těchto příkazů dojde k chybě, transakce se zrušila, vrácení zpět na předchozí změny po celou dobu životnosti s transakcí. By měl `Update` příkaz dokončit bez chyby, transakce se potvrdí v celém rozsahu.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Přidat `UpdateWithTransaction` metodu `ProductsTableAdapter` třídu v rámci částečné třídy v `ProductsTableAdapter.TransactionSupport.cs`. Případně, tato metoda může být přidán do vrstvy obchodní logiky s `ProductsBLL` třída s atributem několik menších syntaktických změn. Konkrétně, klíčové slovo to `this.BeginTransaction()`, `this.CommitTransaction()`, a `this.RollbackTransaction()` by bylo potřeba nahradit `Adapter` (Vzpomeňte si, že `Adapter` je název vlastnosti v `ProductsBLL` typu `ProductsTableAdapter`).

`UpdateWithTransaction` Metoda používá vzor aktualizace služby Batch, ale řadu DB přímé volání lze také v rámci oboru transakcí, jak ukazuje následující metody. `DeleteProductsWithTransaction` Metoda přijímá jako vstup `List<T>` typu `int`, které jsou `ProductID` s odstranit. Metoda inicializuje transakci prostřednictvím volání `BeginTransaction` a pak na `try` blokovat, iteruje zadaný seznam volání vzor DB přímým `Delete` metoda pro každou `ProductID` hodnotu. Pokud je libovolná z volání `Delete` selže, je kontrola předána `catch` bloku, ve kterém transakce je vrácena zpět a znovu vyvolána výjimka. Pokud veškerá volání `Delete` úspěšné, pak je transakce potvrzena. Přidejte tuto metodu za účelem `ProductsBLL` třídy.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Použití transakce napříč více objektů TableAdapter

Transakce související kód, prozkoumat v tomto kurzu umožňuje více příkazů proti `ProductsTableAdapter` zacházeno jako atomickou operaci. Ale co když víc úpravy do jiných databázových tabulek musí provádět atomicky? Například při odstraňování kategorie, může být nejprve chceme změnit přiřazení jeho aktuální produkty do některé kategorie. Tyto dva kroky přiřazení produkty a odstraňuje se kategorie by měl být provedeny jako atomickou operaci. Ale `ProductsTableAdapter` zahrnuje pouze metody pro úpravu `Products` tabulky a `CategoriesTableAdapter` zahrnuje pouze metody pro úpravu `Categories` tabulky. Jak může zahrnovat transakce oba objekty TableAdapter?

Jednou z možností je přidání metody do `CategoriesTableAdapter` s názvem `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` a mají metody volání uložené procedury, která znovu přiřadí produktů i odstraní kategorii v rámci oboru transakce definované v rámci uložené procedury. Podíváme se na tom, jak začít, potvrzení a vrácení zpět transakcí v uložených procedurách v budoucích kurzech.

Další možností je vytvořit v DAL, který obsahuje pomocnou třídu `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` metody. Tato metoda by vytvořit instanci `CategoriesTableAdapter` a `ProductsTableAdapter` a nastavte tyto dva objekty TableAdapter `Connection` vlastnosti na stejný `SqlConnection` instance. AT tento bod jednu ze dvou objektů TableAdapter by zahájení transakce voláním `BeginTransaction`. Objekty TableAdapter metody pro přiřazení produkty a odstraňuje se kategorie by vyvolat v `try...catch` blok s transakce potvrzena nebo vrácena zpět, podle potřeby.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Krok 4: Přidání`UpdateWithTransaction`metodu pro vrstvy obchodní logiky

V kroku 3 jsme přidali `UpdateWithTransaction` metodu `ProductsTableAdapter` v vrstvy DAL. BLL jsme měli přidat odpovídající metody. Zatímco prezentační vrstva může volat přímo do vrstvy DAL k vyvolání `UpdateWithTransaction` metody tyto kurzy mají strived k definování vícevrstvou architekturu, která insulates DAL od prezentační vrstvy. Proto behooves museli tento přístup.

Otevřít `ProductsBLL` třídy soubor a přidejte metodu s názvem `UpdateWithTransaction` volající jednoduše dolů odpovídající metody vrstvy DAL. By měla nyní být dvou nových metod v `ProductsBLL`: `UpdateWithTransaction`, který jste právě přidali, a `DeleteProductsWithTransaction`, který byl přidán v kroku 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Tyto metody nejsou zahrnuté `DataObjectMethodAttribute` atribut přiřazený ke Většina metod v `ProductsBLL` třídy vzhledem k tomu, že nám budete volání těchto metod přímo z třídy modelu code-behind stránky ASP.NET. Vzpomeňte si, že `DataObjectMethodAttribute` slouží k nastavení příznaku, jaké metody by se měla objevit v prvku ObjectDataSource s zdroj dat konfigurace průvodce a na kartě jaké (SELECT, UPDATE, INSERT nebo DELETE). Protože prvku GridView. nemá žádné integrovanou podporu pro službu batch, úpravy nebo odstranění, musíme programově volat tyto metody než pomocí bez použití kódu deklarativní přístup.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Krok 5: Atomicky aktualizace databázových dat od prezentační vrstvy

Pro ilustraci vliv transakce při aktualizaci dávky záznamů, které umožňují s vytváření uživatelského rozhraní, která zobrazuje seznam všech produktů v GridView a zahrnuje webové tlačítko ovládací prvek, který, po kliknutí na znovu přiřadí produkty `CategoryID` hodnoty. Konkrétně se opětovné přiřazení kategorie průběhu tak, aby první několik produktů jsou přiřazeny platný `CategoryID` přiřazena hodnota, zatímco jiné jsou záměrně neexistující `CategoryID` hodnotu. Pokud budeme opakovat pokus o aktualizaci databáze s produktem jehož `CategoryID` neodpovídá existující kategorie s `CategoryID`, dojde k porušení omezení pro cizí klíč a bude vyvolána výjimka. Vidíme v tomto příkladu je, že při použití transakcí výjimka vyvolána v porušení omezení pro cizí klíč způsobí předchozí platné `CategoryID` změny vrátit zpět. Pokud nepoužíváte transakce, ale změny počáteční kategorie zůstane.

Začněte otevřením `Transactions.aspx` stránku `BatchData` složky a GridView přetáhněte z panelu nástrojů do návrháře. Nastavte jeho `ID` k `Products` a z inteligentních značek, jeho vazbu na nového prvku ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource přebírat jeho data ze `ProductsBLL` třída s `GetProducts` metody. To bude GridView jen pro čtení, proto nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný) a klikněte na tlačítko Dokončit.


[![Obrázek 5: Konfigurace ObjectDataSource metody GetProducts ProductsBLL třída s](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Obrázek 5**: obrázek 5: Konfigurace ObjectDataSource k použití `ProductsBLL` třída s `GetProducts` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Obrázek 6**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit záložky (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, vytvoří Visual Studio BoundFields a třídě CheckBoxField pro datová pole produktu. Odebrat všechna tato pole s výjimkou `ProductID`, `ProductName`, `CategoryID`, a `CategoryName` a přejmenovat `ProductName` a `CategoryName` BoundFields `HeaderText` vlastností produkt a kategorie, v uvedeném pořadí. Z inteligentních značek zaškrtněte možnost Povolit stránkování. Po provedení těchto změn, ovládacími prvky GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Dále přidejte tři ovládací prvky tlačítka webového nad prvku GridView. Nastavte na první tlačítko s vlastností Text k aktualizaci mřížky, druhý s upravit kategorie (transakce s) a třetí jeden s upravit kategorie (bez transakce).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

Zobrazení návrhu v sadě Visual Studio v tomto okamžiku by měla vypadat podobně jako obrazovky je vidět na obrázku 7.


[![Tato stránka obsahuje GridView a tři webové ovládací prvky tlačítek](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Obrázek 7**: Tato stránka obsahuje GridView a tři webové ovládací prvky tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Vytváření obslužných rutin událostí pro všechny tři tlačítka s `Click` události a pomocí následujícího kódu:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

Aktualizace tlačítko s `Click` obslužná rutina události je znovu jednoduše připojí data do prvku GridView voláním `Products` GridView s `DataBind` metody.

Druhá obslužná rutina události znovu přiřadí produkty `CategoryID` s a použije metodu nové transakce z knihoven BLL provádět databáze aktualizuje pod zastřešující transakce. Všimněte si, že každý produkt s `CategoryID` libovolně nastavena na stejnou hodnotu jako jeho `ProductID`. To bude fungovat pro první jen málo produktů, protože tyto produkty mají `ProductID` hodnoty, ke kterým dochází k mapování na platný `CategoryID` s. Ale jednou `ProductID` spuštění s začíná být moc velká, tento náhodný překrytí `ProductID` s a `CategoryID` s už neplatí.

Třetí `Click` obslužná rutina události aktualizace produktů `CategoryID` s stejným způsobem, ale pošle aktualizace do databáze pomocí `ProductsTableAdapter` s výchozí `Update` metody. To `Update` metoda nezalamuje řadu příkazů v rámci transakce, tak tyto změny před první chybou porušení došlo k omezení cizího klíče se zachová.

Chcete-li toto chování ilustrují, navštivte tuto stránku prostřednictvím prohlížeče. Původně měli byste vidět první stránka dat, jak je znázorněno na obrázku 8. V dalším kroku klikněte na tlačítko Upravit kategorie (transakce s). To způsobí zpětné odeslání a pokus o aktualizaci všech produktů, které `CategoryID` hodnoty, ale bude mít za následek porušení omezení pro cizí klíč (viz obrázek 9).


[![Produkty jsou zobrazeny v stránkované GridView](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Obrázek 8**: The produkty jsou zobrazeny v stránkované ovládacího prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Přiřazení kategorie výsledků v narušení omezení pro cizí klíč](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Obrázek 9**: přiřazení kategorie výsledků v porušení omezení cizího klíče ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Nyní stiskněte tlačítko zpět váš prohlížeč s a pak klikněte na tlačítko Aktualizovat čar mřížky. Při aktualizaci dat byste měli vidět přesně stejný výstup, jak je znázorněno na obrázku 8. To znamená, i když některé z produktů `CategoryID` s byly změněné na právní hodnoty a aktualizována v databázi, že byly vráceny zpět, když došlo k porušení omezení pro cizí klíč.

Zkuste to teď kliknutím na tlačítko Upravit kategorie (bez transakce). Výsledkem bude stejná chyba porušení omezení pro cizí klíč (viz obrázek 9), ale v tuto chvíli tyto produkty jehož `CategoryID` hodnoty byly změněny na právní hodnotu, nebudou vráceny zpět. Stiskněte v prohlížeči s tlačítko Zpět a potom na tlačítko Aktualizovat mřížky. Obrázek 10 ukazuje, `CategoryID` s produkty prvních osm byl znovu přiřazen. Například na obrázku 8, měl Chang `CategoryID` 1, ale v obrázek 10 it s změnilo na 2.


[![Některé hodnoty ID kategorie produktů nebyly aktualizovány při jiné byly](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Obrázek 10**: některé produkty `CategoryID` hodnoty nebyly aktualizovány při jiné byly ([kliknutím ji zobrazíte obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Souhrn

Ve výchozím nastavení metod TableAdapter s nezalamují příkazy spuštěné databáze v rámci oboru transakce, ale trochu práce můžeme přidat metody, které se vytvoří, potvrzení a vrátit zpět transakci. V tomto kurzu jsme vytvořili v těchto tří metod `ProductsTableAdapter` třídy: `BeginTransaction`, `CommitTransaction`, a `RollbackTransaction`. Jsme viděli, jak používat tyto metody společně s `try...catch` blok atomic provádět řadu příkazů změny data. Konkrétně jsme vytvořili `UpdateWithTransaction` metodu `ProductsTableAdapter`, která používá vzor dávkové aktualizace provést potřebné změny, které řádky zadaného `ProductsDataTable`. Přidali jsme také `DeleteProductsWithTransaction` metodu `ProductsBLL` třídy v BLL, který dokáže akceptovat nezadání `List` z `ProductID` hodnoty jako vstup a volá metodu vzor DB přímo `Delete` pro každou `ProductID`. Obě metody, začněte tím, že vytváření transakcí a spouštěním příkazů úpravy dat v rámci `try...catch` bloku. Pokud dojde k výjimce, transakce se zrušila, jinak nebude potvrzena změna.

Krok 5 ukázal efekt transakční dávkové aktualizace a aktualizace služby batch, které opominul použití transakcí. V následujících třech kurzech budeme vycházejí z základ, podle tohoto kurzu a vytvořit uživatelské rozhraní pro provádění dávkových aktualizací, odstranění a vložení.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Zachování konzistence databáze s transakcemi](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Správa transakcí v systému SQL Server uložené procedury](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transakce provedené snadno: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [Objekt TransactionScope a adaptérů dat](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Použití Oracle databázové transakce v rozhraní .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Dave Gardner Hilton Giesenow a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](batch-updating-cs.md)
