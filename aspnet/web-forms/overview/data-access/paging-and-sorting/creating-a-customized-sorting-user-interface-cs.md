---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Vytváření vlastní řazení uživatelské rozhraní (C#) | Microsoft Docs
author: rick-anderson
description: Při zobrazení dlouhý seznam seřazený dat, může být velmi užitečné k seskupení souvisejících dat zavedením oddělovač řádků. V tomto kurzu ukážeme, jak vytvořit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: c2680f5e47883c9d5fa874a36eb666270c5e406a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-customized-sorting-user-interface-c"></a>Vytváření vlastní řazení uživatelské rozhraní (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) nebo [stáhnout PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Při zobrazení dlouhý seznam seřazený dat, může být velmi užitečné k seskupení souvisejících dat zavedením oddělovač řádků. V tomto kurzu vidíte vytvoření takové řazení uživatelské rozhraní.


## <a name="introduction"></a>Úvod

Při zobrazení dlouhý seznam seřazen data tam, kde jsou pouze několik různých hodnot ve sloupci seřazená, koncový uživatel může být obtížné rozpoznat, kdy, přesně, hranice rozdíl dojít k. Například jsou 81 produkty v databázi, ale pouze devět volby různé kategorie (osm jedinečný kategorií a `NULL` možnost). Podívejte se uživatele, který má zájem o zkoumání produkty, které spadají pod ryby kategorii. Ze stránky, která uvádí *všechny* produktů v jednom GridView uživatele rozhodnout, je jeho nejvhodnější seřadit výsledky podle kategorií, které budou seskupeny dohromady všechny ryby produkty společně. Po řazení podle kategorie, uživatel pak musí hunt prostřednictvím seznamu, vyhledávání kde produkty seskupené ryby začínat a končit. Vzhledem k tomu, že výsledky jsou seřazené podle abecedy podle název kategorie hledání produkty ryby není složité, ale stále vyžaduje úzce kontrolu seznamu položek v mřížce.

Chcete-li pomoci odhalit hranice mezi seřazené skupiny, využívat mnoho webů uživatelské rozhraní, které přidá oddělovač mezi takové skupiny. Oddělovače, jako jsou zobrazené na obrázku 1 umožňuje uživatelům rychleji najít konkrétní skupinu a určit její hranice, a také zjistit, jaké odlišné skupiny existovat v datech.


[![Každá skupina kategorie je jednoznačně identifikovat.](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Obrázek 1**: každou kategorii skupina je jednoznačně identifikuje ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image3.png))


V tomto kurzu vidíte vytvoření takové řazení uživatelské rozhraní.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Krok 1: Vytvoření GridView standardní, řazení

Před nám prozkoumat postup posílení GridView zajistit rozšířené řazení rozhraní, umožní s nejprve vytvořit GridView standardní, řazení, které jsou uvedeny produkty. Začněte otevřením `CustomSortingUI.aspx` stránku `PagingAndSorting` složky. Přidat GridView na stránku, nastavte jeho `ID` vlastnost `ProductList`a navázat jej nové ObjectDataSource. Konfigurace ObjectDataSource používat `ProductsBLL` třídu s `GetProducts()` metoda pro výběr záznamů.

V dalším kroku nakonfigurujte GridView tak, aby obsahoval pouze `ProductName`, `CategoryName`, `SupplierName`, a `UnitPrice` BoundFields a vlastnost CheckBoxField zastaveny. Nakonec, nakonfigurujte GridView pro podporu řazení zaškrtnutím políčka Povolit řazení v GridView s inteligentním (nebo nastavením jeho `AllowSorting` vlastnost `true`). Po provedení těchto doplňky `CustomSortingUI.aspx` stránky, deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Chcete-li zobrazit naše průběh doposud v prohlížeči chvíli trvat. Obrázek 2 ukazuje řazení GridView při jeho data jsou seřazená podle kategorie v abecedním pořadí.


[![Rutina GridView řazení s dat je seřazené podle kategorie](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Obrázek 2**: řazení GridView s dat je seřazené podle kategorie ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Krok 2: Zkoumat techniky pro přidání oddělovač řádků

S Obecné, řazení GridView dokončení jen zbývá moct přidat oddělovač řádků v GridView před každou jedinečnou skupinu seřazená. Ale jak můžete tyto řádky vloženy do GridView? V podstatě jsme potřebovat k iteraci v rámci GridView s řádky, určete, kde probíhat rozdíly mezi hodnotami v seřazený sloupec a poté přidejte příslušný oddělovač řádků. Pokud přemýšlíte o tento problém, nejspíš přirozené někde spočívá řešení v GridView s `RowDataBound` obslužné rutiny události. Jak již bylo zmíněno [vlastní formátování dat na základě po](../custom-formatting/custom-formatting-based-upon-data-cs.md) kurzu této obslužné rutiny události se běžně používá, pokud formátování na úrovni řádků v závislosti na data řádku s. Ale `RowDataBound` obslužné rutiny události není v tomto řešení jako řádky nelze přidat do GridView prostřednictvím kódu programu z této obslužné rutiny události. Rutina GridView s `Rows` kolekce, ve skutečnosti je jen pro čtení.

Chcete-li přidat další řádky k GridView jsme k dispozici tři možnosti:

- Přidat tyto řádky oddělovače metadata na skutečná data, která je vázána na GridView
- Po GridView byla svázána se data, přidejte další `TableRow` instance, které chcete GridView s řízení kolekce
- Vytvořit vlastní serverové ovládací prvek, který rozšiřuje prvek GridView a přepíše tyto metody, která je zodpovědná za vytváření strukturu GridView s

Vytvoření ovládacího prvku vlastního serveru by je nejlepším postupem podle potřeby tato funkce byla v mnoha webových stránek nebo napříč několika weby. Ale by za následek s bit kódu a důkladné průzkum do depths GridView s vnitřní strukturu a funkci. Proto není popíšeme tuto možnost pro účely tohoto kurzu.

Další dva možnosti přidání oddělovač řádků na skutečná data je vázána na GridView a manipulace s nimi GridView s kontrolní kolekcí po jeho byla vázána – jinak útokům problém a si zasloužila se zabývat.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Přidání řádků k datům vázána na GridView

Když GridView vázán na zdroj dat, vytvoří `GridViewRow` pro každý záznam zdroj dat vrátil. Proto můžete vložíme potřebné oddělovač řádků přidáním oddělovače záznamy ke zdroji dat před vazby k GridView. Obrázek 3 znázorňuje tento koncept.


![Jednoho technika, zahrnuje přidání oddělovač řádků ke zdroji dat](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Obrázek 3**: jeden technika, zahrnuje přidání oddělovač řádků ke zdroji dat


Použít oddělovače záznamy termín do uvozovek a protože neexistuje žádný záznam speciální oddělovače; Místo toho jsme musí nějakým způsobem příznak, které konkrétní záznam ve zdroji dat slouží jako oddělovač místo normální data v řádku. Pro našich ukázkách, jsme re vazby `ProductsDataTable` instance GridView, který se skládá z `ProductRows`. Jsme může příznak záznam jako oddělovače řádek nastavením jeho `CategoryID` vlastnost `-1` (protože takové hodnota se nedal neexistuje normálně).

Abyste mohli využívat tato technika d musíme proveďte následující kroky:

1. Načítání dat pro vazbu GridView prostřednictvím kódu programu ( `ProductsDataTable` instance)
2. Řazení dat podle GridView s `SortExpression` a `SortDirection` vlastnosti
3. Iterace `ProductsRows` v `ProductsDataTable`, kteří hledají kde ležet rozdíly v seřazený sloupec
4. V každé skupině hranic, vložit záznam oddělovače `ProductsRow` instance do DataTable, ten, který je s `CategoryID` nastavena na `-1` (nebo libovolnou označení byl rozhodnuto označit záznam jako oddělovače záznam)
5. Po vložení oddělovač řádků, prostřednictvím kódu programu vazby dat k GridView

Kromě těchto pět kroků d také je potřeba zadat obslužné rutiny události pro GridView s `RowDataBound` událostí. Zde jsme d každý zkontrolujte `DataRow` a určit, jestli bylo oddělovač řádek, jeden jejichž `CategoryID` bylo nastavení `-1`. Pokud ano, jsme d pravděpodobně chtít upravit jeho formátování nebo text zobrazovaný v buňky.

Touto technikou pro vložení řazení skupině hranic vyžaduje trochu další práci, než uvedených výše, jako je třeba také zadat obslužné rutiny události pro GridView s `Sorting` události a pravidelně sledovat z `SortExpression` a `SortDirection` hodnoty.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulace GridView s řízení kolekci po jeho s generována

Místo zasílání zpráv data před vazby k GridView, přidáme oddělovač řádků *po* data byla svázána se GridView. Proces vázání dat vytvoří rutina GridView s řízení hierarchie, která ve skutečnosti je jednoduše a `Table` instance tvořený bude kolekce řádků, z nichž každý se skládá z kolekce buněk. Konkrétně GridView s kontrolní kolekcí obsahuje `Table` objekt v jeho kořenové `GridViewRow` (který je odvozen z `TableRow` – třída) pro každý záznam v `DataSource` vázána na GridView a `TableCell` objekt v každé `GridViewRow` instance pro každé datové pole v `DataSource`.

Pokud chcete přidat oddělovač řádků mezi každou skupinou řazení, jsme tuto hierarchii ovládací prvek upravit přímo po vytvoření. Jsme jistotu hierarchii ovládacích prvků s GridView vytvořila naposledy o dobu, kterou vykreslení stránky. Proto tento přístup přepsání `Page` třídu s `Render` metody okamžiku hierarchii GridView s posledním ovládacích prvků aktualizovala a obsahovala potřebné oddělovač řádků. Tento proces je znázorněný na obrázku 4.


[![Alternativní postup manipuluje hierarchii GridView s ovládacích prvků](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Obrázek 4**: alternativní postup manipuluje s GridView hierarchii ovládacích prvků ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image10.png))


V tomto kurzu používáme tento pozdější přístup k přizpůsobení řazení činnost koncového uživatele.

> [!NOTE]
> Kód I m prezentace v tomto kurzu vychází z v příkladu v [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s položku blogu, [přehrávání Bit s seskupení řazení GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Krok 3: Přidání oddělovač řádků do hierarchii GridView s ovládacích prvků

Vzhledem k tomu, že chceme po byla vytvořena a vytvořit naposledy při návštěvě této stránky hierarchii řízení přidat oddělovač řádků hierarchii GridView s ovládacích prvků, chceme k provedení tohoto přidání na konci životního cyklu stránky, ale před skutečným GridView c hierarchie obrazit řídicí vykreslena do kódu HTML. Je nejnovější možné bod, ve kterém jsme se dá dosáhnout `Page` třídu s `Render` událost, která jsme můžete přepsat v našem třídy kódu pomocí následující podpis metody:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Když `Page` třídu s původní `Render` metoda je volána `base.Render(writer)` jednotlivých ovládacích prvků na stránce bude vykreslen, generování značek na základě jejich hierarchie ovládacího prvku. Proto je nutné, aby obě říkáme `base.Render(writer)`tak, aby vykreslení stránky hierarchii před voláním řízení a že jsme manipulaci GridView s `base.Render(writer)`tak, aby byly přidány oddělovač řádků do hierarchie s řízení GridView dříve, než se s byl vykreslen.

Vložení záhlaví skupiny řazení musíme nejprve zkontrolujte, zda uživatel vyžaduje řadit data. Ve výchozím nastavení nejsou seřazené obsah s GridView, a proto jsme nejsou zobrazeny t muset zadávat žádnou skupinu řazení hlavičky.

> [!NOTE]
> Pokud chcete GridView, který se má seřadit podle určitého sloupce při prvním načtení stránky, volání GridView s `Sort` metoda při první návštěvě stránky (ale ne na následné postback). K tomu, přidejte toto volání v `Page_Load` obslužné rutiny události v rámci `if (!Page.IsPostBack)` podmíněného. Odkazovat zpět [stránkování a řazení dat sestavy](paging-and-sorting-report-data-cs.md) kurz informace pro další informace o `Sort` metoda.


Za předpokladu, že data byla seřazená, naše další úlohy je určit, jaké sloupec data byla seřazená podle a pak ke kontrole řádky hledá rozdíly v tomto sloupci s hodnoty. Následující kód zajišťuje, že data seřazení a vyhledá sloupec, podle kterého data seřazená:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Pokud má GridView ještě být seřazen, rutina GridView s `SortExpression` nebude nebyla nastavena vlastnost. Proto chceme jenom přidat oddělovač řádků, pokud je tato vlastnost má určitou hodnotu. Pokud ano, potřebujeme další určit index sloupce, podle kterého se data seřazená. To lze provést ve smyčce přes GridView s `Columns` kolekce, hledání pro sloupec, jehož `SortExpression` vlastnost rovná GridView s `SortExpression` vlastnost. Kromě index sloupce s jsme také získat `HeaderText` vlastnosti, která se používá při zobrazení oddělovač řádků.

S indexem sloupce data jsou seřazená posledním krokem je vytvoření výčtu řádky GridView. Pro každý řádek je potřeba určit, zda hodnota s seřazený sloupec se liší od předchozí řádek s seřadit sloupec s hodnotu. Pokud ano, musíme vložit nový `GridViewRow` instance do ovládacího prvku hierarchie. To se provádí následujícím kódem:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Tento kód spustí prostřednictvím kódu programu odkazem `Table` objekt nalezen na nejnižší úrovni hierarchie ovládacího prvku s GridView a vytvořit řetězec proměnné s názvem `lastValue`. `lastValue` se používá k porovnání aktuální hodnota sloupce s seřadit řádek s předchozí řádek s hodnotou. Další, rutina GridView s `Rows` je vytvořena kolekce a pro každý řádek hodnotu seřazený sloupec je uložená v `currentValue` proměnné.

> [!NOTE]
> K určení hodnoty sloupce s seřadit konkrétního řádku používám buňky s `Text` vlastnost. Tento postup funguje dobře pro BoundFields, ale nebude fungovat podle potřeby pro TemplateFields, CheckBoxFields a tak dále. Podíváme postup účet pro alternativní GridView pole za chvíli.


`currentValue` a `lastValue` proměnné jsou potom porovná. Pokud se liší, je potřeba přidat nový řádek oddělovače hierarchii ovládacích prvků. Toho dosahuje tak, že určíte index `GridViewRow` v `Table` objekt s `Rows` kolekce, vytváření nových `GridViewRow` a `TableCell` instancí a následným přidáním `TableCell` a `GridViewRow` do ovládací prvek hierarchie.

Všimněte si, že oddělovač řádků s jedinou `TableCell` je naformátován tak, aby ho zahrnuje celou šířku GridView, je formátován pomocí `SortHeaderRowStyle` třídy CSS a jeho `Text` vlastnost tak, že zobrazuje skupině řazení název (například kategorie) a Hodnota s skupiny (například nápoje). Nakonec `lastValue` se aktualizuje na hodnotu `currentValue`.

Třída CSS použitá k formátování řazení řádek záhlaví skupiny `SortHeaderRowStyle` je třeba zadat v `Styles.css` souboru. Můžete použít libovolnou nastavení stylů odvoláním proti vám; Mohu použít následující:


[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Aktuální kód, přidá rozhraní řazení záhlaví skupiny řazení při řazení podle libovolné BoundField (viz obrázek 5, který ukazuje snímek při řazení podle dodavatele). Pokud však řazení podle jiný typ pole (například Třída CheckBoxField nebo TemplateField), záhlaví skupiny řazení jsou nikde nalezen (viz obrázek 6).


[![Řazení rozhraní obsahuje záhlaví skupiny řazení při řazení podle BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Obrázek 5**: řazení rozhraní zahrnuje řazení skupiny záhlaví při řazení podle BoundFields ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image13.png))


[![Záhlaví skupiny řazení nejsou chybí při řazení Vlastnost CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Obrázek 6**: jsou záhlaví skupiny řazení chybí při řazení Vlastnost CheckBoxField ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image16.png))


Z důvodu při řazení pomocí třídy CheckBoxField chybí záhlaví skupiny řazení totiž kód aktuálně používá jenom `TableCell` s `Text` vlastnosti k určení hodnoty seřazený sloupec pro každý řádek. Pro CheckBoxFields `TableCell` s `Text` vlastnost je řetězec prázdný; místo toho je k dispozici prostřednictvím ovládacího prvku políčko Web, který se nachází v rámci hodnota `TableCell` s `Controls` kolekce.

Pro zpracování pole jiného typu než BoundFields, musíme posílení kód kde `currentValue` proměnné je přiřazena k Zkontrolujte existenci zaškrtávací políčko v `TableCell` s `Controls` kolekce. Místo použití `currentValue = gvr.Cells[sortColumnIndex].Text`, nahraďte tento kód následujícím kódem:


[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Tento kód prozkoumá seřazený sloupec `TableCell` pro aktuální řádek k určení, zda jsou všechny ovládací prvky v `Controls` kolekce. Pokud existují, a je první ovládací prvek zaškrtávací políčko, `currentValue` proměnná je nastavená na hodnotu Ano nebo Ne, v závislosti na zaškrtávací políčko s `Checked` vlastnost. Jinak hodnota je převzata z `TableCell` s `Text` vlastnost. Tato logika je možné replikovat pro zpracování řazení pro všechny TemplateFields, který může existovat v GridView.

Výše uvedený kód uveďte záhlaví skupiny řazení jsou teď při řazení pomocí třídy CheckBoxField zastaveny (viz obrázek 7).


[![Záhlaví skupiny řazení se nyní nachází při řazení Vlastnost CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Obrázek 7**: záhlaví skupiny řazení se nyní nachází při řazení Vlastnost CheckBoxField ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-customized-sorting-user-interface-cs/_static/image19.png))


> [!NOTE]
> Pokud máte produkty s `NULL` hodnoty pro databáze `CategoryID`, `SupplierID`, nebo `UnitPrice` pole, tyto hodnoty se zobrazí jako prázdné řetězce v GridView ve výchozím nastavení znamená text řádku s oddělovač pro tyto produkty s `NULL`hodnoty se čtou jako kategorie: (to znamená, že s žádný název po kategorie: jako s kategorie: nápoje). Pokud chcete hodnoty zobrazené Zde můžete nastavit buď BoundFields [ `NullDisplayText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) na text, které chcete zobrazit, nebo můžete přidat příkaz podmíněného v metodě vykreslení při přiřazování `currentValue` pro oddělovač řádek s `Text` vlastnost.


## <a name="summary"></a>Souhrn

GridView nezahrnuje mnoho předdefinovaných možností pro přizpůsobení řazení rozhraní. Nicméně s bit nízké úrovně kódu je možné upravit hierarchii GridView s ovládacích prvků k vytvoření více přizpůsobené rozhraní s. V tomto kurzu jsme viděli, jak přidat řádek oddělovače skupiny řazení pro řazení GridView, které snadněji identifikuje odlišné skupiny a tyto skupiny hranic. Další příklady vlastní řazení rozhraní, podívejte se na [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [několik ASP.NET 2.0 GridView řazení tipy a triky](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) položku blogu.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](sorting-custom-paged-data-cs.md)
> [další](paging-and-sorting-report-data-vb.md)
