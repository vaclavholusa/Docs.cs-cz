---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Zobrazení souhrnných informací v prvku GridView zápatí (VB) | Microsoft Docs
author: rick-anderson
description: V dolní části sestavy v souhrnu řádek je často zobrazí souhrnné informace. Ovládací prvek GridView může obsahovat zápatí řádek, na jejichž buněk můžeme pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: d9a1a3f3c680f367395f984254da6cdcdd3c08d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877613"
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Zobrazení souhrnných informací v prvku GridView zápatí (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) nebo [stáhnout PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> V dolní části sestavy v souhrnu řádek je často zobrazí souhrnné informace. Rutina GridView řízení může zahrnovat zápatí řádek, na jejichž buněk můžete programově vložíme agregovaná data. V tomto kurzu jsme zobrazí, jak zobrazit agregovaná data v tomto řádku zápatí stránky.


## <a name="introduction"></a>Úvod

Kromě v pořadí a minimální počet zobrazuje všechny tyto produkty ceny, jednotek na skladě, jednotky, může uživatel také zajímat agregační informace, například průměrná cena, celkový počet jednotek na skladě a tak dále. V dolní části sestavy v souhrnu řádek se často zobrazí souhrnné informace. Rutina GridView řízení může zahrnovat zápatí řádek, na jejichž buněk můžete programově vložíme agregovaná data.

Tato úloha uvede nám s tři problémy:

1. Konfigurace GridView zobrazíte jeho řádek zápatí
2. Určení shrnutí dat; To znamená jak jsme výpočetní průměrná cena nebo celkový počet jednotek ve skladu?
3. Vložení souhrnných dat do příslušné buněk řádku zápatí

V tomto kurzu jsme zobrazí, jak lze vyřešit tyto problémy. Konkrétně vytvoříme stránky, který obsahuje seznam kategorií v rozevíracím seznamu s produkty vybrané kategorie se zobrazí v GridView. GridView bude obsahovat zápatí řádek, který zobrazuje průměrná cena a celkový počet jednotek v stock a v pořadí pro produkty do této kategorie spadají.


[![Souhrnné informace se zobrazí v řádku zápatí GridView.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Obrázek 1**: Souhrn informace se zobrazují v prvku GridView zápatí řádek ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


V tomto kurzu se jeho kategorie produktů a podrobností rozhraní, staví na koncepty uvedenými v dříve [a podrobností filtrování s rozevírací seznam](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kurzu. Pokud jste ještě nebyla úspěšná prostřednictvím starší kurzu, postupujte tak než budete pokračovat na tento jeden.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Krok 1: Přidání rozevírací seznam kategorií a produkty GridView

Před označována týkající se přidání souhrnné informace do prvku GridView zápatí, nejprve jednoduše Vytvořme sestavy a podrobností. Po dokončení jsme tento první krok podíváme postup zahrnutí souhrnná data.

Začněte otevřením `SummaryDataInFooter.aspx` stránku `CustomFormatting` složky. Přidat ovládací prvek rozevírací seznam a nastavit jeho `ID` k `Categories`. V dalším kroku klikněte na odkaz zvolit zdroj dat z inteligentních značek rozevírací seznam a zapojit přidat nové ObjectDataSource s názvem `CategoriesDataSource` , který spustí `CategoriesBLL` třídy `GetCategories()` metoda.


[![Přidat nové ObjectDataSource s názvem CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Obrázek 2**: Přidání nové ObjectDataSource s názvem `CategoriesDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![Mít ObjectDataSource vyvolání metody GetCategories() CategoriesBLL – třída](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Obrázek 3**: mít ObjectDataSource vyvolání `CategoriesBLL` třídy `GetCategories()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


Po dokončení konfigurace ObjectDataSource, Průvodce vrátí nám konfigurace zdroje dat rozevírací seznam průvodce, ze kterého je potřeba zadat hodnotu pole jaká data se má zobrazit a které z nich by měla odpovídat hodnotě rozevírací seznam pro `ListItem` s. Máte `CategoryName` pole zobrazit a použít `CategoryID` jako hodnotu.


[![Použít pole CategoryID a CategoryName jako Text a hodnoty pro položky ListItems, v uvedeném pořadí](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Obrázek 4**: použití `CategoryName` a `CategoryID` polí jako `Text` a `Value` pro `ListItem` s, v uvedeném pořadí ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


V tomto okamžiku máme rozevírací seznam (`Categories`), obsahuje seznam kategorií v systému. Teď je potřeba přidat GridView, který obsahuje seznam těchto produktů, které patří do vybrané kategorie. Před provedeme, ale pozorně zaškrtnutím políčka Povolit automatické odeslání v inteligentních značek rozevírací seznam. Jak je popsáno v *a podrobností filtrování s rozevírací seznam* kurzu, a to nastavením rozevírací seznam `AutoPostBack` vlastnost `True` stránce budou odeslány zpět pokaždé, když se změní hodnota rozevírací seznam. To způsobí, že rutina GridView aktualizovat, zobrazuje tyto produkty pro nově vybranou kategorii. Pokud `AutoPostBack` je nastavena na `False` (výchozí), změna kategorie nezpůsobí zpětné volání a proto nebude aktualizovat uvedené produkty.


[![Zaškrtněte políčko Povolit automatické odeslání v inteligentních značek rozevírací seznam](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Obrázek 5**: Zaškrtnutím políčka Povolit automatické odeslání rozevírací seznam inteligentní značky ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Přidání ovládacího prvku GridView na stránku, aby bylo možné zobrazit produkty pro vybrané kategorie. Nastavte GridView `ID` k `ProductsInCategory` a navázat jej nové ObjectDataSource s názvem `ProductsInCategoryDataSource`.


[![Přidat nové ObjectDataSource s názvem ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Obrázek 6**: Přidání nové ObjectDataSource s názvem `ProductsInCategoryDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Nakonfigurovat ObjectDataSource tak, aby vyvolá `ProductsBLL` třídy `GetProductsByCategoryID(categoryID)` metoda.


[![Mít ObjectDataSource vyvolání metody GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Obrázek 7**: mít ObjectDataSource vyvolání `GetProductsByCategoryID(categoryID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Vzhledem k tomu `GetProductsByCategoryID(categoryID)` metoda přebírá vstupní parametr, v posledním kroku průvodce lze zadat zdroj hodnotu parametru. Aby bylo možné zobrazit tyto produkty z vybrané kategorie, mít parametr načtený z `Categories` rozevírací seznam.


[![Získání categoryID hodnota parametru z rozevírací seznam vybrané kategorie](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Obrázek 8**: získání *`categoryID`* hodnota parametru z rozevírací seznam vybrané kategorie ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


Po dokončení průvodce bude GridView mít BoundField pro jednotlivé vlastnosti produktu. Pojďme vyčištění tyto BoundFields proto pouze `ProductName`, `UnitPrice`, `UnitsInStock`, a `UnitsOnOrder` BoundFields jsou zobrazeny. Libosti přidat všechna nastavení na úrovni pole pro zbývající BoundFields (například formátování `UnitPrice` jako měny). Po provedení těchto změn, deklarativní prvku GridView by měl vypadat takto:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

V tuto chvíli nemáme plně funkční a podrobností sestavu, která zobrazí název, jednotkové ceny, jednotky v stock a jednotky v pořadí těchto produktů, které patří do vybrané kategorie.


[![Získání categoryID hodnota parametru z rozevírací seznam vybrané kategorie](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Obrázek 9**: získání *`categoryID`* hodnota parametru z rozevírací seznam vybrané kategorie ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Krok 2: Zobrazení v prvku GridView zápatí

Rutina GridView prvek může zobrazit řádek záhlaví a zápatí stránky. Tyto řádky se zobrazí v závislosti na hodnoty `ShowHeader` a `ShowFooter` vlastnosti, respektive s `ShowHeader` jako výchozí bude použit `True` a `ShowFooter` k `False`. Chcete-li zahrnout zápatí GridView jednoduše nastavte její `ShowFooter` vlastnost `True`.


[![Nastavte GridView ShowFooter vlastnost na hodnotu True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Obrázek 10**: nastavte GridView `ShowFooter` vlastnost `True` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


Řádek zápatí má buňku pro každé pole definovaná v GridView; tyto buňky jsou však ve výchozím nastavení prázdné. Chcete-li zobrazit naše průběh v prohlížeči chvíli trvat. Pomocí `ShowFooter` vlastnost nastaví na `True`, GridView zahrnuje zápatí prázdný řádek.


[![Rutina GridView nyní obsahuje řádek zápatí](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Obrázek 11**: GridView teď obsahuje řádek zápatí ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


Řádek zápatí v obrázek 11 není zvýraznění, protože má bílé pozadí. Vytvořme `FooterStyle` třídy CSS v `Styles.css` , určuje tmavý červené pozadí a pak nakonfigurujte `GridView.skin` – soubor vzhledu v `DataWebControls` motiv přiřadit tato třída šablon stylů CSS rutina GridView `FooterStyle`na `CssClass` vlastnost. Pokud potřebujete zdokonalit v vzhledy a motivy práci, přečtěte si téma zpět [zobrazení dat pomocí ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) kurzu.

Začněte přidáním následující třídy CSS na `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

`FooterStyle` Třída CSS, která se podobá v styl, který `HeaderStyle` třídy, i když `HeaderStyle`na barvu pozadí je subtlety tmavšího a zobrazí se jeho text tučným písmem. Kromě toho textu v zápatí je vpravo zarovnaný zatímco text záhlaví je umístěn na střed.

Dále přidružit každý GridView zápatí této třídy CSS, otevřete `GridView.skin` v soubor `DataWebControls` motiv a sadu `FooterStyle`na `CssClass` vlastnost. Po přidání souboru značek by měl vypadat podobně jako:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Jako snímek obrazovky níže znázorňuje, tato změna umožňuje zápatí zřetelně informace.


[![Řádek zápatí prvku GridView má teď barvu pozadí načervenalé](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Obrázek 12**: The GridView zápatí řádek má nyní načervenalé barvu pozadí ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Krok 3: Computing souhrnná Data

Pomocí prvku GridView zápatí zobrazí je další výzvy, kterým čelí nám postup výpočetní souhrnná data. Existují dva způsoby, jak výpočetní tyto agregační informace:

1. Pomocí příkazu jazyka SQL jsme může vydávat další dotaz do databáze k výpočtu souhrnná data pro určité kategorie. Zahrnuje několik agregační funkce spolu s SQL `GROUP BY` klauzule určete data, přes který mají být data shrnuty. Následující dotaz SQL by navrácení potřebné informace:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Samozřejmě byste neměli chtít vydat tento dotaz přímo z `SummaryDataInFooter.aspx` stránky, ale spíše vytvořením metoda v `ProductsTableAdapter` a `ProductsBLL`.
2. Výpočetní tyto informace, jako je je přidáván do GridView popsané v [vlastní formátování dat na základě při](custom-formatting-based-upon-data-cs.md) kurzu, GridView `RowDataBound` obslužné rutiny události aktivuje jednou pro každý řádek, který se přidává do GridView po jeho byla Vycentrovat. Vytvořením obslužné rutiny události pro tuto událost, abychom spuštěný celkový počet hodnot, které chcete agregovat. Za poslední řádek dat byla svázána se rutina GridView máme součty a informace potřebné k výpočtu průměru.

Druhý přístup I obvykle využívají, jako cesty se uloží do databáze a úsilí potřebný k implementaci funkci shrnutí v Data Access Layer a vrstvu obchodní logiky, ale buď přístup by stačit. V tomto kurzu budeme používat druhou možnost a sledovat celkové pomocí `RowDataBound` obslužné rutiny události.

Vytvoření `RowDataBound` obslužné rutiny události pro GridView výběrem GridView v návrháři, kliknutím na ikonu bolt v okně Vlastnosti a dvojitým kliknutím `RowDataBound` událostí. Alternativně můžete vybrat GridView a jeho RowDataBound události z rozevíracího seznamu v horní části souboru třída kódu ASP.NET. Tím se vytvoří novou obslužnou rutinu události s názvem `ProductsInCategory_RowDataBound` v `SummaryDataInFooter.aspx` třídy kódu stránky.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Aby byla zachována spuštěný celkový potřebujeme k definování proměnné mimo obor obslužné rutiny události. Vytvořte následující čtyři proměnné na úrovni stránky:

- `_totalUnitPrice`, typu `Decimal`
- `_totalNonNullUnitPriceCount`, typu `Integer`
- `_totalUnitsInStock`, typu `Integer`
- `_totalUnitsOnOrder`, typu `Integer`

Dále napište kód se zvýší těchto tří proměnných pro každý řádek dat zjistil v `RowDataBound` obslužné rutiny události.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

`RowDataBound` Obslužné rutiny události spustí tím zajistí, že jsme pracujete s DataRow. Jakmile tato je vytvořena, `Northwind.ProductsRow` instance, který byl právě spojen `GridViewRow` objekt v `e.Row` je uložené v proměnné `product`. Další, spuštěné celkový proměnné se zvýší o aktuální produkt odpovídající hodnoty (za předpokladu, že nemáte obsahují databázi `NULL` hodnotu). Jsme udržování přehledu o na kterém běží `UnitPrice` celkem a počet jinou hodnotu než`NULL` `UnitPrice` záznamy, protože je průměrná cena podíl těchto dvou čísel.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Krok 4: Zobrazení souhrnu dat v zápatí

S souhrnná data sečteny posledním krokem je pro zobrazení v prvku GridView zápatí řádku. Tento úkol, lze provést programově prostřednictvím `RowDataBound` obslužné rutiny události. Odvolat, který `RowDataBound` aktivuje obslužné rutiny události pro *každých* řádek, který je vázán k GridView, včetně řádku zápatí. Proto jsme můžete posílení naše obslužné rutiny události pro zobrazení dat v řádku zápatí pomocí následujícího kódu:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Vzhledem k tomu, že řádku zápatí se přidá k GridView po všechny řádky dat byly přidány, jsme jistotu na době jsme připraveni zobrazit souhrnná data v zápatí stránky, které bude dokončili celkové výpočty. Poslední krok, pak je nastavit tyto hodnoty v buňkách zápatí.

Chcete-li zobrazit text v buňce konkrétní zápatí, použijte `e.Row.Cells(index).Text = value`, kde `Cells` indexování spustí 0. Následující kód vypočítá průměrnou cenu (celková cena rozdělené podle počtu produktů) a zobrazí se společně s celkový počet jednotek v stock a jednotky v pořadí, v buňkách odpovídající zápatí GridView.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Obrázek 13 zobrazuje sestavu po přidání tohoto kódu. Poznámka: Jak `ToString("c")` způsobí, že průměrná cena souhrnné informace pro formátovat jako měny.


[![Řádek zápatí prvku GridView má teď barvu pozadí načervenalé](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Obrázek 13**: The GridView zápatí řádek má nyní načervenalé barvu pozadí ([Kliknutím zobrazit obrázek v plné velikosti](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Souhrn

Zobrazení souhrnných dat je běžné požadavek sestavy a prvek GridView usnadňuje zahrnout tyto informace v jeho řádek zápatí. Řádek zápatí se zobrazí při prvku GridView `ShowFooter` je nastavena na `True` a může mít textu v buňkách nastavují programově pomocí `RowDataBound` obslužné rutiny události. Computing souhrnná data můžete buď provést znovu dotazováním databázi nebo pomocí kódu v třídě kódu stránky ASP.NET prostřednictvím kódu programu vypočítat souhrnná data.

V tomto kurzu se ukončí součást vlastní formátování s ovládacími prvky GridView DetailsView a FormView. Naše další kurz zahájí naše zkoumání vkládání, aktualizaci a odstraňování dat pomocí tyto stejné ovládací prvky.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](using-the-formview-s-templates-vb.md)
