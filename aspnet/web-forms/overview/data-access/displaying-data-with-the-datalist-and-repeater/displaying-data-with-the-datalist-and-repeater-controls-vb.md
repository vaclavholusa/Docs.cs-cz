---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: Zobrazení dat ovládacími ovládacích prvků DataList a Repeater (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozích kurzech budeme mít používá k zobrazení dat ovládacím prvku GridView. Od v tomto kurzu se podíváme na vytváření běžných sestav vzorů s...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ece899d4c9f3277fc27cdfc9c3f3e25ab7809fb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753679"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Zobrazení dat ovládacími ovládacích prvků DataList a Repeater (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) nebo [stahovat PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> V předchozích kurzech budeme mít používá k zobrazení dat ovládacím prvku GridView. Od v tomto kurzu se podíváme na vytváření běžných sestav vzorů s ovládacími prvky DataList a Repeater od základní informace o zobrazení dat ovládacím prvkem tyto ovládací prvky.


## <a name="introduction"></a>Úvod

Ve všech příkladech v rámci posledních 28 kurzy, pokud jsme potřebovali pro zobrazení více záznamů ze zdroje dat jsme převedena na ovládacím prvku GridView. GridView vykreslí řádek pro každý záznam ve zdroji dat, zobrazení datových polí záznamu s ve sloupcích. Zatímco prvku GridView umožňuje rychle zobrazit, procházet, řadit, úpravy a odstranění dat, je trochu boxy její vzhled. Kromě toho odpovídá kódu pro strukturu s GridView vyřešen zahrnuje HTML `<table>` s řádku tabulky (`<tr>`) pro každý záznam a buňky tabulky (`<td>`) pro každé pole.

Kvůli větší míru přizpůsobení vzhledu a vykreslované značky při zobrazení více záznamů, technologii ASP.NET 2.0 nabízí ovládacích prvků DataList a Repeater (které byly také k dispozici v ASP.NET verze 1.x). Ovládací prvky DataList a Repeater vykreslení svůj obsah pomocí šablon místo BoundFields CheckBoxFields, ButtonFields a tak dále. Stejně jako prvku GridView, prvku DataList vykreslí jako HTML `<table>`, ale umožňuje více záznamů zdroje zobrazeného každý řádek tabulky. Repeater, na druhé straně vykreslí žádné další značky, než co explicitně zadat a je ideální volbou v případě potřebujete mít naprostou kontrolu nad značkami, protože ho.

Za další desítek nebo Ano kurzy podíváme na vytváření běžných sestav vzorů s ovládacími prvky DataList a Repeater od základní informace o zobrazení dat ovládacím prvkem tyto šablony ovládacích prvků. Uvidíme, jak formátovat tyto ovládací prvky, jak změnit rozložení záznamů zdroje dat v prvku DataList, běžné scénáře hlavních/podrobných, způsoby pro úpravy a odstraňování dat, jak stránkovat záznamy a tak dále.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Krok 1: Přidání ovládacích prvků DataList a Repeater kurz webových stránek

Než začneme v tomto kurzu, umožní s nejdřív využít pro přidání stránek ASP.NET, které potřebujeme pro tento kurz a další několik kurzy týkající se zobrazováním dat s využitím ovládacích prvků DataList a Repeater. Začněte tím, že vytvoříte novou složku v projektu s názvem `DataListRepeaterBasics`. Dále přidejte následující pět stránek ASP.NET do této složky s všechny z nich nakonfigurovat tak, aby na hlavní stránce `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Vytvořte složku DataListRepeaterBasics a přidávání stránek kurz ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Obrázek 1**: vytvoření `DataListRepeaterBasics` složky a přidávání stránek kurz ASP.NET


Otevřít `Default.aspx` stránku a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku od `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-vb.md) kurzu mapy webu a zobrazí výčet kurzy z aktuálního oddílu v seznamu s odrážkami.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


Pokud chcete zobrazit seznam s odrážkami v ovládacích prvcích DataList a Repeater kurzech, které nám budete vytvářet, potřebujeme přidat je do mapy webu. Otevřít `Web.sitemap` a přidejte následující kód za kód uzel mapy webu přidání vlastního tlačítka:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Krok 2: Zobrazení informací o produktu pomocí prvku DataList

Podobný třídě FormView, ovládací prvek DataList s vykreslí výstup závisí na šablony spíše než BoundFields CheckBoxFields a tak dále. Na rozdíl od třídy FormView prvku DataList slouží k zobrazení sadu záznamů, místo solitary jeden. Umožní s začít v tomto kurzu se podívat na vazby informací o produktu a v prvku DataList. Začněte otevřením `Basics.aspx` stránku `DataListRepeaterBasics` složky. A v prvku DataList v dalším kroku přetáhněte z panelu nástrojů do návrháře. Jak ukazuje obrázek 4 před určením šablony ovládacích prvků DataList s návrháři zobrazí jako šedé pole.


[![Přetáhněte z panelu nástrojů na Návrhář prvku DataList](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Obrázek 4**: přetažením ovládacích prvků DataList z panelu nástrojů na the návrháře ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


V prvku DataList s inteligentní značky, přidejte nový prvek ObjectDataSource a nakonfigurujte ho na použití `ProductsBLL` třída s `GetProducts` metody. Protože opětovné vytváření DataList jen pro čtení v tomto kurzu nastavíme rozevíracího seznamu na (žádný) v Průvodci s vložit, aktualizovat a odstraňovat karty.


[![Rozhodnout vytvořit nového prvku ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Obrázek 5**: rozhodnout vytvořit nového prvku ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Obrázek 6**: Konfigurace ObjectDataSource k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![Načíst informace o všech produktů pomocí GetProducts – metoda](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Obrázek 7**: načtení informací o všech produktů pomocí `GetProducts` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


Po konfiguraci ObjectDataSource a jeho přidružením k prvku DataList prostřednictvím jeho inteligentních značek, vytvoří Visual Studio automaticky `ItemTemplate` v ovládacím prvku DataList, která zobrazuje název a hodnotu každé datové pole vrácené zdroje dat (najdete v článku Kód níže). Toto výchozí nastavení `ItemTemplate` vzhled je shodná s automaticky vytvoří při vytvoření vazby zdroje dat na třídy FormView pomocí návrháře šablony.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Připomínáme, že po vytvoření vazby zdroje dat k ovládacímu prvku FormView prostřednictvím inteligentních značek s FormView, Visual Studio vytvořeno `ItemTemplate`, `InsertItemTemplate`, a `EditItemTemplate`. Pomocí prvku DataList, ale pouze `ItemTemplate` se vytvoří. Je to proto prvku DataList nemá stejný integrované úpravy a vložení podporu, kterou nabízí ve třídě FormView. Prvku DataList obsahovat upravit a odstranit související události a úpravy a odstranění podpory lze přidat pomocí hodně kódu, ale zde s jednoduchou out-of-the-box nepodporuje jako s FormView. Uvidíme, jak zahrnout úpravy a odstranění podpory s prvku DataList v budoucích kurzech.


Umožní s využít ke zlepšení vzhledu této šablony. Místo zobrazení všech datových polí, umožnit zobrazit pouze s názvem produktu, Dodavatel, kategorie, množství na jednotku a cena za jednotku s. Kromě toho vám umožňují s zobrazovaný název v `<h4>` záhlaví a rozložení zbývající pole pomocí `<table>` pod nadpisem.

Pokud chcete tyto změny, které můžete provést buď pomocí šablony funkce v Návrháři z prvku DataList s inteligentní značky, klikněte na odkaz Upravit šablony nebo můžete upravit šablonu ručně pomocí deklarativní syntaxe s stránky pro úpravu. Pokud použijete možnost Upravit šablony v návrháři, výsledný kód nemusí přesně odpovídat následující značky, ale při zobrazení v prohlížeči by měl vypadají velmi podobně jako obrazovky je vidět na obrázku 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> V příkladu výše používá popisek webové ovládací prvky jejichž `Text` vlastnost přiřazena hodnota Syntaxe datové vazby. Také jsme může zapomněli popisky úplně se vynechá, psát v syntaxi datové vazby. To znamená, že místo použití `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` jsme mohli místo toho použít deklarativní syntaxe `<%# Eval("CategoryName") %>`.


Nechat na ovládací prvky popisek webových, ale nabízí dvě výhody. Nejprve nabízí jednodušší způsob formátování dat na základě dat, jak uvidíme v dalším kurzu. Za druhé: možnost Upravit šablony v návrháři kódu t zobrazení datové vazby deklarativní syntaxe, které se zobrazí mimo některé webový ovládací prvek. Místo toho rozhraní upravit šablony je navržené pro usnadnění práce s statické značek a webové ovládací prvky a předpokládá, že všechny datové vazby se provede pomocí dialogového okna Upravit datové vazby, který je přístupný z inteligentních značek webové ovládací prvky.

Proto při práci s DataList, které poskytuje možnost úpravy šablony prostřednictvím návrháře, chci raději použít popisek webové ovládací prvky tak, aby obsah je dostupný prostřednictvím rozhraní upravit šablony. Jak uvidíme krátce, Opakovači vyžaduje, upravit obsah šablony s ze zobrazení zdroje. V důsledku toho při vytváření šablony s Repeater můžu budete často vynechat popisek webové ovládací prvky, pokud vím, že budete muset formátování vzhledu data vázaná text podle programovou logiku.


[![Každý produkt s výstup je vykreslen pomocí prvku DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Obrázek 8**: každý produkt s výstup je vykreslen pomocí prvku DataList s `ItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Krok 3: Zlepšení vzhledu prvku DataList

Jako prvku GridView, prvku DataList nabízí celou řadu stylu vlastnosti, jako například `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, a tak dále. Při práci s ovládacími prvky GridView a DetailsView, jsme vytvořili souborech skinů v `DataWebControls` motiv, který předem definovanou `CssClass` vlastnosti těchto dvou ovládacích prvků a `CssClass` vlastnost pro některé z jejich objektu třídy subproperties (`RowStyle` `HeaderStyle`, a tak dále). Umožní to samé proveďte pro prvku DataList s.

Jak je popsáno v [zobrazení dat se prvku ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) kurz souboru skinu Určuje výchozí vzhled související vlastnosti pro ovládací prvek webové; motivu je kolekce vzhledu, šablon stylů CSS, image a JavaScript soubory, které definují konkrétní vzhled a chování pro web. V *zobrazení dat se prvku ObjectDataSource* kurzu jsme vytvořili `DataWebControls` motivu (která je implementována jako složku v rámci `App_Themes` složky), který má, v současné době dva soubory se vzhledem - `GridView.skin` a `DetailsView.skin`. Umožňuje přidat třetí soubor vzhledu a zadejte nastavení předdefinovaný styl pro prvku DataList s.

Chcete-li přidat soubor vzhledu, klikněte pravým tlačítkem na `App_Themes/DataWebControls` složky, zvolte možnost Přidat novou položku a ze seznamu vyberte možnost soubor vzhledu. Název souboru `DataList.skin`.


[![Vytvořte nový soubor vzhledu DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Obrázek 9**: Vytvořte nový soubor vzhledu název `DataList.skin` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Použijte následující kód pro `DataList.skin` souboru:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Tato nastavení přiřadit stejné třídy šablony stylů CSS příslušné vlastnosti ovládacích prvků DataList jako byly použity s ovládacími prvky GridView a prvku DetailsView. Třídy šablony stylů CSS se tady použít `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, a tak dále jsou definovány v `Styles.css` souboru a byly přidány v předchozích kurzech.

Přidání tohoto souboru skinu vzhled ovládacích prvků DataList s aktualizován v Návrháři (budete muset aktualizovat zobrazení návrháře, aby viděli efekt nový soubor vzhledu; v nabídce Zobrazit, vyberte příkaz Aktualizovat). Jak je vidět na obrázku 10, má každý střídavé produkt světle růžová pozadí.


[![Vytvořte nový soubor vzhledu DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Obrázek 10**: Vytvořte nový soubor vzhledu název `DataList.skin` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Krok 4: Zkoumání prvku DataList s ostatními šablonami

Kromě `ItemTemplate`, prvku DataList podporuje šest původní šablony:

- `HeaderTemplate` Pokud je zadán, přidá řádek záhlaví do výstupu a slouží k vykreslení tento řádek
- `AlternatingItemTemplate` použije k vykreslení střídavé položky
- `SelectedItemTemplate` použije k vykreslení vybrané položce. Vybraná položka nachází položka, jejíž index odpovídá prvku DataList s [ `SelectedIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` použije k vykreslení položky, který právě upravujete
- `SeparatorTemplate` Pokud je zadán, přidá oddělovač mezi jednotlivými položkami a slouží k vykreslení tento oddělovač
- `FooterTemplate` – Pokud je zadán, přidá řádek zápatí do výstupu a slouží k vykreslení tento řádek

Při zadávání `HeaderTemplate` nebo `FooterTemplate`, prvku DataList přidá další řádek záhlaví nebo zápatí vykresleného výstupu. Stejně jako s GridView s záhlaví a zápatí řádky, záhlaví a zápatí a v prvku DataList nejsou vázány na data. Proto všechny datové vazby syntaxe v poznámce `HeaderTemplate` nebo `FooterTemplate` , pokusy o přístup k vázaných dat vrátí prázdný řetězec.

> [!NOTE]
> Jak jsme viděli v [zobrazuje souhrnné informace v zápatí prvku GridView s](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) kurz, zatímco řádky záhlaví a zápatí zadávat t podporu databinding syntaxe, informace týkající se dat můžete vloží přímo do těchto řádků z GridView s `RowDataBound` obslužné rutiny události. Tato technika může být použito pro obě výpočtu mezisoučtů nebo jiných informací z dat vázány na ovládací prvek také přiřadit tyto informace v zápatí je uvedené. Stejný princip můžete použít u ovládacích prvků DataList a Repeater; jediným rozdílem je, že pro obslužnou rutinu události pro vytvoření ovládacích prvků DataList a Repeater `ItemDataBound` událostí (místo pro `RowDataBound` událostí).


V našem příkladu vám umožňují s mají název produktu informace zobrazené v horní části DataList s výsledky do `<h3>` záhlaví. Chcete-li to provést, přidejte `HeaderTemplate` s odpovídající značky. Z návrháře, můžete to provést kliknutím na odkaz Upravit šablony v prvku DataList s inteligentním, výběr šablony záhlaví z rozevíracího seznamu a zadáním textu po výběru možnosti záhlaví 3 z rozevíracího seznamu styl seznamu (viz obrázek 11).


[![Přidat parametr HeaderTemplate s informacemi o produktu Text](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Obrázek 11**: Přidejte `HeaderTemplate` s informacemi o produktu Text ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Případně jde přidat pomocí deklarace tak, že zadáte následující kód v rámci `<asp:DataList>` značky:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Přidání hodně mezeru mezi každou výpis produktů, umožní s přidat `SeparatorTemplate` , která obsahuje řádek mezi každé části. Vodorovná čára značky (`<hr>`), přidá tyto oddělovač. Vytvořte `SeparatorTemplate` tak, aby byly následující kód:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Podobně jako `HeaderTemplate` a `FooterTemplates`, `SeparatorTemplate` není vázán na libovolný záznam ze zdroje dat, a proto nemůže přímo přístupu zdroj dat záznamů, které jsou vázány na ovládacím prvku DataList.


Po provedení sčítání, při zobrazení stránky přes prohlížeč by měl vypadat podobně jako obrázek 12. Poznamenejte si řádek záhlaví a řádek mezi každou výpis produktů.


[![Prvku DataList obsahuje řádek záhlaví a vodorovná čára mezi každou výpis produktů](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Obrázek 12**: prvku DataList obsahuje řádek záhlaví a vodorovné pravidlo mezi každý výpis produktů ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Krok 5: Vykreslování konkrétní značky s ovládacím prvkem Repeater

Pokud tak učiníte zobrazení/Zdroj v prohlížeči při návštěvě DataList příklad z obrázek 12, uvidíte, že ovládacích prvcích DataList vysílá HTML `<table>` , která obsahuje řádek tabulky (`<tr>`) s buňky jedné tabulky (`<td>`) pro každou položku vázat na V prvku dataList. Tento výstup, ve skutečnosti se shoduje s co by měl vyzařovaného z prvku GridView s TemplateField jeden. Jak uvidíme v budoucích kurzech, prvku DataList umožňuje další přizpůsobení výstupu, umožňuje nám k zobrazení více záznamů zdroje dat na řádek tabulky.

Co když nepoužíváte t chcete generovat HTML `<table>`, i když? Pro celkový počet a kompletní kontrolu nad značkami vygenerovaný webový ovládací prvek dat musíme použít ovládacím prvku opakovače. Stejně jako prvku DataList Opakovači je vytvořený na základě šablony. Repeater, ale pouze nabízí následujících pět šablon:

- `HeaderTemplate` Pokud je zadán, přidá zadanou značku před položkami
- `ItemTemplate` použije k vykreslení položky
- `AlternatingItemTemplate` Pokud je zadán, použije k vykreslení střídavé položky
- `SeparatorTemplate` Pokud je zadán, přidá zadané značky mezi jednotlivými položkami
- `FooterTemplate` – Pokud je zadán, přidá zadanou značku až po položkách

V technologii ASP.NET 1.x, ovládací prvek se běžně používá k zobrazení seznamu s odrážkami, jejichž data pochází z některých zdrojů dat opakovače. V takovém případě `HeaderTemplate` a `FooterTemplates` by obsahoval otevírací a zavírací `<ul>` značky, v uvedeném pořadí, zatímco `ItemTemplate` by obsahoval `<li>` prvky s datovou vazbou syntaxe. Tento přístup můžete použít stále v technologii ASP.NET 2.0, jako jsme viděli v dva příklady v [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-vb.md) kurzu:

- V `Site.master` stránky předlohy, Repeateru byla použita k zobrazení seznamu s odrážkami obsah mapy webu nejvyšší úrovně (základní tvorbou sestav, filtrování sestavy, přizpůsobené formátování a tak dále); jiné, vnořené Repeater byla použita k zobrazení podřízených oddílů nejvyšší úrovně oddíly
- V `SectionLevelTutorialListing.ascx`, Repeateru byla použita k zobrazení seznamu s odrážkami v částech podřízené objekty aktuálního oddílu mapy webu

> [!NOTE]
> ASP.NET 2.0 představuje nové [ovládacího prvku BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), které mohou být vázány na ovládací prvek zdroje dat zobrazíte jednoduchý seznam s odrážkami. Pomocí ovládacího prvku BulletedList jsme není potřeba zadávat žádné související seznamu HTML; Místo toho jsme jednoduše určíte pole se zobrazí jako text pro každou položku seznamu.


Opakovači slouží jako bloku catch. všechna data webový ovládací prvek. Pokud není existující ovládací prvek, který generuje potřebné značky, je možné ovládacím prvku opakovače. Pro ilustraci použití opakovače umožní mít seznam kategorií zobrazený nad DataList informace o produktu vytvořili v kroku 2 s. Zejména umožňují s mít kategorie zobrazuje ve formátu HTML jednoho řádku `<table>` s každou kategorii, zobrazí jako sloupec v tabulce.

K tomu spuštění přetažením ovládacím prvkem Repeater z panelu nástrojů na Návrhář nad DataList informace o produktu. Stejně jako u prvku DataList, Opakovači otevření zobrazí jako šedé pole až do jeho šablony byly definovány.


[![Přidat Repeateru do návrháře](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Obrázek 13**: Přidat do návrháře Repeateru ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Pouze jednou z možností v opakovače s inteligentním tam s: Zvolit zdroj dat. Optimalizované k vytvoření nového prvku ObjectDataSource a nakonfigurujte ho na použití `CategoriesBLL` třída s `GetCategories` metody.


[![Vytvoření nového prvku ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Obrázek 14**: vytvoření nového prvku ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![Konfigurace ObjectDataSource pomocí třídy CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Obrázek 15**: Konfigurace ObjectDataSource k použití `CategoriesBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Načíst informace o všech kategorií pomocí GetCategories – metoda](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Obrázek 16**: načtení informací o všech kategorií pomocí `GetCategories` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


Na rozdíl od prvku DataList Visual Studio neprovádí automatické vytváření šablona ItemTemplate pro Opakovači po vazby ke zdroji dat. Kromě toho šablony opakovače s nelze nakonfigurovat pomocí návrháře a musí být zadán deklarativně.

Chcete-li zobrazit kategorie jako jeden řádek `<table>` sloupec pro každou kategorii, potřebujeme Repeater a vygenerovat kód podobný následujícímu:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Vzhledem k tomu, `<td>Category X</td>` text je část se bude opakovat, budou se zobrazovat opakovače s ItemTemplate. Kód, který se zobrazí před – `<table><tr>` -budou umístěny do `HeaderTemplate` při koncové značky - `</tr></table>` -budou umístěny do `FooterTemplate`. Zadejte tato nastavení šablony, přejděte na deklarativní části stránky ASP.NET po kliknutí na tlačítko zdroj v levém dolním rohu a zadejte následující syntaxi:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Opakovači vysílá přesné značek podle jeho šablony, nic jiného, nic méně. Obrázek 17 zobrazí výstup s Repeater při prohlížení prostřednictvím prohlížeče.


[![Jeden řádek HTML &lt;tabulky&gt; uvádí každou kategorii v samostatném sloupci](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Obrázek 17**: HTML A jednoho řádku `<table>` uvádí každou kategorii v samostatném sloupci ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Krok 6: Zlepšení vzhledu Opakovači

Protože Opakovači vysílá přesně značek určené šablony, by měl mít jako žádným překvapením, že neexistují žádné vlastnosti stylu pro opakovače. Pokud chcete změnit vzhled obsahu generovaného Opakovači, jsme třeba ručně přidat potřebný obsah HTML nebo šablon stylů CSS přímo do šablony opakovače s.

V našem příkladu umožní s mít sloupce kategorie alternativní barvy pozadí, třeba s střídavé řádky v ovládacím prvku DataList. Provedete to tak, musíme přiřadit `RowStyle` třídu šablony stylů CSS pro každou položku Repeater a `AlternatingRowStyle` třídu šablony stylů CSS na jednotlivé střídavé položky Repeater prostřednictvím `ItemTemplate` a `AlternatingItemTemplate` šablony, například takto:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Umožňuje také přidat řádek záhlaví do výstupu s textem kategorie produktů s. Protože jsme zadávat t vědět kolik sloupců naše vyplývající `<table>` se skládá, je nejjednodušší způsob, jak vygenerovat řádek záhlaví, která se zaručeně rozložit všechny sloupce použití *dvě* `<table>` s. První `<table>` bude obsahovat dva řádky záhlaví a řádek, který bude obsahovat druhý, jednoho řádku `<table>` sloupec pro každou kategorii, která má v systému. To znamená, že chceme, aby generoval následující kód:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

Následující `HeaderTemplate` a `FooterTemplate` za následek požadované značky:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

Po provedení těchto změn se zobrazí obrázek 18 opakovače.


[![Alternativní barva pozadí sloupce kategorie a obsahuje řádek záhlaví](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Obrázek 18**: alternativní sloupce v kategorii v barvu pozadí a zahrnuje řádek záhlaví ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Souhrn

Ovládací prvek GridView usnadňuje zobrazení, úpravy, odstranění, řadit a stránkovat data, vzhled je velmi boxy a mřížky. Pro větší kontrolu nad vzhled musíme zapnout ovládací prvky DataList nebo Repeater. Obě tyto ovládací prvky zobrazují sadu záznamů pomocí šablon místo BoundFields CheckBoxFields a tak dále.

Prvku DataList vykreslí jako HTML `<table>` , ve výchozím nastavení, zobrazí každý záznam zdroje dat v řádku jedné tabulky, stejně jako prvek GridView s TemplateField jeden. Jak vidíte v budoucích kurzech, však prvku DataList povolit více záznamů, který se má zobrazit každý řádek tabulky. Repeater, na druhé straně výhradně vysílá značek podle jeho šablony. nepřidává žádné další značky a proto se běžně používá pro jiné než zobrazení dat prvků HTML `<table>` (například v seznamu s odrážkami).

Když v prvku DataList a Repeater nabízejí větší flexibilitu v jejich vykresleného výstupu, nemají řadu integrovaných funkcí v prvku GridView. Jak prozkoumáme v budoucích kurzech, některé z těchto funkcí může být napájen ze sítě zpět bez příliš mnoho úsilí, ale proveďte zachovat v paměti, která pomocí prvku DataList nebo Repeater namísto prvku GridView omezit funkce, které můžete použít bez nutnosti k implementaci těchto funkcí sami.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Yaakov Ellis, Liz Shulok, Randy Schmidt a Stacy Park. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](nested-data-web-controls-cs.md)
> [další](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
