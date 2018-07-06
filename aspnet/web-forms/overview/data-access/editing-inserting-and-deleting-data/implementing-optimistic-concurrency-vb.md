---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementace optimistického řízení souběžnosti (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Pro webovou aplikaci, která umožňuje více uživatelům upravovat data existuje riziko, že dva uživatelé mohou upravovat stejná data ve stejnou dobu. V tomto tutori...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 63b5a274103851b4b60c92d5fe46125cc4a1b0be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832963"
---
<a name="implementing-optimistic-concurrency-vb"></a>Implementace optimistického řízení souběžnosti (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) nebo [stahovat PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Pro webovou aplikaci, která umožňuje více uživatelům upravovat data existuje riziko, že dva uživatelé mohou upravovat stejná data ve stejnou dobu. V tomto kurzu Implementujeme optimistické řízení souběžného zpracování toto riziko.


## <a name="introduction"></a>Úvod

Pro webové aplikace, které jenom povolit uživatelům zobrazit data, nebo pro ty, které zahrnují jenom jeden uživatel, který lze upravovat data neexistuje žádné hrozby dvou souběžných uživatelů náhodnému přepsání nepřípustným změny. Pro webové aplikace, které umožňují více uživatelům aktualizovat nebo odstranit data ale není potenciál pro jednoho uživatele změny nebudou kolidovat s jiných souběžných uživatelů. Bez jakékoli zásady souběžnosti na místě když dva uživatele současně upravujete jeden záznam uživatele, který potvrdí její změny poslední přepíše změny provedené při první.

Představte si například, že dva uživatelé, Jisun a Sam, byly oba navštívit stránku v naší aplikaci, která návštěvníci, aktualizovat a odstraňovat produktům prostřednictvím ovládacího prvku GridView. Obě klikněte na tlačítko Upravit v prvku GridView. přibližně ve stejnou dobu. Jisun název produktu se změní na "Chai čaje" a klikne na tlačítko Aktualizovat. Net výsledek je `UPDATE` příkaz, který je odeslán do databáze, která nastavuje hodnoty *všechny* aktualizovatelné polí produktu (i když Jisun aktualizovat jenom jedno pole `ProductName`). V tomto okamžiku databáze má hodnoty "Chai kávy," do kategorie Nápoje dodavatele exotické tekutin a podobně pro tento konkrétní produkt. Však GridView na obrazovce pro Sam stále ukazovat název produktu v řádku prvku GridView upravitelné "Chai". Několik sekund poté, co Jisun změny byly potvrzeny, Sam aktualizuje kategorii produkty koření a klikne na tlačítko Aktualizovat. Výsledkem je `UPDATE` příkaz odeslán do databáze, který nastaví název produktu a "Chai" `CategoryID` na odpovídající ID kategorie Nápoje a tak dále. Být přepsán Jisun na změny v názvu produktu. Obrázek 1 graficky znázorňuje Tato série událostí.


[![Když dva uživatele současně aktualizace záznamu existovat s potenciál pro jednoho uživatele změny přepsat druhé strany](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Obrázek 1**: když dva uživatele současně aktualizace existuje záznam s potenciál pro jeden uživatel změní na přepsání druhé strany ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image3.png))


Podobně když dva uživatelé navštěvují stránku, jeden uživatel může být uprostřed aktualizaci záznamu, když je odstraněn jiným uživatelem. Nebo až když uživatel načte stránku kliknutím na tlačítko pro odstranění, změněn jiným uživatelem mohou mít obsah daného záznamu.

Existují tři [řízení souběžnosti](http://en.wikipedia.org/wiki/Concurrency_control) strategií, které jsou k dispozici:

- **Neprovádět žádnou akci** – Pokud souběžnými uživateli upravovat stejný záznam, dejte posledního potvrzení win (výchozí nastavení)
- [**Optimistická souběžnost** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -předpokládat, i když to může být souběžnosti je v konfliktu every teď nebo později, většinu času takové nedojde ke konfliktu; proto, pokud vzniknout konflikt, jednoduše informovat uživatele, který jejich změny nelze uložit, protože jiný uživatel upravil stejná data
- **Pesimistická souběžnost** -předpokládají, že je v konfliktu souběžnosti běžné a uvedli, že uživatelé nebudou tolerovat vrácení jako jejich změny nebyly uloženy kvůli souběžnou aktivitu jiným uživatelem; proto, když se jeden uživatel spustí aktualizaci záznamu, uzamkne tak, aby , a tím brání ostatní uživatelé z úpravy nebo odstranění záznamu, dokud uživatel potvrdí jejich úpravy

Všech našich kurzů pro jaký jste použili výchozí strategie řešení souběžnosti – konkrétně, nám sdělili jste poslední zápis vyhrát. V tomto kurzu prozkoumáme implementace optimistického řízení souběžnosti.

> [!NOTE]
> Podíváme se na Pesimistická souběžnost příklady v této sérii kurzů. Pesimistická souběžnost se používá zřídka, protože tyto zámky, není-li správně uvolní, když to, můžete zabránit aktualizace dat jiným uživatelům. Například pokud uživatel uzamkne záznam pro úpravy a pak opustí den odemknutí ji, žádný jiný uživatel bude moct aktualizovat dokud původní uživatel vrátí a dokončí jeho aktualizaci. Proto v situacích, kdy je použít Pesimistická souběžnost, je obvykle, když se dosáhne, zruší zámek vypršení časového limitu. Lístek prodejních webů, ke kterým uživatel dokončí proces zpracování objednávky, uzamknout konkrétní sedadel umístění krátkou dobu, je příkladem pesimistické řízení souběžnosti.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Krok 1: Pohledu na tom, jak optimistického řízení souběžnosti se implementuje.

Tím zajistíte, že záznam bude aktualizován nebo odstraněn má stejné hodnoty, stejně jako při aktualizaci nebo odstranění proces spuštění funguje optimistického řízení souběžnosti. Například při kliknutí na tlačítko Upravit v upravitelné prvku GridView, hodnoty záznamu jsou číst z databáze a zobrazena v textových polí a dalších webových ovládacích prvcích. Tyto původní hodnoty jsou uloženy ve prvku GridView. Později až uživatel provede své změny a klikne na tlačítko Aktualizovat, původní hodnoty a nové hodnoty se odesílají do vrstvy obchodní logiky a pak dolů vrstvy přístupu k datům. Vrstva přístupu k datům, musíte vydat příkaz SQL, který pouze aktualizovat záznam, pokud původní hodnoty, které uživatel zahájil úpravy jsou identické s hodnotami stále v databázi. Obrázek 2 znázorňuje tato posloupnost událostí.


[![Pro Update nebo Delete na úspěšné původní hodnoty musí být rovna aktuální hodnoty databáze](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Obrázek 2**: For Update nebo Delete na hodnotu úspěch, původní hodnoty musí být rovna aktuální hodnot v databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image6.png))


Existují různé přístupy k implementace optimistického řízení souběžnosti (naleznete v tématu [Peter A. Bromberg](http://peterbromberg.net/)společnosti [Optmistic souběžnosti aktualizace logiky](http://www.eggheadcafe.com/articles/20050719.asp) stručný přehled o na řadu možností). Typová ADO.NET poskytuje jednu implementaci, která se dá nakonfigurovat s právě značek zaškrtávací políčko. Povolení optimistického řízení souběžnosti pro TableAdapter v datové sadě zadán argumentech objektu TableAdapter `UPDATE` a `DELETE` příkazy, které chcete zahrnout všechny původní hodnoty v porovnání `WHERE` klauzuli. Následující `UPDATE` příkazu, například aktualizace názvu a cena produktu pouze v případě, že aktuální hodnoty v databázi jsou stejné hodnoty, které byly původně načteny při aktualizaci záznamu v prvku GridView. `@ProductName` a `@UnitPrice` parametry obsahovat nové hodnoty zadané uživatelem, zatímco `@original_ProductName` a `@original_UnitPrice` obsahují hodnoty, které byly původně načten do prvku GridView, když došlo ke kliknutí na tlačítko Upravit:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> To `UPDATE` zjednodušili jsme příkaz pro lepší čitelnost. V praxi však `UnitPrice` vrátit se změnami `WHERE` bude složitější, protože klauzule `UnitPrice` může obsahovat `NULL` s a kontrolu v případě `NULL = NULL` vždy vrátí hodnotu False (místo toho je nutné použít `IS NULL`).


Kromě použití různých základní `UPDATE` příkaz konfigurace TableAdapter použít optimistické souběžnosti taky změní podpis jeho DB přímé metody. Pamatujete z naší první kurz [ *vytvoření vrstvy přístupu k datům*](../introduction/creating-a-data-access-layer-cs.md), DB přímých metod byly ty, které přijímá seznam skalární hodnoty jako vstupní parametry (místo DataRow silného typu nebo Objekt DataTable instance). Při použití optimistického řízení souběžnosti, DB přímé `Update()` a `Delete()` metody patří vstupní parametry pro původní hodnoty. Kromě toho kód v BLL pro používání služby batch aktualizace modelu ( `Update()` přetížení metody, které přijímají DataRows a DataTables spíše než skalární hodnoty) musí být změněny také.

Spíše než rozšiřte naše stávající objektů TableAdapter od DAL optimistického řízení souběžnosti (což by vyžadovalo přechod změna BLL tak, aby vyhovovaly), můžeme použít místo toho vytvořte nové zadané datové sady s názvem `NorthwindOptimisticConcurrency`, pro které přidáme `Products` TableAdapter, který používá Optimistická souběžnost. Pod vytvoříme `ProductsOptimisticConcurrencyBLL` třídu vrstvy obchodní logiky, která má odpovídající změny podporují optimistickou souběžnost vrstvy DAL. Po tomto základy byla přijata, budeme připravení vytvořit na stránce ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Krok 2: Vytvoření vrstvy přístupu k datům, která podporuje optimistické řízení souběžnosti

Chcete-li vytvořit nové datové sady typu, klikněte pravým tlačítkem na `DAL` složky v rámci `App_Code` složky a přidejte novou datovou sadu s názvem `NorthwindOptimisticConcurrency`. Jak jsme viděli v první kurz, tím tak bude přidán nový TableAdapter do typová, automaticky se spouští Průvodce nastavením TableAdapter. Na první obrazovce jsme výzva zadat databázi pro připojení k – připojení ke stejné databázi Northwind pomocí průvodce `NORTHWNDConnectionString` nastavení z `Web.config`.


[![Připojení ke stejné databázi Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Obrázek 3**: připojení ke stejné databázi Northwind ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image9.png))


Dále jsme se výzva k tom, jak zadávat dotazy na data: pomocí příkazu SQL ad-hoc, nové uložené procedury nebo existující uložené procedury. Protože jsme použili ad-hoc dotazy SQL v našich původní DAL, tuto možnost použijte tady také.


[![Zadat Data pro načtení pomocí Ad-Hoc příkazu SQL](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Obrázek 4**: zadání dat pro načtení pomocí příkazu SQL Ad-Hoc ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image12.png))


Na následujícím obrázku zadejte dotaz SQL, který bude použit k načtení informací o produktu. Použijeme přesně stejný dotaz SQL použitý pro `Products` TableAdapter z našich původní DAL, který vrátí všechny `Product` sloupce spolu s názvy dodavatele a kategorie produktu:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![V původní DAL použít stejný dotaz SQL z produktů TableAdapter](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Obrázek 5**: použijte stejný dotaz SQL z `Products` TableAdapter v původní vrstvy DAL ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image15.png))


Před přechodem na další obrazovce klikněte na tlačítko Upřesnit možnosti. Pokud chcete, aby tento ovládací prvek TableAdapter využívají optimistického řízení souběžnosti, stačí zaškrtněte políčko "Pomocí optimistického řízení souběžnosti".


[![Povolit optimistického řízení souběžnosti podle kontroluje &quot;použít optimistické řízení souběžnosti&quot; zaškrtávací políčko](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Obrázek 6**: Povolit optimistického řízení souběžnosti zaškrtnutím políčka "Pomocí optimistického řízení souběžnosti" ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image18.png))


A konečně označit, že by měl TableAdapter použít vzory přístupu k datům, které naplnit DataTable a vrátit tabulku DataTable; také určit, že by měl být vytvořen přímých metod DB. Změňte název metody pro vrácení objektu DataTable vzor z GetData GetProducts, tak, aby zrcadlí zásady vytváření názvů, který jsme použili v našich původní DAL.


[![Mít TableAdapter využívat všechny vzory přístupu k datům](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Obrázek 7**: mají TableAdapter využívat všechny vzory přístupu k datům ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image21.png))


Po dokončení průvodce bude obsahovat návrháři datových sad silného typu `Products` DataTable a TableAdapter. Za chvíli přejmenovat objekt DataTable z `Products` k `ProductsOptimisticConcurrency`, což lze provést pravým tlačítkem myši na záhlaví okna DataTable a zvolením přejmenovat v místní nabídce.


[![Objekt DataTable a TableAdapter jsou přidané do typové datové sady](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Obrázek 8**: objekt DataTable a byly přidány do datové sady typu TableAdapter ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image24.png))


Chcete-li zobrazit rozdíly mezi `UPDATE` a `DELETE` dotazy mezi `ProductsOptimisticConcurrency` TableAdapter, (ta používá optimistické řízení souběžnosti) a TableAdapter produkty (což není), kliknutím na TableAdapter a přejděte do okna Vlastnosti. V `DeleteCommand` a `UpdateCommand` vlastností `CommandText` objektu třídy subproperties můžete zobrazit výchozí syntaxi SQL, který se odešle do databáze, když jsou vyvolány DAL aktualizace nebo odstranění související metody. Pro `ProductsOptimisticConcurrency` TableAdapter `DELETE` je použít příkaz:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Vzhledem k tomu `DELETE` příkazu pro TableAdapter produktu v našich původní DAL je mnohem jednodušší:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Jak je vidět `WHERE` klauzuli v `DELETE` příkazu pro TableAdapter, který používá optimistické řízení souběžnosti zahrnuje porovnání mezi jednotlivými `Product` tabulky existující hodnoty ve sloupcích a původní hodnoty v době Section.Selectable posledního prvku GridView (nebo prvku DetailsView nebo FormView). Od všech polí kromě `ProductID`, `ProductName`, a `Discontinued` může mít `NULL` kontroly, další parametry a hodnoty jsou zahrnuty správně porovnat `NULL` hodnoty v `WHERE` klauzuli.

Jsme nebude přidávat žádné další DataTables optimistického řízení souběžnosti povolené datovou pro účely tohoto kurzu, jak naši stránku ASP.NET poskytne pouze aktualizace nebo odstranění informací o produktu. Nicméně stále potřeba přidat `GetProductByProductID(productID)` metodu `ProductsOptimisticConcurrency` TableAdapter.

Chcete-li to provést, klikněte pravým tlačítkem na záhlaví objektu TableAdapter (oblasti vpravo nahoře `Fill` a `GetProducts` názvy metod) a v místní nabídce zvolte možnost přidat dotaz. Tím spustíte Průvodce konfigurací dotazu TableAdapter. Jak s naší TableAdapter počáteční konfiguraci, rozhodnout vytvořit `GetProductByProductID(productID)` metodu pomocí příkazu SQL ad-hoc (viz obrázek 4). Protože `GetProductByProductID(productID)` metoda vrátí informace o daném produktu, označení, že je tento dotaz `SELECT` typ, který vrátí řádky dotazu.


[![Označit jako typ dotazu &quot;SELECT, který vrací řádky&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Obrázek 9**: označte typ dotazu jako "`SELECT` které vrátí řádky" ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image27.png))


Na další obrazovce jsme se výzva k zadání dotazu SQL pro použití s výchozí dotaz TableAdapter už načtené. Rozšířit existující dotaz pro přidání klauzule `WHERE ProductID = @ProductID`, jak je znázorněno na obrázku 10.


[![Přidat klauzuli WHERE klauzule, která už načtené dotaz, který vrátí záznam určitý produkt](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Obrázek 10**: Přidejte `WHERE` klauzule Pre-Loaded dotaz, který vrací konkrétní záznam produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image30.png))


Nakonec změňte názvy vytvořena metoda k `FillByProductID` a `GetProductByProductID`.


[![Přejmenovat metodu FillByProductID a GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Obrázek 11**: přejmenování metody `FillByProductID` a `GetProductByProductID` ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image33.png))


Pomocí tohoto průvodce kompletní TableAdapter teď obsahuje dvě metody pro načítání dat: `GetProducts()`, která vrací *všechny* produkty; a `GetProductByProductID(productID)`, která vrací zadaný produkt.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Krok 3: Vytvoření vrstvy obchodní logiky pro optimistického řízení souběžnosti povolené DAL

Naše stávající `ProductsBLL` třída obsahuje příklady použití aktualizace služby batch a s přímým přístupem vzory DB. `AddProduct` Metoda a `UpdateProduct` obě přetížení pomocí vzoru aktualizace služby batch předání v `ProductRow` instanci metody Update objektu TableAdapter. `DeleteProduct` Metodě na druhé straně používá vzor s přímým přístupem DB, volání objektu TableAdapter `Delete(productID)` metody.

S novými `ProductsOptimisticConcurrency` TableAdapter, databáze přímé metody nyní vyžadovat, že původní hodnoty také předávat v. Například `Delete` metoda očekává nyní deset vstupní parametry: původní `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, a `Discontinued`. Používá hodnoty těchto dalších vstupních parametrů v `WHERE` klauzuli `DELETE` příkaz odesílají do databáze, pouze pokud mapování aktuálních hodnot databázi do původního odstranění zadaný záznam.

Při podpisu metody pro TableAdapter `Update` nedošlo ke změně metody používané ve vzoru aktualizace služby batch, je kód potřebný k zaznamenání původní a nové hodnoty. Proto namísto pokusí použít optimistické souběžnosti povolené DAL s naší stávající `ProductsBLL` třídy, vytvoříme novou třídu vrstvy obchodní logiky pro práci s naší nové vrstvy DAL.

Přidejte třídu pojmenovanou `ProductsOptimisticConcurrencyBLL` k `BLL` složky v rámci `App_Code` složky.


![Přidat třídu ProductsOptimisticConcurrencyBLL BLL složky](implementing-optimistic-concurrency-vb/_static/image34.png)

**Obrázek 12**: Přidejte `ProductsOptimisticConcurrencyBLL` třídy do složky knihoven BLL


Dále přidejte následující kód, který `ProductsOptimisticConcurrencyBLL` třídy:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Všimněte si, pomocí `NorthwindOptimisticConcurrencyTableAdapters` příkaz nad počáteční deklaraci třídy. `NorthwindOptimisticConcurrencyTableAdapters` Obsahuje obor názvů `ProductsOptimisticConcurrencyTableAdapter` třídu, která poskytuje metody pro vrstvy DAL. Také před deklaraci třídy zjistíte, `System.ComponentModel.DataObject` atribut, který dává pokyn sady Visual Studio pro tuto třídu v prvku ObjectDataSource Průvodce rozevíracího seznamu.

`ProductsOptimisticConcurrencyBLL`Společnosti `Adapter` vlastnost poskytuje rychlý přístup k instanci `ProductsOptimisticConcurrencyTableAdapter` třídy a odpovídá používaným v našich původní třídy BLL (`ProductsBLL`, `CategoriesBLL`, a tak dále). Nakonec `GetProducts()` metoda provede jednoduché volání DAL `GetProducts()` metodu a vrátí `ProductsOptimisticConcurrencyDataTable` vyplní objekt `ProductsOptimisticConcurrencyRow` instance pro každý produkt záznam v databázi.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Odstraňuje se produkt s přímým přístupem vzor DB pomocí optimistického řízení souběžnosti

Při použití vzoru s přímým přístupem DB proti DAL, který používá Optimistická souběžnost, musí být předán nových a původních hodnot metody. Pro odstranění, nejsou žádné nové hodnoty, takže potřebujete předaný pouze původní hodnoty. V našich knihoven BLL potom můžeme musí přijmout všechny původní parametry jako vstupní parametry. Teď se podívejme `DeleteProduct` metodu `ProductsOptimisticConcurrencyBLL` třídy pomocí přímé metody DB. To znamená, že tato metoda je potřeba provést ve všech polích data deset produktů jako vstupní parametry a předat tyto vrstvy Dal, jak je znázorněno v následujícím kódu:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Pokud původní hodnoty – tyto hodnoty, které byly naposledy načtené do ovládacího prvku GridView (nebo prvku DetailsView nebo FormView) - liší od hodnot v databázi, když uživatel klikne na tlačítko Odstranit `WHERE` klauzule nebudou shodovat se záznam v databázi a žádné záznamy bude mít vliv. Proto objektu TableAdapter `Delete` vrátí metoda `0` a BLL `DeleteProduct` vrátí metoda `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualizace produktu vzor aktualizace služby Batch pomocí optimistického řízení souběžnosti

Jak bylo uvedeno dříve, objektu TableAdapter `Update` metoda pro vzor aktualizace služby batch, má stejnou signaturu metody bez ohledu na to, jestli použijí optimistického řízení souběžnosti. A to `Update` metoda očekává, že řádek dat, pole DataRows, datové tabulky nebo typované datové sady. Neexistují žádné další vstupní parametry pro určení původní hodnoty. To je možné, protože objekt DataTable uchovává informace o původní a upravené hodnoty pro jeho DataRow(s). Když DAL vydá jeho `UPDATE` příkazu, `@original_ColumnName` parametry jsou vyplněna původní hodnoty objekt DataRow, že `@ColumnName` parametry jsou vyplněna změněné hodnoty objekt DataRow.

V `ProductsBLL` třídy, (ta používá naše původní, optimistického řízení souběžnosti DAL), při použití vzoru aktualizace služby batch k aktualizaci informací o produktu náš kód provede následující posloupnost událostí:

1. Přečtěte si informace o aktuální databáze produktu do `ProductRow` instance pomocí objektu TableAdapter `GetProductByProductID(productID)` – metoda
2. Přiřazení nové hodnoty, které mají `ProductRow` instanci z kroku 1
3. Volání objektu TableAdapter `Update` metodu `ProductRow` instance

Tahle posloupnost kroků, ale nebude podporovat správně optimistického řízení souběžnosti protože `ProductRow` v kroku 1 se vyplní přímo z databáze, což znamená, že původní hodnoty používané datového řádku jsou ty, které momentálně existují v databáze a těch, které se váží k prvku GridView. na začátku procesu úprav. Místo toho při použití optimistické souběžnosti podporou DAL, musíme alter `UpdateProduct` přetížení metody použít následující kroky:

1. Přečtěte si informace o aktuální databáze produktu do `ProductsOptimisticConcurrencyRow` instance pomocí objektu TableAdapter `GetProductByProductID(productID)` – metoda
2. Přiřazení *původní* hodnoty `ProductsOptimisticConcurrencyRow` instanci z kroku 1
3. Volání `ProductsOptimisticConcurrencyRow` instance `AcceptChanges()` metoda, která nastaví objekt DataRow, jeho aktuální hodnoty jsou "" původního
4. Přiřazení *nové* hodnoty `ProductsOptimisticConcurrencyRow` instance
5. Volání objektu TableAdapter `Update` metodu `ProductsOptimisticConcurrencyRow` instance

Krok 1 čtení ve všech hodnot v aktuální databázi pro záznam zadaný produkt. Tento krok je nadbytečný v `UpdateProduct` přetížení, která aktualizuje *všechny* produktu sloupců (jako tyto hodnoty jsou přepsány v kroku 2), ale je nezbytné pro tyto přetížení, kde jsou pouze podmnožina hodnot sloupců předaný jako vstupní parametry. Jakmile se přiřadily původní hodnoty `ProductsOptimisticConcurrencyRow` instanci, `AcceptChanges()` metoda je volána, který označuje aktuální hodnoty objektu DataRow jako původní hodnoty, které se použijí v `@original_ColumnName` parametry v `UPDATE` příkazu. V dalším kroku nové hodnoty parametrů jsou přiřazeny k `ProductsOptimisticConcurrencyRow` a nakonec `Update` vyvoláním metody předáním datového řádku.

Následující kód ukazuje `UpdateProduct` přetížení přijímající všechna data produktu pole jako vstupní parametry. Zatímco není vidět tady `ProductsOptimisticConcurrencyBLL` součástí staženého souboru pro tento kurz obsahuje také třídy `UpdateProduct` přetížení, které přijímá jenom název a produktu cena jako vstupní parametry.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Krok 4: Předávání původní a nové hodnoty ze stránky ASP.NET do metody BLL

Pomocí vrstvy DAL a BLL kompletní už jen zbývá k vytvoření stránky ASP.NET, který můžete využít logiku optimistického řízení souběžnosti integrované do systému. Konkrétně data webový ovládací prvek (ovládacího prvku GridView, DetailsView nebo FormView) musí mějte na paměti, že jeho původní hodnoty a ObjectDataSource musí projít obě sady hodnot do vrstvy obchodní logiky. Kromě toho musí být nakonfigurován na stránce ASP.NET bez výpadku zpracovávat porušení souběžnosti.

Začněte otevřením `OptimisticConcurrency.aspx` stránku `EditInsertDelete` složky a přidání do Návrháře nastavení GridView jeho `ID` vlastnost `ProductsGrid`. Z inteligentních značek prvku GridView, rozhodnout vytvořit nového prvku ObjectDataSource s názvem `ProductsOptimisticConcurrencyDataSource`. Protože chceme, aby tento prvek ObjectDataSource použití vrstvy DAL, který podporuje optimistické řízení souběžnosti, nakonfigurujte ho na použití `ProductsOptimisticConcurrencyBLL` objektu.


[![Použití prvku ObjectDataSource mají ProductsOptimisticConcurrencyBLL objektu](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Obrázek 13**: mají použití prvku ObjectDataSource `ProductsOptimisticConcurrencyBLL` objektu ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image37.png))


Zvolte `GetProducts`, `UpdateProduct`, a `DeleteProduct` metody z rozevíracích seznamů v průvodci. Pro metodu UpdateProduct, použijte přetížení, která přijímá všechna pole data produktu.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurace vlastností ovládacího prvku ObjectDataSource

Po dokončení průvodce, deklarativní ObjectDataSource by měl vypadat nějak takto:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Jak je vidět `DeleteParameters` kolekce obsahuje `Parameter` instance pro každý deset vstupních parametrů v `ProductsOptimisticConcurrencyBLL` třídy `DeleteProduct` metoda. Podobně `UpdateParameters` kolekce obsahuje `Parameter` instance pro každý vstupní parametry v `UpdateProduct`.

Pro tyto předchozích kurzů, které se podílejí úpravu dat, doporučujeme odebrat ObjectDataSource `OldValuesParameterFormatString` vlastnosti v tomto okamžiku, protože tato vlastnost určuje, že metoda BLL očekává, že původní (nebo původní) hodnoty předávané, jakož i nové hodnoty. Kromě toho tato hodnota vlastnosti označuje názvy vstupních parametrů pro původní hodnoty. Protože jsme v původní hodnoty do BLL prochází, proveďte *není* odeberte tuto vlastnost.

> [!NOTE]
> Hodnota `OldValuesParameterFormatString` vlastnost musí být namapovaný na názvy vstupních parametrů v BLL, které očekávají původní hodnoty. Protože jsme pojmenovali tyto parametry `original_productName`, `original_supplierID`, a tak dále, můžete nechat `OldValuesParameterFormatString` hodnota vlastnosti jako `original_{0}`. Pokud však vstupních parametrů metody BLL měl názvy jako `old_productName`, `old_supplierID`, a tak dále, je potřeba aktualizovat `OldValuesParameterFormatString` vlastnost `old_{0}`.


Existuje jedno nastavení konečné vlastnost, kterou je potřeba provést v pořadí pro prvek ObjectDataSource správně předat metody BLL původní hodnoty. Má ObjectDataSource [vlastnost ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) , který lze přiřadit k [jednu ze dvou hodnot](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` – Výchozí hodnota. původní vstupních parametrů metody BLL neodesílá původní hodnoty
- `CompareAllValues` – odesílat BLL metody; původní hodnoty Tuto možnost zvolte, pokud optimistickou metodu souběžného zpracování

Za chvíli nastavit `ConflictDetection` vlastnost `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurace vlastnosti a pole prvku GridView.

S vlastnostmi ObjectDataSource správně nakonfigurované obraťme pozornost pro nastavení prvku GridView. Nejprve protože chceme, aby GridView pro podporu úpravy a odstranění, klikněte na tlačítko Povolit úpravy a Povolit odstranění zaškrtávací políčka z inteligentních značek v prvku GridView. Tím se přidá CommandField jehož `ShowEditButton` a `ShowDeleteButton` jsou nastaveny na `true`.

Při vázání na `ProductsOptimisticConcurrencyDataSource` prvek ObjectDataSource, prvku GridView obsahuje pole pro každé pole data produktu. Při takové GridView lze upravovat, je jakoukoli jinou přijatelné uživatelské prostředí. `CategoryID` a `SupplierID` BoundFields budou vykreslovat jako textová pole, by uživatel musel zadejte příslušnou kategorii a dodavatele jako ID čísla. Bude bez formátování pro číselná pole a ovládací prvky bez ověření k zajištění, že byl zadán název produktu a že je cena ze jednotku, jednotek v zásobách, jednotky v pořadí a hodnoty úrovně pro změnu uspořádání jsou obě správné číselné hodnoty a jsou větší než nebo rovno na nulu.

Jak jsme probírali v *přidání validačních ovládacích prvků pro úpravy a vložení rozhraní* a *přizpůsobení rozhraní pro úpravu dat* kurzy, uživatelské rozhraní je možné přizpůsobit pomocí Nahraďte vlastností TemplateField BoundFields. Můžu upravili tohoto ovládacího prvku GridView a jeho úprav rozhraní následujícími způsoby:

- Odebrat `ProductID`, `SupplierName`, a `CategoryName` BoundFields
- Převést `ProductName` Vlastnost BoundField na pole TemplateField a přidali ovládací prvek RequiredFieldValidation.
- Převést `CategoryID` a `SupplierID` BoundFields do vlastností TemplateField a upravit úprav rozhraní DropDownLists spíše než textová pole. V těchto vlastností TemplateField `ItemTemplates`, `CategoryName` a `SupplierName` datová pole se zobrazí.
- Převést `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` BoundFields vlastností TemplateField a přidání ovládacích prvků CompareValidator.

Protože jsme jste už se zaměřili na tom, jak provádět tyto úlohy v předchozích kurzech, můžu budete jenom seznam konečné deklarativní syntaxe tady a ponechte implementaci z hlediska.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Jsme velmi blízko s plně funkční příklad. Existuje však několik odlišností, které budou ovládat si a námi způsobit problémy. Kromě toho potřebujeme ještě nějaké rozhraní, které uživatele upozorní, když došlo k narušení souběžného zpracování.

> [!NOTE]
> V pořadí dat webový ovládací prvek ObjectDataSource (které jsou potom předány knihoven BLL) správně předat původní hodnoty, je důležité, který prvku GridView `EnableViewState` je nastavena na `true` (výchozí). Pokud zakážete stav zobrazení, se ztratí, původní hodnoty na zpětné volání.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Předejte správné původní hodnoty prvku ObjectDataSource

Existuje několik možných problémů s způsob, jakým byl nakonfigurován prvku GridView. Pokud ObjectDataSource `ConflictDetection` je nastavena na `CompareAllValues` (jako je náš), když prvku ObjectDataSource `Update()` nebo `Delete()` jsou metody vyvolány pomocí ovládacího prvku GridView (nebo prvku DetailsView nebo FormView), ObjectDataSource pokusí zkopírovat Původní hodnoty prvku GridView do jeho odpovídající `Parameter` instancí. Vraťte se na obrázku 2 pro grafickou reprezentaci tohoto procesu.

Konkrétně prvku GridView původní hodnoty jsou přiřazeny hodnoty v příkazy Obousměrná vazba dat pokaždé, když vázaná k prvku GridView. Proto je důležité, že všechny požadované původní hodnoty se zachycuje prostřednictvím dvousměrnou datovou vazbou a že jsou k dispozici ve formátu převést.

Chcete-li zjistit, proč je důležité, věnujte chvíli najdete na naší stránce v prohlížeči. Podle očekávání, uvádí prvku GridView. každý produkt pomocí tlačítka Upravit a odstranit v levém sloupci.


[![Produkty jsou uvedené v GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Obrázek 14**: The produkty jsou uvedené v GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image40.png))


Pokud kliknete na tlačítko Odstranit pro některý z produktů `FormatException` je vyvolána výjimka.


[![Pokus o odstranění jakékoli výsledky produktů v FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Obrázek 15**: Pokus o odstranění Any výsledky produktů v `FormatException` ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image43.png))


`FormatException` Se vyvolá, když prvku ObjectDataSource se pokusí přečíst v původní `UnitPrice` hodnotu. Protože `ItemTemplate` má `UnitPrice` formátu měny (`<%# Bind("UnitPrice", "{0:C}") %>`), obsahuje symbol měny, jako je 19,95. `FormatException` Dochází při ObjectDataSource se pokusí převést tento řetězec do `decimal`. Chcete-li tento problém obejít, máme řadu možností:

- Odebrat formátování měny `ItemTemplate`. To znamená, že místo použití `<%# Bind("UnitPrice", "{0:C}") %>`, jednoduše použijte `<%# Bind("UnitPrice") %>`. Nevýhodou této je, že cena již formátována.
- Zobrazení `UnitPrice` formátu měny v `ItemTemplate`, ale použít `Eval` – klíčové slovo jak toho dosáhnout. Vzpomeňte si, že `Eval` provádí jednosměrné datové vazby. Stále potřebujeme pro poskytování `UnitPrice` hodnoty pro původní hodnoty, proto budeme i nadále potřebovat příkaz Obousměrná vazba dat v `ItemTemplate`, ale to je možné použít v ovládacím prvku popisek webového jehož `Visible` je nastavena na `false`. Ve vlastnosti ItemTemplate, můžeme použít následující kód:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Odebrat formátování měny `ItemTemplate`s použitím `<%# Bind("UnitPrice") %>`. V prvku GridView `RowDataBound` obslužná rutina události, programově přístup k webu popisek ovládací prvek v rámci kterého `UnitPrice` hodnota se zobrazí a nastavte jeho `Text` vlastnost formátovaná verze.
- Nechte `UnitPrice` naformátovaná jako měnu. V prvku GridView `RowDeleting` obslužná rutina události, nahraďte existující původní `UnitPrice` hodnotu (19,95 USD) se skutečný desítkovou hodnotu pomocí `Decimal.Parse`. Jsme viděli, jak provést něco podobného v `RowUpdating` obslužné rutině událostí ve [ *zpracování knihoven BLL a výjimek úrovni DAL na stránce ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) kurzu.

Moje například volba pro druhý postup přidání skrytých popisek webové ovládací prvek, jehož `Text` vlastnost je obousměrný data vázaná neformátovaný `UnitPrice` hodnotu.

Po vyřešení tohoto problému, zkuste to znovu kliknutím na tlačítko Odstranit pro některý z produktů. Tentokrát získáte `InvalidOperationException` kdy pokusí vyvolat BLL ObjectDataSource `UpdateProduct` metody.


[![Prvku ObjectDataSource nebyla nalezena metoda s vstupní parametry, které chce odeslat](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Obrázek 16**: ObjectDataSource nebyla nalezena metoda s vstupní parametry, které chce odeslat ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image46.png))


Prohlížení zpráva pro výjimku, je jasné, že chce vyvolání BLL ObjectDataSource `DeleteProduct` metoda, která zahrnuje `original_CategoryName` a `original_SupplierName` vstupní parametry. Důvodem je, že `ItemTemplate` iterátory `CategoryID` a `SupplierID` vlastností TemplateField aktuálně obsahují příkazy obousměrné vazby `CategoryName` a `SupplierName` datová pole. Místo toho musíme zahrnout `Bind` příkazy `CategoryID` a `SupplierID` datová pole. K tomu, nahraďte existující příkazy vazby s `Eval` příkazy a pak přidejte ovládací prvky skrytý popisku, jejichž `Text` vlastnosti, které jsou vázány na `CategoryID` a `SupplierID` datových polí pomocí Obousměrná vazba dat, jak je znázorněno níže:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

S těmito změnami jsme schopní teď úspěšně odstranit a upravit informace o produktu! V kroku 5 podíváme na tom, jak ověřit, že jsou detekovány porušení souběžnosti. Ale chvíli trvat pár minut a zkuste aktualizovat a odstranit několik záznamů zajistit, že aktualizace a odstranění pro jednoho uživatele funguje podle očekávání.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Krok 5: Testování podpora optimistické řízení souběžnosti

Pokud chcete ověřit, že porušení souběžnosti se zjištěné (spíše než výsledkem dat slepě přepsání), potřebujeme otevře dvě okna prohlížeče na tuto stránku. V obou případech prohlížeče klikněte na tlačítko Upravit pro Chai. Jen v jedné z prohlížečů, změňte název na "Chai čaje" a kliknutím na tlačítko Aktualizovat. Aktualizace by měla úspěšné a prvku GridView. vraťte se do stavu předem úprav s "Chai čaje" jako nový název produktu.

V další okno instance prohlížeče ale textového pole název produktu stále hlásí "Chai". V této druhé okno prohlížeče, aktualizujte `UnitPrice` k `25.00`. Bez podpory optimistického řízení souběžnosti kliknutím na tlačítko Aktualizovat v druhé instanci prohlížeče se změní název produktu zpět "Chai", a tím přepsání změny provedené při první instanci prohlížeče. Pomocí optimistického řízení souběžnosti použijí, ale kliknutím na tlačítko Aktualizovat v druhé instanci prohlížeče vede [dbconcurrencyexception –](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Když je zjištěna narušení souběžného zpracování, je vyvolána výjimka dbconcurrencyexception –](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Obrázek 17**: po narušení souběžného zpracování zjištění, `DBConcurrencyException` je vyvolána výjimka ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image49.png))


`DBConcurrencyException` Je pouze vyvolána, když se používá vzor aktualizace služby batch DAL. Model s přímým přístupem DB nevyvolá výjimku, pouze znamená, že byly ovlivněny žádné řádky. Pro znázornění je vrátíte do jejich předem úprav stavu ovládacího prvku GridView obě instance prohlížeče. V dalším kroku v první instanci prohlížeče, klikněte na tlačítko Upravit a změnit název produktu z "Chai čaje" zpět do "Chai" a kliknutím na tlačítko Aktualizovat. V druhém okně prohlížeče klikněte na tlačítko Odstranit pro Chai.

Po kliknutí na tlačítko odstranit, na stránce odešle zpět, prvku GridView vyvolá ObjectDataSource `Delete()` metoda a ObjectDataSource zavolá `ProductsOptimisticConcurrencyBLL` třídy `DeleteProduct` předejte podél původní hodnoty. Původní `ProductName` hodnota druhou instanci prohlížeče není "Chai čaje", což se neshoduje s aktuálním `ProductName` hodnotou uloženou v databázi. Proto `DELETE` prohlášení vydané do databáze ovlivňuje nulový počet řádků, protože neexistuje žádný záznam v databázi, která `WHERE` klauzule splňuje. `DeleteProduct` Vrátí metoda `false` a ObjectDataSource dat je znovu připojeno k prvku GridView.

Z pohledu koncového uživatele kliknutím na tlačítko Odstranit pro Chai čaje v druhém okně prohlížeče způsobil tuto obrazovku na flash a po vracející se zpět, je stále existuje, ale teď je uveden jako "Chai" (product name změny provedené první prohlížeče instance). Pokud uživatel klikne na tlačítko Odstranit znovu, bude odstranění úspěšné, jako je původní prvku GridView `ProductName` hodnotu ("Chai") nyní odpovídá hodnotě v databázi.

V obou těchto případech činnost koncového uživatele je daleko od ideální. Jasně nechceme uživateli zobrazit podrobnosti o nitty-gritty `DBConcurrencyException` výjimka při použití vzoru aktualizace služby batch. A jak uživatelé příkaz nejde provést, ale neexistuje žádná upřesnění důvod, proč je trochu matoucí chování při použití vzoru s přímým přístupem DB.

Chcete-li vyřešit tyto dva problémy, vytvoříme popisek webové ovládací prvky na stránce zadání zdůvodnění na důvod, proč aktualizaci nebo odstranění se nezdařilo. Pro vzor aktualizace služby batch, můžeme určit, zda je či není `DBConcurrencyException` popisek upozornění došlo k výjimce v obslužné rutině událostí na úrovni po prvku GridView, zobrazení, podle potřeby. Přímé metody DB jsme můžete prozkoumat vrácenou hodnotu metody BLL (což je `true` Pokud byla vliv na jeden řádek, `false` jinak) a podle potřeby se zobrazí informační zpráva.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Krok 6: Přidání informačních zpráv a zobrazování i v případě narušení souběžného zpracování

Pokud dojde k narušení souběžného zpracování, chování monitorujícím závisí na Určuje, zda byla použita aktualizace služby batch nebo DB model s přímým přístupem vrstvy DAL. V našem kurzu používá oba vzorky se vzorem aktualizace služby batch používá pro aktualizace a DB přímé vzor použitý k odstranění. Začněte tím, Pojďme přidejte dva ovládací prvky popisek Web na stránku, která vysvětluje, že při pokusu o odstranění nebo aktualizaci dat došlo k narušení souběžného zpracování. Nastavení ovládacího prvku popisku `Visible` a `EnableViewState` vlastností `false`; to způsobí, že je na každé stránce, navštivte jako skryté, s výjimkou toho, kteří navštíví konkrétní stránce where jejich `Visible` prostřednictvím kódu programu je vlastnost nastavena na `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Kromě nastavení jejich `Visible`, `EnabledViewState`, a `Text` vlastnosti, také je nastavené `CssClass` vlastnost `Warning`, což způsobí, že popisek uživatele zobrazeného ve velké, červený, kurzíva, tučné písmo. CSS `Warning` třídy byla definována a přidán do Styles.css zpátky *zkoumání události spojené s vložení, aktualizace a odstranění* kurzu.

Po přidání tyto popisky, by měla vypadat podobně jako obrázek 18 návrháře v sadě Visual Studio.


[![Byly přidány dva ovládací prvky popisek na stránku](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Obrázek 18**: dva popisek ovládacích prvků přidaných na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image52.png))


Pomocí těchto ovládacích prvků webového popisek na místě jsme připraveni prozkoumat jak zjistit, kdy narušení souběžného zpracování došlo, ve kterém bodu odpovídajícího označení `Visible` nastavenou na `true`, informační zprávou.

## <a name="handling-concurrency-violations-when-updating"></a>Narušení souběžného zpracování při aktualizaci

Nejprve podíváme na způsob zpracování souběžnosti porušení, při použití vzoru aktualizace služby batch. Od těchto porušení s listem aktualizace vzor Příčina `DBConcurrencyException` výjimka, která je vyvolána, potřebujeme přidat kód na stránku ASP.NET k určení, zda `DBConcurrencyException` během procesu aktualizace došlo k výjimce. Pokud jsme tedy by se zobrazit zpráva pro uživatele s vysvětlením své změny nebyly uloženy, protože změněn jiným uživatelem měly stejná data mezi při zahájil úpravy záznamu a při kliknutí na tlačítko Aktualizovat.

Jak jsme viděli v *zpracování knihoven BLL a výjimek úrovni DAL na stránce ASP.NET* kurz takové výjimky mohou být zjištěna a potlačit v obslužných rutinách událostí po úrovně ovládacího prvku webových dat. Proto potřebujeme vytvořit obslužnou rutinu události pro prvku GridView `RowUpdated` událost, která kontroluje, jestli `DBConcurrencyException` byla vyvolána výjimka. Tato obslužná rutina události je předán odkaz na jakékoli výjimce, která byla vygenerována během procesu aktualizace, jak je znázorněno v případě, že obslužná rutina kódu níže:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Face z `DBConcurrencyException` této obslužné rutiny události výjimky, se zobrazí `UpdateConflictMessage` ovládacímu prvku popisek a znamená, že výjimka byla zpracována. S tímto kódem na místě, pokud při aktualizaci záznamu, dojde k narušení souběžného zpracování uživatele změny budou ztraceny, vzhledem k tomu, že by přepsání změn jiného uživatele ve stejnou dobu. Zejména je prvku GridView. vrátí do stavu před úpravy a vázána na aktuální data databáze. Tím se aktualizuje řádek prvku GridView změnami druhého uživatele, které byly dříve nejsou viditelné. Kromě toho `UpdateConflictMessage` ovládací prvek popisku vysvětlí uživateli, co se právě stalo. Tahle posloupnost událostí je podrobně popsaná v obrázek 19.


[![Uživatel s aktualizací jsou ztraceny stěně narušení souběžného zpracování](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Obrázek 19**: uživatel s aktualizací jsou ztraceny stěně narušení souběžného zpracování ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> Alternativně namísto vrácení prvku GridView do předem úprav stavu, může necháme prvku GridView ve stavu úprav tak, že nastavíte `KeepInEditMode` vlastnost předaným `GridViewUpdatedEventArgs` objektu na hodnotu true. Pokud je provést tento postup, ale ujistěte se, znovu připojit data, která mají prvku GridView (vyvoláním jeho `DataBind()` metoda) tak, aby druhý uživatel hodnoty jsou načteny do rozhraní pro úpravy. Kód, který je k dispozici ke stažení v tomto kurzu má tyto dva řádky kódu ve `RowUpdated` zakomentované obslužné rutiny události; jednoduše zrušte komentář u tyto řádky kódu, které mají prvku GridView zůstávají v režimu úprav po narušení souběžného zpracování.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Reakce na porušení souběžnosti při odstraňování

Pomocí vzoru s přímým přístupem DB není žádná výjimka vyvolána i v případě narušení souběžného zpracování. Místo toho příkaz database jednoduše má vliv na žádné záznamy, jako v klauzuli WHERE se neshoduje s žádným záznamem. Všechny metody úpravy dat v BLL vytvoří byly navrženy tak, že vrátí logickou hodnotu označující, zda jejich vliv na přesně jeden záznam. Proto pokud chcete zjistit, pokud se při odstranění záznamu došlo k narušení souběžného zpracování, jsme prozkoumat vrácenou hodnotu BLL `DeleteProduct` metody.

Návratová hodnota metody BLL se dají prozkoumat v obslužné rutině událostí po úrovně prvku ObjectDataSource prostřednictvím `ReturnValue` vlastnost `ObjectDataSourceStatusEventArgs` objekt předaný do obslužné rutiny události. Od nás zajímají určující návratovou hodnotu z `DeleteProduct` metoda, je nutné vytvořit obslužnou rutinu události pro ObjectDataSource `Deleted` událostí. `ReturnValue` Vlastnost je typu `object` a může být `null` Pokud došlo k výjimce a metodu byl přerušen před dokončením ho může vracet hodnotu. Proto jsme měli nejdřív zajistit, aby `ReturnValue` jedná o vlastnost neumožňující `null` a je logická hodnota. Za předpokladu, že tato kontrola proběhne, ukážeme `DeleteConflictMessage` ovládacímu prvku popisek, pokud `ReturnValue` je `false`. Můžete to provést pomocí následujícího kódu:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

I v případě narušení souběžného zpracování je odstranit žádost uživatele zrušena. Aktualizaci prvku GridView zobrazující, že změny, ke kterým došlo u záznamu daného mezi časem uživatele načtení stránky a má po kliknutí na tlačítko Odstranit. Když je porušení pravidel ukáže, `DeleteConflictMessage` se zobrazí popisek s vysvětlením, co právě se stalo (viz obrázek 20).


[![Uživatel s Delete se zruší i v případě narušení souběžného zpracování](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Obrázek 20**: uživatel s Delete se zruší i v případě narušení souběžného zpracování ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Souhrn

Příležitosti k porušení souběžnosti existovat v každé aplikaci, která umožňuje víc souběžných uživatelů, aktualizovat nebo odstranit data. Pokud takové porušení nejsou zahrnuté u, pokud dva uživatele najednou aktualizovat stejná data, kdo získá poslední zápis "WINS", přepíše jiný uživatel změní změny. Vývojářům můžete alternativně implementovat buď řízení souběžnosti optimistického nebo pesimistického. Optimistického řízení souběžnosti se předpokládá, že porušení souběžnosti jen zřídka a jednoduše nepovoluje aktualizaci nebo odstraňte příkaz, který bude představovat narušení souběžného zpracování. Pesimistické řízení souběžnosti předpokládá, že tento souběžnosti porušení jsou časté a jeden uživatel jednoduše odmítnutí aktualizace nebo odstranění příkaz není platný. S pesimistické řízení souběžnosti aktualizaci záznamu zahrnuje uzamčení, a tím brání ostatní uživatelé ve změnách nebo odstranění záznamu, když je zamknutý.

Typované datové sady v .NET poskytuje funkce pro podporu optimistického řízení souběžnosti. Konkrétně se `UPDATE` a `DELETE` příkazy vydané do databáze zahrnují všechny sloupce v tabulce, a zajistí tak, že aktualizace nebo odstranění se vztahuje pouze na Pokud aktuální data záznamu odpovídají s původními daty uživatel měl při provádění jejich aktualizace nebo odstranění. Jakmile DAL není nakonfigurovaná podpora optimistické souběžnosti, je potřeba aktualizovat BLL metody. Kromě toho stránky ASP.NET, která volá BLL musí být nakonfigurován tak, obnoví původní hodnoty z jeho data webový ovládací prvek ObjectDataSource a předává je do BLL.

Jak jsme viděli v tomto kurzu, implementace optimistického řízení souběžnosti ve webové aplikaci ASP.NET zahrnuje aktualizace knihoven BLL a DAL a přidání podpory na stránce ASP.NET. Určuje, jestli tato přidání práce je z pohledu investování času a úsilí závisí na vaší aplikace. Pokud máte málo souběžných uživatelů, aktualizace dat nebo data, která jsou aktualizace se liší od sebe, řízení souběžnosti není problém s klíče. Pokud ale pravidelně máte více uživatelům ve vaší lokalitě pracovat se stejnými daty, řízení souběžnosti pomáhají zabránit jeden uživatel aktualizace nebo odstranění nechtěně přepsání jiného uživatele.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](customizing-the-data-modification-interface-vb.md)
> [další](adding-client-side-confirmation-when-deleting-vb.md)
