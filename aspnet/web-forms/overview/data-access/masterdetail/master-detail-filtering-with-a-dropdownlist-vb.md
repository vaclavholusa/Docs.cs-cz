---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Filtrování s rozevírací seznam (VB) podrobností | Microsoft Docs
author: rick-anderson
description: V tomto kurzu vidíte zobrazení hlavní záznamy v ovládací prvek rozevírací seznam a podrobnosti vybrané položky seznamu v GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d1ae660ddbc6c8e2874190ade6f3deddeebe820
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880372"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtrování s rozevírací seznam (VB) podrobností
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) nebo [stáhnout PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> V tomto kurzu vidíte zobrazení hlavní záznamy v ovládací prvek rozevírací seznam a podrobnosti vybrané položky seznamu v GridView.


## <a name="introduction"></a>Úvod

Běžný typ sestavy je *a podrobností sestavy*, v sestavě začínající zobrazením některé sady záznamů "hlavní". Uživatele můžete pak rozbalit soubor jeden hlavní záznamy, a tím zobrazení daného záznamu hlavní "podrobností." Seznam podrobnosti sestavy jsou ideálním řešením pro vizualizaci na více vztahů, jako je například sestavy zobrazující všechny kategorie a potom umožní uživatelům vybrat určité kategorie a zobrazit její přidružené produkty. Kromě toho a podrobností sestavy jsou vhodné k zobrazení podrobných informací z zvlášť "širokou" tabulek (ta, která mají mnoho sloupců). Například "hlavní" úroveň a podrobností sestav může zobrazovat jenom název a jednotka cena produktu produkty v databázi a procházení k podrobnostem na konkrétní produkt by zobrazit pole Další produktu (kategorie, dodavatele, množství na jednotku, a atd).

Existuje mnoho způsobů, pomocí kterých se dají implementovat sestavy a podrobností. Přes toto a následující tři kurzy podíváme řadu sestav, a podrobností. V tomto kurzu ukážeme, jak zobrazit hlavní záznamy v [ovládací prvek rozevírací seznam](https://msdn.microsoft.com/library/dtx91y0z.aspx) a podrobnosti vybrané položky seznamu v GridView. Konkrétně tento kurz a podrobností sestavy se zobrazí seznam kategorie a informace o produktu.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Krok 1: Zobrazení kategorií v rozevírací seznam

Naše a podrobností sestava zobrazí seznam kategorií v rozevírací seznam, s produkty položce vybrané seznamu zobrazí další dolů na stránce v GridView. První úlohou před nám, se pak se zobrazí v rozevírací seznam kategorií. Otevřete `FilterByDropDownList.aspx` stránku `Filtering` složku, přetáhněte na rozevírací seznam z panelu nástrojů na Návrhář stránky a nastavit jeho `ID` vlastnost, která má `Categories`. Potom klikněte na odkaz zvolit zdroj dat z inteligentních značek rozevírací seznam. Zobrazí se Průvodce konfigurací zdroje dat služby.


[![Zadejte zdroj dat rozevírací seznam](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Obrázek 1**: Zadejte zdroj dat rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Vyberte, chcete-li přidat nový ObjectDataSource s názvem `CategoriesDataSource` , který spustí `CategoriesBLL` třídy `GetCategories()` metoda.


[![Přidat nové ObjectDataSource s názvem CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Obrázek 2**: Přidání nové ObjectDataSource s názvem `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Rozhodnete použít CategoriesBLL – třída](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Obrázek 3**: Vyberte pro použití `CategoriesBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Konfigurace ObjectDataSource lze pomocí této metody GetCategories()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Obrázek 4**: Konfigurace ObjectDataSource pro použití `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Po dokončení konfigurace ObjectDataSource musíme určit, jaké pole datového zdroje, které mají být zobrazeny v rozevírací seznam a které jeden by měly být přidruženy jako hodnotu pro položku seznamu. Máte `CategoryName` pole jako zobrazení a `CategoryID` jako hodnota pro každou položku seznamu.


[![Mají zobrazení rozevírací seznam CategoryName pole a použití CategoryID jako hodnota](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Obrázek 5**: mít zobrazení rozevírací seznam `CategoryName` pole a použití `CategoryID` jako hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


V tomto okamžiku máme ovládací prvek rozevírací seznam, který se zobrazí v záznamy ze `Categories` tabulky (všechny dosáhnout přibližně šest sekund). Obrázek 6 zobrazuje naše průběh doposud, když zobrazit pomocí prohlížeče.


[![Rozevírací seznamy aktuální kategorií](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Obrázek 6**: rozevírací seznamy A aktuální kategorie ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Krok 2: Přidání GridView produkty

Tento poslední krok v našich a podrobností sestavy je seznam produkty spojené s vybranou kategorii. K tomu, rutina GridView přidat na stránku a vytvořit nové ObjectDataSource s názvem `productsDataSource`. Mít `productsDataSource` jatečné data z ovládacího prvku `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda.


[![Vyberte metodu GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Obrázek 7**: vyberte `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Po výběru této metody, Průvodce ObjectDataSource k zadání nám hodnota metody *`categoryID`* parametr. Chcete použít hodnotu vybrané `categories` rozevírací seznam položky nastavit parametr Zdroj ovládacího prvku a ControlID k `Categories`.


[![Nastaví categoryID parametr na hodnotu rozevírací seznam kategorií](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Obrázek 8**: nastavte *`categoryID`* parametr na hodnotu `Categories` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Podívejte se na naše průběh v prohlížeči chvíli trvat. Při první návštěvě stránky, tyto produkty patří do vybrané kategorie (nápoje) jsou zobrazeny (jak je znázorněno na obrázku 9), ale změna rozevírací seznam neaktualizuje data. To je proto zpětné volání, musí dojít k GridView aktualizovat. K tomu můžeme mít dvě možnosti (ani z nich vyžaduje psaní jakéhokoli kódu):

- **Nastavit kategorie rozevírací seznam**[automatické odeslání vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**na hodnotu True.** (Toho lze dosáhnout kontrolou možnost Povolit automatické odeslání v inteligentních značek rozevírací seznam.) To se spustily vždy, když rozevírací seznam vybraná položka je změněna uživatelem. Proto když si uživatel vybere ze rozevírací seznam novou kategorii výsledkem by měla být zpětné volání a GridView bude aktualizována produktů pro nově zvolené kategorii. (Toto je přístupů, které jste použitý v tomto kurzu).
- **Přidání ovládacího prvku tlačítko webové vedle rozevírací seznam.** Nastavte její `Text` vlastnost na aktualizaci nebo něco podobného. S tímto přístupem uživatel bude muset vyberte novou kategorii a potom klikněte na tlačítko. Kliknutím na tlačítko způsobit zpětné volání a aktualizovat GridView seznam těchto produktů vybrané kategorie.

Následující obrázky 9 a 10 znázorňují, sestavy a podrobností v akci.


[![Při první návštěvě stránky, se zobrazí produkty nápoj](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Obrázek 9**: při první návštěvě stránky, se zobrazí produkty nápoj ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Výběr nového produktu (produktu) automaticky vyvolá PostBack, aktualizace GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Obrázek 10**: Výběr nového produktu (produktu) automaticky vyvolá PostBack, aktualizace GridView ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Přidání položky seznamu "– Vyberte kategorii –"

Při první návštěvě `FilterByDropDownList.aspx` stránky kategorie, které rozevírací seznam pro první položku seznamu (nápoje) je ve výchozím nastavení zobrazující nápoj produkty v GridView. Místo zobrazení první kategorie produktů, že jsme může chtít, aby místo rozevírací seznam položky vybrali, říká něco podobného jako "– Vyberte kategorii –".

Chcete-li přidat novou položku seznamu do rozevírací seznam, přejít do okna vlastností a klikněte na symbol tří teček v `Items` vlastnost. Přidat novou položku seznamu s `Text` "--Vyberte kategorii –" a `Value` `-1`.


[![Přidat a – z položky seznamu výběru kategorie--](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Obrázek 11**: Přidat a – z položky seznamu výběru kategorie--([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Alternativně můžete přidat položku seznamu přidáním následující kód do rozevírací seznam:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Kromě toho je potřeba nastavit ovládací prvek rozevírací seznam `AppendDataBoundItems` na hodnotu True, protože při kategorie je vázána na rozevírací seznam z ObjectDataSource se budete přepsat všech ručně přidat seznam položek, pokud `AppendDataBoundItems` není hodnotu True.


![Nastavte vlastnost AppendDataBoundItems vlastnost na hodnotu True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Obrázek 12**: nastavte `AppendDataBoundItems` vlastnost na hodnotu True


Po provedení těchto změn při první návštěvě stránky je vybrána možnost "– Vyberte kategorii –" a jsou zobrazeny žádné produkty.


[![Počáteční stránka zatížení jsou zobrazeny žádné produkty](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Obrázek 13**: na the počáteční stránka zatížení Ne se zobrazí produkty ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


Z důvodu žádné produkty jsou vzhledem k tomu, že je vybraná položka seznamu "– Vyberte kategorii –" zobrazí, když je, protože její hodnota je `-1` a v databázi s nejsou žádné produkty `CategoryID` z `-1`. Pokud toto chování je budete chtít pak v tomto okamžiku jste hotovi! Pokud však chcete zobrazit *všechny* kategorie Pokud je vybraná položka seznamu "– Vyberte kategorii –", vraťte k `ProductsBLL` třídy a přizpůsobit `GetProductsByCategoryID(categoryID)` metody, které se vyvolá `GetProducts()` metoda Pokud předaný v *`categoryID`* parametru je menší než nula:


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

Technika použít zde je podobná přístup jsme slouží k zobrazení všech dodavatelů zpět v [deklarativní parametry](../basic-reporting/declarative-parameters-cs.md) kurz, i když v tomto příkladu používáme hodnotu `-1` indikující, že by měly být všechny záznamy načíst naproti tomu `Nothing`. Důvodem je, že *`categoryID`* parametr `GetProductsByCategoryID(categoryID)` metoda očekává jako předána hodnota celé číslo, zatímco v tomto kurzu deklarativní parametry jsme byly předávání v vstupní parametr řetězce.

Snímek obrazovky ukazuje obrázek 14 `FilterByDropDownList.aspx` když je vybrána možnost "– Vyberte kategorii –". Zde jsou ve výchozím nastavení zobrazí všechny produkty a uživatele můžete zúžit zobrazení tak, že zvolíte do určité kategorie.


[![Všechny produkty jsou nyní uvedeny ve výchozím nastavení](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Obrázek 14**: všechny produkty jsou nyní uvedeny ve výchozím nastavení ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Souhrn

Při zobrazení hierarchicky související data, je dobré často se data pomocí sestav a podrobností, ze kterých uživatel můžete spustit perusing data od nejvyšší úrovně v hierarchii a podrobnostem podrobnosti k dispozici. V tomto kurzu jsme se zaměřili na vytváření jednoduché a podrobností sestavy zobrazující vybrané kategorie produktů. Bylo to provést pomocí rozevírací seznam pro seznam kategorií a GridView pro produkty, které patří do vybrané kategorie.

V [další kurz](master-detail-filtering-with-two-dropdownlists-vb.md) provedeme jeden krok rozhraní rozevírací seznam navíc pomocí dvou DropDownLists.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [další](master-detail-filtering-with-two-dropdownlists-vb.md)
