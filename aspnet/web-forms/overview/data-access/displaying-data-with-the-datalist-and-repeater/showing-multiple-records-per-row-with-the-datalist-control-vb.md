---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Zobrazení více záznamů na řádku ovládacím prvkem DataList (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto krátkém kurzu podíváme, jak přizpůsobit rozložení prvku DataList prostřednictvím její vlastnosti RepeatColumns a RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 7a23e8897e4fbdf84c4d6916c22bad9687fa5ee6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756622"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Zobrazení více záznamů na řádku ovládacím prvkem DataList (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) nebo [stahovat PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> V tomto krátkém kurzu podíváme, jak přizpůsobit rozložení prvku DataList prostřednictvím její vlastnosti RepeatColumns a RepeatDirection.


## <a name="introduction"></a>Úvod

Příklady ovládacích prvků DataList jsme viděli v posledních dvou kurzech ve mít každý záznam z jeho zdroje dat takto řádků ve formátu HTML jedním sloupcem `<table>`. Když je výchozí chování DataList, je velmi snadné přizpůsobení zobrazení v prvku DataList tak, aby položky zdroje dat jsou rozděleny mezi více sloupci, více řádky tabulky. Kromě toho je možné mít všechna data s zdroje položky zobrazené v a jednoho řádku s více sloupci v prvku DataList.

Můžeme přizpůsobit rozložení ovládacích prvků DataList s prostřednictvím jeho `RepeatColumns` a `RepeatDirection` vlastnosti, které, označuje, kolik sloupců jsou generovány a zda tyto položky jsou rozloženy vodorovně nebo svisle. Obrázek 1 příkladem DataList, která zobrazí informace o produktech v tabulce se třemi sloupci.


[![Prvku DataList zobrazovala tři produkty na každém řádku](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Obrázek 1**: ovládacích prvcích DataList ukazuje tři produkty na každém řádku ([kliknutím ji zobrazíte obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Zobrazením více zdrojové položky dat na každém řádku prvku DataList efektivněji využívat místo na obrazovce vodorovné. V tomto krátkém kurzu se podíváme tyto dvě vlastnosti ovládacích prvků DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Krok 1: Zobrazení informací o produktu v a v prvku DataList

Předtím, než se Zaměřujeme `RepeatColumns` a `RepeatDirection` vlastností, let s nejprve vytvořit a v prvku DataList na naší stránce s informacemi o produktu pomocí rozložení standardní jedním sloupcem, více řádky tabulky. V tomto příkladu umožní s zobrazení s názvem produktu, kategorie a ceny pomocí následující kód:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Jsme ve viděli, jak svázat data s DataList v předchozích příkladech, takže pomocí těchto kroků budete přesunu rychle. Začněte otevřením `RepeatColumnAndDirection.aspx` stránku `DataListRepeaterBasics` složky a a v prvku DataList přetáhněte z panelu nástrojů do návrháře. Z inteligentních značek v prvku DataList s rozhodnout vytvořit nového prvku ObjectDataSource a nakonfigurovat, aby vyžádat data z `ProductsBLL` třída s `GetProducts` metoda výběru (žádný) možnost z průvodce s INSERT, UPDATE a odstranit záložky.

Po vytvoření a připojení nového prvku ObjectDataSource k prvku DataList, vytvoří Visual Studio automaticky `ItemTemplate` , který zobrazí název a hodnotu pro každé pole data produktu. Upravit `ItemTemplate` buď přímo prostřednictvím deklarativní, nebo upravit šablony v prvku DataList s inteligentním možnost tak, aby používala vidíte výše, nahraďte kód *název produktu*, *název kategorie* , a *cena* text pomocí ovládacích prvků popisků, které používají syntaxi příslušné datové vazby k přiřazení hodnot k jejich `Text` vlastnosti. Po aktualizaci `ItemTemplate`, stránka s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Všimněte si, že, které jsem zahrnutý ve specifikátoru formátu v `Eval` syntaxe vázání dat `UnitPrice`, formátování vrácené hodnoty jako měnu - `Eval("UnitPrice", "{0:C}").`

Za chvíli navštivte stránku v prohlížeči. Jak znázorňuje obrázek 2 prvku DataList vykreslí jako jedním sloupcem, více řádky tabulky produktů.


[![Ve výchozím nastavení, vykreslení ovládacích prvků DataList jako jedním sloupcem, více řádky tabulky](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Obrázek 2**: ve výchozím nastavení, v prvku DataList vykreslí jako jedním sloupcem, více řádky tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Krok 2: Změna směr rozložení ovládacích prvků DataList s

Při výchozí chování pro prvku DataList, je rozložení jeho položek svisle v jedním sloupcem, více řádky tabulky, toto chování lze snadno změnit prostřednictvím datového prvku DataList s [ `RepeatDirection` vlastnost](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Vlastnost může přijmout jednu ze dvou možných hodnot: `Horizontal` nebo `Vertical` (výchozí).

Změnou `RepeatDirection` vlastnost z `Vertical` k `Horizontal`, prvku DataList vykreslí své záznamy v jediném řádku, vytváření jeden sloupec na zdrojové položky dat. Pro ilustraci tohoto efektu, klikněte na ovládacím prvku DataList v návrháři a potom v okně Vlastnosti změňte `RepeatDirection` vlastnost z `Vertical` k `Horiztonal`. Okamžitě po tak učiníte, návrháře upraví rozložení ovládacích prvků DataList s vytvořením jednoho řádku s více sloupci rozhraní (viz obrázek 3).


[![Jsou položky RepeatDirection vlastnost určuje, jak the směr DataList s rozložením navýšení kapacity](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Obrázek 3**: `RepeatDirection` vlastnost určuje, jak jsou položky směr DataList s rozložením Out ([kliknutím ji zobrazíte obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Při zobrazení malého množství dat, jednoho řádku vícesloupcové tabulky může být ideální způsob, jak maximalizovat plochy obrazovky. Pro větší objemy dat ale jeden řádek budou vyžadovat mnoho sloupců, které nabízených oznámení ty položky tohoto t můžete přizpůsobit na obrazovce vypnout na pravé straně. Obrázek 4 ukazuje produkty při vykreslení v a jednoho řádku v prvku DataList. Protože je mnoho produktů (více než 80), uživatel bude mít přejděte úplně doprava, abyste viděli informace o jednotlivých produktů.


[![Zdroje dat dostatečně velký a jeden sloupec v prvku DataList bude vyžadovat, aby se vodorovného posouvání](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Obrázek 4**: pro dostatečně velké zdrojů dat, jeden sloupec v prvku DataList bude vyžadovat vodorovného posouvání ([kliknutím ji zobrazíte obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Krok 3: Zobrazení dat v tabulce více sloupci, více řádky

K vytvoření a více sloupci, více řádků v prvku DataList, musíme nastavit [ `RepeatColumns` vlastnost](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) počet zobrazovaných sloupců. Ve výchozím nastavení `RepeatColumns` je nastavena na 0, což způsobí, že DataList zobrazíte všechny položky v jediném řádku nebo sloupce (v závislosti na hodnotu `RepeatDirection` vlastnost).

V našem příkladu umožní s zobrazit tři produkty každý řádek tabulky. Proto nastavte `RepeatColumns` vlastnost na 3. Po provedení této změny, věnujte chvíli zobrazení výsledků v prohlížeči. Jak je vidět na obrázku 5, produkty jsou teď uvedené v tři sloupce, více řádky tabulky.


[![Na každém řádku se zobrazí tři produkty](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Obrázek 5**: tři produkty jsou zobrazeny na každém řádku ([kliknutím ji zobrazíte obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection` Vlastnost ovlivňuje, jak jsou rozloženy položky v ovládacím prvku DataList. Obrázek 5 ukazuje výsledky s `RepeatDirection` nastavenou na `Horizontal`. Všimněte si, že první tři produkty Chai Chang a anýzového sirupu jsou rozloženy zleva doprava a shora dolů. Následující tři produkty (začíná s Chef Anton Cajun Seasoning) se zobrazí pod první tři po sobě. Změna `RepeatDirection` vlastnost zpět do `Vertical`, ale rozložen tyto produkty shora dolů, zleva doprava, jak znázorňuje obrázek 6.


[![Tady jsou produkty podle si svisle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Obrázek 6**: tady, jsou produkty podle si svisle ([kliknutím ji zobrazíte obrázek v plné velikosti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Počet řádků zobrazených ve výsledné tabulce závisí na počtu celkový počet záznamů, které jsou vázány na ovládacím prvku DataList. Přesněji řečeno ji s horní mez celkový počet položek datového zdroje dělený `RepeatColumns` hodnotu vlastnosti. Vzhledem k tomu, `Products` tabulka aktuálně má 84 produkty, což je dělitelná 3, je 28 řádků. Pokud počet položek ve zdroji dat a `RepeatColumns` hodnota vlastnosti nejsou dělitelné a poslední řádek nebo sloupec bude mít prázdné buňky. Pokud `RepeatDirection` je nastavena na `Vertical`, pak posledního sloupce budou mít u prázdných buněk; pokud `RepeatDirection` je `Horizontal`, poslední řádek bude mít prázdné buňky.

## <a name="summary"></a>Souhrn

DataList, ve výchozím nastavení, uvádí jeho položky v tabulce jedním sloupcem, více řádky, která napodobuje rozložení prvek GridView s TemplateField jeden. Tento výchozí rozložení je přijatelné, jsme zobrazením více zdrojové položky dat na každém řádku maximalizovat plochy obrazovky. Provádění to stačí nastavit DataList s `RepeatColumns` na počet sloupců zobrazených na řádek. Kromě toho DataList s `RepeatDirection` vlastnost lze použít k označení, zda obsah více sloupci, více řádky tabulky by měl rozloží vodorovně zleva doprava a shora dolů nebo svisle shora dolů, zleva doprava.

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Suru Jan. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [další](nested-data-web-controls-vb.md)
