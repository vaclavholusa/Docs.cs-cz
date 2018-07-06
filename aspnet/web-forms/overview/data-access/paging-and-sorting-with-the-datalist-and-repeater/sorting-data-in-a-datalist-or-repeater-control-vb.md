---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Řazení dat DataList nebo Repeater (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu prozkoumáme způsob řazení podpory v ovládacích prvcích DataList a Repeater, jakož i jak sestavit DataList nebo Repeater, jejichž data můžete...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fcbc1f83a00621ce0031cdcb775537992e3cb843
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828880"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Řazení dat DataList nebo Repeater (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) nebo [stahovat PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> V tomto kurzu prozkoumáme způsob řazení podpory v ovládacích prvcích DataList a Repeater, jakož i jak sestavit DataList nebo Repeater, jehož data můžete stránkování a řazení.


## <a name="introduction"></a>Úvod

V [předchozí kurz o službě](paging-report-data-in-a-datalist-or-repeater-control-vb.md) jsme se zaměřili na tom, jak přidat podporu stránkování a v prvku DataList. Jsme vytvořili novou metodu v `ProductsBLL` třídy (`GetProductsAsPagedDataSource`), který se vrátil `PagedDataSource` objektu. Při vázání na ovládacích prvcích DataList nebo Repeater, DataList nebo Repeater zobrazí jenom požadovanou stránku. Tento postup je podobný co se používá interně ovládacími prvky GridView DetailsView a FormView k zajištění jejich integrované výchozí funkce stránkování.

Kromě nabízí podporu stránkování prvku GridView. obsahuje i mimo pole řazení podpory. DataList ani Repeater poskytuje integrovanou funkci řazení; řazení funkce však mohou být přidány pomocí bitového kódu. V tomto kurzu prozkoumáme způsob řazení podpory v ovládacích prvcích DataList a Repeater, jakož i jak sestavit DataList nebo Repeater, jehož data můžete stránkování a řazení.

## <a name="a-review-of-sorting"></a>Přehled řazení

Jak jsme viděli v [stránkování a řazení dat sestavy](../paging-and-sorting/paging-and-sorting-report-data-vb.md) kurz, obsahuje prvek GridView mimo pole řazení podporu. Každé pole ovládacího prvku GridView může mít přidruženou `SortExpression`, což znamená datové pole, podle kterého chcete data seřadit. Při GridView s `AllowSorting` je nastavena na `true`, každé pole ovládacího prvku GridView, které má `SortExpression` záhlaví se vykresluje jako odkazem (LinkButton) má hodnotu vlastnosti. Když uživatel klikne na konkrétní hlavičku s pole ovládacího prvku GridView, dojde k postbacku a data seřadí podle kliknutí na pole s `SortExpression`.

Ovládací prvek GridView `SortExpression` , vlastnost, která ukládá `SortExpression` pole ovládacího prvku GridView data seřadí podle. Kromě toho `SortDirection` vlastnost označuje, zda se v datech který se má seřadit v vzestupném nebo sestupném pořadí (Pokud uživatel klikne přepínat konkrétní odkaz záhlaví pole s GridView dvakrát za sebou, pořadí řazení).

Když prvku GridView je vázán na jeho ovládací prvek zdroje dat, se předá jeho `SortExpression` a `SortDirection` vlastnosti, které chcete data správy zdrojového kódu. Ovládací prvek zdroje dat načte data a pak ho seřadí podle zadané `SortExpression` a `SortDirection` vlastnosti. Po seřazení dat, ovládací prvek zdroje dat vrátí ji do prvku GridView.

Tuto funkci s ovládacími prvky DataList nebo Repeater replikovat, budeme musí:

- Vytvoření rozhraní pro řazení
- Mějte na paměti datové pole pro řazení podle a jestli se mají seřadit v vzestupném nebo sestupném pořadí
- Dáte pokyn, aby ObjectDataSource řazení dat podle konkrétní datové pole

Jsme budete řešit tyto tři úkoly v krocích 3 a 4. Pod prozkoumáme jak zahrnout i stránkování a řazení podpory v prvku DataList nebo Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Krok 2: Zobrazení produkty v Repeateru

Předtím, než jsme se starat o implementaci některé funkce související s řazení, umožní s začněte tím, že výpis produktů v ovládacím prvku opakovače. Začněte otevřením `Sorting.aspx` stránku `PagingSortingDataListRepeater` složky. Přidání ovládacího prvku Repeater na webovou stránku, nastavení jeho `ID` vlastnost `SortableProducts`. Opakovače s inteligentním, vytvořte nový prvek ObjectDataSource s názvem `ProductsDataSource` a jeho konfigurace pro načtení dat z `ProductsBLL` třída s `GetProducts()` metody. Vyberte možnost (žádné) rozevírací seznamy na kartách INSERT, UPDATE a DELETE.


[![Vytvoření prvku ObjectDataSource a nakonfigurujte ho na použití GetProductsAsPagedDataSource() – metoda](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Obrázek 1**: vytvoření ObjectDataSource a konfigurace pro použití `GetProductsAsPagedDataSource()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit karty na (žádný)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Obrázek 2**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit karty na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


Na rozdíl od s DataList, Visual Studio neprovádí automatické vytváření `ItemTemplate` pro ovládací prvek Repeater po vazby ke zdroji dat. Kromě toho jsme musí přidat `ItemTemplate` deklarativně, jako je inteligentní značky ovládacího prvku s Repeater chybí možnost Upravit šablony v prvku DataList s. Umožňují s použít stejné `ItemTemplate` v předchozím kurzu, který zobrazí název produktu s, dodavatele a kategorie.

Po přidání `ItemTemplate`, Repeater a prvku ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

Obrázek 3 ukazuje tuto stránku při prohlížení prostřednictvím prohlížeče.


[![Zobrazí se každý produkt s názvem, dodavatele a kategorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Obrázek 3**: se zobrazí každý produkt s názvem, dodavatele a kategorie ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Krok 3: Instruující ObjectDataSource k řazení dat.

Seřadit data zobrazená v Opakovači, musíme informovat ObjectDataSource řadicí výraz, podle kterého mají být řazeny data. Před ObjectDataSource načte data, nejprve aktivuje její [ `Selecting` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), což dává příležitost, abychom mohli zadat výraz řazení. `Selecting` Obslužná rutina události je předán objekt typu [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), který má vlastnost s názvem [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Třída slouží k předávání žádostí souvisejících s daty z příjemce dat pro ovládací prvek zdroje dat a zahrnuje [ `SortExpression` vlastnost](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

K předávání informací řazení ze stránky ASP.NET do ObjectDataSource, vytvořit obslužnou rutinu události pro `Selecting` událostí a pomocí následujícího kódu:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression* hodnota by se měla přiřadit název datového pole seřadit podle data (například ProductName). Není žádná vlastnost související směr řazení, takže pokud chcete data seřadit v sestupném pořadí, připojte řetězec DESC k *sortExpression* hodnotu (například ProductName DESC).

Pokračujte a zkuste několik různých hodnot pevně zakódované pro *sortExpression* a výsledky testů v prohlížeči. Jak ukazuje obrázek 4, při použití ProductName DESC jako *sortExpression*, produkty jsou seřazeny podle názvu ve vzestupném abecedním pořadí.


[![Produkty jsou seřazeny podle názvu v abecedním pořadí reverzní](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Obrázek 4**: The produkty jsou seřazeny podle názvu v abecedním pořadí reverzní ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Krok 4: Vytvoření rozhraní pro řazení a zapamatování výraz řazení a směr

Zapnutí podpory v prvku GridView řazení převede text záhlaví každého pole řazení s odkazem (LinkButton), které při kliknutí seřadí data odpovídajícím způsobem. Řazení rozhraní dává smysl pro aplikaci prvku GridView, kde svá data se elegantně zobrazuje ve sloupci. U ovládacích prvků DataList a Repeater však jiné řazení rozhraní je potřeba. Běžné řazení rozhraní seznam dat (na rozdíl od mřížky dat), je rozevírací seznam, který obsahuje pole, podle kterých lze seřadit data. Umožní s implementovat toto rozhraní pro účely tohoto kurzu.

Přidat ovládací prvek DropDownList webového výše `SortableProducts` Repeater a nastavte jeho `ID` vlastnost `SortBy`. V okně Vlastnosti klikněte na symbol tří teček v `Items` vlastnost přineste si Editor kolekce ListItem. Přidat `ListItem` s řazení dat podle `ProductName`, `CategoryName`, a `SupplierName` pole. Také přidat `ListItem` k seřazení produktů podle jejich názvu ve vzestupném abecedním pořadí.

`ListItem` `Text` Vlastnosti můžete nastavit na libovolnou hodnotu (jako je jméno), ale `Value` vlastnosti musí být nastavena na název datového pole (jako je například ProductName). Chcete-li seřadit výsledky v sestupném pořadí, připojte řetězec DESC s názvem pole dat, jako je ProductName DESC.


![Přidat položky ListItem pro každé pole řazení dat](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Obrázek 5**: Přidejte `ListItem` pro každé pole řazení dat


Nakonec přidejte ovládací prvek tlačítko webového napravo od DropDownList. Nastavte jeho `ID` k `RefreshRepeater` a jeho `Text` vlastnosti k aktualizaci.

Po vytvoření `ListItem` s a přidáte tlačítko Aktualizovat, DropDownList a tlačítko deklarativní syntaxe s by měl vypadat nějak takto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

S kompletní řazení DropDownList musíme dále aktualizovat ObjectDataSource s `Selecting` obslužná rutina události tak, že používá vybraný `SortBy``ListItem` s `Value` vlastnost na rozdíl od pevně zakódované řadicí výraz.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

V tuto chvíli při první návštěvě stránky bude produkty zpočátku seřazené podle `ProductName` datové pole, protože s `SortBy` `ListItem` ve výchozím nastavení zaškrtnuto (viz obrázek 6). Vyberete jinou možnost, jako je například kategorie řazení a kliknutí na Aktualizovat vyvolávají zpětné odeslání a znovu řadit data podle názvu kategorie, jak je vidět na obrázku 7.


[![Produkty jsou zpočátku seřazené podle názvu](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Obrázek 6**: The produkty, které jsou zpočátku seřazené podle názvu ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![Produkty jsou teď seřazené podle kategorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Obrázek 7**: The produkty, které jsou teď seřazené podle kategorie ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> Kliknutím na tlačítko Aktualizovat způsobí, že data si automaticky znovu seřazený, protože stav zobrazení opakovače s je zakázané, a způsobuje Repeater znovu připojit ke zdroji dat při každém postbacku. Pokud jste již opakovače s stav zobrazení povolený, změna řazení rozevíracího seznamu vyhráli t mít jakékoli dopad na pořadí řazení. Chcete-li to napravit, vytvořit obslužnou rutinu události pro tlačítko Aktualizovat s `Click` událostí a obnovení vazby Repeater ke zdroji dat (voláním opakovače s `DataBind()` metoda).


## <a name="remembering-the-sort-expression-and-direction"></a>Zapamatování výraz řazení a směr

Při vytváření seřaditelné DataList nebo Repeater na stránce, kde bez řazení související zpětná volání může dojít, je, že výraz řazení a směr se uloží postbacků dnešní s. Představte si například, že jsme aktualizovali Opakovači v tomto kurzu zahrnout tlačítko Odstranit v jednotlivých produktech. Když uživatel klikne na tlačítko Odstranit jsme d spouštět nějaký kód k odstranění vybraného produktu a potom znovu připojit data, která mají opakovače. Pokud podrobnosti řazení se zachová pro zpětné volání, data zobrazená na obrazovce se vrátí k původní pořadí řazení.

Pro účely tohoto kurzu DropDownList implicitně uloží řazení výraz a směr svůj stav zobrazení pro nás. Kdybychom používali jiné rozhraní pro řazení, jednu s LinkButtons říct, která poskytují různé možnosti řazení d potřebujeme pečlivě postbacků mějte na paměti pořadí řazení. Toho je možné dosáhnout uložením řazení parametry ve stavu zobrazení s stránky, včetně parametr řazení v řetězec dotazu, nebo prostřednictvím některé další techniku trvalého stavu.

Další příklady v tomto kurzu si projděte zachování řazení podrobnosti ve stavu zobrazení stránky s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Krok 5: Přidání podpory řazení DataList, který používá výchozí stránkování

V [předchozím kurzu](paging-report-data-in-a-datalist-or-repeater-control-vb.md) jsme se zaměřili na tom, jak implementovat výchozí stránkování s a v prvku DataList. Umožní s rozšířit tuto předchozí příklad patří schopnost stránkovaná data seřadit. Začněte otevřením `SortingWithDefaultPaging.aspx` a `Paging.aspx` stránky v `PagingSortingDataListRepeater` složky. Z `Paging.aspx` stránky, klikněte na tlačítko zdroj zobrazíte deklarativním označení stránky s. Zkopírování vybraného textu (viz obrázek 8) a vložte ho do deklarativním označení `SortingWithDefaultPaging.aspx` mezi `<asp:Content>` značky.


[![Deklarativní v replikaci &lt;asp: Content&gt; značek z Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Obrázek 8**: deklarativní v replikaci `<asp:Content>` značek z `Paging.aspx` k `SortingWithDefaultPaging.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


Po zkopírování deklarativní, zkopírujte metod a vlastností v `Paging.aspx` stránky s použití modelu code-behind třídy k použití modelu code-behind třídu pro `SortingWithDefaultPaging.aspx`. V dalším kroku věnujte chvíli zobrazíte `SortingWithDefaultPaging.aspx` stránku v prohlížeči. By měl vykazovat stejné funkce a vzhled jako `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Vylepšení ProductsBLL obsahovat výchozí stránkování a řazení – metoda

V předchozím kurzu jsme vytvořili `GetProductsAsPagedDataSource(pageIndex, pageSize)` metoda ve `ProductsBLL` třídy, která vrátila `PagedDataSource` objektu. To `PagedDataSource` se naplní objekt *všechny* produktů (prostřednictvím BLL s `GetProducts()` metoda), ale při vázání na ovládacím prvku DataList jen takové záznamy, které odpovídají zadané *pageIndex* a *pageSize* se zobrazí vstupní parametry.

Dříve v tomto kurzu jsme přidali podporu třídění tak, že zadáte výraz řazení v prvku ObjectDataSource s `Selecting` obslužné rutiny události. Tento postup funguje, i když prvku ObjectDataSource se vrátí objekt, který se dají řadit, jako je třeba `ProductsDataTable` vrácených `GetProducts()` metody. Ale `PagedDataSource` vrácený `GetProductsAsPagedDataSource` metoda nepodporuje řazení jeho vnitřní datového zdroje. Místo toho potřebujeme seřadit výsledky vrácené `GetProducts()` metoda *před* jsme ji umístit do `PagedDataSource`.

K tomu vytvořit novou metodu v `ProductsBLL` třídy, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Řazení `ProductsDataTable` vrácené `GetProducts()` metoda, zadejte `Sort` vlastnost jeho výchozí `DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource` Metoda se liší jen mírně z `GetProductsAsPagedDataSource` metoda vytvořili v předchozím kurzu. Zejména `GetProductsSortedAsPagedDataSource` další vstupní parametr přijímá `sortExpression` a přiřadí tuto hodnotu `Sort` vlastnost `ProductDataTable` s `DefaultView`. Pár řádků kódu později, `PagedDataSource` je přiřazen objekt s zdroje dat `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Volání metody GetProductsSortedAsPagedDataSource a určující hodnotu pro parametr SortExpression vstup

S `GetProductsSortedAsPagedDataSource` metoda kompletní, dalším krokem je poskytnout hodnotu tohoto parametru. Prvek ObjectDataSource v `SortingWithDefaultPaging.aspx` je aktuálně nakonfigurován pro volání `GetProductsAsPagedDataSource` metoda a předá dva vstupní parametry prostřednictvím jeho dvě `QueryStringParameters`, které jsou určené v `SelectParameters` kolekce. Tyto dvě `QueryStringParameters` označuje, že zdroj `GetProductsAsPagedDataSource` metody s *pageIndex* a *pageSize* parametry pocházejí z pole řetězce dotazu `pageIndex` a `pageSize`.

Aktualizace prvku ObjectDataSource s `SelectMethod` vlastnost tak, že vyvolá nový `GetProductsSortedAsPagedDataSource` metody. Pak přidejte nový `QueryStringParameter` tak, aby *sortExpression* vstupní parametr je přístupná z pole řetězce dotazu `sortExpression`. Nastavte `QueryStringParameter` s `DefaultValue` k ProductName.

Po provedení těchto změn prvku ObjectDataSource s deklarativní by měl vypadat jako:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

V tomto okamžiku `SortingWithDefaultPaging.aspx` stránky se jeho výsledky seřadit abecedně podle názvu produktu (viz obrázek 9). Důvodem je, že ve výchozím nastavení, je hodnota ProductName předáno jako `GetProductsSortedAsPagedDataSource` metody s *sortExpression* parametru.


[![Ve výchozím nastavení výsledky jsou seřazené podle ProductName](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Obrázek 9**: ve výchozím nastavení, výsledky jsou seřazeny podle `ProductName` ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


Pokud chcete ručně přidat `sortExpression` pole řetězce dotazu, jako `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` výsledky budou seřazeny podle zadaného `sortExpression`. Nicméně to `sortExpression` parametr není součástí řetězec dotazu při přesunu na jinou stránku data. Kliknutím na další nebo poslední stránku ve skutečnosti tlačítka přebírá nám zpět do `Paging.aspx`! Kromě toho zde s aktuálně žádné řazení rozhraní. Jediným způsobem, uživatel může změnit pořadí řazení stránkovaného dat je přímá manipulace s řetězec dotazu.

## <a name="creating-the-sorting-interface"></a>Vytvoření rozhraní pro řazení

Musíme nejprve aktualizovat `RedirectUser` metodu pro odeslání uživateli `SortingWithDefaultPaging.aspx` (místo `Paging.aspx`) a chcete zahrnout `sortExpression` hodnotu v řetězec dotazu. Také jsme měli přidat jen pro čtení,-úrovni stránky s názvem `SortExpression` vlastnost. Tuto vlastnost, podobně jako `PageIndex` a `PageSize` vlastnosti vytvořené v předchozím kurzu, vrátí hodnotu `sortExpression` pole řetězce dotazu, pokud existuje a výchozí hodnotu (ProductName) jinak.

Aktuálně `RedirectUser` metoda přijímá pouze jeden vstupní parametr index stránky k zobrazení. Může však nastat situace, kdy chceme přesměruje uživatele na konkrétní stránce dat s využitím řadicí výraz, než jaké s zadané v řetězec dotazu. Za chvíli vytvoříme řazení rozhraní pro tuto stránku, která bude obsahovat řadu tlačítko webové ovládací prvky pro řazení data podle zadaného sloupce. Jedno z těchto tlačítek se při kliknutí na, chcete přesměrovat uživatele předanou ve výrazu hodnotu odpovídající řazení. Chcete-li poskytují tuto funkci, vytvořte dvě verze `RedirectUser` metody. První z nich by měla přijímat pouze index stránky k zobrazení, zatímco druhá přijímá výrazu indexu a řazení stránky.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

V prvním příkladu v tomto kurzu jsme vytvořili řazení rozhraní pomocí DropDownList. V tomto příkladu vám umožňují s použít tři ovládací prvky webového tlačítko umístěn nad ovládacím prvku DataList, jeden pro řazení podle `ProductName`, jeden pro `CategoryName`a jeden pro `SupplierName`. Přidejte tři ovládací prvky tlačítka webové nastavení jejich `ID` a `Text` vlastnosti odpovídajícím způsobem:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Dále vytvořte `Click` obslužnou rutinu události pro každý. Obslužné rutiny události by měly volat `RedirectUser` metoda vrací uživatele na první stránku pomocí odpovídající řadicí výraz.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Při první návštěvě stránky, data abecedně seřazené podle názvu produktu (vrátit zpět k obrázek 9). Klikněte na tlačítko Další přejděte k druhé stránce data a potom klikněte na řazení podle kategorie tlačítko. Vrátí se nám na první stránku dat, seřazené podle názvu kategorie (viz obrázek 10). Podobně klepnutím na řazení podle dodavatele tlačítko seřadí data od první stránky dat dodavatelem. Možnost řazení je uloží, protože data je stránkování prostřednictvím. Po seřazení podle kategorie a pak přechodu na stránku třináctou dat obrázku 11 můžete vidět na stránce.


[![Produkty jsou seřazeny podle kategorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Obrázek 10**: The produkty jsou seřazeny podle kategorie ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![Výraz řazení se uloží, při stránkování přes Data](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Obrázek 11**: The řadicí výraz se uloží, při stránkování prostřednictvím Data ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Krok 6: Vlastní stránkování prostřednictvím záznamy v Repeateru

V příkladu v prvku DataList prozkoumat v kroku 5 stránek prostřednictvím její data způsobem neefektivní výchozí stránkování. Při procházení dostatečně velké objemy dat, je nutné použít vlastní stránkování. Zpátky [efektivně stránkování prostřednictvím velkých objemů dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) a [řazení vlastní stránkování dat](../paging-and-sorting/sorting-custom-paged-data-vb.md) kurzy, jsme se zaměřili na rozdíly mezi výchozí a vlastní stránkování a vytvořené metody v BLL pro Použití vlastní stránkování a řazení vlastní stránkovaná data. Konkrétně se tyto dvě předchozích kurzů, jsme přidali následující tři metody, které `ProductsBLL` třídy:

- `GetProductsPaged(startRowIndex, maximumRows)` Vrátí záznamy, které začínají na určitou podskupinu *startRowIndex* a nejvýše *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Vrátí podmnožinu záznamů seřazených podle zadaného konkrétní *sortExpression* vstupního parametru.
- `TotalNumberOfProducts()` poskytuje celkový počet záznamů v `Products` databázové tabulky.

Tyto metody slouží k efektivnímu stránce a roztřídit data pomocí ovládacího prvku DataList nebo Repeater. Pro ilustraci to umožní s začněte vytvořením prvku opakovače s vlastní podporou stránkování; potom přidáme možnosti řazení.

Otevřít `SortingWithCustomPaging.aspx` stránku `PagingSortingDataListRepeater` složky a přidat Repeateru na stránku nastavení jeho `ID` vlastnost `Products`. Opakovače s inteligentním, vytvořte nový prvek ObjectDataSource s názvem `ProductsDataSource`. Vyberte svoje data z konfigurace `ProductsBLL` třída s `GetProductsPaged` metoda.


[![Konfigurace ObjectDataSource metody GetProductsPaged ProductsBLL třída s](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Obrázek 12**: Konfigurace ObjectDataSource k použití `ProductsBLL` třída s `GetProductsPaged` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


Nastavte rozevírací seznamy v UPDATE, INSERT a odstraňovat karty na (žádný) a potom klikněte na tlačítko Další. Průvodce konfigurace zdroje dat vyzve k zadání zdrojů `GetProductsPaged` metody s *startRowIndex* a *maximumRows* vstupní parametry. Ve skutečnosti tyto vstupní parametry budou ignorovány. Místo toho *startRowIndex* a *maximumRows* hodnoty budou předávána ve prostřednictvím `Arguments` vlastnost v prvku ObjectDataSource s `Selecting` obslužná rutina události, stejně jako jak jsme zadali *sortExpression* v první kurz s ukázkou. Proto nechte zdroji parametru rozevírací seznamy v Průvodci nastavte na None.


[![Ponechte sady zdrojů na hodnotu None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Obrázek 13**: ponechte zdroje parametr nastaven na hodnotu None ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> Proveďte *není* nastavit ObjectDataSource s `EnablePaging` vlastnost `true`. To způsobí, že ObjectDataSource automaticky zahrnulo vlastní *startRowIndex* a *maximumRows* parametrů `SelectMethod` s existující seznam parametrů. `EnablePaging` Vlastnost je užitečná, když vlastní vazby dat k ovládacímu prvku GridView, DetailsView nebo FormView stránkováním vzhledem k tomu, že tyto ovládací prvky očekávat určité chování v prvku ObjectDataSource této s k dispozici pouze v případě `EnablePaging` vlastnost `true`. Vzhledem k tomu, že budeme muset ručně přidat podporu stránkování v prvku DataList a Repeater, ponechejte tuto vlastnost nastavenu `false` (výchozí), jak jsme budete zanést do potřebné funkce přímo v rámci naši stránku ASP.NET.


Nakonec definujte opakovače s `ItemTemplate` tak, aby produkt s názvem, kategorie a dodavateli. Po provedení těchto změn Repeater a ObjectDataSource deklarativní syntaxe s by měl vypadat nějak takto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Za chvíli najdete na stránce prostřednictvím prohlížeče a Všimněte si, že nejsou vráceny žádné záznamy. Důvodem je, že jsme ve ještě k určení *startRowIndex* a *maximumRows* hodnoty parametrů; proto hodnoty 0 jsou předávány v obou. Chcete-li zadat tyto hodnoty, vytvořit obslužnou rutinu události pro prvek ObjectDataSource s `Selecting` událostí a nastavte tyto parametry hodnoty prostřednictvím kódu programu k pevně definovaných hodnot 0 až 5 v uvedeném pořadí:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Díky této změně stránky, při zobrazení v prohlížeči zobrazí prvních pěti produkty.


[![Prvních pět záznamů se zobrazí.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Obrázek 14**: jsou zobrazeny prvních pět záznamů ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> Produkty uvedenými v obrázek 14 dojít, který se má seřadit podle názvu produktu, protože `GetProductsPaged` uloženou proceduru, která provádí efektivní vlastní stránkování dotaz řadí výsledky podle `ProductName`.


Umožní uživateli procházení stránek, musíme udržovat přehled o start index řádku a maximální počet řádků a postbacků pamatovat si tyto hodnoty. V příkladu stránkování výchozí jsme použili pole řetězce dotazu k uchování těchto hodnot. pro tuto ukázku umožní zachovat tyto informace ve stavu zobrazení stránky s s. Vytvořte následující dvě vlastnosti:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

V dalším kroku aktualizovat kód v obslužné rutině události Selecting tak, aby používala `StartRowIndex` a `MaximumRows` vlastnosti namísto pevně definovaných hodnot 0 až 5:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

V tomto okamžiku naši stránku stále zobrazuje jenom prvních pět záznamů. Nicméně s těmito vlastnostmi na místě, můžeme znovu připraven na vytvoření našeho stránkovací rozhraní.

## <a name="adding-the-paging-interface"></a>Přidání rozhraní stránkování

Umožněte s pomocí stejné první, Previous, další poslední stránkování rozhraní používá v příkladu stránkování výchozí včetně popisek webové zobrazení ovládacího prvku, který zobrazí stránku co data a celkový počet stránek existovat. Přidejte čtyři ovládací prvky webového tlačítko a popisek pod opakovače.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Dále vytvořte `Click` obslužné rutiny událostí pro čtyři tlačítka. Jedno z těchto tlačítek se při kliknutí na, musíme aktualizovat `StartRowIndex` a obnovení vazby dat k opakovače. Kód pro první, předchozí a další tlačítka je dostatečně jednoduchá, ale pro poslední tlačítko jak můžeme zjistit počáteční index řádků na poslední stránku dat? Pro výpočet tento index stejně jako schopnost určit, že zda by měla být povolená tlačítka Další a poslední potřebujeme vědět, kolik záznamů se stránkování prostřednictvím. Můžeme to určit pomocí volání `ProductsBLL` třída s `TotalNumberOfProducts()` metoda. Umožňují s vytvořit vlastnosti jen pro čtení, úrovni stránky s názvem `TotalRowCount` , který vrátí výsledky `TotalNumberOfProducts()` metody:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

S touto vlastností můžeme nyní určit poslední index stránky s počátečního řádku. Konkrétně to s výsledkem celé číslo z `TotalRowCount` minus 1 hodnotou `MaximumRows`, násobenou `MaximumRows`. Můžeme se teď dá zapisovat `Click` obslužné rutiny událostí pro čtyři tlačítka stránkování rozhraní:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Nakonec musíme zakázat první a předchozí tlačítka stránkování rozhraní při procházení první stránky dat a tlačítka Další a poslední při prohlížení na poslední stránce. Chcete-li to provést, přidejte následující kód do prvku ObjectDataSource s `Selecting` obslužné rutiny události:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Po přidání těchto `Click` obslužné rutiny událostí a kód pro povolení nebo zakázání stránkování prvky rozhraní založené na index aktuálního řádku start, otestovat stránku v prohlížeči. Jak znázorňuje obrázek 15, při první návštěvě stránky první a předchozí tlačítka se jsou zakázané. Kliknutím na další ukazuje na druhé stránce data, při kliknutí na poslední se zobrazí na poslední stránce (viz obrázky 16 a 17). Při prohlížení na poslední stránku dat na další a poslední tlačítka jsou zakázané.


[![Předchozí a poslední tlačítka jsou zakázány při procházení první stránky produktů](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Obrázek 15**: Previous a poslední tlačítka jsou zakázány při procházení první stránky produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![Druhá stránka produkty jsou Dispalyed](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Obrázek 16**: druhou stránku produktů jsou Dispalyed ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![Kliknutím na poslední zobrazí poslední stránky dat](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Obrázek 17**: Kliknutím na poslední zobrazí Data poslední stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Krok 7: Včetně řazení podporu s vlastním stránkováním Repeater

Teď, když byl implementován vlastní stránkování, znovu připravený k zahrnují řazení podporujeme. `ProductsBLL` Třída s `GetProductsPagedAndSorted` metoda má stejný *startRowIndex* a *maximumRows* vstupní parametry jako `GetProductsPaged`, ale umožňuje další  *sortExpression* vstupního parametru. Použít `GetProductsPagedAndSorted` metodu z `SortingWithCustomPaging.aspx`, musíme provést následující kroky:

1. Změnit ObjectDataSource s `SelectMethod` vlastnost z `GetProductsPaged` k `GetProductsPagedAndSorted`.
2. Přidat *sortExpression* `Parameter` objekt ObjectDataSource s `SelectParameters` kolekce.
3. Vytvoření privátní, úrovni stránky `SortExpression` vlastnost, která se uchovávají napříč postbacků prostřednictvím stav zobrazení stránky s jeho hodnotu.
4. Aktualizace prvku ObjectDataSource s `Selecting` obslužná rutina události přiřazení ObjectDataSource s *sortExpression* parametr hodnoty úrovni stránky `SortExpression` vlastnost.
5. Vytvoření rozhraní pro řazení.

Začněte tím, že aktualizace ObjectDataSource s `SelectMethod` vlastností a přidání *sortExpression* `Parameter`. Ujistěte se, že *sortExpression* `Parameter` s `Type` je nastavena na `String`. Po dokončení těchto prvních dvou úloh, ObjectDataSource s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

V dalším kroku budeme potřebovat úrovni stránky `SortExpression` vlastnost, jejíž hodnota je serializována stavu zobrazení. Pokud byla nastavena žádná hodnota výraz řazení, použijte jako výchozí ProductName:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Předtím, než vyvolá ObjectDataSource `GetProductsPagedAndSorted` metoda musíme nastavit *sortExpression* `Parameter` na hodnotu `SortExpression` vlastnost. V `Selecting` obslužná rutina události, přidejte následující řádek kódu:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Už jen zbývá k implementaci rozhraní řazení. Jako jsme to udělali v předchozím příkladu, umožní s řazení rozhraní, které jsou implementované pomocí tří tlačítko ovládacích prvků, které uživateli umožňují řazení výsledků podle názvu produktu, kategorie nebo dodavatele.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Vytvoření `Click` obslužné rutiny událostí pro tyto tři ovládací prvky tlačítek. V obslužné rutiny, resetovat `StartRowIndex` na hodnotu 0, nastavte `SortExpression` odpovídající hodnotu a obnovení vazby dat k opakovače:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Všechny existuje tento s je to! Když došlo k několika kroky k získání vlastní stránkování a řazení implementované, byly kroky velmi podobné těm, které jsou potřebné pro výchozí stránkování. Obrázek 18 ukazuje produkty při prohlížení na poslední stránku dat při řazení podle kategorie.


[![Zobrazí se Data poslední stránky, seřazeno podle kategorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Obrázek 18**: poslední stránku Data, seřazeno podle kategorie, zobrazí se ([kliknutím ji zobrazíte obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> V předchozích příkladech je při řazení podle dodavatele, který dodavatel byl použit jako výraz řazení. Ale pro vlastní implementaci stránkování, musíme použít CompanyName. Důvodem je, že uloženou proceduru, která je zodpovědná za implementaci vlastní stránkování `GetProductsPagedAndSorted` předává výraz řazení do `ROW_NUMBER()` – klíčové slovo, `ROW_NUMBER()` – klíčové slovo vyžaduje skutečný název sloupce, nikoli jako alias. Proto musíme použít `CompanyName` (název sloupce v `Suppliers` tabulky) namísto alias používaný v `SELECT` dotazu (`SupplierName`) pro výraz řazení.


## <a name="summary"></a>Souhrn

Ani ovládacích prvků DataList a Repeater nabízejí integrovanou podporu třídění, ale pomocí bitového kódu a vlastní řazení rozhraní, je možné přidat tyto funkce. Při implementaci řazení, ale ne stránkování, řadicí výraz se dá nastavit až `DataSourceSelectArguments` objekt předaný v prvku ObjectDataSource s `Select` metody. To `DataSourceSelectArguments` objektu s `SortExpression` vlastnost můžete přiřadit v prvku ObjectDataSource s `Selecting` obslužné rutiny události.

Přidání možnosti řazení DataList nebo Repeater, která již poskytuje podporu stránkování, je nejjednodušší způsob přizpůsobení vrstvy obchodní logiky obsahovat metodu, který přijímá řadicí výraz. Tyto informace je pak možné předat v prostřednictvím parametru v prvku ObjectDataSource s `SelectParameters`.

V tomto kurzu dokončíte naše zkoumání stránkování a řazení ovládacími prvky DataList a Repeater. V našem kurzu další a finální prozkoumá jak přidat tlačítko webové ovládací prvky s šablon ovládacích prvků DataList a Repeater negace některé vlastní, kterou inicioval uživatel funkce na základě za položku.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla David Suru. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
