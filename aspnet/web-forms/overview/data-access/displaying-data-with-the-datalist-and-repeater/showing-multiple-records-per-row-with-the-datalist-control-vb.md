---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: "Zobrazení více záznamů na řádek pomocí ovládacího prvku DataList (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu krátké jsme budete prozkoumejte postup přizpůsobení DataList rozložení pomocí jeho RepeatColumns a RepeatDirection vlastnosti."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d6a9c6aef42d1f165567d1a1802bffa853a320e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Zobrazení více záznamů na řádek pomocí ovládacího prvku DataList (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) nebo [stáhnout PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> V tomto kurzu krátké jsme budete prozkoumejte postup přizpůsobení DataList rozložení pomocí jeho RepeatColumns a RepeatDirection vlastnosti.


## <a name="introduction"></a>Úvod

Příklady DataList jsme jste viděli v posledních dvou kurzech mít vykresluje každý záznam od zdroje dat jako řádek ve formátu HTML jednoho sloupce `<table>`. Přestože se výchozí chování DataList, je velmi snadné pro přizpůsobení zobrazení DataList tak, že položky zdroje dat jsou rozloženy vícesloupcového, více řádků tabulky. Kromě toho je možné, že všechna data s zdroje položky zobrazené v DataList jednoho řádku s více sloupci.

Jsme rozložení DataList s prostřednictvím upravit jeho `RepeatColumns` a `RepeatDirection` vlastnosti, které v uvedeném pořadí, označují jsou vykreslovány v tom, kolik sloupců a zda tyto položky jsou nastíněny vodorovně nebo svisle. Obrázek 1 zobrazuje DataList, který obsahuje informace o produktu v tabulce s tři sloupce.


[![DataList zobrazí tři produkty, na řádek](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Obrázek 1**: DataList ukazuje tři produkty na řádek ([Kliknutím zobrazit obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Zobrazením více položky zdroje dat na řádek DataList efektivněji využívat místo vodorovné obrazovky. V tomto kurzu krátké budete nám prozkoumat tyto dvě vlastnosti DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Krok 1: Zobrazení informací o produktu v DataList

Před jsme zkontrolujte `RepeatColumns` a `RepeatDirection` vlastností, umožňují s nejprve vytvořit DataList na naší stránce, které jsou uvedeny informace o produktu pomocí rozložení standardní jednoho sloupce, více řádků tabulky. V tomto příkladu umožní s zobrazení s název produktu, kategorie a ceny pomocí následující kód:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Jsme jste se seznámili s postupem k vytvoření vazby dat DataList v předchozích příkladech, tak I budete rychle přesouvat pomocí těchto kroků. Začněte otevřením `RepeatColumnAndDirection.aspx` stránku `DataListRepeaterBasics` složku a přetáhněte DataList z panelu nástrojů na návrháře. Z inteligentních značek DataList s opt vytvořit nové ObjectDataSource a nakonfigurovat ho načítat data z `ProductsBLL` třídu s `GetProducts` metody výběru (None) možnost z průvodce s příkazy INSERT, UPDATE a odstranit karty.

Po vytvoření a vytvoření vazby nové ObjectDataSource prvku DataList, vytvoří sada Visual Studio automaticky `ItemTemplate` který zobrazí název a hodnota pro každé pole data produktu. Upravit `ItemTemplate` buď přímo prostřednictvím deklarativní nebo ze šablon upravit možnost DataList s inteligentním tak, aby používala uvedené výše, nahraďte kód *název produktu*, *název kategorie* , a *cena* textu s ovládacími prvky popisek, které používají syntaxi příslušné datové vazby k přiřazení hodnoty k jejich `Text` vlastnosti. Po aktualizaci `ItemTemplate`, stránku s deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Všimněte si zadá sunout zahrnuté specifikace formátu v `Eval` Syntaxe datové vazby `UnitPrice`, formátování vrácené hodnoty jako měnu -`Eval("UnitPrice", "{0:C}").`

Za chvíli navštivte stránku v prohlížeči. Jak je vidět na obrázku 2, DataList vykreslí jako jeden sloupec, více řádků tabulky produktů.


[![Ve výchozím nastavení, vykreslí DataList jako jeden sloupec, více řádků tabulky](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Obrázek 2**: ve výchozím nastavení, DataList vykreslí jako jednoho sloupce, více řádků tabulky ([Kliknutím zobrazit obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Krok 2: Změna směru DataList s rozložení

Při výchozí chování pro prvku DataList je pro rozložení jeho položky svisle v tabulce jednoho sloupce, více řádků, toto chování lze snadno změnit prostřednictvím DataList s [ `RepeatDirection` vlastnost](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Vlastnost může přijmout jeden dvě možné hodnoty: `Horizontal` nebo `Vertical` (výchozí).

Změnou `RepeatDirection` vlastnost z `Vertical` k `Horizontal`, prvku DataList vykreslí jeho záznamů do jednoho řádku vytváření za Zdrojová položka dat jeden sloupec. Pro ilustraci tohoto efektu, klikněte na DataList v návrháři a potom v okně vlastností změňte `RepeatDirection` vlastnost z `Vertical` k `Horiztonal`. Okamžitě po to uděláte, návrháře upraví rozložení DataList s vytváření rozhraní jednoho řádku s více sloupci (viz obrázek 3).


[![Položky RepeatDirection vlastnost stanoví, jak směr DataList s jsou umístěné na](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Obrázek 3**: `RepeatDirection` vlastnost určuje, jak položky směr DataList s jsou umístěné na ([Kliknutím zobrazit obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Při zobrazení malé množství dat, jeden řádek, může být více sloupci tabulky nevhodnější způsob maximalizovat nemovitosti obrazovky. Pro větší objemy dat ale jeden řádek bude vyžadovat větší počet sloupců, které nabízených oznámení ty položky tohoto t může přizpůsobit na obrazovce vypnout vpravo. Obrázek 4 ukazuje produkty při vykreslení v DataList jeden řádek. Vzhledem k tomu, že jsou mnoho produkty (více než 80), uživatel bude muset daleko posuňte doprava k zobrazení informací o jednotlivých produktů.


[![Pro dostatečně velké datové zdroje bude vyžadovat jeden sloupec DataList vodorovného posouvání](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Obrázek 4**: pro dostatečně velké zdroje dat, jeden sloupec DataList bude vyžadovat vodorovného posouvání ([Kliknutím zobrazit obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Krok 3: Zobrazení dat v tabulce vícesloupcového, více řádků

K vytvoření DataList vícesloupcového, více řádků, je potřeba nastavit [ `RepeatColumns` vlastnost](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) počtu sloupce k zobrazení. Ve výchozím nastavení `RepeatColumns` je nastavena na 0, což způsobí DataList zobrazíte všechny jeho položky v jeden řádek nebo sloupec (v závislosti na hodnotě `RepeatDirection` vlastnost).

Pro náš příklad umožní s zobrazit tři produkty každý řádek tabulky. Proto nastavit `RepeatColumns` vlastnost 3. Po provedení této změny, pozorně zobrazte výsledky v prohlížeči. Jak je vidět na obrázku 5, jsou nyní uvedeny produkty v tabulce tři sloupce, více řádků.


[![Tři produkty se zobrazují na řádek](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Obrázek 5**: tři produkty se zobrazují na řádek ([Kliknutím zobrazit obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection` Vlastnost ovlivňuje, jak jsou rozloženy položky v prvku DataList. Obrázek 5 ukazuje výsledky s `RepeatDirection` vlastnost nastavena na hodnotu `Horizontal`. Všimněte si, že první tři produkty Chai, změn a anýzového sirupu jsou nastíněny zleva doprava, shora dolů. V řádku pod první tři se zobrazí následující tři produkty (počínaje Chef Anton s Cajun Seasoning). Změna `RepeatDirection` vlastnost zpět do `Vertical`, ale rozložen tyto produkty shora dolů, zleva doprava, jak ukazuje obrázek 6.


[![Zde tyto produkty jsou umístěné na svisle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Obrázek 6**: zde tyto produkty jsou umístěné na svisle ([Kliknutím zobrazit obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Počet řádků v tabulce zobrazí závisí na počtu celkový počet záznamů vázána DataList. Přesněji řečeno ho s dělený se mezní hodnota pro celkový počet položky zdroje dat `RepeatColumns` hodnotu vlastnosti. Vzhledem k tomu `Products` tabulka aktuálně má 84 produkty, které je dělitelná 3, je 28 řádků. Pokud počet položek ve zdroji dat a `RepeatColumns` hodnotu vlastnosti nejsou dělitelná, pak poslední řádek nebo sloupce budou mít prázdné buňky. Pokud `RepeatDirection` je nastaven na `Vertical`, pak posledního sloupce budou mít prázdné buňky; Pokud `RepeatDirection` je `Horizontal`, pak poslední řádek bude mít u prázdných buněk.

## <a name="summary"></a>Souhrn

DataList, ve výchozím nastavení, jsou jeho položky v jednoho sloupce, řádku více tabulku, která napodobuje rozložení GridView s TemplateField jeden. Když toto výchozí rozložení je přijatelné, jsme maximalizovat obrazovky nemovitosti zobrazením více položky zdroje dat na řádek. Způsoby se pouze nastavení DataList s `RepeatColumns` vlastnost počet sloupců pro zobrazení na řádek. Kromě toho DataList s `RepeatDirection` vlastnost lze použít k označení, zda obsah vícesloupcového, více řádků tabulky by měl být nastíněny vodorovně zleva doprava, shora dolů nebo svisle shora dolů, zleva doprava.

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Suru Jan. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
[další](nested-data-web-controls-vb.md)
