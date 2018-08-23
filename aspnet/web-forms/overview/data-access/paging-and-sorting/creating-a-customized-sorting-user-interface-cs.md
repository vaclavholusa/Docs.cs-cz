---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Vytvoření vlastního uživatelského rozhraní pro řazení (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při zobrazování dlouhý seznam řazení dat, může být velmi užitečná k seskupení souvisejících dat zavedením oddělovač řádků. V tomto kurzu uvidíme jak vytv...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 28926276e183ae7cf7ed4087d8c73044741389e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757098"
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Vytvoření vlastního uživatelského rozhraní pro řazení (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) nebo [stahovat PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Při zobrazování dlouhý seznam řazení dat, může být velmi užitečná k seskupení souvisejících dat zavedením oddělovač řádků. V tomto kurzu uvidíme, jak vytvořit takové řazení uživatelské rozhraní.


## <a name="introduction"></a>Úvod

Při zobrazování dlouhý seznam seřadit data tam, kde existují jenom na několik různých hodnot ve sloupci seřazený, koncový uživatel možná pro vás bude obtížné rozpoznat, kdy přesně, hranice rozdíl dojde k. Existují třeba 81 produktů v databázi, ale pouze devět volby jinou kategorii (osm kategorií jedinečný plus `NULL` možnost). Podívejte se na uživatele, kteří chtějí při zkoumání produkty, které spadají do kategorie ryby. Ze stránky, která obsahuje seznam *všechny* produktů v jediné prvek GridView uživateli rozhodnout, je její nejvhodnější seřadit výsledky podle kategorií, které se seskupit dohromady ryby produkty. Po seřazení podle kategorie, uživatel pak musí hunt prostřednictvím seznamu hledání kde produkty seskupené ryby počáteční a koncové. Protože výsledky jsou seřazené podle abecedy podle názvu kategorie hledání ryby produkty není obtížné, ale stále vyžaduje úzce skenování seznamu položek v mřížce.

Chcete-li pomoci odhalit hranice mezi skupinami seřazený, využívat mnoho webů uživatelské rozhraní, které přidá oddělovač mezi tyto skupiny. Oddělovače, jako jsou ty zobrazené na obrázku 1 umožňuje uživatelům rychle najít konkrétní skupinu a identifikovat jeho hranice, jakož i zjistit, jaké odlišné skupiny existují v datech.


[![Každá skupina kategorie je jasně identifikovat.](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Obrázek 1**: každá skupina kategorie je jasně identifikovat ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


V tomto kurzu uvidíme, jak vytvořit takové řazení uživatelské rozhraní.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Krok 1: Vytvoření standardní, seřaditelných GridView

Předtím, než se podíváme na tom, jak rozšířit prvku GridView a poskytuje rozšířené řazení rozhraní, umožňují s nejprve vytvořit standardní, seřaditelných GridView, který obsahuje seznam produktů. Začněte otevřením `CustomSortingUI.aspx` stránku `PagingAndSorting` složky. Přidejte na stránku GridView, nastavte jeho `ID` vlastnost `ProductList`a jeho vazbu na nového prvku ObjectDataSource. Konfigurace ObjectDataSource používat `ProductsBLL` třída s `GetProducts()` metodu pro výběr záznamů.

V dalším kroku nakonfigurujte prvku GridView, tak, aby obsahoval pouze `ProductName`, `CategoryName`, `SupplierName`, a `UnitPrice` BoundFields a třídě CheckBoxField ukončena. A konečně, nakonfigurujte GridView pro podporu třídění zaškrtnutím políčka Povolit řazení v prvku GridView s inteligentním (nebo nastavením jeho `AllowSorting` vlastnost `true`). Po provedení těchto doplňky `CustomSortingUI.aspx` stránce deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Za chvíli doposud zobrazit náš postup v prohlížeči. Obrázek 2 ukazuje seřaditelné GridView při jeho data je seřazený podle kategorií v abecedním pořadí.


[![Řazení ovládacího prvku GridView s Data jsou seřazená podle kategorie](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Obrázek 2**: seřaditelné prvku GridView s Data jsou seřazená podle kategorie ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Krok 2: Zkoumání techniky pro přidání oddělovač řádků

Pomocí obecného, seřaditelných GridView kompletní už jen zbývá umožnit přidání oddělovač řádků v prvku GridView, před každou skupinu jedinečné seřazené. Ale jak můžete tyto řádky vloženy do prvku GridView? V podstatě jsme potřebovat k iteraci v rámci ovládacího prvku GridView s řádky, určete, kdy dojde k rozdíly mezi hodnotami v seřazený sloupec a pak přidejte řádek odpovídající oddělovače. Pokud uvažujete o tomto problému, vypadá to přirozené někde najdete řešení v prvku GridView s `RowDataBound` obslužné rutiny události. Jak jsme probírali v [vlastní formátování založené na Data](../custom-formatting/custom-formatting-based-upon-data-cs.md) výukový program, tato obslužná rutina události se obvykle používá při formátování na úrovni řádků podle data řádku s. Ale `RowDataBound` obslužná rutina události není řešení tady, protože řádků nelze přidat do prvku GridView prostřednictvím kódu programu z této obslužné rutiny události. GridView s `Rows` kolekce, ve skutečnosti je jen pro čtení.

Chcete-li přidat další řádky GridView máme tři možnosti:

- Přidejte tyto řádky oddělovač metadat na skutečná data, která je vázána k prvku GridView.
- Po prvku GridView byl vázán na data, přidat další `TableRow` instance prvku GridView s ovládací prvek kolekce
- Vytvořit vlastní serverový ovládací prvek, který rozšiřuje ovládacím prvku GridView a přepíše tyto metody zodpovědný za vytváření struktury s GridView

Vytváří vlastní serverový ovládací prvek by nejlepším řešením, pokud tato funkce byla potřeba na mnoho webových stránkách nebo přes několik webů. Ale to by znamenalo hodně kódu a důkladným průzkum do hlubin vnitřní činnost GridView s. Proto jsme nebere v úvahu tuto možnost pro účely tohoto kurzu.

Další dvě možnosti přidávání oddělovač řádků, abyste se skutečná data vázaná na prvku GridView a manipulace s nimi kolekce ovládací prvek GridView s po jeho byl vázán – jinak útoku problém a si zasloužila diskusi.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Přidání řádků do Data vázaná na prvku GridView.

Když prvku GridView je vázán na zdroj dat, vytvoří se `GridViewRow` pro každý záznam vrácených zdrojem dat. Proto jsme můžete vložit potřebné oddělovač řádků přidáním oddělovač záznamů ke zdroji dat před jeho vazbu na prvku GridView. Obrázek 3 ilustruje tento koncept.


![Jedna z technik zahrnuje přidání oddělovač řádků do zdroje dat.](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Obrázek 3**: jedna z technik zahrnuje přidání oddělovač řádků do zdroje dat.


Protože neexistuje žádný záznam speciální oddělovače; používám oddělovač záznamů výraz v uvozovkách Místo toho nám, které konkrétní záznam ve zdroji dat slouží jako oddělovač spíše než normální datovém řádku musí nějakým způsobem příznak. Pro naše příklady, můžeme znovu vazby `ProductsDataTable` instance prvku GridView, který se skládá z `ProductRows`. Záznam jsme mohou označit jako oddělovač řádků nastavením jeho `CategoryID` vlastnost `-1` (protože takové hodnotu se nám t obvykle existují).

Aby se začala používat tuto techniku d musíme proveďte následující kroky:

1. Načítání prostřednictvím kódu programu data k vytvoření vazby prvku GridView ( `ProductsDataTable` instance)
2. Řazení dat podle GridView s `SortExpression` a `SortDirection` vlastnosti
3. Iterovat přes `ProductsRows` v `ProductsDataTable`, kteří hledají kde být rozdíly v seřazený sloupec
4. V každé skupině hranic vložit záznam oddělovač `ProductsRow` instance do objektu DataTable, ten, který je s `CategoryID` nastavena na `-1` (nebo jakékoli označení bylo rozhodnuto k označení záznam jako oddělovač záznam)
5. Po vložení oddělovač řádků, prostřednictvím kódu programu vytvořit vazbu dat do prvku GridView.

Kromě těchto pět kroků, d také potřebujeme pro poskytování obslužnou rutinu události pro prvek GridView s `RowDataBound` událostí. Tady, d zkontrolujeme každý `DataRow` a určit, zda byla oddělovač řádků, jeden jehož `CategoryID` nastavení `-1`. Pokud ano, můžeme d pravděpodobně chtít nastavit formátování nebo textu zobrazeného v buňky.

Pomocí této techniky pro vkládání řazení skupině hranic vyžaduje trochu více práce než uvedené výše, musíte také zadat obslužnou rutinu události pro prvek GridView s `Sorting` sledovat události a zachovat `SortExpression` a `SortDirection` hodnoty.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulace GridView s řídit kolekce po něm s byla

Místo zasílání zpráv data před jeho vazbu na prvku GridView, můžeme přidat oddělovač řádků *po* data byla svázána se prvku GridView. Proces vytváření datových vazeb sestavení hierarchie ovládací prvek GridView s, která ve skutečnosti je jednoduše a `Table` instance skládá z kolekce řádků, z nichž každý se skládá z kolekce buněk. Konkrétně obsahuje kolekci ovládací prvek GridView s `Table` objektu na jeho kořenové složky, `GridViewRow` (který je odvozen z `TableRow` třída) pro každý záznam v `DataSource` vázán na prvku GridView a `TableCell` objektu v každé `GridViewRow` instance pro každé datové pole v `DataSource`.

Pro přidání řádků oddělovač mezi každou skupinu řazení, jsme tento ovládací prvek hierarchie manipulovat přímo po jeho vytvoření. Můžeme být jisti, zda byl vytvořen hierarchii ovládací prvek GridView s naposledy v době, kdy vykreslení stránky. Proto, přepíše tento přístup `Page` třída s `Render` metoda v tomto okamžiku hierarchii poslední ovládací prvek GridView s aktualizováno, aby zahrnovalo potřebné oddělovač řádků. Obrázek 4 ukazuje tento proces.


[![Alternativní postup manipuluje hierarchii ovládací prvek GridView s](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Obrázek 4**: alternativní postup manipuluje s GridView hierarchii ovládacích prvků ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


V tomto kurzu použijeme tento druhý přístup k přizpůsobení řazení činnosti koncového uživatele.

> [!NOTE]
> Kód mi m prezentace v tomto kurzu je založen na ukázkové chatovací v [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s blogu, [přehrávání Bit s seskupení řazení ovládacího prvku GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Krok 3: Přidání oddělovač řádků do hierarchie s ovládacího prvku GridView

Protože chceme po hierarchii ovládací prvek byl vytvořen a vytvořen naposledy na tuto stránku navštivte přidat oddělovač řádků do hierarchie s ovládacího prvku GridView, chceme provést toto přidání na konci cyklu stránky, ale před skutečné c GridView hierarchie pro ojů bylo vykresleno do kódu HTML. Je poslední bod je to možné, jakou jsme toho dosáhli `Page` třída s `Render` událost, která nám můžete přepsat v našich použití modelu code-behind třídy pomocí následující podpis metody:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Když `Page` třída s původní `Render` vyvolání metody `base.Render(writer)` všech ovládacích prvků na stránce bude vykreslen, generování kódu na základě jejich hierarchie ovládacího prvku. Proto je nutné, že oba říkáme `base.Render(writer)`, takže se zobrazí na stránce a budeme pracovat s GridView s řízení hierarchie před voláním `base.Render(writer)`tak, aby do hierarchie ovládací prvek GridView s dříve, než byly přidány oddělovač řádků s byl vykreslen.

Řazení skupiny záhlaví vložení musíme nejprve zkontrolujte, že uživatel požaduje, aby data seřadit. Ve výchozím nastavení nejsou seřazené s obsah ovládacího prvku GridView a proto jsme zadávat t musíte zadat jakákoli skupina řazení záhlaví.

> [!NOTE]
> Pokud chcete, který se má seřadit podle konkrétního sloupce při prvním načtení stránky prvku GridView, zavolejte GridView s `Sort` metoda při první návštěvě stránky (ale ne na následné postbacky). K tomu přidat toto volání v `Page_Load` obslužné rutiny události v rámci `if (!Page.IsPostBack)` podmíněný. Vraťte se do [stránkování a řazení dat sestavy](paging-and-sorting-report-data-cs.md) kurz informace pro další informace o `Sort` metody.


Za předpokladu, že bylo seřazeno data, naše dalším krokem je určit, jaké sloupce dat byl seřazen podle a pak prohledávání řádků hledáte rozdíly v daném sloupci s hodnoty. Následující kód zajistí, že data seřazení a najde sloupec, podle kterého bylo seřazeno data:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Pokud má prvku GridView ještě být seřazeny, GridView s `SortExpression` vlastnost nebude byla nastavena. Proto pouze chceme přidat oddělovač řádků, pokud tato vlastnost má některá z hodnot. Pokud ano, potřebujeme dále určit index sloupce, podle kterého se data seřadit. To se provádí ve smyčce GridView s `Columns` shromažďování, vyhledávání pro sloupec, jehož `SortExpression` vlastnost rovná GridView s `SortExpression` vlastnost. Kromě index sloupce s jsme také vzít `HeaderText` vlastnost, která se používá při zobrazování oddělovač řádků.

S indexem sloupce, podle kterého má řazení proběhnout data je posledním krokem je výčet řádků z prvku GridView. Pro každý řádek potřebujeme k určení, zda hodnota s seřazený sloupec se liší od předchozí hodnotu s s seřazený sloupec řádku. Pokud ano, musíme vložit nový `GridViewRow` instanci na hierarchii ovládacího prvku. To lze provést následujícím kódem:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Tento kód spustí programově odkazováním `Table` objektu najít v kořenovém adresáři hierarchii ovládací prvek GridView s a vytvoření řetězcové proměnné s názvem `lastValue`. `lastValue` Umožňuje porovnat aktuální hodnota sloupce s řazení řádků s předchozí řádek s hodnotou. Další, GridView s `Rows` výčtu kolekce a pro každý řádek je uložená hodnota seřazený sloupec v `currentValue` proměnné.

> [!NOTE]
> Chcete-li zjistit hodnotu sloupce s seřazené konkrétního řádku používám buňky s `Text` vlastnost. Tento postup funguje dobře pro BoundFields, ale nebude fungovat podle potřeby vlastností TemplateField CheckBoxFields a tak dále. Podíváme se na postup pro alternativní pole ovládacího prvku GridView účet za chvíli.


`currentValue` a `lastValue` proměnné jsou pak porovnána. Pokud se liší potřebujeme přidat nový řádek oddělovač hierarchii ovládacích prvků. To lze provést tak, že určíte index `GridViewRow` v `Table` objektu s `Rows` kolekce, vytváří se nová `GridViewRow` a `TableCell` instancí a následným přidáním `TableCell` a `GridViewRow` k ovládací prvek hierarchie.

Všimněte si, že oddělovač řádků s jedinou `TableCell` je formátováno tak, že pokrývá celou šířku prvku GridView, je naformátována pomocí `SortHeaderRowStyle` třídu šablony stylů CSS a má jeho `Text` tak, že zobrazuje skupině řazení název vlastnosti (například kategorie) a Hodnota s skupiny (například nápoje). Nakonec `lastValue` se aktualizuje na hodnotu `currentValue`.

Třídu šablony stylů CSS použitý pro řazení řádek záhlaví skupiny `SortHeaderRowStyle` musí být zadaná v `Styles.css` souboru. Nebojte se využít odvolat jakékoliv nastavení stylu pro vás. Můžu použít následující:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

S aktuálním kódem řazení rozhraní přidá řazení skupiny záhlaví při řazení podle libovolné Vlastnost BoundField (viz obrázek 5, který ukazuje snímek obrazovky při řazení podle dodavatele). Ale při řazení podle jiný typ pole (například třídě CheckBoxField nebo TemplateField), řazení skupiny záhlaví nejsou nikde najít (viz obrázek 6).


[![Řazení rozhraní zahrnuje seřadit a seskupit záhlaví při řazení podle BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Obrázek 5**: The řazení rozhraní zahrnuje řazení skupiny záhlaví při řazení podle BoundFields ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Záhlaví skupiny řazení jsou chybějící při řazení třídě CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Obrázek 6**: záhlaví skupiny řazení jsou chybějící při řazení třídě CheckBoxField ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Z důvodu při řazení podle třídě CheckBoxField chybí záhlaví řazení skupin je vzhledem k tomu kód aktuálně používá pouze `TableCell` s `Text` vlastnosti k určení hodnoty seřazený sloupec pro každý řádek. Pro CheckBoxFields `TableCell` s `Text` vlastnosti je prázdný řetězec; místo toho je k dispozici prostřednictvím ovládacího prvku zaškrtávací políčko Web, který se nachází v rámci hodnoty `TableCell` s `Controls` kolekce.

Pro zpracování pole typů jiných než BoundFields, potřebujeme k posílení kód kam `currentValue` proměnná je přiřazená k provedení kontroly existence zaškrtávací políčko ve `TableCell` s `Controls` kolekce. Namísto použití `currentValue = gvr.Cells[sortColumnIndex].Text`, nahraďte tento kód následujícím kódem:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Tento kód prozkoumá seřazený sloupec `TableCell` pro aktuální řádek určí, jestli jsou všechny ovládací prvky v `Controls` kolekce. Pokud existuje a je první ovládací prvek zaškrtávací políčko, `currentValue` proměnná je nastavená na Ano nebo Ne, v závislosti na zaškrtávací políčko s `Checked` vlastnost. V opačném případě hodnota je převzata z `TableCell` s `Text` vlastnost. Tuto logiku je možné replikovat do zpracování řazení pro všechny vlastností TemplateField, které mohou existovat v prvku GridView.

Výše uvedené přidání kódu, hlaviček skupiny řazení jsou teď k dispozici při řazení ve třídě CheckBoxField ukončena (viz obrázek 7).


[![Záhlaví skupiny řazení nejsou nyní k dispozici při řazení třídě CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Obrázek 7**: záhlaví skupiny řazení nejsou nyní k dispozici při řazení třídě CheckBoxField ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Pokud máte produkty s `NULL` hodnoty pro databáze `CategoryID`, `SupplierID`, nebo `UnitPrice` pole, tyto hodnoty se zobrazí jako prázdné řetězce v prvku GridView. ve výchozím nastavení, což znamená text řádku s oddělovač pro tyto produkty s `NULL`hodnoty se číst jako kategorie: (to znamená, existovat s žádný název podle kategorie: kategorie, jako jsou: nápoje). Pokud chcete hodnoty zobrazené Zde můžete nastavit buď BoundFields [ `NullDisplayText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) textu, které chcete zobrazit nebo podmíněného příkazu můžete přidat do metody vykreslení při přiřazování `currentValue` pro oddělovače řádek s `Text` vlastnost.


## <a name="summary"></a>Souhrn

GridView nezahrnuje řadu integrovaných možností pro přizpůsobení rozhraní pro řazení. Nicméně s hodně kódem nízké úrovně je možné upravit hierarchii ovládací prvek GridView s k vytvoření více přizpůsobit rozhraní s. V tomto kurzu jsme viděli, jak přidat řádek oddělovače skupiny řazení pro seřaditelné prvku GridView, snadněji identifikující odlišné skupiny a tyto skupiny hranic. Další příklady vlastní řazení rozhraní, projděte si [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [několik ASP.NET 2.0 GridView řazení tipy a triky](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) blogu.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](sorting-custom-paged-data-cs.md)
> [další](paging-and-sorting-report-data-vb.md)
