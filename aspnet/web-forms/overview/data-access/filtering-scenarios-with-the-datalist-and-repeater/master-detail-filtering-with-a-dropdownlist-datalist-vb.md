---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: "Filtrování s rozevírací seznam (VB) podrobností | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu jsme zjistit, jak zobrazit sestavy a podrobností v jednu webovou stránku zobrazit \"hlavní\" záznamy a DataList s upozorněním pomocí DropDownLists..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f480cfcfb3b02c9398b2db3e66cec432152a05d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtrování s rozevírací seznam (VB) podrobností
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) nebo [stáhnout PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> V tomto kurzu jsme naleznete v části zobrazit sestavy a podrobností v jedné webové stránce pomocí DropDownLists zobrazíte "hlavní" záznamů a DataList zobrazit podrobnosti "o".


## <a name="introduction"></a>Úvod

A podrobností zprávu, která nejprve vytvořené pomocí GridView v dříve [a podrobností filtrování s rozevírací seznam](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) kurzu začne tím, že zobrazuje některé sady záznamů "hlavní". Uživatele můžete pak rozbalit soubor jeden hlavní záznamy, a tím zobrazení daného záznamu hlavní "podrobností." Sestavy a podrobností jsou se ideálním řešením pro vizualizaci na více relací a zobrazení podrobných informací z zvlášť "širokou" tabulky (ta, která mají mnoho sloupců). Jsme jste prozkoumali implementaci použití ovládacích prvků GridView a DetailsView v předchozí kurzy sestavy a podrobností. V tomto kurzu a další dvě budete nám prozkoumat tyto koncepty, ale soustředí na pomocí DataList a místo toho řídí opakovače.

V tomto kurzu budeme zabývat pomocí rozevírací seznam obsahuje záznamy "hlavní", "Podrobnosti" záznamy zobrazeny v DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Krok 1: Přidání a podrobností kurz webové stránky

Než začneme v tomto kurzu, můžeme nejdřív za chvíli přidat složky a stránek ASP.NET, které budeme potřebovat pro tento kurz a další dva týkajících se použití ovládacích prvků DataList a opakovače sestavy a podrobností. Začněte vytvořením novou složku v projektu s názvem `DataListRepeaterFiltering`. Dál přidejte následující pět stránek ASP.NET do této složky rozmístit nakonfigurované na používání stránky předlohy `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Vytvořte složku DataListRepeaterFiltering a přidání stránky kurz ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Obrázek 1**: vytvoření `DataListRepeaterFiltering` složky a přidat stránky kurz ASP.NET


Dále otevřete `Default.aspx` stránky a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek z `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [hlavní stránky a webové navigace](../introduction/master-pages-and-site-navigation-vb.md) kurzu mapy webu a zobrazí výčet kurzů k z aktuálního oddílu v seznamu s odrážkami.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))


Pokud chcete mít v zobrazení seznamu s odrážkami kurzy a podrobností, které jsme budete vytvářet, je třeba je přidáte do mapy webu. Otevřete `Web.sitemap` souboru a přidejte následující kód po značku "Zobrazení dat pomocí DataList a opakovače" mapy webu uzlu:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Krok 2: Zobrazení kategorií v rozevírací seznam

Naše a podrobností sestava zobrazí seznam kategorií v rozevírací seznam, s produkty položce vybrané seznamu zobrazí další dolů na stránce v DataList. První úlohou před nám, se pak se zobrazí v rozevírací seznam kategorií. Začněte otevřením `FilterByDropDownList.aspx` stránku `DataListRepeaterFiltering` složku a přetáhněte rozevírací seznam z panelu nástrojů na stránky návrháře. Dále nastavte rozevírací seznam `ID` vlastnost `Categories`. Klikněte na odkaz zvolit zdroj dat z inteligentních značek rozevírací seznam a vytvořit nové ObjectDataSource s názvem `CategoriesDataSource`.


[![Přidat nové ObjectDataSource s názvem CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Obrázek 4**: Přidání nové ObjectDataSource s názvem `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))


Konfigurace nového ObjectDataSource tak, aby vyvolá `CategoriesBLL` třídy `GetCategories()` metoda. Po dokončení konfigurace ObjectDataSource musíme určit, jaké pole datového zdroje, které mají být zobrazeny v rozevírací seznam a které jeden by měly být přidruženy jako hodnota pro každou položku seznamu. Máte `CategoryName` pole jako zobrazení a `CategoryID` jako hodnota pro každou položku seznamu.


[![Mají zobrazení rozevírací seznam CategoryName pole a použití CategoryID jako hodnota](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Obrázek 5**: mít zobrazení rozevírací seznam `CategoryName` pole a použití `CategoryID` jako hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))


V tomto okamžiku máme ovládací prvek rozevírací seznam, který se zobrazí v záznamy ze `Categories` tabulky (všechny dosáhnout přibližně šest sekund). Obrázek 6 zobrazuje naše průběh doposud, když zobrazit pomocí prohlížeče.


[![Rozevírací seznamy aktuální kategorií](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Obrázek 6**: rozevírací seznamy A aktuální kategorie ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Krok 2: Přidání DataList produkty

Posledním krokem v naší a podrobností sestavy je seznam produkty spojené s vybranou kategorii. K tomu, přidejte na stránku DataList a vytvořit nové ObjectDataSource s názvem `ProductsByCategoryDataSource`. Mít `ProductsByCategoryDataSource` načíst data z ovládacího prvku `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda. Vzhledem k tomu, že tuto sestavu a podrobností je jen pro čtení, zvolte možnost (žádná) na kartách INSERT, UPDATE a DELETE.


[![Vyberte metodu GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Obrázek 7**: vyberte `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))


Po kliknutí na tlačítko Další, Průvodce ObjectDataSource k zadání nám zdroj hodnota `GetProductsByCategoryID(categoryID)` metody  *`categoryID`*  parametr. Chcete použít hodnotu vybrané `categories` rozevírací seznam položky nastavit parametr Zdroj ovládacího prvku a ControlID k `Categories`.


[![Nastaví categoryID parametr na hodnotu rozevírací seznam kategorií](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Obrázek 8**: nastavte  *`categoryID`*  parametr na hodnotu `Categories` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))


Po dokončení průvodce Konfigurace zdroje dat, bude automaticky generovat Visual Studio `ItemTemplate` pro DataList, který zobrazí název a hodnotu každé datové pole. Umožňuje zvýšit DataList místo toho použít `ItemTemplate` který zobrazí pouze název produktu, kategorie, dodavatele, množství jednotce a ceníku spolu s `SeparatorTemplate` , vloží `<hr>` element mezi každou položku. Přechod do použít `ItemTemplate` z příklad v [zobrazení dat s ovládacími prvky opakovače a DataList](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) kurzu, ale můžete použít libovolnou šablonu značek zjistíte nejvíce přitažlivé chování.

Po provedení těchto změn, vaše DataList a jeho ObjectDataSource značek by měl vypadat takto:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Podívejte se na naše průběh v prohlížeči chvíli trvat. Při první návštěvě stránky, tyto produkty, které patří do vybrané kategorie (nápoje) se zobrazí (jak je znázorněno na obrázku 9), ale změna rozevírací seznam neaktualizuje data. To je proto zpětné volání, musí dojít k DataList aktualizovat. Chcete-li tomu můžeme buď nastavte rozevírací seznam `AutoPostBack` vlastnost `true` nebo přidání ovládacího prvku tlačítko na stránku. V tomto kurzu I jste se rozhodli nastavit rozevírací seznam `AutoPostBack` vlastnost `true`.

Následující obrázky 9 a 10 znázorňují, sestavy a podrobností v akci.


[![Při první návštěvě stránky, se zobrazí produkty nápoj](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Obrázek 9**: při první návštěvě stránky, se zobrazí produkty nápoj ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))


[![Výběr nového produktu (produktu) automaticky vyvolá PostBack, aktualizaci prvku DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Obrázek 10**: Výběr nového produktu (produktu) automaticky vyvolá PostBack, aktualizaci prvku DataList ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Přidání položky seznamu "– Vyberte kategorii –"

Při první návštěvě `FilterByDropDownList.aspx` stránky kategorie, které rozevírací seznam pro první položku seznamu (nápoje) je ve výchozím nastavení zobrazující nápoj produkty v prvku DataList. V *a podrobností filtrování s rozevírací seznam* kurzu jsme přidali možnost "– Vyberte kategorii –" na rozevírací seznam, který se ve výchozím nastavení vybrané a při výběru, zobrazí *všechny* z produkty v databázi. Tento postup se spravovat při výpisu produkty v GridView, jak každý řádek produktu trvalo až malé množství nemovitosti obrazovky. S DataList ale každý produkt informace využívá mnohem větší bloku na obrazovce. Stále umožňuje přidat možnost "– Vyberte kategorii –" a mít je standardně vybraná, ale místo nutnosti ho zobrazit všechny produkty, pokud vybraná, umožňuje nakonfigurovat, aby zobrazuje žádné produkty.

Chcete-li přidat novou položku seznamu do rozevírací seznam, přejít do okna vlastností a klikněte na symbol tří teček v `Items` vlastnost. Přidat novou položku seznamu s `Text` "--Vyberte kategorii –" a `Value` `0`.


![Přidat](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Obrázek 11**: Přidání položky seznamu "– Vyberte kategorii –"


Alternativně můžete přidat položku seznamu přidáním následující kód do rozevírací seznam:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Kromě toho je potřeba nastavit ovládací prvek rozevírací seznam `AppendDataBoundItems` k `true` vzhledem k tomu, pokud je nastaven na hodnotu `false` (výchozí), pokud kategorie je vázána na rozevírací seznam z ObjectDataSource se budete přepsat žádný seznam ručně přidat položky.


![Nastavte vlastnost AppendDataBoundItems vlastnost na hodnotu True](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Obrázek 12**: nastavte `AppendDataBoundItems` vlastnost na hodnotu True


Z důvodu jsme zvolili hodnota `0` seznamu "– Vyberte kategorii –" položka je, protože nejsou k dispozici žádné kategorie v systému s hodnotou `0`, proto se žádné záznamy produktu vrátí, pokud je vybrána položka seznamu "– Vyberte kategorii –". Potvrďte to za chvíli navštívit stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 13, při počátečním prohlížení stránky, je vybrána položka seznamu "– Vyberte kategorii –" a jsou zobrazeny žádné produkty.


[![Když](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Obrázek 13**: Pokud je vybrána položka seznamu "– Vyberte kategorii –", jsou zobrazeny žádné produkty ([Kliknutím zobrazit obrázek v plné velikosti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))


Pokud byste místo zobrazení *všechny* produkty Pokud je vybraná možnost "– Vyberte kategorii –", použijte hodnotu `-1` místo. Astute čtečky připomíná, že zpět v *a podrobností filtrování s rozevírací seznam* kurzu aktualizovali jsme `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda tak, aby pokud  *`categoryID`*  Hodnota `-1` byl předán v všechny produktu, záznamů byly vráceny.

## <a name="summary"></a>Souhrn

Při zobrazení hierarchicky související data, je dobré často se data pomocí sestav a podrobností, ze kterých uživatel můžete spustit perusing data od nejvyšší úrovně v hierarchii a podrobnostem podrobnosti k dispozici. V tomto kurzu jsme se zaměřili na vytváření jednoduché a podrobností sestavy zobrazující vybrané kategorie produktů. Bylo to provést pomocí rozevírací seznam pro seznam kategorií a DataList pro produkty, které patří do vybrané kategorie.

V dalším kurzu podíváme oddělení mezi dvěma stránkami záznamy seznamu a podrobností. Na první stránce seznam záznamů "hlavní" se zobrazí, s odkazem k zobrazení podrobností. Kliknutím na odkaz bude whisk uživatele na druhou stránku, která se zobrazí podrobnosti pro vybraný záznam hlavní.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Randy Schmidt. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
[další](master-detail-filtering-acess-two-pages-datalist-vb.md)
