---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Master/Detail s seznam hlavních záznamů s odrážkami podrobnostmi v prvku DataList (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme budete komprimovat dvoustránkový záznamů master/detail sestavu předchozí kurz o službě do jediné stránce zobrazující seznam s odrážkami názvů kategorií v t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 52fd8d1f04b3082250d369b30b5be4db8eac738a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370403"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Master/Detail s seznam hlavních záznamů s odrážkami podrobnostmi v prvku DataList (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) nebo [stahovat PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> V tomto kurzu jsme budete komprimovat dvoustránkový záznamů master/detail sestavu předchozí kurz o službě na jednu stránku, na levé straně obrazovky a vybrané kategorie produktů na pravé straně obrazovky zobrazující seznam s odrážkami názvy kategorií.


## <a name="introduction"></a>Úvod

V [předchozím kurzu](master-detail-filtering-acess-two-pages-datalist-cs.md) jsme se podívali na tom, jak oddělit sestavy záznamů master/detail na dvou stránkách. Na stránce předlohy jsme použili ovládacím prvkem Repeater k vykreslení seznam s odrážkami kategorií. Každý název kategorie byl hypertextový odkaz, že po kliknutí na by take uživatele na stránku podrobností, kde a dva sloupce v prvku DataList jsme si ukázali, tyto produkty patří do vybrané kategorie.

V tomto kurzu jsme budete komprimovat kurzu dvě stránky do jediné stránce zobrazující seznam s odrážkami názvy kategorií s názvy jednotlivých kategorií se vykresluje jako odkazem (LinkButton) na levé straně obrazovky. Klikněte na název kategorie LinkButtons indukuje zpětné volání a sváže s produkty s vybranou kategorii a dva sloupce v prvku DataList na pravé straně obrazovky. Kromě zobrazení každou kategorii s názvem, Repeater na levé straně se zobrazí, kolik existuje celkový počet produktů pro danou kategorii (viz obrázek 1).


[![Kategorie s názvem a celkový počet produktů, které se zobrazí na levé straně](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Obrázek 1**: na levé straně se zobrazí kategorii s názvem a celkový počet produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Krok 1: Zobrazení Repeateru v levé části obrazovky

Pro účely tohoto kurzu potřebujeme mít seznamu s odrážkami kategorií se zobrazí nalevo od produktů s vybranou kategorii. Obsah na webové stránce může být umístěné pomocí standardní značky HTML pro elementy odstavce, pevné mezery `<table>` s a tak dále, nebo prostřednictvím techniky kaskádových stylů (CSS). Všechny naše kurzy doposud použili šablony stylů CSS techniky pro umístění. Když jsme vytvořili navigační uživatelské rozhraní v naší hlavní stránky v [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-cs.md) kurzu jsme použili *absolutní pozici*, určující přesný pixel posunutí pro navigaci seznam a hlavním obsahem. Případně, šablon stylů CSS je možné umístit jeden element doprava nebo doleva jiného prostřednictvím *s plovoucí desetinnou čárkou*. Máme seznamu s odrážkami kategorií podle plovoucí Repeater nalevo od prvku DataList se zobrazí nalevo od produktů s vybranou kategorii.

Otevřít `CategoriesAndProducts.aspx` stránku ze `DataListRepeaterFiltering` složky a přidat na stránku Repeateru a a v prvku DataList. Nastavit opakovače s `ID` k `Categories` a ovládacích prvků DataList s k `CategoryProducts`. Přejděte do zobrazení zdroje a umístit ovládací prvky Repeater a ovládacích prvků DataList v rámci své vlastní `<div>` elementy. To znamená, uzavřete Repeater v rámci `<div>` elementu první a pak DataList ve vlastním `<div>` elementu přímo po opakovače. Váš kód v tomto okamžiku by měl vypadat nějak takto:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Uvolnění Repeater nalevo od prvku DataList, musíme použít `float` atribut stylu CSS, například takto:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;` Čísel s plovoucí čárkou první `<div>` element doleva je druhý řádek. `width` a `padding-right` nastavení označují první `<div>` s `width` a kolik odsazení se přidá mezi `<div>` elementu s obsahem a jeho pravého okraje. Další informace o plovoucí elementy v jazyce CSS podívejte [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Místo určení stylu nastavení přímo prostřednictvím první `<p>` element s `style` atribut, ať s místo toho vytvořte novou třídu šablony stylů CSS v `Styles.css` s názvem `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Pak nahradíme můžete `<div>` s `<div class="FloatLeft">`.

Po přidání třídu šablony stylů CSS a nakonfigurování značky `CategoriesAndProducts.aspx` stránky, přejděte do návrháře. Měli byste vidět opakovače s plovoucí čárkou nalevo od prvku DataList (i když pravé teď oba právě objeví jako šedý polí od jsme ve ještě ke konfiguraci jejich zdroje dat nebo šablony).


[![Opakovače je ponechán v neurčitém stavu nalevo od prvku DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Obrázek 2**: The Repeater je ponechán v neurčitém stavu nalevo od prvku DataList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Krok 2: Určení počtu produktů pro každou kategorii

S Repeater a ovládacích prvků DataList s okolní kompletní kód můžeme znovu připravený k vytvoření vazby data v kategoriích Opakovači řízení. Ale jak seznam s odrážkami kategorií obrázek 1 ukazuje, kromě každé kategorie s názvem musíme také zobrazí počet produktů, které jsou spojené s kategorií. K těmto informacím přistupovat můžeme buď:

- **Určete tyto informace z třídy modelu code-behind s stránky technologie ASP.NET.** Daný konkrétní *`categoryID`* můžeme určit počet související produkty voláním `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metody. Tato metoda vrátí hodnotu `ProductsDataTable` jehož `Count` vlastnost určuje, kolik `ProductsRow` s existuje, což je počet produktů pro zadaný rozbočovač *`categoryID`*. Vytvoříme `ItemDataBound` obslužné rutiny události pro Repeater, která pro každou kategorii vázán na Repeater, volá `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metoda a zahrnuje její vlastnosti count ve výstupu.
- **Aktualizace `CategoriesDataTable` v datové sadě zadán zahrnout `NumberOfProducts` sloupce.** Můžete pak aktualizujte `GetCategories()` metoda ve `CategoriesDataTable` zahrnout tyto informace nebo, nebo můžete parametr `GetCategories()` jako-se a vytvořte nový `CategoriesDataTable` metodu s názvem `GetCategoriesAndNumberOfProducts()`.

Umožní s prozkoumat obě z následujících postupů. Prvním přístupem je snazší implementovat, protože jsme zadávat t je potřeba aktualizovat vrstvy přístupu k datům; to ale vyžaduje další komunikaci s databází. Volání `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metodu `ItemDataBound` obslužná rutina události přidá volání rozhraní navíc databáze pro každou kategorii, zobrazí v Opakovači. Při této technice je *N* + 1 databáze volání, kde *N* je počet kategorií, které jsou zobrazeny v Opakovači. S druhým přístupem, je vrácen počet produktů s informace o jednotlivých kategorií z `CategoriesBLL` třída s `GetCategories()` (nebo `GetCategoriesAndNumberOfProducts()`) metody, což by vedlo k jenom jednu cestu k databázi.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Určení počtu produktů v obslužné rutině události ItemDataBound

Určení počtu produktů pro každou kategorii v opakovače s `ItemDataBound` obslužná rutina události nevyžaduje žádné změny k naší stávající vrstvy přístupu k datům. Všechny změny lze provést přímo v rámci `CategoriesAndProducts.aspx` stránky. Začněte přidáním nového prvku ObjectDataSource s názvem `CategoriesDataSource` prostřednictvím inteligentních značek opakovače s. V dalším kroku nakonfigurujte `CategoriesDataSource` prvek ObjectDataSource, takže se načte data z `CategoriesBLL` třída s `GetCategories()` metody.


[![Konfigurace ObjectDataSource pomocí třídy CategoriesBLL s GetCategories() – metoda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Obrázek 3**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třída s `GetCategories()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Každá položka v `Categories` Repeater musí být možné kliknout a po kliknutí na způsobit, že `CategoryProducts` DataList pro zobrazení těchto produktů pro vybranou kategorii. Toho můžete docílit tak, že každá kategorie hypertextový odkaz, propojení zpět na tuto stránku stejné (`CategoriesAndProducts.aspx`), ale předejte `CategoryID` pomocí řetězce dotazu, stejně jako jsme viděli v předchozím kurzu. Výhodou tohoto přístupu je, že stránka se zobrazenou zprávou určité kategorie s produkty můžete záložek a indexovat pomocí vyhledávacího webu.

Můžete také můžeme provádět jednotlivých kategorií odkazem (LinkButton), který je použitý přístup pro účely tohoto kurzu použijeme. Na prvek LinkButton se vykreslí v prohlížeči uživatele s jako hypertextový odkaz, ale po kliknutí na indukuje zpětné volání; na zpětné volání DataList s ObjectDataSource musí aktualizovat na zobrazení těchto produktů, které patří do vybrané kategorie. V tomto kurzu pomocí hypertextový odkaz vhodnější než při použití odkazem (LinkButton); Nicméně mohou existovat další scénáře, kde je výhodnější používat odkazem (LinkButton). Při přístupu hypertextový odkaz by byla ideální pro účely tohoto příkladu, umožní prozkoumat místo toho použití na prvek LinkButton s. Jak uvidíme, pomocí odkazem (LinkButton) zavádí některé běžné problémy, které by jinak vznikají s hypertextovým odkazem. Proto používat odkazem (LinkButton) v tomto kurzu se zvýrazněte tyto výzvy a poskytnout řešení pro tyto scénáře, kde budeme chtít použít odkazem (LinkButton) namísto hypertextový odkaz.

> [!NOTE]
> Doporučujeme opakování v tomto kurzu pomocí ovládacího prvku hypertextový odkaz nebo `<a>` elementu namísto odkazem (LinkButton).


Následující kód ukazuje Opakovači a ObjectDataSource deklarativní syntaxe. Všimněte si, že šablony opakovače s vykreslení seznam s odrážkami s každou položku jako odkazem (LinkButton):


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> Pro účely tohoto kurzu musíte mít Opakovači svůj stav zobrazení povolený (Všimněte si, vynechání `EnableViewState="False"` z deklarativní syntaxe opakovače s). V kroku 3 jsme budete vytvářet obslužné rutiny události pro opakovače s `ItemCommand` událostí, ve kterém budeme aktualizovat DataList s ObjectDataSource s `SelectParameters` kolekce. Opakovače s `ItemCommand`, ale vyhráli t fire, pokud stav zobrazení je zakázán. Naleznete v tématu [těžkou otázku A otázky ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) a [svoje řešení](http://scottonwriting.net/sowBlog/posts/1268.aspx) Další informace o důvod, proč musí být pro opakovače s povolen stav zobrazení `ItemCommand` události, která se aktivuje.


Na prvek LinkButton s `ID` hodnotou vlastnosti `ViewCategory` nemá jeho `Text` sadu vlastností. Právě jsme měli chtěli zobrazovat název kategorie, jsme by jste nastavili vlastnost Text deklarativně pomocí syntaxe vázání dat, například takto:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Však chceme zobrazit kategorii s názvem *a* počet produktů, které patří do této kategorie spadají. Tyto informace můžete získat z opakovače s `ItemDataBound` obslužná rutina události tím, že zavoláte na `ProductBLL` třída s `GetCategoriesByProductID(categoryID)` metoda a určení, kolik záznamů se vrátí ve výsledné `ProductsDataTable`, jako je následující kód ukazuje:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Začínáme tím, že zajišťuje, že jsme k práci s datovou položku (jeden jehož `ItemType` je `Item` nebo `AlternatingItem`) a pak odkazovat `CategoriesRow` instanci, která se právě byla svázána se aktuální `RepeaterItem`. Dále jsme určit počet produktů pro tuto kategorii tak, že vytvoříte instanci `ProductsBLL` třídy volání jeho `GetCategoriesByProductID(categoryID)` metoda a určení počtu vrácených pomocí záznamů `Count` vlastnost. Nakonec `ViewCategory` odkazem (LinkButton) ve vlastnosti ItemTemplate, je odkazy a jeho `Text` je nastavena na *CategoryName* (*NumberOfProductsInCategory*), kde  *NumberOfProductsInCategory* formátovaný jako číslo s nulovou desetinných míst.

> [!NOTE]
> Alternativně může přidali jsme *formátování funkce* do třídy použití modelu code-behind stránky s ASP.NET, která přijímá kategorie s `CategoryName` a `CategoryID` hodnoty a vrátí `CategoryName` spojeny počtem produkty v kategorii (počítáno od volání `GetCategoriesByProductID(categoryID)` metoda). Výsledky formátování funkce může být deklarativně přiřazen s odkazem (LinkButton) nahrazuje potřebu vlastnost Text `ItemDataBound` obslužné rutiny události. Odkazovat [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) nebo [formátování ovládacích prvků DataList a Repeater na základě na Data](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) kurzy pro další informace o použití funkce formátování.


Po přidání této obslužné rutiny události, věnujte chvíli testovací stránka prostřednictvím prohlížeče. Všimněte si, jak je každá kategorie uvedené v seznamu s odrážkami, zobrazuje kategorii s názvem a počet produktů, které jsou spojené s kategorií (viz obrázek 4).


[![Zobrazené každou kategorii s názvem a počet produktů](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Obrázek 4**: zobrazené každou kategorii s názvem a číslem produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualizuje`CategoriesDataTable`a`CategoriesTableAdapter`zahrnout počet produktů pro každou kategorii

Místo určení počtu produktů pro každou kategorii, protože s vázán na Repeater, můžeme zjednodušit tento proces úpravou `CategoriesDataTable` a `CategoriesTableAdapter` v vrstvy přístupu k datům nativně obsahovala tuto informaci. K dosažení tohoto cíle, jsme musíte přidat nový sloupec, `CategoriesDataTable` pro uchování počtu související produkty. Chcete-li přidat nový sloupec do DataTable určitého, otevřete datovou sadu typu (`App_Code\DAL\Northwind.xsd`), klikněte pravým tlačítkem na objekt DataTable upravit a zvolte Přidat / sloupec. Přidat nový sloupec, `CategoriesDataTable` (viz obrázek 5).


[![Přidat nový sloupec CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Obrázek 5**: přidejte nový sloupec, `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Tato možnost přidá nový sloupec s názvem `Column1`, které můžete změnit tak, že jednoduše zadáte jiný název. Přejmenujte tento nový sloupec na `NumberOfProducts`. V dalším kroku potřeba nakonfigurovat tento sloupec s vlastností. Klikněte na nový sloupec a přejděte do okna Vlastnosti. Změna sloupce s `DataType` vlastnost z `System.String` k `System.Int32` a nastavit `ReadOnly` vlastnost `True`, jak je znázorněno na obrázku 6.


![Nastavte datový typ a vlastnosti jen pro čtení nového sloupce](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Obrázek 6**: nastavte `DataType` a `ReadOnly` vlastnosti nového sloupce


Zatímco `CategoriesDataTable` má teď `NumberOfProducts` sloupec, jeho hodnota není nastavená žádné odpovídající s dotazů TableAdapter. Abychom mohli aktualizovat `GetCategories()` metoda tyto informace mají vracet pokud chceme, aby tyto údaje o vrátila pokaždé, když se načte informace o kategoriích. Pokud, ale musíme vzít číslo související produkty pro kategorie ve výjimečných případech (například jako jen pro účely tohoto kurzu), pak můžete necháme `GetCategories()` jako-je a vytvoření nové metody, která vrací tyto informace. Umožňují s použít tento druhý přístup vytváří novou metodu s názvem `GetCategoriesAndNumberOfProducts()`.

Přidat toto nové `GetCategoriesAndNumberOfProducts()` metoda, klikněte pravým tlačítkem na `CategoriesTableAdapter` a zvolit nový dotaz. To přináší nahoru TableAdapter dotazovat Průvodce konfigurací, které jsme ve použít mnohokrát v předchozích kurzech. Pro tuto metodu spusťte Průvodce označující, že dotaz používá ad-hoc příkazu SQL, který vrací řádky.


[![Vytvořit metodu, pomocí příkazu SQL Ad-Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Obrázek 7**: vytvoření metody, pomocí příkazu SQL Ad-Hoc ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![Příkaz jazyka SQL, vrátí řádky](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Obrázek 8**: SQL příkaz vrátí Rows ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


Na další obrazovce průvodce vyzve k nám na dotaz, který chcete použít. Vrátit s každou kategorii `CategoryID`, `CategoryName`, a `Description` pole, spolu s počtem produkty související s kategorií, použijte následující `SELECT` – příkaz:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Zadejte dotaz k použití](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Obrázek 9**: Zadejte dotaz, který použít ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Všimněte si, že poddotazu, která vypočítá počet produktů, které jsou spojené s kategorií s aliasem jako `NumberOfProducts`. Tuto pojmenování shodu způsobí, že hodnoty vrácené tomto poddotazu přidruženo `CategoriesDataTable` s `NumberOfProducts` sloupce.

Po zadání tohoto dotazu je posledním krokem je vybrat název pro novou metodu. Použití `FillWithNumberOfProducts` a `GetCategoriesAndNumberOfProducts` zaplní, datové tabulky a vrátit objekt DataTable vzory, v uvedeném pořadí.


[![Název nové FillWithNumberOfProducts metody s TableAdapter a GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Obrázek 10**: název nového TableAdapter s metod `FillWithNumberOfProducts` a `GetCategoriesAndNumberOfProducts` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


V tomto okamžiku vrstvy přístupu k datům rozšířilo a zahrnuje počet produktů podle kategorie. Protože všechny naše prezentační vrstva směruje všechna volání DAL prostřednictvím samostatných vrstvy obchodní logiky je potřeba přidat odpovídající `GetCategoriesAndNumberOfProducts` metodu `CategoriesBLL` třídy:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

DAL a BLL kompletní, můžeme znovu jste připravení začít tato data k vytvoření vazby `Categories` Repeater v `CategoriesAndProducts.aspx`! Pokud jste již prvku ObjectDataSource vytvořeny již pro Repeater z určení číslo produkty v `ItemDataBound` části obslužná rutina události, odstraňte tento prvek ObjectDataSource a odeberete opakovače s `DataSourceID` vlastnost nastavení; také unwire Opakovače s `ItemDataBound` událost z obslužné rutiny události tak, že odeberete `Handles Categories.OnItemDataBound` syntaxe v třídě modelu code-behind technologie ASP.NET.

Repeater zpět do původního stavu, přidejte nový prvek ObjectDataSource s názvem `CategoriesDataSource` prostřednictvím inteligentních značek opakovače s. Konfigurace ObjectDataSource používat `CategoriesBLL` třídy, ale namísto toho, aby ji použít `GetCategories()` metodu, mají se používat `GetCategoriesAndNumberOfProducts()` místo toho (viz obrázek 11).


[![Konfigurace ObjectDataSource GetCategoriesAndNumberOfProducts metody](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Obrázek 11**: Konfigurace ObjectDataSource k použití `GetCategoriesAndNumberOfProducts` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Dále, aktualizujte `ItemTemplate` tak, aby s odkazem (LinkButton) `Text` vlastnost přiřazena deklarativně pomocí syntaxe pro vázání dat a zahrnuje i `CategoryName` a `NumberOfProducts` datová pole. Kompletní deklarativní opakovače a `CategoriesDataSource` ObjectDataSource následující:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

Výstup vykreslen metodou aktualizace DAL zahrnout `NumberOfProducts` sloupec je stejné jako při použití `ItemDataBound` přístup obslužné rutiny události (odkazuje zpět na obrázku 4 zobrazíte na obrazovce snímek Repeater zobrazují názvy kategorií a počet produktů).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Krok 3: Zobrazení produkty s vybranou kategorii.

V tuto chvíli máme `Categories` Repeater zobrazení seznamu kategorií spolu s počtem produkty v jednotlivých kategoriích. Opakovače odkazem (LinkButton) používá pro každou kategorii, že po kliknutí na způsobí zpětné volání, ve kterém bodu budeme potřebovat zobrazit tyto produkty pro vybrané kategorie v `CategoryProducts` DataList.

Jedinou nevýhodu směřující nám je jak DataList zobrazí jenom produkty pro vybrané kategorie. V [Master/Detail pomocí volitelných GridView hlavní podrobnosti DetailsView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurzu jsme viděli, jak sestavit jejíž řádky GridView může vybrat, s vybraný řádek s podrobně popisuje zobrazovaná v prvku DetailsView na stejné stránce. GridView s ObjectDataSource vrátí informace o všech produktů pomocí `ProductsBLL` s `GetProducts()` metoda při prvek DetailsView s ObjectDataSource načíst informace o používání produktu `GetProductsByProductID(productID)` metody. *`productID`* Hodnota parametru poskytla pomocí deklarace přidružení s hodnotou GridView s `SelectedValue` vlastnost. Bohužel nemá opakovače `SelectedValue` vlastnost a nemůže sloužit jako zdroj parametru.

> [!NOTE]
> Toto je jeden z těchto problémů, které se zobrazí při použití na prvek LinkButton v Repeateru. Měli jsme použili hypertextový odkaz a zajistěte tak předání `CategoryID` prostřednictvím řetězec dotazu namísto toho jsme použít toto pole řetězce dotazu jako zdroj pro hodnotu parametru s.


Předtím, než jsme starat o nedostatku `SelectedValue` vlastnost Repeater, ale nechat s nejdřív svázat ObjectDataSource prvku DataList a určit jeho `ItemTemplate`.

Z inteligentních značek v prvku DataList s optimalizované pro přidání nového prvku ObjectDataSource s názvem `CategoryProductsDataSource` a nakonfigurujte ho na použití `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` metody. Protože DataList v tomto kurzu nabízí rozhraní jen pro čtení, můžete v INSERT, UPDATE, nastavte rozevírací seznamy a odstranit karty na (žádný).


[![Konfigurace ObjectDataSource ProductsBLL třídy s GetProductsByCategoryID(categoryID) metody](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Obrázek 12**: Konfigurace ObjectDataSource použití `ProductsBLL` třída s `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Protože `GetProductsByCategoryID(categoryID)` metoda očekává, že vstupní parametr (*`categoryID`*), průvodce Konfigurovat zdroj dat umožňuje zadat zdroj s parametrem. Měl kategorie uvedené v GridView nebo a v prvku DataList, d nastavíme parametr zdroj rozevíracího seznamu na ovládací prvek a ControlID k `ID` dat webový ovládací prvek. Ale od chybí opakovače `SelectedValue` vlastnost nelze použít jako zdroj parametru. Pokud zaškrtnete, zjistíte, že ControlID – rozevírací seznam obsahuje pouze jeden ovládací prvek `ID``CategoryProducts`, `ID` z prvku DataList.

Nyní nastavena na hodnotu None rozevíracího seznamu zdroje parametru. Budeme mít programově přiřazení tohoto parametru na hodnotu při kategorii, kterou dojde ke kliknutí na prvek LinkButton v Opakovači.


[![Proveďte není zadán parametr zdroj pro ID kategorie parametr](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Obrázek 13**: není zadán parametr zdroj *`categoryID`* parametr ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


Po dokončení Průvodce nakonfigurovat zdroj dat, sada Visual Studio automaticky vygeneruje DataList s `ItemTemplate`. Nahradit toto výchozí nastavení `ItemTemplate` pomocí šablony jsme použili v předchozím kurzu; také nastavit DataList s `RepeatColumns` vlastnost na 2. Po provedení těchto změn deklarativní třídy DataList a její přidružené ObjectDataSource by měl vypadat nějak takto:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

V současné době `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parametr nikdy nastaven, aby žádné produkty se zobrazí při zobrazení stránky. Co musíme udělat je mít tento parametr hodnotu nastavit na základě `CategoryID` kliknutí na kategorie v Opakovači. Zavádí se dva problémy: nejprve, jak můžeme zjistit při odkazem (LinkButton) v opakovače s `ItemTemplate` kliknutí; a druhý, jak můžeme určit `CategoryID` odpovídající kategorie, jehož odkazem (LinkButton) došlo ke kliknutí na?

Má odkazem (LinkButton), jako jsou ovládací prvky tlačítka a ImageButton `Click` událostí a [ `Command` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Událostí umožňuje jednoduše mějte na paměti, že se kliklo odkazem (LinkButton). V některých případech však kromě poznamenat, že se kliklo na prvek LinkButton musíme také předat určité další informace do obslužné rutiny události. Pokud se jedná o případ, s odkazem (LinkButton) [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) a [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) vlastnosti je možné přiřadit tyto dodatečné informace. Pak při kliknutí na prvek LinkButton jeho `Command` dojde k aktivaci události (místo jeho `Click` událostí) a obslužná rutina události je předána hodnoty `CommandName` a `CommandArgument` vlastnosti.

Když `Command` událost je vyvolána v rámci šablony v Opakovači opakovače s [ `ItemCommand` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) aktivuje se a je předán `CommandName` a `CommandArgument` hodnoty na kliknutí na prvek LinkButton (nebo tlačítko nebo ImageButton). Proto pokud chcete zjistit, kdy se kategorie odkazem (LinkButton) v Opakovači kliklo, musíme udělat následující:

1. Nastavit `CommandName` vlastnost odkazem (LinkButton) v opakovače s `ItemTemplate` na některá z hodnot (I ve použít ListProducts). Když tuto `CommandName` hodnoty s odkazem (LinkButton) `Command` událost aktivuje se při kliknutí na prvek LinkButton.
2. Nastavení s odkazem (LinkButton) `CommandArgument` vlastnost na hodnotu s aktuální položkou `CategoryID`.
3. Vytvořte obslužnou rutinu události pro opakovače s `ItemCommand` událostí. V případě nastavena obslužná rutina, `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametr na hodnotu předaný `CommandArgument`.

Následující `ItemTemplate` značky kategorií Repeater implementuje kroky 1 a 2. Poznámka: Jak `CommandArgument` hodnota přiřazena datová položka s `CategoryID` pomocí syntaxe datové vazby:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Pokaždé, když se vytváří `ItemCommand` obslužná rutina události je vhodné vždy nejprve zkontrolujte příchozí `CommandName` hodnotu, protože *žádné* `Command` události vyvolané *žádné* tlačítko, odkazem (LinkButton), nebo Způsobí, že ImageButton v Opakovači `ItemCommand` události, která se aktivuje. Zatímco aktuálně pouze máme jeden takový odkazem (LinkButton), v budoucnu nám (nebo jiným vývojářem na náš tým) může přidat další tlačítka webové ovládací prvky do opakovače, po kliknutí na vyvolá stejné `ItemCommand` obslužné rutiny události. Proto se s nejlepší vždy ujistěte se, že zkontrolujete `CommandName` vlastnost a pouze pokračujte programovou logiku, pokud to odpovídá hodnotě očekávané.

Až se ujistíte, předaný `CommandName` hodnota se rovná ListProducts, obslužná rutina události poté přiřadí `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametr na hodnotu předaným `CommandArgument`. Tato změna do prvku ObjectDataSource s `SelectParameters` automaticky způsobí, že DataList samotné znovu připojit ke zdroji dat, obsahující produkty pro nově vybranou kategorii.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

S těmito přídavky v našem kurzu byla dokončena. Za chvíli otestování v prohlížeči. Obrázek 14 při první návštěvě stránky se zobrazí na obrazovce. Protože kategorie je ještě nutné vybrat, zobrazí se žádné produkty. Kliknutím na kategorii, jako je například produktu, se zobrazí tyto produkty v kategorii produktu v zobrazení dvou sloupců (viz obrázek 15).


[![Žádné produkty, které jsou zobrazeny při první návštěvě stránky](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Obrázek 14**: žádné produkty, které jsou zobrazeny při první návštěvě stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Kliknutím na seznamy kategorie produktů odpovídající produkty vpravo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Obrázek 15**: Kliknutím na kategorii produktu seznamy produktů, na odpovídající vpravo ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Souhrn

Jak jsme viděli v tomto kurzu a ta předchozí, záznamů master/detail sestavy mohou být umístěny na dvou stránkách nebo konsolidovat v jeden. Na jednu stránku zobrazení hlavních/podrobných sestav, však zavádí některé běžné problémy, o tom, co nejlépe rozložení hlavní a zaznamenává informace na stránce. V *Master/Detail pomocí volitelných GridView hlavní podrobnosti DetailsView* kurzu jsme měli podrobnosti záznamy zobrazovat nad hlavní záznamy; v tomto kurzu mívali jsme techniky šablon stylů CSS float hlavní záznamy na nalevo od podrobnosti.

Spolu s zobrazení hlavních/podrobných sestav, jsme také příležitost prozkoumat, jak načíst počet produktů, které jsou spojené s každou kategorii také, jak k provádění logiku na straně serveru, když odkazem (LinkButton) (nebo tlačítko nebo ImageButton) dojde ke kliknutí na zevnitř Opakovače.

V tomto kurzu dokončíte naše zkoumání záznamů master/detail sestavy ovládacími prvky DataList a Repeater. Naše další sérii kurzů popisuje postup přidání, úpravy a odstranění funkce pro ovládací prvek DataList.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) kurz týkající se plovoucí prvky šablon stylů CSS pomocí šablon stylů CSS
- [Umístění šablon stylů CSS](http://www.brainjar.com/css/positioning/) Další informace o umístění prvků pomocí šablon stylů CSS
- [Vytváření si obsahu HTML](http://www.w3schools.com/html/html_layout.asp) pomocí `<table>` s a dalších prvků HTML pro umístění

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Zack Jones. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [další](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
