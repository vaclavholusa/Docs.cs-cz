---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Dotazování na Data pomocí ovládacího prvku SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: V předchozím kurzech jsme použili ovládacího prvku ObjectDataSource plně z vrstvy přístup k datům oddělení prezentační vrstvy. Od této tutor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d6f681169267ad5c65486c1d1fac0a9396535d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Dotazování na Data pomocí ovládacího prvku SqlDataSource (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) nebo [stáhnout PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> V předchozím kurzech jsme použili ovládacího prvku ObjectDataSource plně z vrstvy přístup k datům oddělení prezentační vrstvy. Od verze v tomto kurzu, jsme zjistěte, jak ovládacího prvku SqlDataSource lze použít pro jednoduché aplikace, které nevyžadují striktní oddělení prezentace a přístup k datům.


## <a name="introduction"></a>Úvod

Všechny z kurzů jsme zkontrolován, pokud jste použili vrstvené architektura skládající se z prezentační, obchodní logiku a Data Access vrstvy. Data přístup Layer (DAL) byl vytvořený v první kurz ([vytváření Data Access Layer](../introduction/creating-a-data-access-layer-cs.md)) a vrstvu obchodní logiky za sekundu ([vytváření vrstvu obchodní logiky](../introduction/creating-a-business-logic-layer-cs.md)). Od verze [zobrazení dat pomocí ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) kurz, jsme viděli, jak pomocí technologie ASP.NET 2.0 s novou prvku ObjectDataSource deklarativně rozhraní s architekturou od prezentační vrstvy.

Při práci s daty všechny kurzy, pokud jste použili architekturu, je také možné přístup, vložit, aktualizovat a odstranit databázi data přímo ze stránky ASP.NET, obcházení architekturu. Díky tomu umístí konkrétní databázové dotazy a obchodní logiku přímo na webové stránce. Pro aplikace dostatečně velká nebo složitá je životně důležitá pro úspěch, aktualizační a jeho udržovatelnost aplikace návrh, implementaci a použití vrstvené architektury. Vývoj robustní architektura, však může být zbytečné při vytváření mimořádně jednoduché jednorázové aplikací.

ASP.NET 2.0 obsahuje pět předdefinovaných datového zdroje ovládací prvky [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [prvku](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), a [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Ve třídě SqlDataSource slouží pro přístup a měnit data přímo z relační databáze, včetně Microsoft SQL Server, Microsoft Access, Oracle, MySQL a dalších. V tomto kurzu a další tři podíváme, jak pracovat s ovládacím prvkem SqlDataSource, prohlížení postup dotazování a data databáze filtru, jakož i způsob použití SqlDataSource k vložit, aktualizovat a odstranit data.


![ASP.NET 2.0 obsahuje pět předdefinovaných dat zdroje ovládacích prvků](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Obrázek 1**: ASP.NET 2.0 obsahuje pět předdefinovaných dat zdroje ovládacích prvků


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Porovnání ObjectDataSource a SqlDataSource

Ovládací prvky ObjectDataSource i SqlDataSource koncepčně, jsou jednoduše proxy servery k datům. Jak je popsáno v [zobrazení dat pomocí ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) kurzu ObjectDataSource obsahuje vlastnosti, které označují typ objektu, který poskytuje data a metody k vyvolání Pokud chcete vybrat, vložit, aktualizovat a odstranit data ze základní typ objektu. Po vlastnosti s ObjectDataSource byly nakonfigurovány, mohou být vázány dat ovládací prvek webu například rutina GridView, DetailsView nebo DataList do ovládacího prvku ObjectDataSource s použitím `Select()`, `Insert()`, `Delete()`, a `Update()` metody komunikovat s základní architektury.

Ve třídě SqlDataSource poskytuje stejné funkce, ale funguje na relační databázi, nikoli knihovny objektů. SqlDataSource můžeme musíte zadat připojovací řetězec databáze a dotazů ad-hoc SQL nebo uložené procedury provést k vložení, aktualizaci, odstranění a načíst data. SqlDataSource s `Select()`, `Insert()`, `Update()`, a `Delete()` metody, při vyvolání, připojení k databázi zadaný a odpovídající dotaz SQL. Jak ukazuje následující diagram, jsou tyto metody udělají tuto práci grunt připojení k databázi, zadání dotazu a vrací výsledky.


![Ve třídě SqlDataSource slouží jako proxy server a databázi](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Obrázek 2**: SqlDataSource slouží jako proxy server a databázi


> [!NOTE]
> V tomto kurzu budete zaměříme na načítání dat z databáze. V [vložení, aktualizace a odstraňování dat pomocí ovládacího prvku SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) kurz, ukážeme, jak nakonfigurovat SqlDataSource k podpoře vkládání, aktualizace a odstranění.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource a AccessDataSource ovládací prvky

Kromě ovládacího prvku SqlDataSource technologii ASP.NET 2.0 obsahuje také ovládacího prvku AccessDataSource. Tyto dva ovládací prvky vést celá řada vývojářů nové na technologii ASP.NET 2.0 předpokládat, že je ovládací prvek AccessDataSource určený pracovat pouze s Microsoft Access pomocí ovládacího prvku SqlDataSource navržený tak, aby pracovat pouze s Microsoft SQL Server. Když AccessDataSource je navržený speciálně s Microsoft Access, ovládacího prvku SqlDataSource pracuje s *žádné* relační databázi, která je přístupná prostřednictvím rozhraní .NET. To zahrnuje všechny OleDb - nebo kompatibilní s rozhraním ODBC úložiště dat, například Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL, PostgreSQL a mezi mnohé další.

Jediný rozdíl mezi ovládacími prvky AccessDataSource a SqlDataSource je, jak zadat informace o připojení databáze. Ovládací prvek AccessDataSource vyžaduje právě cestu k souboru na soubor databáze aplikace Access. SqlDataSource, je zapotřebí na druhé straně úplný připojovací řetězec.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Krok 1: Vytvoření SqlDataSource webové stránky

Než začneme zkoumat jak pracovat přímo s daty databáze pomocí ovládacího prvku SqlDataSource, umožní s nejprve vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro tento kurz a další tři chvíli trvat. Nejprve přidejte novou složku s názvem `SqlDataSource`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Přidání stránky ASP.NET pro kurzy související SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Obrázek 3**: Přidání stránky ASP.NET pro kurzy související SqlDataSource


V jiných složkách, jako `Default.aspx` v `SqlDataSource` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Obrázek 4**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Nakonec přidejte tyto čtyři stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po tlačítka přidání vlastní DataList a opakovače `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vkládání a odstraňování kurzy.


![Mapy webu nyní obsahuje položky pro kurzy SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Obrázek 5**: mapy webu nyní obsahuje položky pro kurzy SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Krok 2: Přidání a konfigurace ovládacího prvku SqlDataSource

Začněte otevřením `Querying.aspx` stránku `SqlDataSource` složku a přepnout do zobrazení návrhu. Přetáhněte ovládací prvek SqlDataSource z panelu nástrojů na Designer a sadu jeho `ID` k `ProductsDataSource`. Stejně jako u ObjectDataSource, SqlDataSource nevytváří žádný výstup vykreslené a proto se zobrazí jako šedé pole na návrhovou plochu. Pokud chcete nakonfigurovat SqlDataSource, klikněte na odkaz Konfigurace zdroje dat z SqlDataSource s inteligentním.


![Klikněte na konfiguraci připojení zdroje dat z SqlDataSource s inteligentní značky](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Obrázek 6**: klikněte na konfiguraci připojení zdroje dat z SqlDataSource s inteligentní značky


Otevře Průvodce konfigurace zdroje dat SqlDataSource ovládacího prvku s. Zatímco průvodce s kroky odlišné z ovládacího prvku ObjectDataSource s cílem je stejný poskytnout podrobné informace o tom, jak načíst, vložit, aktualizovat a odstranit data prostřednictvím zdroj dat. Ve třídě SqlDataSource, to zahrnuje určení základní databáze pro použití a poskytuje příkazy SQL ad-hoc nebo uložené procedury.

Prvním krokem Průvodce nám vyzve k databázi. Rozevírací seznam obsahuje tyto databáze součástí s aplikací webové `App_Data` složky a soubory, které byly přidány do uzlu připojení dat v Průzkumníku serveru. Od jsme jste již přidali připojovací řetězec `NORTHWIND.MDF` databáze v `App_Data` složku pro naše projektu s `Web.config` soubor, rozevíracím seznamu obsahuje odkaz na tento připojovací řetězec `NORTHWINDConnectionString`. Vyberte tuto položku z rozevíracího seznamu a klikněte na tlačítko Další.


![V rozevíracím seznamu vyberte NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Obrázek 7**: Zvolte `NORTHWINDConnectionString` z rozevíracího seznamu


Po výběru databáze, Průvodce zobrazí pro dotaz vrátit data. Můžeme určit buď sloupce tabulky nebo zobrazení pro vrátit nebo můžete zadat vlastní příkaz SQL nebo uloženou proceduru. Můžete přepínat mezi tato volba prostřednictvím určení vlastního příkazu SQL nebo uloženou proceduru a zadat sloupce z tabulky nebo zobrazení přepínače.

> [!NOTE]
> V tomto prvním příkladu umožní s zadat sloupce z tabulky nebo zobrazení možnost použít. Jsme budete později v tomto kurzu se vraťte do průvodce a prozkoumejte určení vlastního příkazu SQL nebo uloženou proceduru možnost.


Obrázek 8 znázorňuje konfigurací při výběru zadat sloupce z tabulky nebo zobrazení přepínače obrazovce příkazu Select. Rozevírací seznam obsahuje sadu tabulek a zobrazení v databázi Northwind s vybranou tabulku nebo zobrazení sloupce s zobrazí zaškrtávací políčko dole v seznamu. V tomto příkladu umožňují s vrátit `ProductID`, `ProductName`, a `UnitPrice` sloupců z `Products` tabulky. Jak ukazuje obrázek 8, po provedením těchto výběrů v průvodci zobrazí výsledná příkaz jazyka SQL `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Vrací Data z tabulky produktů](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Obrázek 8**: vrátit Data z `Products` tabulky


Jakmile jste nakonfigurovali v Průvodci se vrátíte `ProductID`, `ProductName`, a `UnitPrice` sloupců z `Products` tabulky, klikněte na tlačítko Další. Tento poslední obrazovka poskytuje možnost podívejte se na výsledky dotazu nakonfigurované z předchozího kroku. Klepnutím na tlačítko Testovat dotaz provede konfigurovaného `SELECT` příkaz a zobrazí výsledky v mřížce.


![Klikněte na tlačítko Test dotazu zobrazíte vyberte dotazu](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Obrázek 9**: klikněte na tlačítko Test dotazu ke kontrole vaší `SELECT` dotazu


Dokončete průvodce, klikněte na tlačítko Dokončit.

Jako s ObjectDataSource, průvodce s SqlDataSource jenom přiřazuje hodnoty vlastnosti ovládacích prvků s konkrétně [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) a [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) vlastnosti. Po dokončení průvodce se SqlDataSource deklarativní značky ovládacího prvku s vaší by měl vypadat takto:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

`ConnectionString` Vlastnost obsahuje informace o tom, jak připojit k databázi. Tuto vlastnost lze přiřadit hodnotu řetězce dokončení, pevně připojení nebo může ukazovat na připojovací řetězec v `Web.config`. Chcete-li hodnota připojovacího řetězce v souboru Web.config, použijte syntaxi `<%$ expressionPrefix:expressionValue %>`. Obvykle *expressionPrefix* je ConnectionStrings a *expressionValue* je název připojovacího řetězce v `Web.config` [ `<connectionStrings>` části](https://msdn.microsoft.com/library/bf7sd233.aspx). Syntaxe však lze použít k odkazu `<appSettings>` elementy nebo obsah ze zdrojových souborů. V tématu [ASP.NET: Přehled výrazy](https://msdn.microsoft.com/library/d5bd1tad.aspx) Další informace o této syntaxe.

`SelectCommand` Vlastnost určuje ad-hoc příkazu SQL nebo uloženou proceduru provést vrátit data.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Krok 3: Přidání ovládacího prvku webového dat a vytvoření vazby SqlDataSource

Jakmile byl nakonfigurován SqlDataSource, může být vázána k datům ovládací prvek webu, jako je například GridView nebo DetailsView. V tomto kurzu mohli zobrazit data, rutina GridView s. Z panelu nástrojů, přetáhněte GridView na stránku a navázat jej na `ProductsDataSource` SqlDataSource tak, že zvolíte zdroj dat v rozevíracím seznamu v GridView s inteligentním.


[![Přidat GridView a navázat jej do ovládacího prvku SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Obrázek 10**: přidejte GridView a navázat jej do ovládacího prvku SqlDataSource ([Kliknutím zobrazit obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Jakmile jste již vybrali v rozevíracím seznamu v GridView s inteligentním ovládacího prvku SqlDataSource, Visual Studio automaticky přidá BoundField nebo vlastnost CheckBoxField do GridView pro každý z sloupců vrácených dat zdrojového kódu. Vzhledem k tomu, že SqlDataSource vrátí tři databázové sloupce `ProductID`, `ProductName`, a `UnitPrice` v GridView existují tři pole.

Za chvíli nakonfigurovat s GridView tři BoundFields. Změna `ProductName` pole s `HeaderText` vlastnost na název produktu a `UnitPrice` pole s na cenu. Formátovat také `UnitPrice` pole jako měny. Po provedení změny, vaše GridView s deklarativní by měl vypadat takto:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Navštivte tuto stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 11, GridView uvádí každý produkt s `ProductID`, `ProductName`, a `UnitPrice` hodnoty.


[![GridView zobrazí každý produkt s ProductID, ProductName a UnitPrice hodnoty](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Obrázek 11**: The GridView zobrazí každý produkt s `ProductID`, `ProductName`, a `UnitPrice` hodnoty ([Kliknutím zobrazit obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Když je navštívené stránky GridView vyvolá prvek zdroje dat s `Select()` metoda. Pokud jsme použili ovládacího prvku ObjectDataSource, to volat `ProductsBLL` třídu s `GetProducts()` metoda. S SqlDataSource však `Select()` metoda vytvoří připojení k zadané databázi a problémy `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, v tomto příkladu). Ve třídě SqlDataSource vrátí své výsledky, které GridView pak zobrazí, vytváření řádek v GridView pro každý záznam v databázi vrátila.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Funkce integrované dat webové ovládací prvek a ovládacího prvku SqlDataSource

Obecně platí, funkce vyplývajících k datům webové ovládací prvky stránkování, řazení, úpravy, odstranění, vkládání a tak dále jsou specifické pro ovládací prvek webu dat a nejsou závislé na ovládací prvek zdroje dat použít. To znamená GridView můžou využívat jeho vestavěné stránkování, řazení, úpravy a odstraňování, zda je vázána na ObjectDataSource nebo SqlDataSource. Některá data webové funkce nástroje řízení jsou však citlivé prvek zdroje dat, který je používán nebo ovládací prvek s konfigurace zdroje dat.

Například v [efektivně stránkování prostřednictvím velké objemy dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) kurzu jsme probrali jak, ve výchozím nastavení, stránkování logiku pro data webové ovládací prvky naively vrátí *všechny* záznamy ze základní zdroje dat a potom zobrazí pouze příslušnou dílčí sadu záznamů daného index aktuální stránky a počet záznamů zobrazených na stránce. Tento model je vysoce neefektivní při procházení dostatečně velké množství výsledků. Naštěstí ObjectDataSource lze nakonfigurovat pro podporu vlastních stránkování, která vrátí hodnotu jenom přesné podmnožinu záznamů pro zobrazení. Ovládací prvek SqlDataSource však chybí vlastnosti pro implementaci vlastní stránkování.

Ve třídě SqlDataSource stane jiné subtlety s stránkování a řazení. Ve výchozím nastavení lze s daty vrácenými ze SqlDataSource stránkovaného fondu a řadit prostřednictvím GridView. Ukazuje to, zaškrtněte možnost Povolit stránkování a Povolit řazení v GridView s inteligentních značek v `Querying.aspx` a ověřte, že to funguje podle očekávání.

Řazení a stránkování funguje, protože ve třídě SqlDataSource načte dat databáze do volného typu datové sady. Celkový počet záznamů vrácených dotazem zásadní aspekt pro implementaci stránkování lze zjistit z datové sady. Kromě toho lze seřadit výsledky datovou sadu s prostřednictvím v zobrazení DataView. Tyto možnosti jsou automaticky použije SqlDataSource při žádosti o GridView stránkovaného nebo seřazená data.

Ve třídě SqlDataSource lze nakonfigurovat, aby vrátit DataReader – místo datovou sadu změnou jeho [ `DataSourceMode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) z `DataSet` (výchozí) na `DataReader`. Pomocí DataReader může být upřednostňovaná v situacích, při předávání s výsledky SqlDataSource existující kód, který očekává DataReader. Navíc vzhledem k tomu, že DataReaders jsou objekty výrazně jednodušší než datové sady, nabízí lepší výkon. Pokud provedete tuto změnu, ale ovládací prvek webu dat ani seřadit ani stránce vzhledem k tomu, že SqlDataSource nelze zjistit počet záznamů, jsou vrácených dotazem, stejně jako objektu DataReader nabízejí všechny techniky pro řazení vrácená data.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Krok 4: Použití příkazu vlastní SQL nebo uloženou proceduru

Při konfiguraci ovládacího prvku SqlDataSource, jaký dotaz byl použit pro vrácení dat lze v jednom ze dvou přístupů jako vlastní příkaz SQL nebo uloženou proceduru nebo sloupce z existující tabulky nebo zobrazení. V kroku 2 jsme se zaměřili na výběr sloupců z `Products` tabulky. Umožní s, podívejte se na použití vlastního příkazu SQL.

Přidejte další ovládací prvek GridView k `Querying.aspx` stránce a vyberte položku vytvořit nový zdroj dat z rozevíracího seznamu v inteligentní značky. V dalším kroku znamenat, že data bude možné vyžádat z databáze to vytvoří nový ovládací prvek SqlDataSource. Název ovládacího prvku `ProductsWithCategoryInfoDataSource`.


![Vytvořte nový ovládací prvek SqlDataSource s názvem ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Obrázek 12**: Vytvořte nový ovládací prvek SqlDataSource s názvem `ProductsWithCategoryInfoDataSource`


Na další obrazovce zobrazí nám a určete databázi. Jako jsme to udělali zpět na obrázku 7, vyberte `NORTHWINDConnectionString` z rozevíracího seznamu a klikněte na tlačítko Další. V Konfigurace obrazovky vyberte příkaz vyberte určení vlastního příkazu SQL nebo uloženou proceduru přepínač a klikněte na tlačítko Další. Tím se otevře definovat vlastní příkazy nebo uložené procedury obrazovce, která nabízí karty označené SELECT, UPDATE, INSERT a DELETE. V každé kartě můžete zadat vlastní příkaz SQL do textového pole nebo z rozevíracího seznamu vyberte uloženou proceduru. V tomto kurzu se podíváme na zadávání vlastního příkazu SQL; v dalším kurzu obsahuje příklad, který používá uložené procedury.


![Zadejte příkaz vlastní SQL nebo vyberte uložené procedury](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Obrázek 13**: Zadejte příkaz vlastní SQL nebo vyberte uložené procedury


Vlastní příkaz jazyka SQL lze zadat ručně do textového pole nebo jde konstruovat graficky kliknutím na tlačítko Tvůrce dotazů. Tvůrce dotazů nebo textové pole, použijte tento dotaz vrátit `ProductID` a `ProductName` pole z `Products` tabulky pomocí `JOIN` načíst produktu s `CategoryName` z `Categories` tabulky:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Můžete graficky vytvořit dotaz pomocí Tvůrce dotazů](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Obrázek 14**: můžete vytvořit graficky dotazu pomocí Tvůrce dotazů


Po zadání dotazu, klikněte na tlačítko Další pokračujte k obrazovce pro Test dotazu. Kliknutím na tlačítko Dokončit ukončete průvodce SqlDataSource.

Po dokončení průvodce se GridView bude mít tři BoundFields přidána zobrazení `ProductID`, `ProductName`, a `CategoryName` sloupců vrácených dotazem a výsledkem je následující kód:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![GridView ukazuje každý produkt s ID, název název a přidružené kategorie](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Obrázek 15**: ID s The GridView, zobrazuje každou produktu, název a související název kategorie ([Kliknutím zobrazit obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak pro dotazování a zobrazení dat pomocí ovládacího prvku SqlDataSource. Jako ObjectDataSource SqlDataSource slouží jako proxy server, poskytování deklarativní způsob přístupu k datům. Zadejte jeho vlastnosti pro připojení k databázi a SQL `SELECT` dotaz provést; se mohou být zadané prostřednictvím okna vlastnosti nebo pomocí průvodce Konfigurovat zdroje.

`SELECT` Dotazu příklady jsme se zaměřili na v tomto kurzu všechny záznamy vrácených zadaný dotaz. Ovládací prvek SqlDataSource však může zahrnovat `WHERE` klauzule s parametry, jejichž hodnoty jsou přiřazeny prostřednictvím kódu programu, nebo jsou automaticky vyžádat ze zadaného zdroje. Postup vytvoření a používání parametrických dotazů v dalším kurzu podíváme!

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přístup k datům relační databáze](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Přehled ovládacího prvku SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Rychlý start ASP.NET kurzy: SqlDataSource ovládacího prvku](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [The Web.config `<connectionStrings>` Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Odkaz na řetězec připojení databáze.](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Susan Connery Bernadette Leigh a David Suru. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-parameterized-queries-with-the-sqldatasource-cs.md)
