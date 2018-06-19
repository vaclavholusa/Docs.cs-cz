---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: Implementace optimistickou metodu souběžného (C#) | Microsoft Docs
author: rick-anderson
description: Pro webovou aplikaci, která umožňuje více uživatelům upravovat data existuje riziko, že dva uživatele může být úpravy stejná data ve stejnou dobu. V této tutori...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 27441ea9343055b3139468036fc6f201c77667e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891045"
---
<a name="implementing-optimistic-concurrency-c"></a>Implementace optimistickou metodu souběžného (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) nebo [stáhnout PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Pro webovou aplikaci, která umožňuje více uživatelům upravovat data existuje riziko, že dva uživatele může být úpravy stejná data ve stejnou dobu. V tomto kurzu jsme budete implementovat řízení optimistickou metodu souběžného zpracování toto riziko.


## <a name="introduction"></a>Úvod

Pro webové aplikace, které pouze povolit uživatelům zobrazit data, nebo pro ty, které zahrnují jenom jeden uživatel, který může měnit data neexistují hrozby dvou souběžných uživatelů náhodnému přepsání své změny. Pro webové aplikace, které umožňují více uživatelům aktualizovat nebo odstranit data ale není potenciálně pro jednoho uživatele úpravy kolidovat s jinou souběžnou uživatele. Bez žádné zásady souběžnosti na místě Pokud dva uživatelé jsou úpravy současně jeden záznam, uživatele, který potvrdí její změny poslední přepíše změny provedené při první.

Představte si například, že dva uživatelé, Jisun a Sam, byly obě návštěvou stránky v naší aplikaci, která povolena návštěvníky aktualizovat a odstraňovat produkty pomocí ovládacího prvku GridView. Obě klikněte na tlačítko Upravit v GridView přibližně ve stejnou dobu. Jisun změny názvu produktu "Chai čaj" a kliknutím na tlačítko Aktualizovat. Net výsledkem je `UPDATE` příkaz, který je odeslal do databáze, která nastaví *všechny* aktualizovatelné polí produktu (i když Jisun aktualizuje jenom jedno pole `ProductName`). V tuto chvíli databáze má hodnoty "Chai čaj," kategorie Nápoje dodavatele exotické kapaliny pro tento konkrétní produkt a tak dále. Však GridView na obrazovce Sam je stále zobrazena název produktu upravovat řádku GridView jako "Chai". Několik sekund poté, co je Jisun změny byly potvrzeny, Sam aktualizace kategorie přísady a klikne na tlačítko Aktualizovat. To vede `UPDATE` příkaz odeslal do databáze, která nastaví název produktu a "Chai," `CategoryID` na odpovídající ID kategorie Nápoje a tak dále. Být přepsán Jisun na změny v názvu produktu. Obrázek 1 graficky znázorňuje tuto řadu událostí.


[![Při dvou uživatelů současně aktualizovat záznam, existuje s potenciální pro jednoho uživatele s změny přepsat jiné s](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Obrázek 1**: když dva uživatelé současně aktualizace existuje záznam s potenciální pro jednoho uživatele s změny přepsat jiné s ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image3.png))


Podobně pokud dva uživatelé navštěvují stránky, jeden uživatel může být in the midst of aktualizace záznamu, když je odstraněn jiným uživatelem. Nebo mezi načte, když uživatel na stránce a při jejich klikněte na tlačítko odstranit, může mít jiný uživatel upravil obsah daného záznamu.

Existují tři [Kontrola souběžnosti](http://en.wikipedia.org/wiki/Concurrency_control) strategií, které jsou k dispozici:

- **Nedělat nic** -li souběžných uživatelů změnit stejný záznam, umožní poslední potvrzení win (výchozí nastavení)
- [**Optimistickou metodu souběžného** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -předpokládají, která při pravděpodobně souběžnosti konfliktu every teď a potom valná většina času takové nedojde ke konfliktu; proto pokud dojde ke konfliktu, jednoduše informujte uživatele, který jejich změny nelze uložit, protože byl změněn jiným uživatelem stejná data
- **Pesimistické souběžnosti** -předpokládají, že je v konfliktu souběžnosti jsou běžné a že uživatelé nebudou tolerovat vrácení sdělili, jejich změny nebyly uloženy z důvodu souběžných aktivity jiného uživatele; proto když jeden uživatel spustí aktualizaci záznamu, uzamknout , a zabrání všichni ostatní uživatelé z úpravy nebo odstranění záznamů, dokud uživatel potvrdí jejich úpravy

Všechny naše kurzy doposud použili výchozí strategie řešení souběžnosti – konkrétně, jsme jste mohli poslední zápis win. V tomto kurzu podíváme, jak implementovat optimistické řízení souběžného.

> [!NOTE]
> Podíváme nebude na příklady pesimistické souběžnosti tento kurz řady. Pesimistické souběžnosti je zřídka použít, protože například uzamkne, není-li správně uvolní, když, můžete zabránit aktualizace dat jiných uživatelů. Například pokud je uživatel uzamkne záznam pro úpravy a odešel dne odemknutí ho, žádný jiný uživatel bude moct aktualizovat záznamů, dokud původního uživatele, vrátí a dokončí jeho aktualizaci. Proto v situacích, kdy je použít pesimistické souběžnosti, je obvykle vypršení časového limitu, která, pokud je dosaženo, zruší zámek. Lístek prodeje webů, ke kterým Zamknout umístění konkrétní zasedací krátkou dobu, když uživatel dokončí proces pořadí, je příkladem řízení pesimistické souběžnosti.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Krok 1: Prohlížení jak optimistickou metodu souběžného je implementována.

Optimistické řízení souběžného funguje tak, že zajistíte, že bude aktualizován nebo odstraněn záznam má stejné hodnoty, stejně jako při aktualizaci nebo odstranění proces spuštění. Například při kliknutí na tlačítko Upravit v upravitelné GridView hodnoty záznamu se přečíst z databáze a zobrazují v textových polí a jiných webových ovládacích prvků. Tyto původní hodnoty se uloží prostřednictvím GridView. Později až uživatel provede jeho změny a kliknutím na tlačítko Aktualizovat, původní hodnoty a nové hodnoty se odesílají na vrstvu obchodní logiky a pak na Data Access Layer. Data Access Layer musíte vydat příkaz SQL, který bude pouze aktualizovat záznam, pokud původní hodnoty, které uživatel začali upravovat jsou identické s hodnotami stále v databázi. Obrázek 2 znázorňuje posloupnosti událostí.


[![Pro aktualizaci nebo odstranění úspěšné původní hodnoty musí být rovna aktuální hodnot v databázi](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Obrázek 2**: pro aktualizaci nebo odstranění hodnotu úspěch, původní hodnoty musí být rovna hodnot v aktuální databázi ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image6.png))


Existují různé přístupy k implementaci optimistickou metodu souběžného (najdete v části [Petr A. Bromberg](http://peterbromberg.net/)na [Optmistic souběžnosti aktualizace logiku](http://www.eggheadcafe.com/articles/20050719.asp) pro stručný pohled na řadu možností). Datová sada zadali ADO.NET poskytuje jeden implementace, která se dá nakonfigurovat s právě značek zaškrtávací políčko. Povolování TableAdapter v datové sadě zadali rozšiřuje TableAdapter optimistickou metodu souběžného `UPDATE` a `DELETE` příkazy, které chcete zahrnout všechny původní hodnoty v porovnání `WHERE` klauzule. Následující `UPDATE` prohlášení, například aktualizace název a cena produktu pouze v případě, že jsou stejné hodnoty, které byly původně načteny při aktualizaci záznamu v GridView hodnot v aktuální databázi. `@ProductName` a `@UnitPrice` parametry obsahovat nové hodnoty zadané uživatelem, zatímco `@original_ProductName` a `@original_UnitPrice` obsahují hodnoty, které byly původně načíst do GridView, pokud bylo stisknuto tlačítko Upravit:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> To `UPDATE` příkaz je jednodušší čitelnější. V praxi `UnitPrice` změnami `WHERE` klauzule by složitější od `UnitPrice` může obsahovat `NULL` s a probíhá kontrola, zda `NULL = NULL` vždy vrátí hodnotu False (místo toho musíte použít `IS NULL`).


Kromě použití různých základní `UPDATE` prohlášení, konfigurace TableAdapter použít optimistickou metodu souběžného zpracování také upraví podpis jeho DB přímé metody. Odvolat z našich první kurz [ *vytváření Data Access Layer*](../introduction/creating-a-data-access-layer-cs.md), přímé metody DB byly ty, které přijímá seznam skalárních hodnot jako vstupní parametry (místo DataRow silného typu nebo DataTable instance). Při použití optimistickou metodu souběžného přímé DB `Update()` a `Delete()` metody patří vstupní parametry pro původní hodnoty. Kromě toho kód v BLL pro používání dávky aktualizovat vzoru ( `Update()` přetížení metody, které přijímají DataRows a DataTables spíše než skalární hodnoty) je třeba změnit také.

Spíše než rozšířit naše stávající TableAdapters na DAL použít optimistickou metodu souběžného (které by vyžadovaly změnu BLL zohlednit), můžeme místo vytvořit novou zadali datovou sadu s názvem `NorthwindOptimisticConcurrency`, do které přidáme `Products` TableAdapter, používá optimistickou metodu souběžného. Následující, vytvoříme `ProductsOptimisticConcurrencyBLL` třídy vrstvu obchodní logiky, která má odpovídající změny pro podporu optimistickou metodu souběžného DAL. Po této základ je uveden, jsme budete moci vytvořit stránku ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Krok 2: Vytvoření Data Access Layer, který podporuje optimistickou metodu souběžného zpracování

Pokud chcete vytvořit novou sadu dat zadali, klikněte pravým tlačítkem na `DAL` složky v rámci `App_Code` složky a přidat novou datovou sadu s názvem `NorthwindOptimisticConcurrency`. Jak jsme viděli z prvního kurzu, to tak bude přidán nový TableAdapter typové datové sady automaticky spuštěním Průvodce konfigurací TableAdapter. Na první obrazovce jsme se výzva k zadání databáze se připojit k - připojit ke stejné databázi Northwind pomocí `NORTHWNDConnectionString` nastavení z `Web.config`.


[![Připojení do stejné databáze Northwind](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Obrázek 3**: připojení k databázi Northwind stejné ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image9.png))


V dalším kroku jsme se zobrazí výzva, jak zadávat dotazy na data: prostřednictvím příkazu SQL ad-hoc novou uložené procedury nebo existující uložené procedury. Vzhledem k tomu, že jsme použili SQL dotazů ad-hoc v našem původní DAL, tuto možnost použijte zde také.


[![Zadejte Data načíst pomocí příkazu SQL Ad-Hoc](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Obrázek 4**: byla Data určena k načtení příkazu SQL Ad-Hoc ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image12.png))


Na následující obrazovce zadejte příkaz jazyka SQL sloužící k načtení informací o produktu. Umožňuje použít přesný stejný dotaz SQL použít pro `Products` TableAdapter z našich původní DAL, která vrátí všechny `Product` sloupce společně s názvy dodavatele a kategorii produktu:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![V původní DAL použít stejný dotaz SQL z produktů TableAdapter](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Obrázek 5**: použijte stejný dotaz SQL z `Products` TableAdapter v původní DAL ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image15.png))


Než budete pokračovat na další obrazovce, klikněte na tlačítko Upřesnit možnosti. Pokud chcete, aby tento ovládací prvek TableAdapter společnosti využívají optimistickou metodu souběžného, stačí zaškrtněte políčko "Použít optimistickou metodu souběžného".


[![Povolit optimistické řízení souběžného zpracování pomocí kontroluje &quot;použít optimistickou metodu souběžného&quot; zaškrtávací políčko](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Obrázek 6**: Povolit optimistické řízení souběžného zpracování zaškrtnutím políčka "Použít optimistickou metodu souběžného" ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image18.png))


Nakonec označte, že TableAdapter by měl použít vzory přístupu k dat, které vyplnění DataTable i vrátit DataTable; také znamenat, že by se měl vytvořit přímé metody DB. Změňte název metody návratový DataTable vzor z GetData GetProducts, která zrcadlení názvů jsme použili v našem původní DAL.


[![Mít TableAdapter využívat všechny přístupové vzorce dat](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Obrázek 7**: mít TableAdapter využívat všechny přístup vzorky dat ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image21.png))


Po dokončení průvodce bude obsahovat návrháře DataSet silného typu `Products` DataTable a TableAdapter. Za chvíli přejmenovat DataTable z `Products` k `ProductsOptimisticConcurrency`, což lze provést tak, že pravým tlačítkem myši na záhlaví DataTable a výběr přejmenovat v místní nabídce.


[![DataTable a TableAdapter byly přidány do typové datové sady](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Obrázek 8**: A DataTable a TableAdapter byly přidány do typové datové sady ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image24.png))


Pro informace o rozdílech mezi `UPDATE` a `DELETE` dotazuje mezi `ProductsOptimisticConcurrency` TableAdapter (který se používá optimistickou metodu souběžného) a TableAdapter produktů, (který nepodporuje), klikněte na TableAdapter a přejít do okna vlastností. V `DeleteCommand` a `UpdateCommand` vlastnosti `CommandText` podvlastnosti uvidíte skutečné syntaxe SQL, která je odeslána do databáze, pokud jsou vyvolány DAL aktualizace nebo odstranění související metody. Pro `ProductsOptimisticConcurrency` TableAdapter `DELETE` je použít příkaz:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Zatímco `DELETE` prohlášení TableAdapter produktu v našem původní DAL je mnohem jednodušší:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Jak můžete vidět, `WHERE` klauzuli v `DELETE` příkaz pro TableAdapter, který používá optimistickou metodu souběžného obsahuje porovnání mezi jednotlivými `Product` tabulka je existující hodnoty ve sloupcích a původní hodnoty v době Rutina GridView (nebo DetailsView nebo FormView) byl poslední naplněno. Od všech polí jinak než `ProductID`, `ProductName`, a `Discontinued` může mít `NULL` kontroly, další parametry a hodnoty jsou zahrnuty správně porovnat `NULL` hodnoty ve `WHERE` klauzule.

Jsme nebude přidávat žádné další DataTables optimistické datovou sadu souběžnosti povolené v tomto kurzu naší stránce s ASP.NET se poskytuje pouze aktualizace a odstranění informací o produktu. Ale ještě musíme přidat `GetProductByProductID(productID)` metodu `ProductsOptimisticConcurrency` TableAdapter.

K tomu, klikněte pravým tlačítkem na záhlaví TableAdapter (oblasti vpravo nahoře `Fill` a `GetProducts` metoda názvy) a v místní nabídce vyberte Přidat dotazu. Tím se spustí Průvodce konfigurací dotazu TableAdapter. Jak se naše TableAdapter počáteční konfigurace, opt k vytvoření `GetProductByProductID(productID)` metoda příkazu SQL ad-hoc (viz obrázek 4). Vzhledem k tomu `GetProductByProductID(productID)` metoda vrátí informace o konkrétní produkt, znamenat, že tento dotaz `SELECT` dotaz na typ, který vrací řádky.


[![Označit jako typ dotazu &quot;vyberte, které vrací řádky&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Obrázek 9**: označte typ dotazu jako "`SELECT` která vrací řádky" ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image27.png))


Na další obrazovce jsme se výzva k dotazu SQL pro použití s výchozím dotazem TableAdapter předem načtený. Posílení existující dotaz Zahrnout klauzuli `WHERE ProductID = @ProductID`, jak je znázorněno na obrázku 10.


[![Přidat WHERE klauzule předem načtený dotaz vrátit záznam určitý produkt](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Obrázek 10**: přidání `WHERE` klauzule Pre-Loaded dotaz vrátit záznam konkrétní produktu ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image30.png))


Nakonec, změňte názvy generované metoda k `FillByProductID` a `GetProductByProductID`.


[![Přejmenujte metody FillByProductID a GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Obrázek 11**: přejmenovat metody k `FillByProductID` a `GetProductByProductID` ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image33.png))


Pomocí tohoto průvodce dokončení TableAdapter teď obsahuje dvě metody pro načítání dat: `GetProducts()`, která vrátí *všechny* produkty; a `GetProductByProductID(productID)`, která vrací zadaný produkt.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Krok 3: Vytvoření vrstvy obchodní logiky pro optimistické DAL povoleno souběžnosti

Naše stávající `ProductsBLL` třída obsahuje příklady použití dávková aktualizace i přímé vzory DB. `AddProduct` Metoda a `UpdateProduct` přetížení i pomocí vzoru aktualizace batch předávání v `ProductRow` instance TableAdapter aktualizační metody. `DeleteProduct` Metody na druhé straně používá DB vzor přímé volání TableAdapter `Delete(productID)` metoda.

S novým `ProductsOptimisticConcurrency` TableAdapter, databáze přímé metody teď vyžadují, aby původní hodnoty předán také v. Například `Delete` metoda očekává teď deset vstupní parametry: původní `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, a `Discontinued`. Používá hodnoty těchto dalších vstupních parametrů v `WHERE` klauzuli `DELETE` příkaz odeslal do databáze, pouze pokud hodnoty aktuální databáze mapování až původního odstranění zadaný záznam.

Při podpis metody pro TableAdapter `Update` metoda použita ve vzorku aktualizace batch se nezměnilo, kód potřebné k zaznamenání původní a novými hodnotami. Proto místo pokusí použít optimistickou metodu povoleno souběžnosti DAL pomocí našich stávajících `ProductsBLL` třídy, vytvoříme novou třídu vrstvu obchodní logiky pro práci s naší nové DAL.

Přidejte třídu s názvem `ProductsOptimisticConcurrencyBLL` k `BLL` složky v rámci `App_Code` složky.


![Přidání třídy ProductsOptimisticConcurrencyBLL ke složce BLL](implementing-optimistic-concurrency-cs/_static/image34.png)

**Obrázek 12**: Přidat `ProductsOptimisticConcurrencyBLL` třídy ke složce BLL


Dál přidejte následující kód, který `ProductsOptimisticConcurrencyBLL` třídy:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Všimněte si použití `NorthwindOptimisticConcurrencyTableAdapters` příkaz nad spuštění deklaraci třídy. `NorthwindOptimisticConcurrencyTableAdapters` Obor názvů obsahuje `ProductsOptimisticConcurrencyTableAdapter` třídy, která poskytuje metody DAL. Také před deklaraci třídy najdete `System.ComponentModel.DataObject` atribut, který se dá pokyn, chcete-li zahrnout této třídy v rozevíracím seznamu Průvodce ObjectDataSource Visual Studio.

`ProductsOptimisticConcurrencyBLL`Na `Adapter` vlastnost poskytuje rychlý přístup k instanci `ProductsOptimisticConcurrencyTableAdapter` třídy a dodržuje způsobem používaným v našem původní BLL třídy (`ProductsBLL`, `CategoriesBLL`a tak dále). Nakonec `GetProducts()` metoda provede jednoduché volání DAL `GetProducts()` metodu a vrátí `ProductsOptimisticConcurrencyDataTable` objekt naplněný `ProductsOptimisticConcurrencyRow` instance pro každý produkt záznam v databázi.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Odstranění produktu pomocí vzoru přímé DB optimistickou metodu souběžného zpracování

Při použití vzoru DB přímé proti DAL, který používá optimistickou metodu souběžného, musí být předán nové a původní hodnota metody. Pro odstranění, nejsou žádné nové hodnoty, takže musí být předán pouze původní hodnoty. V našem BLL potom jsme musí přijmout všechny původní parametrů jako vstupní parametry. Umožňuje mít `DeleteProduct` metoda v `ProductsOptimisticConcurrencyBLL` třída použít metodu přímé DB. To znamená, že tato metoda je nutné provést ve všech polích data deset produktu jako vstupní parametry a předejte tyto vrstvy Dal, jak je znázorněno v následujícím kódu:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Pokud původní hodnoty - hodnoty, které byly naposledy načtené do GridView (nebo DetailsView nebo FormView) - liší od hodnoty v databázi, když uživatel klikne na tlačítko Odstranit `WHERE` klauzule nebude shodovat se záznam v databázi a žádné záznamy nebude mít vliv. Proto TableAdapter `Delete` metoda vrátí `0` a BLL `DeleteProduct` metoda vrátí `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualizace produktu pomocí vzoru aktualizace Batch optimistickou metodu souběžného zpracování

Jak jsme uvedli dříve, TableAdapter `Update` metoda pro vzor aktualizace batch se stejným podpisem metoda bez ohledu na to, zda je vzhledem k optimistickou metodu souběžného. Konkrétně `Update` metoda očekává DataRow, pole DataRows, DataTable nebo typové datové sady. Neexistují žádné další vstupní parametry pro zadání původní hodnoty. To je možné, protože DataTable uchovává informace o původní a upravené hodnoty pro jeho DataRow(s). Když DAL problémy jeho `UPDATE` příkaz, `@original_ColumnName` parametry se naplní původní hodnoty DataRow, zatímco `@ColumnName` parametry se naplní DataRow změny hodnot.

V `ProductsBLL` – třída (který se používá naše původní, optimistickou metodu souběžného zpracování DAL), při použití vzoru aktualizace batch aktualizovat informace o produktu našem kódu, provede následující posloupnost událostí:

1. Přečtěte si informace o aktuální databáze produktu do `ProductRow` pomocí TableAdapter `GetProductByProductID(productID)` – metoda
2. Přiřazení nové hodnoty, které mají `ProductRow` instance z kroku 1
3. Volání TableAdapter `Update` předávání v případě metody `ProductRow` instance

Toto pořadí kroků, ale nebude podporovat správně optimistickou metodu souběžného protože `ProductRow` zadané v kroku 1 je vyplněný přímo z databáze, což znamená, že původní hodnoty používané DataRow jsou ty, které momentálně existují v databáze a není na ty, které byly vázány na GridView na začátku procesu úprav. Místo toho, když pomocí optimistickou metodu souběžného zpracování povolenou DAL, je potřeba změnit `UpdateProduct` přetížení metody použít následující kroky:

1. Přečtěte si informace o aktuální databáze produktu do `ProductsOptimisticConcurrencyRow` pomocí TableAdapter `GetProductByProductID(productID)` – metoda
2. Přiřazení *původní* hodnoty k `ProductsOptimisticConcurrencyRow` instance z kroku 1
3. Volání `ProductsOptimisticConcurrencyRow` instance `AcceptChanges()` metoda, která nastaví DataRow, že jeho aktuální hodnoty jsou "" původního
4. Přiřazení *nové* hodnoty k `ProductsOptimisticConcurrencyRow` instance
5. Volání TableAdapter `Update` předávání v případě metody `ProductsOptimisticConcurrencyRow` instance

Krok 1 čtení ve všech hodnot v aktuální databázi pro záznam zadaná produktu. Tento krok je nadbytečné v `UpdateProduct` přetížení, která aktualizuje *všechny* sloupců produktu (jako tyto hodnoty budou přepsána v kroku 2), ale je nezbytné pro tyto přetížení, kde jsou pouze podmnožinu hodnoty ve sloupcích předaná jako vstupní parametry. Po přiřazení původní hodnoty `ProductsOptimisticConcurrencyRow` instance, `AcceptChanges()` metoda je volána, který označuje aktuální hodnoty DataRow jako původní hodnoty, které mají být používány `@original_ColumnName` parametry v `UPDATE` příkaz. V dalším kroku nové hodnoty parametrů jsou přiřazeny k `ProductsOptimisticConcurrencyRow` a nakonec `Update` metoda je volána, předávání v DataRow.

Následující kód ukazuje `UpdateProduct` přetížení, které přijímá všechna data produktu polí jako vstupní parametry. Když není tady, zobrazené `ProductsOptimisticConcurrencyBLL` třídy součástí staženého souboru pro tento kurz obsahuje také `UpdateProduct` přetížení, které přijímá jenom produktu název a cena jako vstupní parametry.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Krok 4: Předávání původní a novými hodnotami ze stránky ASP.NET BLL metody

DAL a BLL dokončení zbývá vytvořit stránku ASP.NET, který můžete využít logice optimistickou metodu souběžného založená na systému. Konkrétně data ovládací prvek webu (GridView, DetailsView nebo FormView) musí mějte na paměti, že jeho původní hodnoty a ObjectDataSource musí projít obě sady hodnot pro vrstvu obchodní logiky. Kromě toho musí být nakonfigurované stránku ASP.NET pro pohodlné zpracování porušení souběžnosti.

Začněte otevřením `OptimisticConcurrency.aspx` stránku `EditInsertDelete` složky a přidání GridView návrháře nastavení jeho `ID` vlastnost, která má `ProductsGrid`. Z prvku GridView inteligentních značek, opt k vytvoření nové ObjectDataSource s názvem `ProductsOptimisticConcurrencyDataSource`. Vzhledem k tomu, že chceme, aby tento ObjectDataSource používat DAL, který podporuje optimistickou metodu souběžného, nakonfigurujte ho na používání `ProductsOptimisticConcurrencyBLL` objektu.


[![Mít použití ObjectDataSource ProductsOptimisticConcurrencyBLL objekt](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Obrázek 13**: mít použití ObjectDataSource `ProductsOptimisticConcurrencyBLL` objektu ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image37.png))


Vyberte `GetProducts`, `UpdateProduct`, a `DeleteProduct` metody z rozevíracího seznamu v průvodci. Pro metodu UpdateProduct použijte přetížení, které přijímá všechna pole data produktu.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurace vlastností ovládacího prvku ObjectDataSource

Po dokončení průvodce, ObjectDataSource deklarativní by měl vypadat následovně:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Jak je vidět `DeleteParameters` kolekce obsahuje `Parameter` instance pro každý deset vstupní parametry v `ProductsOptimisticConcurrencyBLL` třídy `DeleteProduct` metoda. Podobně `UpdateParameters` kolekce obsahuje `Parameter` instance pro každý vstupní parametry v `UpdateProduct`.

Pro tyto předchozí návodů, které se podílejí úprava dat, doporučujeme odebrat ObjectDataSource `OldValuesParameterFormatString` vlastnost v tomto okamžiku, protože tato vlastnost určuje, že metoda BLL očekává staré (nebo původní) hodnoty, které mají být předán, jakož i s novými hodnotami. Kromě toho tato vlastnost hodnota označuje názvy vstupních parametrů pro původní hodnoty. Vzhledem k tomu, že jsme se předávání do BLL v původní hodnoty, proveďte *není* odebrat tuto vlastnost.

> [!NOTE]
> Hodnota `OldValuesParameterFormatString` vlastnost musí být mapována na vstupní parametr názvy v BLL, které očekávají původní hodnoty. Vzhledem k tomu, že jsme pojmenovali tyto parametry `original_productName`, `original_supplierID`a tak dále můžete nechat `OldValuesParameterFormatString` hodnotu vlastnosti jako `original_{0}`. Pokud však tyto metody BLL vstupní parametry měl názvy jako `old_productName`, `old_supplierID`a tak dále, je potřeba aktualizovat `OldValuesParameterFormatString` vlastnost `old_{0}`.


Neexistuje jeden nastavení konečné vlastnosti, které musí být provedeny v pořadí pro ObjectDataSource správně metody BLL předat původní hodnoty. Má ObjectDataSource [vlastnost ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) lze přiřadit k [jednu ze dvou hodnot](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -Výchozí hodnota. Tyto metody BLL původní vstupní parametry neodesílá původní hodnoty
- `CompareAllValues` -odesílat původní hodnoty na BLL metody; Tuto možnost zvolte, pokud používáte optimistickou metodu souběžného zpracování

Za chvíli nastavit `ConflictDetection` vlastnost `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurace prvku GridView vlastnosti a pole

S vlastnostmi ObjectDataSource správně nakonfigurovány zaměřme na nastavení GridView. Nejprve vzhledem k tomu, že chceme GridView pro podporu úpravy a odstranění, klikněte na zaškrtávací políčka Povolit úpravy a Povolit odstranění z prvku GridView inteligentních značek. Bude přidáno CommandField jejichž `ShowEditButton` a `ShowDeleteButton` jsou nastaveny na `true`.

Když vázána `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView obsahuje pole pro každou z produktu datová pole. Při takové GridView lze upravovat, je ale cokoli přijatelné činnost koncového uživatele. `CategoryID` a `SupplierID` BoundFields vykreslí jako textová pole, nutnosti uživatele k zadání čísla ID příslušnou kategorii a dodavatele. Bude žádné formátování pro číselná pole a zajistit, že zadaný název produktu a, do jednotkové ceny jednotek na skladě, jednotky v pořadí a změnit pořadí úrovně hodnoty jsou obě správné číselné hodnoty a jsou větší než nebo roven žádné ověřovací ovládací prvky na nulu.

Jak již bylo zmíněno *přidání ověřovací ovládací prvky pro úpravy a vkládání rozhraní* a *přizpůsobení rozhraní pro úpravu dat* kurzy, uživatelské rozhraní lze přizpůsobit pomocí Nahraďte BoundFields TemplateFields. Tato rutina GridView a jeho úpravy rozhraní jste došlo následujícími způsoby:

- Odebrat `ProductID`, `SupplierName`, a `CategoryName` BoundFields
- Převést `ProductName` BoundField na pole TemplateField a přidání ovládacího prvku RequiredFieldValidation.
- Převést `CategoryID` a `SupplierID` BoundFields k TemplateFields a upravit úpravy rozhraní používá DropDownLists namísto textových polí. V těchto TemplateFields `ItemTemplates`, `CategoryName` a `SupplierName` se zobrazují datová pole.
- Převést `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` BoundFields TemplateFields a přidání ovládacích prvků CompareValidator.

Vzhledem k tomu, že jste již jak provést tyto úlohy v předchozí kurzech zkontrolují, jsme I budete právě seznamu zde konečné deklarativní syntaxi a nechte implementace z hlediska.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Nemohli jsme velmi brzy bude dosaženo s příklad plně funkční. Existuje však několik odlišnosti, které budou ovládat nahoru a nám způsobit problémy. Kromě toho potřebujeme ještě některé rozhraní, která upozorní uživatele, když došlo k narušení souběžnosti.

> [!NOTE]
> Aby dat ovládací prvek webu správně předat původní hodnoty ObjectDataSource (které jsou potom předána BLL), je důležité, prvku GridView `EnableViewState` je nastavena na `true` (výchozí). Pokud zakážete stav zobrazení, jsou v postback ztraceny původní hodnoty.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Předání správné původní hodnoty ObjectDataSource

Existuje několik možných problémů s způsob, jakým GridView nebyl nakonfigurován. Pokud ObjectDataSource `ConflictDetection` je nastavena na `CompareAllValues` (jako je náš), když ObjectDataSource `Update()` nebo `Delete()` metody jsou vyvolány GridView (nebo DetailsView nebo FormView), ObjectDataSource pokusí zkopírovat Rutina GridView na původní hodnoty do její odpovídající `Parameter` instance. Odkazovat zpět na obrázku 2 pro grafické reprezentace tohoto procesu.

Konkrétně GridView původní hodnoty přiřazené hodnoty v příkazech obousměrné vazby dat pokaždé, když data je vázána GridView. Proto je důležité, že všechny požadované původní hodnoty jsou zachyceny pomocí obousměrné vazby dat a že jsou k dispozici ve formátu, převést.

Pokud chcete zjistit, proč je důležité, pozorně najdete na naší stránce v prohlížeči. Podle očekávání, GridView uvádí každý produkt pomocí tlačítka Upravit a odstranit v levém sloupci.


[![Produkty jsou uvedeny v GridView](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Obrázek 14**: produkty jsou uvedeny v GridView ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image40.png))


Pokud kliknete na tlačítko Odstranit pro některý z produktů, `FormatException` je vyvolána výjimka.


[![Probíhá pokus o odstranění produktu výsledky v FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Obrázek 15**: Probíhá pokus o odstranění Any produktu výsledky v `FormatException` ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image43.png))


`FormatException` Se vyvolá, když ObjectDataSource se pokusí přečíst v původním `UnitPrice` hodnotu. Vzhledem k tomu `ItemTemplate` má `UnitPrice` formátu měny (`<%# Bind("UnitPrice", "{0:C}") %>`), obsahuje symbol měny, jako je 19,95. `FormatException` Nastane jako ObjectDataSource pokusí převést tento řetězec do `decimal`. Chcete-li tento problém obejít, máme několik možností:

- Odebrat měna formátování z `ItemTemplate`. To znamená místo použití `<%# Bind("UnitPrice", "{0:C}") %>`, jednoduše použijte `<%# Bind("UnitPrice") %>`. Nevýhodou tohoto objektu je, že cenu už formátována.
- Zobrazení `UnitPrice` ve formátu měny v `ItemTemplate`, ale použít `Eval` – klíčové slovo chcete dosáhnout. Odvolat, `Eval` provede datové jednosměrné vazby. Musíme poskytují `UnitPrice` hodnotu pro původní hodnoty, proto budeme potřebovat stále příkaz obousměrný datové vazby v `ItemTemplate`, ale to se může v ovládacím prvku popisek webového jehož `Visible` je nastavena na `false`. V ItemTemplate jsme použít následující kód:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Odebrat měna formátování z `ItemTemplate`pomocí `<%# Bind("UnitPrice") %>`. V prvku GridView `RowDataBound` obslužné rutiny události, prostřednictvím kódu programu přístup k webu popisek řízení v rámci kterého `UnitPrice` hodnota se zobrazí a nastavit jeho `Text` vlastnost formátovaný verzi.
- Ponechejte `UnitPrice` formátu měny. V prvku GridView `RowDeleting` obslužné rutiny události, nahraďte existující původní `UnitPrice` hodnotu (19,95 USD) s k skutečné desetinnou hodnotu pomocí `Decimal.Parse`. Jsme viděli, jak provést něco podobného jako v `RowUpdating` obslužné rutiny událostí v [ *zpracování BLL - a výjimky na úrovni DAL na stránku ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) kurzu.

Moje například se rozhodli se druhý přístup přidání skrytá webové popisek řízení jehož `Text` vlastnost je obousměrný data vázaná na neformátovaný `UnitPrice` hodnotu.

Po řešení tohoto problému, zkuste to znovu kliknutím na tlačítko Odstranit pro některý z produktů. Tentokrát získáte `InvalidOperationException` kdy pokusí vyvolat BLL ObjectDataSource `UpdateProduct` metoda.


[![ObjectDataSource nelze najít metodu s vstupní parametry chce odeslání](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Obrázek 16**: ObjectDataSource nelze najít metodu s vstupní parametry chce odeslání ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image46.png))


Prohlížení zpráv v výjimky, je zřejmé, že ObjectDataSource chce vyvolání BLL `DeleteProduct` metoda, která zahrnuje `original_CategoryName` a `original_SupplierName` vstupní parametry. Důvodem je, že `ItemTemplate` s pro `CategoryID` a `SupplierID` TemplateFields aktuálně obsahovat obousměrné vazby příkazy s `CategoryName` a `SupplierName` datová pole. Místo toho je potřeba zahrnout `Bind` příkazy s `CategoryID` a `SupplierID` datová pole. K tomu, nahraďte existující příkazy vazby s `Eval` příkazy a poté přidejte skrytý popisek ovládací prvky, jejichž `Text` vlastnosti je vázána na `CategoryID` a `SupplierID` datových polí pomocí obousměrné vazby dat, jak je znázorněno níže:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Tyto změny se nám, teď moct úspěšně odstranit a upravit informace o produktu! V kroku 5 podíváme jak ověřit, jestli se porušení souběžnosti detekovaly. Ale prozatím trvat několik minut a zkuste to aktualizaci a odstraňování několik záznamů zajistit, že aktualizace a odstranění pro jednoho uživatele funguje podle očekávání.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Krok 5: Testování podporu optimistickou metodu souběžného zpracování

Chcete-li ověřit, že porušení souběžnosti probíhá zjištěné (nikoli výsledné v datech slepě přepsáním), je potřeba otevřít dvě okna prohlížeče na tuto stránku. V obou případech prohlížeče klikněte na tlačítko Upravit pro Chai. V právě jeden z prohlížečů, změňte název na "Chai čaj" a kliknutím na tlačítko Aktualizovat. Aktualizace by měla být úspěšné a vrátit GridView do předem úpravy stavu, s "Chai čaj" jako nový název produktu.

V jiných okno instance prohlížeče ale textového pole název produktu stále zobrazuje "Chai". V této druhé okno prohlížeče, aktualizovat `UnitPrice` k `25.00`. Bez podpory optimistickou metodu souběžného kliknutím na tlačítko Aktualizovat v druhé instance prohlížeče by změnit název produktu zpět na "Chai", čímž přepsání změny provedené při první instance prohlížeče. S optimistickou metodu souběžného těmto nekompatibilitám, ale kliknutím na tlačítko Aktualizovat na druhou instanci prohlížeče výsledkem [dbconcurrencyexception –](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Když se detekuje narušení souběžného zpracování, je vyvolána dbconcurrencyexception –](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Obrázek 17**: Pokud porušení souběžnosti se zjistí, `DBConcurrencyException` je vyvolána ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image49.png))


`DBConcurrencyException` Se pouze vyvolá, když se využívá vzor aktualizace batch DAL. Vzor přímé DB nevyvolá výjimku, jenom označuje, že měla vliv na žádné řádky. Pro znázornění je vrátí rutina GridView obou instancí prohlížeč stavu předem úpravy. V dalším kroku v první řadě prohlížeče, klikněte na tlačítko Upravit a změňte název produktu z "Chai čaj" do "Chai" a kliknutím na tlačítko Aktualizovat. V okně prohlížeče druhý klikněte na tlačítko Odstranit pro Chai.

Po kliknutí na tlačítko odstranit, je stránka odeslána zpět, GridView vyvolá ObjectDataSource `Delete()` metoda a ObjectDataSource volání `ProductsOptimisticConcurrencyBLL` třídy `DeleteProduct` metodu předáním podél původní hodnoty. Původní `ProductName` hodnotu pro druhou instanci prohlížeče je "Chai čaj", která se neshoduje s aktuálním `ProductName` hodnota v databázi. Proto `DELETE` prohlášení vydané k databázi ovlivňuje nulový počet řádků, protože neexistuje žádný záznam v databázi, `WHERE` splňuje klauzule. `DeleteProduct` Metoda vrátí `false` a data ObjectDataSource je odrážejí na GridView.

Z hlediska koncového uživatele kliknutím na tlačítko Odstranit pro čaj Chai v okně prohlížeče druhý způsobeno obrazovky, flash a při vracející se zpět, produkt je stále existuje, ale nyní je uveden jako "Chai" (produktu změnu názvu provedené první prohlížeče instance). Pokud uživatel klikne na tlačítko Odstranit znovu, bude odstranění úspěšné, jako je původní GridView `ProductName` hodnota ("Chai") se teď odpovídá s hodnotou v databázi.

V obou případech činnost koncového uživatele je daleko od ideálu. Jsme jasně nechcete, aby se uživateli zobrazí podrobnosti o nitty-gritty `DBConcurrencyException` výjimka při použití vzoru aktualizace dávky. A chování při použití vzoru přímé DB je poněkud matoucí, protože uživatelé příkaz se nezdařil, ale žádná přesné informace o důvod, proč se.

Chcete-li vyřešit tyto dva problémy, můžeme vytvořit popisek webové ovládací prvky na stránce, které poskytují vysvětlení k proto aktualizaci nebo odstranění se nezdařilo. Pro aktualizaci vzor batch, můžeme určit, jestli `DBConcurrencyException` došlo k výjimce v obslužné rutině události po úrovni prvku GridView zobrazení upozornění popisek podle potřeby. Pro metodu přímé DB jsme můžete zkontrolovat návratovou hodnotu metody BLL (což je `true` Pokud byl vliv na jeden řádek, `false` jinak) a zobrazit informační zpráva podle potřeby.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Krok 6: Přidání informační zprávy a jejich zobrazení při krátkodobém narušení souběžného zpracování

Když dojde k narušení souběžnosti, chování vystavených závisí na jestli použila DAL dávková aktualizace nebo přímé vzor DB. Naše kurz používá obě vzory, se vzorkem aktualizace batch používá pro aktualizaci a přímé vzor DB použít pro odstranění. Pokud chcete začít, umožňuje přidání dvě popisek na naší stránce, která vysvětlují, že při pokusu o odstranění nebo aktualizaci dat došlo k narušení souběžnosti. Nastavení ovládacího prvku popisek `Visible` a `EnableViewState` vlastnosti, které chcete `false`; to způsobí, že je jako skryté na každé stránce, navštivte, s výjimkou pro ty konkrétní stránky navštíví kde jejich `Visible` prostřednictvím kódu programu je vlastnost nastavena na `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Kromě nastavení jejich `Visible`, `EnabledViewState`, a `Text` vlastností, I taky nastavili `CssClass` vlastnost `Warning`, což způsobí, že popisek elementu na velká, red, kurzíva, tučné písmo. Tato šablon stylů CSS `Warning` třída byla definována a přidají se do Styles.css zpět v *zkoumání události spojené s vložení, aktualizace a odstranění* kurzu.

Po přidání tyto popisky, by měla vypadat podobně jako na obrázku 18 návrháře v sadě Visual Studio.


[![Byly přidány dva ovládací prvky popisek na stránku](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Obrázek 18**: dva popisek – ovládací prvky byly přidány na stránku ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image52.png))


Pomocí těchto popisek webových prvků zavedené, je vše připraveno prozkoumat určení při souběžnosti došlo k narušení, na který bodu odpovídající popisek `Visible` může být nastavena `true`, zobrazení informační zpráva.

## <a name="handling-concurrency-violations-when-updating"></a>Při aktualizaci zpracování porušení souběžnosti

Nejprve podíváme, jak pracovat s porušení souběžnosti při použití vzoru aktualizace dávky. Vzhledem k tomu, že takové porušení službou batch aktualizovat vzor Příčina `DBConcurrencyException` výjimka, která je vyvolána, je potřeba přidat kód pro naše stránka ASP.NET k určení zda `DBConcurrencyException` během procesu aktualizace došlo k výjimce. Pokud ano, by měl zobrazit zprávu vysvětlením uživatele, jejich změny nebyly uloženy, protože jiný uživatel měl stejná data mezi upravovat, když se spouští úpravy záznamu a při kliknutí na tlačítko Aktualizovat.

Jak jsme viděli v *zpracování BLL - a výjimky na úrovni DAL na stránku ASP.NET* kurz, takové výjimky můžete zjistil a potlačena v po úrovně obslužných rutin ovládacího prvku webového data. Proto je potřeba vytvořit obslužnou rutinu události pro prvku GridView `RowUpdated` událost, která ověří, zda se `DBConcurrencyException` byla vyvolána výjimka. Tuto obslužnou rutinu události je předán odkaz na všechny výjimky, která byla vygenerována během procesu aktualizace, jak je znázorněno v případě, že obslužná rutina kódu níže:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

V face z `DBConcurrencyException` zobrazí této obslužné rutiny události výjimky, `UpdateConflictMessage` popisek ovládacího prvku a znamená, že byla zpracována výjimka. S tímto kódem na místě, když při aktualizaci záznamu, dojde k narušení souběžnosti uživatele změny jsou ztraceny, vzhledem k tomu, že by přepsání jiné uživatelské úpravy ve stejnou dobu. GridView zejména, je vrácena do stavu před úpravy a vázána na aktuální data databáze. Tím dojde k aktualizaci GridView řádek s změny jiného uživatele, které byly dříve nejsou viditelné. Kromě toho `UpdateConflictMessage` popisek – ovládací prvek vysvětluje uživateli, co se právě stalo. Tato posloupnost událostí, které je popsané v 19 obrázek.


[![Uživatel s aktualizace jsou ztraceny stěně narušení souběžného zpracování](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Obrázek 19**: uživatele A s aktualizace jsou ztraceny stěně narušení souběžnosti ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> Alternativně místo GridView vrácením předem úpravy stavu, jsme může nechat GridView ve stavu úprav nastavením `KeepInEditMode` vlastnost předána-in `GridViewUpdatedEventArgs` objekt, který má hodnotu true. Pokud pořídíte tento přístup, ale být jistý, se svázat data, která mají GridView (vyvoláním jeho `DataBind()` metoda) tak, aby do rozhraní úpravy jsou načteny hodnoty jiný uživatel. Kód k dispozici ke stažení v tomto kurzu má tyto dva řádky kódu `RowUpdated` obslužné rutiny události komentované; jednoduše zrušte komentář u tyto řádky kódu tak, aby měl GridView zůstat v režimu úprav po porušení souběžnosti.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Při odstraňování neodpovídá na požadavky porušení souběžnosti

Přímé vzor DB už není žádná výjimka vyvolaná při krátkodobém narušení souběžnosti. Místo toho příkaz database jednoduše ovlivňuje žádné záznamy, jako klauzule WHERE se neshoduje s žádným záznamem. Všechny metody úpravy dat vytvořené v BLL byly navrženy tak, aby mohly vrátit logickou hodnotu udávající, zda jejich vliv na přesně jeden záznam. Proto, pokud chcete zjistit, pokud došlo k narušení souběžnosti při odstraňování záznamu, jsme Zkontrolujte návratovou hodnotu BLL `DeleteProduct` metoda.

Návratovou hodnotu metody BLL může být prověřen v prvku ObjectDataSource po úrovně obslužných rutin prostřednictvím `ReturnValue` vlastnost `ObjectDataSourceStatusEventArgs` objekt předaný do obslužné rutiny události. Vzhledem k tomu, že jsme zájem o určení návratovou hodnotou z `DeleteProduct` metody, je potřeba vytvořit obslužnou rutinu události pro ObjectDataSource `Deleted` událostí. `ReturnValue` Vlastnost je typu `object` a může být `null` Pokud došlo k výjimce a metodu byl přerušen dříve, než ji může vrátit hodnotu. Proto jsme měli nejdřív zajistit, aby `ReturnValue` vlastnost není `null` a je logická hodnota. Za předpokladu, že tato kontrola proběhne, ukážeme `DeleteConflictMessage` popisku řízení, pokud `ReturnValue` je `false`. Můžete to provést pomocí následujícího kódu:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

Při krátkodobém narušení souběžného zpracování požadavku na odstranění uživatele zrušeny. Aktualizovat GridView zobrazující, že změny, které došlo k chybě pro tento záznam mezi tím, kdy uživatel načíst stránku a zadá při kliknutí na tlačítko Odstranit. Když je porušení pravidel ukáže, `DeleteConflictMessage` popisek zobrazený, která vysvětluje, co došlo jenom (viz obrázek 20).


[![Uživatel s odstranění je zrušena při krátkodobém narušení souběžného zpracování](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Obrázek 20**: uživatele A s odstranění je zrušeno při krátkodobém narušení souběžnosti ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Souhrn

Příležitosti pro concurrency porušení existovat v každé aplikaci, která umožňuje víc souběžných uživatelům aktualizovat nebo odstranit data. Pokud tato porušování nejsou pozornost u, když se dva uživatelé současně aktualizace stejná data, kdo získá v poslední zápis "wins", přepsal jiný uživatel změní změny. Alternativně můžete vývojářům implementovat buď kontrola optimistickou metodu nebo pesimistické souběžnosti. Optimistické řízení souběžného předpokládá, že porušení souběžnosti jsou jen zřídka a jednoduše zakáže aktualizace nebo odstranění příkaz, který by představovalo narušení souběžnosti. Kontrola souběžnosti pesimistické předpokládá, že tento souběžnosti porušení často dochází a jednoduše odmítat jeden uživatel aktualizovat nebo odstranit příkaz není platný. S řízení pesimistické souběžnosti aktualizace záznamu zahrnuje zamykání, a zabrání všichni ostatní uživatelé z úpravy nebo odstranění záznamu je uzamčena.

Typové datové sady v rozhraní .NET poskytuje funkce pro podporu optimistické řízení souběžného. Konkrétně `UPDATE` a `DELETE` příkazy vydané do databáze zahrnout všechny sloupce tabulky, a zajistí tak, že aktualizace nebo odstranění pouze dojde v případě záznam na aktuální data shoduje s původní data uživatel měl při provedením jejich aktualizaci nebo odstranění. Jakmile DAL byl nakonfigurován pro podporu optimistickou metodu souběžného, je potřeba aktualizovat BLL metody. Kromě toho stránku ASP.NET, který zavolá BLL musí být nakonfigurované tak, aby ObjectDataSource načte původní hodnoty z jeho ovládací prvek webu dat a předává je do BLL.

Jak jsme viděli v tomto kurzu, implementace optimistické řízení souběžného ve webové aplikaci ASP.NET zahrnuje aktualizaci DAL a BLL a přidání podpory do stránky ASP.NET. Zda přidané činnost je vhodné investice čas a úsilí, závisí na vaší aplikace. Pokud máte zřídka souběžných uživatelů aktualizace dat, nebo data, která se aktualizují se liší od sebe navzájem, pak souběžnosti ovládací prvek není klíče problém. Pokud ale pravidelně máte více uživatelů na svém webu práce se stejnými daty, Kontrola souběžnosti pomáhají zabránit jeden uživatel aktualizace nebo odstranění bezděčně přepsání jiné osoby.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](customizing-the-data-modification-interface-cs.md)
> [další](adding-client-side-confirmation-when-deleting-cs.md)
