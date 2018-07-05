---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Stránkování a řazení sestavy dat (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. V tomto kurzu provedeme první pohled na přidání, řazení a...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 75a2206bf3db3af8859fe4de58f67135d31bba0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401919"
---
<a name="paging-and-sorting-report-data-c"></a>Stránkování a řazení dat sestavy (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) nebo [stahovat PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. V tomto kurzu provedeme první pohled na přidání řazení a stránkování naší sestavy, které jsme se pak stavět v budoucích kurzech.


## <a name="introduction"></a>Úvod

Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. Například při vyhledávání pro knihy technologie ASP.NET v online knihkupectví, může být stovky těchto knihy, ale v sestavě Výpis výsledky hledání jsou uvedeny pouze deset shody na stránku. Navíc můžete výsledky seřadit podle názvu, ceny, počet stránek, jméno autora a tak dále. Během posledních 23 kurzy zkoumat, jak vytvářet řadu sestav, včetně rozhraní, které umožňují přidávat, upravovat a odstraňovat data, jsme není podívali se na tom, jak řadit data a pouze ve stránkování příklady jsme viděli ve byly DetailsView a FormView ovládací prvky.

V tomto kurzu uvidíme, jak přidat řazení a stránkování naší sestavy, které se dá udělat jednoduše kontrola několik zaškrtávacích políček. Bohužel této zjednodušenou implementaci má jejích nevýhod opouští chvíli se požadované rozhraní řazení a stránkování rutin nejsou určeny pro účinné stránkování velkých výsledné sady. Další kurzy se podívejte se, jak k překonání omezení out-of-the-box stránkování a řazení řešení.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Přidání stránkování a řazení kurz webových stránek

Než začneme v tomto kurzu, umožní s nejdřív využít pro přidání stránek ASP.NET, které potřebujeme pro tento kurz a další tři. Začněte tím, že vytvoříte novou složku v projektu s názvem `PagingAndSorting`. Dále přidejte následující pět stránek ASP.NET do této složky s všechny z nich nakonfigurovat tak, aby na hlavní stránce `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Vytvořte složku PagingAndSorting a přidávání stránek kurz ASP.NET](paging-and-sorting-report-data-cs/_static/image1.png)

**Obrázek 1**: Vytvořte složku PagingAndSorting a přidávání stránek kurz ASP.NET


Dále otevřete `Default.aspx` stránku a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku od `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-cs.md) kurzu mapy webu a zobrazí výčet tyto kurzy v aktuálním oddílu v seznamu s odrážkami.


![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](paging-and-sorting-report-data-cs/_static/image2.png)

**Obrázek 2**: Přidat SectionLevelTutorialListing.ascx uživatelský ovládací prvek na Default.aspx


Abyste měli zobrazení stránkování a řazení kurzů, které jsme vám vytvoření seznamu s odrážkami, potřebujeme přidat je do mapy webu. Otevřít `Web.sitemap` soubor a přidejte následující kód za úpravy, vložení a odstranění uzlu značky mapy webu:


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Krok 2: Zobrazení informací o produktu v GridView

Předtím, než jsme skutečně implementujte stránkování a řazení možnosti, umožní s prvním vytvoření standardní bez srotable,-nestránkované prvku GridView, který obsahuje informace o produktu. Toto je úloha jsme ve provést v mnoha případech před v celé této sérii kurzů tak tyto kroky by měla být obeznámeni. Začněte otevřením `SimplePagingSorting.aspx` stránku a přetáhněte ovládací prvek GridView z panelu nástrojů do Návrháře nastavení jeho `ID` vlastnost `Products`. Dále vytvořte nový prvek ObjectDataSource, který používá třídu ProductsBLL s `GetProducts()` metody, která vrátí všechny informace o produktu.


![Načíst informace o všech produktů pomocí GetProducts() – metoda](paging-and-sorting-report-data-cs/_static/image4.png)

**Obrázek 4**: načtení informací o všech produktů pomocí GetProducts() – metoda


Vzhledem k tomu, že tato sestava už je jen pro čtení sestavy, neexistuje s, stačí namapovat ObjectDataSource s `Insert()`, `Update()`, nebo `Delete()` metody odpovídající `ProductsBLL` metody; proto zvolte (žádný) z rozevíracího seznamu pro příkaz INSERT, UPDATE a odstranění karty.


![Zvolte možnost (žádné) možnost v rozevíracím seznamu v UPDATE, INSERT a odstranit záložky](paging-and-sorting-report-data-cs/_static/image5.png)

**Obrázek 5**: Zvolte možnost (žádné) možnost v rozevíracím seznamu v UPDATE, INSERT a odstranit záložky


V dalším kroku umožní s přizpůsobit pole s GridView tak, aby se zobrazují pouze názvy produktů, Dodavatelé, kategorie, ceny a ukončená stavy. Kromě toho teď můžete provádět žádné formátování na úrovni pole změní, jako je například nastavení `HeaderText` vlastnosti nebo formátování ceny jako měnu. Po provedení těchto změn vašeho ovládacího prvku GridView s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Obrázek 6 doposud zobrazuje náš postup při prohlížení prostřednictvím prohlížeče. Všimněte si, že na stránce jsou uvedeny všechny produkty na jedné obrazovce zobrazuje každý produkt s názvem, kategorie, Dodavatel, ceny a vyřazuje stav.


[![Každý produkt patří](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Obrázek 6**: každý produkt patří ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Krok 3: Přidání podpory stránkování

Výpis *všechny* produktů na jednu obrazovku může mít za následek přetížení informace pro uživatele perusing data. Abyste se mohli lépe zvládnutelné výsledky, jsme data do menších stránek data rozdělte a umožní uživateli procházení jednu stránku dat najednou. Provedete to stačí zaškrtnout políčko Povolit stránkování v prvku GridView s inteligentním (tím se nastaví prvek GridView s [ `AllowPaging` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) k `true`).


[![Zaškrtnutím políčka Povolit stránkování přidat podporu stránkování](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Obrázek 7**: Zaškrtněte políčko Povolit stránkování na přidání podpory stránkování ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image11.png))


Povolení stránkování omezuje počet zobrazených na stránce záznamy a přidá *stránkovací rozhraní* do prvku GridView. Výchozí rozhraní stránkování, je znázorněno na obrázku 7, je řada čísla stránek, které uživateli umožňují rychle přejít z jedné stránky dat do jiného. Toto rozhraní stránkování by měla vypadat povědomě, jako jsme ve viděli při přidávání podpory stránkování pro ovládací prvky prvku DetailsView a FormView v posledních kurzy.

Prvek DetailsView i FormView ovládací prvky zobrazují jenom jeden záznam na stránce. Prvku GridView, ale consults jeho [ `PageSize` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) k určení, kolik záznamů se má zobrazit na stránce (výchozí hodnota této vlastnosti hodnotu 10).

Rozhraní tohoto ovládacího prvku GridView, DetailsView a FormView stránkování s je možné přizpůsobit pomocí následujících vlastností:

- `PagerStyle` Určuje informace o rozhraní stránkování; stylu můžete zadat nastavení, jako jsou `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, a tak dále.
- `PagerSettings` obsahuje bevy vlastností, které můžete přizpůsobit funkce rozhraní stránkování; `PageButtonCount` označuje maximální počet zobrazuje rozhraní stránkování (výchozí hodnota je 10) čísla číselné stránek; [ `Mode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) označuje, jak rozhraní stránkování funguje a je možné nastavit: 

    - `NextPrevious` Zobrazí se další a předchozí tlačítka, které uživateli umožňují krok dopředu nebo dozadu jedné stránky v čase
    - `NextPreviousFirstLast` kromě tlačítka Další a předchozí první a poslední tlačítka jsou taky součástí, které uživateli umožňují rychle přejít na první nebo poslední stránku dat
    - `Numeric` Zobrazuje sérii čísla stránek, které uživateli umožňují okamžitě přejít na libovolnou stránku
    - `NumericFirstLast` Kromě číslo stránky obsahuje jméno a příjmení tlačítka, které uživateli umožňují rychle přejít na první nebo poslední stránku dat; na první nebo poslední tlačítka se zobrazují pouze pokud se nemůže vejít všechna čísla číselné stránky

Kromě toho ovládacího prvku GridView, DetailsView a FormView všechny nabídky `PageIndex` a `PageCount` vlastnosti, které označují aktuální stránku zobrazení a celkový počet stránek, v uvedeném pořadí. `PageIndex` Vlastnost se indexuje zpětně začínajícím hodnotou 0, to znamená, že při prohlížení na první stránku dat `PageIndex` se bude rovnat 0. `PageCount`, na druhé straně spustí počítání na 1, to znamená, že `PageIndex` je omezený na hodnoty mezi 0 a `PageCount - 1`.

Umožní s využít ke zlepšení našich rozhraní stránkování prvku GridView s výchozí vzhled. Konkrétně umožní s stránkovací rozhraní, zarovnání vpravo světla šedém pozadí. Namísto nastavení těchto vlastností přímo prostřednictvím GridView s `PagerStyle` vlastností, let s vytvořit třídu šablony stylů CSS v `Styles.css` s názvem `PagerRowStyle` a pak mu přiřaďte `PagerStyle` s `CssClass` vlastnosti prostřednictvím našich motiv. Začněte otevřením `Styles.css` a přidat následující šablony stylů CSS definici třídy:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Dále otevřete `GridView.skin` soubor `DataWebControls` složky v rámci `App_Themes` složky. Jak jsme probírali v *stránky předlohy a navigace na webu* výukový program, volitelných vzhledů soubory je možné zadat výchozí hodnoty vlastností pro ovládací prvek webového. Proto se rozšířit stávající nastavení, chcete-li zahrnout nastavení `PagerStyle` s `CssClass` vlastnost `PagerRowStyle`. Navíc umožňují s nakonfigurujte rozhraní stránkování zobrazit maximálně pět číselné stránky tlačítka pomocí `NumericFirstLast` rozhraní stránkování.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Činnost koncového uživatele stránkování

Obrázek 8 ukazuje webové stránky, když uživatel prostřednictvím prohlížeče, poté, co bylo zaškrtnuto políčko Povolit stránkování prvku GridView s a `PagerStyle` a `PagerSettings` konfigurace byly provedeny prostřednictvím `GridView.skin` souboru. Poznámka: jak pouze deset záznamy jsou zobrazeny, a stránkovací rozhraní označuje, že jsme se zobrazuje na první stránku.


[![S povoleno stránkování se zobrazují pouze podmnožinu záznamů najednou](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Obrázek 8**: S povoleno stránkování, se zobrazí pouze podmnožinu záznamů najednou ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image14.png))


Když uživatel klikne na jednu stránku čísel v rozhraní stránkování, vyplývá zpětné volání a stránku znovu načte zobrazující, že požadovaná stránka s záznamy. Obrázek 9 ukazuje výsledky po aktivaci ochrany a zobrazit finální stránku data. Všimněte si, že poslední stránka má jenom jeden záznam. je to proto, že existují záznamy 81 celkem, což vede k osm stránek 10 záznamů na stránku a jednu stránku s jedinou záznam.


[![Kliknutím na číslo stránky vyvolá zpětné volání a ukazuje na příslušnou podmnožinu záznamů](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Obrázek 9**: Kliknutím na číslo stránky vyvolá zpětné volání a příslušné dílčí záznamy ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Stránkování na straně serveru s pracovního postupu

Když koncový uživatel klikne na tlačítko v rozhraní stránkování, vyplývá zpětné volání a začne následující pracovní postup na straně serveru:

1. GridView s (nebo prvku DetailsView nebo FormView) `PageIndexChanging` dojde k aktivaci události
2. Prvku ObjectDataSource znovu požadavků *všechny* dat z knihoven BLL; GridView s `PageIndex` a `PageSize` hodnoty vlastností se používají k určení záznamů vrácených z BLL potřebovat zobrazeného v prvku GridView.
3. GridView s `PageIndexChanged` dojde k aktivaci události

V kroku 2 ObjectDataSource znovu požadavků, všechna data z jeho datového zdroje. Tento styl stránkování se obvykle označuje jako *výchozího stránkování*, protože s chování stránkování používá ve výchozím nastavení při nastavení `AllowPaging` vlastnost `true`. S výchozí naively stránkování dat webový ovládací prvek načte všechny záznamy pro každou stránku dat, i když jsou pouze podmnožinu záznamů ve skutečnosti vykresleny do kódu HTML tuto s odesláno prohlížeči. Pokud databáze je uložen v mezipaměti BLL nebo prvek ObjectDataSource, je výchozí stránkování nefunkčním dostatečně velké množství výsledků nebo pro webové aplikace s více souběžných uživatelů.

V dalším kurzu prozkoumáme implementace *vlastní stránkování*. S vlastní stránkování můžete konkrétně dát pokyn prvek ObjectDataSource, aby načetl pouze přesné sadu záznamů, které jsou potřebné pro požadovanou stránku data. Jak si dokážete představit, vlastní stránkování výrazně zvyšuje efektivitu stránkování prostřednictvím velké množství výsledků.

> [!NOTE]
> Zatímco výchozí stránkování není vhodná, při stránkování prostřednictvím dostatečně velké množství výsledků nebo pro weby s mnoha souběžných uživatelů, uvědomte si, že vlastní stránkování vyžaduje další změny a úsilí k implementaci a není pouhým zaškrtnutím políčka (což je výchozí stránkování). Proto výchozí stránkování může být ideální volbou pro weby s nízkým provozem, malé nebo když stránkování prostřednictvím výsledku poměrně málo početnému nastaví, jak s mnohem jednodušší a rychlejší k implementaci.


Například pokud víme, že nikdy jsme si více než 100 produktů v naší databázi, minimální výkonový zisk plynoucí jako vlastní stránkování je pravděpodobně posunut úsilí nutné k jeho implementaci. Pokud však může jeden den máme tisíců nebo desítek tisíců produktů, *není* implementace vlastní stránkování by nebránily výrazně škálovatelnost naši aplikaci.

## <a name="step-4-customizing-the-paging-experience"></a>Krok 4: Kustomizace možností stránkování

Webové ovládací prvky dat zadejte počet vlastností, které slouží k zajištění lepších možností stránkování uživatele s. `PageCount` Vlastnosti, například označuje, kolik existuje celkový počet stránek se, zatímco `PageIndex` vlastnost označuje aktuální se navštívené stránky a je možné nastavit, jak rychle přesunout uživatel na určitou stránku. Si ukážeme, jak pomocí těchto vlastností vylepšují činnost koncového uživatele s stránkování, nechat přidat popisek s webové ovládací prvek na stránku, která informuje uživatele, které stránky se znovu navštívit aktuálně spolu s DropDownList ovládací prvek, který umožňuje jim rychle přejít na stránku .

Nejprve přidejte ovládací prvek popisek Web na stránku, nastavte jeho `ID` vlastnost `PagingInformation`a vymažte její `Text` vlastnost. Dále vytvořte obslužnou rutinu události pro prvek GridView s `DataBound` událostí a přidejte následující kód:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Přiřadí tuto obslužnou rutinu události `PagingInformation` popisek s `Text` vlastnost zprávu informující uživatele na stránce nyní navštěvují `Products.PageIndex + 1` mimo celkový počet stránek `Products.PageCount` (přidáme 1 `Products.PageIndex` vlastnost protože `PageIndex` se indexuje zpětně počínaje 0). Volba přiřadit tento popisek s `Text` vlastnost v `DataBound` obslužná rutina události, nikoli `PageIndexChanged` obslužné rutiny události protože `DataBound` událost aktivuje se pokaždé, když vázaná na prvku GridView. vzhledem k tomu `PageIndexChanged` pouze obslužné rutiny události je aktivována, když se změní stránka indexu. Pokud prvku GridView je zpočátku data vázaná na první stránce navštíví, `PageIndexChanging` fire události kódu t (vzhledem k tomu `DataBound` událostí nemá).

Uveďte uživatel se teď zobrazují zprávu s oznámením, jaké stránky navštěvují a kolik celkový počet stránek dat existuje.


[![Aktuální číslo stránky a celkový počet stránek zobrazených](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Obrázek 10**: Zobrazí se aktuální číslo stránky a celkový počet stránek ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image20.png))


Kromě ovládacího prvku popisku umožní s také přidat ovládací prvek DropDownList, který obsahuje číslo stránky v prvku GridView s aktuálně zobrazené stránce vybrali. Zde spočívá, že uživatel můžete rychle přejít na aktuální stránce do jiného že jednoduše vyberete nový index stránky z DropDownList. Začněte přidáním DropDownList do Návrháře nastavení jeho `ID` vlastnost `PageList` a zaškrtnete tuto možnost povolit vlastnost AutoPostBack z jeho inteligentních značek.

Pak se vraťte do `DataBound` obslužné rutiny události a přidejte následující kód:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Tento kód začne tím, že zrušíte položek v `PageList` DropDownList. Toto se může zdát nadbytečný, protože jeden t wouldn očekávat počet stránek, chcete-li změnit, ale ostatním uživatelům může být současně pomocí systému, přidání nebo odebrání záznamy ze `Products` tabulky. Počet stránek dat může změnit odpovídající vložení nebo odstranění.

V dalším kroku budeme potřebovat znovu vytvořit číslo stránky a ten, který se mapuje na aktuální GridView `PageIndex` ve výchozím nastavení vybrané. Můžeme to provést pomocí smyčky od 0 do `PageCount - 1`, přidání nového `ListItem` v každé iterace a nastavení jeho `Selected` vlastnost na hodnotu true, pokud je aktuální index iterace GridView s `PageIndex` vlastnost.

Nakonec musíme vytvořit obslužnou rutinu události pro DropDownList s `SelectedIndexChanged` událost, která se spustí pokaždé, když uživatel vybrat jinou položku ze seznamu. K vytvoření této obslužné rutiny události, jednoduše dvakrát klikněte na rozevírací seznam v návrháři a potom přidejte následující kód:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Jak ukazuje obrázek 11 pouze změna GridView s `PageIndex` vlastnosti způsobí, že data, která mají být znovu připojeno k prvku GridView. V prvku GridView s `DataBound` obslužná rutina události, odpovídající DropDownList `ListItem` zaškrtnuto.


[![Uživatel je automaticky Přesměrujeme do šestého stránky při výběru položky seznamu stránky 6 rozevíracího seznamu](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Obrázek 11**: uživatele je automaticky Přesměrujeme do šestého stránky při výběru položky seznamu stránky 6 rozevíracího seznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Krok 5: Přidání obousměrná podpora třídění

Přidání obousměrná podpora třídění je stejně jednoduché jako přidání podpory stránkování stačí zaškrtnout možnost Povolit řazení z ovládacího prvku GridView s inteligentním (které nastaví prvek GridView s [ `AllowSorting` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) k `true`). Tím zkopírujete všech záhlaví pole s GridView jako LinkButtons, při kliknutí na vyvolávají zpětné odeslání a vrátit data seřazená podle kliknutí na sloupce ve vzestupném pořadí. Klepnutím na stejné záhlaví odkazem (LinkButton) opakujte znovu seřadí data v sestupném pořadí.

> [!NOTE]
> Pokud používáte vlastní vrstvy přístupu k datům, nikoli zadané datové sady, nemusíte mít možnost Povolit řazení v prvku GridView s inteligentním. Pouze prvků GridViews vázán ke zdrojům dat, které nativně podporují řazení mít toto zaškrtávací políčko k dispozici. Datové sadě zadán poskytuje podporu řazení out-of-the-box, protože poskytuje ADO.NET DataTable `Sort` metoda, která při vyvolání, seřadí s DataTable DataRows pomocí na zadaných kritériích.


Pokud vaše DAL nevrací objekty, které nativně podporují řazení, budete muset nakonfigurovat ObjectDataSource k předávání informací řazení do vrstvy obchodní logiky, která lze řadit data nebo mít data seřadit vrstvou DAL. Podíváme, jak řadit data na obchodní logiku a vrstvy přístupu k datům v budoucích kurzech.

Řazení LinkButtons jsou vykresleny jako hypertextové odkazy HTML, jehož aktuální barvy (pro nenavštívených odkazů a tmavě červenou barvu pro navštívený odkaz modrá) nebudou kolidovat s barvu pozadí záhlaví řádku. Místo toho vám umožňují s mají všechny odkazy řádek záhlaví zobrazí bílé, a to bez ohledu na to, zda jsou ve byla navštívili, nebo ne. To lze provést přidáním následujícího `Styles.css` třídy:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Tato syntaxe označuje použít bílý text při zobrazení na hypertextové odkazy v rámci elementu, který používá třídu HeaderStyle.

Po přidání této šablony stylů CSS při návštěvě stránky prostřednictvím prohlížeče vaše obrazovka by měla vypadat podobně jako obrázek 12. Zejména obrázek 12 znázorňuje výsledky po kliknutí na odkaz cena pole s záhlaví.


[![Výsledky seřazeny podle UnitPrice ve vzestupném pořadí](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Obrázek 12**: The výsledky byly seřazeny podle UnitPrice ve vzestupném pořadí ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Zkoumání řazení pracovního postupu

Všechny GridView pole Vlastnost BoundField, třídě CheckBoxField, TemplateField a mít tak dále `SortExpression` vlastnost, která určuje výraz, který se má použít k řazení dat. při kliknutí na tuto pole s řazení odkaz záhlaví. Má také prvku GridView `SortExpression` vlastnost. Při řazení odkazem (LinkButton) dojde ke kliknutí na záhlaví prvku GridView přiřadí tohoto pole s `SortExpression` hodnota, která se jeho `SortExpression` vlastnost. V dalším kroku se data z ObjectDataSource znovu načíst a seřazených podle GridView s `SortExpression` vlastnost. Následujícím seznamu jsou uvedeny, která se ukáže, když koncový uživatel seřadí data v GridView posloupnost kroků:

1. GridView s [události řazení](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) aktivována
2. GridView s [ `SortExpression` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) je nastavena na `SortExpression` pole jejichž řazení záhlaví došlo ke kliknutí na odkazem (LinkButton)
3. Prvku ObjectDataSource znovu načte všechna data z knihoven BLL a pak seřadí data s využitím GridView s `SortExpression`
4. GridView s `PageIndex` vlastnost nastaven na hodnotu 0, to znamená, že při řazení uživatele se vrátí na první stránku dat (za předpokladu, že byl implementován podpora stránkování)
5. GridView s [ `Sorted` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) aktivována

Jako s výchozí stránkování, výchozí řazení možnost znovu načte *všechny* záznamy z BLL. Při použití řazení bez stránkovací nebo při použití řazení s výchozí stránkování, tam s žádný způsob, jak obejít tato výkon přenosů (nemá databáze dat do mezipaměti). Nicméně, jak uvidíme v budoucích kurzech, ho s umožňuje efektivně řazení dat při použití vlastní stránkování.

Při vytváření vazby prvku ObjectDataSource do prvku GridView. pomocí rozevíracího seznamu v prvku GridView s inteligentním, automaticky každé pole ovládacího prvku GridView má jeho `SortExpression` přiřazená název datového pole v vlastnost `ProductsRow` třídy. Například `ProductName` Vlastnost BoundField s `SortExpression` je nastavena na `ProductName`, jak je znázorněno v následující kód:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Pole lze nastavit tak, aby se s tím, že zrušíte řazení jeho `SortExpression` vlastnosti (ji přiřadíte prázdný řetězec). Pro znázornění, představte si, že jsme kód nefungoval t bychom, aby naši zákazníci řadit cena naše produkty. `UnitPrice` Vlastnost BoundField s `SortExpression` vlastnost je možné odebrat z deklarativní nebo prostřednictvím pole dialogových oken (která je přístupná po kliknutí na odkaz Upravit sloupce v prvku GridView s inteligentním).


![Výsledky seřazeny podle UnitPrice ve vzestupném pořadí](paging-and-sorting-report-data-cs/_static/image27.png)

**Obrázek 13**: výsledky seřazeny podle UnitPrice ve vzestupném pořadí


Jednou `SortExpression` se odebrala vlastnost `UnitPrice` Vlastnost BoundField, záhlaví se vykreslí jako text, nikoli jako odkaz, a tím brání uživatelům v řazení dat podle ceny.


[![Odebráním vlastnost SortExpression mohou uživatelé řadit už produkty podle ceny](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Obrázek 14**: odebráním vlastnost SortExpression mohou uživatelé řadit už produkty podle cena ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Programově řazení prvku GridView.

Můžete také řadit obsah prvku GridView prostřednictvím kódu programu pomocí ovládacího prvku GridView s [ `Sort` metoda](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Stačí předat `SortExpression` hodnoty seřadíte položky podle spolu s [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` nebo `Descending`), a GridView s data budou znovu seřazené.

Představte si, že z důvodu jsme vypnuto řazení podle `UnitPrice` byl, protože se nám obavy, naši zákazníci by jednoduše koupit jenom produkty nejnižší cenou. Chceme povzbudit je nákup nejdražší produktů, takže jsme d, jako je mohly připojovat k seřazení produktů podle ceny, ale jenom na základě nejdražší ceny nejméně.

K provedení to přidání ovládacího prvku tlačítko Web na stránku, nastavte jeho `ID` vlastnost `SortPriceDescending`a jeho `Text` vlastnost řazení podle ceny. Dále vytvořte obslužnou rutinu události pro tlačítko s `Click` události dvojitým kliknutím ovládacího prvku tlačítko v návrháři. Přidejte následující kód do této obslužné rutiny události:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Kliknutím na toto tlačítko vrátí uživatele na první stránku s produkty, seřazené podle cenu z nejdražší k nejlevnější (viz obrázek 15).


[![Kliknutím na tlačítko Orders produkty z nejnákladnější nejméně](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Obrázek 15**: Kliknutím na tlačítko objednávky produktů z the nejnákladnější nejméně ([kliknutím ji zobrazíte obrázek v plné velikosti](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak implementovat výchozí stránkování a řazení možnosti které byly stejně jednoduše jako si zaškrtnutím políčka! Když uživatel seřadí nebo stránky pomocí dat, kterou se podobá pracovního postupu:

1. Vyplývá zpětné volání
2. Data webový ovládací prvek s předem úroveň dojde k aktivaci události (`PageIndexChanging` nebo `Sorting`)
3. Všechna data znovu načte prvku ObjectDataSource
4. Data webový ovládací prvek s po úroveň dojde k aktivaci události (`PageIndexChanged` nebo `Sorted`)

Při implementaci základní stránkování a řazení podrobným, musí využívat mnohem efektivnější vlastní stránkování nebo k dalšímu vylepšení rozhraní stránkování a řazení definovanými další úsilí. Tato témata se prozkoumat budoucích kurzech.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](efficiently-paging-through-large-amounts-of-data-cs.md)
