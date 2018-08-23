---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Ovládacím prvkem DropDownList (C#) filtrování záznamů Master/Detail | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme zjistit, jak zobrazení záznamů master/detail sestav v jedné webové stránky s využitím DropDownLists zobrazíte "hlavní" záznamy a a v prvku DataList s upozorněním...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 13627c502d00ee67aeb873a6a4a58e3d74377fba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754388"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Ovládacím prvkem DropDownList (C#) filtrování záznamů Master/Detail
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) nebo [stahovat PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> V tomto kurzu jsme naleznete v části zobrazení sestav záznamů master/detail v jedné webové stránky s využitím DropDownLists zobrazíte DataList zobrazíte "details" a "hlavní" záznamů.


## <a name="introduction"></a>Úvod

Sestava hlavních záznamů/podrobností, která nejprve vytvořené pomocí GridView v předchozím [filtrování záznamů Master/Detail s DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kurzu začíná tím, že zobrazuje některé sady záznamů "hlavní". Uživatel pak přejít ke jeden z hlavních záznamů, a tím zobrazení hlavního záznamu "podrobností." Záznamů master/detail sestavy jsou ideálním řešením pro vizualizaci vztahů 1 n a pro zobrazení podrobných informací z zejména "širokých" tabulek (těch, které mají velký počet sloupců). Jak implementovat záznamů master/detail sestavy, které používají ovládací prvky GridView a DetailsView v předchozích kurzech jste Prozkoumali jsme. V tomto kurzu a další dva bude nám prozkoumat tyto koncepty, ale zaměření na používání ovládacích prvků DataList a Repeater řídí místo toho.

V tomto kurzu se podíváme na použití DropDownList obsahuje záznamy "hlavní", "Podrobnosti" záznamy zobrazeny v a v prvku DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Krok 1: Přidání záznamů Master/Detail kurz webových stránek

Než začneme v tomto kurzu, nejprve věnujte chvíli přidat složky a stránek ASP.NET, kterou potřebujeme pro tento kurz a další dva pracující se sestavami záznamů master/detail pomocí ovládacích prvků DataList a Repeater. Začněte tím, že vytvoříte novou složku v projektu s názvem `DataListRepeaterFiltering`. Dále přidejte následující pět stránek ASP.NET do této složky s všechny z nich nakonfigurovat tak, aby na hlavní stránce `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Vytvořte složku DataListRepeaterFiltering a přidávání stránek kurz ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Obrázek 1**: vytvoření `DataListRepeaterFiltering` složky a přidávání stránek kurz ASP.NET


Dále otevřete `Default.aspx` stránku a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku od `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-cs.md) kurzu mapy webu a zobrazí výčet kurzy z aktuálního oddílu v seznamu s odrážkami.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Pokud chcete zobrazit seznam s odrážkami záznamů master/detail kurzy, které nám budete vytvářet, potřebujeme přidat je do mapy webu. Otevřít `Web.sitemap` a přidejte následující kód za kód uzel mapy webu "Zobrazení dat s ovládacích prvcích DataList a Repeater":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Krok 2: Zobrazení kategorií v DropDownList

Naší sestavy záznamů master/detail zobrazí seznam kategorií v DropDownList, s produkty položku vybraného seznamu zobrazí další dolů na stránce v a v prvku DataList. První úkol náskok před nám, pak je kategorie zobrazuje v DropDownList. Začněte otevřením `FilterByDropDownList.aspx` stránku `DataListRepeaterFiltering` složky a DropDownList přetáhněte z panelu nástrojů do návrháře na stránce. Dále nastavte DropDownList `ID` vlastnost `Categories`. Klikněte na odkaz zvolit zdroj dat z inteligentních značek DropDownList a vytvoření nového prvku ObjectDataSource s názvem `CategoriesDataSource`.


[![Přidat nový prvek ObjectDataSource s názvem CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Obrázek 4**: přidejte nový prvek ObjectDataSource s názvem `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Konfigurace nového prvku ObjectDataSource tak, že ji vyvolá `CategoriesBLL` třídy `GetCategories()` metody. Po dokončení konfigurace prvek ObjectDataSource, musíme určit, jaké pole zdroje dat mají být zobrazeny v DropDownList a které jedna by měla být přiřazen jako hodnota pro každou položku seznamu. Máte `CategoryName` pole jako zobrazení a `CategoryID` jako hodnotu pro každou položku seznamu.


[![Mít zobrazení DropDownList CategoryName pole a CategoryID použijte jako hodnotu](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Obrázek 5**: Zobrazit DropDownList `CategoryName` pole a použití `CategoryID` jako hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


V tuto chvíli máme, který je naplněn záznamy z ovládací prvek DropDownList `Categories` tabulky (vše lze provést během přibližně šest sekund). Obrázek 6 doposud zobrazuje náš postup při prohlížení prostřednictvím prohlížeče.


[![Rozevírací seznam aktuálních kategorií](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Obrázek 6**: A rozevírací seznam aktuálních kategorií ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Krok 2: Přidání ovládacích prvků DataList produkty

Posledním krokem v naší sestavy záznamů master/detail je seznam produktů spojené s vybranou kategorii. K tomu přidat na stránku a v prvku DataList a vytvoření nového prvku ObjectDataSource s názvem `ProductsByCategoryDataSource`. Máte `ProductsByCategoryDataSource` načítat data z ovládacího prvku `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda. Tato sestava hlavních záznamů/podrobností je jen pro čtení, zvolte možnost (žádná) na kartách INSERT, UPDATE a DELETE.


[![Vyberte metodu GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Obrázek 7**: vyberte `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Po kliknutí na tlačítko Další, Průvodce ObjectDataSource nám vyzve k zadání hodnoty pro zdroj `GetProductsByCategoryID(categoryID)` metody *`categoryID`* parametru. Chcete použít hodnotu vybraného `categories` DropDownList položka nastavena na ovládací prvek a ControlID na zdroji parametru `Categories`.


[![Nastavit ID kategorie parametr na hodnotu DropDownList kategorie](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Obrázek 8**: nastavte *`categoryID`* parametr na hodnotu `Categories` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Po dokončení průvodce bude konfigurace zdroje dat, bude automaticky generovat sady Visual Studio `ItemTemplate` pro DataList, která zobrazuje název a hodnotu každé datové pole. Můžeme vylepšit DataList místo toho použít `ItemTemplate` , která zobrazuje pouze název produktu, kategorie, Dodavatel, množství na jednotku a cena spolu s `SeparatorTemplate` , který vkládá `<hr>` element mezi jednotlivými položkami. Teď budu používat `ItemTemplate` z příkladu v [zobrazení dat ovládacími prvky DataList a Repeater ovládací prvky](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) kurzu, ale teď můžete použít jakýkoli kód šablony najdete nejvíce vizuálně přitažlivé.

Po provedení těchto změn, by měla vypadat podobně jako následující třídy DataList a jeho ObjectDataSource značky:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Podívejte se na náš postup v prohlížeči chvíli trvat. Při první návštěvě stránky, tyto produkty, které patří do vybrané kategorie (nápoje) se zobrazí (jak je znázorněno na obrázku 9), ale změna DropDownList neaktualizuje data. Je to proto zpětné volání, musí dojít k prvku DataList aktualizovat. K tomu můžeme buď nastavte DropDownList `AutoPostBack` vlastnost `true` nebo přidání ovládacího prvku tlačítko Web na stránku. Pro účely tohoto kurzu, můžu nastavit DropDownList nepřejete `AutoPostBack` vlastnost `true`.

Obrázky 9 a 10 ukazují sestavu záznamů master/detail v akci.


[![Při první návštěvě stránky, se zobrazují produkty nápoje](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Obrázek 9**: při první návštěvě stránky, produkty nápoje zobrazují ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Výběr nového produktu (produkty) automaticky vyvolá zpětné volání, aktualizace prvku DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Obrázek 10**: Výběr nového produktu (produkty) automaticky vyvolá zpětné volání, aktualizace prvku DataList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Přidání položky seznamu "--zvolit některou kategorii--"

Při první návštěvě `FilterByDropDownList.aspx` stránce kategorie DropDownList od první položky seznamu (nápoje) je vybraný ve výchozím nastavení zobrazuje nápoje produktů v ovládacím prvku DataList. V *filtrování záznamů Master/Detail s DropDownList* kurzu jsme přidali možnost "--zvolit některou kategorii--" k DropDownList, který byl vybrán ve výchozím nastavení a pokud je vybráno, zobrazí *všechny* z produkty v databázi. Takový přístup byl spravovatelné při výpisu produkty v prvku GridView, protože každý řádek produktu zabíral příliš malé množství plochy obrazovky. Pomocí prvku DataList ale informace o jednotlivých produktech využívá mnohem větší blok na obrazovce. Stále umožňuje přidáte možnost "--zvolit některou kategorii--" a jeho ve výchozím nastavení vybrané, ale namísto toho jej zobrazit všechny produkty při výběru nakonfigurujeme ho tak, aby zobrazí žádné produkty.

Chcete-li přidat novou položku seznamu do DropDownList, přejděte do okna Vlastnosti a klikněte na symbol tří teček v `Items` vlastnost. Přidat novou položku seznamu s `Text` "--zvolit některou kategorii--" a `Value` `0`.


![Přidat](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Obrázek 11**: Přidání položky seznamu "--zvolit některou kategorii--"


Alternativně můžete přidat položku seznamu tak, že přidáte následující kód k DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Dále musíme nastavit ovládací prvek DropDownList `AppendDataBoundItems` k `true` vzhledem k tomu, pokud je nastavena na `false` (výchozí), když kategorií jsou svázány s DropDownList z ObjectDataSource, budete přepsat všechny ručně přidané seznamy položky.


![Nastavte vlastnost AppendDataBoundItems na hodnotu True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Obrázek 12**: nastavte `AppendDataBoundItems` vlastnost na hodnotu True


Z důvodu jsme zvolili hodnota `0` seznamu "--zvolit některou kategorii--" položka je vzhledem k tomu, že nejsou žádné kategorie v systém hodnotou `0`, proto žádné záznamy produktu nevrátí se při výběru položky seznamu "--zvolit některou kategorii--". Pokud to pokud chcete potvrdit, věnujte chvíli najdete na stránce prostřednictvím prohlížeče. Jak ukazuje obrázek 13, při počátečním zobrazení stránky je vybrána položka seznamu "--zvolit některou kategorii--" a jsou zobrazeny žádné produkty.


[![Když](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Obrázek 13**: Pokud je vybraná položka seznamu "--zvolit některou kategorii--", jsou zobrazeny žádné produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Pokud byste raději zobrazíte *všechny* produktů Pokud je vybraná možnost "--zvolit některou kategorii--", použijte hodnotu `-1` místo. Bystří čtenáři budou odvolat tento znovu v *filtrování záznamů Master/Detail s DropDownList* kurzu jsme aktualizovali `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metodu tak, aby pokud *`categoryID`* Hodnota `-1` bylo předáno do všech produktů vrácených záznamů.

## <a name="summary"></a>Souhrn

Při zobrazení hierarchicky související data, často pomůže prezentovat data pomocí záznamů master/detail sestavy, ze kterých uživatel mohl spustit perusing data z nejvyšší úrovni hierarchie a přejít na podrobnosti. V tomto kurzu jsme se zaměřili na vytváření jednoduchých záznamů master/detail sestavu zobrazující vybranou kategorii produktů. To se provádí pomocí DropDownList seznam kategorií a DataList pro produkty, které patří do vybrané kategorie.

V dalším kurzu podíváme na oddělení záznamy seznamu a podrobností na dvou stránkách. Na první stránce bude se zobrazí seznam "hlavní" záznamy s odkazem k zobrazení podrobností. Kliknutím na odkaz bude whisk uživateli na druhé stránce, která se zobrazí podrobnosti pro hlavní vybraný záznam.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Randym Schmidt. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](master-detail-filtering-acess-two-pages-datalist-cs.md)
