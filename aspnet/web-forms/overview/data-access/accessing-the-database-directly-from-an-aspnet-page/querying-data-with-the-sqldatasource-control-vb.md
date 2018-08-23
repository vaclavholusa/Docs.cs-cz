---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Dotazování na Data ovládacím prvkem SqlDataSource (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozích kurzech jsme použili ovládacího prvku ObjectDataSource prezentační vrstvy plně nezávislá na vrstvě přístupu k datům. Od této tutor...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 341cd518b5875b6cc7739f88fc1a35687ea0e090
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753070"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>Dotazování na Data ovládacím prvkem SqlDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) nebo [stahovat PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> V předchozích kurzech jsme použili ovládacího prvku ObjectDataSource prezentační vrstvy plně nezávislá na vrstvě přístupu k datům. Spouští se s tímto kurzem, Učíme použití ovládacím prvkem SqlDataSource pro jednoduché aplikace, které nevyžadují striktní oddělení prezentace a přístup k datům.


## <a name="introduction"></a>Úvod

Všechny kurzy jsme ve prověřit, pokud jste použili vrstvené architektury skládající se z prezentace, obchodní logika a přístup k datům vrstvy. Data přístup Layer (DAL) byl vytvořený v prvním kurzu ([vytvoření vrstvy přístupu k datům](../introduction/creating-a-data-access-layer-vb.md)) a vrstvu obchodní logiky za sekundu ([vytvoření vrstvy obchodní logiky](../introduction/creating-a-business-logic-layer-vb.md)). Počínaje [zobrazení dat se prvku ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) kurzu jsme viděli, jak pomocí technologie ASP.NET 2.0 s nový ovládací prvek ObjectDataSource deklarativně rozhraní s architekturou od prezentační vrstvy.

I když architektura všechny kurzy, pokud mají použít pro práci s daty, je také možné přístup, vložení, aktualizace a odstranění dat z databáze přímo ze stránky ASP.NET, bez použití architektury. To umístí konkrétní databázové dotazy a obchodní logiku přímo na webové stránce. Pro aplikace dostatečně velká nebo složitá je životně důležité pro úspěch, možnosti aktualizace a udržovatelnosti aplikace návrhu, implementaci a použití vrstvené architektury. Vývoj robustní architektura, ale může být zbytečné při vytváření mimořádně jednoduché jednorázové aplikací.

ASP.NET 2.0 obsahuje pět předdefinovaných datových ovládací prvky zdroje [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [prvku AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), a [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Ve třídě SqlDataSource lze použít pro přístup a úpravy dat přímo z relační databáze, včetně Microsoft SQL Server, Microsoft Access, Oracle, MySQL nebo jiné. V tomto kurzu a další tři prozkoumáme tom, jak pracovat s ovládacím prvkem SqlDataSource, zkoumat, jak provádět dotazy a filtrování dat z databáze, jakož i jak používat ve třídě SqlDataSource k vložit, aktualizovat a odstraňovat data.


![Technologie ASP.NET 2.0 obsahuje pět ovládací prvky předdefinované datové zdroje](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Obrázek 1**: technologie ASP.NET 2.0 obsahuje pět ovládací prvky předdefinované datové zdroje


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Porovnání ObjectDataSource a SqlDataSource

Prvek ObjectDataSource i SqlDataSource ovládací prvky jsou koncepčně, jednoduše proxy servery k datům. Jak je popsáno v [zobrazení dat se prvku ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) kurzu ObjectDataSource má vlastnosti, které označují typ objektu, který poskytuje data a metody, které se má vyvolat pro výběr, vložení, aktualizace a odstranění dat ze základního typu objektu. Až po nakonfigurování vlastností prvku ObjectDataSource s dat webový ovládací prvek například ovládacího prvku GridView, DetailsView nebo DataList mohou být vázány na ovládací prvek, pomocí prvku ObjectDataSource s `Select()`, `Insert()`, `Delete()`, a `Update()` metody pracovat s základní architektuře.

Ve třídě SqlDataSource poskytuje stejné funkce, ale používá relační databáze, spíše než knihovny objektů. S ovládacím prvkem SqlDataSource jsme musíte zadat připojovací řetězec databáze a ad-hoc dotazy SQL nebo uložené procedury k provedení pro vložení, aktualizaci, odstranění a načíst data. SqlDataSource s `Select()`, `Insert()`, `Update()`, a `Delete()` metody, při vyvolání, připojte se k zadané databázi a odpovídající dotaz SQL. Jak ukazuje následující diagram, tyto metody provádět těžkou práci připojení k databázi, zadání dotazu a vrátí výsledky.


![Ve třídě SqlDataSource slouží jako proxy server a databáze](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Obrázek 2**: ve třídě SqlDataSource slouží jako proxy server a databáze


> [!NOTE]
> V tomto kurzu zaměříme na načítání dat z databáze. V [vložení, aktualizace a odstraňování dat ovládacím prvkem SqlDataSource(VB)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) výukový program, uvidíme, jak nakonfigurovat ve třídě SqlDataSource pro podporu vkládání, aktualizaci a odstranění.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource a ovládací prvky prvku AccessDataSource

Kromě ovládacím prvkem SqlDataSource ASP.NET 2.0 obsahuje i ovládací prvek AccessDataSource. Tyto dvě různé ovládací prvky vést celá řada vývojářů nové technologie ASP.NET 2.0 podezření, že ovládací prvek AccessDataSource je navržen pro práci výhradně pomocí aplikace Microsoft Access s ovládacím prvkem SqlDataSource navržené pro práci výhradně se službou Microsoft SQL Server. Když prvku AccessDataSource je navržen pro práci, konkrétně pomocí aplikace Microsoft Access, ovládacím prvkem SqlDataSource funguje s *jakékoli* relační databáze, která je přístupná prostřednictvím rozhraní .NET. To zahrnuje všechny OleDb nebo ODBC-CLS úložišť dat, jako je například Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL a PostgreSQL, mezi mnoha dalších.

Jediný rozdíl mezi ovládacími prvky prvku AccessDataSource a SqlDataSource je, jak zadat informace o připojení k databázi. Ovládací prvek AccessDataSource vyžaduje právě cestu k souboru databáze aplikace Access. Ve třídě SqlDataSource na druhé straně vyžaduje úplný připojovací řetězec.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Krok 1: Vytvoření SqlDataSource webové stránky

Než začneme zkoumat, jak pracovat přímo s ovládacím prvkem SqlDataSource pomocí dat z databáze, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, který budeme potřebovat pro tento kurz a další tři. Začněte přidáním novou složku s názvem `SqlDataSource`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Přidání stránky technologie ASP.NET pro SqlDataSource související kurzy](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Obrázek 3**: Přidání stránky technologie ASP.NET pro SqlDataSource související kurzy


V jiných složkách, jako jsou `Default.aspx` v `SqlDataSource` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Obrázek 4**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


A konečně, přidejte tyto čtyři stránky jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za vlastní tlačítka přidání ovládacích prvků DataList a Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vložení a odstranění kurzy.


![Mapa webu nyní obsahuje záznamy pro SqlDataSource kurzy](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Obrázek 5**: mapy webu nyní obsahuje záznamy pro SqlDataSource kurzy


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Krok 2: Přidání a konfigurace ovládacím prvkem SqlDataSource

Začněte otevřením `Querying.aspx` stránku `SqlDataSource` složky a přejděte do zobrazení návrhu. Přetáhněte ovládací prvek SqlDataSource z panelu nástrojů do návrháře a nastavte jeho `ID` k `ProductsDataSource`. Stejně jako u prvku ObjectDataSource, ovládacím prvkem SqlDataSource nevytvoří žádné vykresleného výstupu a proto se zobrazí jako šedé políčko na návrhové ploše. Konfigurovat ve třídě SqlDataSource, klikněte na odkaz Konfigurovat zdroj dat z inteligentních značek s SqlDataSource.


![Klikněte na konfiguraci propojení na zdroj dat z inteligentních značek s SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Obrázek 6**: klikněte na konfiguraci propojení na zdroj dat z inteligentních značek s SqlDataSource


Tím se zobrazí průvodce Konfigurovat zdroj dat s SqlDataSource ovládacího prvku. Během kroků průvodce s se liší od ovládacího prvku ObjectDataSource s, konečným cílem je poskytovat podrobnosti o tom, jak načíst, vložení, aktualizace a odstranění dat prostřednictvím zdroje dat stejné. Ve třídě SqlDataSource, to zahrnuje určení základní databáze a poskytuje příkazy ad-hoc SQL nebo úložné procedury.

První krok průvodce vyzve nám pro databázi. Rozevírací seznam obsahuje tyto databáze součástí s webovou aplikací `App_Data` složky a ty, které byly přidány do uzlu datového připojení v Průzkumníku serveru. Protože jsme už přidali připojovací řetězec pro `NORTHWIND.MDF` databáze v `App_Data` složku pro náš projekt s `Web.config` soubor, rozevírací seznam obsahuje odkaz na připojovací řetězec, `NORTHWINDConnectionString`. Zvolte tuto položku z rozevíracího seznamu a klikněte na tlačítko Další.


![Z rozevíracího seznamu zvolte NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Obrázek 7**: Zvolte `NORTHWINDConnectionString` z rozevíracího seznamu


Po výběru databáze, průvodce vyzve k zadání dotazu vrátit data. Můžeme určit buď sloupce tabulky nebo zobrazení k vrácení nebo můžete zadat vlastní příkaz SQL nebo uloženou proceduru. Můžete přepínat mezi tato volba prostřednictvím určení vlastní příkaz SQL nebo uloženou proceduru a zadat sloupce z tabulky nebo zobrazení přepínací tlačítka.

> [!NOTE]
> V tomto příkladu první umožní s zadat sloupce z tabulky nebo zobrazení možností použití. Vytvoříme později v tomto kurzu se vraťte do průvodce a prozkoumejte určení vlastní příkaz SQL nebo uloženou proceduru možnost.


Obrázek 8 ukazuje konfigurací při výběru zadat sloupce z tabulky nebo zobrazení přepínací tlačítko na obrazovce příkazu Select. Rozevírací seznam obsahuje sadu tabulek a zobrazení v databázi Northwind pomocí vybrané tabulky nebo sloupce s zobrazení zobrazí zaškrtávací políčko dole v seznamu. V tomto příkladu se vám umožňují s vrátit `ProductID`, `ProductName`, a `UnitPrice` sloupce z `Products` tabulky. Jak ukazuje obrázek 8, po ukazuje výsledný příkaz jazyka SQL, proveďte tyto výběry průvodce `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Vrácení dat z tabulky produktů](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Obrázek 8**: vrátit Data z `Products` tabulky


Po nakonfigurování průvodce se vraťte `ProductID`, `ProductName`, a `UnitPrice` sloupce z `Products` tabulku, klikněte na tlačítko Další. Tento poslední obrazovka poskytuje možnost prozkoumat výsledky dotazu nakonfigurovali v předchozím kroku. Kliknutím na tlačítko Testovat dotaz provede nakonfigurované `SELECT` příkaz a zobrazí výsledky v mřížce.


![Klikněte na tlačítko Test dotazu ke kontrole dotaz SELECT](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Obrázek 9**: klikněte na tlačítko Test dotaz k revizi vaše `SELECT` dotazu


Dokončete průvodce, klikněte na tlačítko Dokončit.

Jako ovládacím prvkem ObjectDataSource, průvodce s SqlDataSource pouze přiřazuje hodnoty vlastnosti ovládacích prvků s totiž [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) a [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) vlastnosti. Po dokončení průvodce se vaše SqlDataSource ovládacího prvku s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString` Vlastnost obsahuje informace o tom, jak se připojit k databázi. Tato vlastnost je možné přiřadit hodnotu kompletní a pevně zakódované připojovacího řetězce nebo může odkazovat na připojovací řetězec v `Web.config`. Pokud chcete odkazovat na hodnotu připojovacího řetězce v souboru Web.config, použijte syntaxi `<%$ expressionPrefix:expressionValue %>`. Obvykle *expressionPrefix* je ConnectionStrings a *expressionValue* je název připojovacího řetězce v `Web.config` [ `<connectionStrings>` části](https://msdn.microsoft.com/library/bf7sd233.aspx). Ale můžete použít syntaxi na odkaz `<appSettings>` prvky nebo obsah ze zdrojových souborů. Zobrazit [ASP.NET: Přehled výrazů](https://msdn.microsoft.com/library/d5bd1tad.aspx) pro další informace o této syntaxi.

`SelectCommand` Vlastnost určuje ad-hoc příkaz SQL nebo uloženou proceduru provést, aby vrátit data.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Krok 3: Přidání ovládacího prvku webových dat a vytvoříte jejich vazbu na ovládacím prvkem SqlDataSource

Jakmile ve třídě SqlDataSource není nakonfigurovaná, může být vázána k datům webový ovládací prvek, jako je například GridView nebo prvku DetailsView. Pro účely tohoto kurzu nechte s zobrazení dat v GridView. Z panelu nástrojů přetáhněte na stránku GridView a vytvořte mu vazbu k `ProductsDataSource` SqlDataSource výběrem zdroj dat z rozevíracího seznamu v prvku GridView s inteligentním.


[![Přidat GridView a svázat s ovládacím prvkem SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Obrázek 10**: Přidat GridView a svázat s ovládacím prvkem SqlDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


Jakmile jste již vybrali ovládacím prvkem SqlDataSource z rozevíracího seznamu v prvku GridView s inteligentním, Visual Studio automaticky přidá vlastnost BoundField nebo třídě CheckBoxField do prvku GridView. pro jednotlivé sloupce vrácený ovládací prvek zdroje dat. Vzhledem k tomu, že ve třídě SqlDataSource vrátí tři databázové sloupce `ProductID`, `ProductName`, a `UnitPrice` existují tři pole v prvku GridView.

Za chvíli ke konfiguraci GridView s třemi BoundFields. Změnit `ProductName` pole s `HeaderText` nastavte na název produktu a `UnitPrice` pole s cenou. Formátovat také `UnitPrice` poli jako měnu. Po provedení těchto změn, vaše GridView s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Navštivte tuto stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 11 prvku GridView uvádí jednotlivé produkty s `ProductID`, `ProductName`, a `UnitPrice` hodnoty.


[![GridView zobrazí každý produkt s ProductID, ProductName a UnitPrice hodnoty](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Obrázek 11**: každý produkt zobrazí GridView s `ProductID`, `ProductName`, a `UnitPrice` hodnoty ([kliknutím ji zobrazíte obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


Při návštěvě stránky prvku GridView vyvolá jeho ovládací prvek zdroje dat s `Select()` metody. Když jsme používali ovládací prvek ObjectDataSource, označuje `ProductsBLL` třída s `GetProducts()` metoda. S ovládacím prvkem SqlDataSource, ale `Select()` metoda naváže připojení k zadané databázi a problémy `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, v tomto příkladu). Ve třídě SqlDataSource vrátí jeho výsledky, které prvku GridView. pak vytvoří výčet, vytvoření řádku v prvku GridView. pro každý záznam v databázi vrátil.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Funkce ovládacího prvku webového předdefinované datové a ovládacím prvkem SqlDataSource

Obecně funkce přináší dat webové ovládací prvky stránkování, řazení, úprava, odstranění, vkládání a tak dále jsou specifické pro ovládací prvek webových dat a nejsou závislé na ovládací prvek zdroje dat použít. Prvku GridView. to znamená, můžete využít integrované stránkování, řazení, úpravy a odstranění, zda je vázán prvku ObjectDataSource nebo SqlDataSource. Určitá data webové funkce správy jsou však citlivé ovládací prvek zdroje dat se používají a konfigurace ovládacího prvku s zdroje dat.

Například v [efektivně stránkování prostřednictvím velkých objemů dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) kurzu jsme probírali jak, ve výchozím nastavení, stránkování logiky pro data webové ovládací prvky naively vrátí *všechny* záznamy ze základního zdroje dat a potom zobrazí jenom na příslušnou podmnožinu aktuálního indexu stránky a počet záznamů zobrazených na stránce záznamy. Tento model je velmi neefektivní při procházení dostatečně velké množství výsledků. Naštěstí ObjectDataSource můžou být nakonfigurované pro podporu vlastní stránkování, které vrací pouze přesné podmnožinu záznamů pro zobrazení. Ovládacím prvkem SqlDataSource, ale chybí vlastnosti pro implementaci vlastní stránkování.

S ovládacím prvkem SqlDataSource nastane jiný subtlety s stránkování a řazení. Ve výchozím nastavení lze stránkovat data vrácená z SqlDataSource nebo seřazeny pomocí prvku GridView. Abychom si předvedli to, zaškrtněte možnost Povolit stránkování a Povolit řazení v prvku GridView s inteligentních značek v `Querying.aspx` a ověřte, že to funguje podle očekávání.

Řazení a stránkování funguje, protože ve třídě SqlDataSource databáze data načte do datové sady volného typu. Celkový počet záznamů vrácených dotazem zásadní aspekt k implementaci stránkování lze zjistit z datové sady. Kromě toho můžete být řazeny výsledky s datovou sadu prostřednictvím DataView. Tyto funkce používají ve třídě SqlDataSource automaticky, při žádosti GridView stránkovat nebo seřazená data.

Ve třídě SqlDataSource lze nastavit k vrácení objektu DataReader místo datové sady tak, že změníte jeho [ `DataSourceMode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) z `DataSet` (výchozí) pro `DataReader`. Pomocí čtečky dat může být líp v situacích, při předávání SqlDataSource výsledky s existující kód, který očekává, že čtečky dat Navíc vzhledem k tomu, že čtečky dat jsou výrazně jednodušší objekty do datové sady, nabízí lepší výkon. Pokud tuto změnu však můžete ovládací prvek webových dat ani řadit ani stránku, protože ve třídě SqlDataSource nelze zjistit, kolik záznamů se vrácené dotazem, ani neprovádí objektu DataReader nabízí všechny metody pro řazení vrácená data.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Krok 4: Použití příkazu vlastní SQL nebo uloženou proceduru

Při konfiguraci ovládacím prvkem SqlDataSource dotazu používá k vrácení dat lze v jednom ze dvou následujících metod jako vlastní příkaz SQL nebo uloženou proceduru nebo jako sloupce z existující tabulky nebo zobrazení. V kroku 2 jsme se zaměřili na výběr sloupce z `Products` tabulky. Umožní s, podívejte se na použití vlastního příkazu SQL.

Přidejte další ovládací prvek GridView k `Querying.aspx` stránky a zvolte možnost vytvořit nový zdroj dat z rozevíracího seznamu v inteligentních značek. V dalším kroku znamenat, že data se načíst z databáze tím se vytvoří nový ovládací prvek SqlDataSource. Pojmenujte ovládací prvek `ProductsWithCategoryInfoDataSource`.


![Vytvořit nový ovládací prvek SqlDataSource s názvem ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Obrázek 12**: Vytvořte nový ovládací prvek SqlDataSource s názvem `ProductsWithCategoryInfoDataSource`


Na další obrazovce dotazem, abychom mohli zadat databázi. Jako jsme to udělali zpět na obrázku 7, vyberte `NORTHWINDConnectionString` z rozevíracího seznamu a klikněte na tlačítko Další. V konfigurovat příkaz Select obrazovky zvolte možnost určení vlastní příkaz SQL nebo uloženou proceduru přepínací tlačítka a klikněte na tlačítko Další. Tím se otevře obrazovku definovat vlastní příkazy nebo uložené procedury, která nabízí karty označené SELECT, UPDATE, INSERT a DELETE. Na každé kartě můžete do textového pole zadejte vlastní příkaz SQL nebo uloženou proceduru z rozevíracího seznamu zvolte. V tomto kurzu se podíváme na zadání vlastní příkaz jazyka SQL; Další kurz obsahuje příklad použití uložené procedury.


![Zadejte příkaz SQL vlastní nebo vyberte uloženou proceduru](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Obrázek 13**: Zadejte příkaz SQL vlastní nebo vyberte uloženou proceduru


Vlastní příkaz jazyka SQL, můžete ručně zadat do textového pole nebo lze sestavit graficky kliknutím na tlačítko Tvůrce dotazů. Z editoru dotazů nebo textového pole, použijte tento dotaz se vraťte `ProductID` a `ProductName` pole z `Products` tabulky pomocí `JOIN` načíst produkt s `CategoryName` z `Categories` tabulky:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Můžete graficky sestavování dotazu pomocí Tvůrce dotazů](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Obrázek 14**: můžete vytvořit graficky dotazu pomocí Tvůrce dotazů


Po zadání dotaz, klikněte na tlačítko Další přejděte na obrazovku testovat dotaz. Kliknutím na Dokončit dokončíte Průvodce SqlDataSource.

Po dokončení průvodce se prvku GridView, bude mít tři BoundFields přidá do něj zobrazení `ProductID`, `ProductName`, a `CategoryName` sloupců vrácených dotazem a výsledkem je následující kód:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![Každý produkt s ID, název názvu a přidružené kategorie zobrazuje prvku GridView.](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Obrázek 15**: The ovládacího prvku GridView, ukazuje, každý produkt s ID, název a název kategorie přidružené ([kliknutím ji zobrazíte obrázek v plné velikosti](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak dotazovat a zobrazení dat ovládacím prvkem SqlDataSource použitím. Stejně jako prvku ObjectDataSource ve třídě SqlDataSource slouží jako proxy server, poskytují deklarativní přístup k přístupu k datům. Určit jeho vlastnosti pro připojení k databázi a SQL `SELECT` dotaz k provedení, mohou být určeny pomocí okna vlastnosti nebo pomocí průvodce nakonfigurujte zdroj dat.

`SELECT` Příklady dotazů v tomto kurzu jsme se zaměřili vrátí všechny záznamy z zadaný dotaz. Ovládacím prvkem SqlDataSource však můžete zahrnout `WHERE` klauzule s parametry, jejichž hodnoty jsou přiřazeny prostřednictvím kódu programu nebo se automaticky vyžádá ze zadaného zdroje. Vytvoření a použití parametrizovaných dotazů v dalším kurzu prozkoumáme!

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přístup k datům relační databáze](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Přehled ovládacího prvku SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Kurzy rychlý start ASP.NET: Ovládacím prvkem SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Soubor Web.config `<connectionStrings>` – Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Odkaz na řetězec připojení databáze](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Susan Connery Bernadette Leigh a David Suru. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [další](using-parameterized-queries-with-the-sqldatasource-vb.md)
