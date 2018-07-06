---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: Stránkování dat sestavy ovládacími DataList nebo Repeater (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při DataList ani Repeater nabídka Automatické stránkovací nebo řazení podpory tento kurz ukazuje, jak přidat podporu stránkování v prvku DataList nebo Repeater...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 90df9c096b5411201da35b7076fdd3cd9b1f86d1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842316"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Stránkování dat sestavy ovládacími DataList nebo Repeater (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) nebo [stahovat PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> Zatímco DataList ani Repeater nabídka Automatické stránkování a řazení podpory, tento kurz ukazuje, jak přidat podporu stránkování v prvku DataList nebo Repeater, což umožňuje mnohem flexibilnější stránkování a data zobrazení rozhraní.


## <a name="introduction"></a>Úvod

Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. Například při vyhledávání pro knihy technologie ASP.NET v online knihkupectví, může být stovky těchto knihy, ale v sestavě Výpis výsledky hledání jsou uvedeny pouze deset shody na stránku. Navíc můžete výsledky seřadit podle názvu, ceny, počet stránek, jméno autora a tak dále. Jak jsme probírali v [stránkování a řazení dat sestavy](../paging-and-sorting/paging-and-sorting-report-data-cs.md) kurz, ovládací prvky GridView, DetailsView a FormView všechny poskytují integrovanou podporu stránkování, který se dá nastavit na značek zaškrtávací políčko. Řazení podpora zahrnuje také prvku GridView.

Bohužel v prvku DataList ani Repeater nabízet automatické stránkování nebo řazení podpory. V tomto kurzu prozkoumáme, jak přidat podporu stránkování v prvku DataList nebo Repeater. Jsme musí ručně vytvořit stránkovací rozhraní, zobrazí odpovídající stránku záznamy a mějte na paměti se navštívené postbacků stránky. Když to trvá, než více času a kódu než pomocí ovládacího prvku GridView, DetailsView nebo FormView, ovládacích prvků DataList a Repeater povolit pro mnohem flexibilnější stránkování a data zobrazení rozhraní.

> [!NOTE]
> Tento kurz se zaměřuje výhradně na stránkování. V dalším kurzu jsme vám zapnout pozornost na přidání možnosti řazení.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Přidání stránkování a řazení kurz webových stránek

Než začneme v tomto kurzu, umožní s nejdřív využít pro přidání stránek ASP.NET, budeme potřebovat pro tento kurz a další příkaz. Začněte tím, že vytvoříte novou složku v projektu s názvem `PagingSortingDataListRepeater`. Dále přidejte následující pět stránek ASP.NET do této složky s všechny z nich nakonfigurovat tak, aby na hlavní stránce `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Vytvořte složku PagingSortingDataListRepeater a přidávání stránek kurz ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Obrázek 1**: vytvoření `PagingSortingDataListRepeater` složky a přidávání stránek kurz ASP.NET


Dále otevřete `Default.aspx` stránku a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku od `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-cs.md) kurzu mapy webu a zobrazí výčet tyto kurzy v aktuálním oddílu v seznamu s odrážkami.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


Abyste měli zobrazení stránkování a řazení kurzů, které jsme vám vytvoření seznamu s odrážkami, potřebujeme přidat je do mapy webu. Otevřít `Web.sitemap` soubor a přidejte následující kód za úpravy a odstranění se značkami uzel mapy webu DataList:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="a-review-of-paging"></a>Přehled stránkování

V předchozích kurzech jsme viděli, jak stránkovat data v ovládacích prvcích ovládacího prvku GridView, DetailsView a FormView. Tyto tři ovládací prvky nabízejí jednoduchý formulář stránkování volá *výchozího stránkování* , který může být implementována pouze kontrola možnost Povolit stránkování v inteligentní značky ovládacího prvku s. Pomocí výchozího stránkování, pokaždé, když je požadováno stránku dat, buď na první stránce navštivte nebo když uživatel přejde na jinou stránku dat prvku GridView, DetailsView, nebo ovládacího prvku FormView znovu požadavků *všechny* dat z Prvek ObjectDataSource. To pak výstřižky si konkrétní sadu záznamů zobrazíte daného indexu požadovanou stránku a počet záznamů zobrazených na stránce. Jsme probírali výchozího stránkování podrobně [stránkování a řazení dat sestavy](../paging-and-sorting/paging-and-sorting-report-data-cs.md) kurzu.

Protože výchozí stránkování znovu vyžádá všechny záznamy pro každou stránku, není praktické při procházení dostatečně velké objemy dat po stránkách. Představte si například stránkování až 50 000 záznamů s velikostí stránky 10. Pokaždé, když uživatel přesune na novou stránku, všechny 50 000 záznamů musí načíst z databáze, i když se zobrazí pouze deset z nich.

*Vlastní stránkování* řeší nedostatečnému výkonu výchozí stránkování podle uchopíte jeho pouze přesné podmnožinu záznamů se zobrazí na požadovanou stránku. Při implementaci vlastní stránkování, jsme musíte napsat dotaz SQL, který efektivně vrátí pouze správnou sadu záznamů. Jsme viděli, jak vytvořit dotaz, pomocí SQL Server 2005 s novou [ `ROW_NUMBER()` – klíčové slovo](http://www.4guysfromrolla.com/webtech/010406-1.shtml) zpátky [efektivně stránkování prostřednictvím velkých objemů dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) kurzu.

K implementaci stránkování na výchozím nastavení v ovládacích prvcích DataList nebo Repeater, můžeme použít [ `PagedDataSource` třídy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) jako obálka kolem `ProductsDataTable` jehož obsah je stránkování. `PagedDataSource` Třída nemá `DataSource` vlastnost, která lze přiřadit libovolný vyčíslitelný objekt a [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) a [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) vlastnosti, které označují, kolik záznamů se má Zobrazit na stránce a aktuální index stránky. Po nastavení těchto vlastností mají `PagedDataSource` může sloužit jako zdroj dat jakýchkoli dat webový ovládací prvek. `PagedDataSource`, Při výčtu, bude pouze vrátit na příslušnou podmnožinu záznamů jeho vnitřní `DataSource` na základě `PageSize` a `CurrentPageIndex` vlastnosti. Obrázek 4 znázorňuje funkce `PagedDataSource` třídy.


![PagedDataSource zabalí vyčíslitelný objekt s rozhraním stránkované](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Obrázek 4**: `PagedDataSource` zabalí vyčíslitelný objekt s rozhraním stránkované


`PagedDataSource` Objekt může být vytvořené a nakonfigurovaná přímo z vrstvy obchodní logiky a vázán na ovládacích prvcích DataList nebo Repeater prostřednictvím ObjectDataSource, nebo může být a nakonfigurovaná přímo v třídě modelu code-behind stránky s ASP.NET. Pokud použijete druhý přístup jsme musí nepoužít ObjectDataSource a místo toho svázat stránkovaná data DataList nebo Repeater prostřednictvím kódu programu.

`PagedDataSource` Objekt má také vlastnosti, které chcete podporovat vlastní stránkování. Jsme ale obejít, použití `PagedDataSource` pro vlastní stránkování, protože už máme BLL metody `ProductsBLL` třídu navrženou pro vlastní stránkování, který vrátí přesné záznamy, které chcete zobrazit.

V tomto kurzu se podíváme implementace výchozího stránkování tak, že přidáte novou metodu pro v a v prvku DataList `ProductsBLL` třídu, která vrátí správně nakonfigurované `PagedDataSource` objektu. V dalším kurzu se podíváme, jak používat vlastní stránkování.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Krok 2: Přidání vrstvy obchodní logiky výchozí metoda stránkování

`ProductsBLL` Třídy aktuálně má metodu pro vrácení všechny informace o produktu `GetProducts()` a jeden pro vracející podmnožinu produktů na počáteční index konkrétní `GetProductsPaged(startRowIndex, maximumRows)`. Pomocí výchozího stránkování prvku GridView, DetailsView a FormView řídí veškeré možnosti použití `GetProducts()` metodu pro načtení všech produktů, ale pak použít `PagedDataSource` interně k zobrazení pouze správné podmnožinu záznamů. Tuto funkci s ovládacími prvky DataList a Repeater replikovat, můžeme vytvořit novou metodu v BLL, která napodobuje toto chování.

Přidejte metodu k `ProductsBLL` třídu s názvem `GetProductsAsPagedDataSource` , který přebírá dva vstupní parametry celé číslo:

- `pageIndex` index stránky zobrazíte indexované od nuly, a
- `pageSize` počet záznamů zobrazených na stránce.

`GetProductsAsPagedDataSource` Spustí načtením *všechny* záznamy z `GetProducts()`. Pak vytvoří `PagedDataSource` objekt nastavení jeho `CurrentPageIndex` a `PageSize` vlastnosti a hodnoty předané-in `pageIndex` a `pageSize` parametry. Metoda dojde k závěru vrácením nakonfigurovaný `PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Krok 3: Zobrazení informací o produktu v DataList pomocí výchozího stránkování

S `GetProductsAsPagedDataSource` metoda přidána do `ProductsBLL` třídy, můžete teď vytvoříme DataList nebo Repeater, která poskytuje výchozí stránkování. Začněte otevřením `Paging.aspx` stránku `PagingSortingDataListRepeater` složky a a v prvku DataList přetáhněte z panelu nástrojů do Návrháře nastavení DataList s `ID` vlastnost `ProductsDefaultPaging`. V prvku DataList s inteligentním, vytvoření nového prvku ObjectDataSource s názvem `ProductsDefaultPagingDataSource` a nakonfigurujte ho tak, aby ho načte data s využitím `GetProductsAsPagedDataSource` metody.


[![Vytvoření ObjectDataSource a nakonfigurujte ho na použití GetProductsAsPagedDataSource () – metoda](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Obrázek 5**: vytvoření ObjectDataSource a konfigurace pro použití `GetProductsAsPagedDataSource` `()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


Nastavte rozevírací seznamy v UPDATE, INSERT a odstranit karty na (žádný).


[![Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit karty na (žádný)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Obrázek 6**: Nastavte rozevírací seznam obsahuje v UPDATE, INSERT a odstranit karty na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


Vzhledem k tomu, `GetProductsAsPagedDataSource` metoda očekává dva vstupní parametry, Průvodce nám vyzve k zadání zdrojové hodnoty těchto parametrů.

Index stránky a hodnoty velikosti stránky musí být zapamatovaných postbacků. Mohou být uloženy v zobrazení stavu, trvale uložena do řetězec dotazu, uložené v proměnné relace nebo uloží, pomocí některé jiné techniky. Pro účely tohoto kurzu používáme řetězce dotazu, který nabízí výhodu v podobě povolení konkrétní stránce dat do záložek.

Zejména použijte řetězec dotazu pole pageIndex a pageSize pro `pageIndex` a `pageSize` parametry, respektive (viz obrázek 7). Za chvíli nastavit výchozí hodnoty těchto parametrů, jako hodnoty řetězce dotazu získaných t být k dispozici, když uživatel navštíví nejprve tuto stránku. Pro `pageIndex`, nastavit výchozí hodnotu 0 (ve kterém se zobrazí první stránka dat) a `pageSize` s výchozí hodnotu 4.


[![Použijte řetězec dotazu jako zdroj pro parametry pageIndex a pageSize](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Obrázek 7**: použijte řetězec dotazu jako zdroj pro `pageIndex` a `pageSize` parametry ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


Po dokončení konfigurace ObjectDataSource, sada Visual Studio automaticky vytvoří `ItemTemplate` pro prvku DataList. Přizpůsobit `ItemTemplate` tak, aby se zobrazí jenom produkt s názvem, kategorie a dodavateli. Také nastavit DataList s `RepeatColumns` vlastnost na 2, jeho `Width` na 100 % a jeho `ItemStyle` s `Width` na 50 %. Tato nastavení šířky bude poskytovat stejné mezery pro dva sloupce.

Po provedení těchto změn, značky s ovládacích prvků DataList a ObjectDataSource by měl vypadat nějak takto:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Protože jsme nejsou provádění žádné aktualizace nebo odstranění funkce v tomto kurzu, můžete kdykoli deaktivovat stav zobrazení v prvku DataList s ke zmenšení velikosti vykreslené stránky.


Při počáteční ani návštěvě této stránky v prohlížeči `pageIndex` ani `pageSize` parametry řetězce dotazu jsou k dispozici. Proto jsou použity výchozí hodnoty 0 a 4. Jak ukazuje obrázek 8, výsledkem v prvku DataList, který se zobrazí první čtyři produkty.


[![První čtyři produkty jsou uvedené.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Obrázek 8**: The první čtyři produktů se nachází ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


Bez rozhraní stránkování, tam s aktuálně není jednoduché znamená, že uživatel přejít na druhé stránce data. Vytvoříme rozhraní stránkování v kroku 4. Prozatím se však stránkování pouze dosáhnete přímo zadáním kritérií stránkování v řetězec dotazu. Například chcete-li zobrazit na druhé stránce, změnit adresu URL do adresního řádku prohlížeče s z `Paging.aspx` k `Paging.aspx?pageIndex=2` a stiskněte Enter. To způsobí, že data, který se má zobrazit na druhé stránce (viz obrázek 9).


[![Zobrazí se druhé Data stránky](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Obrázek 9**: druhý stránky dat se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Krok 4: Vytvoření rozhraní stránkování

Existuje široká škála různých stránkovací rozhraní, které je možné implementovat. Ovládací prvky GridView DetailsView a FormView poskytují čtyři různá rozhraní zvolit si takový:

- **Další, předchozí** mohou uživatelé přesouvat jednu stránku najednou, buď další nebo předchozí obrazovku.
- **Další, předchozí; First, Last** vedle tlačítka Další a předchozí toto rozhraní obsahuje jméno a příjmení tlačítka pro přechod na úplně první nebo poslední stránku.
- **Číselné** obsahuje číslo stránky stránkovací rozhraní, která uživatelům umožňuje rychle přejít na konkrétní stránce.
- **Číselné literály, nejprve poslední** kromě čísla číselné stránek obsahuje tlačítka pro přechod na úplně první nebo poslední stránku.

Pro prvky DataList a Repeater máme odpovědnost za rozhodování o stránkovací rozhraní a jeho implementace. To zahrnuje vytváření potřebných kontrol webové stránky a zobrazení požadovaná stránka při kliknutí na konkrétní rozhraní tlačítka stránkování. Kromě toho některé ovládací prvky rozhraní stránkování muset zakázat. Při procházení první stránky dat s využitím další, předchozí, nejprve poslední rozhraní, první a předchozí tlačítka by zakázána.

Pro účely tohoto kurzu, umožňují s použití na další, předchozí, nejprve poslední rozhraní. Přidejte čtyři ovládací prvky tlačítka webové stránky a nastavte jejich `ID` s `FirstPage`, `PrevPage`, `NextPage`, a `LastPage`. Nastavte `Text` vlastností &lt; &lt; nejprve &lt; předchozí, další &gt;a poslední &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

Dále vytvořte `Click` obslužnou rutinu události pro každý z těchto tlačítek. Za chvíli přidáme kód, který zobrazíte požadovanou stránku.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Zapamatování celkový počet záznamů stránkování prostřednictvím

Bez ohledu na vybrané rozhraní stránkování musíme compute a celkový počet záznamů stránkování prostřednictvím mějte na paměti. Celkový počet řádků (ve spojení s velikost stránky) určuje, kolik celkem stránek dat jsou stránkování, který určuje, co ovládací prvky rozhraní stránkování se přidají nebo jsou povoleny. V další, předchozí, první poslední rozhraní, které vytváříme, počet stránek slouží dvěma způsoby:

- Chcete-li zjistit, jestli jsme se v takovém případě zobrazuje poslední stránky, tlačítka Další a poslední jsou zakázané.
- Pokud uživatel klepne na poslední tlačítko musíme whisk je na poslední stránce, jejíž index je jeden menší než stránce počítat.

Počet stránek se vypočte jako celkový počet řádků horní mez děleného velikostí stránky. Například, pokud jsme se procházení 79 záznamy s čtyři záznamy na stránce, pak počet stránek je 20 (horní mez 79 / 4). Pokud používáme rozhraní číselné stránkování, tyto informace nám informuje o tom, kolik číselné stránky tlačítka zobrazíte; Pokud naše stránkovací rozhraní obsahuje další nebo poslední tlačítka, počet stránek slouží k určení toho, kdy chcete zakázat tlačítko Další nebo poslední.

Pokud rozhraní stránkování obsahuje poslední tlačítko, je nutné, že celkový počet záznamů stránkování prostřednictvím zapamatuje postbacků tak, aby po kliknutí na tlačítku poslední můžeme určit poslední index stránky. K provedení této, vytvořit `TotalRowCount` vlastnost ve třídě použití modelu code-behind stránky s ASP.NET, která udržuje jeho hodnotu na zobrazení stavu:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Kromě `TotalRowCount`trvat několik minut vytvořit vlastnosti jen pro čtení úrovně stránky snadného přístupu k indexu stránky, velikost stránky a počet stran:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Určení celkový počet záznamů stránkování prostřednictvím

`PagedDataSource` Objekt se vrátil ze ObjectDataSource s `Select()` metoda má v něm *všechny* záznamů produktu, i když pouze jejich podmnožinu se zobrazí v ovládacím prvku DataList. `PagedDataSource` s [ `Count` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) vrátí počet položek, které se zobrazí v ovládacím prvku DataList; [ `DataSourceCount` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) vrátí celkový počet položek v rámci `PagedDataSource`. Proto potřebujeme přiřadit stránky s ASP.NET `TotalRowCount` vlastnost hodnotu z `PagedDataSource` s `DataSourceCount` vlastnost.

Chcete-li to provést, vytvořte obslužnou rutinu události pro prvek ObjectDataSource s `Selected` událostí. V `Selected` obslužná rutina události máme přístup k prvku ObjectDataSource s vrácenou hodnotu `Select()` metoda v tomto případě `PagedDataSource`.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>Požadovanou stránku dat zobrazení

Po kliknutí na jedno z tlačítek v rozhraní stránkování, musíme požadovanou stránku dat zobrazení. Protože jsou zadány parametry stránkování prostřednictvím řetězce dotazu, požadovaná stránka za využívání dat zobrazíte `Response.Redirect(url)` chcete, aby uživatel s prohlížeči znovu požádat o `Paging.aspx` stránky s příslušnými parametry stránkování. Například pro zobrazení dat na druhé stránce jsme by má uživatel přesměrovat `Paging.aspx?pageIndex=1`.

K provedení této, vytvořit `RedirectUser(sendUserToPageIndex)` metodu, která přesměruje uživatele na `Paging.aspx?pageIndex=sendUserToPageIndex`. Potom tuto metodu volat z čtyři tlačítka `Click` obslužných rutin událostí. V `FirstPage` `Click` obslužná rutina události, volání `RedirectUser(0)`, aby jim odeslala na první stránku; v `PrevPage` `Click` obslužná rutina události, použijte `PageIndex - 1` jako index stránky; a tak dále.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

S `Click` dokončení obslužné rutiny událostí, záznamy DataList s možností stránkování prostřednictvím pomocí tlačítek. Za chvíli to vyzkoušet!

## <a name="disabling-paging-interface-controls"></a>Zakázání stránkování ovládací prvky rozhraní

V současné době jsou povoleny všechny čtyři tlačítka bez ohledu na stránku zobrazení. Ale chcete zakázat tlačítka první a předchozí při zobrazování první stránka dat a tlačítka Další a poslední při poslední stránkou. `PagedDataSource` Vrácený ObjectDataSource s `Select()` metoda má vlastnosti [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) a [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , prozkoumáme můžete určit, pokud jsme prohlížíte první nebo poslední stránku data.

Přidejte následující prvek ObjectDataSource s `Selected` obslužné rutiny události:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Uveďte první a předchozí tlačítka se deaktivuje, při procházení první stránky, tlačítka Další a poslední bude zakázáno, při zobrazení na poslední stránce.

Umožňují s dokončení rozhraní stránkování podle informací pro uživatele co jsou stránce re aktuálně zobrazenou a celkový počet stránek existovat. Přidání ovládacího prvku popisek na stránku a nastavit jeho `ID` vlastnost `CurrentPageNumber`. Nastavte jeho `Text` vlastnost v prvku ObjectDataSource s vybrané obslužná rutina události takové, že zahrnuje aktuální stránky zobrazení (`PageIndex + 1`) a celkový počet stránek (`PageCount`).


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

Obrázek 10 ukazuje `Paging.aspx` když první uživatel. Řetězec dotazu je prázdný, prvku DataList výchozím nastavení zobrazí první čtyři produkty; První a předchozí tlačítka jsou zakázané. Kliknutím na další zobrazí další čtyři záznamy (viz obrázek 11); První a předchozí tlačítka jsou nyní k dispozici.


[![Zobrazí se první Data stránky](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Obrázek 10**: první stránky dat se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![Zobrazí se druhé Data stránky](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Obrázek 11**: druhý stránky dat se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> Rozhraní stránkování dá dál vylepšit tím, že uživatel k určení, kolik stránek zobrazíte na stránce. Například může být DropDownList přidán výpis možnosti velikost stránky jako je 5, 10, 25, 50 a všechny. Při výběru velikosti stránky, bude uživatel muset jeho přesměrování zpátky na `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Opuštění implementace toto vylepšení jako cvičení pro čtečku.


## <a name="using-custom-paging"></a>Používá vlastní stránkování

Na stránkách DataList prostřednictvím její data způsobem neefektivní výchozí stránkování. Při procházení dostatečně velké objemy dat, je nutné použít vlastní stránkování. I když podrobnosti implementace poněkud lišit, jsou Principy implementace vlastní stránkování v a v prvku DataList stejná jako u výchozího stránkování. Pomocí vlastní stránkování, `ProductBLL` třída s `GetProductsPaged` – metoda (místo `GetProductsAsPagedDataSource`). Jak je popsáno v [efektivně stránkování prostřednictvím velkých objemů dat](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) kurzu `GetProductsPaged` musí být předán počáteční řádek indexu a maximální počet vrácených řádků. Tyto parametry se dají udržovat pomocí řetězce dotazu stejně jako `pageIndex` a `pageSize` parametrů použitých ve výchozích stránkování.

Od existovat s žádné `PagedDataSource` s vlastní stránkování, musí použít alternativní postupy k určení celkový počet záznamů stránkování prostřednictvím a zda jsme opětovné zobrazení na první nebo poslední stránku data. `TotalNumberOfProducts()` Metoda ve `ProductsBLL` třídy vrátí celkový počet stránkování prostřednictvím produktů. Pokud chcete zjistit, pokud je první stránka data zobrazení, zkontrolujte start index řádku Pokud je nula, pak zobrazení první stránka. Pokud start index řádku a maximální počet vrácených řádků je větší než nebo rovna hodnotě celkový počet záznamů stránkování prostřednictvím je poslední stránky zobrazení.

Ukážeme si některé implementace vlastní stránkování podrobněji v dalším kurzu.

## <a name="summary"></a>Souhrn

Zatímco DataList ani Repeater vzdálený nabízí podporu stránkování v prvku GridView, DetailsView, nalezen a FormView řídí, tyto funkce přidané s minimálním úsilím. Nejjednodušší způsob, jak implementovat výchozí stránkování je zabalit celou sadu produkty v rámci `PagedDataSource` a pak vytvoříte vazbu `PagedDataSource` DataList nebo Repeater. V tomto kurzu jsme přidali `GetProductsAsPagedDataSource` metodu `ProductsBLL` třídy se vraťte `PagedDataSource`. `ProductsBLL` Třída již obsahuje metody, potřebné pro vlastní stránkování `GetProductsPaged` a `TotalNumberOfProducts`.

Spolu s načítání buď přesnou sadu záznamů, které má být zobrazen pro vlastní stránkování, nebo všechny záznamy v `PagedDataSource` výchozí stránkování, musíme také ručně přidejte stránkovací rozhraní. Pro účely tohoto kurzu jsme vytvořili další, předchozí, nejprve poslední rozhraní s čtyři tlačítka webové ovládací prvky. Kromě toho byl přidán ovládací prvek popisku zobrazuje aktuální číslo stránky a celkový počet stránek.

V dalším kurzu uvidíme, jak přidat podporu řazení ovládacích prvků DataList a Repeater. Také uvidíme vytvoření ovládacích prvků DataList, který může být stránkovaného fondu a seřazené (s příklady použití výchozí a vlastní stránkování).

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Liz Shulok, Ken Pespisa a Bernadette Leigh. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](sorting-data-in-a-datalist-or-repeater-control-cs.md)
