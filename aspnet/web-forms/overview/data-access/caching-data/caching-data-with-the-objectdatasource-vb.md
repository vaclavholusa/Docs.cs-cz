---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: Ukládání do mezipaměti dat ovládacím prvkem ObjectDataSource (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Ukládání do mezipaměti může znamenat, že rozdíl mezi stránky pomalá a rychlé webové aplikace. Tento kurz je první ze čtyř, které využívají podrobný pohled na ukládání do mezipaměti v technologii ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: baa6fd0c290c0b09cf137f12ce62f50bae52be23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756066"
---
<a name="caching-data-with-the-objectdatasource-vb"></a>Ukládání dat do mezipaměti ovládacím prvkem ObjectDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) nebo [stahovat PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> Ukládání do mezipaměti může znamenat, že rozdíl mezi stránky pomalá a rychlé webové aplikace. Tento kurz je první ze čtyř, které využívají podrobný pohled na ukládání do mezipaměti v technologii ASP.NET. Přečtěte si klíčové koncepty ukládání do mezipaměti a tom, jak používat ukládání do mezipaměti do prezentační vrstvy do ovládacího prvku ObjectDataSource.


## <a name="introduction"></a>Úvod

V počítačových věd *ukládání do mezipaměti* je proces trvá data nebo informace, které je náročné získat a ukládání do umístění, které je rychlejší přístup k jeho kopii. Pro datově řízené aplikace velkých a složitých dotazů běžně využívají většinu času spuštění aplikace s. Tyto s výkon aplikace, pak můžete často vylepšené uložením výsledků dotazů nákladné databáze v paměti s aplikací.

Technologie ASP.NET 2.0 nabízí širokou škálu možností ukládání do mezipaměti. Celou webovou stránku nebo uživatelský ovládací prvek s vykreslení značek můžete uložit do mezipaměti, prostřednictvím *ukládání výstupu do mezipaměti*. Ovládací prvek ObjectDataSource a SqlDataSource umožňují ukládání do mezipaměti i, a tím umožní data uložit do mezipaměti na úrovni ovládacího prvku. A ASP.NET s *přístupů do datové mezipaměti* poskytuje bohaté možnosti ukládání do mezipaměti rozhraní API, která umožňuje vývojářům stránky objektů mezipaměti prostřednictvím kódu programu. V tomto kurzu a další tři, kterou prozkoumáme pomocí prvku ObjectDataSource s ukládání do mezipaměti funkce, jakož i datové mezipaměti. Také se podíváme do mezipaměti ukládat data o celé aplikace při spuštění a zachovat data uložená v mezipaměti čerstvé prostřednictvím použití závislostí mezipaměti SQL. Tyto kurzy není prozkoumat ukládání výstupu do mezipaměti. Podrobný pohled na ukládání výstupu do mezipaměti, naleznete v tématu [ukládání výstupu do mezipaměti v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Ukládání do mezipaměti se dá použít na libovolné místo v architektuře z vrstvy přístupu k datům se prostřednictvím prezentační vrstvy. V tomto kurzu podíváme na použití ukládání do mezipaměti do prezentační vrstvy do ovládacího prvku ObjectDataSource. V dalším kurzu, kterou prozkoumáme ukládání do mezipaměti dat na vrstvy obchodní logiky.

## <a name="key-caching-concepts"></a>Klíč ukládání do mezipaměti koncepty

Ukládání do mezipaměti může výrazně zlepšit aplikace s celkový výkon a škálovatelnost na základě data, která je nákladná ke generování a ukládání kopie se v umístění, který je přístupný efektivněji. Vzhledem k tomu, že mezipaměť obsahuje pouze kopie skutečné, podkladová data, můžete začnou být zastaralé, nebo *zastaralé*, pokud se změní podkladová data. Z toho důvodu může vývojář můžete určit kritéria, podle kterých bude položka mezipaměti *vyřazení* z mezipaměti, buď pomocí:

- **Kritéria založeného na čase** položky se můžou přidat do mezipaměti absolutní nebo klouzavé doby trvání. Například může vývojář může znamenat dobu trvání, třeba 60 sekund. S dobou trvání absolutní položku z mezipaměti dojde k jejich vyřazení 60 sekund poté, co byl přidán do mezipaměti bez ohledu na to, jak často se použila. S určitou dobou trvání posuvného položku z mezipaměti dojde k jejich vyřazení 60 sekund, po posledním přístupu.
- **Na základě závislostí kritéria** závislost může být přidružena k položce, když se přidá do mezipaměti. Při změně položky s závislostí je odstraněn z mezipaměti. Závislost může být soubor, jiné položce v mezipaměti nebo kombinaci obojího. ASP.NET 2.0 umožňuje také závislosti mezipaměti SQL, které umožňují vývojářům přidat položku do mezipaměti a jeho vyřazení, když se změní podkladová data databáze. Prozkoumáme závislosti mezipaměti SQL v nadcházející [použití závislostí mezipaměti SQL](using-sql-cache-dependencies-vb.md) kurzu.

Bez ohledu na to, na zadaných kritériích vyřazení, může být položka v mezipaměti *úklid* předtím, než byla splněna kritéria založeného na čase nebo na základě závislostí. Pokud mezipaměti dosáhne své kapacity, musíte před přidáním nové odebrat existující položky. V důsledku toho při programově práci s data uložená v mezipaměti je s důležité vždy předpokládat, že nemusí být data uložená v mezipaměti k dispozici. Vzor pro použití při přístupu k datům z mezipaměti prostřednictvím kódu programu v našem dalším kurzu se podíváme *ukládání dat do mezipaměti v architektuře*.

Ukládání do mezipaměti poskytuje ekonomičtější způsob stlačení výkonnější z aplikace. Jako [Steven Smith](http://aspadvice.com/blogs/ssmith/) zabývá se výhodami ve svém článku [ukládání do mezipaměti ASP.NET: technik a nejlepších postupů](https://msdn.microsoft.com/library/aa478965.aspx):

Ukládání do mezipaměti může být dobrým způsobem, jak získat dobrou dostatečný výkon bez nutnosti spoustu času a analýzy. Paměť je levné, takže pokud obdržíte výkonu potřebujete díky ukládání do mezipaměti výstup po dobu 30 sekund nemusíte trávit denně nebo týdně pokusu o optimalizaci kódu nebo databáze, proveďte řešení ukládání do mezipaměti (30 – stará druhé data za předpokladu, že je v pořádku) a přejít. Nakonec nekvalitním návrhem bude pravděpodobně dohnat, takže kurzu byste měli navrhnout aplikace správně. Pokud potřebujete získat dobrou dnes dostatečný výkon, ukládání do mezipaměti jde vynikající [přístup], ale nákup čas k refaktorování aplikace později, pokud máte čas Uděláte to tak.

Při ukládání do mezipaměti může poskytovat výrazné vylepšení, není možné ho ve všech situacích, třeba s aplikacemi, které používají v reálném čase, často aktualizuje data, nebo kde zastaralá data i za chvíli žil klesne na nepřijatelnou. Ale pro většinu aplikací, ukládání do mezipaměti je třeba použít. Další informace o ukládání do mezipaměti v technologii ASP.NET 2.0, najdete [ukládání do mezipaměti pro výkon](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) část [kurzy rychlý start ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Krok 1: Vytvoření ukládání do mezipaměti webové stránky

Než začneme naše zkoumání funkcí ObjectDataSource s ukládání do mezipaměti, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, který budeme potřebovat pro tento kurz a další tři. Začněte přidáním novou složku s názvem `Caching`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Přidání stránky technologie ASP.NET pro kurzy týkající se ukládání do mezipaměti](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Obrázek 1**: Přidání stránky technologie ASP.NET pro kurzy týkající se ukládání do mezipaměti


V jiných složkách, jako jsou `Default.aspx` v `Caching` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Obrázek 2: Přidáte na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Obrázek 2**: obrázek 2: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image4.png))


A konečně, přidejte tyto stránky jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za práce s binárními daty `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro ukládání do mezipaměti kurzy.


![Mapa webu nyní obsahuje záznamy pro ukládání do mezipaměti kurzy](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Obrázek 3**: mapy webu nyní obsahuje záznamy pro ukládání do mezipaměti kurzy


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Krok 2: Zobrazení seznamu produktů, které na webové stránce

Tento kurz se věnuje použití prvku ObjectDataSource funkcí ovládacího prvku s integrovanou ukládání do mezipaměti. Předtím, než abychom se mohli podívat na tyto funkce, ale nejprve potřebujeme stránku, kterou chcete pracovat. Umožňují s vytvořit webovou stránku, která používá GridView načíst ObjectDataSource z seznam informací o produktech `ProductsBLL` třídy.

Začněte otevřením `ObjectDataSource.aspx` stránku `Caching` složky. Přetáhněte z panelu nástrojů na Návrhář GridView, nastavte jeho `ID` vlastnost `Products`a z inteligentních značek, vyberte a vytvořte jeho vazbu nového ovládacího prvku ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource pracovat `ProductsBLL` třídy.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLL](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Obrázek 4**: Konfigurace ObjectDataSource k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image8.png))


Pro tuto stránku umožní s vytvořit upravitelné GridView tak, aby nám můžete zkoumat, co se stane při změně dat v mezipaměti v ObjectDataSource prostřednictvím rozhraní s ovládacího prvku GridView. Ponechejte rozevíracím seznamu na kartě vyberte nastaví výchozí hodnoty, `GetProducts()`, ale změnit vybrané položky na kartě aktualizace `UpdateProduct` přetížení přijímající `productName`, `unitPrice`, a `productID` jako jeho vstupní parametry.


[![Nastavte aktualizace kartu s rozevíracím seznamu příslušné UpdateProduct přetížení](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Obrázek 5**: kartu aktualizace s rozevíracím seznamu možnost vhodná `UpdateProduct` přetížení ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image11.png))


Nakonec nastavte rozevírací seznamy na kartách INSERT a DELETE na (žádný) a klikněte na tlačítko Dokončit. Po dokončení průvodce bude konfigurace zdroje dat, aplikace Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}`. Jak je popsáno v [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) kurz, tato vlastnost musí odebrat z deklarativní syntaxe nebo se nastaví zpátky na výchozí hodnotu, `{0}`, aby naše pracovního postupu aktualizace Pokračujte bez chyb.

Kromě toho po dokončení Průvodce vytvořením sady Visual Studio přidá pole do prvku GridView. pro každé pole data produktu. Odeberte všechny kromě na `ProductName`, `CategoryName`, a `UnitPrice` BoundFields. Dále, aktualizujte `HeaderText` vlastnosti každé z těchto BoundFields na produkt, kategorie a ceny, v uvedeném pořadí. Vzhledem k tomu, `ProductName` pole je povinné, převést na pole TemplateField Vlastnost BoundField a přidat RequiredFieldValidator k `EditItemTemplate`. Podobně, převést `UnitPrice` Vlastnost BoundField na pole TemplateField a přidejte CompareValidator tak, aby byl uživatelem zadaná hodnota platná měny hodnota této s větší než nebo rovna hodnotě nula. Kromě těchto změn, nebojte se provádět změny aesthetic, jako je například zarovnání vpravo `UnitPrice` hodnoty nebo určení formátování `UnitPrice` textu v jeho rozhraní jen pro čtení a úpravy.

Zkontrolujte prvku GridView upravitelné zaškrtnutím políčka Povolit úpravy v prvku GridView s inteligentním. Také zaškrtněte zaškrtávací políčka Povolit stránkování a Povolit řazení.

> [!NOTE]
> Je potřeba zkontrolovat, jak přizpůsobit rozhraní GridView s úpravy? Pokud ano, vraťte se do [přizpůsobení rozhraní pro úpravu dat](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) kurzu.


[![Povolte podporu GridView pro úpravy, řazení a stránkování](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Obrázek 6**: Povolení podpory GridView pro úpravy, řazení a stránkování ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image14.png))


Po provedení těchto změn ovládacího prvku GridView, ovládacími prvky GridView a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Jak je vidět na obrázku 7, upravitelné GridView uvádí název, kategorie a cen jednotlivých produktů v databázi. Využít k otestování funkce řazení stránky s výsledky stránkovat a pokud chcete záznam upravit.


[![Každý produkt s názvem, kategorie a cena je uveden v Sortable, Pageable, upravitelné GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Obrázek 7**: každý produkt s názvem, kategorie a cena je uveden v Sortable, Pageable, upravitelné ovládacího prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Krok 3: Posouzení, když prvku ObjectDataSource jsou žádosti o Data

`Products` GridView načte data pro zobrazení vyvoláním `Select` metodu `ProductsDataSource` ObjectDataSource. Tento prvek ObjectDataSource vytvoří instanci s vrstvy obchodní logiky `ProductsBLL` třídy a volání jeho `GetProducts()` metodu, která volá vrstvy přístupu k datům s `ProductsTableAdapter` s `GetProducts()` metody. Metoda DAL se připojí k databázi Northwind a vydá nakonfigurované `SELECT` dotazu. Tato data je pak vrácen do vrstvy DAL, která zabalí v `NorthwindDataTable`. Objekt DataTable je vrácen do knihoven BLL, které vrací pro prvek ObjectDataSource, které vrací do prvku GridView. Vytvoří prvku GridView `GridViewRow` objekt pro každou `DataRow` v objektu DataTable a pro každou `GridViewRow` nakonec vykreslením do kódu HTML, který je vrácen do klienta a zobrazí v prohlížeči návštěvník s.

Toto pořadí událostí dojde každém prvku GridView. musí se vytvořit vazbu na podkladová data. Který se stane, když je nejprve navštívené stránky, při přesunu z jedné stránky dat do jiného, při řazení prvku GridView, nebo při změně dat s GridView prostřednictvím integrované úpravy nebo odstranění rozhraní. Pokud je stav zobrazení ovládacího prvku GridView s zakázán, prvku GridView bude odrážejí na každého zpětného odeslání. Prvku GridView. může také být explicitně znovu připojeno k jeho datům voláním jeho `DataBind()` metoda.

Plně vyhodnotit četnost, se kterým je načítání dat z databáze, umožní s zobrazit zpráva, když se znovu načíst data. Přidat ovládací prvek popisek webového nad prvek GridView s názvem `ODSEvents`. Vymazání jeho `Text` vlastnost a nastavte jeho `EnableViewState` vlastnost `False`. Pod popisek, přidejte ovládací prvek webového tlačítko a nastavte jeho `Text` vlastnost zpětného odeslání.


[![Přidejte tlačítko a popisek na stránku nad prvku GridView.](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Obrázek 8**: přidejte popisek a tlačítka do stránky výše prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image20.png))


Během data access, ObjectDataSource s `Selecting` událost je aktivována před vytvořením základní objekt a jeho nakonfigurovaná metoda vyvolána. Vytvořte obslužnou rutinu události pro tuto událost a přidejte následující kód:


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

Pokaždé, když prvku ObjectDataSource učiní žádost vůči architektury pro data, popisek se zobrazí události Selecting text, který se aktivuje.

Navštivte tuto stránku v prohlížeči. Při první návštěvě stránky, aktivuje událost výběr textu se zobrazí. Klikněte na tlačítko zpětného volání a Všimněte si, že text zmizí (za předpokladu, že prvek GridView s `EnableViewState` je nastavena na `True`, výchozí hodnota). Důvodem je, že na zpětné volání, prvku GridView je znovu vytvořena z svůj stav zobrazení a proto zapnout t kódu pro prvek ObjectDataSource pro svá data. Řazení, stránkování a úpravy dat, ale způsobí, že GridView znovu připojit ke zdroji dat, a proto výběr událost aktivuje se bude zobrazovat text.


[![Pokaždé, když prvku GridView je znovu připojeno ke zdroji dat, zobrazí se události Selecting aktivováno](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Obrázek 9**: pokaždé, když prvku GridView je znovu připojeno ke zdroji dat, zobrazí se aktivuje události Selecting ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![Kliknutím na tlačítko způsobí zpětné odeslání prvku GridView. Chcete-li být znovu vytvořena z svůj stav zobrazení](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Obrázek 10**: Kliknutím na tlačítko Postback způsobí, že prvku GridView. Chcete-li být znovu vytvořena z svůj stav zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image26.png))


To může zdát plýtvání, aby se načetla data databáze pokaždé, když je stránkování prostřednictvím nebo seřazená data. Po všech od jsme znovu pomocí výchozího stránkování prvku ObjectDataSource načte všechny záznamy při zobrazení na první stránce. I v případě, že prvku GridView neposkytuje řazení a stránkování podpory, data musí být načíst z databáze pokaždé, když na stránce první návštěvě libovolným uživatelem (a při každém postbacku, pokud je stav zobrazení je zakázané). Ale pokud prvku GridView se zobrazuje na stejná data pro všechny uživatele, jsou tyto žádosti navíc databáze nadbytečný. Případně proč bezpečná není výsledky vrácené z mezipaměti `GetProducts()` metoda a vazby prvku GridView. ty výsledky do mezipaměti?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Krok 4: Data s využitím ObjectDataSource ukládání do mezipaměti.

Nastavením jednoduše několik vlastností, lze nastavit prvku ObjectDataSource pro ukládání do mezipaměti automaticky načtena data v datové mezipaměti technologie ASP.NET. Následující seznam shrnuje vlastnosti související s mezipamětí ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) musí být nastaveno na `True` povolení ukládání do mezipaměti. Výchozí hodnota je `False`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) množství času v sekundách, který je uložen v mezipaměti. Výchozí hodnota je 0. Prvku ObjectDataSource se pouze data do mezipaměti pokud `EnableCaching` je `True` a `CacheDuration` je nastavena na hodnotu větší než nula.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) může být nastaven na `Absolute` nebo `Sliding`. Pokud `Absolute`, ObjectDataSource ukládá do mezipaměti načíst data pro `CacheDuration` sekund; Pokud `Sliding`, vyprší platnost dat až po nepřistupovalo pro `CacheDuration` sekund. Výchozí hodnota je `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) pomocí této vlastnosti lze přidružit položky mezipaměti s ObjectDataSource existujícího závislosti mezipaměti. Prvek ObjectDataSource s datové položky můžete předčasně vyřazení z mezipaměti zneplatněním jeho přidruženého `CacheKeyDependency`. Tato vlastnost se nejčastěji používá pro přidružení k mezipaměti s ObjectDataSource závislosti mezipaměti SQL, téma prozkoumáme v budoucnu [použití závislostí mezipaměti SQL](using-sql-cache-dependencies-vb.md) kurzu.

Umožní s nakonfigurovat `ProductsDataSource` ObjectDataSource pro ukládání do mezipaměti svá data po dobu 30 sekund na absolutní měřítko. Nastavit prvek ObjectDataSource s `EnableCaching` vlastnost `True` a jeho `CacheDuration` vlastnost do 30. Nechte `CacheExpirationPolicy` nastavenou na výchozí `Absolute`.


[![Konfigurace ObjectDataSource pro ukládání do mezipaměti svá Data po dobu 30 sekund](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Obrázek 11**: Konfigurace ObjectDataSource pro ukládání do mezipaměti svá Data po dobu 30 sekund ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-with-the-objectdatasource-vb/_static/image29.png))


Uložte změny a návštěvě této stránky v prohlížeči. Výběr textu události vyvolané zobrazí při první návštěvě stránky tak, že počáteční data se nenachází v mezipaměti. Ale následné postbacky spustit kliknutím na Postback tlačítko řazení, stránkování, nebo kliknutím na tlačítko Upravit nebo zrušit *není* redisplay události Selecting aktivuje text. Důvodem je, že `Selecting` události pouze aktivuje se v případě ObjectDataSource získává data od jeho základní objekt; `Selecting` událost se neaktivuje, pokud data se načítají z mezipaměti data.

Po 30 sekund bude vyloučena data z mezipaměti. Data budou odstraněna také z mezipaměti, pokud prvek ObjectDataSource s `Insert`, `Update`, nebo `Delete` jsou metody vyvolány. V důsledku toho zobrazení události Selecting textu aktivuje po 30 sekundách prošly nebo na tlačítko Aktualizovat se kliklo, řazení, stránkování, nebo kliknutím na tlačítko Upravit nebo zrušit způsobí, že ObjectDataSource pro získání dat z jeho základní objekt, když `Selecting` dojde k aktivaci události. Vrácené výsledky jsou umístěny do datové mezipaměti.

> [!NOTE]
> Pokud se zobrazí výběr textu aktivuje událost často, i když očekáváte, že ObjectDataSource pracovat se službou data uložená v mezipaměti, může být z důvodu omezení paměti. Pokud není k dispozici dostatek volné paměti, data přidaná do mezipaměti podle ObjectDataSource může mít byla úklid. Pokud se zdá být správně ukládání do mezipaměti dat nebo jenom ty mezipaměti t kódu prvku ObjectDataSource data nedojde, zavřete některé aplikace volné paměti a akci opakujte.


Obrázek 12 znázorňuje ObjectDataSource s ukládání do mezipaměti pracovního postupu. Při vyvolání události Selecting text se zobrazí na obrazovce, protože data nebyla v mezipaměti a musel načíst z podkladových objektů. Když tento text nebyl nalezen, ale jeho s vzhledem k tomu, že data byla k dispozici z mezipaměti. Pokud je vrácená data z mezipaměti tam s žádná volání na základní objekt a proto žádný databázový dotaz spustit.


![ObjectDataSource ukládá a načítá Data z mezipaměti dat](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Obrázek 12**: ObjectDataSource ukládá a načítá Data z mezipaměti dat


Každá aplikace technologie ASP.NET má své vlastní datové mezipaměti instance této s sdílené ve všech stránek a návštěvníků. To znamená, že data uložená v mezipaměti dat ObjectDataSource podobně sdílí všichni uživatelé, kteří navštíví stránku. Chcete-li to ověřit, otevřete `ObjectDataSource.aspx` stránku v prohlížeči. Při první návštěvě stránky, výběr události vyvolané textu se zobrazí (za předpokladu, že data byla přidána do mezipaměti podle předchozích testů, nyní byla odebrána). Otevřete druhou instanci prohlížeče a zkopírujte a vložte adresu URL z první instance prohlížeče na druhý. Ve druhé instanci prohlížeče, výběr události vyvolané textu neuvádíme, protože ho používají stejná data jako první v mezipaměti.

Při vkládání jeho načtená data do mezipaměti, ObjectDataSource používá hodnotu klíče mezipaměti, která zahrnuje: `CacheDuration` a `CacheExpirationPolicy` hodnoty vlastností, typ základní obchodní objekt používá v prvku ObjectDataSource, který je zadán prostřednictvím [ `TypeName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, v tomto příkladu); hodnota `SelectMethod` vlastnost a názvu a hodnoty parametrů v `SelectParameters` shromažďování; a hodnoty jeho `StartRowIndex`a `MaximumRows` vlastnosti, které se používají při implementaci [vlastní stránkování](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

Položka jedinečný mezipaměti vytváření hodnotu klíče mezipaměti jako kombinace těchto vlastností zajišťuje, jak tyto hodnoty změnit. Například v posledních kurzy jsme ve podívali se na používání `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)`, který vrátí všechny produkty pro zadané kategorie. Jeden uživatel by mohl pocházet na nápoje stránky a zobrazení, která má `CategoryID` 1. Pokud ObjectDataSource uložené v mezipaměti bez ohledu na její výsledky `SelectParameters` hodnoty, když jiný uživatel přišel na stránku Chcete-li zobrazit produkty koření při nápoje produkty byly v mezipaměti, d zobrazí produkty v mezipaměti nápoje místo produkty koření. Změnou klíče mezipaměti podle těchto vlastností, které obsahují hodnoty `SelectParameters`, ObjectDataSource udržuje samostatné položky v mezipaměti pro nápoje a produkty koření.

## <a name="stale-data-concerns"></a>Aspekty zastaralých dat

Prvku ObjectDataSource automaticky vyloučí položky z mezipaměti, pokud jeden z jeho `Insert`, `Update`, nebo `Delete` vyvolání metody. To pomáhá chránit proti zastaralých dat tím, že zrušíte, položky v mezipaměti při změně dat prostřednictvím stránky. Je však možné prvku ObjectDataSource stále zobrazení zastaralých dat pomocí ukládání do mezipaměti. V nejjednodušším případě může být z důvodu data změny přímo v databázi. Správce databáze pravděpodobně právě spustili skript, který upravuje některé záznamy v databázi.

Tento scénář může také rozbalit jemnější způsobem. ObjectDataSource vyloučí položky z mezipaměti, když jedna z jeho metod změny dat se nazývá, položek v mezipaměti odebrat jsou pro prvek ObjectDataSource s určitou kombinaci hodnot vlastností (`CacheDuration`, `TypeName`, `SelectMethod`, a tak dále). Pokud máte dva ObjectDataSources, které používají různé `SelectMethods` nebo `SelectParameters`, ale stále můžete aktualizovat stejná data, může aktualizovat řádek a zneplatnit její vlastní položky mezipaměti, ale odpovídající řádek pro druhý prvek ObjectDataSource jednoho prvku ObjectDataSource se dají obsluhovat z mezipaměti. Neváhejte se vytvářet stránky je tuto funkci. Vytvořit stránku, která zobrazuje upravitelný prvku GridView, který si vyžádá data z ObjectDataSource, který používá ukládání do mezipaměti a je nakonfigurován k získání dat z `ProductsBLL` třída s `GetProducts()` metody. Přidejte další upravitelné GridView a ObjectDataSource na této stránce (nebo jinému), ale pro tento druhý prvek ObjectDataSource mají ji použít `GetProductsByCategoryID(categoryID)` metody. Od dvou ObjectDataSources `SelectMethod` vlastnosti liší, se bude každý mají svoje vlastní hodnoty uložené v mezipaměti. Pokud upravíte produktu v jedné mřížce, při příštím svázat data zpět do jiné tabulky (podle stránkování, řazení a tak dále), bude dál obsluhovat data staré, uložená v mezipaměti a neodráží změny, která byla vytvořená z jiné tabulky.

Stručně řečeno použijte pouze podle času expiries Pokud jste ochotni mohou mít potenciálně zastaralých dat a použijte kratší expiries pro scénáře, kde aktuálnosti dat je důležité. Pokud zastaralých dat není přijatelná, buď forgo ukládání do mezipaměti, nebo použijte závislosti mezipaměti SQL (za předpokladu, že je databáze data níž ukládání do mezipaměti). V budoucích kurzech se podíváme závislosti mezipaměti SQL.

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na prvku ObjectDataSource s integrovanými ukládání do mezipaměti možnostmi. Nastavením jednoduše několik vlastností, nám můžete dát pokyn ObjectDataSource pro ukládání do mezipaměti na výsledky vrácené ze zadaného `SelectMethod` do datové mezipaměti technologie ASP.NET. `CacheDuration` a `CacheExpirationPolicy` vlastnosti určují dobu trvání položka se uloží do mezipaměti a zda je absolutní nebo klouzavé vypršení platnosti. `CacheKeyDependency` Vlastnost všechny položky v prvku ObjectDataSource s mezipaměti přidruží existujícího závislosti mezipaměti. To umožňuje vyřadit položky v prvku ObjectDataSource s mezipaměti před koncem platnosti podle času je dosaženo a obvykle se používá s závislosti mezipaměti SQL.

Protože ObjectDataSource jednoduše ukládá do mezipaměti hodnoty do datové mezipaměti, jsme prvku ObjectDataSource s vestavěnou funkci replikovat prostřednictvím kódu programu. To t nemá smysl to provést v prezentační vrstvě, protože ObjectDataSource nabízí tyto funkce úprav, ale můžeme implementovat možnosti ukládání do mezipaměti v samostatné vrstvy architektury. Uděláte to tak, potřebujeme ObjectDataSource používá stejnou logiku opakování. Podíváme, jak prostřednictvím kódu programu pracovat s datové mezipaměti v architektuře v následujícím kurzem.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Ukládání do mezipaměti ASP.NET: Techniky a osvědčené postupy](https://msdn.microsoft.com/library/aa478965.aspx)
- [Ukládání do mezipaměti Průvodce architekturou aplikací .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Ukládání výstupu do mezipaměti v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](using-sql-cache-dependencies-cs.md)
> [další](caching-data-in-the-architecture-vb.md)
