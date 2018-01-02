---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: "Pomocí závislosti mezipaměti SQL (VB) | Microsoft Docs"
author: rick-anderson
description: "Nejjednodušší strategie ukládání do mezipaměti je umožnit data uložená v mezipaměti vyprší po zadaném časovém období. Ale tento jednoduchý přístup znamená, že data uložená v mezipaměti maintai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 46521f48d31414ffff2707986d6f869ca2f9bc9a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-sql-cache-dependencies-vb"></a>Pomocí závislosti mezipaměti SQL (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) nebo [stáhnout PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> Nejjednodušší strategie ukládání do mezipaměti je umožnit data uložená v mezipaměti vyprší po zadaném časovém období. Ale tento jednoduchý přístup znamená, že data uložená v mezipaměti udržuje žádné přidružení jeho základní zdroj dat, výsledkem je zastaralá data, která trvá příliš dlouho nebo aktuální data, která je platnost příliš brzy vyprší. Lepším řešením je použití třídy SqlCacheDependency tak, aby data zůstala v mezipaměti, dokud podkladová data změnila ve službě SQL database. Tento kurz ukazuje, jak.


## <a name="introduction"></a>Úvod

Ukládání do mezipaměti techniky ověřuje při [ukládání dat pomocí ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) a [ukládání dat v architektuře](caching-data-in-the-architecture-vb.md) kurzy používá vypršení platnosti založené na čase vyřazení dat z mezipaměti po zadané období. Tento přístup je nejjednodušší způsob, jak vyrovnávání zvýšení výkonu ukládání do mezipaměti pro data typu prošlostí. Výběrem a čas vypršení platnosti *x* sekund, concedes vývojář stránky se má využívat výhod ukládání do mezipaměti pro pouze výkonu *x* sekund, ale můžete easy rest, že jeho data nikdy budou zastaralé delší než maximální z *x* sekund. Samozřejmě pro statických dat *x* lze rozšířit na dobu životnosti webové aplikace, jako byl zkontrolován v [ukládání dat při spuštění aplikace](caching-data-at-application-startup-vb.md) kurzu.

Při ukládání do mezipaměti data databáze, na základě času vypršení platnosti je často zvolené pro jeho snadné použití, ale je často nedostatečné řešení. V ideálním případě databázových dat by zůstanou v mezipaměti, dokud v základních datech se změnilo v databázi. pak by mezipaměti vyloučeno. Tento přístup maximalizuje výkon výhod ukládání do mezipaměti a minimalizuje trvání zastaralá data. Ale aby bylo možné získejte tyto výhody existuje musí být některé systému na místě, že zná, když zdrojová data databáze byla změněna a vyloučí odpovídající položky z mezipaměti. Před aplikaci ASP.NET 2.0 byly stránky vývojáři zodpovědný za implementaci tohoto systému.

Poskytuje technologie ASP.NET 2.0 [ `SqlCacheDependency` třída](https://msdn.microsoft.com/en-us/library/system.web.caching.sqlcachedependency.aspx) a může být vyřazena potřebnou infrastrukturu k určení, kdy došlo ke změně v databázi tak, aby příslušné položky v mezipaměti. Existují dvě metody pro určení, kdy došlo ke změně v základních datech: oznámení a dotazování. Po hovoříte o rozdílech mezi oznámení a dotazování, vytvoříme infrastruktury nezbytné pro podporu cyklického dotazování a potom zkoumat použití `SqlCacheDependency` třídy v deklarativní a programově scénáře.

## <a name="understanding-notification-and-polling"></a>Principy oznámení a dotazování

Existují dvě techniky, které lze použít k určení, kdy data v databázi byla změněna: oznámení a dotazování. Díky oznámení databáze automaticky upozorní modulem runtime ASP.NET, když se výsledky konkrétní dotaz se změnily od posledního spustit dotaz, v tomto okamžiku jsou vyřazování položek v mezipaměti přidruženou k dotazu. Pomocí cyklického dotazování, databázový server uchovává informace o Pokud máte konkrétní tabulky poslední aktualizace. Modul runtime ASP.NET pravidelně se dotazuje databáze ke kontrole tabulky, které je změnily vzhledem k tomu, že jste zadali do mezipaměti. Tyto tabulky, jejichž data změnila obsahovat jejich přidružené mezipaměti položky vyřazování.

Možnost oznámení vyžaduje méně nastavení než cyklického dotazování a je podrobnější, protože ji sleduje změny na úrovni dotazu a nikoli na úrovni tabulky. Bohužel oznámení jsou k dispozici pouze v úplné edicích systému Microsoft SQL Server 2005 (tj, edice bez Express). Možnost dotazování však lze použít pro všechny verze systému Microsoft SQL Server z 7.0 2005. Vzhledem k tomu, že tyto kurzy použít edice Express systému SQL Server 2005, se zaměříme na nastavení a pomocí možnosti cyklického dotazování. Na konci tohoto kurzu další prostředky v systému SQL Server 2005 s oznámení možnosti najdete v části Další čtení.

Pomocí cyklického dotazování, je nutné nakonfigurovat databázi patří do tabulky s názvem `AspNet_SqlCacheTablesForChangeNotification` který má tři sloupce - `tableName`, `notificationCreated`, a `changeId`. Tato tabulka obsahuje řádek pro každou tabulku, která obsahuje data, která by mohla potřebovat použít v mezipaměti závislost SQL ve webové aplikaci. `tableName` Sloupce určuje název tabulky při `notificationCreated` označuje datum a čas řádek byl přidán do tabulky. `changeId` Sloupec je typu `int` a má počáteční hodnotu 0. Jeho hodnota se zvyšuje s každou změny v tabulce.

Kromě `AspNet_SqlCacheTablesForChangeNotification` tabulky databáze také musí obsahovat aktivační události na každé z tabulek, které se mohou objevit v mezipaměti závislost SQL. Tyto triggery jsou vykonány vždy, když je řádek vložit, aktualizovat nebo odstranit a zvýšit v tabulce s `changeId` hodnotu `AspNet_SqlCacheTablesForChangeNotification`.

Modul runtime ASP.NET sleduje aktuální `changeId` pro tabulku při ukládání do mezipaměti pomocí data `SqlCacheDependency` objektu. Databáze se pravidelně kontroluje a jakýkoli `SqlCacheDependency` objekty, jehož `changeId` se liší od hodnoty v databázi se vyřazování od liší `changeId` hodnota znamená, že došlo ke změně v tabulce vzhledem k tomu, že data se ukládá do mezipaměti.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Krok 1: Zkoumat`aspnet_regsql.exe`příkazového řádku programu

S přístupem dotazování databáze musí být instalační program tak, aby obsahovala infrastruktury popsané výše: předdefinované tabulce (`AspNet_SqlCacheTablesForChangeNotification`), několik uložených procedur a aktivačních událostí jednotlivých tabulek, které je možné použít závislosti mezipaměti SQL na webu aplikace. Tyto tabulky, uložených procedur a aktivačních událostí lze vytvořit prostřednictvím příkazového řádku programu `aspnet_regsql.exe`, který se nachází v `$WINDOWS$\Microsoft.NET\Framework\version` složky. Chcete-li vytvořit `AspNet_SqlCacheTablesForChangeNotification` tabulky a přidružené uložené procedury, spusťte následující z příkazového řádku:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Spuštění těchto příkazů přihlášení k zadané databázi musí být v [ `db_securityadmin` ](https://msdn.microsoft.com/en-us/library/ms188685.aspx) a [ `db_ddladmin` ](https://msdn.microsoft.com/en-us/library/ms190667.aspx) role. K prozkoumání odeslal do databáze pomocí T-SQL `aspnet_regsql.exe` příkazového řádku programu, použijte [tuto položku blogu](http://scottonwriting.net/sowblog/posts/10709.aspx).


Například přidejte infrastrukturu pro dotazování do databáze Microsoft SQL Server s názvem `pubs` na serveru databáze s názvem `ScottsServer` pomocí ověřování systému Windows, přejděte do příslušného adresáře a z příkazového řádku, zadejte:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Po přidání infrastruktury úroveň databáze, je potřeba přidat do těchto tabulek, které se použijí v mezipaměti závislosti SQL aktivačních událostí. Použít `aspnet_regsql.exe` příkazového řádku programu znovu, ale zadat pomocí názvu tabulky `-t` přepínače a místo `-ed` přepínače použijte `-et`, takto:


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Přidání aktivačních událostí k `authors` a `titles` tabulky na `pubs` databáze na `ScottsServer`, použijte:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

V tomto kurzu přidat aktivačních událostí k `Products`, `Categories`, a `Suppliers` tabulky. Podíváme syntaxe konkrétní příkazového řádku v kroku 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Krok 2: Odkazování na databázi Microsoft SQL Server 2005 Express Edition v`App_Data`

`aspnet_regsql.exe` Příkazového řádku program vyžaduje název databáze a serveru chcete-li přidat infrastruktury potřebné dotazování. Jaký je název databáze a serveru pro který se nachází v databázi Microsoft SQL Server 2005 Express, ale `App_Data` složky? Místo zjistit, co jsou názvy databáze a serveru, I sunout zjistil, že je nejjednodušší je připojit databázi k `localhost\SQLExpress` databáze instance a přejmenujte dat pomocí [SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/ms174173.aspx). Pokud nemáte úplné verze systému SQL Server 2005 v počítači nainstalován, pak pravděpodobně už máte v počítači nainstalovaný SQL Server Management Studio. Pokud máte pouze edice Express, si můžete stáhnout bezplatnou [aktualizace Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Spusťte ukončením sady Visual Studio. Dále otevřete SQL Server Management Studio a zvolte pro připojení k `localhost\SQLExpress` serveru pomocí ověřování systému Windows.


![Připojení k localhost\SQLExpress serveru](using-sql-cache-dependencies-vb/_static/image1.gif)

**Obrázek 1**: připojení k `localhost\SQLExpress` serveru


Po připojení k serveru, bude Management Studio zobrazit serveru a mít podsložky pro databáze, zabezpečení a tak dále. Klikněte pravým tlačítkem na složku databází a vyberte možnost připojit. Tím se otevře dialogové okno Připojit databáze (viz obrázek 2). Kliknutím na tlačítko Přidat a vyberte `NORTHWND.MDF` složka databáze ve vaší webové aplikace s `App_Data` složky.


[![Připojte NORTHWND. MDF databáze ze složky App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Obrázek 2**: připojení `NORTHWND.MDF` databáze z `App_Data` složky ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image2.png))


Databáze se přidá do složku databáze. Název databáze může být úplná cesta k souboru databáze nebo se přidá jako předpona úplnou cestu [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Abyste se vyhnuli nutnosti zadejte název této zdlouhavé databáze při použití aspnet\_regsql.exe nástroj příkazového řádku, přejmenovat databázi lidských popisný název kliknutím pravým tlačítkem na databázi právě připojit a zvolením přejmenovat. I sunout přejmenován na DataTutorials databáze.


![Přejmenovat databázi připojené k lidské popisný název](using-sql-cache-dependencies-vb/_static/image3.gif)

**Obrázek 3**: Přejmenovat databázi připojené k lidské popisný název


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Krok 3: Přidání infrastruktury dotazování do databáze Northwind

Teď, když jsme připojenými `NORTHWND.MDF` databáze z `App_Data` složky, jsme re připraveno k přidání infrastruktury cyklického dotazování. Za předpokladu, že jste již přejmenován na DataTutorials databáze, spusťte následující čtyři příkazy:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Po spuštění těchto příkazů čtyři, klikněte pravým tlačítkem na název databáze v aplikaci Management Studio, přejděte na dílčí úlohy a zvolte Odpojit. Potom zavřete Management Studio a znovu otevřete Visual Studio.

Jakmile se znovu otevřete Visual Studio, přejdete na databázi pomocí Průzkumníka serveru. Všimněte si novou tabulku (`AspNet_SqlCacheTablesForChangeNotification`), nový uložené procedury a aktivačních událostí na `Products`, `Categories`, a `Suppliers` tabulky.


![Databáze nyní zahrnuje infrastruktury potřebné dotazování](using-sql-cache-dependencies-vb/_static/image4.gif)

**Obrázek 4**: databáze nyní zahrnuje infrastruktury potřebné dotazování


## <a name="step-4-configuring-the-polling-service"></a>Krok 4: Konfigurace služby dotazování

Po vytvoření potřebné tabulky, triggery a uložené procedury v databázi, posledním krokem je konfigurace cyklického dotazování služby, která se provádí prostřednictvím `Web.config` zadáním databáze použití a frekvenci dotazování v milisekundách. Následující kód dotazuje databázi Northwind jednou za sekundu.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

`name` Hodnotu `<add>` – element (NorthwindDB) přidruží popisný název konkrétní databázi. Při práci se službou SQL mezipaměti závislosti, budeme potřebovat k odkazování na název databáze, které jsou definované zde a také tabulky, která je na základě data uložená v mezipaměti. Ukážeme, jak používat `SqlCacheDependency` třída prostřednictvím kódu programu přidružit závislosti mezipaměti SQL s mezipaměti dat v kroku 6.

Po vytvoření mezipaměti závislost SQL systému dotazování se budou připojovat k definované v databáze `<databases>` elementy každých `pollTime` milisekundách a provést `AspNet_SqlCachePollingStoredProcedure` uložené procedury. Tuto uloženou proceduru - přidala zpátky v kroku 3 pomocí `aspnet_regsql.exe` nástroj pro příkazový řádek – vrátí `tableName` a `changeId` hodnoty pro každý záznam v `AspNet_SqlCacheTablesForChangeNotification`. Zastaralé závislosti mezipaměti SQL jsou odstraněna z mezipaměti.

`pollTime` Nastavení zavádí kompromis mezi výkonem a typu dat prošlostí. Malá `pollTime` hodnota zvyšuje počet požadavků na databázi, ale další rychle vyloučí zastaralá data z mezipaměti. Větší `pollTime` hodnoty sníží počet požadavků databáze, ale zvyšuje zpoždění mezi při změně dat back-end a když jsou vyřazování položky související mezipaměti. Naštěstí požadavek databáze provádí jednoduché uložené procedury této s vrácení pár řádků z tabulky jednoduchý, lightweight. Ale experimentovat s různými `pollTime` hodnoty najít ideální rovnováhu mezi databáze typu prošlostí přístup a dat pro vaši aplikaci. Bude se nejmenší `pollTime` povolená hodnota je 500.

> [!NOTE]
> Výše uvedeném příkladu obsahuje jeden `pollTime` hodnotu `<sqlCacheDependency>` elementu, ale můžete volitelně můžete zadat `pollTime` hodnotu `<add>` element. To je užitečné, pokud máte více databází zadán a chcete přizpůsobit frekvence cyklického dotazování na databázi.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Krok 5: Deklarativně práce závislosti mezipaměti SQL

V krocích 1 až 4 jsme se podívali na nastavení infrastruktury potřebné databáze a konfigurace systému cyklického dotazování. Pomocí této infrastruktury na místě jsme nyní můžete přidat položky do mezipaměti dat s přidružené závislosti mezipaměti SQL pomocí technik, programový nebo deklarativní. V tomto kroku prozkoumáme deklarativně práce s závislosti mezipaměti SQL. V kroku 6 podíváme programový přístup.

[Ukládání dat pomocí ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) kurzu prozkoumali deklarativních funkcí ukládání do mezipaměti ObjectDataSource. Jednoduše nastavením `EnableCaching` vlastnost `True` a `CacheDuration` vlastnost některé časový interval, ObjectDataSource bude automaticky ukládat do mezipaměti s daty vrácenými z jeho základní objekt po zadanou dobu. ObjectDataSource můžete také použít jeden nebo více závislostí mezipaměti SQL.

K předvedení deklarativně pomocí závislosti mezipaměti SQL, otevřete `SqlCacheDependencies.aspx` stránku `Caching` složku a přetáhněte GridView z panelu nástrojů na návrháře. Nastavit GridView s `ID` k `ProductsDeclarative` a z jeho inteligentních značek, zvolte pro vazbu k nové ObjectDataSource s názvem `ProductsDataSourceDeclarative`.


[![Vytvořit nový ObjectDataSource s názvem ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Obrázek 5**: vytvoření nové ObjectDataSource s názvem `ProductsDataSourceDeclarative` ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image4.png))


Konfigurace ObjectDataSource používat `ProductsBLL` třídy a nastavit rozevíracím seznamu vyberte kartu a `GetProducts()`. Na kartě aktualizace, zvolte `UpdateProduct` přetížení se tři vstupní parametry - `productName`, `unitPrice`, a `productID`. Nastavte rozevírací seznamy (None) na kartách INSERT a DELETE.


[![Použijte přetížení UpdateProduct s tři vstupní parametry](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Obrázek 6**: použití přetížení UpdateProduct s tři vstupní parametry ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image6.png))


[![Nastavit rozevíracího seznamu (None) pro příkaz INSERT a DELETE karty](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Obrázek 7**: nastavení rozevíracího seznamu (None) pro vložení a odstranit karty ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image8.png))


Po dokončení průvodce Konfigurace zdroje dat se vytvoří sada Visual Studio BoundFields a CheckBoxFields v GridView pro každé pole data. Odeberte všechna pole, ale `ProductName`, `CategoryName`, a `UnitPrice`a tyto pole formátu podle svých potřeb. Ze s GridView inteligentní značky kontrola zaškrtávací políčka Povolit stránkování, Povolit řazení a povolit úpravy. Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`. Aby funkci GridView s upravit tak, aby správně fungovat, buď odeberte tuto vlastnost zcela z deklarativní syntaxe nebo sadu zpátky na jeho výchozí hodnotu `{0}`.

Nakonec přidejte ovládací prvek popisek webového nad GridView a sadu jeho `ID` vlastnost `ODSEvents` a jeho `EnableViewState` vlastnost `False`. Po provedení těchto změn, vaše stránka s deklarativní by měl vypadat takto. Poznámka: Tento I jste provedli několik estetické přizpůsobení GridView pole, která nejsou nutné k předvedení funkce mezipaměti závislost SQL.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Dále vytvořte obslužnou rutinu události pro ObjectDataSource s `Selecting` událostí a v jeho přidejte následující kód:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Odvolat, ObjectDataSource s `Selecting` událost je jenom v případě, že načítání dat z jeho základní objekt. Pokud ObjectDataSource přistupuje k datům z vlastní mezipaměti, tato událost není aktivována.

Nyní najdete na této stránce prostřednictvím prohlížeče. Od jsme měli jste zatím k implementaci žádné ukládání do mezipaměti, pokaždé, když stránky, řazení nebo úpravám stránky mřížky zobrazit text, zvolíte událost je aktivována, jak ukazuje obrázek 8.


[![S ObjectDataSource zvolíte událost se aktivuje vždy, když je stránkovaného GridView, upravovat, nebo Sorted](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Obrázek 8**: The ObjectDataSource s `Selecting` aktivuje každý čas události GridView je stránkovaného fondu, upravovaný nebo Sorted ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image10.png))


Jak jsme viděli v [ukládání dat pomocí ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) kurzu nastavení `EnableCaching` vlastnost `True` způsobí, že ObjectDataSource pro ukládání do mezipaměti jeho data po dobu trvání určeného jeho `CacheDuration` vlastnost. ObjectDataSource má také [ `SqlCacheDependency` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), která přidá jeden nebo více závislosti mezipaměti SQL pro data uložená v mezipaměti pomocí vzoru:


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Kde *databaseName* je název databáze, jak je uvedeno v `name` atribut `<add>` element v `Web.config`, a *tableName* je název tabulky databáze. Například vytvoření ObjectDataSource, která po neomezenou dobu ukládá data do mezipaměti na základě závislost SQL mezipaměti proti Northwind s `Products` tabulky, nastavte ObjectDataSource s `EnableCaching` vlastnost `True` a jeho `SqlCacheDependency` vlastnosti NorthwindDB:Products.

> [!NOTE]
> Můžete použít závislost SQL mezipaměti *a* vypršení platnosti založené na čase nastavením `EnableCaching` k `True`, `CacheDuration` časový interval, a `SqlCacheDependency` o název databáze a tabulky. Když je dosaženo vypršení platnosti na základě času, nebo když poznámky k dotazování systému, že se změnila základní databázových dat, podle toho, co se stane nejprve ObjectDataSource vyřazení jeho data.


Rutina GridView v `SqlCacheDependencies.aspx` zobrazí data ze dvou tabulek - `Products` a `Categories` (produktu s `CategoryName` pole se načítají prostřednictvím `JOIN` na `Categories`). Proto jsme chtít zadat obě závislosti mezipaměti SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Konfigurace ObjectDataSource k podpoře ukládání do mezipaměti pomocí závislosti mezipaměti SQL na produkty a kategorie](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Obrázek 9**: Konfigurace ObjectDataSource pro podporu ukládání do mezipaměti pomocí SQL mezipaměti závislosti na `Products` a `Categories` ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image12.png))


Po dokončení konfigurace ObjectDataSource k podpoře ukládání do mezipaměti, otevírat stránku prostřednictvím prohlížeče. Událost zvolíte textu aktivováno znovu, by se měla objevit při první návštěvě stránky, ale měli zmizí při stránkování, řazení nebo kliknutím na tlačítko Upravit nebo zrušit. Důvodem je, že po načtení dat do mezipaměti s ObjectDataSource zůstane existuje až `Products` nebo `Categories` změně tabulek nebo data se aktualizují pomocí GridView.

Po stránkování mřížky a poznamenat nedostatek zvolíte událost je aktivována text, otevřete nové okno prohlížeče a přejděte na kurz základy v úpravy, vkládání a odstraňování část (`~/EditInsertDelete/Basics.aspx`). Aktualizujte název nebo cena produktu. Potom z do okna prohlížeče první zobrazení na jinou stránku dat, seřazení mřížky nebo klikněte na tlačítko Upravit řádek s. Tentokrát zvolíte událost je aktivována by měla objevit znova, jako základní databáze, data byla úspěšně upravená (viz obrázek 10). Pokud text se nezobrazí, chvíli počkejte a zkuste to znovu. Mějte na paměti, že služba dotazování kontroluje změny `Products` tabulky každých `pollTime` milisekundách, takže dochází ke zpoždění mezi při aktualizaci základní data a při vyřazování data uložená v mezipaměti.


[![Úprava tabulky produktů vyloučí Data uložená v mezipaměti produktu.](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Obrázek 10**: úprava tabulky produktů vyloučí produktu Data do mezipaměti ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Krok 6: Prostřednictvím kódu programu práce`SqlCacheDependency`– třída

[Ukládání dat v architektuře](caching-data-in-the-architecture-vb.md) kurzu zvážení výhody používání samostatné vrstvě ukládání do mezipaměti v architektuře oproti úzce spojovacích ukládání do mezipaměti s ObjectDataSource. V tomto kurzu jsme vytvořili `ProductsCL` třída k předvedení prostřednictvím kódu programu práci s mezipamětí data. Chcete-li využívat závislosti mezipaměti SQL ve vrstvě ukládání do mezipaměti, použijte `SqlCacheDependency` třídy.

Pomocí systému cyklického dotazování `SqlCacheDependency` objekt musí být přidružen pár konkrétní databázi a tabulku. Následující kód například vytvoří `SqlCacheDependency` objekt založena na databázi Northwind s `Products` tabulky:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Dva vstupní parametry, které `SqlCacheDependency` konstruktor s jsou názvy databáze a tabulky, v uvedeném pořadí. Upozorňujeme ObjectDataSource s `SqlCacheDependency` vlastnost, použít název databáze je stejná jako hodnota zadaná v `name` atribut `<add>` element v `Web.config`. Název tabulky je skutečný název tabulky databáze.

Pro přidružení `SqlCacheDependency` s položku přidaných do mezipaměti dat, použijte jednu z `Insert` přetížení metody, které přijímá závislost. Následující kód přidá *hodnotu* do mezipaměti data na neomezenou dobu, ale přidruží ji s `SqlCacheDependency` na `Products` tabulky. Stručně řečeno *hodnotu* zůstane v mezipaměti, dokud je vyřazena z důvodu omezení paměti, nebo protože dotazování systém zjistil, že `Products` tabulky se změnila, protože se ukládá do mezipaměti.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

Ukládání do mezipaměti vrstvy s `ProductsCL` třída aktuálně ukládá data do mezipaměti z `Products` tabulky pomocí založené na čase uplynutí 60 sekund. Umožní s aktualizace této třídy tak, aby používala závislosti mezipaměti SQL místo. `ProductsCL` Třídu s `AddCacheItem` metoda, která je zodpovědný za přidání dat do mezipaměti, teď obsahuje následující kód:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Aktualizace tohoto kódu pro použití `SqlCacheDependency` objektu místo `MasterCacheKeyArray` závislosti mezipaměti:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Pokud chcete vyzkoušet tuto funkci, přidejte na stránku pod existující GridView `ProductsDeclarative` GridView. Nastavit tento nový GridView s `ID` k `ProductsProgrammatic` a pomocí inteligentních značek, navázat jej na nové ObjectDataSource s názvem `ProductsDataSourceProgrammatic`. Konfigurace ObjectDataSource používat `ProductsCL` třída, nastavení rozevíracích seznamech vyberte ji a aktualizace karet `GetProducts` a `UpdateProduct`, v uvedeném pořadí.


[![Konfigurace ObjectDataSource použití třídy ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Obrázek 11**: Konfigurace ObjectDataSource pro použití `ProductsCL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image16.png))


[![Vyberte metodu GetProducts z rozevíracího seznamu vyberte kartu s](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Obrázek 12**: vyberte `GetProducts` metoda z vyberte kartu s rozevíracím seznamu ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image18.png))


[![Zvolte metodu UpdateProduct ze seznamu aktualizace karta s rozevíracím](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Obrázek 13**: z karty aktualizace s rozevíracím seznamu vyberte způsob UpdateProduct ([Kliknutím zobrazit obrázek v plné velikosti](using-sql-cache-dependencies-vb/_static/image20.png))


Po dokončení průvodce Konfigurace zdroje dat se vytvoří sada Visual Studio BoundFields a CheckBoxFields v GridView pro každé pole data. Jako s prvním GridView přidán k této stránce, odeberte všechna pole, ale `ProductName`, `CategoryName`, a `UnitPrice`a tyto pole formátu podle svých potřeb. Ze s GridView inteligentní značky kontrola zaškrtávací políčka Povolit stránkování, Povolit řazení a povolit úpravy. Stejně jako u `ProductsDataSourceDeclarative` nastaví ObjectDataSource, Visual Studio `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`. V pořadí pro funkci GridView s upravit tak, aby správně fungovat, nastavte tuto vlastnost zpět do `{0}` (nebo vlastnosti přiřazení úplně odebrat z deklarativní syntaxe).

Po dokončení těchto úloh se výsledná GridView a ObjectDataSource deklarativní by měl vypadat následovně:


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

K testování SQL závislost mezipaměti ve vrstvě ukládání do mezipaměti zarážku `ProductCL` třídu s `AddCacheItem` metoda a pak spustit ladění. Při první návštěvě `SqlCacheDependencies.aspx`, zarážce by měl přístupů, jako data jsou požadované pro první a umístí do mezipaměti. V dalším kroku přesunout na jinou stránku GridView nebo některý ze sloupců seřadit. To způsobí, že rutina GridView k requery svá data, ale data musí být nalezených v mezipaměti od `Products` databázové tabulky nebyl změněn. Pokud data je opakovaně nebyl nalezen v mezipaměti, ujistěte se, že v počítači je k dispozici dostatek paměti a zkuste to znovu.

Po stránkování prostřednictvím několika stránek GridView, otevřete druhé okno prohlížeče a přejděte na kurz základy v úpravy, vkládání a odstraňování část (`~/EditInsertDelete/Basics.aspx`). Aktualizace záznamu z tabulky produktů a pak z okna prohlížeče první zobrazit nová stránka nebo klikněte na jednu z hlaviček řazení.

V tomto scénáři se zobrazí jednu ze dvou akcí: buď zarážce se setkají, která určuje, že data uložená v mezipaměti byla vyřazena z důvodu změn v databázi. nebo, zarážce nedosáhnete stropu, znamená to, že `SqlCacheDependencies.aspx` se teď zobrazuje zastaralá data. Pokud není průchodu zarážkou, je pravděpodobné, protože cyklického dotazování služby nebyl ještě aktivována vzhledem k tomu, že data změnila. Mějte na paměti, že služba dotazování kontroluje změny `Products` tabulky každých `pollTime` milisekundách, takže dochází ke zpoždění mezi při aktualizaci základní data a při vyřazování data uložená v mezipaměti.

> [!NOTE]
> Toto opoždění je pravděpodobnější, než se objeví při úpravě jeden z produktů prostřednictvím GridView v `SqlCacheDependencies.aspx`. V [ukládání dat v architektuře](caching-data-in-the-architecture-vb.md) kurzu jsme přidali `MasterCacheKeyArray` závislosti zajistit, aby data upravovaný prostřednictvím mezipaměti `ProductsCL` třídu s `UpdateProduct` metoda byla odstraněna z mezipaměti. Však jsme nahradit tuto závislost mezipaměti při úpravě `AddCacheItem` metoda dříve v tomto kroku a proto `ProductsCL` třída bude nadále zobrazit data uložená v mezipaměti, dokud systém dotazování zaznamená změnu `Products` tabulky. Ukážeme, jak znovu zavést `MasterCacheKeyArray` mezipaměti závislostí v kroku 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Krok 7: Přidružit více závislostí položky v mezipaměti

Odvolat, který `MasterCacheKeyArray` závislost mezipaměti se používá k zajištění toho, aby *všechny* data týkající se produktu je vyřazena z mezipaměti při aktualizaci jakoukoli položku přidružené v něm. Například `GetProductsByCategoryID(categoryID)` metoda mezipamětí `ProductsDataTables` instance pro každý jedinečný *categoryID* hodnotu. Pokud některý z těchto objektů vyřazování, `MasterCacheKeyArray` závislost mezipaměti zajistí, že jiné jsou také odebrány. Bez této závislosti mezipaměti při změně data uložená v mezipaměti existuje možnost, že další data uložená v mezipaměti produktu může být zastaralý. V důsledku toho je důležité, aby uchováváme s `MasterCacheKeyArray` závislosti mezipaměti při použití SQL mezipaměti závislosti. Však data do mezipaměti s `Insert` metoda umožňuje pouze pro objekt jeden závislostí.

Kromě toho při práci se službou SQL závislosti mezipaměti může musíme přidružit více tabulek databáze jako závislosti. Například `ProductsDataTable` do mezipaměti `ProductsCL` třída obsahuje názvy kategorií a dodavatele pro každý produkt, ale `AddCacheItem` metoda používá jenom závislost na `Products`. V této situaci pokud uživatel aktualizuje název kategorie nebo dodavatele, data uložená v mezipaměti produktu, zůstanou v mezipaměti a zastaralá. Proto jsme mají být data uložená v mezipaměti produktu závisí na nejen `Products` tabulky, ale na `Categories` a `Suppliers` také tabulky.

[ `AggregateCacheDependency` Třída](https://msdn.microsoft.com/en-us/library/system.web.caching.aggregatecachedependency.aspx) představuje způsob, jak přidružit více závislostí položky mezipaměti. Začněte vytvořením `AggregateCacheDependency` instance. Dál přidejte sadu závislosti s využitím `AggregateCacheDependency` s `Add` metoda. Při vkládání položka do mezipaměti dat po tomto datu, předejte `AggregateCacheDependency` instance. Když *žádné* z `AggregateCacheDependency` změnit instanci s závislosti, položky v mezipaměti bude vyloučena.

Následující zobrazí aktualizovaný kód `ProductsCL` třídu s `AddCacheItem` metoda. Metoda vytvoří `MasterCacheKeyArray` závislosti spolu s mezipaměti `SqlCacheDependency` objekty pro `Products`, `Categories`, a `Suppliers` tabulky. Tyto jsou všechny zkombinované do jednoho `AggregateCacheDependency` objekt s názvem `aggregateDependencies`, který byl následně předán do `Insert` metoda.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Test se tento nový kód. Nyní se změní `Products`, `Categories`, nebo `Suppliers` tabulky způsobit, že data uložená v mezipaměti k vyloučení. Kromě toho `ProductsCL` třídu s `UpdateProduct` metodu, která je volána při úpravě produktu prostřednictvím GridView, vyloučí `MasterCacheKeyArray` závislosti, což způsobí, že v mezipaměti mezipaměti `ProductsDataTable` k vyloučení a data mají být načteny znovu při příštím požadavek.

> [!NOTE]
> Závislosti mezipaměti SQL můžete použít taky s [ukládání výstupu do mezipaměti](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Ukázka tuto funkci, najdete v tématu: [pomocí ASP.NET ukládání výstupu do mezipaměti se systémem SQL Server](https://msdn.microsoft.com/en-us/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Souhrn

Při ukládání do mezipaměti data databáze, data se v ideálním případě zůstanou v mezipaměti, dokud se mění v databázi. S prostředím ASP.NET 2.0 můžete vytvořit a použít ve scénářích deklarativní a programové závislosti mezipaměti SQL. Jedním z problémů s tímto přístupem je v zjišťování, když se data změnila. Plné verze systému Microsoft SQL Server 2005 nabízí možnosti oznámení, které aplikace může upozornit, když došlo ke změně výsledku dotazu. U Express edice systému SQL Server 2005 a starší verze systému SQL Server musí být použito dotazování systému místo. Nastavení infrastruktury potřebné cyklického dotazování je naštěstí přímočará.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Používání oznámení dotazu v systému Microsoft SQL Server 2005](https://msdn.microsoft.com/en-us/library/ms175110.aspx)
- [Vytváření oznámení o dotazu](https://msdn.microsoft.com/en-us/library/ms188669.aspx)
- [Ukládání do mezipaměti technologie ASP.NET s `SqlCacheDependency` – třída](https://msdn.microsoft.com/en-us/library/ms178604(VS.80).aspx)
- [Nástroj pro registraci serveru SQL technologie ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx)
- [Přehled`SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Marko Rangel Teresy Murphy a Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](caching-data-at-application-startup-vb.md)
