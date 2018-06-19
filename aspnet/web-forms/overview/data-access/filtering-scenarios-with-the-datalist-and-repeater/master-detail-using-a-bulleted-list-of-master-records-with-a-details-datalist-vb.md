---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Seznam záznamů hlavní s odrážkami pomocí podrobnosti DataList (VB) podrobností | Microsoft Docs
author: rick-anderson
description: V tomto kurzu jsme budete komprimovat dvě stránky a podrobností sestavy předchozí kurzu do jediné stránce s odrážkami seznam názvů kategorie na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d87dc7f4fb00e96d9eb2653e6fbc1efb8bb656c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889017"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Seznam záznamů hlavní s odrážkami pomocí podrobnosti DataList (VB) podrobností
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) nebo [stáhnout PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> V tomto kurzu jsme budete komprimovat dvě stránky a podrobností sestavy předchozí kurzu do jediné stránce s odrážkami seznam názvů kategorie zobrazuje na levé straně obrazovky a vybrané kategorie produktů na pravé straně obrazovky.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](master-detail-filtering-acess-two-pages-datalist-vb.md) jsme se podívali na tom, jak jednotlivé sestavy a podrobností mezi dvěma stránkami. Na hlavní stránce jsme prvku Repeater použít k vykreslení seznamu s odrážkami kategorií. Každý název kategorie byl hypertextový odkaz, že při kliknutí na, by proveďte uživatele na stránku podrobností, kde vám ukázal DataList dva sloupce, tyto produkty patřící do vybrané kategorie.

V tomto kurzu jsme budete komprimovat kurzu dvě stránky do jediné stránce zobrazuje seznam názvů kategorie s odrážkami na levé straně obrazovky s každý název kategorie se vykresluje jako LinkButton. Kliknutím na název kategorie LinkButtons indukuje zpětné volání a sváže s produkty vybrané kategorie DataList dva sloupce na pravé straně obrazovky. Kromě zobrazování každý název kategorie s, opakovače na levé straně se zobrazí, kolik existuje celkový počet produktů pro danou kategorii (viz obrázek 1).


[![Kategorie s názvem a celkový počet produktů, které se zobrazují na levé straně](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Obrázek 1**: kategorii s názvem a celkový počet produktů, které se zobrazují na levé straně ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Krok 1: Zobrazení prvku Repeater v levé části obrazovky

Pro účely tohoto kurzu je potřeba mít s odrážkami seznamu kategorií se zobrazí vlevo produkty s vybranou kategorii. Obsah na webové stránce můžete umístit pomocí standardní značky HTML pro elementy odstavce, pevné mezery, `<table>` s a tak dále, nebo prostřednictvím techniky kaskádových stylů (CSS). Všechny naše kurzy doposud použili šablon stylů CSS techniky pro umístění. Pokud jsme vytvořili uživatelské rozhraní navigace v našem hlavní stránce [hlavní stránky a navigace na webu](../introduction/master-pages-and-site-navigation-vb.md) kurzu jsme použili *absolutní umístění*, označující posun přesné pixelů pro navigaci seznam a hlavní obsah. Alternativně lze na pozici jeden element práva nebo nalevo od jiného prostřednictvím šablon stylů CSS *plovoucí*. Máme s odrážkami seznamu kategorií se zobrazí vlevo produkty s vybranou kategorii podle plovoucí opakovače nalevo od prvku DataList

Otevřete `CategoriesAndProducts.aspx` stránku z `DataListRepeaterFiltering` složky a přidat na stránku prvku Repeater a DataList. Nastavit opakovače s `ID` k `Categories` a DataList s k `CategoryProducts`. Přejděte do zobrazení zdroje a opakovače a DataList ovládací prvky v rámci své vlastní `<div>` elementy. To znamená, uzavřete opakovače v rámci `<div>` element první a pak DataList ve svém vlastním `<div>` element přímo po opakovače. Váš kód v tomto okamžiku by měl vypadat takto:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Chcete-li uvolnit opakovače nalevo od prvku DataList, je potřeba použít `float` atribut stylu CSS, například takto:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;` Jako plovoucí první `<div>` elementu vlevo na druhou. `width` a `padding-right` nastavení udávají první `<div>` s `width` a kolik odsazení se přidá mezi `<div>` element s obsahu a jeho pravý okraj. Další informace o plovoucí elementy v jazyce CSS podívejte se [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Spíše než nastavení styl přímo přes první `<p>` element s `style` atribut, aby s místo toho vytvořte novou třídu CSS v `Styles.css` s názvem `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Potom můžete nahradit jsme `<div>` s `<div class="FloatLeft">`.

Po přidání třídy CSS a kódu značek v konfiguraci `CategoriesAndProducts.aspx` stránky, přejděte do návrháře. Měli byste vidět opakovače plovoucí na levé straně DataList (i když vpravo nyní obě právě zobrazí jako šedý polí od jsme jste zatím ke konfiguraci jejich zdroje dat nebo šablony).


[![Opakovače obtékané nalevo od prvku DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Obrázek 2**: opakovače obtékané nalevo od prvku DataList ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Krok 2: Určení počtu produktů pro každou kategorii

Opakovače a DataList s, které obaluje značek dokončení jsme re připravené k vytvoření vazby dat kategorie opakovače kontrolu. Ale jak seznamu s odrážkami kategorií obrázek 1 ukazuje, kromě každý název kategorie s také potřebujeme zobrazíte počet produktů, které jsou spojené s kategorií. Chcete-li přistupovat k těmto informacím jsme provést jeden z následujících kroků:

- **Určete tyto informace z třídy s kódu stránky ASP.NET.** Zadané konkrétní *`categoryID`* jsme můžete určit počet produktů, přidružené voláním `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda. Tato metoda vrátí hodnotu `ProductsDataTable` objekt, jehož `Count` vlastnost udává, kolik `ProductsRow` s existuje, což je počet produktů, pro zadané *`categoryID`*. Můžeme vytvořit `ItemDataBound` obslužné rutiny události pro opakovače, která pro každou kategorii vázána na opakovače, zavolá `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda i jeho počet ve výstupu.
- **Aktualizace `CategoriesDataTable` v datové sadě zadali zahrnout `NumberOfProducts` sloupce.** Jsme může aktualizovat `GetCategories()` metoda v `CategoriesDataTable` zahrnout tyto informace, nebo můžete taky nechat `GetCategories()` jako-je a vytvořte novou `CategoriesDataTable` metodu s názvem `GetCategoriesAndNumberOfProducts()`.

Umožní s prozkoumat obě z následujících postupů. Prvním přístupem je jednodušší implementace vzhledem k tomu, že jsme nejsou zobrazeny t potřeba aktualizovat Data Access Layer; to ale vyžaduje další komunikaci s databází. Volání `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda v `ItemDataBound` obslužné rutiny události přidá volání další databáze pro každou kategorii zobrazí v Opakovači. Při této technice existují *N* + volání 1 databáze, kde *N* je počet kategorií zobrazí v Opakovači. Druhý přístup, je vrácen počet produktů s informace o každé kategorie z `CategoriesBLL` třídu s `GetCategories()` (nebo `GetCategoriesAndNumberOfProducts()`) metoda, což by vedlo k jenom jednu cestu k databázi.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Určení počtu produktů v obslužná rutina události ItemDataBound

Určení počtu produktů pro každou kategorii v opakovače s `ItemDataBound` obslužné rutiny události nevyžaduje žádné úpravy naše existující Data Access Layer. Můžete provést všechny úpravy přímo v rámci `CategoriesAndProducts.aspx` stránky. Začněte přidáním nové ObjectDataSource s názvem `CategoriesDataSource` pomocí inteligentních značek opakovače s. V dalším kroku nakonfigurujte `CategoriesDataSource` ObjectDataSource, které se načte data z `CategoriesBLL` třídu s `GetCategories()` metoda.


[![Konfigurace ObjectDataSource použití třídy CategoriesBLL s GetCategories() – metoda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Obrázek 3**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` třídu s `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Každá položka v `Categories` opakovače musí být lze klepnout, a při kliknutí na, způsobit, že `CategoryProducts` DataList k zobrazení těchto produktů pro vybranou kategorii. To lze provést tak, že každou kategorii hypertextový odkaz, propojení zpět na této stránce stejné (`CategoriesAndProducts.aspx`), ale předejte `CategoryID` pomocí řetězce dotazu, jako jsme viděli v předchozí kurzu. Výhodou tohoto přístupu je, že na stránce zobrazení určité kategorie s produkty může být aktuálně přihlášeni a indexovat pomocí vyhledávacího webu.

Alternativně bychom mohli každou kategorii LinkButton, což je přístupů, které v tomto kurzu použijeme. LinkButton vykreslení v prohlížeči uživatele s jako hypertextový odkaz, ale po kliknutí na indukuje zpětné volání; na zpětné volání DataList s ObjectDataSource, je potřeba aktualizovat zobrazíte těchto produktů, které patří do vybrané kategorie. V tomto kurzu pomocí hypertextový odkaz smysl více než použití LinkButton; však může být dalších scénářích, kdy pomocí LinkButton výhodnější. Hypertextový odkaz přístup by být ideální pro tento příklad, mohli s místo prozkoumat pomocí LinkButton. Jako uvidíme, použití LinkButton zavádí některé běžné problémy, které by jinak nastat u hypertextový odkaz. Proto používat LinkButton v tomto kurzu bude zvýrazněte tyto problémy a pomáhají zajistit řešení pro tyto scénáře, kde může chceme používat LinkButton místo hypertextový odkaz.

> [!NOTE]
> Doporučujeme opakujte tento kurz pomocí ovládacího prvku hypertextový odkaz nebo `<a>` element místo LinkButton.


Následující kód ukazuje deklarativní syntaxi pro opakovače a ObjectDataSource. Všimněte si, že šablony opakovače s vykreslení seznamu s odrážkami s každou položku jako LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> V tomto kurzu opakovače musí mít svůj stav zobrazení, které jsou povolené (Všimněte si, vynechání `EnableViewState="False"` z deklarativní syntaxi opakovače s). V kroku 3 jsme budete vytvářet obslužné rutiny události pro opakovače s `ItemCommand` událostí, ve kterém budete mít aktualizujeme DataList s ObjectDataSource s `SelectParameters` kolekce. Opakovače s `ItemCommand`, ale won t ještě efektivněji, pokud je stav zobrazení zakázán. V tématu [těžkou otázku A otázky ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) a [jeho řešení](http://scottonwriting.net/sowBlog/posts/1268.aspx) Další informace o důvod, proč musí být povolen stav zobrazení pro opakovače s `ItemCommand` na aktivaci události.


LinkButton s `ID` hodnota vlastnosti `ViewCategory` nemá jeho `Text` sadu vlastností. Právě měl jsme chtěli zobrazovaný název kategorie, jsme by mít pro vlastnost Text deklarativně, prostřednictvím syntaxe vazby dat, například takto:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Ale chceme zobrazit název kategorie s *a* počet produktů, které patří do této kategorie spadají. Tyto informace lze načíst z opakovače s `ItemDataBound` obslužné rutiny události tak, že volání `ProductBLL` třídu s `GetCategoriesByProductID(categoryID)` metoda a zjištění počet záznamů, jsou vráceny v výsledná `ProductsDataTable`, jako následující kód znázorňuje:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Začneme zajištěním, který jsme re práce s datovou položku (jeden jejichž `ItemType` je `Item` nebo `AlternatingItem`) a pak odkazovat `CategoriesRow` instanci, která právě bylo svázáno se aktuální `RepeaterItem`. V dalším kroku jsme určit počet produktů pro tuto kategorii tak, že vytvoříte instanci `ProductsBLL` třídy, volání jeho `GetCategoriesByProductID(categoryID)` metoda a zjištění počet záznamů vrácených pomocí `Count` vlastnost. Nakonec `ViewCategory` LinkButton v ItemTemplate je odkazy a jeho `Text` je nastavena na *CategoryName* (*NumberOfProductsInCategory*), kde  *NumberOfProductsInCategory* je formátovaný jako číslo nulový počet desetinných míst.

> [!NOTE]
> Alternativně může přidali jsme *formátování funkce* do třídy kódu stránky s ASP.NET, která přijímá kategorie s `CategoryName` a `CategoryID` hodnoty a vrátí `CategoryName` zřetězen s počet produkty v kategorii (počítáno od volání `GetCategoriesByProductID(categoryID)` metoda). Výsledky formátování funkce může být deklarativně přiřazovány s LinkButton Text – vlastnost nahrazuje potřebu `ItemDataBound` obslužné rutiny události. Odkazovat [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) nebo [formátování DataList a opakovače dat na základě při](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) kurzy pro další informace o použití formátování funkce.


Po přidání této obslužné rutiny události, pozorně testovací stránka prostřednictvím prohlížeče. Všimněte si, jak jednotlivé kategorie je uveden v seznamu s odrážkami, zobrazí název kategorie s a počet produktů, které jsou spojené s kategorií (viz obrázek 4).


[![Zobrazí se každý kategorie s název a číslo produkty](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Obrázek 4**: Zobrazí se s každou kategorii název a číslo produkty ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualizace`CategoriesDataTable`a`CategoriesTableAdapter`zahrnout počet produktů, pro každou kategorii

Namísto určení počtu produktů pro jednotlivé kategorie, jako je vázán s opakovače, jsme zjednoduší se tento proces úpravou `CategoriesDataTable` a `CategoriesTableAdapter` v Data Access Layer nativně zahrnout tyto informace. Jak toho docílit, jsme musíte přidat nový sloupec na `CategoriesDataTable` k uložení všech přidružených produktů. Chcete-li přidat nový sloupec do DataTable, otevřete typové datové sady (`App_Code\DAL\Northwind.xsd`), klikněte pravým tlačítkem na DataTable upravit a zvolte možnost Přidat nebo sloupec. Přidat nový sloupec, který má `CategoriesDataTable` (viz obrázek 5).


[![Přidejte nový sloupec CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Obrázek 5**: Přidat nový sloupec, který má `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


To bude přidat nový sloupec s názvem `Column1`, které můžete změnit zadáním jednoduše v jiný název. Přejmenujte tento nový sloupec `NumberOfProducts`. Dále je potřeba nakonfigurovat tento sloupec s vlastností. Klikněte na nový sloupec a přejít do okna vlastností. Sloupec s změnit `DataType` vlastnost z `System.String` k `System.Int32` a nastavte `ReadOnly` vlastnost `True`, jak je znázorněno na obrázku 6.


![Nastavit vlastnosti jen pro čtení nového sloupce a datový typ](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Obrázek 6**: nastavte `DataType` a `ReadOnly` vlastnosti nového sloupce


Když `CategoriesDataTable` má nyní `NumberOfProducts` sloupce, jeho hodnota není nastaven žádný odpovídající dotazů TableAdapter s. Jsme můžete aktualizovat `GetCategories()` metoda vrátit tyto informace v případě chceme, aby tyto informace vrátila pokaždé, když je načíst informace o kategoriích. Pokud, ale musíme získat počet související produkty kategorií ve výjimečných případech (například jako pouze pro účely tohoto kurzu), pak můžete nechat jsme `GetCategories()` jako-je a vytvoření nové metody, která vrací tyto informace. Umožňují s použít tento pozdější přístup, vytvoření nové metody s názvem `GetCategoriesAndNumberOfProducts()`.

To přidat nové `GetCategoriesAndNumberOfProducts()` metoda, klikněte pravým tlačítkem na `CategoriesTableAdapter` a vyberte nový dotaz. To přináší až TableAdapter dotaz Průvodce konfigurací, které jsme sunout použít mnohokrát v předchozí kurzy. Tuto metodu která určuje, že dotaz používá příkaz SQL ad-hoc, která vrací řádky spusťte průvodce.


[![Create – metoda příkazu SQL Ad-Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Obrázek 7**: vytvoření metody, pomocí příkazu SQL Ad-Hoc ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![Příkaz jazyka SQL vrací řádky](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Obrázek 8**: řádků vrátí příkaz SQL ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


Na další obrazovce Průvodce zobrazí výzvu nám pro dotaz, který bude použit. Vrátit každou kategorii s `CategoryID`, `CategoryName`, a `Description` pole, spolu s počtem produkty spojené s kategorií, použijte následující `SELECT` příkaz:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Zadejte dotaz pro použití](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Obrázek 9**: Zadejte dotaz pro použití ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Upozorňujeme, že je poddotaz, která vypočítá počet produktů, které jsou spojené s kategorií alias jako `NumberOfProducts`. Tato pojmenování shody hodnoty vrácené tento poddotazu má být spojen s způsobí, že `CategoriesDataTable` s `NumberOfProducts` sloupce.

Po zadání tohoto dotazu je posledním krokem je vybrat název pro novou metodu. Použití `FillWithNumberOfProducts` a `GetCategoriesAndNumberOfProducts` výplně DataTable a vraťte se DataTable vzory, v uvedeném pořadí.


[![Název nové FillWithNumberOfProducts metody s TableAdapter a GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Obrázek 10**: název nové TableAdapter s metody `FillWithNumberOfProducts` a `GetCategoriesAndNumberOfProducts` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


V tomto okamžiku Data Access Layer rozšířilo a zahrnují počet produktů, na kategorie. Vzhledem k tomu, že naše prezentační vrstvy směruje všechna volání DAL prostřednictvím samostatné vrstvy obchodní logiky, je potřeba přidat odpovídající `GetCategoriesAndNumberOfProducts` metodu `CategoriesBLL` třídy:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

DAL a BLL dokončení, jsme re připravené k vytvoření vazby na dat `Categories` opakovače v `CategoriesAndProducts.aspx`! Pokud jste již ObjectDataSource vytvořeny již pro opakovače z určení počtu produktů v `ItemDataBound` části obslužné rutiny události odstranit tento ObjectDataSource a odeberte opakovače s `DataSourceID` vlastnost nastavení; také unwire Opakovače s `ItemDataBound` událost z obslužné rutiny události odebráním `Handles Categories.OnItemDataBound` syntaxe ve třídě kódu ASP.NET.

S opakovače zpět do původního stavu, přidejte nový ObjectDataSource s názvem `CategoriesDataSource` pomocí inteligentních značek opakovače s. Konfigurace ObjectDataSource používat `CategoriesBLL` třídy, ale místo nutnosti ho použít `GetCategories()` metoda, mají se používat `GetCategoriesAndNumberOfProducts()` místo (viz obrázek 11).


[![Konfigurace ObjectDataSource lze pomocí této metody GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Obrázek 11**: Konfigurace ObjectDataSource pro použití `GetCategoriesAndNumberOfProducts` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Potom aktualizujte `ItemTemplate` tak, aby LinkButton s `Text` vlastnost přiřazena deklarativně pomocí syntaxe datové vazby a zahrnuje i `CategoryName` a `NumberOfProducts` datová pole. Dokončení deklarativní opakovače a `CategoriesDataSource` ObjectDataSource postupujte podle kroků:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

Výstup poskytnutý aktualizace DAL zahrnout `NumberOfProducts` sloupec je stejná jako `ItemDataBound` přístup obslužné rutiny události (odkazuje zpět na obrázku 4 zobrazíte obrazovky snímek opakovače s názvy kategorií a počet produktů,).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Krok 3: Zobrazení vybrané kategorie s produkty

V tomto okamžiku máme `Categories` opakovače zobrazení seznamu kategorií spolu s počtem produkty v každé kategorii. Opakovače používá LinkButton pro každou kategorii, že po kliknutí na příčiny a zpětné volání, ve kterém bodu budeme potřebovat zobrazit tyto produkty pro vybrané kategorie v `CategoryProducts` DataList.

Problémy čelí nám je jak vám má DataList zobrazit právě produkty pro vybrané kategorie. V [hlavní/podrobností volitelný GridView hlavní pomocí DetailsView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) kurzu jsme viděli, jak sestavit GridView jejichž řádky může vybrat, s vybraný řádek s podrobností se zobrazí v DetailsView na stejné stránce. Rutina GridView s ObjectDataSource vrátí informace o všech produktů pomocí `ProductsBLL` s `GetProducts()` metoda při DetailsView s ObjectDataSource načíst informace o používání vybrané produktu `GetProductsByProductID(productID)` metoda. *`productID`* Hodnota parametru zadaná deklarativně tím, že přidružíte s hodnotou GridView s `SelectedValue` vlastnost. Bohužel opakovače nemá `SelectedValue` vlastnost a nemůže sloužit jako zdroj parametru.

> [!NOTE]
> Toto je jedna z těchto problémů, které se zobrazí při použití LinkButton v prvku Repeater. Měli jsme použili hypertextový odkaz předávat `CategoryID` prostřednictvím řetězec dotazu místo toho jsme může použít toto pole řetězce dotazu jako zdroj pro s hodnotu parametru.


Před jsme starat o chybějícím `SelectedValue` vlastnost opakovače, ale nechat s nejprve vytvořit vazbu DataList ObjectDataSource a zadejte jeho `ItemTemplate`.

Z inteligentních značek DataList s opt přidat nové ObjectDataSource s názvem `CategoryProductsDataSource` a nakonfigurujte ho na používání `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` metoda. Vzhledem k tomu, že DataList v tomto kurzu nabízí rozhraní, které je jen pro čtení, klidně si nastavte rozevírací seznamy v příkaz INSERT, UPDATE a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource lze pomocí metody GetProductsByCategoryID(categoryID) s ProductsBLL – třída](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Obrázek 12**: Konfigurace ObjectDataSource pro použití `ProductsBLL` třídu s `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Vzhledem k tomu `GetProductsByCategoryID(categoryID)` metoda očekává vstupní parametr (*`categoryID`*), Průvodce konfigurace zdroje dat umožňuje určit zdroj s parametr. Měl kategorie uvedené v GridView nebo DataList, jsme d parametr zdroj rozevíracím seznamu nastavte řízení a ControlID k `ID` dat ovládací prvek webu. Ale, protože chybí opakovače `SelectedValue` vlastnost nelze zadat jako parametr zdroj. Pokud zaškrtnete, zjistíte, že rozevíracího seznamu ControlID obsahuje pouze jeden prvek `ID``CategoryProducts`, `ID` z prvku DataList.

Nyní nastavena na hodnotu None parametr zdrojového rozevíracího seznamu. Budete jsme skončili prostřednictvím kódu programu přiřazení tento parametr hodnotu, pokud kategorie, kterou LinkButton po kliknutí na v Opakovači.


[![Proveďte není uveden zdroj parametru pro parametr categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Obrázek 13**: není zadán parametr zdroje *`categoryID`* parametr ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Po dokončení průvodce Konfigurace zdroje dat, Visual Studio automaticky generuje DataList s `ItemTemplate`. Nahraďte toto výchozí nastavení `ItemTemplate` se šablonou jsme použili v předchozí kurzu; také nastavit DataList s `RepeatColumns` vlastnost na hodnotu 2. Po provedení těchto změn deklarativní vaší DataList a jeho přidružené ObjectDataSource by měl vypadat následovně:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

V současné době `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parametr nikdy nastaven, tak při prohlížení stránky, se nezobrazí žádné produkty. Co je potřeba udělat je mít tento parametr hodnotu nastavit na základě `CategoryID` kliknutelnou kategorie v Opakovači. Vzniká dva problémy: nejdřív, jak jsme je zjistit při LinkButton v opakovače s `ItemTemplate` kliknutelnou; a druhé, jak jsme zjistit `CategoryID` odpovídající kategorie, jejichž LinkButton označeného?

Má LinkButton jako a ImageButton ovládacích prvků `Click` událostí a [ `Command` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Událostí slouží k jednoduše Všimněte si, že LinkButton klepnutí na. V některých případech ale kromě poznamenat, že bylo stisknuto LinkButton také potřebujeme předat některé doplňující informace k obslužné rutině událostí. Pokud ano, LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) a [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) vlastnosti lze přiřadit tyto doplňující informace. Potom po kliknutí LinkButton jeho `Command` aktivuje událost (místo jeho `Click` událostí) a obslužné rutiny události je předána hodnoty `CommandName` a `CommandArgument` vlastnosti.

Když `Command` událost se vyvolá z v rámci šablonu opakovače s opakovače [ `ItemCommand` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) aktivuje a je předána `CommandName` a `CommandArgument` hodnoty kliknutelnou LinkButton (nebo tlačítko nebo ImageButton). Proto pokud chcete zjistit, kdy bylo stisknuto kategorii LinkButton v Opakovači, potřebujeme postupujte takto:

1. Nastavte `CommandName` vlastnost LinkButton v opakovače s `ItemTemplate` na určitou hodnotu (I sunout používá ListProducts). Pomocí tohoto nastavení `CommandName` hodnoty, LinkButton s `Command` událost aktivuje se při kliknutí na LinkButton.
2. Nastavit LinkButton s `CommandArgument` vlastnost na hodnotu aktuální položky s `CategoryID`.
3. Vytvoření obslužné rutiny události pro opakovače s `ItemCommand` událostí. Události obslužnou rutinu, nastavte `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametr na hodnotu předané `CommandArgument`.

Následující `ItemTemplate` kód pro opakovače kategorií implementuje kroky 1 a 2. Poznámka: Jak `CommandArgument` hodnota je přiřazena položku dat s `CategoryID` pomocí syntaxe vazby dat:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Při každém vytvoření `ItemCommand` obslužné rutiny události je doporučeno vždy nejprve zkontrolujte příchozí `CommandName` hodnota, protože *žádné* `Command` událostí aktivováno *žádné* tlačítko, LinkButton, nebo Způsobí, že ImageButton v rámci opakovače `ItemCommand` na aktivaci události. Když jsme aktuálně k dispozici pouze jeden takový LinkButton nyní, v budoucnu jsme (nebo jiné vývojáře na náš tým) může přidání další tlačítko na opakovače, při kliknutí na, vyvolá stejné `ItemCommand` obslužné rutiny události. Proto je nejlepší vždy ujistěte se, abyste zkontrolovat s `CommandName` vlastnost a pouze pokračujte programové logiky, pokud odpovídá na hodnotu očekávanou pro.

Po zajištění, že předané `CommandName` hodnota rovná ListProducts, obslužné rutiny události poté přiřadí `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametr na hodnotu předané `CommandArgument`. Tuto změnu ObjectDataSource s `SelectParameters` automaticky způsobí, že DataList k sám rebind ke zdroji dat obsahující produkty pro nově zvolené kategorii.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

S těmito přídavky našem kurzu je dokončena. Za chvíli otestování v prohlížeči. Obrázek 14 zobrazuje obrazovky, když nejdřív na stránce. Vzhledem k tomu, že kategorii ještě má být vybrána, zobrazí se žádné produkty. Kliknutím na kategorii, například produktu, se zobrazí těchto produktů v kategorii produktu v zobrazení dvou sloupců (viz obrázek 15).


[![Žádné produkty se zobrazí při první návštěvě stránky](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Obrázek 14**: Ne produkty se zobrazí při prvním na stránce ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Kliknutím na seznamy kategorie produktů odpovídající produkty vpravo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Obrázek 15**: Kliknutím na kategorii produktu jsou uvedeny produkty odpovídající vpravo ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Souhrn

Jak jsme viděli v tomto kurzu a ten předchozí, a podrobností sestavy může být rozloženy dvě stránky, nebo konsolidovat v jednom. Zobrazení hlavní/podrobnosti sestavy na jedné stránce, ale zavádí některé běžné problémy, o tom, co nejlépe rozložení hlavní a podrobnosti záznamy na stránce. V *hlavní/podrobností volitelný GridView hlavní pomocí DetailsView podrobnosti* kurzu jsme měli podrobnosti záznamy se nad hlavní záznamy; v tomto kurzu jsme použili šablon stylů CSS techniky tak, aby měl float hlavní záznamy do nalevo od podrobnosti.

Společně s zobrazení hlavní/podrobnosti sestavy, jsme také měli možnost zjistit, jak načíst počet produktů, které jsou spojené s každou kategorii a jak provádět logiku na straně serveru, když LinkButton (nebo tlačítka nebo ImageButton) po kliknutí na uvnitř Opakovače.

V tomto kurzu dokončení naše prozkoumání a podrobností sestavy s DataList a opakovače. Naše další sadu kurzy ilustruje způsob přidání, úpravy a odstraňování možnosti do ovládacího prvku DataList.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) kurz týkající se plovoucí elementů šablon stylů CSS pomocí šablon stylů CSS
- [Umístění šablon stylů CSS](http://www.brainjar.com/css/positioning/) Další informace o umístění elementů pomocí šablon stylů CSS
- [Kterým se obsah s HTML](http://www.w3schools.com/html/html_layout.asp) pomocí `<table>` s a další elementy HTML pro umístění

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Zack Petr. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-acess-two-pages-datalist-vb.md)
