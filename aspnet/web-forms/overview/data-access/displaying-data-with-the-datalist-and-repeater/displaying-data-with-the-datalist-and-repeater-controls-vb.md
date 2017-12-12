---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: "Zobrazení dat s ovládacími prvky opakovače (VB) a DataList | Microsoft Docs"
author: rick-anderson
description: "V předchozím kurzech jsme použili prvek GridView zobrazujících data. Od v tomto kurzu se podíváme na vytváření běžných vzorů vytváření sestav s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: b6e71470ae2888ac4332f896c34f666618a425e7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Zobrazení dat s ovládacími prvky opakovače (VB) a DataList
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) nebo [stáhnout PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> V předchozím kurzech jsme použili prvek GridView zobrazujících data. Od v tomto kurzu se podíváme na vytváření běžných vzorů vytváření sestav s ovládacími prvky DataList a opakovače počínaje základní informace o zobrazení dat pomocí těchto ovládacích prvků.


## <a name="introduction"></a>Úvod

Ve všech v příkladech v minulosti 28 kurzy, podle potřeby jsme zobrazovat více záznamů ze zdroje dat, jsme do ovládacího prvku GridView vypnuté. GridView vykreslí řádek pro každý záznam ve zdroji dat zobrazení datových polí záznamu s ve sloupcích. GridView umožňuje rychle zobrazit, procházet, řazení, upravit a odstranit data, i když je trochu boxy její vzhled. Kromě toho zodpovědná značek pro strukturu s GridView vyřešen zahrnuje HTML `<table>` s řádku tabulky (`<tr>`) pro každý záznam a buňky tabulky (`<td>`) pro každé pole.

Zajistit vyšší stupeň přizpůsobení vzhledu a vykreslované značky při zobrazení více záznamů, technologii ASP.NET 2.0 nabízí funkci DataList a opakovače ovládacích prvků (obou z nich byly také k dispozici v technologii ASP.NET verze 1.x). Ovládací prvky DataList a opakovače vykreslení obsah pomocí šablony, nikoli BoundFields, CheckBoxFields, ButtonFields a tak dále. Jako GridView, prvku DataList vykreslí jako HTML `<table>`, ale umožňuje více data zobrazit na řádku tabulky zdrojové záznamy. Opakovače, na druhé straně nevykreslí žádný další kód než co je explicitně zadat a je ideální volbou, když potřebujete přesné kontrolu nad značek vygenerované.

Přes vedle desítek nebo Ano kurzy podíváme vytváření běžných vzorů vytváření sestav s ovládacími prvky DataList a opakovače počínaje základní informace o zobrazení dat s těmito šablonami ovládací prvky. Ukážeme, jak formátovat tyto ovládací prvky, jak pozměnit rozložení záznamů zdroje dat v DataList, běžné scénáře hlavní/podrobnosti, způsoby, upravovat a odstraňovat data, jak procházet záznamy a tak dále.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Krok 1: Přidání DataList a opakovače kurz webové stránky

Než začneme v tomto kurzu, umožní s nejprve přidat na stránky ASP.NET, které budeme potřebovat pro tento kurz a další několik kurzy týkající se zobrazení dat pomocí DataList a opakovače chvíli trvat. Začněte vytvořením novou složku v projektu s názvem `DataListRepeaterBasics`. Dál přidejte následující pět stránek ASP.NET do této složky rozmístit nakonfigurované na používání stránky předlohy `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Vytvořte složku DataListRepeaterBasics a přidání stránky kurz ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Obrázek 1**: vytvoření `DataListRepeaterBasics` složky a přidat stránky kurz ASP.NET


Otevřete `Default.aspx` stránky a přetáhněte ji `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek z `UserControls` složky na návrhovou plochu. Tento uživatelský ovládací prvek, který jsme vytvořili v [hlavní stránky a webové navigace](../introduction/master-pages-and-site-navigation-vb.md) kurzu mapy webu a zobrazí výčet kurzů k z aktuálního oddílu v seznamu s odrážkami.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


Pokud chcete mít v zobrazení seznamu s odrážkami DataList a opakovače kurzy, které jsme budete vytvářet, je třeba je přidáte do mapy webu. Otevřete `Web.sitemap` souboru a přidejte následující kód po přidání tlačítka vlastní lokality mapy uzlu značek:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Aktualizace mapy webu zahrnout nové stránky ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Obrázek 3**: aktualizace mapy webu zahrnout nové stránky ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Krok 2: Zobrazení informací o produktu pomocí DataList

Podobně jako FormView ovládacího prvku DataList s vykreslení výstupu závisí na šablony místo BoundFields, CheckBoxFields a tak dále. Na rozdíl od FormView prvku DataList slouží k zobrazení sadu záznamů, místo solitary jeden. Umožní s začít v tomto kurzu se podívejte se na vytvoření vazby informace o produktu DataList. Začněte otevřením `Basics.aspx` stránku `DataListRepeaterBasics` složky. V dalším kroku přetáhněte DataList z panelu nástrojů na návrháře. Jak ukazuje obrázek 4, před zadáním šablony DataList s návrháře ji zobrazí jako šedé pole.


[![Přetáhněte DataList z panelu nástrojů na návrháře](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Obrázek 4**: přetáhněte DataList ze sady nástrojů do Návrhář ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


Z DataList s inteligentní značky, přidat nové ObjectDataSource a nakonfigurujte ho na použití `ProductsBLL` třídu s `GetProducts` metoda. Vzhledem k tomu, že jsme re vytváření DataList jen pro čtení v tomto kurzu nastavit rozevíracím seznamu (None) v průvodce s příkazy INSERT, aktualizovat a odstranit karty.


[![OPT k vytvoření nové ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Obrázek 5**: Opt vytvořit nové ObjectDataSource ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![Konfigurace ObjectDataSource použití třídy ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Obrázek 6**: Konfigurace ObjectDataSource pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![Načtení informací o všechny produkty pomocí metody GetProducts](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Obrázek 7**: načíst informace o všech produktů použití `GetProducts` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


Po konfiguraci ObjectDataSource a přiřadí se mu DataList prostřednictvím jeho inteligentních značek, vytvoří sada Visual Studio automaticky `ItemTemplate` v DataList, který zobrazí název a hodnotu každé datové pole vrácené zdroje dat (najdete značka níže). Toto výchozí nastavení `ItemTemplate` s vzhled je stejná jako šablon automaticky vytvoří při vytvoření vazby zdroje dat na FormView pomocí návrháře.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Odvolat, aby při vytvoření vazby zdroje dat do ovládacího prvku FormView prostřednictvím FormView s inteligentním, Visual Studio vytvoří `ItemTemplate`, `InsertItemTemplate`, a `EditItemTemplate`. S DataList, ale pouze `ItemTemplate` je vytvořena. Je to proto, že DataList nemá stejné předdefinované úpravy a vkládání podpory, které nabízí FormView. DataList neobsahuje upravit a odstranit související události a úpravy a odstraňování podporu přidáním s malou část kódu, ale zde s jednoduché se na pole nepodporuje jako s FormView. Jak se zahrnuje úpravy a odstraňování podporu s prvku DataList v budoucí kurzu uvidíme.


Umožní s pozorně vylepšení vzhledu této šablony. Místo zobrazení všechna pole dat, umožní s zobrazit pouze s název produktu, Dodavatel, kategorie, množství jednotce a jednotkové ceny. Navíc umožňují s zobrazovaný název v `<h4>` záhlaví a rozložení zbývající polí pomocí `<table>` pod záhlaví.

K provedení těchto změn, můžete buď použijte šablonu funkce v Návrháři z DataList s inteligentní značky klikněte na odkaz Upravit šablony nebo můžete upravit šablonu ručně prostřednictvím stránky s deklarativní syntaxi pro úpravy. Pokud použijete možnost Upravit šablony v návrháři, výsledný zápis nemusí odpovídat přesně následující značky, ale při zobrazení v prohlížeči by měl vypadají velmi podobně jako na uvedené na obrázku 8 snímku obrazovky.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> V příkladu výše používá popisek webové ovládací prvky jejichž `Text` vlastnosti přiřazena hodnota Syntaxe datové vazby. Alternativně jsme může mít tento parametr vynechán popisků zcela, zadáním v právě Syntaxe datové vazby. To znamená místo použití `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` může místo toho použili jsme deklarativní syntaxi `<%# Eval("CategoryName") %>`.


Ponechat v ovládacích prvcích popisek Web, ale nabízí dvě výhody. Nejprve nabízí jednodušší znamená pro formátování dat na základě dat, protože ukážeme v dalším kurzu. Za druhé: možnost Upravit šablony v syntaxi Návrhář nemá t zobrazení deklarativní vazby dat, které se zobrazí mimo některé ovládací prvek webu. Místo toho rozhraní upravit šablony je navržena pro usnadnění práce s statické značek a webové ovládací prvky a předpokládá, že žádné datové vazby bude provedeno pomocí dialogového okna Upravit datové vazby, která je přístupná z inteligentních značek webové ovládací prvky.

Proto při práci s DataList, která poskytuje možnost úpravy šablon prostřednictvím návrháře, raději nechci použití popisek webových ovládacích prvků tak, aby obsah je dostupný přes rozhraní upravit šablony. Jak jsme krátce zobrazí, opakovače vyžaduje, že obsah s šablonu upravit ze zobrazení zdroje. V důsledku toho při věnujte I budete často vynechejte popisek webové šablony opakovače s prvky, pokud to že budete muset formátování vzhledu dat vázán text na základě programové logiky.


[![Každý produkt s výstup je vykreslen pomocí DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Obrázek 8**: každý produkt s výstup je vykreslen pomocí DataList s `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Krok 3: Vylepšení vzhledu ovládacího prvku DataList

Jako GridView, prvku DataList nabízí řadu vlastnosti související se styly, jako například `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`a tak dále. Při práci s ovládacími prvky GridView a DetailsView, jsme vytvořili soubory vzhledu v `DataWebControls` motiv, který předem definované `CssClass` vlastnosti těchto dvou ovládacích prvků a `CssClass` vlastnost pro některé z jejich podvlastnosti (`RowStyle` `HeaderStyle`a tak dále). Umožní s Totéž proveďte pro prvku DataList.

Jak je popsáno v [zobrazení dat pomocí ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) kurz, soubor vzhledu Určuje výchozí vlastnosti týkající se vzhledu ovládacího prvku webového; je kolekce vzhledu, CSS, image a JavaScript soubory, které definují motivu konkrétní vzhled a chování pro web. V *zobrazení dat pomocí ObjectDataSource* kurzu jsme vytvořili `DataWebControls` motiv (která je implementovaná jako složky v rámci `App_Themes` složky) s, v současné době dva soubory vzhledu - `GridView.skin` a `DetailsView.skin`. Umožní s přidat třetí – soubor vzhledu a zadejte nastavení předdefinovaný styl pro prvku DataList.

Chcete-li přidat soubor vzhledu, klikněte pravým tlačítkem na `App_Themes/DataWebControls` složku, zvolte možnost Přidat novou položku a vyberte možnost – soubor vzhledu ze seznamu. Název souboru `DataList.skin`.


[![Vytvořte nový soubor vzhledu s názvem DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Obrázek 9**: Vytvořte nový soubor vzhledu název `DataList.skin` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Použít následující kód pro `DataList.skin` souboru:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Tato nastavení přiřadit stejné tříd CSS na odpovídající vlastnosti DataList jako měla použít s ovládacími prvky GridView a DetailsView. Tříd CSS použít se zde `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`a tak dále jsou definovány v `Styles.css` souboru a byly přidány v předchozí kurzy.

Přidání tento soubor vzhledu vzhled s DataList aktualizován v Návrháři (pravděpodobně nutné aktualizovat zobrazení Návrhář projeví se nový soubor vzhledu; z nabídky zobrazit, vyberte aktualizaci). Jak ukazuje obrázek 10, má každý střídání produkt barvu světla růžový pozadí.


[![Vytvořte nový soubor vzhledu s názvem DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Obrázek 10**: Vytvořte nový soubor vzhledu název `DataList.skin` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Krok 4: Zkoumat DataList s další šablony

Kromě `ItemTemplate`, prvku DataList podporuje šesti další volitelné šablony:

- `HeaderTemplate`Pokud je zadán, přidá do výstupu řádek záhlaví a slouží k vykreslení tento řádek
- `AlternatingItemTemplate`použít k vykreslení střídání položek
- `SelectedItemTemplate`použít k vykreslení vybrané položce. Vybraná položka je položek, jejichž index odpovídá DataList s [ `SelectedIndex` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`použít k vykreslení upravované položky
- `SeparatorTemplate`Pokud je zadán, přidá oddělovač mezi každou položku a slouží k vykreslení tento oddělovače
- `FooterTemplate`-Pokud je zadán, přidá do výstupu řádek zápatí a slouží k vykreslení tento řádek

Při zadávání `HeaderTemplate` nebo `FooterTemplate`, prvku DataList přidá další řádek záhlaví nebo zápatí stránky vykreslené výstupu. Jako s GridView s záhlaví a zápatí řádků, záhlaví a zápatí v DataList nejsou vázání na data. Proto všechny Syntaxe datové vazby v `HeaderTemplate` nebo `FooterTemplate` , pokusy o přístup k datům vázané vrátí prázdný řetězec.

> [!NOTE]
> Jak jsme viděli v [zobrazení souhrn informací v GridView s zápatí](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) kurzu, zatímco řádky záhlaví a zápatí nejsou zobrazeny t podporu datové vazby syntaxe, informace specifické pro dat můžete vložit přímo do tyto řádky z Rutina GridView s `RowDataBound` obslužné rutiny události. Tento postup lze provádět i výpočtu mezisoučtů nebo Další informace z dat vázáno na ovládací prvek a také zápatí přiřadit tyto informace. Stejný princip lze použít pro ovládací prvky DataList a opakovače; jediným rozdílem je, že pro DataList a opakovače vytvořit obslužnou rutinu události pro `ItemDataBound` událostí (místo pro `RowDataBound` událostí).


Pro náš příklad umožňují s mají název produktu informace zobrazené v horní části DataList s výsledky v `<h3>` záhlaví. Chcete-li dosáhnout, přidejte `HeaderTemplate` odpovídajícími značkami. Z návrháře, můžete to provést klepnutím na odkaz Upravit šablony v DataList s inteligentním, výběr šablony záhlaví z rozevíracího seznamu a zadáním v textu po výběru možnosti záhlaví 3 z rozevíracího seznamu styl seznamu (viz obrázek 11).


[![Přidat HeaderTemplate s informace o produktu textu](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Obrázek 11**: přidání `HeaderTemplate` s informace o produktu Text ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Případně to přidáním deklarativně tak, že zadáte následující kód v rámci `<asp:DataList>` značky:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Přidat bit prostoru mezi každý výpis produktu, umožní s přidat `SeparatorTemplate` řádek mezi každý oddíl, který obsahuje. Vodorovné pravítko značky (`<hr>`), přidá takové oddělovač. Vytvořte `SeparatorTemplate` tak, aby měl následující kód:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Podobně jako `HeaderTemplate` a `FooterTemplates`, `SeparatorTemplate` není vázán na všechny záznamy ze zdroje dat a nemůže proto přímo přístup zdroj dat, záznamy vázána DataList.


Po provedení tohoto přidání při zobrazení stránky prostřednictvím prohlížeče by měl vypadat podobně jako obrázek 12. Poznamenejte si řádek záhlaví a řádek mezi každý výpis produktu.


[![DataList obsahuje řádek záhlaví a vodorovné pravítko mezi každý výpis produktu](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Obrázek 12**: prvku DataList obsahuje řádek záhlaví a vodorovné pravidlo mezi každou produktu výpis ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Krok 5: Vykreslování konkrétní značku s ovládacím prvku Repeater

Pokud uděláte zobrazení/zdroj z prohlížeče při návštěvě příklad DataList z obrázek 12, uvidíte, že DataList vysílá HTML `<table>` řádku tabulky, který obsahuje (`<tr>`) s buňku jedné tabulky (`<td>`) pro každou položku vázána DataList. Tento výstup ve skutečnosti je stejný jako co by vygenerované z GridView s TemplateField jeden. Jak jsme budete vidět v budoucích kurzu, DataList povolit další přizpůsobení výstupu, to nám umožňuje zobrazit více záznamů zdroje dat na řádek tabulky.

Co když neuděláte chcete t emitování HTML `<table>`, ačkoli? Celkový počet a dokončení kontrolu nad značek generované ovládacího prvku webového data musí používáme ovládacím prvku opakovače. Jako DataList opakovače sestavený na základě šablony. Opakovače, ale pouze nabízí následujících pět šablon:

- `HeaderTemplate`Pokud je zadán, přidá zadaný kód před položky
- `ItemTemplate`použít k vykreslení položky
- `AlternatingItemTemplate`Pokud je zadán, použije k vykreslení střídání položek
- `SeparatorTemplate`Pokud je zadán, přidá zadaný kód mezi každou položku
- `FooterTemplate`-Pokud je zadán, přidá zadaný kód za položky

V technologii ASP.NET 1.x, opakovače kontrolu byl nejčastěji používaných k zobrazení seznamu s odrážkami, jejichž data pocházejí z některých zdrojů dat. V takovém případě `HeaderTemplate` a `FooterTemplates` by obsahovala otevření a zavření `<ul>` značky, respektive při `ItemTemplate` by obsahovat `<li>` elementů pomocí syntaxe datové vazby. Tento postup můžete použít stále v technologii ASP.NET 2.0, jako jsme viděli v dva příklady v [hlavní stránky a webové navigace](../introduction/master-pages-and-site-navigation-vb.md) kurzu:

- V `Site.master` stránky předlohy prvku Repeater byl použit k zobrazení seznamu s odrážkami mapa obsahu lokality nejvyšší úrovně (základní vytváření sestav, filtrování sestavy, přizpůsobené formátování a tak dále); jiný, vnořené opakovače byl slouží k zobrazení v částech podřízených prvků nejvyšší úrovně části
- V `SectionLevelTutorialListing.ascx`, prvku Repeater byl použit k zobrazení seznamu s odrážkami v částech podřízené objekty aktuálního oddílu mapy webu

> [!NOTE]
> Technologie ASP.NET 2.0 přináší nové [ovládací prvek BulletedList](https://msdn.microsoft.com/en-us/library/ms228101.aspx), které mohou být vázány na ovládací prvek zdroje dat aby bylo možné zobrazit seznam s odrážkami jednoduché. Pomocí ovládacího prvku BulletedList jsme není nutné zadávat žádné související seznamu HTML; Místo toho jsme jednoduše znamenat pole dat zobrazuje jako text pro každou položku seznamu.


Opakovače slouží jako catch všechna data ovládací prvek webu. Pokud není ovládací prvek existující, který generuje potřebné značky, lze použít ovládacím prvku opakovače. Pro ilustraci použití opakovače, umožní s mít seznamu kategorie zobrazuje nad DataList informace o produktu, vytvořili v kroku 2. Zejména umožňují s mít kategorie zobrazuje ve formátu HTML jednoho řádku `<table>` s každou kategorii zobrazen jako sloupec v tabulce.

K tomu, spusťte tak, že přetáhnete prvku Repeater z panelu nástrojů na návrháře výše DataList informace o produktu. Stejně jako u DataList, opakovače jeho otevření zobrazí jako šedé pole dokud nebyly definovány jeho šablony.


[![Přidání prvku Repeater do návrháře](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Obrázek 13**: přidání prvku Repeater do návrháře ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Zde s jenom jedna možnost v opakovače s inteligentním: Zvolit zdroj dat. OPT vytvořte nové ObjectDataSource a nakonfigurujte ho na použití `CategoriesBLL` třídu s `GetCategories` metoda.


[![Vytvořit nový ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Obrázek 14**: vytvoření nové ObjectDataSource ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![Konfigurace ObjectDataSource použití třídy CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Obrázek 15**: Konfigurace ObjectDataSource pro použití `CategoriesBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Načtení informací o všechny kategorie metodou GetCategories](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Obrázek 16**: načíst informace o všech kategorií použití `GetCategories` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


Na rozdíl od DataList Visual Studio automaticky nevytvoří ItemTemplate pro opakovače po vazby ke zdroji dat. Kromě toho opakovače s šablony nelze nakonfigurovat pomocí návrháře a musí být zadán deklarativně.

K zobrazení kategorií jako jeden řádek `<table>` se sloupcem pro každou kategorii, potřebujeme opakovače pro vydávání značek podobný následujícímu:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Vzhledem k tomu `<td>Category X</td>` text je část, která se opakuje, ta se zobrazí v opakovače s ItemTemplate. Kód, který se zobrazuje před jeho - `<table><tr>` -bude uložena v umístění `HeaderTemplate` při koncová značka - `</tr></table>` -bude uložena v umístění `FooterTemplate`. Pokud chcete zadat nastavení těchto šablon, přejděte na deklarativní část stránky ASP.NET kliknutím na tlačítko zdroj v levém dolním rohu a pak zadejte následující syntaxi:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Opakovače vysílá přesné značek podle specifikace jeho šablony, nic jiného, nic menší. Obrázek 17 se zobrazuje výstup opakovače s při zobrazení prostřednictvím prohlížeče.


[![Jeden řádek HTML &lt;tabulky&gt; uvádí každou kategorii v samotném sloupci](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Obrázek 17**: jeden řádek HTML A `<table>` uvádí každou kategorii v samotném sloupci ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Krok 6: Vylepšení vzhledu opakovače

Vzhledem k tomu, že opakovače vysílá přesněji kód určeného jeho šablony, by měl mít jako žádné neočekávaném, nejsou žádné související se styly vlastnosti pro opakovače. Chcete-li změna vzhledu obsahu generovaného opakovače, jsme musí přidat ručně potřebný obsah HTML nebo šablon stylů CSS přímo do šablon opakovače s.

Pro náš příklad umožní s mít sloupce kategorie alternativní barvy pozadí, třeba s střídavých řádků v prvku DataList. K tomu je potřeba přiřadit `RowStyle` třída šablon stylů CSS na jednotlivé položky opakovače a `AlternatingRowStyle` třída šablon stylů CSS na jednotlivé položky střídání opakovače prostřednictvím `ItemTemplate` a `AlternatingItemTemplate` šablony, například takto:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Umožní s také přidat řádek záhlaví výstupu textem kategorie produktů. Vzhledem k tomu, že jsme nejsou zobrazeny t vědět kolik sloupců naše výsledná `<table>` se skládá, je použití nejjednodušší způsob, jak generovat řádek záhlaví, který zaručeně span všechny sloupce *dva* `<table>` s. První `<table>` bude obsahovat dva řádky v řádku záhlaví a řádek, který bude obsahovat druhé, jeden řádek `<table>` , má sloupec pro každou kategorii v systému. To znamená chceme emitování následující kód:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

Následující `HeaderTemplate` a `FooterTemplate` za následek požadované značky:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

Obrázek 18 ukazuje opakovače po provedly tyto změny.


[![Kategorie sloupce alternativní v barvu pozadí a obsahuje řádek záhlaví](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Obrázek 18**: alternativní sloupce kategorii v barvu pozadí a zahrnuje řádek záhlaví ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Souhrn

Při prvek GridView usnadňuje zobrazení, úpravy, odstranění, řazení a procházet data, vzhled je velmi boxy a mřížky. Pro větší kontrolu nad vzhled musíme zapněte s ovládacími prvky buď DataList nebo opakovače. Obě tyto ovládací prvky zobrazení sadu záznamů pomocí šablony místo BoundFields, CheckBoxFields a tak dále.

DataList vykreslí jako HTML `<table>` , ve výchozím nastavení, zobrazí každý záznam zdroje dat v jedné tabulky řádek, stejně jako GridView s TemplateField jeden. Jak jsme bude vidět v budoucích kurzu, DataList povolit více záznamů, který se má zobrazit na řádek tabulky. Opakovače, na druhé straně výhradně vysílá kód uvedený v jeho šablony; ho nepřidává žádné další značky a proto se běžně používá pro jiné než zobrazení dat v elementů HTML `<table>` (například v seznamu s odrážkami).

Při DataList a opakovače nabízejí větší flexibilitu v jejich vykreslené výstup, nemají mnoho předdefinovaných funkcí v GridView nalezen. Jako podíváme v nadcházející kurzy, některé z těchto funkcí může být zapojen zpět bez příliš mnoho úsilí, ale provést na paměti, používat DataList nebo opakovače místo GridView omezením funkcí, které můžete použít bez nutnosti implementovat tyto funkce sami.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Yaakov Ellis, Liz Shulok, Randy Schmidt a Stacy parku. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](nested-data-web-controls-cs.md)
[další](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
