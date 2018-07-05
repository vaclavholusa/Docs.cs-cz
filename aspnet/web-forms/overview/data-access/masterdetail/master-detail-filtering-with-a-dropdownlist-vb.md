---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Ovládacím prvkem DropDownList (VB) filtrování záznamů Master/Detail | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu uvidíme, jak zobrazit hlavní záznamy v ovládací prvek DropDownList a podrobnosti vybrané položky seznamu v GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d15348812abf88fa35485c0d3415b2683225b6b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386692"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Ovládacím prvkem DropDownList (VB) filtrování záznamů Master/Detail
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) nebo [stahovat PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> V tomto kurzu uvidíme, jak zobrazit hlavní záznamy v ovládací prvek DropDownList a podrobnosti vybrané položky seznamu v GridView.


## <a name="introduction"></a>Úvod

Společný typ sestavy, je *sestavy záznamů master/detail*, ve které sestavy začíná tím, že zobrazuje některé sady záznamů "hlavní". Uživatel pak přejít ke jeden z hlavních záznamů, a tím zobrazení hlavního záznamu "podrobností." Záznamů master/detail sestavy jsou ideální volbou pro vizualizaci vztahů jednoho k několika, jako je například sestavu zobrazující všechny kategorie a pak umožní uživateli vybrat určitou kategorii a zobrazit jeho související produkty. Kromě toho záznamů master/detail zprávy jsou užitečné pro zobrazení podrobných informací z zejména "širokých" tabulek (těch, které mají velký počet sloupců). Například "hlavní" úroveň záznamů master/detail sestav může zobrazovat jenom název a jednotku cena produktu produktů v databázi a procházení k podrobnostem konkrétního produktu by zobrazil pole Další produkt (kategorie, Dodavatel, množství na jednotku, a tak dále).

Existuje mnoho způsobů, kterými je možné implementovat záznamů master/detail sestavy. Toto a následující tři kurzy podíváme na širokou škálu hlavních záznamů/podrobností sestav. V tomto kurzu uvidíme, jak zobrazit záznamy v hlavní [ovládací prvek DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) a podrobnosti o vybrané položky seznamu v GridView. Zejména sestavy záznamů master/detail v tomto kurzu, zobrazí se seznam kategorií a informace o produktu.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Krok 1: Zobrazení kategorií v DropDownList

Naší sestavy záznamů master/detail zobrazí seznam kategorií v DropDownList, s produkty položku vybraného seznamu zobrazí další dolů na stránce v GridView. První úkol náskok před nám, pak je kategorie zobrazuje v DropDownList. Otevřít `FilterByDropDownList.aspx` stránku `Filtering` složky, přetáhněte z panelu nástrojů do návrháře na stránce DropDownList a nastavte jeho `ID` vlastnost `Categories`. Pak klikněte na odkaz zvolit zdroj dat z inteligentních značek DropDownList. Zobrazí se Průvodce konfigurací zdroje dat.


[![Zadejte zdroj dat DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Obrázek 1**: určení zdroje dat DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Vybrat přidání nového prvku ObjectDataSource s názvem `CategoriesDataSource` , která vyvolává `CategoriesBLL` třídy `GetCategories()` metody.


[![Přidat nový prvek ObjectDataSource s názvem CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Obrázek 2**: přidejte nový prvek ObjectDataSource s názvem `CategoriesDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Zvolte možnost použití třídy CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Obrázek 3**: Zvolte možnost k použití `CategoriesBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Konfigurace ObjectDataSource GetCategories() metody](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource k použití `GetCategories()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Po dokončení konfigurace prvek ObjectDataSource, musíme určit, jaké pole zdroje dat mají být zobrazeny v DropDownList a které jedna by měla být přiřazen jako hodnota pro položku seznamu. Máte `CategoryName` pole jako zobrazení a `CategoryID` jako hodnotu pro každou položku seznamu.


[![Mít zobrazení DropDownList CategoryName pole a CategoryID použijte jako hodnotu](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Obrázek 5**: Zobrazit DropDownList `CategoryName` pole a použití `CategoryID` jako hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


V tuto chvíli máme, který je naplněn záznamy z ovládací prvek DropDownList `Categories` tabulky (vše lze provést během přibližně šest sekund). Obrázek 6 doposud zobrazuje náš postup při prohlížení prostřednictvím prohlížeče.


[![Rozevírací seznam aktuálních kategorií](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Obrázek 6**: A rozevírací seznam aktuálních kategorií ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Krok 2: Přidání ovládacího prvku GridView produkty

Tento poslední krok v naší sestavy záznamů master/detail je seznam produktů spojené s vybranou kategorii. K tomu přidat na stránku GridView a vytvoření nového prvku ObjectDataSource s názvem `productsDataSource`. Mít `productsDataSource` jatečné data z ovládacího prvku `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metody.


[![Vyberte metodu GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Obrázek 7**: vyberte `GetProductsByCategoryID(categoryID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Po výběru této metody, Průvodce ObjectDataSource nám vyzve k zadání hodnoty pro metody *`categoryID`* parametru. Chcete použít hodnotu vybraného `categories` DropDownList položka nastavena na ovládací prvek a ControlID na zdroji parametru `Categories`.


[![Nastavit ID kategorie parametr na hodnotu DropDownList kategorie](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Obrázek 8**: nastavte *`categoryID`* parametr na hodnotu `Categories` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Podívejte se na náš postup v prohlížeči chvíli trvat. Při první návštěvě stránky, produkty patří do vybrané kategorie (nápoje) se zobrazí (jak je znázorněno na obrázku 9), ale změna DropDownList neaktualizuje data. Je to proto zpětné volání, musí dojít k prvku GridView. Chcete-li aktualizovat. K tomu že máme k dispozici dvě možnosti (ani jeden z nich vyžaduje psaní jakéhokoli kódu):

- **Nastavení kategorie DropDownList**[vlastnost AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**na hodnotu True.** (Lze provést tak, že zkontrolujete možnost povolit vlastnost AutoPostBack v inteligentních značek DropDownList.) Zpětné volání tím se aktivuje vždy, když DropDownList vybraná položka je uživatel změní. Proto když uživatel vybere novou kategorii z DropDownList nevzniknou zpětné volání a aktualizuje prvku GridView s produkty pro nově vybranou kategorii. (Toto je přístup, který jsem spotřeboval/v tomto kurzu).
- **Přidejte ovládací prvek webového tlačítko vedle DropDownList.** Nastavte jeho `Text` vlastnosti aktualizovat nebo něco podobného. S tímto přístupem bude uživatel muset vybrat nové kategorie a potom klikněte na tlačítko. Kliknutím na tlačítko vyvolávají zpětné odeslání a aktualizujte GridView seznam těchto produktů vybrané kategorie.

Obrázky 9 a 10 ukazují sestavu záznamů master/detail v akci.


[![Při první návštěvě stránky, se zobrazují produkty nápoje](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Obrázek 9**: při první návštěvě stránky, produkty nápoje zobrazují ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Výběr nového produktu (produkty) automaticky vyvolá zpětné volání, aktualizace prvku GridView.](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Obrázek 10**: Výběr nového produktu (produkty) automaticky vyvolá zpětné volání, aktualizace prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Přidání položky seznamu "--zvolit některou kategorii--"

Při první návštěvě `FilterByDropDownList.aspx` stránce kategorie DropDownList od první položky seznamu (nápoje) je vybraný ve výchozím nastavení zobrazuje nápoje produkty v prvku GridView. Ne, který zobrazuje první kategorie produktů, že může být vhodné místo toho obsahují položku DropDownList vybrali řekne něco jako "--zvolit některou kategorii--".

Chcete-li přidat novou položku seznamu do DropDownList, přejděte do okna Vlastnosti a klikněte na symbol tří teček v `Items` vlastnost. Přidat novou položku seznamu s `Text` "--zvolit některou kategorii--" a `Value` `-1`.


[![Přidání--výběrem kategorie – položka seznamu](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Obrázek 11**: přidání--výběrem kategorie – položka seznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Alternativně můžete přidat položku seznamu tak, že přidáte následující kód k DropDownList:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Kromě toho musíme nastavit ovládací prvek DropDownList `AppendDataBoundItems` na hodnotu True, protože při kategorií jsou svázány s DropDownList z ObjectDataSource, budete-li přepsat všechny ručně přidané položky `AppendDataBoundItems` není pravda.


![Nastavte vlastnost AppendDataBoundItems na hodnotu True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Obrázek 12**: nastavte `AppendDataBoundItems` vlastnost na hodnotu True


Po provedení těchto změn při první návštěvě stránky je vybraná možnost "--zvolit některou kategorii--" a jsou zobrazeny žádné produkty.


[![Ve počáteční načtení stránky nejsou zobrazeny žádné produkty.](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Obrázek 13**: jsou zobrazeny na the počáteční stránky zatížení žádné produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


Z důvodu žádné produkty jsou vzhledem k tomu, že je vybrána položka seznamu "--zvolit některou kategorii--" zobrazí, když je, protože její hodnota je `-1` a v databázi s neodpovídají žádné produkty `CategoryID` z `-1`. Pokud je to chování, které chcete, aby pak budete mít v tomto okamžiku! Pokud však chcete zobrazit *všechny* kategorií při výběru položky seznamu "--zvolit některou kategorii--", vraťte se do `ProductsBLL` třídy a přizpůsobit `GetProductsByCategoryID(categoryID)` metodu tak, že volá `GetProducts()` – metoda Pokud předaný v *`categoryID`* parametru je menší než nula:


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

Techniku použít zde je podobný přístup jsme slouží k zobrazení všech dodavatelů zpátky [deklarované parametry](../basic-reporting/declarative-parameters-cs.md) výukový program, i když v tomto příkladu používáme hodnotu `-1` k označení, že by měly být všechny záznamy načíst nikoli `Nothing`. Důvodem je, že *`categoryID`* parametr `GetProductsByCategoryID(categoryID)` metoda očekává, že předáno celé číslo, že v tomto kurzu deklarované parametry jsme se předávání v řetězci vstupního parametru.

Snímek obrazovky znázorňuje obrázek 14 `FilterByDropDownList.aspx` když je vybraná možnost "--zvolit některou kategorii--". Tady všech produktů, které jsou ve výchozím nastavení zobrazí a uživatele můžete zúžit zobrazení výběrem konkrétní kategorie.


[![Všechny produkty jsou teď uvedené ve výchozím nastavení](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Obrázek 14**: všech produktů, které jsou teď uvedené ve výchozím nastavení ([kliknutím ji zobrazíte obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Souhrn

Při zobrazení hierarchicky související data, často pomůže prezentovat data pomocí záznamů master/detail sestavy, ze kterých uživatel mohl spustit perusing data z nejvyšší úrovni hierarchie a přejít na podrobnosti. V tomto kurzu jsme se zaměřili na vytváření jednoduchých záznamů master/detail sestavu zobrazující vybranou kategorii produktů. To se provádí pomocí DropDownList seznam kategorií a GridView pro produkty, které patří do vybrané kategorie.

V [další kurz](master-detail-filtering-with-two-dropdownlists-vb.md) provedeme jeden krok DropDownList rozhraní kromě toho pomocí dvou DropDownLists.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [další](master-detail-filtering-with-two-dropdownlists-vb.md)
