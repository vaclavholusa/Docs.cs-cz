---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Zabalení změny databáze v rámci transakce (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu je první čtyři, který zjistí aktualizaci, odstranění a vložení dávky data. V tomto kurzu jsme zjistěte, jak povolit databázové transakce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: a3f8ec2de7b9259e4bb83f4346bde8abfd643fb4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Zabalení změny databáze v rámci transakce (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) nebo [stáhnout PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> V tomto kurzu je první čtyři, který zjistí aktualizaci, odstranění a vložení dávky data. V tomto kurzu jsme zjistěte, jak povolit databázové transakce batch změny prováděné jako atomickou operaci, která zajistí, že buď všechny kroky úspěch nebo neúspěch všechny kroky.


## <a name="introduction"></a>Úvod

Jak jsme viděli začínající [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) kurzu GridView poskytuje integrovanou podporu pro úpravy na úrovni řádků a odstranění. Pomocí několika kliknutí myší je možné vytvořit rozhraní úpravy bohaté dat bez nutnosti psaní řádek kódu, tak dlouho, dokud se obsah s úpravy a odstraňování na základě na řádek. Ale v některých případech to není dostatečné a je potřeba uživatelům poskytnout možnost upravit nebo odstranit dávky záznamů.

Například většina webových e-mailové klienty pomocí mřížky seznam každou zprávu kde každý řádek obsahuje zaškrtávací políčko společně s informacemi s e-mailu (předmět, odesílatele a tak dále). Toto rozhraní umožňuje uživatelům odstranit více zpráv tak, že je a potom klikněte na tlačítko Odstranit vybrané zprávy. Úpravy rozhraní batch je ideální v situacích, kde uživatelé běžně upravovat mnoho různých záznamů. Místo vynucení uživateli klikněte na tlačítko Upravit, proveďte jejich změnu a pak kliknutím na tlačítko Aktualizovat pro každý záznam, který má být změněn, dávce úpravy rozhraní vykreslí každý řádek s jeho úpravy rozhraní. Uživatel můžete rychle změnit sadu řádků, které se musí změnit a poté tyto změny uložit kliknutím tlačítko Aktualizovat vše. V této sadě kurzy podíváme, jak vytvořit rozhraní pro vkládání, úpravy a odstraňování dávky data.

Při provádění operací batch ho s důležité určit, zda by mělo být možné pro některé operace v dávce úspěšné ostatní při selhání. Vezměte v úvahu dávky odstraňování rozhraní – které dojde, pokud byla úspěšně odstraněna první vybraný záznam, ale druhý selže, můžete, kvůli narušení omezení pro cizí klíč? Musí být první odstranění záznamu s vrácena zpět nebo je přijatelné pro první záznam zůstat odstraněné?

Pokud chcete, aby dávkové operace jsou považovány za [atomické operace](http://en.wikipedia.org/wiki/Atomic_operation), jeden kde buď všechny kroky úspěšné, nebo všechny kroky nezdaří a Data Access Layer musí být rozšíření, zahrnují podporu pro [databáze transakce](http://en.wikipedia.org/wiki/Database_transaction). Databázové transakce zaručit nedělitelnost pro sadu `INSERT`, `UPDATE`, a `DELETE` příkazy provést pod pojmem transakce a jsou funkce podporuje většinu systémy moderní databáze.

V tomto kurzu budeme zabývat postup rozšíření DAL použít databázové transakce. Další kurzy prozkoumá implementující webové stránky pro dávkové vložení, aktualizace a odstranění rozhraní. Umožňují s začít!

> [!NOTE]
> Při změně dat v transakci batch, není potřeba vždy nedělitelnost. V některých případech může být přijatelná některé změny dat úspěšné a jiné v stejné dávce selže, jako např. kdy odstranění sadu e-mailů z klienta e-mailové zprávy. Pokud je tam s zpracování chyb databáze se v polovině prostřednictvím odstranění, ho s pravděpodobně přijatelné, zůstaly odstraněné záznamy zpracovat bez chyby. V takových případech DAL nemusí být upravena pro podporu databázové transakce. Existují další batch operaci scénáře, ale kde nedělitelnost je životně důležité. Pokud zákazník přesune jeho prostředků z jednoho účtu bank na jiný, musí být provedeny dvě operace: fondů musí být odečtena od první účet a pak přidá do druhého. Banka nemusí paměti, že prvním krokem úspěšné, ale druhý krok nezdaří, může být svým zákazníkům understandably nespokojený. I doporučujeme absolvování tohoto kurzu a implementovat vylepšení podpory transakcí databáze i v případě, že neplánujete na jejich používání v dávkového vložení, aktualizace a odstranění rozhraní, které jsme budete vytvářet v následujících kurzech tři vrstvy Dal.


## <a name="an-overview-of-transactions"></a>Přehled transakcí

Většina databáze zahrnují podporu pro *transakce*, která umožňují více příkazů databáze byly seskupeny do jedné logické jednotky práce. Databáze příkazy, které tvoří transakce je zaručeno bylo atomické, což znamená, že buď všechny příkazy selžou nebo všechny bude úspěšné.

Obecně platí transakce jsou implementovány pomocí příkazů SQL pomocí následujícího vzorce:

1. Upozornění na zahájení transakce.
2. Spouštění příkazů jazyka SQL, které tvoří transakce.
3. Pokud dojde k chybě v jednom z příkazy z kroku 2, vrácení transakce.
4. Pokud všechny příkazy z kroku 2 dokončeno bez chyb, potvrzení transakce.

Příkazy SQL použít k vytvoření, potvrzení a vrácení transakce lze zadat ručně při psaní skriptů SQL nebo vytváření uložené procedury nebo prostřednictvím programový znamená pomocí ADO.NET nebo třídy v [ `System.Transactions` obor názvů](https://msdn.microsoft.com/library/system.transactions.aspx). V tomto kurzu pouze vyzkoušíme Správa transakcí použitím technologie ADO.NET. V budoucích kurzu se podíváme na tom, jak pomocí uložených procedur v Data Access Layer, po kterých jsme budete prozkoumat příkazy SQL pro vytváření, vrácení zpět a potvrzením transakce. Do té doby najdete [Správa transakcí v SQL serveru uložené procedury](http://www.4guysfromrolla.com/webtech/080305-1.shtml) Další informace.

> [!NOTE]
> [ `TransactionScope` Třída](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) v `System.Transactions` obor názvů umožňuje vývojářům prostřednictvím kódu programu zalomit řadu příkazy v oboru transakce a zahrnuje podporu pro komplexní transakce, které zahrnují více zdrojů, například dvou různých databází nebo dokonce heterogenní typy úložiště dat, jako jsou databáze Microsoft SQL Server, databáze Oracle a webové služby. I jste se rozhodli použít ADO.NET transakce v tomto kurzu místo `TransactionScope` třída protože ADO.NET je specifičtější pro databázové transakce a v mnoha případech je mnohem méně náročné. Kromě toho, v některých případech `TransactionScope` třída používá Coordinator služby (Microsoft Distributed transakci MSDTC). Konfigurace, implementaci a výkonu problémům okolního služby MSDTC je téma místo specializované a pokročilých a nad rámec těchto kurzů.


Při práci s poskytovatel Sqlclienta v ADO.NET, transakce se spouští pomocí volání [ `SqlConnection` třída](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), která vrátí [ `SqlTransaction` objekt](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Příkazy úpravy dat umístěných v rámci způsob vytvoření transakce `try...catch` bloku. Pokud dojde k chybě v příkazu v `try` blokovat spuštění přenosů, aby se `catch` bloku, kde můžete transakce vrácena zpět prostřednictvím `SqlTransaction` objekt s [ `Rollback` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Pokud všechny příkazy úspěšně dokončil, volání `SqlTransaction` objekt s [ `Commit` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) na konci `try` bloku potvrdí transakce. Následující fragment kódu ukazuje tento vzor. V tématu [zachování konzistence databáze s transakcemi](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) pro další syntaxe a příklady použití transakcí s ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Ve výchozím nastavení nepoužívejte TableAdapters v datové sadě zadali transakce. Kvůli zajištění podpory pro transakce musíme posílení třídy TableAdapter zahrnout další metody, které používají výše vzorec k provedení řady příkazy Úpravy dat v rámci oboru transakce. V kroku 2 vidíte použití částečné třídy k přidání těchto metod.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Krok 1: Vytvoření pracovní s dávkové Data webové stránky

Než začneme zkoumat postupy k posílení DAL pro podporu databázové transakce, umožní s nejprve vytvořit webových stránek ASP.NET, které je nutné zadat pro účely tohoto kurzu a tři, které budou následovat chvíli trvat. Nejprve přidejte novou složku s názvem `BatchData` a poté přidejte následující stránky ASP.NET přidružení každou stránku s `Site.master` stránky předlohy.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Přidání stránky ASP.NET pro kurzy související SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Obrázek 1**: Přidání stránky ASP.NET pro kurzy související SqlDataSource


Stejně jako u jiných složkách `Default.aspx` použije `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek seznam kurzy v příslušném oddílu. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Nakonec přidejte tyto čtyři stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po vlastní nastavení mapy webu `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro práci s kurzy dávkové data.


![Mapy webu nyní obsahuje položky pro práci s kurzy dávkové dat](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje položky pro práci s kurzy dávkové dat


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Krok 2: Aktualizace Data Access Layer pro podporu databázové transakce

Jak již bylo zmíněno zpět v první kurz [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-cs.md), typové datové sady v našem DAL se skládá z DataTables a TableAdapters. DataTables obsahovat data, zatímco TableAdapters poskytují funkce pro čtení dat z databáze do DataTables, chcete-li aktualizovat databázi s změny provedené DataTables a tak dále. Odvolat, že TableAdapters poskytovat dva vzory pro aktualizace dat, který uvedené jako dávková aktualizace a DB-Direct. S vzoru dávková aktualizace je předán TableAdapter datovou sadu, DataTable nebo kolekce DataRows. Tato data je výčet a pro každou vložit, upravit nebo odstranit řádek, `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` se spustí. S použitím vzoru názvů DB přímo TableAdapter místo toho předá hodnot sloupců, které jsou nezbytné pro vložení, aktualizaci nebo odstranění jeden záznam. DB přímá metoda vzor použije tyto hodnoty předané provést odpovídající `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` příkaz.

Bez ohledu na aktualizace vzoru používá automaticky generovaný metody TableAdapters nepoužívejte transakce. Ve výchozím nastavení je každý insert, update nebo delete provádí TableAdapter považována za jedné diskrétní operace. Představte si například, že nějaký kód v BLL používá vzor DB přímo k vložit deset záznamů do databáze. Tento kód by volání TableAdapter s `Insert` metoda desetkrát. Pokud prvních pět vložení úspěšné, ale ten šesté způsobil výjimku, prvních pět vložené záznamy by zůstat v databázi. Podobně pokud vzor dávková aktualizace se používá k provádění vložení, aktualizace a odstranění na vložené, upravit a odstranit řádků v DataTable, pokud první několik změn bylo úspěšné, ale novější verzi došlo k chybě, tyto starší úpravy, Dokončit by zůstat v databázi.

V některých scénářích chceme, aby byla zajištěna nedělitelnost celé řady úpravy. K tomu TableAdapter jsme musí ručně rozšířit přidáním nových metod, které jsou spouštěny `InsertCommand`, `UpdateCommand`, a `DeleteCommand` s pod pojmem transakce. V [vytváření Data Access Layer](../introduction/creating-a-data-access-layer-cs.md) jsme se podívali na pomocí [částečné třídy](http://en.wikipedia.org/wiki/Partial_type) rozšířit funkce DataTables v rámci typové datové sady. Tento postup můžete použít taky s TableAdapters.

Typové datové sady `Northwind.xsd` se nachází v `App_Code` složku s `DAL` podsložky. Vytvořit podsložku v `DAL` složku s názvem `TransactionSupport` a přidejte nový soubor třídy s názvem `ProductsTableAdapter.TransactionSupport.cs` (viz obrázek 4). Tento soubor bude obsahovat částečné implementace `ProductsTableAdapter` , který obsahuje metody pro provedení změny dat pomocí transakce.


![Přidat složku s názvem TransactionSupport a soubor třídy s názvem ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Obrázek 4**: Přidat složku s názvem `TransactionSupport` a soubor třídy s názvem `ProductsTableAdapter.TransactionSupport.cs`


Zadejte následující kód do `ProductsTableAdapter.TransactionSupport.cs` souboru:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

`partial` – Klíčové slovo v deklaraci třídy označuje kompilátoru, které jsou členy přidat do mají být přidány do `ProductsTableAdapter` třídy v `NorthwindTableAdapters` oboru názvů. Poznámka: `using System.Data.SqlClient` příkaz v horní části souboru. Vzhledem k tomu, že TableAdapter byla nakonfigurovaná pro použití poskytovatele SqlClient, interně používá [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objekt, který chcete vydat jeho příkazů do databáze. V důsledku toho je potřeba použít `SqlTransaction` třída zahájíte transakce a pak ji zapište nebo vrátit zpět. Pokud používáte úložiště dat než ze systému Microsoft SQL Server, budete muset použít příslušného poskytovatele.

Tyto metody poskytují stavební bloky potřebné ke spuštění, vrácení zpět a potvrzení transakce. Jsou označeny `public`, je pro použití v rámci povolení `ProductsTableAdapter`, z jiné třídy v DAL, nebo z jiné vrstvy architektury, jako je například BLL. `BeginTransaction` Otevře interní TableAdapter s `SqlConnection` (v případě potřeby), začne transakce a přiřadí ji k `Transaction` vlastnost a připojí k interní transakce `SqlDataAdapter` s `SqlCommand` objekty. `CommitTransaction` a `RollbackTransaction` volání `Transaction` objekt s `Commit` a `Rollback` metody, v uvedeném pořadí, před jeho zavřením interní `Connection` objektu.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Krok 3: Přidání metody pro aktualizace a odstranění dat pod pojmem transakce

Pomocí těchto metod dokončení jsme re připraveni přidejte metody do `ProductsDataTable` nebo BLL, které provádějí řadu příkazů pod pojmem transakce. Následující metodu vzoru aktualizovat Batch používá k aktualizaci `ProductsDataTable` instance pomocí transakce. Spuštění transakce voláním `BeginTransaction` metoda a pak používá `try...catch` bloku k vydávání dat úpravy příkazy. Pokud volání `Adapter` objekt s `Update` metoda za následek výjimku, provádění bude přenášet do `catch` blok, kde transakce bude vrácena zpět a znovu došlo k výjimce. Odvolat, který `Update` metoda implementuje vzor dávková aktualizace vytyčením řádky zadaných `ProductsDataTable` a provádění nezbytné `InsertCommand`, `UpdateCommand`, a `DeleteCommand` s. Pokud některá z těchto příkazů dojde k chybě, transakce je vrácena zpět, vrátí zpět předchozí úpravy provedené v průběhu životního cyklu s transakce. Má `Update` příkaz dokončeno bez chyb, transakce se potvrdí jako celek.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Přidat `UpdateWithTransaction` metodu `ProductsTableAdapter` třídu v rámci třídu v `ProductsTableAdapter.TransactionSupport.cs`. Případně, tato metoda nebylo možné přidat do vrstvu obchodní logiky s `ProductsBLL` se několik syntaktických mírně. Konkrétně, klíčové slovo v `this.BeginTransaction()`, `this.CommitTransaction()`, a `this.RollbackTransaction()` by bylo potřeba nahradí `Adapter` (který odvolat `Adapter` je název vlastnosti v `ProductsBLL` typu `ProductsTableAdapter`).

`UpdateWithTransaction` Metoda používá vzor dávková aktualizace, ale řadu DB přímé volání lze také v rámci oboru transakce, jak ukazuje následující metoda. `DeleteProductsWithTransaction` Metoda přijímá jako vstup `List<T>` typu `int`, které jsou `ProductID` s odstranit. Metoda inicializuje transakci prostřednictvím volání `BeginTransaction` a pak na `try` blokovat, prochází seznamu zadaný vzor DB přímé volání `Delete` metoda pro každou `ProductID` hodnotu. Pokud žádné z volání `Delete` řízení selže, se převede do `catch` blok, kde transakce je vrácena zpět a znovu došlo k výjimce. Pokud veškerá volání `Delete` úspěšné, pak je transakce potvrzena. Přidejte tuto metodu za účelem `ProductsBLL` třídy.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Provádění transakcí v celém více objektů TableAdapters

Kód související transakci zkontrolován v tomto kurzu umožňuje více příkazů proti `ProductsTableAdapter` považován za atomické operace. Ale co když provádění více úprav jiných databázových tabulek musí být provedeny atomicky? Například při odstraňování kategorii, může první chceme přiřazení jeho aktuální produktů na některé další kategorie. Tyto dva kroky přeřazení produkty a odstraňování kategorii musí provést, protože atomické operace. Ale `ProductsTableAdapter` obsahuje pouze metody pro úpravu `Products` tabulky a `CategoriesTableAdapter` obsahuje pouze metody pro úpravu `Categories` tabulky. Jak tedy můžete zahrnovat transakce obou TableAdapters?

Jednou z možností je přidání metody do `CategoriesTableAdapter` s názvem `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` a mít dané metody volání uložené procedury, která znovu přiřadí produkty i odstraní kategorii v rámci oboru transakce definované v rámci uložené procedury. Podíváme tom, jak začít, potvrzení a vrácení transakce v uložené procedury v budoucnu kurzu.

Další možností je vytvořit v DAL, který obsahuje pomocnou třídu `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` metoda. Tato metoda by vytvořit instanci `CategoriesTableAdapter` a `ProductsTableAdapter` a nastavte tyto dvě TableAdapters `Connection` vlastnosti na stejnou `SqlConnection` instance. Na tento bod kterékoli dvě TableAdapters by zahájení transakce s volání `BeginTransaction`. TableAdapters metody pro přeřazení produkty nebo kategorie odstranění by být volána v `try...catch` blok s transakce potvrzena nebo vrácena zpět podle potřeby.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Krok 4: Přidání`UpdateWithTransaction`metodu pro vrstvu obchodní logiky

V kroku 3 jsme přidali `UpdateWithTransaction` metodu `ProductsTableAdapter` v DAL. Jsme měli přidat odpovídající metodu BLL. Při prezentační vrstvy může volat přímo do vrstvy DAL k vyvolání `UpdateWithTransaction` metoda, tyto kurzy mít strived k definování vrstveného architekturu, která insulates DAL od prezentační vrstvy. Proto behooves nám pokračujte tento přístup.

Otevřete `ProductsBLL` třídy souboru a přidejte metodu s názvem `UpdateWithTransaction` této jednoduše volání dolů odpovídající metoda DAL. Měla by existovat teď dvě nové metody v `ProductsBLL`: `UpdateWithTransaction`, který jste právě přidali, a `DeleteProductsWithTransaction`, který byl přidán v kroku 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Tyto metody nezahrnují `DataObjectMethodAttribute` přiřazené většinu metod v atributu `ProductsBLL` třídy, protože jsme budete vyvolání tyto metody přímo z třídy kódu stránky ASP.NET. Odvolat, `DataObjectMethodAttribute` se používá k příznak, jaké metody by se měla objevit v ObjectDataSource s zdroj dat konfigurace průvodce a jaké kartě (SELECT, UPDATE, INSERT nebo DELETE). Vzhledem k tomu, že GridView nemá žádné integrovanou podporu pro dávku úpravy nebo odstranění, jsme budete muset tyto metody vyvolání prostřednictvím kódu programu, místo použití bez kódu deklarativní přístup.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Krok 5: Atomicky aktualizace dat databáze od prezentační vrstvy

Pro ilustraci o tom, že transakce má při aktualizaci dávky záznamů, umožňují s vytvoření uživatelského rozhraní, které jsou uvedeny všechny produkty v GridView a zahrnují tlačítko webu řídit, která po kliknutí na pouze Změna přiřazení produkty `CategoryID` hodnoty. Konkrétně, opětovné přiřazení kategorie proběhne tak, aby první několik produkty jsou přiřazeny platná `CategoryID` hodnotu, zatímco ostatní záměrně jsou přiřazené neexistující `CategoryID` hodnotu. Pokud jsme pokus o aktualizaci databáze s produktem, s jehož `CategoryID` neodpovídá existující kategorii s `CategoryID`, dojde k porušení omezení cizího klíče a k výjimce. Co jsme najdete v tomto příkladu je, že při použití transakce výjimky vyvolané z porušení omezení pro cizí klíč způsobí předchozí platné `CategoryID` změny vrátit zpět. Pokud nepoužíváte transakce, ale změny do počáteční kategorií zůstane.

Začněte otevřením `Transactions.aspx` stránku `BatchData` složku a přetáhněte GridView z panelu nástrojů na návrháře. Nastavte její `ID` k `Products` a z jeho inteligentních značek navázat jej na nové ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource načítat data z `ProductsBLL` třídu s `GetProducts` metoda. Tato akce se GridView jen pro čtení, tak nastavte rozevírací seznamy v aktualizaci, vložení a odstraní karty na (žádný) a klikněte na tlačítko Dokončit.


[![Obrázek 5: Konfigurace ObjectDataSource lze pomocí této metody GetProducts ProductsBLL třídu s](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Obrázek 5**: obrázek 5: Konfigurace ObjectDataSource pro použití `ProductsBLL` třídu s `GetProducts` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Obrázek 6**: rozevíracího seznamu jsou uvedeny ve UPDATE, INSERT a odstranit karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Po dokončení průvodce Konfigurace zdroje dat se vytvoří sada Visual Studio BoundFields a vlastnost CheckBoxField pro produkt datová pole. Odeberte všechna tyto pole s výjimkou `ProductID`, `ProductName`, `CategoryID`, a `CategoryName` a přejmenujte `ProductName` a `CategoryName` BoundFields `HeaderText` vlastnosti produktu a kategorie, v uvedeném pořadí. Ze inteligentní značky zaškrtnutí políčka Povolit stránkování. Po provedení změny, rutina GridView a ObjectDataSource s deklarativní by měl vypadat následovně:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Dál přidejte tři ovládací prvky webového tlačítko výše GridView. Na první tlačítko s Text – vlastnost nastavena na aktualizaci mřížky, druhý s do upravit kategorií (transakce s) a třetí jeden s do upravit kategorií (bez transakce).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

Zobrazení návrhu v sadě Visual Studio v tomto okamžiku by mělo vypadat jako na uvedené na obrázku 7 snímku obrazovky.


[![Tato stránka obsahuje GridView a ovládací prvky webového tři tlačítka](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Obrázek 7**: Tato stránka obsahuje GridView a tři ovládací prvky webového tlačítko ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Vytváření obslužných rutin událostí pro všechny tři tlačítka s `Click` události a použijte následující kód:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

Aktualizace tlačítko s `Click` obslužné rutiny události znovu data, která mají GridView jednoduše připojí voláním `Products` GridView s `DataBind` metoda.

Druhá obslužná rutina události pouze Změna přiřazení produkty `CategoryID` s a použije metodu nové transakce z BLL k provedení databáze aktualizuje pod pojmem transakce. Všimněte si, že každý produkt s `CategoryID` nahodile nastavena na stejnou hodnotu jako jeho `ProductID`. To se bez problémů fungují pro první několik produkty, vzhledem k tomu, že mají tyto produkty `ProductID` hodnoty, které mají na platné `CategoryID` s. Ale jednou `ProductID` s počáteční získání příliš velký, tento náhodný překrytí `ProductID` s a `CategoryID` s již nepoužívá.

Třetí `Click` obslužné rutiny události aktualizace produktů `CategoryID` s stejným způsobem, ale odešle aktualizace do databáze pomocí `ProductsTableAdapter` s výchozí `Update` metoda. To `Update` metoda nezalamuje řadu příkazů v rámci transakce, tak před první chyba porušení omezení cizího klíče došlo k provedení těchto změn bude zachována.

Chcete-li toto chování ilustrují, navštivte tuto stránku prostřednictvím prohlížeče. Nejdřív měli vidět na první stránku dat, jak je znázorněno na obrázku 8. Potom klikněte na tlačítko Upravit kategorií (transakce s). Tato akce způsobí zpětné volání a pokus o aktualizaci všechny produkty `CategoryID` hodnoty, ale bude mít za následek narušení omezení cizího klíče (viz obrázek 9).


[![Produkty jsou zobrazeny v stránkovatelné GridView](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Obrázek 8**: produkty jsou zobrazeny v stránkovatelné GridView ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Opětovné přiřazování kategorií výsledky v narušení omezení pro cizí klíč](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Obrázek 9**: opětovné přiřazování kategorií výsledky v narušení omezení pro cizí klíč ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Nyní stiskněte váš prohlížeč s tlačítko Zpět a potom klikněte na tlačítko Aktualizovat mřížky. Při aktualizaci dat byste měli vidět přesně stejný výstup, jak je znázorněno na obrázku 8. To znamená, i když některé produkty `CategoryID` s byly změněné na právní hodnoty a aktualizována v databázi, že byly vráceny zpět v případě, že došlo k porušení omezení pro cizí klíč.

Zkuste nyní klepnutím na tlačítko Upravit kategorií (bez transakce). Tato akce způsobí ke stejné chybě porušení omezení pro cizí klíč (viz obrázek 9), ale v tuto chvíli těchto produktů jejichž `CategoryID` hodnoty byly změněny na právního hodnota nebude vrácena zpět. Stiskněte váš prohlížeč s tlačítko Zpět a potom na tlačítko Aktualizovat mřížky. Jak ukazuje obrázek 10, `CategoryID` s produkty prvních 8 byl znovu přiřazen. Například na obrázku 8, měl změn `CategoryID` 1, ale v obrázek 10 it s byla přeřazena na 2.


[![Některé hodnoty CategoryID produkty nebyly aktualizovány při jiné byly](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Obrázek 10**: některá produkty `CategoryID` hodnoty nebyly aktualizovány při jiné byly ([Kliknutím zobrazit obrázek v plné velikosti](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Souhrn

Ve výchozím nastavení metod TableAdapter s není zalomen příkazy spuštění databáze v rámci oboru transakce, ale s malým množstvím pracovní jsme přidejte metody, které se vytvoří, potvrzení a vrácení transakce. V tomto kurzu jsme vytvořili v těchto tří metod `ProductsTableAdapter` – třída: `BeginTransaction`, `CommitTransaction`, a `RollbackTransaction`. Jsme viděli, jak používat tyto metody spolu s `try...catch` blok atomic aby řadu dat úpravy příkazy. Konkrétně jsme vytvořili `UpdateWithTransaction` metoda v `ProductsTableAdapter`, který používá vzor dávková aktualizace provést požadované změny zadané řádky `ProductsDataTable`. Jsme přidali i `DeleteProductsWithTransaction` metodu `ProductsBLL` třídy v BLL, které přijímá `List` z `ProductID` hodnoty jako vstup a volá metodu vzor DB-Direct `Delete` pro každou `ProductID`. Obě metody spuštění vytvořením transakci a pak provádění příkazů úpravy dat v rámci `try...catch` bloku. Pokud dojde k výjimce transakce je vrácena zpět, v opačném případě je potvrzená.

Krok 5 zobrazené účinku transakční dávce aktualizace versus batch aktualizace, které nevrátil použít transakce. V následujících třech kurzech jsme stavějí foundation umístěné v tomto kurzu a vytvoření uživatelského rozhraní pro provádění dávková aktualizace, odstranění a vložení.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Zachování konzistence databáze s transakce](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Správa transakcí v systému SQL Server uložené procedury](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transakce, umožněno: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope a DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Použití Oracle databázové transakce v rozhraní .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Dave Gardner Hilton Giesenow a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](batch-updating-cs.md)
