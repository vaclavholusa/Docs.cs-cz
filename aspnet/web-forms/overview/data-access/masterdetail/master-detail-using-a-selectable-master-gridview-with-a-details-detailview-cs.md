---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Master/Detail s volitelnou GridView hlavní podrobnosti DetailView (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu budou mít ovládacího prvku GridView, jejíž řádky patří název a ceny jednotlivých produktů spolu s tlačítko pro výběr. Vyberte tlačítko pro particu...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: ea1bb03e2ea39198b70cf71c1105b3e6134e3971
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825753"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Master/Detail s volitelnou GridView hlavní podrobnosti DetailView (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) nebo [stahovat PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> V tomto kurzu budou mít ovládacího prvku GridView, jejíž řádky patří název a ceny jednotlivých produktů spolu s tlačítko pro výběr. Kliknutím na tlačítko pro výběr u konkrétního produktu způsobí, že jeho všechny podrobnosti, který se má zobrazit v ovládacím prvku DetailsView na stejné stránce.


## <a name="introduction"></a>Úvod

V [předchozí kurz o službě](master-detail-filtering-across-two-pages-cs.md) jsme viděli, jak vytvořit pomocí dvou webových stránek sestavy záznamů master/detail: "hlavní" webové stránky, ze kterého jsme zobrazí seznam suppliers; a webová stránka "details", uvedené produkty poskytnuté vybrané Dodavatel. Tento formát dvě stránky sestavy můžete vyjádřit na jednu stránku. V tomto kurzu budou mít ovládacího prvku GridView, jejíž řádky patří název a ceny jednotlivých produktů spolu s tlačítko pro výběr. Kliknutím na tlačítko pro výběr u konkrétního produktu způsobí, že jeho všechny podrobnosti, který se má zobrazit v ovládacím prvku DetailsView na stejné stránce.


[![Kliknutím na tlačítko pro výběr zobrazí podrobnosti o produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Obrázek 1**: Kliknutím na tlačítko pro výběr zobrazí podrobnosti o produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Krok 1: Vytvoření volitelných GridView

Vzpomínáte, že dvě stránky hlavního/podrobného hlásit, že každý hlavní záznam obsahuje hypertextový odkaz, který po kliknutí na odesílat uživatele stránce s podrobnostmi o předání kliknutí na řádek `SupplierID` hodnotu v řetězec dotazu. Na každý řádek prvku GridView, použití HyperLinkField byl přidán na hypertextový odkaz. Pro jednu stránku hlavních/podrobných sestav, potřebujeme tlačítko pro každý prvek GridView řádek, který po kliknutí na zobrazuje podrobnosti. Dá se ovládací prvek GridView zahrnout vyberte tlačítko pro každý řádek, který vyvolá zpětné volání a označí tento řádek prvku GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Začněte přidáním ovládacího prvku GridView k `DetailsBySelecting.aspx` stránku `Filtering` složku, nastavení jeho `ID` vlastnost `ProductsGrid`. V dalším kroku přidejte nový prvek ObjectDataSource s názvem `AllProductsDataSource` , která vyvolává `ProductsBLL` třídy `GetProducts()` metody.


[![Vytvoření s názvem AllProductsDataSource ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Obrázek 2**: vytvoření s názvem prvku ObjectDataSource `AllProductsDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![Použití třídy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Obrázek 3**: použijte `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![Prvek ObjectDataSource, která se má vyvolat metodu GetProducts() konfigurace](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource metodě Invoke `GetProducts()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


Upravit pole prvku GridView odebírá se všechno kromě na `ProductName` a `UnitPrice` BoundFields. Navíc libovolně přizpůsobit tyto BoundFields podle potřeby, například formátování `UnitPrice` Vlastnost BoundField jako měnu a změna `HeaderText` vlastnosti BoundFields. Tyto kroky můžete udělat graficky, kliknutím na odkaz Upravit sloupce v prvku GridView inteligentních značek nebo ruční konfigurací deklarativní syntaxe.


[![Odeberte všechny kromě ProductName a UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Obrázek 5**: Odeberte všechny kromě `ProductName` a `UnitPrice` BoundFields ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


Je konečný kód pro prvku GridView:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

V dalším kroku potřebujeme k označení prvku GridView jako volitelný, který přidá tlačítko pro výběr na každý řádek. K tomu stačí zaškrtněte políčko Povolit výběr v prvku GridView inteligentních značek.


[![Zkontrolujte vybrat řádky GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Obrázek 6**: Ujistěte se, lze vybrat řádky prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


Možnost povolení výběru kontroly přidá CommandField k `ProductsGrid` prvek GridView s jeho `ShowSelectButton` nastavenou na hodnotu True. V důsledku vyberte tlačítko pro každý řádek prvku GridView, jak znázorňuje obrázek 6. Ve výchozím nastavení, vyberte tlačítka jsou vykresleny jako LinkButtons, ale můžete použít tlačítka nebo ImageButtons místo prostřednictvím CommandField `ButtonType` vlastnost.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Po kliknutí na tlačítko pro výběr řádku prvku GridView vyplývá zpětné volání a prvku GridView `SelectedRow` vlastnost aktualizovat. Kromě `SelectedRow` poskytuje vlastnosti prvku GridView [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), a [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) vlastnosti. `SelectedIndex` Vlastnost vrátí index vybraného řádku vzhledem k tomu `SelectedValue` a `SelectedDataKey` vlastnosti návratové hodnoty na základě prvku GridView [vlastnost DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Vlastnost se používá k přidružit jeden nebo více dat, pole hodnoty jednotlivých řádků a se běžně používá k atributu jedinečné identifikační informace z podkladových dat a jednotlivých řádků ovládacího prvku GridView. `SelectedValue` Vlastnost vrací hodnotu prvního `DataKeyNames` datové pole pro vybraný řádek where jako `SelectedDataKey` vlastnost vrací vybraný řádek `DataKey` objektu, který obsahuje všechny hodnoty pro pole klíče pro zadaná data Tento řádek.

`DataKeyNames` Je automaticky nastavena na pole data Jednoznačná identifikace Pokud svážete zdrojem dat prvku GridView, DetailsView nebo třídy FormView pomocí návrháře. Zatímco tato vlastnost byla nastavena pro nás automaticky v předchozích kurzech, příklady by fungovat bez `DataKeyNames` zadanou vlastnost. Ale pro volitelný prvek GridView v tomto kurzu, stejně jako pro budoucí kurzů, ve kterých jsme budete mít zkoumání vložení, aktualizace nebo odstranění `DataKeyNames` vlastnost musí být nastavena správně. Ujistěte se, že prvku GridView za chvíli `DataKeyNames` je nastavena na `ProductID`.

Podívejme se na náš postup doposud prostřednictvím prohlížeče. Všimněte si, že prvku GridView se zobrazuje název a cena za všechny tyto produkty spolu s LinkButton se vybrat. Kliknutím na tlačítko pro výběr vyvolá zpětné volání. V kroku 2 uvidíme, jak vám má DetailsView reakce na tento postback tím, že zobrazuje podrobnosti o vybraných produktů.


[![Každý řádek produkt obsahuje vyberte odkazem (LinkButton)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Obrázek 7**: každý produkt řádek obsahuje LinkButton se vyberte ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>Zvýraznění vybraných řádků

`ProductsGrid` Má prvek GridView `SelectedRowStyle` vlastnost, která umožňuje určovat vizuální styl pro vybraný řádek. Správně použít tím lze vylepšit uživatelské prostředí tím, že další jasně zobrazuje aktuálně není vybrán který řádek prvku GridView. Pro účely tohoto kurzu můžeme mít vybraný řádek se zvýrazní se žlutým pozadím.

Stejně jako u našich kurzů pro starší, Pojďme snažit udržovat aesthetic související nastavení definované jako třídy šablony stylů CSS. Proto, vytvořte novou třídu šablony stylů CSS v `Styles.css` s názvem `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Chcete-li použít tuto třídu šablony stylů CSS `SelectedRowStyle` vlastnost *všechny* úpravy prvků GridViews v našem kurzu `GridView.skin` vzhledu v `DataWebControls` motiv, který patří `SelectedRowStyle` nastavení, jak je znázorněno níže:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Uveďte vybraný řádek prvku GridView teď zvýrazní se žlutým pozadím.


[![Přizpůsobení vzhledu vybraný řádek pomocí vlastnosti SelectedRowStyle prvku GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Obrázek 8**: přizpůsobení pomocí vzhled vybraný řádek prvku GridView `SelectedRowStyle` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Krok 2: Zobrazení podrobností o vybraný produkt v DetailsView

S `ProductsGrid` dokončení ovládacího prvku GridView, vše, co už jen zbývá pro přidání prvku DetailsView, který se zobrazí informace o konkrétní vybranému produktu. Přidání ovládacího prvku DetailsView nad prvku GridView a vytvoření nového prvku ObjectDataSource s názvem `ProductDetailsDataSource`. Protože chceme, aby tento prvku DetailsView. Chcete-li zobrazit konkrétní informace o vybraný produkt, nakonfigurujte `ProductDetailsDataSource` používat `ProductsBLL` třídy `GetProductByProductID(productID)` metoda.


[![Volání metody třídy ProductsBLL GetProductByProductID(productID)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Obrázek 9**: vyvolat `ProductsBLL` třídy `GetProductByProductID(productID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


Máte *`productID`* hodnoty získané z ovládacího prvku GridView `SelectedValue` vlastnost. Jak jsme probírali dříve, prvku GridView `SelectedValue` vlastnost vrací hodnotu pro vybraný řádek klíče první data. Proto je nutné, který prvku GridView `DataKeyNames` je nastavena na `ProductID`tak, aby vybraný řádek `ProductID` je vrácena hodnota `SelectedValue`.


[![Nastavte parametr productID k vlastnosti SelectedValue prvku GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Obrázek 10**: nastavte *`productID`* parametr prvku GridView `SelectedValue` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


Jednou `productDetailsDataSource` ObjectDataSource byla správně nakonfigurovaná a vázán na ovládacím prvku DetailsView, tento kurz je dokončeno! Při první návštěvě stránky není vybrán žádný řádek, takže v prvku GridView `SelectedValue` vrátí vlastnost `null`. Vzhledem k tomu, že nejsou žádné produkty s `NULL` `ProductID` hodnotu, jsou vráceny žádné záznamy `GetProductByProductID(productID)` metodu, což znamená, že se nezobrazí ovládacím prvku DetailsView (viz obrázek 11). Po kliknutí na tlačítko pro výběr řádku prvku GridView, vyplývá zpětné volání a aktualizují ovládacím prvku DetailsView. V tuto chvíli prvku GridView `SelectedValue` vrátí vlastnost `ProductID` vybraného řádku `GetProductByProductID(productID)` metoda vrátí hodnotu `ProductsDataTable` s informacemi o tento konkrétní produkt a ovládacím prvku DetailsView. obsahuje tyto podrobnosti (viz obrázek 12).


[![Když se objeví první navštívili, pouze prvku GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Obrázek 11**: když uživatel poprvé, zobrazí se pouze prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![Při výběru řádku, se zobrazí podrobnosti o produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Obrázek 12**: při výběru řádku, se zobrazí podrobnosti produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>Souhrn

V této a předchozích tří kurzů zaznamenali jsme řadu technik pro zobrazení záznamů master/detail sestav. V tomto kurzu, který jsme se zaměřili na použití volitelných GridView budou umístěné hlavní záznamy a DetailsView zobrazíte podrobnosti o vybrané hlavního záznamu na stejné stránce. V předchozích kurzech jsme se podívali na tom, jak zobrazit hlavních/podrobných sestav pomocí DropDownLists a zobrazení předlohy záznamy v jedné webové stránky a podrobných záznamů na jiném.

V tomto kurzu končí naše zkoumání záznamů master/detail sestavy. Počínaje další tutorialwe zobrazí za přibližně naše průzkum vlastní formátování pomocí ovládacího prvku GridView, DetailsView a FormView. Uvidíme, jak přizpůsobit vzhled tyto ovládací prvky založené na data vázaná k nim, jak slouží ke shrnutí dat v zápatí prvku GridView a tom, jak pomocí šablon můžete získat vyšší stupeň kontroly nad rozložení.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-across-two-pages-cs.md)
> [další](master-detail-filtering-with-a-dropdownlist-vb.md)
