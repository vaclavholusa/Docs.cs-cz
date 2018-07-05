---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Vytváření vlastního databázově řízeného zprostředkovatele mapy webu (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Výchozího zprostředkovatele mapy webu v technologii ASP.NET 2.0 načte data ze statického souboru XML. Je vhodné pro mnoho malé a střední okno velikosti zprostředkovatele založený na formátu XML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e041a5a9163c7f9fe55c6aa06f35301cbdb48a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393967"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Vytváření vlastního databázově řízeného zprostředkovatele mapy webu (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) nebo [stahovat PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Výchozího zprostředkovatele mapy webu v technologii ASP.NET 2.0 načte data ze statického souboru XML. Zatímco zprostředkovatele založený na formátu XML je vhodný pro mnoho malé a středně velké webové stránky, větší webových aplikací vyžaduje více dynamická Mapa webu. V tomto kurzu vytvoříme vlastního zprostředkovatele mapy webu, který načte data z vrstvy obchodní logiky, která načte data z databáze.


## <a name="introduction"></a>Úvod

ASP.NET 2.0 s funkce mapy webu umožňuje vývojářům definovat mapy webu webové aplikace s některé trvalou média, jako například v souboru XML. Po definování data mapy webu je přístupná prostřednictvím kódu programu přes [ `SiteMap` třídy](https://msdn.microsoft.com/library/system.web.sitemap.aspx) v [ `System.Web` obor názvů](https://msdn.microsoft.com/library/system.web.aspx) nebo prostřednictvím různých navigace webové ovládací prvky, například Ovládací prvky SiteMapPath, nabídky a prvku TreeView. Mapa systém lokality používá [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) tak, aby implementace serializace jiné lokalitě mapy se dají vytvářet a zapojí se do webové aplikace. Výchozího zprostředkovatele mapy webu, která je dodávána s prostředím ASP.NET 2.0 nevyřeší struktury mapy webu v souboru XML. Zpátky [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-vb.md) kurzu jsme vytvořili soubor s názvem `Web.sitemap` , která obsahovala tuto strukturu a se aktualizuje její XML s každé nové části kurzu.

Zprostředkovatele mapy webu založený na formátu XML výchozí dobře fungují v případě strukturu s mapy webu je poměrně statická, například pro tyto kurzy. V mnoha případech ale více dynamická Mapa webu je potřeba. Vezměte v úvahu mapy webu je znázorněno na obrázku 1, kde každé kategorie a produkt zobrazí jako oddíly ve struktuře s webu. Pomocí této mapy webu navštívit webovou stránku odpovídající kořenový uzel může být seznam všech kategorií, že určité kategorie s webové stránce by zobrazila seznam této kategorie s produkty a zobrazení webové stránky s konkrétním produktu by zobrazil produktu s podrobnosti.


[![Kategorie a produkty strukturu mapu s struktury webu](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Obrázek 1**: The kategorie a strukturu produktů mapy webu s strukturu ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


I když tato struktura podle kategorie a produktů může být pevně zakódovaný do `Web.sitemap` souboru, soubor bude potřeba aktualizovat pokaždé, když kategorie nebo produkt byl přidat, odebrat nebo přejmenovat. V důsledku toho mapy Údržba webového serveru by se dá výrazně zjednodušit. Pokud své struktury byla získána z databáze, nebo v ideálním případě z vrstvy obchodní logiky architektury s aplikací. Tímto způsobem, jak byly přidány produktů a kategorie, přejmenována nebo odstraněna, mapy webu bude automaticky aktualizovat tak, aby odrážela tyto změny.

Protože serializace mapování lokality s ASP.NET 2.0 je vytvořeným na základě podle modelu poskytovatele, můžeme vytvořit vlastní vlastního zprostředkovatele mapy webu, který získá data z nebo alternativní datové úložiště, jako jsou databáze nebo architekturu. V tomto kurzu vytvoříme vlastní poskytovatele, který načte data z BLL. Začínáme s let!

> [!NOTE]
> Vlastního zprostředkovatele mapy webu v tomto kurzu vytvořili je těsně spjat s aplikace s architekturou a datový model. Jeff Prosise s [ukládání mapy webu v systému SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) a [zprostředkovatele mapy webu SQL jste již bylo čekání](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) články prozkoumat zobecněný přístup k ukládání dat mapy webu v systému SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Krok 1: Vytvoření vlastní stránky mapy poskytovatele webových stránek

Než začneme vytvořením vlastního zprostředkovatele mapy webu, umožní s nejprve přidat stránek ASP.NET, kterou potřebujeme pro účely tohoto kurzu. Začněte přidáním novou složku s názvem `SiteMapProvider`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Také přidat `CustomProviders` podsložku `App_Code` složky.


![Přidání stránky technologie ASP.NET pro kurzy související zprostředkovatele mapy webu](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Obrázek 2**: Přidání stránky ASP.NET pro web mapování kurzy týkající se poskytovatele


Vzhledem k tomu, že existuje pouze jeden kurz v této části, budeme zadávat nepotřebujete `Default.aspx` seznam v části s kurzech. Místo toho `Default.aspx` kategorií se zobrazí v ovládacím prvku GridView. To jsme budete řešit v kroku 2.

Dále, aktualizujte `Web.sitemap` zahrnout odkaz na `Default.aspx` stránky. Konkrétně, přidejte následující kód za ukládání do mezipaměti `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně nyní obsahuje položku pro tento kurz zprostředkovatele mapy jediný server.


![Mapa webu nyní obsahuje záznam pro tento kurz zprostředkovatele mapy webu](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje záznam pro tento kurz zprostředkovatele mapy webu


Tento kurz s hlavní fokus je pro ilustraci vlastního zprostředkovatele mapy webu vytváření a konfigurace webové aplikace k používání tohoto zprostředkovatele. Zejména vytvoříme poskytovatele, který vrátí mapy webu, který obsahuje kořenový uzel společně s uzlem pro každou kategorii a produkt, jak je znázorněno na obrázku 1. Obecně platí může každý uzel v mapě webu zadejte adresu URL. Pro naše Mapa webu, bude mít adresu URL uzlu s kořenové `~/SiteMapProvider/Default.aspx`, který zobrazí seznam všech kategorií v databázi. Každý uzel kategorie v mapě webu bude mít adresu URL, která odkazuje na `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, který se zobrazí všechny produkty v zadaném *categoryID*. A konečně, bude každý uzel mapy webu produktu přejděte na `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, který se zobrazí podrobnosti o konkrétním produktu s.

Chcete-li spustit potřebujeme vytvořit `Default.aspx`, `ProductsByCategory.aspx`, a `ProductDetails.aspx` stránky. Tyto stránky se provádí v kroku 2, 3 a 4, v uvedeném pořadí. Protože těžiště tohoto kurzu je na poskytovatelé map webu, a od poslední kurzy pokryli vytváření sestav se tyto druhy vícestránkového záznamů master/detail jsme se hurry prostřednictvím kroky 2 až 4. Pokud potřebujete občerstvit o vytváření záznamů master/detail sestav, které přesahují délku jedné stránky, vraťte se do [filtrování záznamů Master/Detail napříč dvěma stránkami](../masterdetail/master-detail-filtering-across-two-pages-vb.md) kurzu.

## <a name="step-2-displaying-a-list-of-categories"></a>Krok 2: Zobrazení seznamu kategorií

Otevřít `Default.aspx` stránku `SiteMapProvider` složky a GridView přetáhněte z panelu nástrojů do Návrháře nastavení jeho `ID` k `Categories`. Z inteligentních značek GridView s vázat na nového prvku ObjectDataSource s názvem `CategoriesDataSource` a nakonfigurujte ho tak, aby jej obnoví svoje data pomocí `CategoriesBLL` třída s `GetCategories` metody. Od tohoto ovládacího prvku GridView stačí zobrazí kategorie a neposkytuje funkce pro úpravu dat, nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource vrátit kategorií pomocí GetCategories – metoda](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Obrázek 4**: Konfigurace ObjectDataSource vrátit pomocí kategorií `GetCategories` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Obrázek 5**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit záložky (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio přidá vlastnost BoundField pro `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, a `BrochurePath`. Upravte prvku GridView, tak, aby obsahoval pouze `CategoryName` a `Description` BoundFields a aktualizovat `CategoryName` Vlastnost BoundField s `HeaderText` vlastnosti do kategorií.

V dalším kroku přidejte HyperLinkField a umístěte ho tak, že s pole nejvíce vlevo. Nastavte `DataNavigateUrlFields` vlastnost `CategoryID` a `DataNavigateUrlFormatString` vlastnost `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Nastavte `Text` vlastnost zobrazit produkty.


![Přidat HyperLinkField do kategorií GridView](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Obrázek 6**: Přidat HyperLinkField k `Categories` GridView


Po vytvoření ObjectDataSource a přizpůsobení polí s ovládacího prvku GridView, dva ovládací prvky deklarativní bude vypadat nějak takto:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Obrázek 7 znázorňuje `Default.aspx` při prohlížení prostřednictvím prohlížeče. Kliknutím na kategorii s zobrazit produkty odkaz vás nasměruje na `ProductsByCategory.aspx?CategoryID=categoryID`, který vytvoříme v kroku 3.


[![Každá kategorie je uvedený spolu s odkazem produkty zobrazení](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Obrázek 7**: Každá kategorie je uvedený spolu s odkazem Zobrazit produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Krok 3: Seznam produktů s vybranou kategorii.

Otevřít `ProductsByCategory.aspx` stránce a přidejte prvku GridView, jeho pojmenování `ProductsByCategory`. Z inteligentních značek, svázání prvku GridView nového prvku ObjectDataSource s názvem `ProductsByCategoryDataSource`. Konfigurace ObjectDataSource používat `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metoda a nastavte rozevíracího seznamu jsou uvedeny na (žádný) na kartách UPDATE, INSERT a DELETE.


[![Pomocí této metody s GetProductsByCategoryID(categoryID) ProductsBLL třídy](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Obrázek 8**: použijte `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


V posledním kroku v průvodci Konfigurace zdroje dat zobrazí výzvu k zadání parametrů zdroj pro *categoryID*. Protože tyto informace se předává pole řetězce dotazu `CategoryID`vyberte řetězce dotazu z rozevíracího seznamu a zadejte ID kategorie v textovém poli vlastnost QueryStringField, jak je znázorněno na obrázku 9. Kliknutím na Dokončit dokončíte průvodce.


[![Použijte pole řetězce dotazu ID kategorie pro ID kategorie parametr](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Obrázek 9**: použití `CategoryID` pro pole řetězce dotazu *categoryID* parametr ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Po dokončení průvodce, Visual Studio přidá odpovídající BoundFields a třídě CheckBoxField do prvku GridView pro datová pole produktu. Odeberte všechny kromě na `ProductName`, `UnitPrice`, a `SupplierName` BoundFields. Přizpůsobit tyto tři BoundFields `HeaderText` vlastností ke čtení produktu, ceny a dodavatele, v uvedeném pořadí. Formát `UnitPrice` Vlastnost BoundField jako měnu.

V dalším kroku přidejte HyperLinkField a přesunout na pozici nejvíce vlevo. Nastavte jeho `Text` vlastnosti chcete zobrazit podrobnosti, jeho `DataNavigateUrlFields` vlastnost `ProductID`a jeho `DataNavigateUrlFormatString` vlastnost `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Přidat podrobnosti HyperLinkField zobrazení, který odkazuje na ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Obrázek 10**: Přidání, zobrazení podrobností HyperLinkField, který odkazuje na `ProductDetails.aspx`


Po provedení tyto úpravy, ovládacími prvky GridView a prvku ObjectDataSource s deklarativní by měl vypadat takto:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Vraťte se do zobrazení `Default.aspx` prostřednictvím prohlížeče a klikněte na Zobrazit produkty odkaz nápoje. Tím přejdete na `ProductsByCategory.aspx?CategoryID=1`, zobrazení názvy, ceny a dodavatelů, produkty, které patří do kategorie Nápoje databáze Northwind (viz obrázek 11). Nebojte se dál vylepšit tuto stránku a obsahovat odkaz pro návrat na stránku výpis kategorie uživatelů (`Default.aspx`) a ovládacího prvku DetailsView nebo FormView zobrazující vybranou kategorii s název a popis.


[![Jsou zobrazeny názvy nápoje, ceny a dodavatelů](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Obrázek 11**: zobrazené názvy nápoje, ceny a dodavatelů ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Krok 4: S podrobnostmi produktu s

Na poslední stránce `ProductDetails.aspx`, se zobrazí podrobnosti vybrané produkty. Otevřít `ProductDetails.aspx` a DetailsView přetáhněte z panelu nástrojů do návrháře. Nastavit prvek DetailsView s `ID` vlastnost `ProductInfo` a vymažte její `Height` a `Width` hodnot vlastností. Z inteligentních značek, svázat s ovládacím prvku DetailsView nového prvku ObjectDataSource s názvem `ProductDataSource`, konfigurace ObjectDataSource přebírat jeho data ze `ProductsBLL` třída s `GetProductByProductID(productID)` metody. Stejně jako u předchozí webových stránek vytvořených v krocích 2 a 3, nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource GetProductByProductID(productID) metody](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Obrázek 12**: Konfigurace ObjectDataSource k použití `GetProductByProductID(productID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Poslední krok v průvodci Konfigurace zdroje dat zobrazí výzvu k zadání zdroj *productID* parametru. Protože tato data prochází pole řetězce dotazu `ProductID`, nastavte rozevírací seznam na řetězce dotazu a vlastnost QueryStringField textového pole na ProductID. Nakonec kliknutím na tlačítko Dokončit dokončete průvodce.


[![Konfigurace productID parametr načítat jeho hodnotu z pole řetězce dotazu ProductID](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Obrázek 13**: konfigurace *productID* o přijetí změn svou hodnotu z parametru `ProductID` pole řetězce dotazu ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, vytvoří Visual Studio odpovídající BoundFields a třídě CheckBoxField v ovládacím prvku DetailsView pro datová pole produktu. Odeberte `ProductID`, `SupplierID`, a `CategoryID` BoundFields a nakonfigurujte zbývající pole podle svých potřeb. Po několika aesthetic konfigurace my DetailsView a prvku ObjectDataSource s deklarativní vypadal následující:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Chcete-li otestovat tuto stránku, vraťte se na `Default.aspx` a klikněte na Zobrazit produkty pro kategorie Nápoje. V seznamu nápoje produktů, klikněte na odkaz zobrazit podrobnosti pro Chai čaje. Tím přejdete na `ProductDetails.aspx?ProductID=1`, zobrazí s Chai čaje podrobnosti (viz obrázek 14).


[![Zobrazí se Chai čaje s dodavatel, kategorie, ceny a dalších informací](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Obrázek 14**: Chai čaje s dodavatel, kategorie, ceny a další informace se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Krok 5: Principy vnitřní fungování zprostředkovatele mapy webu

Mapa webu reprezentované v paměti webového serveru s jako kolekci `SiteMapNode` instancí, které tvoří hierarchii. Musí být přesně jeden kořenový, všechny uzly nekořenovými musí obsahovat přesně jeden nadřazený uzel a všechny uzly mohou mít libovolný počet podřízené položky. Každý `SiteMapNode` objekt představuje oddíl ve struktuře webu s; tyto části běžně mít odpovídající webové stránky. V důsledku toho [ `SiteMapNode` třídy](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) má vlastnosti, jako je `Title`, `Url`, a `Description`, které poskytují informace o části `SiteMapNode` představuje. K dispozici je také `Key` vlastnost, která jednoznačně identifikuje každý `SiteMapNode` v hierarchii, jakož i vlastnosti, které používá k navázání této hierarchie `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`a tak dále.

Obrázek 15 znázorňuje strukturu obecný web mapování z obrázku 1, ale s šrafují podrobně jemnější podrobnosti implementace.


[![Každý SiteMapNode má vlastnosti jako název, adresu Url, klíč a tak dále](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Obrázek 15**: každý `SiteMapNode` má vlastnosti jako `Title`, `Url`, `Key`, a tak dále ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Mapa webu je přístupný prostřednictvím [ `SiteMap` třídy](https://msdn.microsoft.com/library/system.web.sitemap.aspx) v [ `System.Web` obor názvů](https://msdn.microsoft.com/library/system.web.aspx). Tato třída s `RootNode` vlastnost vrací mapy s kořenového webu `SiteMapNode` instance; `CurrentNode` vrátí `SiteMapNode` jehož `Url` vlastnost odpovídá adrese URL aktuálně požadovanou stránku. Tato třída se používá interně ve webové ovládací prvky pro navigaci s ASP.NET 2.0.

Když `SiteMap` vlastnosti třídy s přistupuje, se musí serializovat strukturu mapy webu z některých trvalou média do paměti. Ale logiky serializace mapy webu není pevný zakódovaný do `SiteMap` třídy. Místo toho v době běhu `SiteMap` určuje třídy, které mapy webu *poskytovatele* pro serializaci. Ve výchozím nastavení [ `XmlSiteMapProvider` třída](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) se používá, která čte mapu s struktury webu ze souboru XML správně naformátovaná. Však stačí nepatrné práce, můžeme vytvořit vlastní vlastního zprostředkovatele mapy webu.

Všichni poskytovatelé map webu musí být odvozen od [ `SiteMapProvider` třída](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), což zahrnuje základní metody a vlastnosti potřebné pro server mapování poskytovatele, ale vynechá řadu podrobnosti implementace. Druhá třída [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), rozšiřuje `SiteMapProvider` třída a obsahuje více robustní provádění požadovaných funkcí. Interně `StaticSiteMapProvider` ukládá `SiteMapNode` instance serveru namapovat ve `Hashtable` a poskytuje metody, jako jsou `AddNode(child, parent)`, `RemoveNode(siteMapNode),` a `Clear()` , přidávat a odebírat `SiteMapNode` s vnitřní `Hashtable`. `XmlSiteMapProvider` je odvozen z `StaticSiteMapProvider`.

Při vytváření vlastního zprostředkovatele mapy webu, který rozšiřuje `StaticSiteMapProvider`, existují dvě abstraktní metody, které se musí přepsat: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) a [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, jak již název napovídá, zodpovídá za načítání strukturu mapy webu z trvalého úložiště a vytvářet v paměti. `GetRootNodeCore` Vrátí kořenový uzel mapy webu.

Před webové aplikace můžete použít zprostředkovatele mapy webu, který musí být zaregistrovaný v konfiguraci aplikace s. Ve výchozím nastavení `XmlSiteMapProvider` třída je registrována pomocí názvu `AspNetXmlSiteMapProvider`. Registrace zprostředkovatele mapy další lokality, přidejte následující kód k `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*Název* hodnotu přiřadí popisný název zprostředkovatele při *typ* Určuje typ plně kvalifikovaný název zprostředkovatele mapy webu. Ukážeme si některé konkrétní hodnoty *název* a *typ* hodnoty v kroku 7, po jsme vytvořili ve naše vlastního zprostředkovatele mapy webu.

Třída zprostředkovatele mapy webu je vytvořena instance při prvním je přistupovat z `SiteMap` třídy a zůstanou v paměti dobu životnosti webové aplikace. Vzhledem k tomu, že existuje pouze jedna instance zprostředkovatele mapy webu, které mohou být vyvolány z více, návštěvníkům webu souběžných, je nutné, aby se metody zprostředkovatele s *bezpečné pro vlákna*.

Pro výkon a škálovatelnost je důležité, že jsme mezipaměti webu v paměti s zmapování struktury a vraťte to do mezipaměti, struktura, místo opětovného vytvoření pokaždé, když `BuildSiteMap` vyvolání metody. `BuildSiteMap` může být volána několikrát za požadavek na stránku na uživatele, v závislosti na navigační ovládací prvky používané na stránce a hloubka strukturu mapy webu. V případě, pokud jsme Neukládat do mezipaměti struktury mapy webu v `BuildSiteMap` pokaždé, když je vyvolána jsme nutné znovu načíst informace o produktu a kategorie z architektury (což by vytvořilo dotaz do databáze). Jak jsme probírali v předchozích kurzech ukládání do mezipaměti, můžete data uložená v mezipaměti stala zastaralými. Z toho důvodu, můžeme použít buď čas - nebo expiries založené na závislost mezipaměti SQL.

> [!NOTE]
> Volitelně může přepsat zprostředkovatele mapy webu [ `Initialize` metoda](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` je voláno, když při prvním vytvoření instance zprostředkovatele mapy webu a je předán všemi vlastními atributy přiřazené k poskytovateli v `Web.config` v `<add>` prvky jako: `<add name="name" type="type" customAttribute="value" />`. To je užitečné, pokud chcete povolit vývojářům určit různé lokality mapy týkající se poskytovatele nastavení bez nutnosti upravovat kód s zprostředkovatele. Pokud jsme byly čtení dat kategorie a produktů pravděpodobně přímo z databáze nikoli prostřednictvím architektury, jsme d třeba chcete, aby vývojář stránky zadejte připojovací řetězec databáze prostřednictvím `Web.config` místo použití pevně zakódované Hodnota v kódu s zprostředkovatele. Vlastního zprostředkovatele mapy webu vytvoříme v kroku 6 nepřepisuje to `Initialize` metody. Příklad použití `Initialize` metodu, najdete [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [ukládání mapy webu v systému SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) článku.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Krok 6: Vytvoření vlastního zprostředkovatele mapy webu

Chcete-li vytvořit vlastního zprostředkovatele mapy webu, který vytváří Mapa webu z kategorie a produkty v databázi Northwind, musíme vytvořit třídu, která rozšiřuje `StaticSiteMapProvider`. V kroku 1 se zobrazuje výzva k přidání `CustomProviders` složky `App_Code` složka – přidejte novou třídu do této složky s názvem `NorthwindSiteMapProvider`. Přidejte následující kód, který `NorthwindSiteMapProvider` třídy:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Umožní s začínat zkoumání této třídy s `BuildSiteMap` metodu, která začíná [ `lock` příkaz](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Příkaz povoluje jenom jedno vlákno najednou zadat, a tím serializace přístup k jeho kódu a zabraňuje krokování na prstech s sebou dvou souběžných vláken.

Úrovni třídy `SiteMapNode` proměnnou `root` se používá k ukládání do mezipaměti strukturu mapy webu. Když poprvé nebo poprvé po podkladová data byla změněna, mapy webu `root` bude `Nothing` a částka se vypočte strukturu mapy webu. Kořenový uzel mapy s lokalitě přiřazen `root` během procesu vytváření se nazývá procesu tak, aby příště tuto metodu, `root` nebudou `Nothing`. V důsledku toho tak dlouho, dokud `root` není `Nothing` strukturu mapy webu bude vrácen volajícímu bez nutnosti znovu ho vytvořte.

Pokud je kořenový `Nothing`, strukturu mapy webu je vytvořený z informací o produktu a kategorie. Mapa webu je sestaven tak, že vytvoříte `SiteMapNode` instancí a pak tvoří hierarchii pomocí volání `StaticSiteMapProvider` třída s `AddNode` metoda. `AddNode` provádí se účetnictví ukládání různé `SiteMapNode` instance `Hashtable`. Než začneme, vytváření hierarchie, začneme voláním `Clear` metodu, která vymaže prvky z vnitřní `Hashtable`. Dále `ProductsBLL` třída s `GetProducts` metoda a výsledné `ProductsDataTable` jsou uloženy v místní proměnné.

Konstrukce mapy s lokality začne vytvořením kořenový uzel a ji přiřadíte `root`. Přetížení [ `SiteMapNode` konstruktor s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) používá zde a v rámci tohoto `BuildSiteMap` se předá tyto informace:

- Odkaz na zprostředkovatele mapy webu (`Me`).
- `SiteMapNode` s `Key`. Tato oprávnění vyžadují hodnota musí být jedinečná pro každou `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url` je volitelný, ale pokud je zadán, každý `SiteMapNode` s `Url` hodnota musí být jedinečný.
- `SiteMapNode` s `Title`, což je povinné.

`AddNode(root)` Přidá volání metody `SiteMapNode` `root` do mapy webu jako kořen. Dále, každý `ProductRow` v `ProductsDataTable` výčtu. Pokud už existuje `SiteMapNode` pro aktuální kategorii produktu s se odkazuje. V opačném případě nový `SiteMapNode` pro kategorii je vytvořen a přidán jako podřízený objekt `SiteMapNode``root` prostřednictvím `AddNode(categoryNode, root)` volání metody. Po odpovídající kategorii `SiteMapNode` uzlu byl nalezen nebo vytvořen, `SiteMapNode` je vytvořen pro aktuální produkt a přidán jako podřízený objekt kategorie `SiteMapNode` prostřednictvím `AddNode(productNode, categoryNode)`. Všimněte si, že kategorie `SiteMapNode` s `Url` hodnota vlastnosti je `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` while produktu `SiteMapNode` s `Url` je vlastnosti přiřazen `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Tyto produkty, které mají databáze `NULL` hodnotu pro jejich `CategoryID` jsou seskupené podle kategorie `SiteMapNode` jehož `Title` nastavenou na None a jehož `Url` je nastavena na prázdný řetězec. Jsem se rozhodla nastavit `Url` na prázdný řetězec, protože `ProductBLL` třída s `GetProductsByCategory(categoryID)` schopnost vrátit pouze tyto produkty se v tuto chvíli nemá metodu `NULL` `CategoryID` hodnotu. Navíc já bych chtěl předvést způsob vykreslení ovládací prvky navigace `SiteMapNode` , který nemá hodnotu pro jeho `Url` vlastnost. Neváhejte se v tomto kurzu rozšířit tak, aby žádný `SiteMapNode` s `Url` vlastnost odkazuje na `ProductsByCategory.aspx`, ještě zobrazí jenom produkty s `NULL` `CategoryID` hodnoty.


Po vytváření mapy webu, je libovolný objekt přidán do mezipaměti dat na závislost mezipaměti SQL `Categories` a `Products` tabulky prostřednictvím `AggregateCacheDependency` objektu. Prozkoumali jsme použití závislostí mezipaměti SQL v předchozím kurzu *použití závislostí mezipaměti SQL*. Vlastního zprostředkovatele mapy webu, ale používá přetížení mezipaměť dat s `Insert` metody, který jsme ve ještě chcete prozkoumat. Toto přetížení jako jeho poslední vstupní parametr přijímá delegát, který se volá, když se odebere objekt z mezipaměti. Konkrétně jsme předat v novém [ `CacheItemRemovedCallback` delegovat](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) , která odkazuje na `OnSiteMapChanged` definována metoda další dolů v `NorthwindSiteMapProvider` třídy.

> [!NOTE]
> Reprezentace v paměti mapy webu se uloží do mezipaměti prostřednictvím proměnné úrovni třídy `root`. Vzhledem k tomu, že existuje pouze jedna instance třídy zprostředkovatele mapy vlastní stránky a od této instance je sdílena mezi všemi vlákny ve webové aplikaci, tato proměnná třídy slouží jako mezipaměť. `BuildSiteMap` Metoda také používá mezipaměť dat, ale pouze jako prostředky, na které přijde upozornění, když data v podkladové databázi `Categories` nebo `Products` tabulky změn. Mějte na paměti, že hodnota do datové mezipaměti je jenom aktuálním datem a časem. Je aktuální data mapy *není* umístit do datové mezipaměti.


`BuildSiteMap` Dokončení metody tak, že vrací kořenový uzel mapy webu.

Ostatní metody jsou poměrně jednoduché. `GetRootNodeCore` je zodpovědná za navrácení kořenového uzlu. Protože `BuildSiteMap` vrátí kořen, `GetRootNodeCore` jednoduše vrací `BuildSiteMap` s návratovou hodnotu. `OnSiteMapChanged` Metody nastaví `root` zpět `Nothing` při odebrání položky mezipaměti. Se nastaví zpátky kořenem `Nothing`, při příštím `BuildSiteMap` je vyvolána, strukturu mapy webu bude znovu vytvořen. A konečně `CachedDate` vlastnost vrací hodnoty data a času, které jsou uložené v mezipaměti dat, pokud taková hodnota neexistuje. Tato vlastnost slouží vývojář stránky k určení, kdy byl naposledy do mezipaměti data mapy webu.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Krok 7: Registrace`NorthwindSiteMapProvider`

V pořadí pro naši webovou aplikaci používat `NorthwindSiteMapProvider` zprostředkovatele mapy webu vytvořené v kroku 6, musíme ho zaregistrovat `<siteMap>` část `Web.config`. Konkrétně, přidejte následující kód v rámci `<system.web>` prvek `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Tento kód provede dvě věci: nejprve znamená, že integrované `AspNetXmlSiteMapProvider` je zprostředkovatele mapy webu výchozí; za druhé, zaregistruje vlastního zprostředkovatele mapy webu vytvořené v kroku 6 s lidských – popisný název Northwind.

> [!NOTE]
> Pro zprostředkovatele mapy webu v aplikaci s `App_Code` složky, hodnota `type` atribut je jednoduše název třídy. Můžete také vlastního zprostředkovatele mapy webu by byla vytvořena v samostatném projektu knihovny tříd zkompilovaného sestavení umístí do webové aplikace s `/Bin` adresáře. V takovém případě `type` by byla hodnota atributu *Namespace*. *ClassName*, *AssemblyName* .


Po aktualizaci `Web.config`, věnujte chvíli zobrazení všech stránek z kurzů v prohlížeči. Mějte na paměti, že rozhraní navigace na levé straně stále zobrazuje v částech a kurzy definované v `Web.sitemap`. Důvodem je, že ponechali jsme `AspNetXmlSiteMapProvider` jako výchozího zprostředkovatele. Chcete-li vytvořit prvek navigace uživatelského rozhraní, který používá `NorthwindSiteMapProvider`, budeme muset explicitně zadat, že má být použita zprostředkovatele mapy webu Northwind. Uvidíme, jak to provést v kroku 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Krok 8: Zobrazení informací o lokalitě mapy pomocí vlastního zprostředkovatele mapy webu

Vlastní web zprostředkovatele mapy vytvořené a registrované v `Web.config`, můžeme znovu připravení přidat ovládací prvky pro navigaci na `Default.aspx`, `ProductsByCategory.aspx`, a `ProductDetails.aspx` stránky v `SiteMapProvider` složky. Začněte otevřením `Default.aspx` stránku a přetáhněte ji `SiteMapPath` z panelu nástrojů do návrháře. Ovládací prvky SiteMapPath ovládací prvek se nachází v části navigace na panelu nástrojů.


[![Přidat ovládací prvky SiteMapPath Default.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Obrázek 16**: Přidat SiteMapPath k `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Ovládací prvky SiteMapPath ovládací prvek zobrazuje navigace s popisem cesty, určující umístění s aktuální stránky v rámci mapy webu. Přidali jsme do horní části stránky předlohy ovládací prvky SiteMapPath zpátky *stránky předlohy a navigace na webu* kurzu.

Za chvíli zobrazení této stránky prostřednictvím prohlížeče. Ovládací prvky SiteMapPath přidá obrázek 16 používá zprostředkovatele mapy webu výchozí načítání jeho dat ze `Web.sitemap`. Proto tento navigační prvek určuje ukazuje Domů &gt; přizpůsobení mapy webu, stejně jako s popisem cesty v pravém horním rohu.


[![Tento navigační prvek určuje používá výchozí zprostředkovatele mapy webu](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Obrázek 17**: Tento navigační prvek určuje používá výchozí zprostředkovatele mapy webu ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Pokud chcete, aby SiteMapPath přidá obrázek 16 použijte zprostředkovatele mapy vlastního webu, který jsme vytvořili v kroku 6, nastavte jeho [ `SiteMapProvider` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind, název jsme přiřazena k `NorthwindSiteMapProvider` v `Web.config`. Bohužel návrháře i nadále používat výchozí zprostředkovatele mapy webu, ale pokud najdete na stránce prostřednictvím prohlížeče po provedení této změny vlastnosti uvidíte, že tento navigační prvek určuje teď používá vlastního zprostředkovatele mapy webu.


[![Tento navigační prvek určuje teď používá NorthwindSiteMapProvider zprostředkovatele mapy vlastní web](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Obrázek 18**: Tento navigační prvek určuje teď používá vlastní zprostředkovatele mapy webu `NorthwindSiteMapProvider` ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


Ovládací prvky SiteMapPath ovládací prvek zobrazuje více funkcí uživatelského rozhraní v `ProductsByCategory.aspx` a `ProductDetails.aspx` stránky. Přidání ovládací prvky SiteMapPath na tyto stránky nastavení `SiteMapProvider` vlastnost v obou, a Northwind. Z `Default.aspx` klikněte na odkaz zobrazit produkty nápoje a potom na odkaz zobrazit podrobnosti pro Chai čaje. Jak ukazuje obrázek 19, zahrnuje tento navigační prvek určuje aktuální části mapy webu (Chai čaje) a jeho nadřazenými prvky: nápoje a všechny kategorie.


[![Tento navigační prvek určuje teď používá NorthwindSiteMapProvider zprostředkovatele mapy vlastní web](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Obrázek 19**: Tento navigační prvek určuje teď používá vlastní zprostředkovatele mapy webu `NorthwindSiteMapProvider` ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Další prvky uživatelského rozhraní navigace je možné kromě SiteMapPath, jako je například ovládací prvky nabídky a prvku TreeView. `Default.aspx`, `ProductsByCategory.aspx`, A `ProductDetails.aspx` stránky soubor ke stažení pro účely tohoto kurzu, například všechny obsahují ovládací prvky nabídky (viz obrázek 20). Naleznete v tématu [zkoumání ASP.NET 2.0 s funkcemi navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) a [ovládací prvky navigace pomocí lokality](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) část [ASP.NET 2.0 šablon rychlý Start](https://quickstarts.asp.net/QuickStartv20/aspnet/) podrobnější rozbor ovládací prvky pro navigaci a systému lokality mapy v technologii ASP.NET 2.0.


[![Menu – ovládací prvek obsahuje seznam všech kategorií a produkty](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Obrázek 20**: nabídky ovládací prvek obsahuje seznam každý kategorií a produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Jak už bylo zmíněno dříve v tomto kurzu, strukturu mapy webu je přístupná prostřednictvím kódu programu přes `SiteMap` třídy. Následující kód vrátí kořen `SiteMapNode` výchozího zprostředkovatele:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Vzhledem k tomu, `AspNetXmlSiteMapProvider` je výchozím zprostředkovatelem pro naši aplikaci výše uvedený kód vracel kořenový uzel definovaný ve `Web.sitemap`. Chcete-li odkazovat na zprostředkovatele mapy webu jiné než výchozí, použijte `SiteMap` třída s [ `Providers` vlastnost](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) takto:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Kde *název* je název zprostředkovatele mapy webu vlastní (pro naši webovou aplikaci Northwind).

Chcete-li přístup ke členu specifické pro zprostředkovatele mapy webu, použijte `SiteMap.Providers["name"]` načtení instance zprostředkovatele a pak ji přetypujte na typ odpovídající. Například, chcete-li zobrazit `NorthwindSiteMapProvider` s `CachedDate` vlastnosti stránky technologie ASP.NET, použijte následující kód:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Ujistěte se, k otestování funkce závislosti mezipaměti SQL. Po najdete `Default.aspx`, `ProductsByCategory.aspx`, a `ProductDetails.aspx` stránky, přejděte do jednoho z kurzů v úpravy, vložení a odstranění oddílu a upravit název, kategorie nebo produktu. Pak se vraťte do jedné stránky `SiteMapProvider` složky. Za předpokladu, že uplynul dostatek času pro dotazovací mechanismus poznamenat změn do databáze, by měl být Mapa webu aktualizuje a zobrazí nový produkt nebo název kategorie.


## <a name="summary"></a>Souhrn

ASP.NET 2.0 obsahuje prvky mapy webu s `SiteMap` třídy, určuje řadu integrovaných navigační Web a výchozího zprostředkovatele mapy webu, který očekává, že informace mapy webu trvale uložena do souboru XML. Chcete-li použít informace o lokalitě mapování z jiného zdroje, jako z databáze, architektura s aplikací nebo vzdálené webové služby je nutné vytvořit vlastního zprostředkovatele mapy webu. To zahrnuje vytvoření třídy, která je odvozena přímo nebo nepřímo ze `SiteMapProvider` třídy.

V tomto kurzu jsme viděli, jak vytvořit vlastního zprostředkovatele mapy webu, který mapy webu na základě produktů a kategorii informací vyřazeny z architektury aplikace. Náš poskytovatel rozšířené `StaticSiteMapProvider` třídy a zavedením vytváření `BuildSiteMap` metodu, která načte data, mapování hierarchie lokality který je v mezipaměti výsledný struktury v proměnnou na úrovni. Pomocí funkce zpětného volání jsme použili závislosti mezipaměti SQL zneplatnění mezipaměti strukturu, kdy základní `Categories` nebo `Products` byla změněna data.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Ukládání mapy webu v systému SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) a [zprostředkovatele mapy webu SQL jste již bylo čekání](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Model poskytovatele s najdete v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Sada nástrojů pro poskytovatele](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Zkoumání ASP.NET 2.0 s funkcemi navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Dave Gardner, Zack Jones, Teresy Murphy a Bernadette Leigh. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](building-a-custom-database-driven-site-map-provider-cs.md)
