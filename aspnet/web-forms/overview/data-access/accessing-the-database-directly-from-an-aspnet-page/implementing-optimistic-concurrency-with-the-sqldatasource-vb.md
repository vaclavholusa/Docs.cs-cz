---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementace optimistickou metodu souběžného s SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu jsme zkontrolujte essentials optimistickou metodu souběžného řízení a poté zjistit, jak ji pomocí ovládacího prvku SqlDataSource implementovat.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e7e81b3f3a54596c033caa2cf75e5e3ec01764c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877548"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementace optimistickou metodu souběžného s SqlDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) nebo [stáhnout PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> V tomto kurzu jsme zkontrolujte essentials optimistickou metodu souběžného řízení a poté zjistit, jak ji pomocí ovládacího prvku SqlDataSource implementovat.


## <a name="introduction"></a>Úvod

V předchozím kurzu jsme se zaměřili na tom, jak přidat vložení, aktualizace a odstranění možnosti do ovládacího prvku SqlDataSource. Stručně řečeno, a zajistit tak tyto funkce potřebnou k zadejte odpovídající `INSERT`, `UPDATE`, nebo `DELETE` příkazu SQL v ovládacím prvku s `InsertCommand`, `UpdateCommand`, nebo `DeleteCommand` vlastnosti společně s odpovídající Parametry v `InsertParameters`, `UpdateParameters`, a `DeleteParameters` kolekce. Když tyto vlastnosti a kolekce lze zadat ručně, nabízí tlačítko Rozšířené s Průvodce konfigurace zdroje dat generovat `INSERT`, `UPDATE`, a `DELETE` na základě políčko příkazy, které budou automaticky vytvářet tyto příkazy `SELECT` příkaz.

Společně s generování `INSERT`, `UPDATE`, a `DELETE` příkazy políčko, dialogové okno pokročilé možnosti generování SQL nabízí možnost Použít optimistickou metodu souběžného (viz obrázek 1). Pokud je zaškrtnuto, `WHERE` klauzule v generován automaticky `UPDATE` a `DELETE` příkazy jsou upraveny umožní provádět jenom aktualizace nebo odstranění Pokud základní t ještě data databáze byl upraven od uživatele posledního načtení dat do mřížky.


![Můžete přidat podporu optimistickou metodu souběžného z rozšířené dialogové okno Možnosti generování SQL](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Obrázek 1**: můžete přidat podporu optimistickou metodu souběžného z rozšířené dialogové okno Možnosti generování SQL


Zpět v [implementace optimistickou metodu souběžného](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurzu jsme se zaměřili na základy optimistické řízení souběžného a jak ho přidat do ObjectDataSource. V tomto kurzu jsme budete retušovat na essentials optimistickou metodu souběžného řízení a poté zjistit, jak implementovat pomocí SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Rekapitulace optimistickou metodu souběžného zpracování

Pro webové aplikace, které umožňují víc souběžných uživatelům upravit nebo odstranit stejná data, existuje možnost, že jeden uživatel může nechtěně přepsat jiné změny s. V [implementace optimistickou metodu souběžného](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurzu I zadaný v následujícím příkladu:

Představte si, že dva uživatelé, Jisun a Sam, byly obě návštěvou stránky v aplikaci, která povolené návštěvníky aktualizovat a odstraňovat produkty pomocí ovládacího prvku GridView. Obě klikněte na tlačítko Upravit pro Chai přibližně ve stejnou dobu. Jisun změny názvu produktu Chai čaj a kliknutím na tlačítko Aktualizovat. Net výsledkem je `UPDATE` příkaz, který je odeslal do databáze, která nastaví *všechny* lze aktualizovat pole produktu s (i když Jisun aktualizuje jenom jedno pole `ProductName`). V tuto chvíli databáze má hodnoty Chai čaj, kategorie Nápoje, kapaliny exotické dodavatele pro tento konkrétní produkt a tak dále. Ale GridView na obrazovce s Sam stále zobrazuje název produktu upravovat řádku GridView jako Chai. Několik sekund poté, co Jisun s změny byly potvrzeny, Sam aktualizace kategorie přísady a klikne na tlačítko Aktualizovat. To vede `UPDATE` příkaz odeslal do databáze, která nastaví Chai, název produktu `CategoryID` na odpovídající ID kategorie přísady a tak dále. Změny s Jisun název produktu být přepsán.

Obrázek 2 ukazuje tato interakce.


[![Při dvou uživatelů současně aktualizovat záznam, existuje s potenciální pro jednoho uživatele s změny přepsat jiné s](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Obrázek 2**: když dva uživatelé současně aktualizace existuje záznam s potenciální pro jednoho uživatele s změny přepsat jiné s ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Tento scénář zabránit unfolding forma [Kontrola souběžnosti](http://en.wikipedia.org/wiki/Concurrency_control) , musí být implementována. [Optimistickou metodu souběžného](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) fokus tohoto kurzu funguje za předpokladu, že může být konfliktů souběžnosti every teď a potom, většina čas takové konflikty won t nastat. Proto pokud dojde ke konfliktu, optimistické řízení souběžného jednoduše informuje uživatele, že jejich t může změny uložit, protože jiný uživatel upravil stejná data.

> [!NOTE]
> Pro aplikace, kde se předpokládá, že bude velký počet konfliktů souběžnosti, nebo pokud takové konflikty nejsou přípustné pak pesimistické souběžnosti řízení lze použít místo. Odkazovat zpět [implementace optimistickou metodu souběžného](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) kurz podrobné podrobnější informace o řízení pesimistické souběžnosti.


Optimistické řízení souběžného funguje tak, že zajistíte, že bude aktualizován nebo odstraněn záznam má stejné hodnoty, stejně jako při aktualizaci nebo odstranění proces spuštění. Například při kliknutí na tlačítko Upravit v upravitelné GridView záznam s hodnotami se přečíst z databáze a zobrazují v textových polí a jiných webových ovládacích prvků. Tyto původní hodnoty se uloží prostřednictvím GridView. Později, až uživatel provede jeho změny a klikne na tlačítko Aktualizovat `UPDATE` příkazu použitému musí vzít v úvahu původní hodnoty a nové hodnoty a pokud původní hodnoty, že uživatel začali upravovat aktualizovat pouze základní záznamů databáze jsou identické s hodnotami stále v databázi. Obrázek 3 znázorňuje posloupnosti událostí.


[![Pro aktualizaci nebo odstranění úspěšné původní hodnoty musí být rovna aktuální hodnot v databázi](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Obrázek 3**: pro aktualizaci nebo odstranění hodnotu úspěch, původní hodnoty musí být rovna hodnot v aktuální databázi ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Existují různé přístupy k implementaci optimistickou metodu souběžného (najdete v části [Petr A. Bromberg](http://peterbromberg.net/) s [Optmistic souběžnosti aktualizace logiku](http://www.eggheadcafe.com/articles/20050719.asp) pro stručný pohled na řadu možností). Metoda používaná podle SqlDataSource (a ADO.NET typové datové sady použité v našem Data Access Layer) rozšiřuje `WHERE` klauzule zahrnout porovnání všechny původní hodnoty. Následující `UPDATE` prohlášení, například aktualizace název a cena produktu pouze v případě, že jsou stejné hodnoty, které byly původně načteny při aktualizaci záznamu v GridView hodnot v aktuální databázi. `@ProductName` a `@UnitPrice` parametry obsahovat nové hodnoty zadané uživatelem, zatímco `@original_ProductName` a `@original_UnitPrice` obsahují hodnoty, které byly původně načíst do GridView, pokud bylo stisknuto tlačítko Upravit:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Jak jsme zobrazí v tomto kurzu, povolení optimistické řízení souběžného s SqlDataSource je stejně jednoduché jako kontroluje zaškrtávací políčko.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Krok 1: Vytvoření SqlDataSource, který podporuje optimistickou metodu souběžného zpracování

Začněte otevřením `OptimisticConcurrency.aspx` stránku z `SqlDataSource` složky. Přetáhněte ovládací prvek SqlDataSource z panelu nástrojů na návrháře nastavení jeho `ID` vlastnost `ProductsDataSourceWithOptimisticConcurrency`. Potom klikněte na odkaz Konfigurace zdroje dat z ovládacího prvku s inteligentním. Na první obrazovce v průvodci vyberte pro práci s `NORTHWINDConnectionString` a klikněte na tlačítko Další.


[![Zvolte pro práci s NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Obrázek 4**: Zvolte práce se sadami `NORTHWINDConnectionString` ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


V tomto příkladu jsme přidali GridView, který umožňuje uživatelům upravovat `Products` tabulky. Proto z konfigurace obrazovce příkazu Select, vyberte `Products` tabulky z rozevíracího seznamu a vyberte `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce, jak je znázorněno na obrázku 5.


[![Vrátí ProductID, ProductName, UnitPrice a – starší formáty sloupce z tabulky produktů](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Obrázek 5**: Z `Products` tabulky, vraťte se `ProductID`, `ProductName`, `UnitPrice`, a `Discontinued` sloupce ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Po výběr sloupce, klikněte na tlačítko Upřesnit se otevře dialogové okno pokročilé možnosti generování SQL. Zkontrolujte generování `INSERT`, `UPDATE`, a `DELETE` příkazy pomocí zaškrtávacích políček optimistickou metodu souběžného a klikněte na tlačítko OK (odkazuje zpět na obrázku 1 pro snímek). Dokončete průvodce kliknutím na tlačítko Další a pak dokončit.

Po dokončení průvodce Konfigurace zdroje dat se za chvíli zkontrolujte výsledná `DeleteCommand` a `UpdateCommand` vlastnosti a `DeleteParameters` a `UpdateParameters` kolekce. Nejjednodušším způsobem je klikněte na kartě Zdroj v levém dolním rohu chcete podívat na stránku s deklarativní syntaxi. Zde najdete informace `UpdateCommand` hodnotu:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

S sedm parametry v `UpdateParameters` kolekce:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Podobně `DeleteCommand` vlastnost a `DeleteParameters` kolekce by měla vypadat takto:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Kromě rozšířit `WHERE` klauzulích `UpdateCommand` a `DeleteCommand` vlastnosti (a přidání dalších parametrů do kolekce příslušných parametrů), výběr použít optimistickou metodu souběžného zpracování možnost upraví dva další vlastnosti:

- Změny [ `ConflictDetection` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) z `OverwriteChanges` (výchozí) na `CompareAllValues`
- Změny [ `OldValuesParameterFormatString` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) z {0} (výchozí) na původní\_{0}.

Pokud data ovládací prvek webu vyvolá SqlDataSource s `Update()` nebo `Delete()` metoda, předává v původní hodnoty. Pokud SqlDataSource s `ConflictDetection` je nastavena na `CompareAllValues`, tyto původní hodnoty se přidají do příkazu. `OldValuesParameterFormatString` Vlastnost poskytuje vzoru pro pojmenovávání použít pro tyto parametry s původní hodnotou. Průvodce konfigurace zdroje dat používá původní\_{0} a názvy jednotlivých původní parametrů v `UpdateCommand` a `DeleteCommand` vlastnosti a `UpdateParameters` a `DeleteParameters` kolekce odpovídajícím způsobem.

> [!NOTE]
> Vzhledem k tomu, že jsme re nepoužíváte s řízení SqlDataSource, vkládání možnosti, můžete bez obav odstranit `InsertCommand` vlastnost a její `InsertParameters` kolekce.


## <a name="correctly-handlingnullvalues"></a>Správně zpracování`NULL`hodnoty

Bohužel rozšířená `UPDATE` a `DELETE` provést automaticky vygenerovanou příkazy v Průvodci nakonfigurujte zdroj dat při použití optimistickou metodu souběžného *není* pracovní se záznamy, které obsahují `NULL` hodnoty. Pokud chcete zjistit, proč, zvažte naše SqlDataSource s `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice` Sloupec v `Products` tabulka může mít `NULL` hodnoty. Pokud má záznam konkrétní `NULL` hodnota `UnitPrice`, `WHERE` klauzule část `[UnitPrice] = @original_UnitPrice` bude *vždy* vyhodnotit na hodnotu False, protože `NULL = NULL` vždy vrátí hodnotu False. Proto záznamy obsahující `NULL` hodnoty nelze upravit ani odstranit, jako `UPDATE` a `DELETE` příkazy `WHERE` klauzule won t vrátí všechny řádky aktualizovat nebo odstranit.

> [!NOTE]
> Tato chyba byla nejprve ohlášena společnosti Microsoft v červnu 2004 v [SqlDataSource generuje nesprávný příkazů SQL](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) a naplánoval údajně vyřešený v příští verzi technologie ASP.NET.


Chcete-li odstranit tento problém, musíme aktualizovat ručně `WHERE` klauzule v obou `UpdateCommand` a `DeleteCommand` vlastnosti **všechny** sloupce, které může mít `NULL` hodnoty. Obecně platí, změnit `[ColumnName] = @original_ColumnName` na:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Tato úprava můžete provedeny přímo prostřednictvím deklarativní prostřednictvím možnosti UpdateQuery nebo DeleteQuery v okně Vlastnosti nebo aktualizace a odstranění karty v určení vlastního příkazu SQL nebo uloženou proceduru možnost konfigurovat Data Průvodce zdrojem. Znovu, musí být tato změna provedena *každých* sloupec v `UpdateCommand` a `DeleteCommand` s `WHERE` klauzuli, která může obsahovat `NULL` hodnoty.

Toto použití na našem příkladu má za následek následující upravit `UpdateCommand` a `DeleteCommand` hodnoty:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Krok 2: Přidání GridView s upravit a odstranit možnosti

S SqlDataSource nakonfigurován pro podporu optimistickou metodu souběžného jen zbývá se přidat data ovládací prvek webu na stránku, která využívá tato kontrola souběžnosti. V tomto kurzu mohli s přidat GridView, která poskytuje i upravit a odstranit funkce. K tomu, přetáhněte GridView z panelu nástrojů na Designer a sadu jeho `ID` k `Products`. Ze inteligentní značky s GridView vytvořte mu vazbu k `ProductsDataSourceWithOptimisticConcurrency` prvek SqlDataSource přidali v kroku 1. Nakonec zkontrolujte možnosti Povolit úpravy a Povolit odstranění z inteligentních značek.


[![Vytvořit vazbu SqlDataSource GridView a povolit úpravy a odstraňování](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Obrázek 6**: vytvoření vazby GridView SqlDataSource a povolit úpravy a odstraňování ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


Po přidání GridView, nakonfigurujte její vzhled odebráním `ProductID` BoundField, změna `ProductName` BoundField s `HeaderText` vlastnosti produktu a aktualizace `UnitPrice` BoundField tak, aby jeho `HeaderText` vlastnost jednoduše cena. V ideálním případě jsme d vylepšení úpravy rozhraní, které chcete zahrnout RequiredFieldValidator pro `ProductName` hodnota a CompareValidator pro `UnitPrice` hodnotu (tak, aby byl s správně formátovaný číselnou hodnotu). Odkazovat [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurz pro podrobnější pohled na přizpůsobení s GridView úpravy rozhraní.

> [!NOTE]
> Rutina GridView, které stav zobrazení s musí být povolena, protože původní hodnoty z GridView předaný SqlDataSource jsou uloženy v zobrazení stavu.


Po provedení tyto úpravy, aby GridView, rutina GridView a SqlDataSource deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Optimistickou metodu souběžného ovládacího prvku akce najdete otevřete dvě okna prohlížeče a zatížení `OptimisticConcurrency.aspx` stránky v obou. Klikněte na tlačítka Upravit pro první produktu v obou prohlížeče. V jedné prohlížeče změňte název produktu a klikněte na tlačítko Aktualizovat. V prohlížeči bude odeslat zpět a GridView vrátí na jeho režimu předem úprav zobrazující nový název produktu pro záznam právě upravit.

V okně prohlížeče druhý změňte cenu (ale ponechte název produktu jako původní hodnotu) a klikněte na tlačítko Aktualizovat. Na zpětné volání mřížky vrátí do režimu jeho předem úprav, ale není zaznamenaná změna na cenu. Druhý prohlížeč zobrazí název nového produktu s stará cena stejnou hodnotu jako první. Změny provedené v okně prohlížeče druhý byly ztraceny. Kromě toho změny byly ztraceny místo tiše, jako tomu bylo žádná výjimka nebo zpráva označující, že právě došlo k porušení souběžnosti.


[![Změny v okně prohlížeče druhý byly bezobslužně ztraceny.](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Obrázek 7**: změny druhé prohlížeče okno bezobslužně ztratily ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


Důvod, proč druhý prohlížeče s změny nebyly potvrdí byl, protože `UPDATE` příkaz s `WHERE` klauzule odfiltrovat všechny záznamy a proto neovlivnila žádné řádky. Umožní s podívat `UPDATE` příkaz znovu:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Když druhý okna prohlížeče aktualizuje záznam, v zadán původní název produktu `WHERE` klauzule shody nemá t až s existující název produktu (protože byl změněn v první prohlížeči). Proto příkaz `[ProductName] = @original_ProductName` vrátí hodnotu False a `UPDATE` nemá vliv na žádné záznamy.

> [!NOTE]
> Odstraňte funguje stejným způsobem. Se dvě okna prohlížeče otevřete začněte upravit daný produkt s jedním a potom uložit jeho změny. Po uložení změn do jednoho prohlížeče, klikněte na tlačítko Odstranit pro stejný produkt v druhém. Vzhledem k tomu, že původní hodnoty na ochranu t v odpovídat `DELETE` příkaz s `WHERE` klauzuli odstranění bez upozornění selže.


Z pohledu s koncovým uživatelem v okně prohlížeče druhý po kliknutí na tlačítko Aktualizovat mřížky vrátí do režimu předem úprav, jejich změny se ale ztraceny. Ale zde s žádné visual zpětnou vazbu, která přilepit t dodán jejich změny. V ideálním případě Pokud uživatele s změny jsou ztraceny k narušení souběžnosti, jsme d upozorněním a pravděpodobně, zachovat mřížky v režimu úprav. Umožní s, podívejte se na tom, jak dosáhnout.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Krok 3: Určení, kdy došlo k narušení souběžného zpracování

Vzhledem k tomu, že porušení souběžnosti odmítne změny, které jeden udělal, je dobrý k upozornění uživatele, pokud došlo k narušení souběžnosti. Upozornit uživatele, přidání umožňují s ovládacího prvku popisek do horní části stránky s názvem `ConcurrencyViolationMessage` jejichž `Text` vlastnost zobrazí následující zprávu: Pokusili jste se aktualizovat nebo odstranit záznam, který byl současně aktualizován jiným uživatelem. Prosím zkontrolujte změny jiného uživatele a pak znovu proveďte aktualizace nebo odstranění. Nastavení ovládacího prvku popisek s `CssClass` vlastnost na upozornění, která je třídu CSS definovaná v `Styles.css` který zobrazí text písmeny červené, kurzíva, tučné a velké. Nakonec nastavený štítek s `Visible` a `EnableViewState` vlastnosti, které chcete `False`. To se skrýt štítek s výjimkou pouze postback, kde jsme explicitně nastavit jeho `Visible` vlastnost `True`.


[![Přidání ovládacího prvku popisek na stránku a zobrazit upozornění](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Obrázek 8**: Přidání ovládacího prvku popisek na stránku a zobrazit upozornění ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Při provádění aktualizace nebo odstranění, rutina GridView s `RowUpdated` a `RowDeleted` obslužné rutiny událostí fire po jeho ovládací prvek zdroje dat byla provedena požadovaná aktualizace nebo odstranění. Můžeme-li určit, kolik řádků situace měla vliv na operaci z těchto obslužných rutin událostí. Pokud byly nulový počet řádků, chceme zobrazit `ConcurrencyViolationMessage` popisek.

Vytvoření obslužné rutiny události pro oba `RowUpdated` a `RowDeleted` události a přidejte následující kód:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

V obou obslužné rutiny událostí jsme zkontrolujte `e.AffectedRows` vlastnost a pokud se rovná 0, nastavte `ConcurrencyViolationMessage` štítek s `Visible` vlastnost `True`. V `RowUpdated` obslužné rutiny události, jsme také dá pokyn GridView zůstane v režimu úprav nastavením `KeepInEditMode` vlastnost na hodnotu true. Za tím účelem, musíme rebind data k mřížce tak, aby ostatní uživatele s data načtená do rozhraní úpravy. To lze provést voláním GridView s `DataBind()` metoda.

Jak ukazuje obrázek 9, se v těchto obslužných rutinách událostí dvě znatelných zpráva se zobrazí, vždy, když dojde k narušení souběžnosti.


[![Zobrazí se zpráva při krátkodobém narušení souběžného zpracování](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Obrázek 9**: A zpráva se zobrazí při krátkodobém narušení souběžnosti ([Kliknutím zobrazit obrázek v plné velikosti](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Souhrn

Při vytváření webové aplikace, kde více souběžných uživatelů může být úpravy stejná data, je důležité zvážit možnosti řízení souběžnosti. Ve výchozím nastavení data technologie ASP.NET, které webové ovládací prvky a ovládací prvky zdroje dat není použít libovolný ovládací prvek souběžnosti. Jak jsme viděli v tomto kurzu, implementace optimistické řízení souběžného s SqlDataSource je poměrně rychle a snadno. Ve třídě SqlDataSource zpracovává většinu legwork pro vaše přidání rozšířen `WHERE` klauzule k generován automaticky `UPDATE` a `DELETE` jsou příkazy, ale existuje několik odlišnosti ve zpracování `NULL` hodnotu sloupce, jak je popsáno v Správně zpracování `NULL` hodnoty oddílu.

V tomto kurzu se ukončí součást SqlDataSource. Vrátí naše kurzy zbývající práce s daty pomocí ObjectDataSource a vrstvené architektura.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
