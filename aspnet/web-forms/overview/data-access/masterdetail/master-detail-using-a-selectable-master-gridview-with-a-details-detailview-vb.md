---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Vybrat hlavní GridView pomocí podrobnosti DetailView (VB) podrobností | Microsoft Docs
author: rick-anderson
description: V tomto kurzu bude mít GridView, jejichž řádky zahrnovat jméno a cena jednotlivých produktů společně s vyberte tlačítko. Kliknutím na tlačítko Vybrat pro particu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: 80db1589de901f7364c05c5bb67829145579b6c0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Vybrat hlavní GridView pomocí podrobnosti DetailView (VB) podrobností
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) nebo [stáhnout PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> V tomto kurzu bude mít GridView, jejichž řádky zahrnovat jméno a cena jednotlivých produktů společně s vyberte tlačítko. Kliknutím na tlačítko Vybrat pro konkrétní produkt způsobí, že jeho úplné podrobnosti, který se má zobrazit v ovládacím prvku DetailsView na stejné stránce.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](master-detail-filtering-across-two-pages-vb.md) jsme viděli postup vytvoření hlavního/podrobného sestavy pomocí dvou webových stránek: "hlavní" webové stránky, ze kterého jsme zobrazí seznam dodavatelů; a webové stránky "Podrobnosti", která uvedené těchto produktů poskytované vybrané dodavatele. Tento formát dvě stránky sestavy můžete vyjádřit těmito jednu stránku. V tomto kurzu bude mít GridView, jejichž řádky zahrnovat jméno a cena jednotlivých produktů společně s vyberte tlačítko. Kliknutím na tlačítko Vybrat pro konkrétní produkt způsobí, že jeho úplné podrobnosti, který se má zobrazit v ovládacím prvku DetailsView na stejné stránce.


[![Kliknutím na tlačítko Vybrat zobrazí podrobnosti o produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Obrázek 1**: Kliknutím na tlačítko Vybrat zobrazí podrobnosti o produktu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Krok 1: Vytvoření volitelný GridView

Odvolání, že v dvě stránky hlavního a podrobného sestavy, aby každý záznam hlavní zahrnuta hypertextový odkaz, který po kliknutí na odeslané uživatele na stránku podrobností předávání kliknutelnou řádek `SupplierID` hodnota v řetězci dotazu. Na hypertextový odkaz byla přidána na každý řádek GridView pomocí HyperLinkField. Jednostránkové hlavní/podrobnosti sestavy, je nutné zadat tlačítka pro každý GridView řádek, který, po kliknutí na zobrazuje podrobnosti. Ovládací prvek GridView lze nakonfigurovat zahrnout vyberte tlačítko pro každý řádek, který způsobí, že zpětné volání a označí tento řádek prvku GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Začněte přidáním ovládacího prvku GridView k `DetailsBySelecting.aspx` stránku `Filtering` složku, nastavení jeho `ID` vlastnost, která má `ProductsGrid`. Dál přidejte nové ObjectDataSource s názvem `AllProductsDataSource` , který spustí `ProductsBLL` třídy `GetProducts()` metoda.


[![Vytvoření ObjectDataSource s názvem AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Obrázek 2**: vytvoření s názvem ObjectDataSource `AllProductsDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Použití třídy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Obrázek 3**: použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Konfigurace ObjectDataSource k vyvolání metody GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource k Invoke `GetProducts()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Upravte pole GridView odebrání všech ale na `ProductName` a `UnitPrice` BoundFields. Navíc Nebojte se přizpůsobit tyto BoundFields podle potřeby, jako je například formátování `UnitPrice` BoundField jako měny a změna `HeaderText` vlastnosti BoundFields. Tyto kroky lze provést graficky, kliknutím na odkaz Upravit sloupce z prvku GridView inteligentní značky nebo ruční konfigurací deklarativní syntaxi.


[![Odeberte všechny kromě ProductName a UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Obrázek 5**: Odeberte všechny ale `ProductName` a `UnitPrice` BoundFields ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Poslední značky GridView se:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

V dalším kroku musíme označit rutina GridView jako volitelný, který přidá vyberte tlačítko pro každý řádek. K tomu, stačí zaškrtněte políčko Povolit výběr v prvku GridView inteligentních značek.


[![Zkontrolujte vybrat řádky GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Obrázek 6**: Ujistěte se, lze vybrat řádky prvku GridView ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Kontrola povolení výběru možnost přidá CommandField k `ProductsGrid` GridView s jeho `ShowSelectButton` vlastností nastavenou na hodnotu True. Výsledkem vyberte tlačítko pro každý řádek GridView, jak ukazuje obrázek 6. Ve výchozím nastavení, vyberte tlačítka se vykresluje jako LinkButtons, ale můžete použít tlačítka nebo ImageButtons místo prostřednictvím CommandField `ButtonType` vlastnost.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Při kliknutí na tlačítko Vybrat řádek GridView vyplývá zpětné volání a prvku GridView `SelectedRow` vlastnosti. Kromě `SelectedRow` vlastnost, poskytuje GridView [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), a [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) vlastnosti. `SelectedIndex` Vlastnost vrátí index vybraného řádku vzhledem k tomu `SelectedValue` a `SelectedDataKey` vlastnosti návratové hodnoty na základě prvku GridView [vlastnost DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Vlastnosti lze přidružit jeden nebo více datové pole hodnoty s každým řádkem a se často používá ke atribut jedinečné identifikační informace ze zadaných dat s každým řádkem GridView. `SelectedValue` Vlastnost vrací hodnotu první `DataKeyNames` datové pole pro vybraný řádek where jako `SelectedDataKey` vlastnost vrací vybraného řádku `DataKey` objekt, který obsahuje všechny hodnoty pro klíčová pole zadaných dat pro Tento řádek.

`DataKeyNames` Je automaticky nastavena na data polí jedinečně identifikující při vytvoření vazby zdroje dat GridView, DetailsView nebo FormView pomocí návrháře. Když tato vlastnost je nastavená pro nás automaticky v předchozím kurzy, příklady by již dříve pracovali bez `DataKeyNames` zadanou vlastnost. Ale pro volitelný GridView v tomto kurzu, a také pro budoucí kurzy, ve kterých jsme budete mít zkoumání vložení, aktualizace a odstranění `DataKeyNames` vlastnost musí být nastavena správně. Ujistěte se, že vaše GridView za chvíli `DataKeyNames` je nastavena na `ProductID`.

Podívejme se na naše průběh doposud prostřednictvím prohlížeče. Všimněte si, že GridView uvádí název a ceny pro všechny produkty spolu s LinkButton se vyberte. Kliknutím na tlačítko Vybrat způsobí, že zpětné volání. V kroku 2 vidíte jak vám má DetailsView reakce na tento zpětné volání zobrazením podrobnosti vybrané produktu.


[![Každý řádek produkt obsahuje vyberte LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Obrázek 7**: každý řádek produkt obsahuje vyberte LinkButton ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Zvýraznění vybraného řádku

`ProductsGrid` Má rutina GridView `SelectedRowStyle` vlastnost, která slouží k stanovují vizuální styl pro vybraný řádek. Použít správně, tím lze vylepšit možnosti pro uživatele další jasně ukazuje, který řádek GridView je momentálně zvolen. V tomto kurzu budeme mít vybraný řádek zvýrazněna se žlutým pozadím.

Stejně jako u naše kurzy starší budeme snažit zachovat estetické související nastavení definované jako tříd CSS. Proto, vytvořte novou třídu CSS v `Styles.css` s názvem `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Chcete-li použít tuto třídu CSS do `SelectedRowStyle` vlastnost *všechny* GridViews v našem kurzu řady upravit `GridView.skin` vzhledu v `DataWebControls` motiv zahrnout `SelectedRowStyle` nastavení, jak je uvedeno níže:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Pomocí tohoto Přidání vybraného řádku GridView zvýraznění barvou žlutý pozadí.


[![Přizpůsobení vzhledu vybraného řádku pomocí vlastnosti SelectedRowStyle GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Obrázek 8**: přizpůsobení vybraný řádek vzhled pomocí prvku GridView `SelectedRowStyle` vlastnost ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Krok 2: Zobrazení podrobností o vybrané produktu v DetailsView

Pomocí `ProductsGrid` dokončit GridView, všechno, co zbývá přidat DetailsView, který zobrazí informace o konkrétní produkt vybrané. Přidání ovládacího prvku DetailsView výše GridView a vytvořit nové ObjectDataSource s názvem `ProductDetailsDataSource`. Vzhledem k tomu, že chceme, aby tento DetailsView chcete zobrazit konkrétní informace o vybrané produktu, nakonfigurujte `ProductDetailsDataSource` používat `ProductsBLL` třídy `GetProductByProductID(productID)` metoda.


[![Volání metody GetProductByProductID(productID) ProductsBLL – třída](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Obrázek 9**: vyvolání `ProductsBLL` třídy `GetProductByProductID(productID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Máte *`productID`* hodnoty parametru získané z ovládacího prvku GridView `SelectedValue` vlastnost. Jak již bylo zmíněno dříve, prvku GridView `SelectedValue` vlastnost vrací hodnotu pro vybraný řádek klíče první data. Proto je nutné, prvku GridView `DataKeyNames` je nastavena na `ProductID`tak, aby vybraný řádek `ProductID` vrácena hodnota `SelectedValue`.


[![Nastavit productID parametrů pro vlastnost SelectedValue GridView.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Obrázek 10**: nastavte *`productID`* parametru prvku GridView `SelectedValue` vlastnost ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Jednou `productDetailsDataSource` ObjectDataSource je správně nakonfigurována a vázána na DetailsView, v tomto kurzu je dokončena! Při první návštěvě stránky není vybrán žádný řádek, proto je rutina GridView `SelectedValue` vlastnost vrátí `Nothing`. Vzhledem k tomu, že nejsou žádné produkty s `NULL` `ProductID` hodnotu, jsou vráceny žádné záznamy `GetProductByProductID(productID)` metoda, což znamená, že se nezobrazí DetailsView (viz obrázek 11). Po kliknutí na tlačítko vyberte řádek GridView vyplývá zpětné volání a DetailsView se aktualizují. V tuto chvíli rutina GridView `SelectedValue` vlastnost vrátí `ProductID` vybraného řádku `GetProductByProductID(productID)` metoda vrátí `ProductsDataTable` s informacemi o konkrétním produktu a DetailsView obsahuje tyto informace (viz obrázek 12).


[![Když se zobrazí první navštívili, pouze GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Obrázek 11**: Pokud nejprve navštívili, zobrazí se pouze GridView ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Po výběru řádku, se zobrazují podrobnosti o produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Obrázek 12**: při výběru řádku, jsou zobrazené podrobnosti produktu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Souhrn

V této a předchozích tři kurzy jsme viděli počet metody pro zobrazení sestavy a podrobností. V tomto kurzu jsme se zaměřili na pomocí volitelný GridView levné hlavní záznamů a DetailsView si můžete zobrazit podrobnosti o vybraný hlavní záznam na stejné stránce. V dřívějších kurzech jsme se podívali na tom, jak zobrazit hlavní/podrobnosti sestavy pomocí DropDownLists a zobrazení hlavní záznamy v jedné webové stránky a podrobných záznamů na jiném.

V tomto kurzu se ukončí součást sestavy a podrobností. Od verze v dalším kurzu jsme brzy naše zkoumání vlastní formátování GridView, DetailsView a FormView. Uvidíme postup přizpůsobení vzhledu tyto ovládací prvky založené na data vázaná na nich, jak shrnout data v prvku GridView zápatí a použití šablon zajistit vyšší úroveň kontroly nad rozložení.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-detail-filtering-across-two-pages-vb.md)
