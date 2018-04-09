---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Vytváření vlastních vycházející z databáze lokality poskytovatele mapy (C#) | Microsoft Docs
author: rick-anderson
description: Výchozí poskytovatel mapy webu v technologii ASP.NET 2.0 načte data ze statického souboru XML. Zprostředkovatel formátu XML je vhodné pro mnoho malé a střední – okno velikosti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: cab1b02dff27e9bacec2f4d4f7facc9f99d76b0a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Vytváření vlastních vycházející z databáze lokality poskytovatele mapy (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) nebo [stáhnout PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Výchozí poskytovatel mapy webu v technologii ASP.NET 2.0 načte data ze statického souboru XML. Zprostředkovatel formátu XML je vhodné pro mnoho malých a středních webů, větší webové aplikace vyžadují více dynamická Mapa webu. V tomto kurzu jsme budete vytvářet poskytovatele mapy vlastní web, který načte data z vrstvu obchodní logiky, která načte data z databáze.


## <a name="introduction"></a>Úvod

ASP.NET 2.0 s lokality mapy funkce umožňuje vývojářům definovat mapy webu webové aplikace s některé trvalé média, jako třeba do souboru XML. Po definování data mapy webu je přístupná prostřednictvím kódu programu prostřednictvím [ `SiteMap` třída](https://msdn.microsoft.com/library/system.web.sitemap.aspx) v [ `System.Web` obor názvů](https://msdn.microsoft.com/library/system.web.aspx) prostřednictvím řady různých navigační webových ovládacích prvků, nebo SiteMapPath, nabídky a TreeView – ovládací prvky. Mapa systému lokality používá [modelu poskytovatelů](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) tak, aby implementace serializace mapy jiné lokalitě lze vytvořit a zapojen do webové aplikace. Výchozí poskytovatel mapy webu, který se dodává s prostředím ASP.NET 2.0 trvá struktury mapy webu v souboru XML. Zpět v [hlavní stránky a navigace na webu](../introduction/master-pages-and-site-navigation-cs.md) kurzu jsme vytvořili soubor s názvem `Web.sitemap` která obsahuje tuto strukturu a byla aktualizace jeho XML s každou novou část kurzu.

Výchozí poskytovatel mapy webu na základě XML funguje dobře v případě, že s struktury mapy webu je docela statické, například pro tyto kurzy. V mnoha případech ale dynamičtější mapy webu je potřeba. Vezměte v úvahu mapy webu na obrázku 1, kde každou kategorii a produktu se zobrazí jako oddíly ve struktuře s webu. Pomocí této mapy webu webové stránce odpovídající do kořenového uzlu může seznamu všechny kategorie, zatímco webové stránce určité kategorie s by seznam této kategorie s produkty a zobrazení webové stránky s konkrétním produktu by zobrazit produktu s podrobnosti.


[![Kategorie a produkty způsob vytvoření mapu s struktury webu](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Obrázek 1**: kategorií a produkty způsob vytvoření mapy webu s struktury ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Když tato struktura založené na kategorii a produktu může být pevně zakódované do `Web.sitemap` souboru, soubor by bylo potřeba aktualizovat pokaždé, když kategorie nebo produktu byla přidat, odebrat nebo přejmenovat. V důsledku toho mapy údržby lokality by výrazně zjednodušit pokud jeho struktura byl načteny z databáze, nebo v ideálním případě z vrstvu obchodní logiky s architektury aplikace. Tímto způsobem, jak byly přidány kategorií a produkty, přejmenován nebo odstraněn, mapy webu by automaticky aktualizovat tak, aby odrážela tyto změny.

Vzhledem k tomu, že serializace mapy lokality s prostředím ASP.NET 2.0 je vytvořené na modelu poskytovatelů, můžeme vytvořit vlastní poskytovatel mapy vlastního webu, která získá data z alternativní datové úložiště, jako je například databáze nebo architekturu. V tomto kurzu jsme budete vytvářet vlastní poskytovatele, který načte data z BLL. Umožňují s začít!

> [!NOTE]
> Poskytovatel mapy webu vlastní vytvořili v tomto kurzu je úzce spojeny s aplikačního modelu architektura a data. Jeff Prosise s [ukládání mapy webu v systému SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) a [poskytovatel mapy webu SQL jste již byla čekání](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) články zkontrolujte zobecněný přístup k ukládání dat mapy webu v systému SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Krok 1: Vytvoření vlastní stránky mapu zprostředkovatele webové stránky

Než začneme vytvoření zprostředkovatele mapy vlastní web, mohli s nejprve přidat na stránky ASP.NET, které budeme potřebovat pro účely tohoto kurzu. Nejprve přidejte novou složku s názvem `SiteMapProvider`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Také přidat `CustomProviders` podsložky k `App_Code` složky.


![Přidání stránky ASP.NET pro kurzy související zprostředkovatele mapy webu](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Obrázek 2**: Přidání stránky ASP.NET pro web mapování související zprostředkovatele kurzy


Vzhledem k tomu, že existuje jenom jeden kurzu pro tento oddíl, jsme nejsou zobrazeny nepotřebujete `Default.aspx` seznam kurzů k části s. Místo toho `Default.aspx` kategorie se zobrazí v ovládacím prvku GridView. To jsme budete řešení v kroku 2.

Potom aktualizujte `Web.sitemap` odkaz na `Default.aspx` stránky. Konkrétně, přidejte následující kód po ukládání do mezipaměti `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně nyní obsahuje položku pro tento kurz zprostředkovatele mapy jedinou lokality.


![Mapy webu nyní obsahuje položku pro tento kurz zprostředkovatele mapy webu](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Obrázek 3**: mapy webu nyní obsahuje položku pro tento kurz zprostředkovatele mapy webu


Tento kurz s hlavní výběr je ilustrovat vytváření poskytovatel mapy vlastního webu a konfigurace webové aplikace pro používání tohoto zprostředkovatele. Konkrétně jsme budete vytvářet poskytovatele, který vrátí mapy webu, která zahrnuje kořenový uzel společně s uzlem pro každou kategorii a produktu, jak je znázorněno na obrázku 1. Obecně platí může každý uzel v mapě webu zadejte adresu URL. Pro naše mapy webu adresa URL kořenový uzel s bude `~/SiteMapProvider/Default.aspx`, který se zobrazí seznam všech kategorií v databázi. Každý uzel kategorie v mapě webu bude mít adresu URL, která odkazuje na `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, který se zobrazí všechny produkty v zadaném *categoryID*. Nakonec bude každý uzel mapy webu produktu přejděte na `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, který se zobrazí podrobnosti o konkrétním produktu s.

Spuštění je potřeba vytvořit `Default.aspx`, `ProductsByCategory.aspx`, a `ProductDetails.aspx` stránky. Kroky 2, 3 a 4, jsou dokončit tyto stránky v uvedeném pořadí. Vzhledem k tomu, že těžiště tohoto kurzu je na lokality mapu zprostředkovatele, a vzhledem k tomu, že posledních kurzy mít zahrnutých vytváření sestav se tyto řazení vícestránkového a podrobností jsme se hurry prostřednictvím kroky 2 až 4. Pokud potřebujete aktualizačnímu programu na vytváření sestav a podrobností, které přesahují délku jedné stránky, přečtěte si téma zpět [filtrování a podrobností mezi dvěma stránkami](../masterdetail/master-detail-filtering-across-two-pages-cs.md) kurzu.

## <a name="step-2-displaying-a-list-of-categories"></a>Krok 2: Zobrazení seznamu kategorií

Otevřete `Default.aspx` stránku `SiteMapProvider` složku a přetáhněte GridView z panelu nástrojů na návrháře nastavení jeho `ID` k `Categories`. Ze inteligentní značky s GridView navázat jej na nové ObjectDataSource s názvem `CategoriesDataSource` a nakonfigurujte ji tak, aby ho načte jeho dat pomocí `CategoriesBLL` třídu s `GetCategories` metoda. Vzhledem k tomu, že tato rutina GridView právě zobrazí kategorie a neposkytuje funkce změny dat, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource vrátit kategorií metodou GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Obrázek 4**: Konfigurace ObjectDataSource vrátit pomocí kategorií `GetCategories` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Obrázek 5**: rozevíracího seznamu jsou uvedeny ve UPDATE, INSERT a odstranit karty na hodnotu (žádná) ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Po dokončení průvodce Konfigurace zdroje dat, Visual Studio. přidá BoundField pro `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, a `BrochurePath`. Upravte GridView tak, aby obsahoval pouze `CategoryName` a `Description` BoundFields a aktualizovat `CategoryName` BoundField s `HeaderText` vlastnost kategorie.

V dalším kroku přidejte HyperLinkField a umístěte ho, aby s poli nejvíce vlevo. Nastavte `DataNavigateUrlFields` vlastnost `CategoryID` a `DataNavigateUrlFormatString` vlastnost `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Nastavte `Text` vlastnost zobrazit produkty.


![Přidání HyperLinkField do kategorií GridView](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Obrázek 6**: přidejte HyperLinkField k `Categories` GridView


Po vytvoření ObjectDataSource a přizpůsobení pole s GridView, deklarativní dvou ovládacích prvků bude vypadat takto:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

Obrázek 7 znázorňuje `Default.aspx` při zobrazení prostřednictvím prohlížeče. Kliknutím na kategorii s produkty zobrazení odkazu přejdete na `ProductsByCategory.aspx?CategoryID=categoryID`, které vytvoříme v kroku 3.


[![Každá kategorie je uveden spolu s odkazem produkty zobrazení](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Obrázek 7**: Každá kategorie je uveden spolu s odkazem produkty zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Krok 3: Výpis produkty s vybranou kategorii

Otevřete `ProductsByCategory.aspx` stránky a přidejte GridView, pojmenování ho `ProductsByCategory`. Z jeho inteligentních značek navázat GridView na nové ObjectDataSource s názvem `ProductsByCategoryDataSource`. Konfigurace ObjectDataSource používat `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda a sadu rozevíracího seznamu jsou uvedeny na (žádný) na kartách UPDATE, INSERT a DELETE.


[![Pomocí této metody GetProductsByCategoryID(categoryID) s ProductsBLL – třída](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Obrázek 8**: použití `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Posledním krokem v průvodci Konfigurace zdroje dat zobrazí výzvu pro parametr zdroj pro *categoryID*. Vzhledem k tomu, že tyto informace je předána pole řetězce dotazu `CategoryID`vyberte řetězce dotazu z rozevíracího seznamu a zadejte do textového pole vlastnost QueryStringField CategoryID, jak je znázorněno na obrázku 9. Kliknutím na tlačítko Dokončit ukončete průvodce.


[![Použití pole řetězce dotazu CategoryID ČísloKategorie parametr](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Obrázek 9**: použití `CategoryID` pole řetězce dotazu pro *categoryID* parametr ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


Po dokončení průvodce se přidá Visual Studio odpovídající BoundFields a třídy CheckBoxField do GridView pro produkt datová pole. Odeberte všechny ale na `ProductName`, `UnitPrice`, a `SupplierName` BoundFields. Přizpůsobit tyto tři BoundFields `HeaderText` vlastnosti číst produktu, ceny a dodavatele, v uvedeném pořadí. Formát `UnitPrice` BoundField jako měny.

V dalším kroku přidejte HyperLinkField a přesunout ho na pozici nejvíce vlevo. Nastavit jeho `Text` vlastnost zobrazit podrobnosti, jeho `DataNavigateUrlFields` vlastnost `ProductID`a jeho `DataNavigateUrlFormatString` vlastnost `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Přidat podrobnosti HyperLinkField zobrazení, která odkazuje na ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Obrázek 10**: Přidání, zobrazení podrobností HyperLinkField, která odkazuje na `ProductDetails.aspx`


Po přizpůsobení, rutina GridView a ObjectDataSource s deklarativní by měl vypadat takto:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Vrátí k zobrazení `Default.aspx` prostřednictvím prohlížeče a klikněte na Zobrazit produkty propojení pro nápoje. Tím přejdete na `ProductsByCategory.aspx?CategoryID=1`, zobrazení názvy, ceny a všichni dodavatelé produktů v databázi Northwind, která patří do kategorie nápoje (viz obrázek 11). Nebojte se dále vylepšit tuto stránku zahrnout odkaz na návrat na stránku výpis kategorie uživatelů (`Default.aspx`) a DetailsView nebo FormView ovládací prvek, který zobrazí vybrané kategorie s název a popis.


[![Zobrazují názvy nápoje, ceny a všichni dodavatelé](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Obrázek 11**: se zobrazují nápoje názvy, ceny a všichni dodavatelé ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Krok 4: Zobrazení s podrobnosti produktu

Na poslední stránce `ProductDetails.aspx`, zobrazí podrobnosti vybrané produkty. Otevřete `ProductDetails.aspx` a přetáhněte ji DetailsView z panelu nástrojů na návrháře. Nastavit DetailsView s `ID` vlastnost `ProductInfo` a vymažte její `Height` a `Width` hodnot vlastností. Z jeho inteligentních značek navázat DetailsView na nové ObjectDataSource s názvem `ProductDataSource`, konfigurace ObjectDataSource načítat data z `ProductsBLL` třídu s `GetProductByProductID(productID)` metoda. Stejně jako u předchozí webové stránky vytvořené v kroky 2 a 3, nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource lze pomocí této metody GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Obrázek 12**: Konfigurace ObjectDataSource pro použití `GetProductByProductID(productID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


Zobrazí výzvu k poslednímu kroku průvodce Konfigurace zdroje dat pro zdroj *productID* parametr. Vzhledem k tomu, že tato data přicházejí pole řetězce dotazu `ProductID`, nastavit rozevíracího seznamu a do textového pole vlastnost QueryStringField na ProductID řetězce dotazu. Nakonec kliknutím na tlačítko Dokončit dokončete průvodce.


[![Konfigurace productID parametr načítat svou hodnotu z na základě pole řetězce dotazu](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Obrázek 13**: konfigurace *productID* parametr vyžádání svou hodnotu z `ProductID` pole řetězce dotazu ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


Po dokončení průvodce Konfigurace zdroje dat se vytvoří sada Visual Studio odpovídající BoundFields a třídy CheckBoxField v ovládacím prvku DetailsView pro produkt datová pole. Odeberte `ProductID`, `SupplierID`, a `CategoryID` BoundFields a nakonfigurujte ostatní pole, podle potřeby. Po někteří z nich estetické konfigurace hledá Moje DetailsView a ObjectDataSource s deklarativní takto:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Chcete-li otestovat této stránky, vraťte se k `Default.aspx` a klikněte na Zobrazit produkty pro kategorii Nápoje. Z seznam produktů nápoj, klikněte na odkaz zobrazit podrobnosti pro Chai čaj. Tím přejdete na `ProductDetails.aspx?ProductID=1`, zobrazující s Chai čaj (viz obrázek 14) podrobností.


[![Chai čaj s dodavatel, kategorie, ceny a další informace se zobrazí.](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Obrázek 14**: Chai čaj s dodavatel, kategorie, ceny a další informace se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Krok 5: Principy vnitřního chodu poskytovatel mapy webu

Mapy webu je vyjádřené v paměti webového serveru s kolekce `SiteMapNode` instance, které vytvářejí hierarchii. Musí být přesně jeden kořenový, všechny jiné než kořenové uzly musí mít přesně jeden nadřazený uzel a všechny uzly může mít libovolný počet podřízených prvků. Každý `SiteMapNode` objekt reprezentuje oddíl ve struktuře webu s; tyto části běžně mít odpovídající webové stránky. V důsledku toho [ `SiteMapNode` – třída](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) obsahuje vlastnosti, například `Title`, `Url`, a `Description`, které poskytují informace o části `SiteMapNode` představuje. K dispozici je také `Key` vlastnost, která jednoznačně identifikuje každý `SiteMapNode` v hierarchii, jakož i vlastnosti, na které se použijí k vytvoření této hierarchie `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, a tak dále.

Obrázek 15 zobrazí mapu struktury obecné webu z obrázku 1, ale s podrobnosti implementace šrafují podrobně jemnějšího.


[![Každý SiteMapNode má vlastnosti jako název, Url, klíč atd.](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Obrázek 15**: každý `SiteMapNode` má vlastnosti jako `Title`, `Url`, `Key`a tak dále ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


Mapa lokality je přístupná prostřednictvím [ `SiteMap` třída](https://msdn.microsoft.com/library/system.web.sitemap.aspx) v [ `System.Web` obor názvů](https://msdn.microsoft.com/library/system.web.aspx). Tato třída s `RootNode` vlastnost vrací mapy s kořenovým adresářem webu `SiteMapNode` instance; `CurrentNode` vrátí `SiteMapNode` jejichž `Url` vlastnost odpovídá adrese URL aktuálně požadované stránky. Tato třída se používá interně webové ovládací prvky pro navigaci s prostředím ASP.NET 2.0.

Když `SiteMap` přístup k vlastnostem třídy s, se musí serializovat struktury mapy webu z některé trvalé střední do paměti. Ale logiku serializace mapy webu není pevný zakódovaný do `SiteMap` třídy. Místo toho za běhu `SiteMap` třída určuje, které mapy webu *zprostředkovatele* používat pro serializaci. Ve výchozím nastavení [ `XmlSiteMapProvider` třída](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) se používá, který čte mapu s struktury webu ze souboru XML správně naformátován. S chvilku pracovní jsme však můžete vytvořit vlastní poskytovatel mapy vlastního webu.

Všichni poskytovatelé mapy webu musí být odvozen od [ `SiteMapProvider` třída](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), což zahrnuje základní metody a vlastnosti, které jsou potřebné pro lokalitu mapování poskytovatelů, ale vynechá řadu podrobnosti implementace. Třídu sekundu [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), rozšiřuje `SiteMapProvider` třídy a obsahuje robustnější provádění požadovaných funkcí. Interně `StaticSiteMapProvider` ukládá `SiteMapNode` instance lokality mapování v `Hashtable` a poskytuje metody, jako `AddNode(child, parent)`, `RemoveNode(siteMapNode),` a `Clear()` , přidávat a odebírat `SiteMapNode` s k interní `Hashtable`. `XmlSiteMapProvider` je odvozený od `StaticSiteMapProvider`.

Při vytváření poskytovatele mapy vlastní web, který rozšiřuje `StaticSiteMapProvider`, existují dvě abstraktní metody, které se musí přepsat: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) a [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, jak již název napovídá, zodpovídá za načítání struktury mapy webu z trvalého úložiště a vytváření v paměti. `GetRootNodeCore` Vrátí kořenového uzlu v mapě webu.

Před webové aplikace můžete použít poskytovatele mapy webu, které musí být zaregistrovaný v konfiguraci s aplikace. Ve výchozím nastavení `XmlSiteMapProvider` je zaregistrovat pomocí názvu `AspNetXmlSiteMapProvider`. Registrace zprostředkovatele mapy další lokality, přidejte následující kód do `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

*Název* hodnotu přiřadí popisný název zprostředkovatele při *typ* Určuje typ plně kvalifikovaný název poskytovatele mapy webu. Jsme budete prozkoumat konkrétní hodnoty *název* a *typ* hodnoty v kroku 7 po jsme vytvořili sunout naše poskytovatel mapy vlastního webu.

Vytvoření prvním je přístupný z instance třídy zprostředkovatele mapy webu `SiteMap` třídy a zůstanou v paměti po dobu jeho existence webové aplikace. Vzhledem k tomu, že existuje pouze jedna instance poskytovatele mapy webu, který může být volána z více souběžných webových návštěvníky, je nutné, aby metody zprostředkovatele s být *vláken*.

Pro výkon a škálovatelnost je důležité, že jsme mezipaměti v paměti lokality s namapovat struktura a vrátit to do mezipaměti, struktura, nikoli jeho opětné vytvoření pokaždé, když `BuildSiteMap` metoda je volána. `BuildSiteMap` může být volána několikrát za požadavek na stránku na uživatele, v závislosti na ovládací prvky navigace používán na stránce a hloubka struktury mapy webu. V případě, pokud jsme Neukládat do mezipaměti struktury mapy webu v `BuildSiteMap` pokaždé, když je volána, jsme nutné, aby znovu načíst informace o produktu a kategorie z architektury (což by způsobilo dotaz do databáze). Jak již bylo zmíněno v předchozí kurzech ukládání do mezipaměti, se může stát zastaralá data uložená v mezipaměti. Boje proti to, můžeme použít buď čas - nebo expiries na základě závislostí mezipaměti SQL.

> [!NOTE]
> Poskytovatel mapy webu může volitelně přepsat [ `Initialize` metoda](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` je voláno, když je poskytovatel mapy webu při prvním vytvoření instance a je předána žádné vlastní atributy, které jsou přiřazené k poskytovateli v `Web.config` v `<add>` element jako: `<add name="name" type="type" customAttribute="value" />`. Je vhodné, pokud chcete povolit vývojáři stránky zadejte různé lokality mapu zprostředkovatele související nastavení bez nutnosti upravovat kód s zprostředkovatele. Například pokud jsme byly čtení dat kategorie a produkty pravděpodobně přímo z databáze naproti tomu prostřednictvím architektury, jsme d Chcete umožnit vývojář stránky zadejte připojovací řetězec databáze prostřednictvím, aby `Web.config` místo pomocí pevně zakódovaný Hodnota v kódu zprostředkovatele s. Poskytovatel mapy webu vlastní jsme budete sestavení v kroku 6 nepřepisuje to `Initialize` metoda. Příklad použití `Initialize` metoda, odkazovat na [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [ukládání mapy webu v systému SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) článku.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Krok 6: Vytvoření zprostředkovatele mapy vlastní stránky

Pokud chcete vytvořit vlastní stránky mapu zprostředkovatele, který sestaví mapy webu z kategorií a produktů v databázi Northwind, musíme vytvořit třídu, která rozšiřuje `StaticSiteMapProvider`. V kroku 1 zpráva, můžete přidat `CustomProviders` složku `App_Code` složky - přidejte novou třídu do této složky s názvem `NorthwindSiteMapProvider`. Přidejte následující kód, který `NorthwindSiteMapProvider` třídy:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Umožní s začínat zkoumat tuto třídu s `BuildSiteMap` metodu, která začíná [ `lock` příkaz](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Příkaz umožňuje použití jenom jedno vlákno současně zadat, a tím serializaci přístup k jeho kód a brání krokování na sebe navzájem s pouze dvěma souběžných vláken.

Úroveň třída `SiteMapNode` proměnná `root` se používá pro ukládání do mezipaměti struktury mapy webu. Když se mapy webu poprvé nebo poprvé po podkladová data byla změněna, `root` bude `null` a bude vypočten struktury mapy webu. Kořenový uzel mapy s webu je přiřazena k `root` během konstrukce proces tak, aby příště tato metoda se označuje jako, `root` nebude `null`. V důsledku toho tak dlouho, dokud `root` není `null` struktury mapy webu bude vrácen volajícímu bez nutnosti ho znovu vytvořit.

Pokud je kořenový `null`, struktury mapy webu je vytvořený z informací o produktu a kategorie. Mapy webu je sestavena vytváření `SiteMapNode` instancí a pak které tvoří hierarchii prostřednictvím volání `StaticSiteMapProvider` třídu s `AddNode` metoda. `AddNode` provede účetnictví ukládání různé `SiteMapNode` instancí v `Hashtable`. Než začneme vytvořením hierarchii, začneme voláním `Clear` metodu, která vymaže prvky z interní `Hashtable`. Dále `ProductsBLL` třídu s `GetProducts` metoda a výsledná `ProductsDataTable` jsou uloženy v místní proměnné.

Vytváření mapy s lokality začíná vytvořením kořenového uzlu a přiřadit ji k `root`. Přetížení [ `SiteMapNode` konstruktor s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) používá sem a v rámci to `BuildSiteMap` je předán následující informace:

- Odkaz na poskytovatel mapy webu (`this`).
- `SiteMapNode` s `Key`. To vyžaduje, hodnota musí být jedinečný pro každý `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url` je volitelný, ale pokud je zadán, každý `SiteMapNode` s `Url` hodnota musí být jedinečná.
- `SiteMapNode` s `Title`, což je vyžadováno.

`AddNode(root)` Volání metody přidá `SiteMapNode` `root` do mapy webu jako kořen. Další, každý `ProductRow` v `ProductsDataTable` výčtu. Pokud už existuje `SiteMapNode` pro aktuální kategorii produktu s, se odkazuje. Jinak nový `SiteMapNode` pro kategorii je vytvořen a přidán jako podřízený `SiteMapNode``root` prostřednictvím `AddNode(categoryNode, root)` volání metody. Po příslušné kategorii `SiteMapNode` uzel byl nalezen nebo vytvořili, `SiteMapNode` se vytvoří pro aktuální produkt a přidán jako podřízený kategorie `SiteMapNode` prostřednictvím `AddNode(productNode, categoryNode)`. Všimněte si, že kategorie `SiteMapNode` s `Url` hodnota vlastnosti je `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` při produktu `SiteMapNode` s `Url` je přiřazená vlastnost `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Tyto produkty, které máte databázi `NULL` hodnotu pro jejich `CategoryID` jsou seskupené v rámci kategorii `SiteMapNode` jejichž `Title` je nastavena na žádné a jehož `Url` je nastavena na prázdný řetězec. Rozhodli nastavit `Url` na prázdný řetězec od `ProductBLL` třídu s `GetProductsByCategory(categoryID)` metoda aktuálně nemá schopnost vrátit pouze tyto produkty s `NULL` `CategoryID` hodnotu. Navíc měla být ukazují, jak vykreslit ovládací prvky pro navigaci `SiteMapNode` který nemá hodnotu pro jeho `Url` vlastnost. I doporučujeme rozšiřovat tak, aby žádný `SiteMapNode` s `Url` vlastnost ukazuje na `ProductsByCategory.aspx`, ještě pouze zobrazí produkty s `NULL` `CategoryID` hodnoty.


Po vytváření mapy webu, je libovolný objekt přidán do mezipaměti dat pomocí závislost SQL mezipaměti na `Categories` a `Products` tabulky prostřednictvím `AggregateCacheDependency` objektu. Jsme prozkoumali pomocí závislosti mezipaměti SQL v předchozím kurzu *pomocí závislosti mezipaměti SQL*. Poskytovatel mapy vlastního webu, ale používá přetížení datovou mezipaměť s `Insert` metoda který jsme jste ještě chcete prozkoumat. Toto přetížení akceptuje jako vstupní parametr jeho poslední delegáta, který je volána, když je objekt odebrat z mezipaměti. Konkrétně jsme předat v nové [ `CacheItemRemovedCallback` delegovat](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) odkazující `OnSiteMapChanged` metoda definované další dolů v `NorthwindSiteMapProvider` – třída.

> [!NOTE]
> Je uložený v mezipaměti v paměti reprezentace mapy webu prostřednictvím proměnné úrovni třídy `root`. Vzhledem k tomu, že existuje pouze jedna instance třídy zprostředkovatele mapy vlastního webu a vzhledem k tomu, že instance sdílí všechna vlákna ve webové aplikaci, tato proměnná třída slouží jako mezipaměť. `BuildSiteMap` Metoda také používá mezipaměť dat, ale pouze jako prostředek k přijetí oznámení při základní data v databázi `Categories` nebo `Products` tabulky změn. Upozorňujeme, že hodnota vložena data se právě aktuální datum a čas. Data lokality mapy *není* put v datové mezipaměti.


`BuildSiteMap` Metoda dokončení vrácením kořenového uzlu mapy webu.

Ostatní metody jsou poměrně jednoduché. `GetRootNodeCore` zodpovídá za vrácení kořenového uzlu. Vzhledem k tomu `BuildSiteMap` vrátí kořenová `GetRootNodeCore` jednoduše vrátí `BuildSiteMap` s vrátit hodnotu. `OnSiteMapChanged` Metoda nastaví `root` zpět na `null` po odebrání položky v mezipaměti. S kořenem zpět `null`, při příštím `BuildSiteMap` je vyvolána, strukturu mapy lokality bude znovu vytvořen. Nakonec `CachedDate` vlastnost vrací hodnoty data a času, které jsou uložené v mezipaměti dat, pokud taková hodnota existuje. Tuto vlastnost lze použít vývojáři stránky, kdy byl naposledy do mezipaměti data mapy webu.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Krok 7: Registrace`NorthwindSiteMapProvider`

Aby naše webovou aplikaci pro použití `NorthwindSiteMapProvider` poskytovatel mapy webu vytvořené v kroku 6, potřebujeme a zaregistrujte ho v `<siteMap>` části `Web.config`. Konkrétně, přidejte následující kód v rámci `<system.web>` element v `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Tento kód provádí dvě věci: nejdřív znamená, že integrované `AspNetXmlSiteMapProvider` je výchozí poskytovatel mapy webu; druhé registraci poskytovatele mapy vlastní webu s názvem lidské friendly Northwind vytvořena v kroku 6.

> [!NOTE]
> Pro zprostředkovatele mapy webu v aplikaci s `App_Code` složky, hodnota `type` atribut je jednoduše název třídy. Alternativně poskytovatel mapy vlastního webu by byla vytvořena v samostatných projektu knihovny tříd kompilované sestavení umístěny webové aplikace s `/Bin` adresáře. V takovém případě `type` by být hodnota atributu *Namespace*. *Název třídy*, *AssemblyName* .


Po aktualizaci `Web.config`, za chvíli k zobrazení všech stránek z kurzů k v prohlížeči. Upozorňujeme, že rozhraní navigace na levé straně stále zobrazuje v částech a kurzy definované v `Web.sitemap`. Důvodem je, že jsme zbývajících `AspNetXmlSiteMapProvider` jako výchozího zprostředkovatele. Chcete-li vytvořit element uživatelského rozhraní navigace, který používá `NorthwindSiteMapProvider`, je třeba explicitně zadat, že poskytovatel mapy webu Northwind má být použita. Uvidíme, jak to provést v kroku 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Krok 8: Zobrazení informací mapy webu pomocí zprostředkovatele mapy vlastní stránky

Vlastní web poskytovatele mapy vytvořen a zaregistrován v `Web.config`, jsme re připraveno k přidání ovládací prvky pro navigaci k `Default.aspx`, `ProductsByCategory.aspx`, a `ProductDetails.aspx` stránky v `SiteMapProvider` složky. Začněte otevřením `Default.aspx` stránky a přetáhněte ji `SiteMapPath` z panelu nástrojů na návrháře. Ovládací prvek SiteMapPath se nachází v části navigačního panelu nástrojů.


[![Přidání SiteMapPath do Default.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Obrázek 16**: přidejte SiteMapPath k `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


Ovládací prvek SiteMapPath zobrazí zobrazení cesty, která určuje umístění s aktuální stránky v rámci mapy webu. Jsme přidali SiteMapPath do horní části stránky předlohy zpět v *hlavní stránky a webové navigace* kurzu.

Chcete-li zobrazit tuto stránku prostřednictvím prohlížeče chvíli trvat. SiteMapPath přidali v obrázku 16 používá výchozí poskytovatel mapy webu, stahování data z `Web.sitemap`. Proto s popisem cesty zobrazuje Domů &gt; přizpůsobení mapy webu, stejně jako zobrazení cesty v pravém horním rohu.


[![S popisem cesty používá výchozí poskytovatel mapy webu](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Obrázek 17**: S popisem cesty používá výchozí poskytovatel mapy webu ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Aby SiteMapPath přidali v obrázku 16 použijte poskytovatel mapy webu vlastní jsme vytvořili v kroku 6, nastavte jeho [ `SiteMapProvider` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) k Northwind, název jsme se přiřadila `NorthwindSiteMapProvider` v `Web.config`. Bohužel návrháře budou nadále používat výchozí poskytovatel mapy webu, ale pokud najdete na stránce prostřednictvím prohlížeče po provedení této změny vlastnosti se zobrazí s popisem cesty teď používá poskytovatele mapy vlastní stránky.


[![Teď používá NorthwindSiteMapProvider zprostředkovatele mapy vlastní stránky s popisem cesty](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Obrázek 18**: S popisem cesty teď používá vlastní poskytovatel mapy webu `NorthwindSiteMapProvider` ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


Ovládací prvek SiteMapPath zobrazí více funkcí uživatelského rozhraní v `ProductsByCategory.aspx` a `ProductDetails.aspx` stránky. Přidat SiteMapPath na těchto stránkách, nastavení `SiteMapProvider` na Northwind vlastnost. Z `Default.aspx` klikněte na odkaz zobrazit produkty pro nápoje a potom na odkaz zobrazit podrobnosti pro Chai čaj. Jak ukazuje obrázek 19, s popisem cesty zahrnuje aktuálním lokality mapy (Chai čaj) a jeho předchůdců: nápoje a všechny kategorie.


[![Teď používá NorthwindSiteMapProvider zprostředkovatele mapy vlastní stránky s popisem cesty](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Obrázek 19**: S popisem cesty teď používá vlastní poskytovatel mapy webu `NorthwindSiteMapProvider` ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Další prvky uživatelského rozhraní navigační lze kromě SiteMapPath, jako jsou nabídky a TreeView – ovládací prvky. `Default.aspx`, `ProductsByCategory.aspx`, A `ProductDetails.aspx` stránky v souboru ke stažení pro tento kurz, například všechny obsahují nabídky ovládacích prvků (viz obrázek 20). V tématu [zkoumání technologii ASP.NET 2.0 s funkcemi navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) a [ovládací prvky navigace pomocí webu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) části [ASP.NET 2.0 – elementy QuickStart](https://quickstarts.asp.net/QuickStartv20/aspnet/) pro podrobnější pohled na ovládací prvky pro navigaci a mapy systému lokality v technologii ASP.NET 2.0.


[![Ovládací prvek nabídky obsahuje seznam všech kategorií a produkty](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Obrázek 20**: V nabídce ovládací prvek uvádí každý kategorií a produktů ([Kliknutím zobrazit obrázek v plné velikosti](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Jak je uvedeno v tomto kurzu, struktury mapy webu je přístupná prostřednictvím kódu programu prostřednictvím `SiteMap` třídy. Následující kód vrátí kořenu `SiteMapNode` výchozího poskytovatele:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Vzhledem k tomu `AspNetXmlSiteMapProvider` je výchozím zprostředkovatelem pro naši aplikaci ve výše uvedeném kódu by vrátit kořenového uzlu definována v `Web.sitemap`. Chcete-li poskytovatele mapy webu jiné než výchozí, použijte `SiteMap` třídu s [ `Providers` vlastnost](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) takto:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Kde *název* je název vlastního zprostředkovatele služby site mapy (Northwind, pro naše webové aplikace).

Chcete-li přístup ke členu specifické pro poskytovatele mapy webu, použijte `SiteMap.Providers["name"]` načtení instance poskytovatele a pak ho převést na příslušného typu. Chcete-li například zobrazit `NorthwindSiteMapProvider` s `CachedDate` vlastnosti stránky ASP.NET, použijte následující kód:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Ujistěte se, že otestování funkce závislosti mezipaměti SQL. Po navštívíte `Default.aspx`, `ProductsByCategory.aspx`, a `ProductDetails.aspx` stránky, přejděte na jeden z kurzů v úpravy, vkládání a odstraňování část a upravte název kategorie nebo produktu. Poté se vraťte na jednu ze stránek v `SiteMapProvider` složky. Za předpokladu, že uplynutí dostatek času pro dotazování mechanismus si uvědomit, tato změna základní databáze, je třeba aktualizovat mapy webu zobrazíte nového produktu nebo název kategorie.


## <a name="summary"></a>Souhrn

ASP.NET 2.0 obsahuje funkce mapy webu s `SiteMap` třídu, několik předdefinovaných navigační webové ovládací prvky a trvalé výchozí poskytovatel mapy webu, která očekává mapy informace o lokalitě do souboru XML. Chcete-li použít informace mapy webu z z jiného zdroje, jako z databáze, architektura s aplikací nebo vzdálené webové služby je potřeba vytvořit vlastní web mapovat zprostředkovatele. To zahrnuje vytvoření třídu, která pochází, přímo ani nepřímo, z `SiteMapProvider` třídy.

V tomto kurzu jsme viděli, jak vytvořit vlastní stránky mapu zprostředkovatele, který mapy webu na základě produktů a kategorii informací vyřazeny z architektury aplikace. Naše zprostředkovatele rozšířené `StaticSiteMapProvider` třídy a bylo vytváření `BuildSiteMap` metoda, která načíst data, sestavený mapy hierarchie lokality a uloží do mezipaměti strukturu výsledné v proměnné úrovni třídy. Jsme použili závislost SQL mezipaměti pomocí funkce zpětného volání zneplatní uložená v mezipaměti při struktury základní `Categories` nebo `Products` data je upravit.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Ukládání mapy webu v systému SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) a [poskytovatel mapy webu SQL jste již byla pro čekání.](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Podívejte se na technologii ASP.NET 2.0 zprostředkovatele s modelu](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Sada nástrojů zprostředkovatele](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Zkoumání ASP.NET 2.0 s funkcemi navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Dave Gardner, Zack Petr, Teresy Murphy a Bernadette Leigh. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](building-a-custom-database-driven-site-map-provider-vb.md)
