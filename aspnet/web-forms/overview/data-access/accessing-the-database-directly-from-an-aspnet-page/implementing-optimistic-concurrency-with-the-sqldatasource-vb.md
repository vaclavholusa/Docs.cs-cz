---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementace optimistického řízení souběžnosti ovládacím prvkem SqlDataSource (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme zkontrolujte essentials optimistického řízení souběžnosti a pak prozkoumejte, jak implementovat pomocí ovládacím prvkem SqlDataSource.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: a71e5ee06e4a970e7e6d6c3b5b0351ad218e0253
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391044"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementace optimistického řízení souběžnosti ovládacím prvkem SqlDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) nebo [stahovat PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> V tomto kurzu jsme zkontrolujte essentials optimistického řízení souběžnosti a pak prozkoumejte, jak implementovat pomocí ovládacím prvkem SqlDataSource.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme se zaměřili na tom, jak přidat vložení, aktualizace nebo odstranění možnosti ovládacím prvkem SqlDataSource. Stručně řečeno, a zajistit tak tyto funkce jsme potřebovali k určení odpovídajícího `INSERT`, `UPDATE`, nebo `DELETE` příkazu SQL v ovládacím prvku s `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` vlastnosti, společně s odpovídající v parametrech `InsertParameters`, `UpdateParameters`, a `DeleteParameters` kolekce. Zatímco tyto vlastnosti a kolekce lze zadat ručně, nabízí tlačítko Upřesnit s průvodce nakonfigurujte zdroj dat generovat `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávacího políčka, která bude automaticky vytvářet tyto příkazy na základě `SELECT` příkazu.

Spolu s generovat `INSERT`, `UPDATE`, a `DELETE` příkazy zaškrtávací políčko, dialogové okno Upřesnit možnosti generování SQL nabízí možnost použití optimistického řízení souběžnosti (viz obrázek 1). Pokud je zaškrtnuto, `WHERE` ustanovení název vygenerovaný automaticky `UPDATE` a `DELETE` jsou příkazy upravit tak, aby pouze proveďte aktualizaci nebo odstranění Pokud základní t nedostane data databáze byla změněna od uživatele posledního načtení dat do mřížky.


![Můžete přidat podporu optimistického řízení souběžnosti z rozšířené dialogové okno Možnosti generování SQL](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Obrázek 1**: můžete přidat podporu optimistického řízení souběžnosti z rozšířené dialogové okno Možnosti generování SQL


Zpátky [implementace optimistického řízení souběžnosti](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurzu jsme se zaměřili na základy optimistického řízení souběžnosti a přidání do ObjectDataSource. V tomto kurzu vytvoříme retušovat na základní optimistického řízení souběžnosti a pak prozkoumejte, jak implementovat pomocí ovládacím prvkem SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Rekapitulace toho optimistického řízení souběžnosti

Pro webové aplikace, které umožňují více souběžných uživatelů pro úpravy nebo odstranění stejná data, existuje možnost, že jeden uživatel může přepsat omylem jiné změny s. V [implementace optimistického řízení souběžnosti](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurzu mám k dispozici v následujícím příkladu:

Představte si, že dva uživatelé, Jisun a Sam, byly oba navštívit stránku v aplikaci, která návštěvníci, aktualizovat a odstraňovat produktům prostřednictvím ovládacího prvku GridView. Obě klikněte na tlačítko Upravit pro Chai přibližně ve stejnou dobu. Jisun změní název produktu na čaj Chai a klikne na tlačítko Aktualizovat. Net výsledek je `UPDATE` příkaz, který je odeslán do databáze, která nastavuje hodnoty *všechny* aktualizovatelné polí produktů s (i když Jisun aktualizovat jenom jedno pole `ProductName`). V tomto okamžiku databáze má hodnoty Chai kávy, kategorie Nápoje, dodavatel exotické kapalin, a tak dále pro tento konkrétní produkt. Ale GridView na obrazovce s Sam stále zobrazuje název produktu v řádku prvku GridView upravitelné jako Chai. Několik sekund poté, co Jisun s změny byly potvrzeny, Sam aktualizuje kategorii produkty koření a klikne na tlačítko Aktualizovat. Výsledkem je `UPDATE` příkaz odeslán do databáze, který nastaví název produktu Chai `CategoryID` na odpovídající ID kategorie produkty koření a tak dále. Jisun změn s názvem produktu mají přepsat.

Obrázek 2 znázorňuje tuto interakci.


[![Pokud dva uživatele najednou aktualizovat záznam existuje s potenciál pro jednoho uživatele s změní přepsat další prostředky](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Obrázek 2**: když dva uživatelé současně aktualizovat existuje záznam s potenciál pro jednoho uživatele s změny přepsat tím s ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


K této situaci zabránit unfolding určitou formu [řízení souběžnosti](http://en.wikipedia.org/wiki/Concurrency_control) musí být implementován. [Optimistická souběžnost](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) fokus v tomto kurzu funguje na předpokladu, že i když to může být konfliktů souběžnosti every a poté, většinu času takové konflikty je vyhráli t nastat. Proto pokud vzniknout konflikt, optimistického řízení souběžnosti jednoduše informuje uživatele, že jejich t může změny uložit, protože jiný uživatel upravil stejná data.

> [!NOTE]
> U aplikací, kde se předpokládá, že bude existovat mnoho konfliktů souběžnosti, nebo pokud takové konflikty je nejsou přípustné pak pesimistické řízení souběžnosti lze použít místo toho. Vraťte se do [implementace optimistického řízení souběžnosti](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurz podrobnější informace o pesimistické řízení souběžnosti.


Tím zajistíte, že záznam bude aktualizován nebo odstraněn má stejné hodnoty, stejně jako při aktualizaci nebo odstranění proces spuštění funguje optimistického řízení souběžnosti. Například při kliknutí na tlačítko Upravit v upravitelné prvku GridView, záznam s hodnotami jsou čtení z databáze a zobrazena v textových polí a dalších webových ovládacích prvcích. Tyto původní hodnoty jsou uloženy ve prvku GridView. Později, až uživatel provede své změny a klikne na tlačítko Aktualizovat `UPDATE` příkazu použitému musí vzít v úvahu původní hodnoty a nové hodnoty a aktualizovat pouze základní záznam databáze, pokud původní hodnoty, že uživatel zahájil úpravy jsou identické s hodnotami stále v databázi. Obrázek 3 znázorňuje tato posloupnost událostí.


[![Pro Update nebo Delete na úspěšné původní hodnoty musí být rovna aktuální hodnoty databáze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Obrázek 3**: For Update nebo Delete na hodnotu úspěch, původní hodnoty musí být rovna aktuální hodnot v databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Existují různé přístupy k implementace optimistického řízení souběžnosti (naleznete v tématu [Peter A. Bromberg](http://peterbromberg.net/) s [Optmistic souběžnosti aktualizace logiky](http://www.eggheadcafe.com/articles/20050719.asp) stručný přehled o na řadu možností). Argumentech techniku použít podle ovládacím prvkem SqlDataSource (a datové sady ADO.NET zadali používaných pro naše Data Access Layer) `WHERE` klauzule, která zahrnují porovnání všechny původní hodnoty. Následující `UPDATE` příkazu, například aktualizace názvu a cena produktu pouze v případě, že aktuální hodnoty v databázi jsou stejné hodnoty, které byly původně načteny při aktualizaci záznamu v prvku GridView. `@ProductName` a `@UnitPrice` parametry obsahovat nové hodnoty zadané uživatelem, zatímco `@original_ProductName` a `@original_UnitPrice` obsahují hodnoty, které byly původně načten do prvku GridView, když došlo ke kliknutí na tlačítko Upravit:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Jak uvidíme v tomto kurzu, povolení optimistického řízení souběžnosti ovládacím prvkem SqlDataSource je stejně jednoduché jako zaškrtnutím políčka.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Krok 1: Vytvoření SqlDataSource, který podporuje optimistické řízení souběžnosti

Začněte otevřením `OptimisticConcurrency.aspx` stránku ze `SqlDataSource` složky. Přetáhněte ovládací prvek SqlDataSource z panelu nástrojů do Návrháře nastavení jeho `ID` vlastnost `ProductsDataSourceWithOptimisticConcurrency`. Pak klikněte na odkaz Konfigurovat zdroj dat z ovládacího prvku s inteligentním. Na první obrazovce v Průvodci zvolte pracovat `NORTHWINDConnectionString` a klikněte na tlačítko Další.


[![Zvolte pro práci s NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Obrázek 4**: Zvolte možnost pro práci s `NORTHWINDConnectionString` ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


V tomto příkladu budeme přidávat prvku GridView, který umožňuje uživatelům upravovat `Products` tabulky. Proto z konfigurovat příkaz Select obrazovky, zvolte `Products` tabulky z rozevíracího seznamu a vyberte `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce, jak je znázorněno na obrázku 5.


[![Z tabulky produktů vrátíte ProductID, ProductName, UnitPrice a již nepoužívané sloupce](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Obrázek 5**: Z `Products` tabulky, vraťte se `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Po výběru sloupce, klikněte na tlačítko Upřesnit zobrazíte dialogové okno Upřesnit možnosti generování kódu SQL. Zkontrolujte, generování `INSERT`, `UPDATE`, a `DELETE` příkazy a zaškrtávacích políček optimistického řízení souběžnosti a klikněte na tlačítko OK (vrátit zpět na obrázku 1 pro snímek obrazovky). Dokončete průvodce kliknutím na další a pak dokončit.

Po dokončení Průvodce nakonfigurovat zdroj dat, věnujte chvíli zkontrolujte výsledný `DeleteCommand` a `UpdateCommand` vlastnosti a `DeleteParameters` a `UpdateParameters` kolekce. Nejjednodušší způsob je na kartě Zdroj v levém dolním rohu zobrazíte na stránce s deklarativní syntaxe. Zde najdete informace `UpdateCommand` hodnotu:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Sedm parametrů v `UpdateParameters` kolekce:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Podobně platí `DeleteCommand` vlastnost a `DeleteParameters` kolekce by měl vypadat nějak takto:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Kromě rozšíření `WHERE` klauzulích `UpdateCommand` a `DeleteCommand` vlastnosti (a přidání dalších parametrů do příslušného parametru kolekce), výběr optimistického řízení souběžnosti možnost upraví dva další použití vlastnosti:

- Změny [ `ConflictDetection` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) z `OverwriteChanges` (výchozí) pro `CompareAllValues`
- Změny [ `OldValuesParameterFormatString` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) z {0} (výchozí) s původním\_ {0} .

Když data webový ovládací prvek vyvolá SqlDataSource s `Update()` nebo `Delete()` metody předá v původní hodnoty. Pokud SqlDataSource s `ConflictDetection` je nastavena na `CompareAllValues`, tyto původní hodnoty se přidají do příkazu. `OldValuesParameterFormatString` Vlastnost poskytuje pojmenování používaným pro tyto parametry s původní hodnotou. Průvodce konfigurace zdroje dat využívá původní\_ {0} a názvy každý parametr původní `UpdateCommand` a `DeleteCommand` vlastnosti a `UpdateParameters` a `DeleteParameters` kolekce odpovídajícím způsobem.

> [!NOTE]
> Protože jsme znovu bez použití s ovládací prvek SqlDataSource vložení funkce, můžete bez obav odstranit `InsertCommand` vlastnost a její `InsertParameters` kolekce.


## <a name="correctly-handlingnullvalues"></a>Správné zpracování`NULL`hodnoty

Bohužel rozšířená `UPDATE` a `DELETE` příkazy automaticky generované průvodcem konfigurace zdroje dat při použití optimistického řízení souběžnosti provést *není* práce se záznamy, které obsahují `NULL` hodnoty. Pokud chcete zjistit, proč, vezměte v úvahu s naší SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice` Sloupec `Products` tabulka může mít `NULL` hodnoty. Pokud má konkrétní záznam `NULL` hodnota `UnitPrice`, `WHERE` klauzule část `[UnitPrice] = @original_UnitPrice` bude *vždy* vyhodnotit na hodnotu False, protože `NULL = NULL` vždy vrátí hodnotu False. Proto se záznamy, které obsahují `NULL` hodnoty nelze upravovat ani odstranit, jako `UPDATE` a `DELETE` příkazy `WHERE` klauzule vyhráli návratový t všechny řádky, aktualizovat nebo odstranit.

> [!NOTE]
> Tato chyba byla poprvé oznámil společnosti Microsoft v června 2004 v [SqlDataSource generuje nesprávné SQL příkazy](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) a údajně naplánované vyřešen v příští verzi technologie ASP.NET.


To pokud chcete napravit, budeme muset ručně aktualizovat `WHERE` klauzule v obou `UpdateCommand` a `DeleteCommand` vlastnosti **všechny** sloupce, které může mít `NULL` hodnoty. Obecně platí, změňte `[ColumnName] = @original_ColumnName` na:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Tuto změnu můžete provést přímo prostřednictvím deklarativní, prostřednictvím možností UpdateQuery nebo DeleteQuery z okna vlastnosti nebo aktualizace a odstranění karty v určení vlastní příkaz SQL nebo uloženou proceduru možnost konfigurovat Data Průvodce zdroje. Znovu, musí být tato změna provedena *každý* sloupec `UpdateCommand` a `DeleteCommand` s `WHERE` klauzuli, která může obsahovat `NULL` hodnoty.

Použití to pro náš příklad výsledky do následujícího změnil `UpdateCommand` a `DeleteCommand` hodnoty:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Krok 2: Přidání prvek GridView s upravit a odstranit možnosti

S ovládacím prvkem SqlDataSource nakonfigurovaný tak, aby podporují optimistickou souběžnost už jen zbývá k přidání dat webový ovládací prvek na stránce, která využívá tento ovládací prvek souběžnosti. Pro účely tohoto kurzu nechte s přidání prvku GridView, která obsahuje oba úpravy a odstranění. K tomu, přetáhněte GridView z panelu nástrojů do návrháře a nastavte jeho `ID` k `Products`. Z inteligentních značek GridView s vytvořte mu vazbu k `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource řízení přidali v kroku 1. Nakonec zaškrtněte možnost Povolit úpravy a Povolit odstranění z inteligentních značek.


[![Svázat s ovládacím prvkem SqlDataSource prvku GridView a povolte úpravy a odstranění](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Obrázek 6**: svázat SqlDataSource a povolit úpravy a odstranění prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


Po přidání prvku GridView, nakonfigurujte její vzhled tak, že odeberete `ProductID` Vlastnost BoundField, změna `ProductName` Vlastnost BoundField s `HeaderText` na produkt a aktualizuje `UnitPrice` Vlastnost BoundField tak, aby jeho `HeaderText` vlastnost je jednoduše cena. V ideálním případě d jsme vylepšení rozhraní zahrnout RequiredFieldValidator pro úpravy `ProductName` hodnotu a CompareValidator pro `UnitPrice` hodnotu (abyste měli jistotu, že s správně formátovaná číselnou hodnotu). Odkazovat [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurz najdete podrobnější rozbor přizpůsobení GridView s úpravy rozhraní.

> [!NOTE]
> Prvku GridView, které stav zobrazení s musí být povolena, protože jsou původní hodnoty předané z prvku GridView. na ovládacím prvkem SqlDataSource uložené v zobrazení stavu.


Po provedení těchto změn do prvku GridView, ovládacími prvky GridView a SqlDataSource deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Chcete-li zobrazit ovládací prvek optimistického řízení souběžnosti v akci, otevře dvě okna prohlížeče a načíst `OptimisticConcurrency.aspx` stránky v obou. Kliknutím na tlačítka Upravit pro první produktu v prohlížeči. V jeden prohlížeč změňte název produktu a klikněte na aktualizovat. Bude odeslat zpět v prohlížeči a prvku GridView vrátí předem úpravy režimu, zobrazuje nový název produktu pro záznam upravit.

V druhém okně prohlížeče se změní cena (ale ponechejte tuto položku jako původní hodnotu název produktu) a kliknutím na tlačítko Aktualizovat. Zpětné volání mřížky vrátí do režimu jeho předem úprav, ale změna ceny není zaznamenána. Druhý prohlížeč zobrazí stejnou hodnotu jako první z nich nový název produktu se stará cena. Změny provedené v druhém okně prohlížeče byly ztraceny. Kromě toho změny byly ztraceny spíše tiše, protože žádná výjimka nebo zpráva oznamující, že právě došlo k narušení souběžného zpracování.


[![Změny v druhém okně prohlížeče se bezobslužném režimu ztráty](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Obrázek 7**: The změny druhý prohlížeče okno tiše ztracených ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


Důvod, proč druhý prohlížeče s změny nebyly potvrzeny byl, protože `UPDATE` příkaz s `WHERE` klauzule vyfiltrovala všechny záznamy a proto neovlivnila žádné řádky. Umožní s podívat `UPDATE` příkaz znovu:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Při druhém okně prohlížeče aktualizuje záznam, původní název produktu podle `WHERE` klauzuli match kódu t nahoru s existující název produktu (protože ji změnil první prohlížeče). Proto příkaz `[ProductName] = @original_ProductName` vrátí hodnotu False a `UPDATE` nemá vliv na všechny záznamy.

> [!NOTE]
> Odstraňte funguje stejným způsobem. Se dvě okna prohlížeče otevřete začněte úpravou daný produkt s jednou a následně uložit své změny. Po uložení změn v jeden prohlížeč, klikněte na tlačítko Odstranit pro stejný produkt v jiném. Protože původní hodnoty don t shodují v `DELETE` příkaz s `WHERE` klauzule odstranění bez upozornění selže.


Z pohledu koncového uživatele s v druhém okně prohlížeče se po kliknutí na tlačítko Aktualizace mřížky vrátí do režimu předem úpravy, ale jejich změny byly ztraceny. Ale tam s žádný vizuální zpětnou vazbu zůstat t nefungoval jejich změny. V ideálním případě jestli uživatele s změny se ztratí k narušení souběžného zpracování, d upozorněním a, možná zachovat mřížky v režimu úprav. Podívejte se na tom, jak to provést s let.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Krok 3: Určení, kdy došlo k narušení souběžného zpracování

Protože narušení souběžného zpracování odmítne změny, které jeden provedl, by upozornit uživatele, že došlo k narušení souběžného zpracování. Upozornit uživatele, umožňují s přidání ovládacího prvku popisku k hornímu okraji stránky s názvem `ConcurrencyViolationMessage` jehož `Text` vlastnost se zobrazí následující zpráva: Pokusili jste se aktualizovat nebo odstranit záznam, který byl zároveň aktualizována jiným uživatelem. Prosím zkontrolujte změny dalších uživatelů a potom znovu provést aktualizaci nebo odstranění. Nastavení ovládacího prvku popisek s `CssClass` vlastnost na upozornění, která třídu šablony stylů CSS je definována v `Styles.css` , který zobrazí text červené, kurzíva, tučné písmo a velké písmem. Nakonec nastavte popisek s `Visible` a `EnableViewState` vlastností `False`. To se skrýt popisek s výjimkou pouze postbacků, kde jsme explicitně nastavit jeho `Visible` vlastnost `True`.


[![Přidání ovládacího prvku popisek na stránku a zobrazí varování](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Obrázek 8**: Přidání ovládacího prvku popisek na stránku a zobrazí varování ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Při aktualizaci nebo odstranění, GridView s `RowUpdated` a `RowDeleted` obslužné rutiny událostí aktivuje po jeho ovládací prvek zdroje dat byla provedena požadovaná aktualizace nebo odstranění. Můžeme-li určit, kolik řádků vliv na operace z těchto obslužných rutin událostí. Pokud byly nulový počet řádků, chceme zobrazit `ConcurrencyViolationMessage` popisek.

Vytvořte obslužnou rutinu události pro obě `RowUpdated` a `RowDeleted` události a přidejte následující kód:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

V obou obslužné rutiny událostí zkontrolujeme `e.AffectedRows` vlastnost a pokud se rovná 0, nastavte `ConcurrencyViolationMessage` popisek s `Visible` vlastnost `True`. V `RowUpdated` obslužná rutina události, jsme také dáte pokyn, aby GridView zůstat v režimu úprav nastavením `KeepInEditMode` vlastnost na hodnotu true. Přitom potřebujeme obnovení vazby dat k mřížce tak, aby ostatní uživatele s data se načtou do rozhraní pro úpravy. To lze provést zavoláním GridView s `DataBind()` metody.

Jak znázorňuje obrázek 9, se tyto dvě obslužné rutiny a zobrazí se zpráva znatelných pokaždé, když dojde k narušení souběžného zpracování.


[![Zobrazí se zpráva i v případě narušení souběžného zpracování](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Obrázek 9**: zpráva se zobrazí i v případě narušení souběžného zpracování ([kliknutím ji zobrazíte obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Souhrn

Při vytváření webové aplikace, kde více souběžných uživatelů může být úpravy stejná data, je důležité vzít v úvahu možnosti řízení souběžnosti. Datům v technologii ASP.NET, které webové ovládací prvky a ovládací prvky zdroje dat ve výchozím nastavení nepoužívají žádné řízení souběžnosti. Jak jsme viděli v tomto kurzu, implementace optimistického řízení souběžnosti ovládacím prvkem SqlDataSource je poměrně rychlé a snadné. Ve třídě SqlDataSource zpracovává většinu legwork pro vaše přidání rozšířených `WHERE` klauzulí k název vygenerovaný automaticky `UPDATE` a `DELETE` jsou příkazy, ale existuje několik odlišností při zpracování `NULL` hodnotu sloupce, jak je popsáno v Správné zpracování `NULL` hodnoty oddílu.

V tomto kurzu končí naše zkoumání ve třídě SqlDataSource. Vrátí našich kurzů pro zbývající práce s daty pomocí prvku ObjectDataSource a vrstvené architektury.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
