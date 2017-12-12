---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: "Ukládání dat pomocí ObjectDataSource (VB) | Microsoft Docs"
author: rick-anderson
description: "Ukládání do mezipaměti může znamenat, že rozdíl mezi pomalou a rychlé webové aplikace. V tomto kurzu je první čtyři, prohlédněte si podrobné ukládání do mezipaměti technologie ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: fa0a0f1f80a407f8f68d5fe081b5b144e2945700
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-with-the-objectdatasource-vb"></a>Ukládání dat do mezipaměti s ObjectDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) nebo [stáhnout PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> Ukládání do mezipaměti může znamenat, že rozdíl mezi pomalou a rychlé webové aplikace. V tomto kurzu je první čtyři, prohlédněte si podrobné ukládání do mezipaměti technologie ASP.NET. Zjistěte, klíčové koncepty ukládání do mezipaměti a jak se má použít na prezentační vrstvu prostřednictvím ovládacího prvku ObjectDataSource ukládání do mezipaměti.


## <a name="introduction"></a>Úvod

V oblasti vědy *ukládání do mezipaměti* je proces trvá data nebo informace, která je nákladná, získání a ukládání jeho kopii v umístění, které je rychlejší přístup. Pro datové aplikace spotřebovávat velký a komplexní dotazy běžně většinu doby spuštění aplikace s. Tyto s výkonu aplikace, pak je možné zlepšit často uloží výsledky dotazů nákladné databázi v paměti s aplikací.

Technologie ASP.NET 2.0 nabízí celou řadu možností ukládání do mezipaměti. Celou webovou stránku nebo uživatelský ovládací prvek s vykreslení značek můžete uložit do mezipaměti, prostřednictvím *ukládání výstupu do mezipaměti*. Ovládací prvky ObjectDataSource a SqlDataSource nabízí ukládání do mezipaměti možnosti i, a tím umožní data do mezipaměti na úrovni ovládacího prvku. A ASP.NET s *datové mezipaměti* poskytuje bohaté rozhraní API ukládání do mezipaměti, která umožňuje vývojářům stránku prostřednictvím kódu programu mezipaměti objektů. V tomto kurzu a další tři, které podíváme pomocí ObjectDataSource s ukládání do mezipaměti funkcí, jakož i datové mezipaměti. Také jsme budete prozkoumejte jak pro ukládání do mezipaměti data celou aplikaci při spuštění a jak chcete zachovat data uložená v mezipaměti novou prostřednictvím závislosti mezipaměti SQL. Tyto kurzy není prozkoumejte ukládání výstupu do mezipaměti. Podrobný pohled ukládání výstupu do mezipaměti, najdete v části [ukládání výstupu do mezipaměti v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Ukládání do mezipaměti se dá použít na libovolné místo v architektuře, z Data Access Layer až prostřednictvím prezentační vrstvy. V tomto kurzu budeme zabývat použití ukládání do mezipaměti na prezentační vrstvu prostřednictvím ovládacího prvku ObjectDataSource. V dalším kurzu, které podíváme ukládání do mezipaměti data na vrstvu obchodní logiky.

## <a name="key-caching-concepts"></a>Klíč ukládání do mezipaměti koncepty

Ukládání do mezipaměti může výrazně zlepšit aplikaci s celkový výkon a škálovatelnost převzetím data, která je nákladná, ke generování a ukládání jeho kopii v umístění, které mají efektivnější přístup. Vzhledem k tomu, že mezipaměť obsahuje pouze kopie skutečný, zdrojová data, můžete začnou být zastaralé, nebo *zastaralé*, pokud se změní v základních datech. Boje proti to, vývojář stránky může naznačovat kritéria, podle kterých bude možné položku mezipaměti *vyřazování* z mezipaměti, buď pomocí:

- **Založené na čase kritéria** položky mohou být přidány do mezipaměti pro absolutní nebo klouzavé doby trvání. Třeba vývojář stránky může znamenat trvání, například 60 sekund. S absolutní dobou trvání položky v mezipaměti vyřazování 60 sekund poté, co byl přidán do mezipaměti, bez ohledu na to, jak často se používají. S určitou dobou trvání klouzavou položce v mezipaměti vyřazování 60 sekund po posledním přístupu.
- **Na základě závislostí kritéria** závislost může být přidružený položce při přidány do mezipaměti. Když se změní položky s závislost je vyřazena z mezipaměti. Závislost může být soubor, další položky mezipaměti nebo kombinaci obojího. ASP.NET 2.0 umožňuje také závislosti mezipaměti SQL, které umožňuje vývojářům přidání položky do mezipaměti a mějte ho vyřazování při změně podkladového dat databáze. Vyzkoušíme závislosti mezipaměti SQL v nadcházející akce [pomocí závislosti mezipaměti SQL](using-sql-cache-dependencies-vb.md) kurzu.

Bez ohledu na zadaných kritériích vyřazení, může být položky v mezipaměti *úklid* předtím, než byla splněna kritéria založené na čase nebo na základě závislostí. Pokud mezipaměti již dosáhl maximálního, musíte před přidáním nové odstranit existující položky. V důsledku toho při prostřednictvím kódu programu práci se službou data uložená v mezipaměti ho s důležitých, že jste vždy předpokládáme, že data uložená v mezipaměti nemohou být obsaženy. Podíváme vzor při přístupu k datům z mezipaměti prostřednictvím kódu programu v našem kurzu Další *ukládání dat v architektuře*.

Ukládání do mezipaměti poskytuje ekonomické prostředky pro stlačení další výkonu z aplikace. Jako [Steven Smith](http://aspadvice.com/blogs/ssmith/) articulates v jeho článku [ukládání do mezipaměti ASP.NET: technik a nejlepších postupů](https://msdn.microsoft.com/en-us/library/aa478965.aspx):

Ukládání do mezipaměti, může být dobrým způsobem, jak získat dobrý dostatečný výkon bez náročné na čas a analýzy. Paměť je levných, takže pokud získáte výkonu, je nutné pomocí ukládání výstupu do mezipaměti pro 30 sekund místo výdaje denně nebo týdně pokusu optimalizovat kód nebo databáze, proveďte řešení ukládání do mezipaměti (za předpokladu, že 30 - stará sekundu data je ok) a přesunutí na. Nakonec nízký návrhu bude pravděpodobně nezaznamenaly, takže kurzu, že byste měli zkusit návrhu aplikace správně. Ale pokud potřebujete získat dobrý dnes dostatečný výkon, ukládání do mezipaměti může být vynikající [přístup], kdybyste kupovali času na Refaktorovat aplikace později, kdy máte čas Uděláte to tak.

Při ukládání do mezipaměti může poskytnout výrazné vylepšení, není použitelný ve všech situacích, například s aplikacemi, které využívají v reálném čase, často aktualizace dat, nebo kde je zastaralá data i krátce žít nepřijatelné. Ale pro většinu aplikací, musí používat ukládání do mezipaměti. Pro další informace o ukládání do mezipaměti v technologii ASP.NET 2.0, naleznete [ukládání do mezipaměti pro výkon](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) části [ASP.NET 2.0 rychlý start kurzy](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Krok 1: Vytvoření ukládání do mezipaměti webové stránky

Než začneme naše zkoumání funkce ukládání do mezipaměti s ObjectDataSource, umožní s nejprve vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro tento kurz a další tři chvíli trvat. Nejprve přidejte novou složku s názvem `Caching`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Přidání stránky ASP.NET pro kurzy související s ukládání do mezipaměti](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Obrázek 1**: Přidání stránky ASP.NET pro kurzy související s ukládání do mezipaměti


V jiných složkách, jako `Default.aspx` v `Caching` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Obrázek 2: Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Obrázek 2**: obrázek 2: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image4.png))


Nakonec přidejte tyto stránek jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po práci s binární Data `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro kurzů k ukládání do mezipaměti.


![Mapy webu nyní obsahuje položky pro ukládání do mezipaměti kurzy](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Obrázek 3**: mapy webu nyní obsahuje položky pro ukládání do mezipaměti kurzy


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Krok 2: Zobrazení Seznam produktů na webové stránce

Tento tento kurz popisuje, jak používat ObjectDataSource funkcí ovládacího prvku s integrovanou ukládání do mezipaměti. Než můžete podíváme na tyto funkce, ale nejdřív potřebujeme stránku, kterou chcete pracovat. Umožňují s vytvořit webovou stránku, která používá rutina GridView načíst ObjectDataSource z seznam informací o produktech `ProductsBLL` třídy.

Začněte otevřením `ObjectDataSource.aspx` stránku `Caching` složky. Přetáhněte GridView z panelu nástrojů na návrháře, nastavte jeho `ID` vlastnost `Products`a od jeho inteligentních značek, vyberte pro vytvoření vazby do ovládacího prvku ObjectDataSource nové s názvem `ProductsDataSource`. Konfigurace ObjectDataSource pro práci s `ProductsBLL` třídy.


[![Konfigurace ObjectDataSource použití třídy ProductsBLL](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Obrázek 4**: Konfigurace ObjectDataSource pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image8.png))


Pro tuto stránku umožní s vytvoří upravitelné GridView, takže jsme můžete zkontrolovat, co se stane, když dojde k úpravě dat uložených v mezipaměti v ObjectDataSource přes rozhraní s GridView. Ponechejte rozevíracím seznamu na kartě vyberte nastavení na svou výchozí `GetProducts()`, ale změnit vybrané položky v kartě aktualizace tak, aby `UpdateProduct` přetížení, které přijímá `productName`, `unitPrice`, a `productID` jako jeho vstupní parametry.


[![Nastavte aktualizace karta s rozevíracím seznamu příslušné UpdateProduct přetížení](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Obrázek 5**: nastavte aktualizace karta s rozevíracím seznamu na vhodná `UpdateProduct` přetížení ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image11.png))


Nakonec nastavte rozevírací seznamy na kartách INSERT a DELETE (None) a klikněte na tlačítko Dokončit. Po dokončení průvodce Konfigurace zdroje dat, nastaví Visual Studio ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`. Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurzu, tato vlastnost je nutné odebrat z deklarativní syntaxi nebo nastavit zpět na výchozí hodnotu, `{0}`, aby naše pracovního postupu aktualizace Pokračujte bez chyby.

Kromě toho po dokončení průvodce Visual Studio přidá pole do GridView pro každé pole data produktu. Odeberte všechny ale na `ProductName`, `CategoryName`, a `UnitPrice` BoundFields. Potom aktualizujte `HeaderText` vlastnosti každého z těchto BoundFields na produkt, kategorie a cenu v uvedeném pořadí. Vzhledem k tomu `ProductName` pole je povinné, převést BoundField TemplateField a přidejte RequiredFieldValidator k `EditItemTemplate`. Podobně, převést `UnitPrice` BoundField do TemplateField a přidejte CompareValidator pro zajištění platný měny hodnotu této s větší než nebo rovna hodnotě nula. hodnota zadaná uživatelem. Kromě tyto úpravy Nebojte se provádět změny estetické například zarovnání doprava `UnitPrice` hodnotu, nebo zadáte formátování pro `UnitPrice` textu v jeho rozhraní jen pro čtení a úpravy.

Zkontrolujte GridView zaškrtnutím políčka Povolit úpravy v GridView s inteligentním, upravovat. Také zkontrolujte zaškrtávací políčka Povolit stránkování a Povolit řazení.

> [!NOTE]
> Potřebujete o tom, jak přizpůsobit rozhraní GridView s úpravy? Pokud ano, přečtěte si téma zpět [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzu.


[![Povolte podporu GridView pro úpravy, řazení a stránkování](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Obrázek 6**: Povolení podpory GridView pro úpravy, řazení a stránkování ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image14.png))


Po provedení změny GridView, rutina GridView a ObjectDataSource s deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Jak je vidět na obrázku 7, upravovat GridView uvádí název, kategorie a cena jednotlivých produktů v databázi. Za chvíli k otestování řazení funkce stránky s výsledky, procházet a úpravy záznamů.


[![Každý produkt s název, kategorie a cena je uveden v Sortable, Pageable, upravovat GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Obrázek 7**: každý produkt s název, kategorie a cena je uveden v Sortable, Pageable, upravovat GridView ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Krok 3: Prozkoumání při the ObjectDataSource se požaduje Data

`Products` GridView načte data pro zobrazení vyvoláním `Select` metodu `ProductsDataSource` ObjectDataSource. Tato ObjectDataSource vytvoří instanci s vrstvu obchodní logiky `ProductsBLL` třídy a volání jeho `GetProducts()` metoda, která volá Data Access Layer s `ProductsTableAdapter` s `GetProducts()` metoda. Metoda DAL připojuje k databázi Northwind a vydá konfigurovaného `SELECT` dotazu. Tato data se pak vrátí DAL, který zabalí v `NorthwindDataTable`. Objekt DataTable je vrácen do BLL, který vrátí ji do ObjectDataSource, který vrátí ji do GridView. Vytvoří GridView `GridViewRow` objekt pro každou `DataRow` DataTable a pro každou `GridViewRow` nakonec vykreslením do kódu HTML, který je vrácen do klienta a zobrazit v prohlížeči návštěvníka s.

Toto pořadí událostí dojde každé době GridView potřebuje k vytvoření vazby. jeho základní data. K tomu dojde, když je je nejdřív navštívené stránky, při přesouvání z jedné stránky dat na jiný, při řazení GridView nebo při změně dat s GridView prostřednictvím jeho vestavěné úpravy nebo odstranění rozhraní. Pokud je stav zobrazení s GridView vypnutá, GridView bude odrážejí na každé zpětné volání. GridView můžete také být explicitně odrážejí na svá data pomocí volání jeho `DataBind()` metoda.

Plně vyhodnotit četnost, se kterým jsou načtena data z databáze, umožní s zobrazí zprávu s upozorněním, když se znovu načtena data. Přidání ovládacího prvku popisek webové výše GridView s názvem `ODSEvents`. Vymažte jeho `Text` vlastnost a sadu jeho `EnableViewState` vlastnost `False`. Pod štítku, přidání ovládacího prvku tlačítko a nastavit jeho `Text` vlastnost na zpětné volání.


[![Přidání popisku a tlačítko na stránku výše GridView](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Obrázek 8**: Přidání popisku a tlačítko na stránce nahoře GridView ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image20.png))


Během přístupu pracovního postupu systému dat, ObjectDataSource s `Selecting` aktivuje událost před vytvořením základní objekt a jeho nakonfigurovaná metoda volána. Vytvoření obslužné rutiny události pro tuto událost a přidejte následující kód:


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

Pokaždé, když ObjectDataSource odešle požadavek architekturu pro data, zobrazí popisek aktivována událost zvolíte textu.

Navštivte tuto stránku v prohlížeči. Při první je navštívené stránky, je aktivována událost zvolíte textu zobrazí. Klikněte na tlačítko zpětného volání a poznamenejte si, že text zmizí (za předpokladu, že rutina GridView s `EnableViewState` je nastavena na `True`, výchozí hodnota). Je to proto, že na zpětné volání, GridView je znovu vytvořena z svůj stav zobrazení, a proto nemá t zapněte k ObjectDataSource pro svoje data. Řazení, stránkování, nebo úpravě dat, ale způsobí, že rutina GridView se svázat svůj zdroj dat, a proto zvolíte událost aktivována se bude zobrazovat text.


[![Vždy, když je GridView odrážejí svůj zdroj dat, zobrazí se události zvolíte aktivováno](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Obrázek 9**: je vždy, když GridView odrážejí svůj zdroj dat, se zobrazí události zvolíte aktivováno ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![Kliknutím na tlačítko Postback způsobí GridView k znovu vytvořena z svůj stav zobrazení](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Obrázek 10**: Klepnutím na tlačítko Postback způsobí, že rutina GridView k znovu vytvořena z svůj stav zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image26.png))


To nemusí připadat plýtvání k načtení dat databáze pokaždé, když je stránkovaného prostřednictvím nebo seřazená data. Po všech od jsme znovu pomocí výchozího stránkování ObjectDataSource má načíst všechny záznamy při zobrazení na první stránku. I v případě, že GridView neposkytuje řazení a stránkování podpory, data musí být načíst z databáze pokaždé, když je je nejdřív navštívené stránky žádný uživatel (a na každý zpětné volání, pokud je stav zobrazení zakázán). Ale pokud GridView se zobrazuje stejná data pro všechny uživatele, jsou tyto požadavky navíc databáze nadbytečné. Proč ne výsledky vrácené z mezipaměti `GetProducts()` metoda a vazby GridView těm, které do mezipaměti výsledky?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Krok 4: Ukládání dat pomocí ObjectDataSource

Nastavením jednoduše několik vlastností, můžete nakonfigurovat ObjectDataSource automaticky data do mezipaměti na jeho načtené v datové mezipaměti technologie ASP.NET. Následující seznam shrnuje vlastnosti související s mezipamětí ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) musí být nastavena na `True` povolení ukládání do mezipaměti. Výchozí hodnota je `False`.
- [CacheDuration](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) množství času v sekundách, který je uložen v mezipaměti. Výchozí hodnota je 0. ObjectDataSource bude pouze mezipaměti dat, pokud `EnableCaching` je `True` a `CacheDuration` je nastaven na hodnotu větší než nula.
- [CacheExpirationPolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) může být nastaven na `Absolute` nebo `Sliding`. Pokud `Absolute`, ObjectDataSource ukládá do mezipaměti jeho načtená data pro `CacheDuration` sekund; Pokud `Sliding`, vyprší platnost dat pouze po nebyl byly přístupné pro `CacheDuration` sekund. Výchozí hodnota je `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) pomocí této vlastnosti můžete přidružit existující závislost mezipaměti položky mezipaměti s ObjectDataSource. Položky dat s ObjectDataSource můžete předčasně vyloučena z mezipaměti zneplatněním přidružené `CacheKeyDependency`. Tato vlastnost se nejčastěji používá přidružit mezipaměti s ObjectDataSource mezipaměti závislost SQL, téma jsme budete prozkoumat v budoucnu [pomocí závislosti mezipaměti SQL](using-sql-cache-dependencies-vb.md) kurzu.

Umožní s nakonfigurovat `ProductsDataSource` ObjectDataSource pro ukládání do mezipaměti po dobu 30 sekund v měřítku absolutní jeho data. Nastavit ObjectDataSource s `EnableCaching` vlastnost `True` a jeho `CacheDuration` vlastnost do 30. Ponechte `CacheExpirationPolicy` vlastností nastavenou na výchozí `Absolute`.


[![Konfigurace ObjectDataSource pro ukládání do mezipaměti jeho Data po dobu 30 sekund.](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Obrázek 11**: Konfigurace ObjectDataSource pro ukládání do mezipaměti jeho Data po dobu 30 sekund ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image29.png))


Uložte změny a pokroku tuto stránku v prohlížeči. Výběr textu je aktivována událost se zobrazí při první návštěvě stránky, protože původně dat není v mezipaměti. Ale následné postback aktivovány klepnutím na tlačítko zpětného volání, řazení, stránkování, nebo kliknutím na tlačítko Upravit nebo zrušit *není* redisplay zvolíte událost je aktivována text. Důvodem je, že `Selecting` událostí pouze aktivuje se v případě ObjectDataSource získává data z jeho základní objekt; `Selecting` událost neaktivuje, pokud jsou získávány z datové mezipaměti.

Po 30 sekund bude vyloučena data z mezipaměti. Data budou odstraněna také z mezipaměti, pokud ObjectDataSource s `Insert`, `Update`, nebo `Delete` jsou vyvolány metody. V důsledku toho zobrazení událostí zvolíte text aktivováno po 30 sekund předané bylo stisknuto tlačítko aktualizace, řazení, stránkování, nebo kliknutím na tlačítko Upravit nebo zrušit způsobí, že ObjectDataSource pro získání dat z jeho základní objekt, když `Selecting` je aktivována událost. Tyto vrácených výsledků jsou umístěny zpět do datové mezipaměti.

> [!NOTE]
> Pokud se zobrazí výběr textu událost je aktivována často, i v případě, že byste měli ObjectDataSource pracujete data uložená v mezipaměti, může být z důvodu omezení paměti. Pokud není k dispozici dostatek volné paměti, dat přidaných do mezipaměti ObjectDataSource může mít byla úklid. Pokud se zdá být správně ukládání do mezipaměti data nebo pouze mezipamětí t nemá ObjectDataSource data k, zavřete některé aplikace a volné paměti a zkuste to znovu.


Obrázek 12 znázorňuje s ObjectDataSource ukládání do mezipaměti pracovního postupu. Když zvolíte událost aktivována text se zobrazí na obrazovce, že se data nebyla v mezipaměti a museli být načtena z podkladových objektů. Když tento text nebyl nalezen, ale je s protože dat nebyly k dispozici z mezipaměti. Pokud je vrácen data z mezipaměti zde s bez volání základní objekt a proto žádné databázový dotaz spustit.


![ObjectDataSource úložiště a načte Data z datové mezipaměti](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Obrázek 12**: ObjectDataSource ukládá a načte Data z datové mezipaměti


Každá aplikace technologie ASP.NET má svou vlastní datové mezipaměti instance této s sdílet všech stránek a návštěvníky. To znamená, že data uložená v mezipaměti dat pomocí ObjectDataSource podobně sdílet všichni uživatelé, kteří najdete na stránce. Chcete-li to ověřit, otevřete `ObjectDataSource.aspx` stránku v prohlížeči. Při první návštěvě stránky se zobrazí výběr textu událost je aktivována (za předpokladu, že data přidány do mezipaměti podle předchozích testů, nyní by byla odebrána). Otevřít druhou instanci prohlížeče a zkopírujte a vložte adresu URL z první instance prohlížeče na druhý. Ve druhé instance prohlížeč není zobrazen výběr textu událostí aktivováno, protože ho používají stejné mezipaměti data jako první.

Při vkládání jeho načtená data do mezipaměti, ObjectDataSource používá hodnotu klíče mezipaměti, která zahrnuje: `CacheDuration` a `CacheExpirationPolicy` hodnoty vlastností; typ podkladového objektu obchodní používá ObjectDataSource, který je určený prostřednictvím [ `TypeName` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, v tomto příkladu); hodnota `SelectMethod` vlastnost a názvu a hodnot parametrů v `SelectParameters` kolekce; a hodnoty jeho `StartRowIndex`a `MaximumRows` vlastnosti, které se používají při implementaci [vlastní stránkování](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

Položka jedinečný mezipaměti věnujte hodnota klíče mezipaměti jako kombinaci těchto vlastností zajišťuje, jak tyto hodnoty změnit. Například v posledních kurzy jsme sunout hledá pomocí `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)`, která vrací všechny produkty, pro zadané kategorii. Jeden uživatel by mohl pocházet na stránku a zobrazení nápoje, který má `CategoryID` 1. Pokud ObjectDataSource mezipaměti své výsledky bez ohledu na `SelectParameters` hodnoty, když byla přijata jiného uživatele na stránku zobrazíte přísady při produkty nápoje byly v mezipaměti, zobrazí se jim d přísady produktů uloženou v mezipaměti nápoj spíše než. Pomocí různých klíče mezipaměti podle těchto vlastností, které obsahují hodnoty `SelectParameters`, ObjectDataSource udržuje samostatné položky v mezipaměti pro nápoje a přísady.

## <a name="stale-data-concerns"></a>Zastaralá Data otázky

ObjectDataSource automaticky vyloučí jeho položky z mezipaměti, pokud jeden z jeho `Insert`, `Update`, nebo `Delete` vyvolání metody. Tato funkce pomáhá chránit proti zastaralá data tak, že smažete se položky v mezipaměti při změně dat prostřednictvím stránky. Je však možné ObjectDataSource stále zobrazení zastaralých dat pomocí ukládání do mezipaměti. V nejjednodušším případě může být z důvodu data změna přímo v databázi. Správce databáze možná právě spustili skript, který změní některé záznamy v databázi.

Tento scénář může také unfold více jemně způsobem. Když ObjectDataSource vyloučí jeho položky z mezipaměti, pokud jeden z jeho metod úpravy dat se nazývá, položek v mezipaměti odebrat jsou pro ObjectDataSource s určitou kombinaci hodnot vlastností (`CacheDuration`, `TypeName`, `SelectMethod`, a tak dále). Pokud máte dva ObjectDataSources, které používají různé `SelectMethods` nebo `SelectParameters`, ale stále můžete aktualizovat stejná data, pak může aktualizovat řádek a zneplatnit vlastní položky mezipaměti, ale odpovídající řádek pro druhý ObjectDataSource jeden ObjectDataSource se stále zpracuje z uložená v mezipaměti. I doporučujeme vytvářet stránky vykazovat tuto funkci. Vytvořte stránku, která zobrazuje upravitelný GridView, který žádá o jeho data ze ObjectDataSource, který používá ukládání do mezipaměti a je nakonfigurován k získání dat z `ProductsBLL` třídu s `GetProducts()` metoda. Přidejte další upravitelné GridView a ObjectDataSource na této stránce (nebo jinou), ale pro tento druhý ObjectDataSource mají ho používat `GetProductsByCategoryID(categoryID)` metoda. Od dvě ObjectDataSources `SelectMethod` vlastnosti liší, budou všechny každý mají své vlastní uložené v mezipaměti hodnoty. Pokud chcete upravit produktu v jedné mřížky, při příštím vytvoření vazby dat zpět do jiných mřížky (podle stránkování, řazení a tak dále), bude i nadále odesílat data starý, uložené v mezipaměti a změny, která byla vytvořená z jiných mřížky v úvahu.

Stručně řečeno použijte pouze založené na čase expiries Pokud chcete mít potenciálně zastaralá data a použijte kratší expiries pro scénáře, kde je důležité aktuálnosti dat. Pokud zastaralá data není přijatelné, buď forgo ukládání do mezipaměti, nebo použijte závislosti mezipaměti SQL (za předpokladu, že je databáze data níž ukládání do mezipaměti). V budoucích kurzu jsme budete prozkoumejte závislosti mezipaměti SQL.

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili ObjectDataSource s integrované ukládání do mezipaměti funkce. Nastavením jednoduše několik vlastností, jsme určit, aby ObjectDataSource pro ukládání do mezipaměti výsledky vrácené z určeného `SelectMethod` do mezipaměti data technologie ASP.NET. `CacheDuration` a `CacheExpirationPolicy` vlastnosti určují dobu trvání položka se uloží do mezipaměti a zda je absolutní nebo klouzavé vypršení platnosti. `CacheKeyDependency` Vlastnost všechny položky mezipaměti s ObjectDataSource Přidruží existující závislost mezipaměti. Tímto lze vyřazení ObjectDataSource s položky z mezipaměti před vypršením platnosti na základě času je dosaženo a se obvykle používá s závislosti mezipaměti SQL.

Vzhledem k tomu, že ObjectDataSource jednoduše ukládá do mezipaměti jeho hodnoty do mezipaměti dat, můžeme může replikovat integrovanou funkci s ObjectDataSource prostřednictvím kódu programu. Ho t nemá smysl to udělat na prezentační vrstvy, vzhledem k tomu, že ObjectDataSource nabízí tuto funkci mimo pole, ale můžeme implementovat možnosti ukládání do mezipaměti v samostatné vrstvě architektury. To pokud chcete udělat, budeme potřebovat opakování stejné logiky, která používá ObjectDataSource. Jak programově pracovat s datovou mezipaměť z v architektuře v našem kurzu další jsme budete prozkoumat.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Ukládání do mezipaměti ASP.NET: Technik a nejlepších postupů](https://msdn.microsoft.com/en-us/library/aa478965.aspx)
- [Ukládání do mezipaměti Průvodce architekturou pro aplikace .NET Framework](https://msdn.microsoft.com/en-us/library/ee817645.aspx)
- [Výstupní mezipaměti technologie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](using-sql-cache-dependencies-cs.md)
[další](caching-data-in-the-architecture-vb.md)
