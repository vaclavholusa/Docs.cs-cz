---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: "Stránkování Data sestavy v DataList nebo prvku Repeater (VB) | Microsoft Docs"
author: rick-anderson
description: "Při řazení podporu nebo DataList ani opakovače nabídka Automatické stránkování tento kurz ukazuje, jak přidat podporu stránkování DataList nebo opakovače..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cb469252dc36ced98357dd984d36668af1c430b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Data sestavy stránkování v DataList nebo prvku Repeater (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) nebo [stáhnout PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Při DataList ani opakovače nabídka Automatické stránkování nebo řazení podporu, tento kurz ukazuje, jak přidat podporu stránkování DataList nebo opakovače, což umožňuje mnohem víc možností stránkování a data zobrazení rozhraní.


## <a name="introduction"></a>Úvod

Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. Například při hledání ASP.NET v online knihkupectví objednáváte knihy, může být stovky takové knihy, ale v sestavě Výpis výsledky hledání jsou uvedeny pouze deset odpovídá na stránce. Kromě toho můžete výsledky seřadit podle název, ceny, počet stránek, jméno autora a tak dále. Jak již bylo zmíněno [stránkování a řazení dat sestavy](../paging-and-sorting/paging-and-sorting-report-data-vb.md) kurzu, ovládací prvky GridView, DetailsView a FormView všechny poskytují integrovanou podporu stránkování, který se dá nastavit na značek zaškrtávací políčko. GridView zahrnuje taky podporu řazení.

Bohužel DataList ani opakovače nabízí automatické stránkování nebo řazení podpory. V tomto kurzu podíváme, jak přidat podporu stránkování DataList nebo opakovače. Jsme musí ručně vytvořit rozhraní stránkování, zobrazení na příslušnou stránku záznamů a nezapomeňte se navštívené napříč postback stránky. Když to trvat další čas a kódu než pomocí GridView, DetailsView nebo FormView, DataList a opakovače povolit pro mnohem víc možností stránkování a data zobrazení rozhraní.

> [!NOTE]
> Tento kurz se zaměřuje jen na stránkování. V dalším kurzu jsme budete zapněte naše pozornost přidání schopností řazení.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Přidání stránkování a řazení kurz webové stránky

Než začneme v tomto kurzu, umožní s nejprve přidat na stránky ASP.NET, které budeme potřebovat pro tento kurz a dalším chvíli trvat. Začněte vytvořením novou složku v projektu s názvem `PagingSortingDataListRepeater`. Dál přidejte následující pět stránek ASP.NET do této složky rozmístit nakonfigurované na používání stránky předlohy `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Vytvořte složku PagingSortingDataListRepeater a přidání stránky kurz ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Obrázek 1**: vytvoření `PagingSortingDataListRepeater` složky a přidat stránky kurz ASP.NET


Dále otevřete `Default.aspx` stránky a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek z `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [hlavní stránky a webové navigace](../introduction/master-pages-and-site-navigation-vb.md) kurzu mapy webu a zobrazí výčet tyto kurzy v aktuálním oddílu v seznamu s odrážkami.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Aby bylo možné používat seznamu s odrážkami zobrazení stránkování a řazení kurzů, které jsme budete vytvářet, je potřeba přidat je do mapy webu. Otevřete `Web.sitemap` souboru a přidejte následující kód po úpravy a odstranění se značkami uzlu mapy DataList lokality:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="a-review-of-paging"></a>O stránkování

V předchozích kurzech jsme viděli postup data v ovládacích prvcích GridView DetailsView a FormView procházet po stránkách. Tyto tři ovládací prvky nabízejí jednoduchý formu stránkování názvem *výchozí stránkování* , může být implementována jednoduše kontrola v inteligentní značky ovládacího prvku s možnost pro povolení stránkování. S výchozí stránkování, při každém požadavku na stránku dat, buď na první stránce navštívit nebo když uživatel přejde na jinou stránku dat GridView, DetailsView, nebo znovu žádostí FormView řízení *všechny* dat z ObjectDataSource. Ji pak výstřižků na konkrétní sadu záznamů pro zobrazení daného index požadované stránky a počet záznamů zobrazených na stránce. Jsme probrali výchozí stránkování v podrobně [stránkování a řazení dat sestavy](../paging-and-sorting/paging-and-sorting-report-data-vb.md) kurzu.

Vzhledem k tomu, že výchozí stránkování znovu požaduje všechny záznamy pro jednotlivé stránky, není praktické při procházení dostatečně velké objemy dat. Představte si například stránkování až 50 000 záznamů s velikostí stránky 10. Pokaždé, když uživatel přesune na novou stránku, všechny 50 000 záznamů musí být načtená z databáze, i když se zobrazí pouze deset z nich.

*Vlastní stránkování* nedostatečnému výkonu výchozí stránkování řeší tím, že metodou jenom přesné podmnožinu záznamů zobrazíte na požadovanou stránku. Při implementaci vlastní stránkování, jsme musíte napsat dotaz SQL, který vrátí efektivně právě správnou sadu záznamů. Jsme viděli, jak vytvořit dotaz, pomocí systému SQL Server 2005 s novou [ `ROW_NUMBER()` – klíčové slovo](http://www.4guysfromrolla.com/webtech/010406-1.shtml) zpět v [efektivně stránkování prostřednictvím velké objemy dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) kurzu.

K implementaci stránkování na výchozím nastavení v ovládacích prvcích DataList nebo opakovače, můžeme použít [ `PagedDataSource` třída](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.aspx) jako obálku kolem `ProductsDataTable` jejichž obsah se stránkování. `PagedDataSource` Třída má `DataSource` vlastnost, která lze přiřadit k žádné vyčíslitelný objekt a [ `PageSize` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) a [ `CurrentPageIndex` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) označují, kolik záznamů do Zobrazit na stránce a index aktuální stránky. Jakmile tyto vlastnosti byly nastaveny, `PagedDataSource` lze použít jako zdroj dat o všech datech ovládací prvek webu. `PagedDataSource`, Když ve výčtu, bude pouze vrátit odpovídající podmnožinu záznamů jeho vnitřní `DataSource` na základě `PageSize` a `CurrentPageIndex` vlastnosti. Obrázek 4 znázorňuje funkce `PagedDataSource` třídy.


![PagedDataSource zabalí vyčíslitelný objekt stránkovatelné rozhraní](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Obrázek 4**: `PagedDataSource` zabalí vyčíslitelný objekt stránkovatelné rozhraní


`PagedDataSource` Objektu lze vytvořit a konfigurovat přímo z vrstvu obchodní logiky a svázané se DataList nebo opakovače prostřednictvím ObjectDataSource, nebo vytvořit a nakonfigurovat přímo v kódu třídy stránky s ASP.NET. Pokud se používá pozdější přístup, jsme musí nepoužít ObjectDataSource a místo toho prostřednictvím kódu programu vytvořte vazbu stránkové data DataList nebo opakovače.

`PagedDataSource` Objekt také obsahuje vlastnosti, které chcete podporovat vlastní stránkování. Jsme ale vynechat, pomocí `PagedDataSource` pro vlastní stránkování, protože jsme už BLL metody `ProductsBLL` třída určená pro vlastní stránkování, který vrátí přesné záznamy, které chcete zobrazit.

V tomto kurzu budeme zabývat implementace výchozí stránkování v DataList přidáním nové metody pro `ProductsBLL` třídu, která vrátí správně nakonfigurované `PagedDataSource` objektu. V dalším kurzu uvidíme, jak používat vlastní stránkování.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Krok 2: Přidání metody výchozí stránkování v vrstvu obchodní logiky

`ProductsBLL` Třída aktuálně má metodu pro vrácení všechny informace o produktu `GetProducts()` a jeden pro vrácení určitou podskupinu produkty počáteční indexem `GetProductsPaged(startRowIndex, maximumRows)`. S výchozí stránkování, rutina GridView, DetailsView a FormView řídí všechny použití `GetProducts()` metoda načíst všechny produkty, ale pak použít `PagedDataSource` interně k zobrazení pouze správné podmnožinu záznamů. Tato funkce s ovládacími prvky DataList a opakovače replikovat, můžeme vytvořit nové metody v BLL, která napodobuje toto chování.

Přidejte metodu k `ProductsBLL` třídu s názvem `GetProductsAsPagedDataSource` který přebírá dva vstupní parametry celé číslo:

- `pageIndex`index stránky, které chcete zobrazit, indexované na nule, a
- `pageSize`počet záznamů zobrazených na stránce.

`GetProductsAsPagedDataSource`Spustí načtením *všechny* záznamy z `GetProducts()`. Ta poté vytvoří `PagedDataSource` objektu, nastavení jeho `CurrentPageIndex` a `PageSize` vlastnosti na hodnoty předané `pageIndex` a `pageSize` parametry. Metoda ukončí vrácení nakonfigurovaný `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Krok 3: Zobrazení informací o produktu v DataList, pomocí výchozího stránkování

S `GetProductsAsPagedDataSource` metoda přidán do `ProductsBLL` třídu, můžete nyní vytvoříme DataList nebo opakovače, která poskytuje výchozí stránkování. Začněte otevřením `Paging.aspx` stránku `PagingSortingDataListRepeater` složku a přetáhněte DataList z panelu nástrojů na návrháře nastavení DataList s `ID` vlastnost `ProductsDefaultPaging`. Inteligentní značky DataList s, vytvořte nový ObjectDataSource s názvem `ProductsDefaultPagingDataSource` a nakonfigurujte ji tak, aby ho načte data pomocí `GetProductsAsPagedDataSource` metoda.


[![Vytvoření ObjectDataSource a nakonfigurujte ho na použití (GetProductsAsPagedDataSource) – metoda](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Obrázek 5**: ObjectDataSource vytvořit a nakonfigurovat jej pro použití `GetProductsAsPagedDataSource` `()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Nastavte rozevírací seznamy v aktualizaci UPDATE, INSERT a odstranit karty na (žádný).


[![Nastavte rozevíracím seznamu v aktualizaci UPDATE, INSERT a odstranit karty na (žádný)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Obrázek 6**: nastavte rozevíracím seznamu v aktualizaci UPDATE, INSERT a odstranit karty na (žádný) ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Vzhledem k tomu `GetProductsAsPagedDataSource` metoda očekává dva vstupní parametry, Průvodce zobrazí výzvu nám pro zdroj těchto hodnot parametrů.

Index stránky a hodnoty velikosti stránky musí mít na paměti mezi postback. Se mohou být uloženy ve stav zobrazení, trvalé do řetězce dotazu, uložené v proměnné relace nebo zapamatovaných pomocí některé další techniky. V tomto kurzu použijeme řetězce dotazu, který má výhodu v podobě povolení konkrétní stránky dat se aktuálně přihlášeni.

Zejména použijte index stránky pole řetězce dotazu a pageSize pro `pageIndex` a `pageSize` parametry, v uvedeném pořadí (viz obrázek 7). Za chvíli nastavit výchozí hodnoty pro tyto parametry, jako hodnoty řetězce dotazu won t být k dispozici, pokud uživatel navštíví nejprve tuto stránku. Pro `pageIndex`, nastavit výchozí hodnota 0 (ve kterém se zobrazí na první stránku dat) a `pageSize` s výchozí hodnotu na 4.


[![Použít řetězec dotazu jako zdroj pro index stránky a pageSize parametry](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Obrázek 7**: použít jako zdroj pro řetězec dotazu `pageIndex` a `pageSize` parametry ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Po dokončení konfigurace ObjectDataSource, Visual Studio automaticky vytvoří `ItemTemplate` pro prvku DataList. Přizpůsobení `ItemTemplate` tak, aby se zobrazují pouze název produktu s, kategorie a dodavatele. Také nastavit DataList s `RepeatColumns` vlastnost na hodnotu 2, jeho `Width` na 100 % a jeho `ItemStyle` s `Width` na 50 %. Tato nastavení Šířka bude poskytovat stejné mezery pro dva sloupce.

Po provedení těchto změn, značku s DataList a ObjectDataSource by měl vypadat podobně jako následující:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Vzhledem k tomu, že jsme nejsou provádění nějakou aktualizaci nebo odstranění funkce v tomto kurzu, zakážete stav zobrazení s DataList ke snížení velikosti vykreslené stránky.


Při počátečním návštěvě této stránky prostřednictvím prohlížeče, ani `pageIndex` ani `pageSize` jsou uvedeny parametry řetězce dotazu. Proto se použijí výchozí hodnoty 0 a 4. Jak ukazuje obrázek 8, výsledkem DataList, který zobrazí první čtyři produkty.


[![První čtyři produkty jsou uvedeny](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Obrázek 8**: jsou uvedené první čtyři produkty ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Bez rozhraní stránkování, zde s aktuálně žádné přehledné znamená, že pro uživatele a přejděte na druhé stránce data. V kroku 4 vytvoříme rozhraní stránkování. Teď když, stránkování můžete pouze provést zadáním přímo kritéria stránkování v řetězci dotazu. Můžete například změnit adresu URL v adresním řádku prohlížeče s z druhé stránce zobrazíte `Paging.aspx` k `Paging.aspx?pageIndex=2` a stiskněte Enter. To způsobí, že data zobrazit druhé stránce (viz obrázek 9).


[![Zobrazí se druhé Data stránky](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Obrázek 9**: druhý stránky dat se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Krok 4: Vytvoření rozhraní stránkování

Existují celou řadu různých stránkování rozhraní, které lze provádět. Ovládací prvky GridView DetailsView a FormView obsahují čtyři různé rozhraní zvolit si takový:

- **Další, předchozí** mohou uživatelé přesouvat na dobu, a další nebo předchozí jeden jednu stránku.
- **Další, předchozí, první, poslední** kromě tlačítka Další a předchozí toto rozhraní obsahuje první a poslední tlačítka pro přesun na úplně první nebo poslední stránku.
- **Číselné** uvádí číslo stránky v rozhraní stránkování, umožňuje uživatelům snadno přejít na konkrétní stránku.
- **Číselné literály, první, poslední** kromě číslo číselné stránky obsahuje tlačítka pro přesun na úplně první nebo poslední stránku.

Pro DataList a opakovače jsou zodpovědní za rozhodování o stránkování rozhraní a jeho implementace. To zahrnuje vytváření potřebné ovládací prvky webového na stránce a zobrazování k požadované stránce, když po kliknutí na konkrétní tlačítko rozhraní stránkování. Kromě toho některé ovládací prvky rozhraní stránkování muset zakázán. Například při prohlížení na první stránku dat pomocí další, předchozí, první, poslední rozhraní, první a předchozí tlačítka by zakázán.

V tomto kurzu použijte umožňují s na další, předchozí, první, poslední rozhraní. Přidejte čtyři ovládací prvky webového tlačítko na stránku a nastavte jejich `ID` chcete s `FirstPage`, `PrevPage`, `NextPage`, a `LastPage`. Nastavte `Text` vlastnosti, které chcete &lt; &lt; nejdřív &lt; Prev, další &gt;a poslední &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Dále vytvořte `Click` obslužné rutiny události pro každý z těchto tlačítek. Za chvíli přidáme kód nutné zobrazit k požadované stránce.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Nezapomeňte, celkový počet záznamů stránkování prostřednictvím

Bez ohledu na vybrané rozhraní stránkování musíme výpočetní a mějte na paměti, celkový počet záznamů stránkování prostřednictvím. Celkový počet řádků (ve spojení s velikost stránky) určuje, kolik celkového počtu stránek dat jsou stránkování prostřednictvím, která určuje, co ovládací prvky rozhraní stránkování se přidají nebo jsou povolené. V další, předchozí, první poslední rozhraní, které jsme vytváříte počet stránek slouží dvěma způsoby:

- Chcete-li zjistit, zda jsme zobrazenou poslední stránku, v takovém případě jsou zakázány tlačítka Další a poslední.
- Pokud uživatel klikne na poslední tlačítko musíme whisk je na poslední stránce, jejichž index je jeden menší než stránce count.

Počet stránek se počítá jako horní meze celkový počet řádků děleného velikostí stránky. Například pokud jsme jsou stránkování prostřednictvím 79 záznamy s čtyři záznamy na stránce, pak počet stránek je 20 (horní meze 79 / 4). Pokud se používá rozhraní číselné stránkování, tyto informace nám informuje o tom, kolik tlačítka číselné stránky se zobrazí; Pokud naše rozhraní stránkování obsahuje další nebo poslední tlačítka, počet stránek se používá k určení, kdy chcete zakázat tlačítko Další nebo poslední.

Pokud stránkování rozhraní obsahuje poslední tlačítko, je nutné, aby celkový počet záznamů stránkování prostřednictvím zapamatován napříč postback tak, aby při kliknutí na tlačítko poslední můžeme určit poslední index stránky. K provedení této vytvořit `TotalRowCount` vlastnost v třídě kódu stránky s ASP.NET, která je uchována jeho hodnota stavu zobrazení:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Kromě `TotalRowCount`trvat několik minut pro vytvoření jen pro čtení vlastnosti úrovně stránky pro snadno přístup k index stránky, velikost stránky a stránky počet:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Určení celkový počet záznamů stránkování prostřednictvím

`PagedDataSource` Objekt byl vrácen ze ObjectDataSource s `Select()` metoda má v něm *všechny* záznamů produktu, i když jenom některé z nich se zobrazují v prvku DataList. `PagedDataSource` s [ `Count` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.count.aspx) vrátí počet položek, které se zobrazí v DataList; [ `DataSourceCount` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) vrátí celkový počet položek v rámci `PagedDataSource`. Proto je potřeba přiřadit ASP.NET stránky s `TotalRowCount` hodnotu vlastnosti o `PagedDataSource` s `DataSourceCount` vlastnost.

K tomu, vytvoření obslužné rutiny události pro ObjectDataSource s `Selected` událostí. V `Selected` obslužné rutiny události budeme mít přístup k vrácenou hodnotu ObjectDataSource s `Select()` metoda v tomto případě `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>K požadované stránce dat zobrazení

Po kliknutí na jedno z tlačítek v rozhraní stránkování, musíme zobrazení dat k požadované stránce. Vzhledem k tomu, že jsou zadány parametry stránkování pomocí řetězce dotazu, zobrazíte k požadované stránce použití dat `Response.Redirect(url)` tak, aby měl uživatel s prohlížeči znovu žádost o na `Paging.aspx` stránky s příslušnými parametry stránkování. Například zobrazíte druhé stránce dat jsme by přesměruje uživatele na `Paging.aspx?pageIndex=1`.

K provedení této vytvořit `RedirectUser(sendUserToPageIndex)` metoda, která přesměruje uživatele na `Paging.aspx?pageIndex=sendUserToPageIndex`. Potom tuto metodu volat ze čtyř tlačítko `Click` obslužné rutiny událostí. V `FirstPage` `Click` obslužné rutiny události, volání `RedirectUser(0)`, tak o jejich odeslání na první stránku; v `PrevPage` `Click` obslužné rutiny události, použijte `PageIndex - 1` jako index stránky; a tak dále.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Pomocí `Click` dokončit obslužné rutiny událostí, záznamy DataList s možností stránkování prostřednictvím klepnutím na tlačítka. Za chvíli vyzkoušejte ji!

## <a name="disabling-paging-interface-controls"></a>Zakázání stránkování ovládací prvky rozhraní

Všechny čtyři tlačítka v současné době jsou povolena bez ohledu na stránku zobrazení. Chceme zakázat tlačítka první a předchozí až na první stránku dat a tlačítka Další a poslední při poslední stránkou. `PagedDataSource` Objekt vrácený ObjectDataSource s `Select()` metoda má vlastnosti [ `IsFirstPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) a [ `IsLastPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , jsme zkontrolovat, určete, pokud jsme prohlížíte na první nebo poslední stránku data.

Přidejte následující ObjectDataSource s `Selected` obslužné rutiny události:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Pomocí tohoto přidání bude zakázáno tlačítka první a předchozí při prohlížení na první stránce tlačítka Další a poslední bude zakázáno, při zobrazení poslední stránky.

Umožňují s dokončení rozhraní stránkování podle informující uživatele co se stránky znovu aktuálně a celkový počet stránek neexistuje. Přidání ovládacího prvku popisek na stránku a nastavit jeho `ID` vlastnost `CurrentPageNumber`. Nastavte její `Text` vlastnost ObjectDataSource s vybrané obslužné rutiny události takové obsahuje aktuální stránky zobrazení (`PageIndex + 1`) a celkový počet stránek (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Obrázek 10 ukazuje `Paging.aspx` po první návštěvě. Vzhledem k tomu, že řetězec dotazu je prázdný, výchozí hodnoty DataList zobrazí první čtyři produktů; tlačítka první a předchozí jsou zakázány. Klepnutím na další zobrazíte další čtyři záznamy (viz obrázek 11); tlačítka první a předchozí jsou nyní k dispozici.


[![Zobrazí se první Data stránky](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Obrázek 10**: první stránky dat se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![Zobrazí se druhé Data stránky](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Obrázek 11**: druhý stránky dat se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> Rozhraní stránkování lze dále zvýšit tím, že se uživatel k určení, kolik stránky zobrazíte na stránce. Například rozevírací seznam nebylo možné přidat možnosti velikosti stránky výpis jako 5, 10, 25, 50 a všechny. Po výběru velikost stránky, bude uživatel muset přesměrováni zpět na `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Nechat implementace toto vylepšení jako cvičení pro čtečku.


## <a name="using-custom-paging"></a>Pomocí vlastních stránkování

Na stránkách DataList prostřednictvím svá data pomocí techniky stránkování neefektivní výchozí. Při procházení dostatečně velké objemy dat, je nutné použít vlastní stránkování. I když podrobnosti implementace mírně lišit, principy implementace vlastní stránkování v DataList jsou stejné jako s výchozí stránkování. S vlastní stránkování, použijte `ProductBLL` třídu s `GetProductsPaged` – metoda (místo `GetProductsAsPagedDataSource`). Jak je popsáno v [efektivně stránkování prostřednictvím velké objemy dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) kurzu `GetProductsPaged` musí být předán počáteční řádek indexu a maximální počet řádků vrátit. Tyto parametry se dají udržovat pomocí jenom jako řetězce dotazu `pageIndex` a `pageSize` parametry použité ve výchozích stránkování.

Od tam s žádné `PagedDataSource` s vlastní stránkování, musí použít alternativní techniky k určení celkový počet záznamů stránkování prostřednictvím a jestli jsme re na první nebo poslední stránku dat zobrazení. `TotalNumberOfProducts()` Metoda v `ProductsBLL` třída vrátí celkový počet produktů stránkování prostřednictvím. Pokud chcete zjistit, pokud je na první stránku data zobrazení, zkontrolujte počáteční index řádku Pokud je nula, a zobrazení na první stránku. Pokud počáteční index řádku plus maximální počet vrácených řádků je větší než nebo rovna hodnotě celkový počet záznamů stránkování prostřednictvím bude poslední stránka zobrazena.

Jsme budete prozkoumejte, které implementují vlastní stránkování podrobněji v dalším kurzu.

## <a name="summary"></a>Souhrn

Při DataList ani opakovače nabízí mimo podporu stránkování pole Najít v GridView, DetailsView a FormView ovládací prvky, tyto funkce lze přidat s minimálním úsilím. Nejjednodušší způsob, jak implementovat výchozí stránkování je zabalit celou sadu produktů v rámci `PagedDataSource` a pak vytvořte vazbu `PagedDataSource` DataList nebo opakovače. V tomto kurzu jsme přidali `GetProductsAsPagedDataSource` metodu `ProductsBLL` třídy vracení `PagedDataSource`. `ProductsBLL` Třídy již obsahuje metody, které jsou potřebné pro vlastní stránkování `GetProductsPaged` a `TotalNumberOfProducts`.

Společně s načítání buď přesné sadu záznamů pro zobrazení pro vlastní stránkování nebo všechny záznamy v `PagedDataSource` pro výchozí stránkování, také je potřeba ručně přidat rozhraní stránkování. V tomto kurzu jsme vytvořili další, předchozí, první, poslední rozhraní s čtyři ovládací prvky webového tlačítko. Navíc se přidala ovládací prvek popisek zobrazení aktuální číslo stránky a celkový počet stránek.

V dalším kurzu vidíte přidání řazení podpory DataList a opakovače. Jsme dozvíte se taky, jak vytvořit DataList, který může být stránkovaného i seřadit (s příklady použití výchozích a vlastních stránkování).

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Liz Shulok, Ken Pespisa a Bernadette Leigh. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](sorting-data-in-a-datalist-or-repeater-control-cs.md)
[další](sorting-data-in-a-datalist-or-repeater-control-vb.md)
