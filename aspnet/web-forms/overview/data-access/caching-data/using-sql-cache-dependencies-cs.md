---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: Použití závislostí mezipaměti SQL (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Nejjednodušší strategií ukládání do mezipaměti je umožnit data uložená v mezipaměti vyprší po zadaném časovém období. Ale tento jednoduchý přístup znamená, že data uložená v mezipaměti maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: ddd0ce9e8e0f69da6f9c0f65165e4842d460f0c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756886"
---
<a name="using-sql-cache-dependencies-c"></a>Použití závislostí mezipaměti SQL (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) nebo [stahovat PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> Nejjednodušší strategií ukládání do mezipaměti je umožnit data uložená v mezipaměti vyprší po zadaném časovém období. Ale tento jednoduchý přístup znamená, že data uložená v mezipaměti udržuje žádné jejich spojení se jeho podkladového zdroje dat, výsledkem je zastaralá data, která se nachází příliš dlouhý nebo aktuální data, která je příliš brzy vypršela platnost. Lepším řešením je použití třídy SqlCacheDependency tak, aby data zůstanou v mezipaměti, dokud se změnila jeho podkladová data ve službě SQL database. V tomto kurzu se dozvíte, jak.


## <a name="introduction"></a>Úvod

Ukládání do mezipaměti techniky kontrolován [ukládání dat do mezipaměti ovládacím prvkem ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) a [ukládání dat do mezipaměti v architektuře](caching-data-in-the-architecture-cs.md) kurzy použít časovou synchronizací vypršení platnosti vyřazení dat z mezipaměti po zadanou období. Tento přístup je nejjednodušší způsob, jak vyvážit zvýšení výkonu ukládání do mezipaměti proti neaktuálnost data. Výběrem doba uplynutí *x* sekund, vývojář stránky concedes mohli využívat výhody výkonu ukládání do mezipaměti pouze *x* sekund, ale můžete snadno rest, že její data nikdy budou zastaralá déle, než maximální z *x* sekund. Samozřejmě pro statická data *x* je možné rozšířit na životního cyklu webové aplikace, jako je zkontrolován v [ukládání dat do mezipaměti při spuštění aplikace](caching-data-at-application-startup-cs.md) kurzu.

Při ukládání do mezipaměti dat z databáze, podle času vypršení platnosti je často vybrán pro jeho snadnější použití ale často je nedostatečné řešení. V ideálním případě databázových dat by zůstat v mezipaměti, dokud se změnila podkladová data v databázi. teprve pak bude vyloučeno mezipaměti. Tento přístup se maximalizuje výkon výhody ukládání do mezipaměti a minimalizuje dobu trvání zastaralá data. Aby mohli využívat tyto výhody existuje musí však být některé systému, který zná při podkladová data databáze byl změněn a vyloučí odpovídající položky z mezipaměti. Před ASP.NET 2.0 tým byl zodpovědný za implementace tohoto systému vývojářům stránky.

Poskytuje technologie ASP.NET 2.0 [ `SqlCacheDependency` třídy](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) a potřebnou infrastrukturu k určení, kdy došlo ke změně v databázi tak, aby odpovídající položky v mezipaměti může být vyřazena. Existují dvě techniky pro stanovení změny podkladových dat: oznámení a dotazování. Po diskuze o rozdílech mezi oznámení a dotazování, vytvoříme infrastruktury potřebné pro podporu cyklického dotazování a pak prozkoumejte, jak používat `SqlCacheDependency` třída v deklarativní a programová scénáře.

## <a name="understanding-notification-and-polling"></a>Principy upozornění a dotazování

Existují dvě techniky, které lze použít k určení, kdy byla změněna data v databázi: oznámení a dotazování. S oznámením databáze automaticky upozorní na modul runtime ASP.NET výsledky speciální dotaz byl změněn od naposledy spustil dotaz, v tomto okamžiku se vyřadí položek v mezipaměti přidruženou k dotazu. Pomocí cyklického dotazování, serveru databáze uchovává informace o když mají určité tabulky poslední aktualizace. Modul runtime ASP.NET pravidelně se dotazuje databáze, kterou chcete zkontrolovat, co jste změnili tabulky vzhledem k tomu, že jste zadali do mezipaměti. Tyto tabulky, jejichž data se změnila mají jejich položky mezipaměti přidružené k vyloučení.

Možnost oznámení vyžaduje méně nastavení než dotazování a je podrobnější, protože sleduje změny na úrovni dotazů, spíše než na úrovni tabulky. Bohužel oznámení jsou k dispozici pouze v úplné edice Microsoft SQL Server 2005 (tedy ne Express edice). Ale možnost dotazování použít pro všechny verze systému Microsoft SQL serveru z 7.0 pro 2005. Protože těchto kurzů použijte edice Express systému SQL Server 2005, se zaměříme na vytváření a použití možnosti dotazování. Na konci tohoto kurzu pro další prostředky na možnostech oznámení systému SQL Server 2005 s najdete v části Další materiály.

Pomocí cyklického dotazování, je třeba nastavit databázi zahrnout tabulku s názvem `AspNet_SqlCacheTablesForChangeNotification` , který má tři sloupce – `tableName`, `notificationCreated`, a `changeId`. Tato tabulka obsahuje řádek pro každou tabulku, která obsahuje data, která může být nutné použít v závislosti mezipaměti SQL ve webové aplikaci. `tableName` Sloupci Určuje název tabulky při `notificationCreated` Určuje datum a čas přidání řádku do tabulky. `changeId` Sloupec je typu `int` a má počáteční hodnotu 0. Jeho hodnota se zvyšuje s každou změnu do tabulky.

Kromě `AspNet_SqlCacheTablesForChangeNotification` tabulky databáze také musí obsahovat aktivační události na každé z tabulek, které se mohou objevit v závislosti mezipaměti SQL. Tyto triggery jsou provedeny vždy, když je řádek vložit, aktualizovat ani odstranit a zvýšit v tabulce s `changeId` hodnota v `AspNet_SqlCacheTablesForChangeNotification`.

Modul runtime ASP.NET sleduje aktuální `changeId` pro tabulku při ukládání do mezipaměti pomocí dat `SqlCacheDependency` objektu. Databáze se pravidelně kontroluje a jakékoli `SqlCacheDependency` objekty, jejichž `changeId` liší od hodnot v databázi se vyřadí od lišící se `changeId` hodnota značí, že došlo ke změně v tabulce od data ukládá do mezipaměti.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Krok 1: Zkoumání`aspnet_regsql.exe`Program příkazového řádku

S přístupem dotazování databáze musí být nastavena tak, aby obsahovala infrastruktury popsané výše: předdefinované tabulce (`AspNet_SqlCacheTablesForChangeNotification`), několik uložených procedur a aktivačních událostí na všech tabulek, které lze použít v závislosti mezipaměti SQL na webu aplikace. Tyto tabulky, uložených procedur a aktivačních událostí je možné vytvářet přes příkazový řádek programu `aspnet_regsql.exe`, která byla nalezena v `$WINDOWS$\Microsoft.NET\Framework\version` složky. Chcete-li vytvořit `AspNet_SqlCacheTablesForChangeNotification` tabulky a přidružené uložených procedur, spusťte následující příkaz z příkazového řádku:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Aby se tyto příkazy přihlášení k zadané databázi musí být v [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) a [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) role. Prozkoumat odeslán do databáze pomocí jazyka T-SQL `aspnet_regsql.exe` příkazového řádku programu, přečtěte si [tomto blogu](http://scottonwriting.net/sowblog/posts/10709.aspx).


Například chcete-li přidat infrastrukturu pro dotazování databáze Microsoft SQL Server s názvem `pubs` na databázovém serveru s názvem `ScottsServer` používáte ověřování Windows, přejděte do příslušného adresáře a z příkazového řádku, zadejte:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Po přidání infrastruktury na úrovni databáze, potřebujeme přidat aktivační události na těchto tabulkách, které se použijí v závislosti mezipaměti SQL. Použít `aspnet_regsql.exe` příkazového řádku program znovu, ale zadat pomocí názvu tabulky `-t` přepnout a místo `-ed` přepnout použití `-et`, takto:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Chcete-li přidat aktivační události pro `authors` a `titles` tabulky na `pubs` databáze na `ScottsServer`, použijte:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

Pro účely tohoto kurzu přidat aktivační události na `Products`, `Categories`, a `Suppliers` tabulky. Podíváme se na konkrétní příkazový řádek syntaxe v kroku 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Krok 2: Odkazování na databáze Microsoft SQL Server 2005 Express Edition v`App_Data`

`aspnet_regsql.exe` Příkazového řádku program vyžaduje název databáze a serveru chcete-li přidat infrastruktury potřebné cyklického dotazování. Jaký je název databáze a serveru pro databázi Microsoft SQL Server 2005 Express, který se nachází v, ale `App_Data` složky? Místo zjistit, jaké jsou názvy databáze a serveru, můžu ve zjištěno, že je nejjednodušším přístupem připojit databázi k `localhost\SQLExpress` databáze instance a data s využitím přejmenujte [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Pokud nemáte plné verze SQL Server 2005 na vašem počítači nainstalovaný, pravděpodobně již máte v počítači nainstalovaný SQL Server Management Studio. Pokud máte jenom edice Express, si můžete stáhnout bezplatnou [aktualizace Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Začněte tím, že zavření sady Visual Studio. Dále otevřete SQL Server Management Studio a zvolte možnost pro připojení k `localhost\SQLExpress` serveru pomocí ověřování Windows.


![Připojení k serveru localhost\SQLExpress](using-sql-cache-dependencies-cs/_static/image1.gif)

**Obrázek 1**: připojení k `localhost\SQLExpress` serveru


Po připojení k serveru, bude zobrazit server Management Studio a obsahovat podsložky pro databáze, zabezpečení a tak dále. Klikněte pravým tlačítkem na složku databází a zvolte možnost připojení. Tím se otevře dialogové okno Připojit databáze (viz obrázek 2). Klikněte na tlačítko Přidat a vyberte `NORTHWND.MDF` složka databáze ve vaší webové aplikace s `App_Data` složky.


[![Připojte NORTHWND. MDF databáze ze složky App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Obrázek 2**: připojit `NORTHWND.MDF` databáze z `App_Data` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image2.png))


Tím přidáte databáze ke složce databáze. Název databáze může být úplná cesta k souboru databáze nebo úplnou cestu zvolený se přidá [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Aby se nemusela zadejte tento název zdlouhavé databáze při použití aspnet\_regsql.exe nástroj příkazového řádku, přejmenovat databázi pro více lidských – popisný název kliknutím pravým tlačítkem na databázi jenom připojit a zvolíte přejmenovat. Můžu odebrat přejmenován na DataTutorials Moje databáze.


![Přejmenujte připojené databázi na lidské popisný název](using-sql-cache-dependencies-cs/_static/image3.gif)

**Obrázek 3**: přejmenování připojené databáze více lidských – popisný název


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Krok 3: Přidání dotazování infrastrukturu k databázi Northwind

Teď, když nám připojili `NORTHWND.MDF` databáze z `App_Data` složky, můžeme znovu připraven k přidání infrastruktury cyklického dotazování. Za předpokladu, že jste již přejmenovali databáze do DataTutorials, spusťte následující čtyři příkazy:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Po spuštění těchto čtyř příkazů, klikněte pravým tlačítkem na název databáze v aplikaci Management Studio, přejděte na podnabídky úkoly a zvolte Odpojit. Potom zavřete Management Studio a znovu otevřete Visual Studio.

Jakmile sada Visual Studio znovu otevřel, přejít k podrobnostem databáze prostřednictvím Průzkumníka serveru. Poznámka: Nová tabulka (`AspNet_SqlCacheTablesForChangeNotification`), nový uložených procedur a aktivačních událostí na `Products`, `Categories`, a `Suppliers` tabulky.


![Databáze nyní obsahuje nezbytné dotazování infrastruktury](using-sql-cache-dependencies-cs/_static/image4.gif)

**Obrázek 4**: databáze nyní obsahuje nezbytné dotazování infrastruktury


## <a name="step-4-configuring-the-polling-service"></a>Krok 4: Konfigurace služby cyklického dotazování

Po vytvoření potřebné tabulky, triggery a uložené procedury v databázi, je posledním krokem konfigurace cyklického dotazování služby, která se provádí prostřednictvím `Web.config` zadáním databázi, kterou chcete použít a frekvence cyklického dotazování v milisekundách. Následující kód se dotazuje databáze Northwind jednou za sekundu.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

`name` Hodnotu `<add>` – element (NorthwindDB) přidruží popisný název s danou databází. Při práci s závislosti mezipaměti SQL, potřebujeme k odkazování na název databáze lze tam definovat také v tabulce, které je na základě dat uložených v mezipaměti. Uvidíme, jak používat `SqlCacheDependency` třídy programově přidružení závislosti mezipaměti SQL s uložené v mezipaměti dat v kroku 6.

Po vytvoření závislosti mezipaměti SQL systému dotazování se připojit k databázím definované v `<databases>` prvky každý `pollTime` milisekund a spustit `AspNet_SqlCachePollingStoredProcedure` uložené procedury. Tato uložená procedura – byla přidána zpět v kroku 3 pomocí `aspnet_regsql.exe` nástroj příkazového řádku – vrátí `tableName` a `changeId` hodnoty pro každý záznam v `AspNet_SqlCacheTablesForChangeNotification`. Zastaralé závislosti mezipaměti SQL se vyřadí z mezipaměti.

`pollTime` Kompromis mezi výkonem a odolnost dat představuje nastavení. Malá `pollTime` hodnota se zvyšuje počet požadavků na databázi, ale více rychle vyloučí zastaralých dat z mezipaměti. Větší `pollTime` hodnotu snižuje počet požadavků na databázi, ale zvyšuje zpoždění mezi při změně dat back-endu a při vyřazení mezipaměti souvisejících položek. Naštěstí požadavek databáze provádí jednoduchou uloženou proceduru této s vrací jenom pár řádků z tabulky jednoduché, zjednodušené. Ale experimentovat s různými `pollTime` hodnoty najít ideální rovnováhu mezi databáze neaktuálnost přístup a data pro vaši aplikaci. Bude se nejmenší `pollTime` povolená hodnota je 500.

> [!NOTE]
> Výše uvedený příklad poskytuje jedinou `pollTime` hodnota v `<sqlCacheDependency>` element, ale můžete volitelně zadat `pollTime` hodnota v `<add>` elementu. To je užitečné, pokud máte více databází a chcete přizpůsobit frekvence cyklického dotazování na databázi.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Krok 5: Deklarativně práce s závislosti mezipaměti SQL

V krocích 1 až 4 jsme se podívali na nastavení infrastruktury potřebné databáze a konfigurace systému cyklického dotazování. Pomocí této infrastruktury na místě jsme teď můžete přidávat položky do datové mezipaměti přidružené závislosti mezipaměti SQL pomocí prostřednictvím kódu programu nebo deklarativní technik. V tomto kroku prozkoumáme deklarativně práce s závislosti mezipaměti SQL. V kroku 6 podíváme na programový přístup.

[Ukládání dat do mezipaměti ovládacím prvkem ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) kurzu prozkoumali deklarativních funkcí ObjectDataSource ukládání do mezipaměti. Jednoduše nastavením `EnableCaching` vlastnost `true` a `CacheDuration` vlastnost některé časový interval, prvku ObjectDataSource automaticky ukládá data vrácená z jeho základní objekt pro zadaný interval. Prvku ObjectDataSource můžete také použít jeden nebo více závislostí mezipaměti SQL.

Abychom si předvedli deklarativně použití závislostí mezipaměti SQL, otevřete `SqlCacheDependencies.aspx` stránku `Caching` složky a GridView přetáhněte z panelu nástrojů do návrháře. Nastavit prvek GridView s `ID` k `ProductsDeclarative` a z inteligentních značek, vyberte a vytvořte jeho vazbu nového prvku ObjectDataSource s názvem `ProductsDataSourceDeclarative`.


[![Vytvoření nového prvku ObjectDataSource s názvem ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Obrázek 5**: vytvoření nového prvku ObjectDataSource s názvem `ProductsDataSourceDeclarative` ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image4.png))


Konfigurace ObjectDataSource používat `ProductsBLL` třídy a nastavit rozevíracího seznamu vyberte kartě `GetProducts()`. Na kartě aktualizace, zvolte `UpdateProduct` přetížení se třemi vstupní parametry - `productName`, `unitPrice`, a `productID`. Nastavte rozevírací seznamy na (žádný) na kartách INSERT a DELETE.


[![Použijte přetížení UpdateProduct se třemi vstupní parametry](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Obrázek 6**: se třemi parametry vstup pomocí přetížení UpdateProduct ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image6.png))


[![Nastavení rozevíracího seznamu na (žádný) pro vložení a odstranění karty](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Obrázek 7**: Nastavte rozevírací seznam na (žádný) pro vložení a odstranění karty ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image8.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio vytvoří BoundFields a CheckBoxFields v prvku GridView. pro každé datové pole. Odebrat všechna pole, ale `ProductName`, `CategoryName`, a `UnitPrice`a tato pole formátu podle svých potřeb. Z inteligentních značek GridView s zaškrtněte zaškrtávací políčka Povolit stránkování, Povolit řazení a povolit úpravy. Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`. Aby funkce úprav GridView s fungovalo správně, buď odeberte tuto vlastnost z deklarativní syntaxe nebo nastavte ji zpět na výchozí hodnotu, zcela `{0}`.

Nakonec přidejte popisek webový ovládací prvek výše GridView a nastavte jeho `ID` vlastnost `ODSEvents` a jeho `EnableViewState` vlastnost `false`. Po provedení těchto změn kódu s deklarativní stránky s by měl vypadat nějak takto. Všimněte si, které jsem ve provedli několik aesthetic přizpůsobení GridView pole, které nejsou potřebné, abychom si předvedli funkci závislosti mezipaměti SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Dále vytvořte obslužnou rutinu události pro prvek ObjectDataSource s `Selecting` událostí a v přidejte následující kód:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Vzpomeňte si, že ObjectDataSource s `Selecting` události dochází pouze v případě, že načítání dat z jeho základního objektu. Pokud prvku ObjectDataSource přistupuje k datům z vlastní mezipaměti, není tato událost aktivuje.

Nyní navštivte tuto stránku prostřednictvím prohlížeče. Protože jsme ve ještě provádět žádné ukládání do mezipaměti, pokaždé, když stránku, řazení nebo upravit stránku mřížky by se zobrazit textu, výběr události vyvolané, jak ukazuje obrázek 8.


[![Prvek ObjectDataSource s události Selecting aktivuje vždy, když je stránkování prvku GridView, upravit, nebo seřazeno](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Obrázek 8**: prvku ObjectDataSource s `Selecting` událost je aktivována každý čas stránkování prvku GridView, upravovaný nebo seřazeno ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image10.png))


Jak jsme viděli v [ukládání dat do mezipaměti ovládacím prvkem ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) kurz nastavení `EnableCaching` vlastnost `true` způsobí, že ObjectDataSource pro ukládání do mezipaměti svá data po dobu určeného jeho `CacheDuration` vlastnost. Prvku ObjectDataSource má také [ `SqlCacheDependency` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), přidává jeden nebo více závislostí mezipaměti SQL pro data uložená v mezipaměti pomocí vzoru:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Kde *databaseName* je název databáze, jak je uvedeno v `name` atribut `<add>` prvek `Web.config`, a *tableName* je název databázové tabulky. Například k vytvoření ObjectDataSource, který po neomezenou dobu ukládá data do mezipaměti na základě závislosti mezipaměti SQL proti Northwind s `Products` tabulky, nastavte ObjectDataSource s `EnableCaching` vlastnost `true` a jeho `SqlCacheDependency` vlastnost NorthwindDB:Products.

> [!NOTE]
> Můžete použít závislosti mezipaměti SQL *a* vypršení platnosti podle času nastavením `EnableCaching` k `true`, `CacheDuration` časový interval a `SqlCacheDependency` na názvy databáze a tabulky. Prvku ObjectDataSource vyřazení svá data po dosažení konce platnosti podle času, nebo když systém dotazování poznámky, že podkladová data databáze změnila, podle toho, co nastane dřív.


V prvku GridView `SqlCacheDependencies.aspx` zobrazí data ze dvou tabulek - `Products` a `Categories` (produkt s `CategoryName` prostřednictvím se načítají pole `JOIN` na `Categories`). Proto budeme chtít zadat dvě závislosti mezipaměti SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Konfigurace ObjectDataSource pro podporu ukládání do mezipaměti použití závislostí mezipaměti SQL na produkty a kategorie](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Obrázek 9**: Konfigurace ObjectDataSource pro podporu ukládání do mezipaměti pomocí závislosti mezipaměti SQL na `Products` a `Categories` ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image12.png))


Po dokončení konfigurace ObjectDataSource pro podporu ukládání do mezipaměti, otevírat stránku prostřednictvím prohlížeče. Znovu aktivuje události Selecting text by se měla objevit při první návštěvě stránky, ale by měla zmizet po stránkování, řazení nebo kliknutím na tlačítko Upravit nebo zrušit. Důvodem je, že po načtení dat do mezipaměti s ObjectDataSource existuje zůstává až `Products` nebo `Categories` dojde k úpravě tabulky nebo data se aktualizují pomocí prvku GridView.

Po vyvolání stránkování mřížky a poznamenat chybějící události Selecting text, otevřete nové okno prohlížeče a přejděte na kurz základy v úpravy, vložení a odstranění oddílu (`~/EditInsertDelete/Basics.aspx`). Aktualizujte název nebo cena produktu. Pak z první okna prohlížeče, zobrazte na jinou stránku dat, seřazení mřížky nebo klikněte na tlačítko Upravit řádek s. Tentokrát, aktivuje události Selecting by měl znovu, jako jsou databáze, které byla data změny (viz obrázek 10). Pokud text se nezobrazí, chvíli počkejte a zkuste to znovu. Mějte na paměti, že dotazování služby kontroluje změny `Products` tabulky každý `pollTime` milisekund, takže dochází ke zpoždění mezi při aktualizaci podkladových dat a pokud dojde k jejich vyřazení dat uložených v mezipaměti.


[![Změna tabulky produktů vyloučí Data produktu uložená v mezipaměti](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Obrázek 10**: úprava tabulky produktů vyloučí produktu Data v mezipaměti ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Krok 6: Programově pracovat`SqlCacheDependency`třídy

[Ukládání dat do mezipaměti v architektuře](caching-data-in-the-architecture-cs.md) kurzu podívali se na výhody používání samostatné vrstvě ukládání do mezipaměti v architektuře, která na rozdíl od pevné párování ukládání do mezipaměti ovládacím prvkem ObjectDataSource. V tomto kurzu jsme vytvořili `ProductsCL` třídy k předvedení programově práci s mezipamětí data. Chcete-li využívají závislosti mezipaměti SQL ve vrstvě ukládání do mezipaměti, použijte `SqlCacheDependency` třídy.

Systém cyklického dotazování `SqlCacheDependency` objekt musí být spojeny s konkrétní dvojici databázi a tabulku. Následující kód například vytvoří `SqlCacheDependency` objektu podle databázi Northwind s `Products` tabulky:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Dva vstupní parametry pro `SqlCacheDependency` konstruktor s jsou názvy databáze a tabulky v uvedeném pořadí. Třeba v prvku ObjectDataSource s `SqlCacheDependency` vlastnost, použít název databáze je stejná jako hodnota určená v `name` atribut `<add>` prvek `Web.config`. Název tabulky je skutečný název databázové tabulky.

Přidružení `SqlCacheDependency` s položkou přidány do datové mezipaměti, použijte jednu z `Insert` přetížení metod, které přijímá závislost. Následující kód přidává *hodnotu* do mezipaměti dat na neomezenou dobu, ale přidruží ji k `SqlCacheDependency` na `Products` tabulky. Stručně řečeno *hodnotu* zůstane v mezipaměti, dokud je vyřazena z důvodu omezení paměti, nebo protože dotazování systém zjistil, že `Products` tabulka změnila od uložení do mezipaměti.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

Ukládání do mezipaměti vrstvu s `ProductsCL` třídy aktuálně ukládá data do mezipaměti z `Products` tabulky pomocí podle času uplynutí 60 sekund. Umožní s aktualizace této třídy tak, aby místo toho používal závislosti mezipaměti SQL. `ProductsCL` Třída s `AddCacheItem` metodu, která je zodpovědný za přidání dat do mezipaměti, aktuálně obsahuje následující kód:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Aktualizovat tento kód pro použití `SqlCacheDependency` místo objektu `MasterCacheKeyArray` závislost mezipaměti:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Pokud chcete vyzkoušet tuto funkci, přidejte na stránku pod existující GridView `ProductsDeclarative` ovládacího prvku GridView. Nastavit tento nový prvek GridView s `ID` k `ProductsProgrammatic` a prostřednictvím inteligentních značek, jeho vazbu na nového prvku ObjectDataSource s názvem `ProductsDataSourceProgrammatic`. Konfigurace ObjectDataSource používat `ProductsCL` třídy nastavením rozevíracích seznamech vyberte a aktualizace karet `GetProducts` a `UpdateProduct`v uvedeném pořadí.


[![Konfigurace ObjectDataSource pomocí třídy ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Obrázek 11**: Konfigurace ObjectDataSource k použití `ProductsCL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image16.png))


[![Vyberte z rozevíracího seznamu vyberte kartu s GetProducts – metoda](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Obrázek 12**: vyberte `GetProducts` metodu z rozevíracího seznamu vyberte kartu s ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image18.png))


[![Zvolte z rozevíracího seznamu aktualizace kartu s UpdateProduct – metoda](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Obrázek 13**: z kartu aktualizace s rozevíracím seznamu zvolte metodu UpdateProduct ([kliknutím ji zobrazíte obrázek v plné velikosti](using-sql-cache-dependencies-cs/_static/image20.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio vytvoří BoundFields a CheckBoxFields v prvku GridView. pro každé datové pole. Jako s prvním prvku GridView přidaných na tuto stránku, odeberte všechna pole, ale `ProductName`, `CategoryName`, a `UnitPrice`a tato pole formátu podle svých potřeb. Z inteligentních značek GridView s zaškrtněte zaškrtávací políčka Povolit stránkování, Povolit řazení a povolit úpravy. Stejně jako u `ProductsDataSourceDeclarative` nastaví prvek ObjectDataSource, Visual Studio `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`. Aby funkce úprav GridView s správně fungovat, nastavte tuto vlastnost zpět do `{0}` (nebo zcela odebrat přiřazení vlastnosti z deklarativní syntaxe).

Po dokončení těchto úloh, výsledný ovládacími prvky GridView a ObjectDataSource deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

K otestování SQL závislosti mezipaměti ve vrstvě ukládání do mezipaměti, nastavte zarážku v `ProductCL` třída s `AddCacheItem` metodu a poté spuštění ladění. Při první návštěvě `SqlCacheDependencies.aspx`, by měla být zarážka dosažena jako data požadovaná pro první a umístí do mezipaměti. V dalším kroku přesunout na jinou stránku v prvku GridView nebo jeden z těchto sloupců seřadit. To způsobí, že GridView k requery svá data, ale data by měla být nalezených v mezipaměti od `Products` databázové tabulky nebyl změněn. Pokud data je opakovaně nebyl nalezen v mezipaměti, ujistěte se, že ve vašem počítači není k dispozici dostatek paměti a zkuste to znovu.

Po stránkování prostřednictvím několika stránky prvku GridView, otevřete druhou okno prohlížeče a přejděte na kurz základy v úpravy, vložení a odstranění oddílu (`~/EditInsertDelete/Basics.aspx`). Aktualizovat záznam z tabulky produktů a pak z první okna prohlížeče, zobrazte novou stránku nebo na některou z hlaviček řazení.

V tomto scénáři se zobrazí jednu ze dvou akcí: buď bude dosaženo zarážkou, která udává, že data uložená v mezipaměti byl vyřazen z důvodu změny v databázi. nebo, zarážka nebude dosažena, to znamená, že `SqlCacheDependencies.aspx` se teď zobrazuje zastaralá data. Pokud není zarážka dosažena, je pravděpodobné, protože cyklického dotazování služby nebyla aktivována ještě od změny data. Mějte na paměti, že dotazování služby kontroluje změny `Products` tabulky každý `pollTime` milisekund, takže dochází ke zpoždění mezi při aktualizaci podkladových dat a pokud dojde k jejich vyřazení dat uložených v mezipaměti.

> [!NOTE]
> Toto zpoždění je pravděpodobnější, že se zobrazí při úpravách produktů prostřednictvím GridView v `SqlCacheDependencies.aspx`. V [ukládání dat do mezipaměti v architektuře](caching-data-in-the-architecture-cs.md) kurzu jsme přidali `MasterCacheKeyArray` závislosti, chcete-li zajistit dat, který právě upravujete prostřednictvím mezipaměti `ProductsCL` třída s `UpdateProduct` metoda byl vyřazen z mezipaměti. Ale jsme tuto závislost mezipaměti nahradilo při úpravě `AddCacheItem` metoda dříve v tomto kroku a proto `ProductsCL` třídy nadále zobrazovat data uložená v mezipaměti, dokud se změna – zpráva k dotazování systému `Products` tabulky. Uvidíme, jak zavést `MasterCacheKeyArray` mezipaměti závislostí v kroku 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Krok 7: Přiřazení více závislostí položce v mezipaměti

Vzpomeňte si, že `MasterCacheKeyArray` závislosti mezipaměti se používá k zajištění toho, aby *všechny* týkající se produktu data se vyřadí jako z mezipaměti, když se aktualizuje jednu položku v ní spojené. Například `GetProductsByCategoryID(categoryID)` metoda mezipamětí `ProductsDataTables` instance pro každý jedinečný *categoryID* hodnotu. Pokud některý z těchto objektů vyřadí `MasterCacheKeyArray` závislost mezipaměti zajišťuje, že ostatní se taky odeberou. Bez této závislosti mezipaměti při změně dat uložených v mezipaměti existuje možnost, že další data v mezipaměti produktů může být zastaralá. V důsledku toho je důležité, že budeme udržovat `MasterCacheKeyArray` závislost mezipaměti při použití závislostí mezipaměti SQL. Nicméně data do mezipaměti s `Insert` metoda povoluje jenom pro objekt jednu závislost.

Kromě toho při práci s závislosti mezipaměti SQL budeme muset přidružit více databázových tabulek jako závislosti. Například `ProductsDataTable` ukládají do mezipaměti `ProductsCL` třída obsahuje názvy kategorií a dodavatel pro jednotlivé produkty, ale `AddCacheItem` metoda používá pouze závislost na `Products`. V této situaci pokud uživatel aktualizuje název kategorie nebo dodavatele, data produktu uložená v mezipaměti, zůstanou v mezipaměti a být zastaralá. Proto Chceme mít data uložená v mezipaměti produktu závisí na nejen `Products` tabulky, ale na `Categories` a `Suppliers` i tabulky.

[ `AggregateCacheDependency` Třídy](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) poskytuje prostředky pro přidružení k položku mezipaměti více závislostí. Začněte vytvořením `AggregateCacheDependency` instance. Dále přidejte sadu závislosti pomocí `AggregateCacheDependency` s `Add` metody. Při vkládání položky do datové mezipaměti po tomto datu, předejte `AggregateCacheDependency` instance. Když *jakékoli* z `AggregateCacheDependency` změnit instanci s závislostí, bude vyloučena položku z mezipaměti.

Následuje ukázka aktualizovaný kód pro `ProductsCL` třída s `AddCacheItem` metody. Metoda vytvoří `MasterCacheKeyArray` závislosti spolu s mezipaměti `SqlCacheDependency` objekty pro `Products`, `Categories`, a `Suppliers` tabulky. Tyto jsou všechny zkombinované do jedné `AggregateCacheDependency` objekt s názvem `aggregateDependencies`, který je poté předán `Insert` metody.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Otestování tohoto nového kódu si. Nyní se změní na `Products`, `Categories`, nebo `Suppliers` tabulek způsobit, že data uložená v mezipaměti k vyloučení. Kromě toho `ProductsCL` třída s `UpdateProduct` metodu, která je volána při úpravách produktu prostřednictvím prvku GridView, vyloučí `MasterCacheKeyArray` závislosti, což způsobí, že v mezipaměti mezipaměti `ProductsDataTable` vyřazení a data, která mají být znovu načíst následující požadavek.

> [!NOTE]
> Závislosti mezipaměti SQL, je také možné s [ukládání výstupu do mezipaměti](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Ukázku této funkce naleznete v tématu: [pomocí technologie ASP.NET ukládání výstupu do mezipaměti se systémem SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Souhrn

Při ukládání do mezipaměti dat z databáze, data se v ideálním případě zůstanou v mezipaměti, dokud je změněný v databázi. S prostředím ASP.NET 2.0 můžete vytvořit a použít ve scénářích deklarativní a programová závislosti mezipaměti SQL. Jedním z problémů s tímto přístupem Probíhá zjišťování, když se data změnila. Plné verze Microsoft SQL Server 2005 poskytují možnosti oznámení, které aplikace může upozornit, když došlo ke změně výsledku dotazu. Express edici systému SQL Server 2005 a starších verzí SQL serveru systému cyklického dotazování musí použít. Nastavení infrastruktury potřebné cyklického dotazování je naštěstí poměrně jednoduché.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Použití oznámení dotazů v systému Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Vytvoření oznámení dotazů](https://msdn.microsoft.com/library/ms188669.aspx)
- [Ukládání do mezipaměti v ASP.NET s `SqlCacheDependency` třídy](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Nástroj pro registraci serveru SQL technologie ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Přehled `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Marko Rangel Teresy Murphy a Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](caching-data-at-application-startup-cs.md)
> [další](caching-data-with-the-objectdatasource-vb.md)
