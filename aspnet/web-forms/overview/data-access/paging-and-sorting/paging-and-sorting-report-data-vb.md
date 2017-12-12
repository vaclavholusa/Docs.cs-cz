---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: "Sestava stránkování a řazení dat (VB) | Microsoft Docs"
author: rick-anderson
description: "Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. V tomto kurzu provedeme první pohled na přidání řazení a..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ef1bb0b68a46535e3320834a0374b9a4f66182c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="paging-and-sorting-report-data-vb"></a>Stránkování a řazení dat sestavy (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) nebo [stáhnout PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. V tomto kurzu provedeme první pohled na přidání řazení a stránkování pro naše sestavy, které jsme se potom stavět na kurzy v budoucnosti.


## <a name="introduction"></a>Úvod

Stránkování a řazení jsou dvě velmi běžné funkce při zobrazení dat v aplikaci online. Například při hledání ASP.NET v online knihkupectví objednáváte knihy, může být stovky takové knihy, ale v sestavě Výpis výsledky hledání jsou uvedeny pouze deset odpovídá na stránce. Kromě toho můžete výsledky seřadit podle název, ceny, počet stránek, jméno autora a tak dále. Při minulosti 23 kurzy zkoumat, jak vytvářet řadu sestav, včetně rozhraní, které umožňují přidávat, upravovat a odstraňovat data, jsme sunout není podívat, jak řadit data a jediným stránkování příklady jsme viděli jste byli s DetailsView a FormView ovládací prvky.

V tomto kurzu jsme zobrazí, jak přidat řazení a stránkování pro naše sestavy, které lze provést jednoduše zaškrtnutím políček pár. Bohužel tato zneužívající vlastností prohlížeče implementace má jeho nevýhody opouští chvíli k být potřeby rozhraní řazení a stránkování rutiny nejsou navrženy pro efektivní stránkování prostřednictvím velké množství výsledků. Budoucí kurzy bude prozkoumejte jak překonat omezení out-of-the-box stránkování a řazení řešení.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Přidání stránkování a řazení kurz webové stránky

Než začneme v tomto kurzu, umožní s nejprve přidat na stránky ASP.NET, které budeme potřebovat pro tento kurz a další tři chvíli trvat. Začněte vytvořením novou složku v projektu s názvem `PagingAndSorting`. Dál přidejte následující pět stránek ASP.NET do této složky rozmístit nakonfigurované na používání stránky předlohy `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Vytvořte složku PagingAndSorting a přidání stránky kurz ASP.NET](paging-and-sorting-report-data-vb/_static/image1.png)

**Obrázek 1**: Vytvořte složku PagingAndSorting a přidání stránky kurz ASP.NET


Dále otevřete `Default.aspx` stránky a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek z `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [hlavní stránky a webové navigace](../introduction/master-pages-and-site-navigation-vb.md) kurzu mapy webu a zobrazí výčet tyto kurzy v aktuálním oddílu v seznamu s odrážkami.


![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Obrázek 2**: Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx


Aby bylo možné používat seznamu s odrážkami zobrazení stránkování a řazení kurzů, které jsme budete vytvářet, je potřeba přidat je do mapy webu. Otevřete `Web.sitemap` souboru a přidejte následující kód po úpravy, vkládání a odstraňování uzlu značky mapy lokality:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Krok 2: Zobrazení informací o produktu v GridView

Předtím, než ve skutečnosti implementaci stránkování a možnosti řazení, umožní s nejprve vytvořit standardní jiný srotable, nestránkovatelné GridView, které jsou uvedeny informace o produktu. Toto je úloha jsme sunout mnohokrát před provést v rámci tohoto kurzu řady proto tyto kroky měli seznámit. Začněte otevřením `SimplePagingSorting.aspx` stránky a přetáhněte ovládací prvek GridView z panelu nástrojů na návrháře nastavení jeho `ID` vlastnost `Products`. Dál vytvořte novou ObjectDataSource, který používá třídu ProductsBLL s `GetProducts()` metody, která vrátí všechny informace o produktu.


![Načtení informací o všechny produkty pomocí metody GetProducts()](paging-and-sorting-report-data-vb/_static/image4.png)

**Obrázek 4**: načtení informací o všechny produkty pomocí metody GetProducts()


Vzhledem k tomu, že tato sestava je jen pro čtení sestavy, že s již nutné namapovat ObjectDataSource s `Insert()`, `Update()`, nebo `Delete()` metody pro odpovídající `ProductsBLL` metody; proto (None) vyberte z rozevíracího seznamu pro aktualizaci, vložení, a odstranění karty.


![Zvolte (None) možnost v rozevíracím seznamu v aktualizaci UPDATE, INSERT a odstranit karty](paging-and-sorting-report-data-vb/_static/image5.png)

**Obrázek 5**: Zvolte (None) možnost v rozevíracím seznamu v aktualizaci UPDATE, INSERT a odstranit karty


V dalším kroku umožní s přizpůsobit pole s GridView tak, aby se zobrazují pouze názvy produktů, dodavatelů, kategorie, ceny a stavy, které jsou – starší formáty. Kromě toho chování volné aby veškeré formátování úrovni poli změní, jako je například úprava `HeaderText` vlastnosti nebo formátování cena jako měny. Po provedení těchto změn vaše GridView s deklarativní by měl vypadat takto:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Obrázek 6 zobrazuje naše průběh doposud, když zobrazit pomocí prohlížeče. Upozorňujeme, že stránky zobrazí všechny produkty v jednu obrazovku, zobrazující každý produkt s název, kategorie, dodavatele, ceny a zastaví stavu.


[![Každý z nich jsou uvedeny](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Obrázek 6**: jednotlivých produktů, jsou uvedeny ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Krok 3: Přidání podpory stránkování

Výpis *všechny* produktů na jednu obrazovku, může dojít k přetížení informace pro uživatele perusing data. Pomáhat udržitelnější výsledky, jsme můžete rozdělit data do menší stránky dat a umožnit uživatelům procházet jednu stránku dat najednou. Provedete to stačí zaškrtnout políčko Povolit stránkování z GridView s inteligentním (nastaví Tato rutina GridView s [ `AllowPaging` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) k `true`).


[![Zaškrtnutím políčka Povolit stránkování pro přidání podpory stránkování](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Obrázek 7**: Zaškrtněte políčko Povolit stránkování přidání podpory stránkování ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image11.png))


Povolení stránkování omezuje počet záznamů zobrazených na stránce a přidá *rozhraní stránkování* k GridView. Výchozí rozhraní stránkování, vidět na obrázku 7, je řada čísel stránek, které uživateli umožňují rychle přecházejí z jedné stránky dat na jiný. Toto rozhraní stránkování by měla vypadat povědomě, jako jsme jste ji viděli při přidání podpory stránkování pro ovládací prvky DetailsView a FormView v posledních kurzy.

Ovládací prvky DetailsView i FormView zobrazit pouze jeden záznam na stránce. Rutina GridView, ale zajímají jeho [ `PageSize` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.pagesize.aspx) k určení, kolik zaznamenává zobrazíte na stránce (Tato vlastnost má výchozí hodnotu 10).

Tato rutina GridView DetailsView a FormView rozhraní stránkování s lze přizpůsobit pomocí následujících vlastností:

- `PagerStyle`Určuje styl informace o rozhraní stránkování; můžete zadat nastavení jako `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`a tak dále.
- `PagerSettings`obsahuje bevy vlastností, které můžete přizpůsobit funkce rozhraní stránkování; `PageButtonCount` určuje maximální počet v stránkování rozhraní (výchozí hodnota je 10) čísla číselné stránek; [ `Mode` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.pagersettings.mode.aspx) Určuje, jak funguje rozhraní stránkování a může být nastaven na: 

    - `NextPrevious`Zobrazí se další a předchozí tlačítka, které uživateli umožňují krok dopředný nebo zpětné jednu stránku současně
    - `NextPreviousFirstLast`kromě tlačítka Další a předchozí první a poslední tlačítka jsou taky součástí, které uživateli umožňují rychle přesunout na první nebo poslední stránku dat
    - `Numeric`Zobrazuje sérii čísel stránek, které uživateli umožňují okamžitě přejít na libovolnou stránku
    - `NumericFirstLast`Kromě číslo stránky obsahuje první a poslední tlačítka, které uživateli umožňují rychle přesunout na první nebo poslední stránku dat; tlačítka prvním či posledním se zobrazují pouze, pokud se nemůže vejít všechna čísla číselné stránky

Kromě toho GridView, DetailsView a FormView všech nabízet `PageIndex` a `PageCount` vlastnosti, které označují aktuální stránky zobrazení a celkový počet stránek dat, v uvedeném pořadí. `PageIndex` Vlastnost je indexovaný začínající na 0, znamená to, že při prohlížení na první stránku dat `PageIndex` se rovnat 0. `PageCount`, na druhé straně spustí počítání v 1, znamená to, že `PageIndex` je omezená na hodnoty mezi 0 a `PageCount - 1`.

Umožní s za chvíli ke zlepšení výchozí vzhled naše rozhraní GridView s stránkování. Konkrétně umožní s mít rozhraní stránkování vpravo zarovnaný barvou pozadí světla. Místo nastavení těchto vlastností přímo přes GridView s `PagerStyle` vlastnost, umožňují s vytvořit třídu CSS v `Styles.css` s názvem `PagerRowStyle` a pak mu přiřaďte `PagerStyle` s `CssClass` vlastnosti prostřednictvím našich motivu. Začněte otevřením `Styles.css` a přidání následujících šablon stylů CSS definici třídy:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Dále otevřete `GridView.skin` v soubor `DataWebControls` složky v rámci `App_Themes` složky. Jak již bylo zmíněno *hlavní stránky a webové navigace* kurz, kůže soubory lze použít k určení výchozí hodnoty vlastností pro ovládací prvek webu. Proto posílení stávající nastavení zahrnout nastavení `PagerStyle` s `CssClass` vlastnost `PagerRowStyle`. Navíc umožňují s nakonfigurovat rozhraní stránkování zobrazit maximálně pět číselné stránky tlačítka, pomocí `NumericFirstLast` rozhraní stránkování.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Činnost koncového uživatele stránkování

Obrázek 8 ukazuje webové stránky, když navštívili prostřednictvím prohlížeče po byl vrácen políčka Povolit stránkování s GridView a `PagerStyle` a `PagerSettings` provedené konfigurace `GridView.skin` souboru. Poznámka: jak pouze deset záznamy se zobrazují, a rozhraní stránkování označuje, že jsme prohlížíte na první stránku dat.


[![S stránkování povoleno je zobrazena pouze podmnožina záznamy současně](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Obrázek 8**: S povoleno stránkování, jsou zobrazena pouze podmnožina záznamy v čase ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image14.png))


Když uživatel klikne na jednom z čísla stránek v rozhraní stránkování, vyplývá zpětné volání a stránky znovu načte zobrazující, že požadovaná stránka s záznamy. Obrázek 9 ukazuje výsledky po výslovném zobrazíte na poslední stránku data. Všimněte si, že poslední stránka obsahuje pouze jeden záznam; je to proto, že jsou k dispozici 81 záznamy celkem, což vede k osm stránek 10 záznamů na stránku a jednu stránku s jedinou záznam.


[![Kliknutím na číslo stránky vyvolá Postback a zobrazuje příslušnou dílčí sady záznamů](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Obrázek 9**: Kliknutím na číslo stránky vyvolá Postback a zobrazuje příslušné podmnožina záznamy ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Pracovní postup serverové s stránkování

Když koncový uživatel klikne na tlačítko v rozhraní stránkování, vyplývá zpětné volání a začne následující pracovní postup na straně serveru:

1. Rutina GridView s (nebo DetailsView nebo FormView) `PageIndexChanging` aktivuje událost
2. Znovu požadavky ObjectDataSource *všechny* dat z BLL; GridView s `PageIndex` a `PageSize` hodnoty vlastností se používají k určení toho, co záznamů vrácených ze BLL potřebovat pro zobrazení v prvku GridView.
3. Rutina GridView s `PageIndexChanged` aktivuje událost

V kroku 2 ObjectDataSource znovu požadavků, všechna data z datového zdroje. Tento styl stránkování se obvykle označuje jako *stránkování výchozí*, jako je s chování stránkování používá ve výchozím nastavení při nastavování `AllowPaging` vlastnost `true`. Výchozí stránkování dat ovládací prvek webu naively načte všechny záznamy pro jednotlivé stránky dat, i když jsou pouze podmnožinu záznamů ve skutečnosti vykresluje do kódu HTML této s odesláno prohlížeči. Pokud databáze je uložen v mezipaměti BLL nebo ObjectDataSource, je výchozí stránkování nefunkčním dostatečně velké množství výsledků nebo pro webové aplikace s více souběžných uživatelů.

V dalším kurzu podíváme, jak implementovat *vlastní stránkování*. S vlastní stránkování můžete konkrétně v programu ObjectDataSource pouze načíst sadu záznamů, které jsou potřebné pro požadovanou stránku dat přesné. Jak si lze představit, vlastní stránkování výrazně zvyšuje efektivitu stránkování prostřednictvím velké množství výsledků.

> [!NOTE]
> Přestože výchozí stránkování není vhodné, pokud stránkování prostřednictvím dostatečně velké množství výsledků nebo pro lokality s velký počet současně připojených uživatelů, uvědomte si, že vlastní stránkování vyžaduje další změny a úsilí na implementaci a není stejně jednoduché jako kontroluje zaškrtávací políčko (jako je výchozí stránkování). Proto výchozí stránkování může být ideálním řešením pro malé, nízkým provozem webů nebo při stránkování prostřednictvím poměrně malý výsledků nastaví, jak s mnohem snadnější a rychlejší k implementaci.


Například pokud víme, že budete nikdy obsahuje více než 100 produkty v naší databázi, minimální výkonnější líbilo ve vlastní stránkování je pravděpodobně posunut je třeba k implementaci. Pokud však může jeden den máme tisíce nebo desetitisíce produkty, *není* implementace vlastní stránkování by výrazně omezovat škálovatelnost naší aplikace.

## <a name="step-4-customizing-the-paging-experience"></a>Krok 4: Přizpůsobení prostředí stránkování

Ovládací prvky webového data poskytují několik vlastností, které slouží k zajištění lepších možností stránkování uživatele s. `PageCount` Vlastnost, například udává, kolik existuje celkového počtu stránek, zatímco `PageIndex` vlastnost označuje aktuální se navštívené stránky a může být nastaven na rychle přesunout uživatele na konkrétní stránku. Pro ilustraci použití tyto vlastnosti k zdokonalit možnosti uživatele s stránkování, umožní přidat štítek s webové řídit k naší stránce, která informuje uživatele, které stránky se znovu aktuálně návštěvou společně s ovládací prvek rozevírací seznam, který vám umožní rychle přejít na stránku .

Nejprve přidání ovládacího prvku popisek na stránku, nastavte jeho `ID` vlastnost `PagingInformation`a vymažte její `Text` vlastnost. Dále vytvořte obslužnou rutinu události pro GridView s `DataBound` událostí a přidejte následující kód:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Přiřadí této obslužné rutiny události `PagingInformation` štítek s `Text` vlastnost zprávu informující uživatele stránce aktuálně navštěvují `Products.PageIndex + 1` mimo celkový počet stránek `Products.PageCount` (přidáme 1 `Products.PageIndex` vlastnost protože `PageIndex` je indexovaný počínaje 0). Vybrali jste přiřadit tento popisek s `Text` vlastnost v `DataBound` obslužné rutiny události naproti tomu `PageIndexChanged` obslužné rutiny události protože `DataBound` událost aktivuje se pokaždé, když data je vázána na GridView, zatímco `PageIndexChanged` pouze obslužné rutiny události Aktivuje se v případě, že se změnil index stránky. GridView po začátku data vázaná na první stránce navštěvovat, `PageIndexChanging` událostí nemá t fire (vzhledem k tomu `DataBound` událostí nemá).

Pomocí tohoto přidání uživatele se nyní zobrazí zprávu s upozorněním, jaké stránku navštěvují a kolik celkový počet stránek dat existuje.


[![Zobrazí se aktuální číslo stránky a celkový počet stránek](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Obrázek 10**: Zobrazí se aktuální číslo stránky a celkový počet stránek ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image20.png))


Kromě prvku popisek umožní s také přidat ovládací prvek rozevírací seznam, který obsahuje seznam čísel stránek v GridView s aktuálně zobrazenou stránku vybrané. Rada tady je, že uživatel rychle přejít z aktuální stránku do jiného jednoduše výběrem nové index stránky z rozevírací seznam. Začněte přidáním rozevírací seznam pro návrháře nastavení jeho `ID` vlastnost `PageList` a kontrola tuto možnost Povolit automatické odeslání z jeho inteligentních značek.

Pak se vraťte do `DataBound` obslužné rutiny události a přidejte následující kód:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Tento kód začne tím, že zrušíte se položky v `PageList` rozevírací seznam. Toto se může zdát nadbytečné, protože jeden t wouldn očekávat počet stránek, chcete-li změnit, ale ostatní uživatelé mohou pomocí systému současně, přidávat nebo odebírat záznamy ze `Products` tabulky. Počet stránek dat může změnit takové vložení nebo odstranění.

Dále je potřeba znovu vytvářet číslo stránky a ten, který se mapuje na aktuální GridView `PageIndex` ve výchozím nastavení zaškrtnuto. Jsme to provést pomocí smyčky od 0 do `PageCount - 1`, přidání nové `ListItem` v každé iteraci a nastavení jeho `Selected` vlastnost na hodnotu true, pokud se aktuální index iterace rovná GridView s `PageIndex` vlastnost.

Nakonec je potřeba vytvořit obslužnou rutinu události pro rozevírací seznam s `SelectedIndexChanged` událost, která aktivuje se pokaždé, když uživatel vyberte jinou položku ze seznamu. K vytvoření této obslužné rutiny události, jednoduše dvakrát klikněte na rozevírací seznam v návrháři a pak přidejte následující kód:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Jak ukazuje obrázek 11, změna jenom GridView s `PageIndex` vlastnost způsobí, že data, která mají být odrážejí na GridView. V GridView s `DataBound` obslužné rutiny události, příslušné rozevírací seznam `ListItem` je vybrána.


[![Uživatel je automaticky provedena, aby se šesté stránky při výběru položky stránky 6 rozevíracího seznamu](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Obrázek 11**: uživatele je automaticky provedena, aby se šesté stránky při výběru položky stránky 6 rozevíracím seznamu ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Krok 5: Přidání řazení obousměrná podpora

Přidání obousměrná podpora řazení je jednoduché, přidání podpory stránkování jednoduše zaškrtnutí políčka Povolit řazení z GridView s inteligentním (který nastaví GridView s [ `AllowSorting` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) k `true`). To vykreslí každý záhlaví pole s GridView jako LinkButtons, při kliknutí na, způsobí zpětné volání a vrátit data seřazené podle kliknutelnou sloupce ve vzestupném pořadí. Kliknutím na tlačítko hlavičku stejné LinkButton znovu seřadí data v sestupném pořadí.

> [!NOTE]
> Pokud používáte vlastní Data Access Layer spíše než typové datové sady, nemusí mít možnost Povolit řazení v GridView s inteligentním. Pouze GridViews vázán ke zdrojům dat, které nativně podporují řazení mít toto políčko, které jsou k dispozici. Typové datové sady poskytuje podporu řazení se na pole, vzhledem k tomu, že poskytuje ADO.NET DataTable `Sort` metoda, která při vyvolání, seřadí DataTable s DataRows pomocí na zadaných kritériích.


Pokud vaše DAL nevrací objekty, které nativně podporují řazení, budete muset nakonfigurovat ObjectDataSource předávání řazení informací do vrstvy obchodní logiky, která lze řadit data nebo se data seřadit Dal. Jak řadit data na obchodní logiku a Data přístup vrstvy v budoucnu kurzu jsme budete prozkoumat.

Řazení LinkButtons se vykresluje jako hypertextové odkazy HTML, jehož aktuální barvy (modrá pro Nenavštívené odkaz a tmavým červenou barvu pro navštívené odkaz) kolidovat s barvu pozadí řádku záhlaví. Místo toho umožňují s mají všechny odkazy řádek záhlaví zobrazí v bílé, a to bez ohledu na to, jestli se sunout byla navštívené nebo ne. Můžete to provést přidáním následujícího `Styles.css` třídy:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Tuto syntaxi označuje používané bílý text při zobrazení těchto hypertextové odkazy v rámci elementu, který používá třídu HeaderStyle.

Po přidání šablon stylů CSS Pokud na stránce prostřednictvím prohlížeče obrazovce měla vypadat podobně jako obrázek 12. Obrázek 12 zejména, zobrazuje výsledky po klepnutí na odkaz cena s pole hlavičky.


[![Výsledky seřazeny podle pole UnitPrice vzestupné řazení](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Obrázek 12**: mají byly výsledky seřazené podle pole UnitPrice ve vzestupném pořadí ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Zkoumání řazení pracovního postupu

Všechny GridView polí BoundField, vlastnost CheckBoxField, TemplateField a tak dále mít `SortExpression` vlastnost, která určuje výraz, který se má použít k řazení dat. po klepnutí na tento pole s řazení záhlaví odkaz. GridView má také `SortExpression` vlastnost. Při řazení záhlaví po kliknutí na LinkButton, GridView přiřadí tohoto pole s `SortExpression` hodnoty na jeho `SortExpression` vlastnost. Data jsou v dalším kroku znovu načíst z ObjectDataSource a seřazené podle GridView s `SortExpression` vlastnost. Následující seznam popisuje, o pořadí kroků, které ukáže Pokud koncový uživatel seřadí data v GridView:

1. Rutina GridView s [událost Sorting](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) aktivuje
2. Rutina GridView s [ `SortExpression` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) je nastaven na `SortExpression` pole jejíž řazení hlavička LinkButton označeného kliknutím
3. ObjectDataSource znovu načte všechna data z BLL a potom seřadí data pomocí GridView s`SortExpression`
4. Rutina GridView s `PageIndex` vlastnost nastaven na hodnotu 0, znamená to, že při řazení uživatele se vrátí na první stránku dat (za předpokladu, že byl implementován podporu stránkování)
5. Rutina GridView s [ `Sorted` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) aktivuje

Jako s výchozí stránkování, výchozí řazení možnost znovu načte *všechny* záznamy z BLL. Při použití řazení bez stránkování, nebo při použití řazení s výchozí stránkování, zde s žádný způsob, jak obejít tento výkon přenosů (souborem ukládání do mezipaměti databázových dat). Ale, jak vidíte budete v budoucnu kurzu ji s efektivně řadit data při použití vlastní stránkování.

Při vytváření vazby ObjectDataSource k GridView pomocí rozevíracího seznamu v GridView s inteligentním, každé pole GridView automaticky má jeho `SortExpression` přiřazená název datového pole v vlastnost `ProductsRow` třídy. Například `ProductName` BoundField s `SortExpression` je nastaven na `ProductName`, jak ukazuje následující kód:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Pole lze nakonfigurovat tak, aby ho řazení zrušením s jeho `SortExpression` vlastnost (jeho přiřazení k prázdný řetězec). Pro ilustraci to si představte, který jsme dodán t má naše zákazníky seřadit cena našich produktů. `UnitPrice` BoundField s `SortExpression` vlastnost lze odebrat z deklarativní nebo prostřednictvím pole dialogových oken (která je přístupná kliknutím na odkaz Upravit sloupce v GridView s inteligentním).


![Výsledky seřazeny podle pole UnitPrice vzestupné řazení](paging-and-sorting-report-data-vb/_static/image27.png)

**Obrázek 13**: výsledky seřazeny podle pole UnitPrice vzestupné řazení


Jednou `SortExpression` bylo odebrané vlastnost `UnitPrice` BoundField, záhlaví je vykreslen jako text a nikoli jako odkaz, a tím brání uživatelům v řazení dat podle ceny.


[![Odebráním vlastnosti SortExpression mohou uživatelé seřadit už produkty podle ceny](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Obrázek 14**: odebráním vlastnost SortExpression mohou uživatelé už seřadit podle ceny produktů ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Prostřednictvím kódu programu řazení GridView

Můžete také řadit obsah GridView programově pomocí GridView s [ `Sort` metoda](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sort.aspx). Jednoduše předat `SortExpression` hodnotu seřadit podle spolu s [ `SortDirection` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` nebo `Descending`), a GridView s data budou znovu seřazená.

Představte si, že z důvodu jsme vypnuto řazení podle `UnitPrice` byl, protože nám obavy, že naše zákazníky by jednoduše koupit jenom nejlevnější produkty. Chceme motivovat je koupit nejnákladnější produkty, takže d chceme, aby mohli seřadit podle ceny, ale pouze z nejnákladnější cena produkty k nejmenší.

Provedete to přidání ovládacího prvku tlačítko webové na stránku, nastavte jeho `ID` vlastnost `SortPriceDescending`a jeho `Text` vlastnosti řazení podle ceny. Dále vytvořte obslužnou rutinu události pro tlačítko s `Click` událostí dvojitým kliknutím tlačítko – ovládací prvek v návrháři. Přidejte následující kód do této obslužné rutiny události:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Kliknutím na toto tlačítko vrátí uživatele na první stránku s produkty seřazené podle ceny z nejnákladnější způsob nejlevnější (viz obrázek 15).


[![Kliknutím na tlačítko řadí produkty z nejnákladnější k nejméně](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Obrázek 15**: Kliknutím na tlačítko řadí produkty z nejnákladnější k nejméně ([Kliknutím zobrazit obrázek v plné velikosti](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak implementovat výchozí stránkování a řazení možnosti které byly stejně snadná jako kontroluje zaškrtávací políčko! Když uživatel seřadí nebo stránky pomocí dat, kterou se podobně jako pracovního postupu:

1. Vyplývá zpětné volání
2. Data ovládací prvek webu s předem úrovně aktivuje událost (`PageIndexChanging` nebo `Sorting`)
3. Všechna data znovu načte prvku ObjectDataSource
4. Ovládací prvek webu s dat na úrovni po aktivuje událost (`PageIndexChanged` nebo `Sorted`)

Při implementaci a základní stránkování uloženy, musí být další úsilí působící využívat efektivnější vlastní stránkování nebo dále vylepšit rozhraní stránkování nebo řazení. Budoucí kurzy zaměřovat na tato témata.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Předchozí](creating-a-customized-sorting-user-interface-cs.md)
[další](efficiently-paging-through-large-amounts-of-data-vb.md)
