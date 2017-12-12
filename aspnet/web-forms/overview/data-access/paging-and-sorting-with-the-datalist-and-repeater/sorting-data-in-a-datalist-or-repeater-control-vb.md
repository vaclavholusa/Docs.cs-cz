---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: "Řazení dat v DataList nebo prvku Repeater (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu podíváme, jak se zahrnuje řazení podpora v DataList a opakovače, jakož i konstruování DataList nebo opakovače, jejichž data můžete..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e3f505e525fd5e701bb40dc3e6467b880bf75447
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Řazení dat v DataList nebo prvku Repeater (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) nebo [stáhnout PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> V tomto kurzu podíváme, jak se zahrnuje řazení podpora v DataList a opakovače, jakož i konstruování DataList nebo opakovače, jejichž data můžete stránkovaného fondu a seřazeny.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](paging-report-data-in-a-datalist-or-repeater-control-vb.md) jsme se zaměřili na tom, jak přidat podporu stránkování k DataList. Jsme vytvořili novou metodu v `ProductsBLL` – třída (`GetProductsAsPagedDataSource`) vrácená `PagedDataSource` objektu. Když je vázán k DataList nebo opakovače, by DataList nebo opakovače zobrazí právě k požadované stránce data. Tento postup je podobný co se používá interně ovládacími prvky GridView DetailsView a FormView zajistit jejich integrované výchozí funkce stránkování.

Nabízí podporu stránkování, kromě GridView také zahrnuje předinstalované řazení podpory. DataList ani opakovače poskytuje vestavěnou funkci řazení; řazení funkce však mohou být přidány s malou část kódu. V tomto kurzu podíváme, jak se zahrnuje řazení podpora v DataList a opakovače, jakož i konstruování DataList nebo opakovače, jejichž data můžete stránkovaného fondu a seřazeny.

## <a name="a-review-of-sorting"></a>Zkontrolujte řazení

Jak jsme viděli v [stránkování a řazení dat sestavy](../paging-and-sorting/paging-and-sorting-report-data-vb.md) kurzu prvek GridView poskytuje předinstalované řazení podpory. Každé pole GridView může mít přiřazený `SortExpression`, což naznačuje datové pole, podle kterého chcete řadit data. Při GridView s `AllowSorting` je nastavena na `true`, každé pole GridView, který má `SortExpression` záhlaví se vykresluje jako LinkButton má hodnotu vlastnosti. Když uživatel klikne na konkrétní GridView pole s hlavičky, nastane zpětné volání a data jsou seřazená podle pole kliknutelnou s `SortExpression`.

Ovládací prvek GridView má `SortExpression` taky vlastnost, která ukládá `SortExpression` pole GridView data jsou seřazená podle. Kromě toho `SortDirection` vlastnost určuje, zda data který se má seřadit ve vzestupném nebo sestupném pořadí (Pokud uživatel klikne na odkaz záhlaví pole s konkrétní GridView dvakrát za sebou, pořadí řazení je přepínat stav).

Když GridView je vázán k jeho řízení zdrojů dat, se předá jeho `SortExpression` a `SortDirection` vlastnosti, které chcete data zdroj ovládacího prvku. Ovládací prvek zdroj dat načte data a potom je seřadí podle zadaných `SortExpression` a `SortDirection` vlastnosti. Po řazení dat, vrátí ovládací prvek zdroje dat ji do GridView.

Tato funkce s ovládacími prvky DataList nebo opakovače replikovat, jsme musí:

- Vytvoření řazení rozhraní
- Mějte na paměti, seřadit podle pole data a jestli se má seřadit ve vzestupném nebo sestupném pořadí
- Vyzvat ObjectDataSource k řazení dat. podle konkrétní datové pole

Jsme budete řešení tyto tři úlohy v krocích 3 a 4. Následující, podíváme, jak se zahrnuje stránkování i řazení podpory v DataList nebo opakovače.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Krok 2: Zobrazení v prvku Repeater produkty

Před jsme starat o implementaci všechny funkce související s řazení, umožní s začněte tím, že seznam produktů v ovládacím prvku opakovače. Začněte otevřením `Sorting.aspx` stránku `PagingSortingDataListRepeater` složky. Přidání ovládacího prvku opakovače na webovou stránku, nastavení jeho `ID` vlastnost `SortableProducts`. Opakovače s inteligentním, vytvořte nový ObjectDataSource s názvem `ProductsDataSource` a nakonfigurovat ji k načtení dat z `ProductsBLL` třídu s `GetProducts()` metoda. Vyberte možnost (žádné) možnost z rozevíracího seznamu na kartách INSERT, UPDATE a DELETE.


[![ObjectDataSource vytvořit a nakonfigurovat jej lze pomocí této metody GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Obrázek 1**: ObjectDataSource vytvořit a nakonfigurovat jej pro použití `GetProductsAsPagedDataSource()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![Nastavte rozevíracím seznamu v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Obrázek 2**: nastavte rozevíracím seznamu v aktualizaci UPDATE, INSERT a odstranit karty na (žádný) ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


Na rozdíl od s DataList, Visual Studio automaticky nevytvoří `ItemTemplate` po vazby ke zdroji dat ovládacího prvku opakovače. Kromě toho musí přidáme to `ItemTemplate` deklarativně jako inteligentní značky ovládacího prvku s opakovače chybí možnost Upravit šablony v DataList s nalezen. Umožňují s používat stejné `ItemTemplate` z předchozí kurz, který zobrazí název produktu s, dodavatele a kategorie.

Po přidání `ItemTemplate`, opakovače a ObjectDataSource s deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

Obrázek 3 ukazuje tuto stránku při zobrazení prostřednictvím prohlížeče.


[![Zobrazí se každý produkt s názvem, dodavatele a kategorii](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Obrázek 3**: každý produkt s názvem, dodavatele a kategorie se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Krok 3: Instruující ObjectDataSource k řazení dat.

Řazení dat, které zobrazuje v Opakovači, je potřeba informovat ObjectDataSource výrazu řazení, podle kterého má řazení proběhnout data. Předtím, než ObjectDataSource načítá data, nejprve aktivuje její [ `Selecting` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), což dává příležitost, abychom mohli zadejte výraz řazení. `Selecting` Obslužné rutiny události se předá objekt typu [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), který má vlastnost s názvem [ `Arguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Třída slouží k předávání žádostí souvisejících s daty z příjemce dat k řízení zdrojů dat a zahrnuje [ `SortExpression` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Chcete-li předat řazení informace ze stránky ASP.NET ObjectDataSource, vytvořit obslužnou rutinu události pro `Selecting` událostí a použijte následující kód:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression* hodnota by měla být přiřazená název datového pole seřadit podle data (například ProductName). Neexistuje vlastnost související směr řazení, takže pokud chcete data seřaďte v sestupném pořadí, doplňovací řetězec DESC k *sortExpression* hodnotu (například ProductName DESC).

Pokračujte a zkuste několik různých hodnot pevně pro *sortExpression* a testovat výsledky v prohlížeči. Jak ukazuje obrázek 4, při použití ProductName DESC jako *sortExpression*, produkty jsou seřazeny podle názvu jejich ve vzestupném abecedním pořadí.


[![Produkty jsou seřazeny podle názvu jejich Reverse abecedně](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Obrázek 4**: The produkty jsou seřazeny podle názvu jejich v abecedním pořadí Reverse ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Krok 4: Vytvoření rozhraní řazení a zapamatování výrazů řazení a směr

Zapnutí podpory v GridView řazení převede text záhlaví každý řazení pole s LinkButton, po kliknutí na seřadí data odpovídajícím způsobem. Řazení rozhraní má smysl pro GridView, kde jeho data přehledně rozložená ve sloupcích. Pro ovládací prvky DataList a opakovače ale jiné řazení rozhraní je potřeba. Společné rozhraní řazení seznam dat (na rozdíl od mřížky dat), je rozevíracího seznamu, který poskytuje pole, podle kterých se dají řadit data. Umožní s implementovat takového rozhraní pro účely tohoto kurzu.

Přidat ovládací prvek rozevírací seznam webového výše `SortableProducts` opakovače a nastavte její `ID` vlastnost `SortBy`. V okně vlastností klikněte na výpustky v `Items` vlastnost, aby si Editor kolekce ListItem. Přidat `ListItem` s k řazení dat. tím `ProductName`, `CategoryName`, a `SupplierName` pole. Také přidat `ListItem` seřadit produkty podle názvu jejich ve vzestupném abecedním pořadí.

`ListItem` `Text` Vlastnosti lze nastavit na jakoukoli hodnotu (například název), ale `Value` vlastnosti musí být nastavena na název datového pole (například ProductName). Chcete-li seřadit výsledky v sestupném pořadí, připojte řetězec DESC název pole dat, jako je ProductName DESC.


![Přidání položky ListItem pro každé pole řazení dat](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Obrázek 5**: přidání `ListItem` pro každé pole řazení dat


Nakonec přidejte ovládacího prvku tlačítko webové napravo rozevírací seznam. Nastavte její `ID` k `RefreshRepeater` a jeho `Text` vlastnosti aktualizace.

Po vytvoření `ListItem` s a přidání tlačítko Aktualizovat rozevírací seznam a tlačítko deklarativní syntaxi s by měl vypadat takto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Pomocí řazení rozevírací seznam dokončení, musíme další aktualizace ObjectDataSource s `Selecting` obslužné rutiny události tak, aby používala vybraný `SortBy``ListItem` s `Value` vlastnost oproti výraz pevně řazení.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

V tomto okamžiku při první návštěvě stránce bude produkty původně seřazené podle `ProductName` pole dat, protože s `SortBy` `ListItem` vybrané ve výchozím nastavení (viz obrázek 6). Vyberete různé řazení možnost například kategorie a kliknutím na tlačítko Aktualizovat způsobí zpětné volání a znovu seřadit data název kategorie, jak je vidět na obrázku 7.


[![Tyto produkty jsou původně seřazené podle názvu](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Obrázek 6**: produkty jsou původně seřazené podle názvu jejich ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![Tyto produkty jsou nyní seřazené podle kategorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Obrázek 7**: produkty jsou nyní seřazené podle kategorie ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> Kliknutím na tlačítko Aktualizovat způsobí, že data jako automaticky znovu seřazené protože stav opakovače s zobrazení je zakázané, a způsobuje opakovače se svázat svůj zdroj dat na každé zpětné volání. Pokud jste již opakovače s zobrazení stav povoleno, změna toto řazení rozevíracího seznamu won t mít žádné dopad na pořadí řazení. Chcete-li to opravit, vytvoření obslužné rutiny události pro tlačítko Aktualizovat s `Click` událostí a obnovení vazby opakovače svůj zdroj dat (voláním opakovače s `DataBind()` metoda).


## <a name="remembering-the-sort-expression-and-direction"></a>Zapamatování výrazů řazení a směr

Při vytváření řazení DataList nebo opakovače na stránce, kde bez řazení související postback může dojít, je s imperativní že výrazů řazení a směr zapamatován napříč postback. Představte si například, že v tomto kurzu zahrnout tlačítko Odstranit u každého produktu aktualizován opakovače. Když uživatel klikne na tlačítko Odstranit jsme d spouštět nějaký kód odstranit vybrané produktu a konkrétním data, která mají opakovače. Pokud nejsou podrobnosti řazení zachová pro zpětné volání, údaje zobrazené na obrazovce se vrátí do původního pořadí řazení.

V tomto kurzu rozevírací seznam implicitně ukládá řazení výraz a směr svůj stav zobrazení pro nás. Pokud jsme používali různé řazení rozhraní, jeden s Řekněme, LinkButtons, které poskytuje různé možnosti řazení d musíme postará pamatovat pořadí řazení mezi postback. To je možné dosáhnout uložením řazení parametry ve stavu zobrazení stránky s, včetně řazení parametr v řetězci dotazu nebo prostřednictvím některé jiné technika trvalost stavu.

Budoucí příklady v tomto kurzu prozkoumejte postup zachovat řazení podrobnosti ve stavu zobrazení stránky s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Krok 5: Přidání řazení podporu pro DataList, který používá výchozí stránkování

V [předchozí kurzu](paging-report-data-in-a-datalist-or-repeater-control-vb.md) jsme se zaměřili na implementaci stránkování výchozí s DataList. Umožní s tato předchozí příklad rozšířit pro zahrnují možnost řadit stránkové data. Začněte otevřením `SortingWithDefaultPaging.aspx` a `Paging.aspx` stránky v `PagingSortingDataListRepeater` složky. Z `Paging.aspx` klikněte na tlačítko zdroj pro zobrazení stránky s deklarativní. Kopírovat vybraný text (viz obrázek 8) a vložte jej do deklarativní z `SortingWithDefaultPaging.aspx` mezi `<asp:Content>` značky.


[![Deklarativní v replikaci &lt;asp: obsah&gt; značek z Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Obrázek 8**: deklarativní v replikaci `<asp:Content>` značky z `Paging.aspx` k `SortingWithDefaultPaging.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


Po zkopírování deklarativní, zkopírujte metody a vlastnosti ve `Paging.aspx` stránky třídu s kódem v pozadí k třídě kódu pro `SortingWithDefaultPaging.aspx`. V dalším kroku pozorně zobrazení `SortingWithDefaultPaging.aspx` stránku v prohlížeči. By měl mít květy stejné funkce a vzhled jako `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Rozšíření ProductsBLL zahrnout výchozí stránkování a řazení – metoda

V předchozích kurzu jsme vytvořili `GetProductsAsPagedDataSource(pageIndex, pageSize)` metoda v `ProductsBLL` třídy, který vrátil `PagedDataSource` objektu. To `PagedDataSource` objekt byl naplněný *všechny* produkty (prostřednictvím BLL s `GetProducts()` metoda), ale když vázaný DataList pouze záznamy odpovídající zadanému *index stránky* a *pageSize* byly zobrazeny vstupní parametry.

V tomto kurzu jsme přidali řazení podporu zadáním výrazů řazení z ObjectDataSource s `Selecting` obslužné rutiny události. Tento postup funguje i když ObjectDataSource se vrátí objekt, který lze seřadit, jako je třeba `ProductsDataTable` vrácené `GetProducts()` metoda. Ale `PagedDataSource` objekt vrácený `GetProductsAsPagedDataSource` metoda nepodporuje řazení jeho vnitřní datové zdroje. Místo toho je potřeba seřadit výsledky vrácené `GetProducts()` metoda *před* jsme ji umístit do `PagedDataSource`.

K tomu, vytvoření nové metody v `ProductsBLL` třídy `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Seřadit `ProductsDataTable` vrácený `GetProducts()` metoda, zadejte `Sort` vlastnost jeho výchozí `DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource` Metoda se pouze mírně liší od `GetProductsAsPagedDataSource` metoda vytvořili v předchozí kurzu. Konkrétně `GetProductsSortedAsPagedDataSource` přijímá další vstupní parametr `sortExpression` a přiřadí této hodnoty `Sort` vlastnost `ProductDataTable` s `DefaultView`. Po zadání několika řádků kódu později, `PagedDataSource` objekt s zdroj dat je přiřazen `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Volání metody GetProductsSortedAsPagedDataSource a určující hodnotu pro parametr SortExpression vstup

Pomocí `GetProductsSortedAsPagedDataSource` Metoda dokončení, dalším krokem je zadejte hodnotu tohoto parametru. ObjectDataSource v `SortingWithDefaultPaging.aspx` je nyní nakonfigurován, aby volání `GetProductsAsPagedDataSource` metoda a předává dva vstupní parametry prostřednictvím jeho dva `QueryStringParameters`, které jsou určené v `SelectParameters` kolekce. Tyto dva `QueryStringParameters` označuje, že zdroj `GetProductsAsPagedDataSource` metoda s *index stránky* a *pageSize* parametry pocházejí z pole řetězce dotazu `pageIndex` a `pageSize`.

Aktualizace ObjectDataSource s `SelectMethod` vlastnosti, které se vyvolá nové `GetProductsSortedAsPagedDataSource` metoda. Pak přidejte nový `QueryStringParameter` tak, aby *sortExpression* vstupní parametr přistupuje z pole řetězce dotazu `sortExpression`. Nastavte `QueryStringParameter` s `DefaultValue` k ProductName.

Po provedení těchto změn ObjectDataSource s deklarativní by měl vypadat podobně jako:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

V tomto okamžiku `SortingWithDefaultPaging.aspx` stránka bude abecedně seřaďte své výsledky podle názvu produktu (viz obrázek 9). Důvodem je, že ve výchozím nastavení, je předaná hodnota ProductName jako `GetProductsSortedAsPagedDataSource` metoda s *sortExpression* parametr.


[![Ve výchozím nastavení jsou výsledky seřazené podle ProductName](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Obrázek 9**: ve výchozím výsledky jsou seřazené podle `ProductName` ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


Pokud ručně přidáte `sortExpression` pole řetězce dotazu, jako `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` výsledky budou seřazeny podle zadaného `sortExpression`. Ale to `sortExpression` parametr není zahrnutý v řetězci dotazu při přesunu dat na jinou stránku. Kliknutím na další nebo poslední stránku ve skutečnosti tlačítka trvá nám zpět do `Paging.aspx`! Kromě toho že s aktuálně žádné řazení rozhraní. Jediným způsobem, uživatel může změnit pořadí řazení stránkové dat je přímo manipulace s řetězci dotazu.

## <a name="creating-the-sorting-interface"></a>Vytváření rozhraní řazení

Je potřeba nejdřív aktualizovat `RedirectUser` metodu pro odeslání uživatelům `SortingWithDefaultPaging.aspx` (místo `Paging.aspx`) a zahrnout `sortExpression` hodnota v řetězci dotazu. Také jsme měli přidat jen pro čtení, úrovni stránky s názvem `SortExpression` vlastnost. Tato vlastnost, podobně jako `PageIndex` a `PageSize` vlastnosti vytvořili v předchozí kurzu, vrátí hodnotu `sortExpression` pole řetězce dotazu, pokud existuje a výchozí hodnota (ProductName) jinak.

Aktuálně `RedirectUser` metoda přijímá pouze jeden vstupní parametr index stránky pro zobrazení. Může však nastat situace, když chcete přesměrovat uživatele na konkrétní stránku dat pomocí výraz řazení, než jaké s zadaný v řetězci dotazu. Za chvíli vytvoříme rozhraní řazení pro tuto stránku, která bude zahrnovat řadu ovládací prvky webového tlačítko pro řazení dat podle zadaného sloupce. Po kliknutí těchto tlačítek na, chceme přesměruje uživatele předávání v hodnota výrazu odpovídající řazení. Aby bylo možné tuto funkci, vytvářet dvě verze `RedirectUser` metoda. První z nich by měl přijmout právě index stránky, pokud chcete zobrazit, zatímco druhý přijímá stránky indexu a řazení výraz.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

V prvním příkladu v tomto kurzu jsme vytvořili řazení rozhraní pomocí rozevírací seznam. V tomto příkladu umožňují s použít tři ovládací prvky webového tlačítko umístěna nad DataList, jednu pro řazení podle `ProductName`, jeden pro `CategoryName`a jeden pro `SupplierName`. Přidejte tři tlačítko webové ovládací prvky, nastavení jejich `ID` a `Text` vlastnosti odpovídajícím způsobem:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Dále vytvořte `Click` obslužné rutiny události pro každý. Obslužné rutiny událostí by měly volat `RedirectUser` metoda, vrácení uživatele na první stránku pomocí výrazu odpovídající řazení.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Při první návštěvě stránky, data jsou seřazená podle abecedy podle názvu produktu (odkazuje zpět na obrázku 9). Klikněte na tlačítko Další přechodu na druhé stránce dat a klikněte na tlačítko řazení podle kategorie tlačítko. To nám vrátí na první stránku dat, seřazené podle názvu kategorie (viz obrázek 10). Podobně klepnutím na řazení podle dodavatele tlačítko seřadí data od první stránky dat dodavatelem. Volba řazení je zapamatován jako data je prostřednictvím stránkovaného fondu. Obrázek 11 zobrazuje stránku po řazení podle kategorie a pak přechodu na stránku třináctou data.


[![Produkty jsou seřazené podle kategorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Obrázek 10**: produkty jsou seřazené podle kategorie ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![Výraz řazení je zapamatovaných při stránkování prostřednictvím dat](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Obrázek 11**: výraz řazení je zapamatovaných při stránkování prostřednictvím dat ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Krok 6: Vlastní stránkování prostřednictvím záznamy v prvku Repeater

Příklad DataList zkontrolován v kroku 5 stránky prostřednictvím svá data pomocí techniky stránkování neefektivní výchozí. Při procházení dostatečně velké objemy dat, je nutné použít vlastní stránkování. Zpět v [efektivně stránkování prostřednictvím velké objemy dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) a [řazení dat vlastní stránkovaného](../paging-and-sorting/sorting-custom-paged-data-vb.md) kurzy, jsme se zaměřili na rozdíly mezi výchozími a vlastní stránkování a vytvořený metody v BLL pro Použití vlastní stránkování a řazení vlastní stránkové data. Konkrétně v těchto dvou předchozí kurzy, jsme přidali následující tři metody, které `ProductsBLL` třídy:

- `GetProductsPaged(startRowIndex, maximumRows)`Vrátí podmnožinu záznamy začínající na konkrétní *startRowIndex* a nejvýše *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`Vrátí podmnožinu konkrétní záznamů seřazené podle zadaného *sortExpression* vstupní parametr.
- `TotalNumberOfProducts()`poskytuje celkový počet záznamů v `Products` databázové tabulky.

Tyto metody slouží k efektivní stránky a řazení prostřednictvím dat pomocí ovládacího prvku DataList nebo opakovače. Pro znázornění je umožnit s začněte tím, že vytvoření ovládacího prvku opakovače s vlastní podporu stránkování; potom přidáme možnosti řazení.

Otevřete `SortingWithCustomPaging.aspx` stránku `PagingSortingDataListRepeater` složky a přidat prvku Repeater na stránku nastavení jeho `ID` vlastnost, která má `Products`. Inteligentní značky opakovače s, vytvořte nový ObjectDataSource s názvem `ProductsDataSource`. Nakonfigurovat, aby vyberte data z `ProductsBLL` třídu s `GetProductsPaged` metoda.


[![Konfigurace ObjectDataSource lze pomocí této metody GetProductsPaged ProductsBLL třídu s](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Obrázek 12**: Konfigurace ObjectDataSource pro použití `ProductsBLL` třídu s `GetProductsPaged` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstraňte karty na (žádný) a pak klikněte na tlačítko Další. Průvodce konfigurace zdroje dat vyzve k zadání zdroje `GetProductsPaged` metoda s *startRowIndex* a *maximumRows* vstupní parametry. Ve skutečnosti se ignorují tyto vstupní parametry. Místo toho *startRowIndex* a *maximumRows* hodnoty budou předány v prostřednictvím `Arguments` vlastnost ObjectDataSource s `Selecting` obslužné rutiny události, stejně jako jsme zadali *sortExpression* v této ukázce první kurz s. Proto nechejte zdrojový parametr rozevíracích seznamů v Průvodci nastavte na žádný.


[![Zadaná sada parametrů zdroje nechte na hodnotu None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Obrázek 13**: ponechte zdroje parametr nastaven na hodnotu None ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> Proveďte *není* nastavit ObjectDataSource s `EnablePaging` vlastnost `true`. To způsobí, že ObjectDataSource automaticky zahrnout vlastní *startRowIndex* a *maximumRows* parametry, které `SelectMethod` s existující seznam parametrů. `EnablePaging` Vlastnost je užitečná, když vlastní vazby stránkovaného data do ovládacího prvku GridView, DetailsView nebo FormView, protože tyto ovládací prvky očekávat určité chování z ObjectDataSource této s k dispozici pouze v případě `EnablePaging` vlastnost je `true`. Vzhledem k tomu, že máme ručně přidat podporu stránkování pro DataList a opakovače, ponechte tuto vlastnost nastavit na `false` (výchozí), protože jsme budete cukrárna potřebných funkcí přímo v naší stránce s ASP.NET.


Nakonec zadejte opakovače s `ItemTemplate` tak, aby produkt s název, kategorie a dodavatele. Po provedení těchto změn opakovače a ObjectDataSource deklarativní syntaxi s by měl vypadat takto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Za chvíli navštívit stránku prostřednictvím prohlížeče a Všimněte si, že jsou vráceny žádné záznamy. Důvodem je, že jsme jste zatím k určení *startRowIndex* a *maximumRows* hodnoty parametrů; proto hodnoty 0 jsou předávány v obou. Chcete-li tyto hodnoty zadat vytvořit obslužnou rutinu události pro ObjectDataSource s `Selecting` událostí a nastavte tyto parametry hodnoty prostřednictvím kódu programu pevně hodnoty 0 a 5, v uvedeném pořadí:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Díky této změně stránky, při zobrazení prostřednictvím prohlížeče, zobrazuje prvních pět produkty.


[![Zobrazení první pět záznamů](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Obrázek 14**: zobrazení první pět záznamů ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> Produkty uvedené v obrázku 14 dojít, který se má seřadit podle názvu produktu, protože `GetProductsPaged` uložené procedury, která provádí efektivní vlastního dotazu stránkování řadí výsledky podle `ProductName`.


Aby bylo možné povolit uživateli krok prostřednictvím stránky, je potřeba sledovat počáteční index řádku a maximální počet řádků a mějte na paměti tyto hodnoty mezi postback. V příkladu stránkování výchozí jsme použili pole řetězce dotazu se zachovat tyto hodnoty; v této ukázce umožní zachovat tyto informace ve stavu zobrazení stránky s s. Vytvořte následující dvě vlastnosti:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Potom aktualizujte kód obslužné rutiny události zvolíte tak, aby používala `StartRowIndex` a `MaximumRows` vlastnosti místo pevně hodnoty 0 a 5:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

V tomto okamžiku Naše stránka stále zobrazuje pouze prvních pět záznamů. Nicméně s těmito vlastnostmi na místě, jsme re připravené k vytvoření naše rozhraní stránkování.

## <a name="adding-the-paging-interface"></a>Přidání rozhraní stránkování

V příkladu stránkování výchozí štítek webové zobrazení ovládací prvek, který zobrazí jaké stránky dat včetně a celkový počet stránek existují použít umožňují s pomocí stejné první, předchozí a další, poslední stránkování rozhraní. Přidejte čtyři ovládací prvky webového tlačítko a popisek pod opakovače.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Dále vytvořte `Click` obslužných rutin událostí pro čtyři tlačítka. Po kliknutí z těchto tlačítek na, musíme aktualizovat `StartRowIndex` a rebind data, která mají opakovače. Kód pro první, předchozí a další tlačítka je dostatečně jednoduchá, ale pro poslední tlačítko jak jsme je zjistit počáteční index řádků na poslední stránku dat? K výpočtu tento index, jakož i schopnost určit, že zda by měla být povolená tlačítka Další a poslední potřebujeme vědět, celkový počet záznamů jsou stránkování prostřednictvím. Jsme můžete určit voláním `ProductsBLL` třídu s `TotalNumberOfProducts()` metoda. Umožňují s vytvoření vlastnosti jen pro čtení, úrovně stránky s názvem `TotalRowCount` , vrátí výsledky `TotalNumberOfProducts()` metoda:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

S touto vlastností jsme můžete teď určit poslední stránku s počáteční index řádku. Konkrétně se s výsledkem celé číslo z `TotalRowCount` minus 1 dělený `MaximumRows`, násobenou `MaximumRows`. Jsme teď může zapisovat `Click` obslužných rutin událostí pro čtyři tlačítka rozhraní stránkování:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Nakonec musíme zakázat tlačítka první a předchozí v rozhraní stránkování, při zobrazení na první stránku dat a tlačítka Další a poslední při zobrazení poslední stránky. K tomu, přidejte následující kód do ObjectDataSource s `Selecting` obslužné rutiny události:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Po přidání těchto `Click` obslužné rutiny událostí a kód k povolení nebo zakázání prvky rozhraní stránkování podle aktuální index řádku počáteční testovací stránce v prohlížeči. Jako znázorňuje obrázek 15, pokud nejprve na stránce první, a bude předchozí tlačítka jsou zakázány. Kliknutím na tlačítko Další zobrazuje stránku druhý dat, když kliknete na poslední zobrazí poslední stránce (viz následující obrázky 16 a 17). Při zobrazení na poslední stránku dat jsou zakázány na další a poslední tlačítka.


[![Poslední tlačítka předchozí a nejsou při zobrazení první stránka produkty](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Obrázek 15**: poslední tlačítka a Previous nejsou při zobrazení první stránka produkty ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![Druhá stránka produkty jsou Dispalyed](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Obrázek 16**: druhé stránce produkty jsou Dispalyed ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![Kliknutím na poslední zobrazí na poslední stránku dat](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Obrázek 17**: Kliknutím na poslední zobrazí poslední stránku Data ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Krok 7: Včetně řazení podporu pro vlastní opakovače stránkovaného fondu

Teď, když byl implementován vlastní stránkování, znovu připravení zahrnují řazení podporujeme. `ProductsBLL` Třídu s `GetProductsPagedAndSorted` metoda má stejné *startRowIndex* a *maximumRows* vstupní parametry jako `GetProductsPaged`, ale umožňuje další  *sortExpression* vstupní parametr. Použít `GetProductsPagedAndSorted` metoda z `SortingWithCustomPaging.aspx`, je potřeba provést následující kroky:

1. Změnit ObjectDataSource s `SelectMethod` vlastnost z `GetProductsPaged` k `GetProductsPagedAndSorted`.
2. Přidat *sortExpression* `Parameter` objekt, který chcete ObjectDataSource s `SelectParameters` kolekce.
3. Vytvoření privátního, úrovně stránky `SortExpression` vlastnost, která je uchována jeho hodnota napříč postback prostřednictvím stav zobrazení stránky s.
4. Aktualizace ObjectDataSource s `Selecting` obslužné rutiny události přiřadit ObjectDataSource s *sortExpression* parametr hodnota úrovně stránky `SortExpression` vlastnost.
5. Vytvořte rozhraní řazení.

Začněte tím, že aktualizace ObjectDataSource s `SelectMethod` vlastnost a přidávání *sortExpression* `Parameter`. Ujistěte se, že *sortExpression* `Parameter` s `Type` je nastavena na `String`. Po dokončení těchto úloh první dva, ObjectDataSource s deklarativní by měl vypadat následovně:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Dále je třeba úrovně stránky `SortExpression` vlastnost, jehož hodnota je serializováno stavu zobrazení. Pokud byla nastavena žádná hodnota výraz řazení, použijte jako výchozí ProductName:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Předtím, než ObjectDataSource vyvolá `GetProductsPagedAndSorted` metoda je potřeba nastavit *sortExpression* `Parameter` na hodnotu `SortExpression` vlastnost. V `Selecting` obslužné rutiny události, přidejte následující řádek kódu:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Zbývá k implementaci rozhraní řazení. Jako jsme to udělali v posledním příkladu, umožní s mít rozhraní řazení implementovaná pomocí tří ovládací prvky webového tlačítko, které umožňují uživatelům řazení výsledků podle názvu produktu, kategorie nebo dodavatele.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Vytvoření `Click` obslužné rutiny události pro tyto tři ovládací prvky tlačítek. Události obslužnou rutinu, resetovat `StartRowIndex` na hodnotu 0, nastavte `SortExpression` na odpovídající hodnotu a obnovení vazby dat opakovače:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Že všechny existuje s je k němu! Když došlo k několika kroků získat vlastní řazení a stránkování implementována, kroky měla velmi podobné těm, které jsou potřebné pro výchozí stránkování. Obrázek 18 ukazuje produkty při prohlížení na poslední stránku dat při seřazené podle kategorie.


[![Zobrazí se poslední Data stránky, Sorted podle kategorií,](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Obrázek 18**: poslední stránku dat, Sorted podle kategorií, se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> V předchozích příkladech, při řazení podle dodavatele, které dodavatel byl použit jako výraz řazení. Ale pro vlastní implementaci stránkování, potřebujeme používat NázevSpolečnosti. Důvodem je, že uložené procedury, která je zodpovědná za implementaci vlastní stránkování `GetProductsPagedAndSorted` předá výraz řazení do `ROW_NUMBER()` – klíčové slovo, `ROW_NUMBER()` – klíčové slovo vyžaduje místo aliasu, raději skutečný název sloupce. Proto musí používáme `CompanyName` (název sloupce v `Suppliers` tabulky) namísto aby alias použitý v `SELECT` dotazu (`SupplierName`) pro výraz řazení.


## <a name="summary"></a>Souhrn

Ani DataList ani opakovače nabízí integrovanou podporu řazení, ale pomocí bit kódu a vlastní řazení rozhraní, je možné přidat tyto funkce. Při implementaci řazení, ale není stránkování, výrazů řazení lze zadat prostřednictvím `DataSourceSelectArguments` objekt předaný do ObjectDataSource s `Select` metoda. To `DataSourceSelectArguments` objekt s `SortExpression` vlastnost může být přiřazena aplikace ObjectDataSource s `Selecting` obslužné rutiny události.

Pro přidání možností řazení do DataList nebo opakovače, který již poskytuje podporu stránkování, je nejjednodušší způsob přizpůsobení vrstvu obchodní logiky obsahovat metodu, která přijímá výraz řazení. Tyto informace lze předat pak ve prostřednictvím parametru v ObjectDataSource s `SelectParameters`.

V tomto kurzu dokončení součást stránkování a řazení s ovládacími prvky DataList a opakovače. Naše další a finální kurzu bude zkontrolujte postup přidání tlačítka na DataList a opakovače s šablony chcete-li provést některé funkce vlastní, zahájená uživatelem na základě položky.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl David Suru. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
