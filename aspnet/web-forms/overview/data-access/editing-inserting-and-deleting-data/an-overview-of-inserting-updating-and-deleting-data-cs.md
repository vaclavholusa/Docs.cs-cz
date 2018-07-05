---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Přehled vložení, aktualizace a odstranění dat (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu uvidíme, jak namapovat ObjectDataSource Insert(), Update(), a metody Delete() metodám BLL třídy a také jak konfigu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e1b47782bd24824707266d1ed61e24789cc7a49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369835"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Přehled vložení, aktualizace a odstranění dat (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) nebo [stahovat PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> V tomto kurzu uvidíme, jak namapovat ObjectDataSource Insert(), Update(), a Delete() metody do metody BLL třídy, jak se dá nakonfigurovat ovládací prvky GridView DetailsView a FormView k poskytování funkcí změny dat.


## <a name="introduction"></a>Úvod

Za posledních několik kurzů jsme jsme se zaměřili na způsob zobrazení dat na stránce ASP.NET pomocí ovládacích prvků ovládacího prvku GridView, DetailsView a FormView. Tyto ovládací prvky se jednoduše pracovat s daty zadat k nim. Tyto ovládací prvky běžně, přístup k datům prostřednictvím použití prvku zdroje dat, jako je například ObjectDataSource. Zaznamenali jsme, jak prvku ObjectDataSource funguje jako proxy mezi stránky technologie ASP.NET a příslušná data. Když GridView potřebuje k zobrazení dat, vyvolá jeho ObjectDataSource `Select()` metodu, která pak volá metodu z našich obchodní logiky vrstvy (BLL), která volá metodu příslušná Data Access vrstvy (DAL) TableAdapter, který pak odesílá `SELECT` dotaz k databázi Northwind.

Vzpomeňte si, že když jsme vytvořili objekty TableAdapter v DAL v [v našem prvním kurzu](../introduction/creating-a-data-access-layer-cs.md), Visual Studio automaticky přidá metody pro vkládání, aktualizaci, a odstraňování dat z podkladové tabulky databáze. Kromě toho v [vytvoření vrstvy obchodní logiky](../introduction/creating-a-business-logic-layer-cs.md) jsme navrhovali metody v BLL, který volá do těchto metod úpravy DAL data.

Kromě jeho `Select()` metody ObjectDataSource má také `Insert()`, `Update()`, a `Delete()` metody. Podobně jako `Select()` metody tyto tři metody lze mapovat na metody v základní objekt. Když je nakonfigurován ke vložení, aktualizace nebo odstranění dat, ovládací prvky GridView, DetailsView a FormView nabízí uživatelské rozhraní pro úpravu podkladová data. Toto uživatelské rozhraní zavolá `Insert()`, `Update()`, a `Delete()` metody prvku ObjectDataSource, které poté vyvolat základní objekt přidružený k tomuto metody (viz obrázek 1).


[![ObjectDataSource Insert() Update() a Delete() metody slouží jako proxy server do BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Obrázek 1**: prvku ObjectDataSource `Insert()`, `Update()`, a `Delete()` metody slouží jako proxy server do BLL ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


V tomto kurzu uvidíme, jak namapovat ObjectDataSource `Insert()`, `Update()`, a `Delete()` metody metod tříd v BLL, jakož i jak nakonfigurovat ovládací prvky GridView, DetailsView a FormView poskytnout úprava dat Možnosti.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Krok 1: Vytvoření Insert, Update a Delete kurzy webových stránek

Než začneme, jak vkládat, aktualizovat a odstranit data zkoumat, nejprve věnujte chvíli vytvářet stránky technologie ASP.NET v našem projektu webu, které potřebujeme pro tento kurz a další několik předpon. Začněte přidáním novou složku s názvem `EditInsertDelete`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Přidání stránky technologie ASP.NET pro kurzy týkající se změny dat](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Obrázek 2**: Přidání stránky technologie ASP.NET pro kurzy týkající se změny dat


V jiných složkách, jako jsou `Default.aspx` v `EditInsertDelete` složky zobrazí seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` jeho přetažením z Průzkumníka řešení do zobrazení návrhu.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Obrázek 3**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


A konečně, přidejte na stránkách jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za přizpůsobené formátování `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vložení a odstranění kurzy.


![Mapa webu nyní obsahuje záznamy pro úpravy, vložení a odstranění kurzy](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Obrázek 4**: mapy webu nyní obsahuje záznamy pro úpravy, vložení a odstranění kurzy


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Krok 2: Přidání a konfigurace ovládacího prvku ObjectDataSource

Od verze ovládacího prvku GridView, DetailsView a FormView každý se liší v jejich možnosti úprav data a rozložení Podívejme se na každý z nich samostatně. A nenechat každý ovládací prvek používat vlastní prvek ObjectDataSource, ale můžeme stačí vytvořit jeden prvek ObjectDataSource, všechny příklady tří ovládacích prvků můžete sdílet.

Otevřít `Basics.aspx` stránky, přetáhněte z panelu nástrojů do návrháře prvku ObjectDataSource a klikněte na odkaz Konfigurovat zdroj dat z jeho inteligentních značek. Vzhledem k tomu, `ProductsBLL` je jediná BLL třída, která obsahuje úpravy, vložení a odstranění metody, nakonfigurujte prvku ObjectDataSource použít tuto třídu.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Na další obrazovce určíme jaké metody `ProductsBLL` třídy jsou mapované na ObjectDataSource `Select()`, `Insert()`, `Update()`, a `Delete()` vyberete příslušnou kartu a zvolíte metodu z rozevíracího seznamu. Obrázek 6, která by měla vypadat povědomě nyní, mapuje ObjectDataSource `Select()` metodu `ProductsBLL` třídy `GetProducts()` metody. `Insert()`, `Update()`, A `Delete()` metody lze nakonfigurovat tak, že vyberete příslušnou kartu v seznamu nahoře.


[![Prvku ObjectDataSource vrátit všechny produkty](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Obrázek 6**: mají prvku ObjectDataSource vrátit všechny produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Obrázky 7, 8 a 9 zobrazit UPDATE, INSERT a DELETE prvku ObjectDataSource karty. Konfigurace těchto karet tak, aby `Insert()`, `Update()`, a `Delete()` vyvolání metody `ProductsBLL` třídy `UpdateProduct`, `AddProduct`, a `DeleteProduct` metody, v uvedeném pořadí.


[![Map – Metoda Update() ObjectDataSource metodě UpdateProduct ProductBLL třídy](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Obrázek 7**: mapování ObjectDataSource `Update()` metodu `ProductBLL` třídy `UpdateProduct` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Map – metoda Insert() ObjectDataSource metodě AddProduct ProductBLL třídy](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Obrázek 8**: mapování ObjectDataSource `Insert()` metodu `ProductBLL` třídy přidat `Product` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Map – Metoda Delete() ObjectDataSource metodě DeleteProduct ProductBLL třídy](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Obrázek 9**: mapování ObjectDataSource `Delete()` metodu `ProductBLL` třídy `DeleteProduct` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Mohli jste si všimnout, že rozevírací seznamy na kartách UPDATE, INSERT a DELETE již měli tyto metody vybrali. Toto je díky používáme `DataObjectMethodAttribute` , který upraví metody `ProducstBLL`. Například metoda DeleteProduct má následující podpis:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

`DataObjectMethodAttribute` Atribut označuj účel každé metody, ať už jde o pro výběr, vkládání, aktualizace nebo odstranění a určuje, jestli je výchozí hodnota. Pokud tyto atributy vynechán, při vytváření třídy BLL, je budete potřebovat ručně vybrat metody Update, Vložit a odstranit záložky.

Až se ujistíte, že odpovídající `ProductsBLL` metody jsou mapovány na ObjectDataSource `Insert()`, `Update()`, a `Delete()` metody, klikněte na tlačítko Dokončit dokončete průvodce.

## <a name="examining-the-objectdatasources-markup"></a>Zkoumání kódu prvku ObjectDataSource

Po dokončení konfigurace ObjectDataSource prostřednictvím jeho průvodce, přejděte do zobrazení zdroje k prozkoumání vygenerované deklarativní. `<asp:ObjectDataSource>` Značka určuje základní objekt a metody, která se má vyvolat. Kromě toho existují `DeleteParameters`, `UpdateParameters`, a `InsertParameters` , která mapují na vstupní parametry pro `ProductsBLL` třídy `AddProduct`, `UpdateProduct`, a `DeleteProduct` metody:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

Prvku ObjectDataSource obsahuje parametr pro každý vstupní parametry pro její přidružené metody, stejně jako seznam `SelectParameter` s je k dispozici, pokud je prvku ObjectDataSource nakonfigurována pro volání metody select, který očekává, že vstupní parametr (jako je například `GetProductsByCategoryID(categoryID)`). Jak uvidíme krátce, hodnoty pro tyto `DeleteParameters`, `UpdateParameters`, a `InsertParameters` jsou automaticky nastaveny pomocí ovládacího prvku GridView, DetailsView a FormView před vyvoláním ObjectDataSource `Insert()`, `Update()`, nebo `Delete()` Metoda. Tyto hodnoty můžete také nastavit programově, podle potřeby probereme v budoucích kurzech.

Pomocí Průvodce konfigurací ObjectDataSource jeden vedlejším účinkem je, že Visual Studio nastaví [vlastnosti OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) k `original_{0}`. Hodnota této vlastnosti je použít k zahrnutí původní hodnoty dat, který právě upravujete a je užitečné ve dvou scénářích:

- Pokud při úpravě záznamu, uživatelé se můžou změnit hodnotu primárního klíče. V takovém případě nová hodnota primárního klíče a původní hodnotu primárního klíče musí být zadaná tak, aby záznam s původní hodnotu primárního klíče můžete najít a mít hodnotu podle nich aktualizuje.
- Při použití optimistického řízení souběžnosti. Optimistická souběžnost je technika, a zkontrolujte, že dvě současně připojených uživatelů nepřepisujte nepřípustným změny a je téma budoucí kurz.

`OldValuesParameterFormatString` Vlastnost určuje název vstupní parametry v základní objekt aktualizace a metod delete pro původní hodnoty. Tomu se budeme podrobněji tuto vlastnost a její účel podrobněji když budeme věnovat optimistického řízení souběžnosti. Můžu vyvolali ho nyní, ale protože naše BLL metody Nečekejte původní hodnoty a proto je důležité, že jsme odeberte tuto vlastnost. Opuštění `OldValuesParameterFormatString` nastavenou na jinou hodnotu než výchozí (`{0}`) způsobí chybu, když se data webový ovládací prvek pokusí vyvolat ObjectDataSource `Update()` nebo `Delete()` metody vzhledem k tomu, že se prvku ObjectDataSource Pokus o předání v obou `UpdateParameters` nebo `DeleteParameters` zadán jako původní hodnoty parametrů.

Není-li to ale dost vážně zrušte v tomto okamžiku, Nedělejte si starosti, tato vlastnost a její nástroj prozkoumáme v budoucích kurzech. Teď stačí být některé z deklarativní syntaxe zcela odebrat tuto deklaraci vlastnosti nebo nastavte hodnotu na výchozí hodnotu ({0}).

> [!NOTE]
> Pokud jednoduše smažte `OldValuesParameterFormatString` hodnota vlastnosti z okna vlastnosti v okně návrhu, vlastnost zůstanou uchovány deklarativní syntaxe, ale nastavit na prázdný řetězec. To bohužel stále způsobí stejným problémem bylo uvedeno výše. Proto se buď odstranit vlastnost úplně z deklarativní syntaxe, nebo v okně Vlastnosti nastavte hodnotu na výchozí hodnotu, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Krok 3: Přidání ovládacího prvku webových dat a nakonfigurovat jej pro úpravu dat

Když prvku ObjectDataSource byl přidán na stránku a nakonfigurované, jsme připraveni přidat data webové ovládací prvky stránky k zobrazení dat i prostředkem pro koncového uživatele jej upravit. Podíváme na prvku GridView, DetailsView a FormView samostatně, jak tyto webové ovládací prvky dat se liší v jejich funkcí změny dat a konfigurace.

Uvidíme ve zbývající části tohoto článku, přidání velmi základní úpravy, vložení a odstranění podpory prostřednictvím prvku GridView, DetailsView, a řídí FormView je velmi snadné – stačí kontrola několik zaškrtávacích políček. Existuje mnoho odlišností a hraniční případy reálného světa, kterým je poskytování takové funkce zapojí víc než jen pomocí ukázání a kliknutí. Tento kurz se zaměřuje však výhradně na prokázání možnosti úprav zjednodušenou data. Další kurzy prozkoumá, se kterými bude nepochybně nastat při nastavení reálného světa.

## <a name="deleting-data-from-the-gridview"></a>Odstranění dat z prvku GridView.

Začněte přetažením GridView z panelu nástrojů do návrháře. V dalším kroku svázat ObjectDataSource prvku GridView. výběrem z rozevíracího seznamu v prvku GridView inteligentních značek. V tomto okamžiku prvku GridView deklarativní bude:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Vazby prvku GridView ObjectDataSource prostřednictvím jeho inteligentních značek má dvě výhody:

- Pro každé pole vrácené ObjectDataSource automaticky vytvoří BoundFields a CheckBoxFields. Kromě toho vlastnosti BoundField a na třídě CheckBoxField jsou nastaveny na základě metadat podkladové pole. Například `ProductID`, `CategoryName`, a `SupplierName` pole jsou označena jako jen pro čtení v `ProductsDataTable` a proto by neměl být aktualizovat při úpravách. Tak, aby vyhovovaly, tyto BoundFields' [vlastnosti jen pro čtení](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) jsou nastaveny na `true`.
- [Vlastnost DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) přiřazen primárního klíče základního objektu. To je nezbytné při použití prvku GridView pro úpravy nebo odstranění dat, protože tato vlastnost určuje pole (nebo sadu polí), který jedinečný identifikuje každý záznam. Další informace o `DataKeyNames` vlastnost, vraťte se do [Master/Detail pomocí volitelných GridView hlavní DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurzu.

Když prvku GridView mohou být vázány na prvek ObjectDataSource prostřednictvím okna vlastnosti nebo deklarativní syntaxe, to vyžaduje, abyste ručně přidat odpovídající vlastnost BoundField a `DataKeyNames` značek.

Ovládací prvek GridView obsahuje integrovanou podporu pro úpravy na úrovni řádků a odstranění. Konfigurace GridView pro podporu odstranění přidá sloupec tlačítka odstranit. Když koncový uživatel klikne na tlačítko Odstranit pro konkrétního řádku, zpětného odeslání vyplývá a prvku GridView provede následující kroky:

1. Prvku ObjectDataSource `DeleteParameters` jsou přiřazeny hodnoty
2. Prvku ObjectDataSource `Delete()` je vyvolána metoda odstranění zadaný záznam
3. GridView znovu připojí vlastní k ObjectDataSource vyvoláním jeho `Select()` – metoda

Hodnoty přiřazené k `DeleteParameters` jsou hodnoty `DataKeyNames` pole pro řádek došlo ke kliknutí na tlačítko jehož odstranit. Proto je důležité, který prvku GridView `DataKeyNames` správně nastavit vlastnost. Pokud není nalezena, `DeleteParameters` přiřadí `null` hodnotu v kroku 1, která zase nepovede v libovolném odstraní záznamy v kroku 2.

> [!NOTE]
> `DataKeys` Kolekce je uložen v stav ovládacího prvku GridView s, to znamená, že `DataKeys` hodnoty se zachová napříč postback i v případě, že stav zobrazení ovládacího prvku GridView s byla zakázána. Je však velmi důležité, zůstane stav zobrazení prvků GridViews, která podporuje úpravy nebo odstranění (výchozí chování) povolen. Pokud nastavíte GridView s `EnableViewState` vlastnost `false`, úpravy a odstraňování chování bude fungovat pro jednoho uživatele, ale pokud existují souběžných uživatelů odstranění dat, existuje možnost náhodně může tyto souběžných uživatelů odstranění nebo úprava záznamů, kterou kód nefungoval t určené pro instalaci. Zobrazit Moje blogu [upozornění: problém souběžnosti s ASP.NET 2.0 prvků GridViews/DetailsView/FormViews tohoto podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázaná](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), další informace.


Toto upozornění stejným platí také pro DetailsViews a FormViews.

Přidat k GridView odstranění funkce, jednoduše přejděte do jeho inteligentních značek a zaškrtněte políčko Povolit odstranění.


![Zaškrtněte políčko Povolit odstranění zaškrtávací políčko](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Obrázek 10**: Zaškrtněte políčko Povolit odstranění zaškrtávací políčko


Zaškrtnete políčko Povolit odstranění z inteligentních značek přidá CommandField do prvku GridView. Vykreslí CommandField sloupce v prvku GridView. pomocí tlačítka pro provádění jedné nebo více z následujících úloh: výběr záznamu, úpravy záznamu a odstranění záznamu. Jsme viděli dříve CommandField v akci s výběru záznamů [Master/Detail pomocí volitelných GridView hlavní DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurzu.

CommandField obsahuje řadu `ShowXButton` vlastnosti, které označují, jaké sérii tlačítek se zobrazí v CommandField. Zaškrtnutím políčka Povolit odstranění CommandField jehož `ShowDeleteButton` vlastnost `true` byl přidán do kolekce sloupců ovládacího prvku GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

V tomto okamžiku believe to nebo ne jsme hotovi s přidáváním odstranění podpory do prvku GridView! Jak ukazuje obrázek 11 při návštěvě této stránky prostřednictvím prohlížeče sloupec tlačítka Odstranit je k dispozici.


[![CommandField přidá sloupec tlačítka Odstranit](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Obrázek 11**: Přidá sloupce z odstranění tlačítka CommandField ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Pokud jste se vytváříte v tomto kurzu od samotného začátku sami, při testování této stránce kliknutím na tlačítko Odstranit vyvolá výjimku. Pokračujte ve čtení se dozvíte, proč se tyto výjimky vyvolána a jak je opravit.

> [!NOTE]
> Pokud jste postupovali společně pomocí stahování doprovodném tento kurz, již byly zahrnutí pro tyto problémy. Ale můžu vám doporučujeme si podrobnosti níže vám pomůže identifikovat problémy, které mohou vzniknout a vhodné řešení.


Pokud při pokusu o odstranění produktu, dojde k výjimce, jejíž zprávy je podobný "*prvek ObjectDataSource"ObjectDataSource1"nenalezl neobecnou metodu 'DeleteProduct", který obsahuje parametry: productID, původní\_ ProductID*, "zapomněli jste pravděpodobně odebrat `OldValuesParameterFormatString` vlastnost z ObjectDataSource. S `OldValuesParameterFormatString` vlastnost určena, ObjectDataSource pokusí předávání v obou `productID` a `original_ProductID` vstupní parametry pro `DeleteProduct` metody. `DeleteProduct`, ale přijímá pouze jeden vstupní parametr, proto výjimku. Odebírá `OldValuesParameterFormatString` vlastnost (nebo ji nastavíte na `{0}`) dává pokyn k nebude pokoušet a zajistěte tak předání původního vstupního parametru ObjectDataSource.


[![Ujistěte se, že vlastnosti OldValuesParameterFormatString se vymazala navýšení kapacity](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Obrázek 12**: Ujistěte se, že `OldValuesParameterFormatString` vlastnost má byl vymazán navýšení kapacity ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


I v případě, že odstraněn `OldValuesParameterFormatString` vlastnost, stále získáte výjimku při pokusu o odstranění produktu s touto zprávou: "*příkaz DELETE způsobil konflikt s omezením odkaz" FK\_pořadí\_podrobnosti \_Produkty, které*. " Databáze Northwind obsahuje omezení cizího klíče mezi `Order Details` a `Products` tabulky, což znamená, že produkt ze systému nelze odstranit, pokud jeden nebo více záznamů pro něj v `Order Details` tabulky. Vzhledem k tomu, že má každý produkt v databázi Northwind alespoň jeden záznam `Order Details`, nemůžeme odstranit všechny produkty, dokud jsme nejprve odstranit záznamy podrobnosti o produktu přidružené objednávky.


[![Omezení cizího klíče zakazuje odstranění produkty](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Obrázek 13**: omezení pro cizí klíč zakazuje odstranění produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


V našem kurzu teď stačí odstranit všechny záznamy z `Order Details` tabulky. V reálné aplikaci byste musíme buď:

- Máte další obrazovku pro správu informací o podrobnosti objednávky
- Rozšířit `DeleteProduct` tak, aby zahrnoval logiky odstranit zadaný produkt OrderDetails
- Upravit dotaz SQL zahrnují odstranění zadaného produktu OrderDetails pomocí TableAdapter

Teď stačí odstranit všechny záznamy z `Order Details` tabulky pro obejití omezení cizího klíče. Přejděte do Průzkumníka serveru v sadě Visual Studio, klikněte pravým tlačítkem na `NORTHWND.MDF` uzel a vyberte nový dotaz. Potom v okně dotazu spusťte následující příkaz SQL: `DELETE FROM [Order Details]`


[![Odstranit všechny záznamy z tabulky Details pořadí](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Obrázek 14**: Odstraňte všechny záznamy z `Order Details` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Po vymazání navýšení kapacity `Order Details` tabulky kliknutím na tlačítko Odstranit odstraníte produktu bez chyb. Pokud kliknete na tlačítko Odstranit nedojde k odstranění produktu, zkontrolujte, ujistěte se, že prvku GridView `DataKeyNames` je nastavena na pole primárního klíče (`ProductID`).

> [!NOTE]
> Když kliknete na tlačítko Odstranit vyplývá zpětné volání a záznam odstranit. To může být nebezpečné, protože jde snadno omylem kliknete na tlačítko Odstranit nesprávný řádek. V budoucích kurzech uvidíme, jak přidat potvrzení na straně klienta při odstranění záznamu.


## <a name="editing-data-with-the-gridview"></a>Úpravy dat pomocí prvku GridView.

Spolu s odstraněním, ovládací prvek GridView také poskytuje integrovanou podporu úpravy na úrovni řádků. Konfigurace GridView pro podporu úpravy přidá sloupec tlačítka Upravit. Z pohledu koncového uživatele, kliknutím řádek úpravy tlačítko příčiny, které řádek se upravovat zapnutí buňky do textových polí obsahující existující hodnoty a nahrazování tlačítko pro úpravy s aktualizací Update a tlačítka Storno. Po provedení požadované změny, koncový uživatel může klepnout na tlačítko Aktualizovat pro potvrzení změn nebo na tlačítko Storno je zahodit. V obou případech po kliknutí na aktualizaci nebo Storno prvku GridView vrátí do stavu před úprav.

Z naší perspektivy jako vývojář když koncový uživatel klikne na tlačítko Upravit pro konkrétní řádek, vyplývá zpětné volání a prvku GridView provede následující kroky:

1. Prvku GridView `EditItemIndex` vlastnost přiřazen index řádku, jehož tlačítko pro úpravy došlo ke kliknutí na
2. GridView znovu připojí vlastní k ObjectDataSource vyvoláním jeho `Select()` – metoda
3. Index řádku, který odpovídá `EditItemIndex` se vykreslí v "režimu úprav." V tomto režimu tlačítko Upravit nahrazuje tlačítka aktualizace a zrušit a BoundFields jehož `ReadOnly` vlastnosti jsou False (výchozí) jsou vykresleny jako textové pole webové ovládací prvky, jejichž `Text` vlastností jsou přiřazeny hodnoty datová pole.

V tomto okamžiku značky se vrátí do prohlížeče, koncový uživatel měnit data řádku. Když uživatel klikne na tlačítko Aktualizovat, vyvolá zpětné volání a prvku GridView provede následující kroky:

1. Prvku ObjectDataSource `UpdateParameters` hodnoty jsou přiřazeny hodnoty zadané koncovým uživatelem do rozhraní pro úpravy prvku GridView.
2. Prvku ObjectDataSource `Update()` je vyvolána metoda aktualizuje zadaný záznam
3. GridView znovu připojí vlastní k ObjectDataSource vyvoláním jeho `Select()` – metoda

Přiřazené hodnoty primárního klíče `UpdateParameters` v kroku 1 pocházejí z hodnoty zadané v `DataKeyNames` vlastností, zatímco jiné než primární klíče hodnoty pocházejí z textu v ovládacích prvcích TextBox webového upravených řádku. Stejně jako u odstranění, je důležité, který prvku GridView `DataKeyNames` správně nastavit vlastnost. Pokud není nalezena, `UpdateParameters` přiřadí hodnotu primárního klíče `null` hodnota v kroku 1, která zase nepovede v libovolném aktualizovat záznamy v kroku 2.

Úpravy funkce lze aktivovat pouze zaškrtnutím políčka Povolit úpravy v prvku GridView inteligentních značek.


![Zaškrtněte políčko Povolit úpravy zaškrtávací políčko](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Obrázek 15**: Zaškrtněte políčko Povolit úpravy zaškrtávací políčko


Kontrola zaškrtávací políčko Povolit úpravy se přidá CommandField (v případě potřeby) a nastavte jeho `ShowEditButton` vlastnost `true`. Jak jsme viděli dříve, CommandField obsahuje řadu `ShowXButton` vlastnosti, které označují, jaké sérii tlačítek se zobrazí v CommandField. Zaškrtnete políčko Povolit úpravy přidá `ShowEditButton` vlastnost na existující CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

To je vše je k přidání podpory pro základní úpravy. Jak Figure16 ukazuje, je místo hrubého rozhraní úprav každá vlastnost BoundField jehož `ReadOnly` je nastavena na `false` (výchozí) je vykreslen jako textové pole. To zahrnuje pole, jako jsou `CategoryID` a `SupplierID`, které jsou klíčů s jinými tabulkami.


[![Kliknutím na tlačítko pro úpravy s Chai zobrazí řádek v režimu úprav](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Obrázek 16**: zobrazí s Chai kliknutím na tlačítko pro úpravy řádku v režimu úprav ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Kromě vyzývají, chcete-li upravit hodnoty cizího klíče přímo, chybí rozhraní úprav rozhraní následujícími způsoby:

- Pokud uživatel zadá `CategoryID` nebo `SupplierID` , který neexistuje v databázi, `UPDATE` bude porušovat omezení cizího klíče, způsobí vyvolání výjimky.
- Úpravy rozhraní neobsahuje žádné ověřování. Pokud nezadáte povinnou hodnotu (například `ProductName`), nebo zadejte hodnotu řetězce, kde je očekávána číselnou hodnotu (jako je například zadání "Příliš mnoho!" do `UnitPrice` textové pole), bude vyvolána výjimka. Budoucí kurz se zaměřuje na přidání validačních ovládacích prvků uživatelského rozhraní pro úpravy.
- V současné době *všechny* pole produktů, které nejsou jen pro čtení musí být součástí prvku GridView. Pokud nám chcete pole odebrat z prvku GridView, Řekněme, že `UnitPrice`, že při aktualizaci dat prvku GridView. nenastavujte `UnitPrice` `UpdateParameters` hodnotu, která by záznamů databáze změnit `UnitPrice` k `NULL` hodnotu. Podobně, pokud povinné pole, jako například `ProductName`, se odebere z prvku GridView, se aktualizace nezdaří se stejným "*'ProductName' sloupec nepovoluje hodnoty Null*" výše uvedené výjimce.
- Úpravy rozhraní formátování opustí být požadovaného mnoho dalších. `UnitPrice` Se zobrazí s čtyři desetinné čárky. V ideálním případě `CategoryID` a `SupplierID` hodnoty by obsahovat DropDownLists, který seznam kategorií a dodavatele v systému.

Jedná se o všechny nedostatky, které nám budete muset za provozu se teď ale budou řešit v budoucích kurzech.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Vložení, úpravy a odstraňování dat ovládacím prvkem ovládacím prvku DetailsView.

Jak jsme viděli v předchozích kurzech se ovládacím prvku DetailsView. ovládací prvek zobrazí jeden záznam v čase a jako prvku GridView, umožňuje úpravy a odstranění aktuálně zobrazené záznamu. Prostředí obou koncového uživatele s úpravy a odstraňování položek ze DetailsView a pracovní postup ze strany technologie ASP.NET je stejný jako u prvku GridView. Kde se liší od prvku GridView ovládacím prvku DetailsView je, že také poskytuje integrovanou podporu vkládání.

Abychom si předvedli možnosti úprav dat prvku GridView, začněte přidáním prvku DetailsView k `Basics.aspx` stránce nad existujícího ovládacího prvku GridView a jeho vazbu na existující prvek ObjectDataSource prostřednictvím inteligentních značek v ovládacím prvku DetailsView. Další vymazání ovládacím prvku DetailsView `Height` a `Width` vlastnosti a zaškrtněte možnost Povolit stránkování z inteligentních značek. Aby se povolily úpravy, vložení a odstranění podpory, stačí zaškrtnout políčka Povolit úpravy, Povolit vložení a povolení odstranění v inteligentních značek.


![Konfigurace ovládacím prvku DetailsView. k podpoře úpravy, vložení a odstranění](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Obrázek 17**: Konfigurace ovládacím prvku DetailsView. k podpoře úpravy, vložení a odstranění


Jako s použitím prvku GridView, přidání, úpravy, vložení nebo odstranění podpory přidá CommandField do ovládacího prvku DetailsView, jak ukazuje následující deklarativní syntaxe:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Všimněte si, že prvku DetailsView CommandField se zobrazí na konci kolekce sloupců ve výchozím nastavení. Protože ovládacím prvku DetailsView pole jsou vykresleny jako řádky, CommandField představována jedním řádkem s vloženým, upravovat a odstraňovat tlačítek v dolní části ovládacím prvku DetailsView.


[![Konfigurace ovládacím prvku DetailsView. k podpoře úpravy, vložení a odstranění](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Obrázek 18**: Konfigurace ovládacím prvku DetailsView. pro podporu úpravy, vložení a odstranění ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Stejně jako u prvku GridView. Kliknutím na tlačítko Odstranit spustí stejnou posloupnost událostí: a postbacku; Následuje ovládacím prvku DetailsView naplnění jeho ObjectDataSource `DeleteParameters` na základě `DataKeyNames` hodnoty; a dokončit pomocí volání jeho ObjectDataSource `Delete()` metodu, která ve skutečnosti produktu odebere z databáze. Úpravy v ovládacím prvku DetailsView také funguje způsobem, který je stejný jako u prvku GridView.

Pro vkládání, koncový uživatel předloží nové tlačítko, které, po kliknutí na vykreslí ovládacím prvku DetailsView v "režimu vkládání." S "režimu vkládání" nové tlačítko nahrazuje tlačítka Vložit a zrušit a pouze ty BoundFields jehož `InsertVisible` je nastavena na `true` (výchozí) se zobrazí. Tato data pole identifikována jako pole Automatické zvyšování čísla, jako například `ProductID`, mají jejich [InsertVisible vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) nastavena na `false` při vytváření vazby ovládacím prvku DetailsView ke zdroji dat prostřednictvím inteligentních značek.

Při vytváření vazby zdroje dat k prvku DetailsView prostřednictvím inteligentních značek, Visual Studio nastaví `InsertVisible` vlastnost `false` pouze u polí s automatickým krokem. Pole jen pro čtení, jako jsou `CategoryName` a `SupplierName`, se zobrazí v uživatelském rozhraní "režimu vkládání", pokud jejich `InsertVisible` je explicitně nastavena na `false`. Za chvíli nastavit tyto dvě pole `InsertVisible` vlastností `false`, buď prostřednictvím ovládacím prvku DetailsView deklarativní syntaxe nebo upravit pole na odkaz v inteligentních značek. Obrázek 19 zobrazuje nastavení `InsertVisible` vlastností `false` kliknutím na Upravit pole na odkaz.


[![Northwind Traders teď nabízí Acme čaje](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Obrázek 19**: Northwind Traders teď nabízí Acme čaje ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Po nastavení `InsertVisible` vlastnosti, zobrazení `Basics.aspx` stránku v prohlížeči a klikněte na tlačítko Nový. Obrázek 20 ukazuje ovládacím prvku DetailsView. při přidávání nové nápoje Acme čaje naší řadě produktů.


[![Northwind Traders teď nabízí Acme čaje](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Obrázek 20**: Northwind Traders teď nabízí Acme čaje ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Po zadání podrobností pro Acme čaj a kliknutím na tlačítko pro vložení, vyplývá zpětné volání a přidá nový záznam `Products` databázové tabulky. Od tohoto prvku DetailsView zobrazuje seznam produktů v pořadí, které existují v tabulce databáze, jsme musí na poslední stránce produktu Chcete-li zobrazit nový produkt.


[![Podrobnosti o Acme čaje](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Obrázek 21**: Podrobnosti o Acme čaje ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> Ovládacím prvku DetailsView [CurrentMode vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) označuje rozhraní se zobrazí a může být jedna z následujících hodnot: `Edit`, `Insert`, nebo `ReadOnly`. [Vlastnost DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) označuje režim ovládacím prvku DetailsView vrátí po úpravy nebo vložení bylo dokončeno a je užitečné pro zobrazení DetailsView, který je trvale v režimu úpravy nebo režimu vkládání.


Bod a klikněte na vkládání a možností prvku DetailsView úprav trpí stejná omezení jako prvku GridView: uživatel musí zadat existující `CategoryID` a `SupplierID` hodnoty prostřednictvím textové pole; chybí rozhraní jakékoli logiky ověřování; všechny pole produktů, které nejsou povoleny `NULL` hodnoty nebo nemají výchozí hodnotu zadanou na úrovni databáze, musí být součástí vkládání rozhraní a tak dále.

Techniky prozkoumáme pro rozšíření a vylepšení prvku GridView úpravy rozhraní v budoucích článků lze použít u prvku DetailsView úpravy a vložení a rozhraní.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Pomocí FormView flexibilnější Data změny uživatelského rozhraní

FormView nabízí vestavěnou podporu pro vkládání, úpravy a odstraňování dat, ale protože používá šablony místo pole neexistuje žádné místo pro přidání BoundFields nebo CommandField používané ovládací prvky GridView a prvku DetailsView. k získání dat úpravy rozhraní. Místo toho tato rozhraní, webové ovládací prvky pro shromažďování uživatelského vstupu při přidání nové položky nebo úpravou existující spolu s nový, upravit, odstranit, Insert, Update a stornovací tlačítka musí ručně přidat do příslušných šablon. Naštěstí sady Visual Studio automaticky vytvoří požadované rozhraní při vytváření vazby FormView ke zdroji dat pomocí rozevíracího seznamu v jeho inteligentních značek.

Pro ilustraci těchto technik, začněte přidáním FormView k `Basics.aspx` stránky a z ovládacího prvku FormView inteligentních značek, jeho vazbu na ObjectDataSource už vytvořili. Tím se vygeneruje `EditItemTemplate`, `InsertItemTemplate`, a `ItemTemplate` pro třídy FormView pomocí textového pole ovládacích prvků pro shromažďování vstupu uživatele a tlačítko webové ovládací prvky pro nový, upravit, odstranit, Insert, Update a stornovací tlačítka. Kromě toho FormView `DataKeyNames` je nastavena na pole primárního klíče (`ProductID`) objektů vrácené objektem ObjectDataSource. A konečně zaškrtněte možnost Povolit stránkování ve třídě FormView inteligentních značek.

Následující příklad zobrazuje deklarativní pro ovládacího prvku FormView `ItemTemplate` po FormView byla svázána se ObjectDataSource. Ve výchozím nastavení, je každý produkt pole hodnoty – datový typ Boolean vázán na `Text` vlastnost ovládacího prvku popisku webového při každé pole logická hodnota (`Discontinued`) je vázán na `Checked` vlastnost zablokovaný ovládací prvek zaškrtávací políčko Web. Aby aktivovat určité FormView chování při kliknutí na tlačítka Nový, Edit a Delete, je nutné, jejich `CommandName` nastavit hodnoty na `New`, `Edit`, a `Delete`v uvedeném pořadí.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

Obrázek 22 ukazuje ovládacího prvku FormView `ItemTemplate` při prohlížení prostřednictvím prohlížeče. Každé pole produktu je uvedený s tlačítka Nový, Edit a Delete v dolní části.


[![ItemTemplate Defaut FormView obsahuje seznam všech polí produktů spolu s nové, upravovat a odstraňovat tlačítka](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Obrázek 22**: třídy Defaut FormView `ItemTemplate` uvádí každý produkt pole společně s nové, úpravy a odstranění tlačítka ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Třeba v prvku GridView a DetailsView, kliknutím na tlačítko Odstranit nebo jakékoli tlačítko, odkazem (LinkButton) nebo ImageButton jehož `CommandName` vlastnost je nastavena na odstranění příčiny zpětné volání, naplní ObjectDataSource `DeleteParameters` podle FormView `DataKeyNames`hodnotu a vyvolá ObjectDataSource `Delete()` metody.

Po kliknutí na tlačítko Upravit vyplývá zpětné volání a dat je znovu připojeno k `EditItemTemplate`, který je zodpovědný za vykreslování rozhraní úprav. Toto rozhraní zahrnuje webové ovládací prvky pro úpravy dat spolu s tlačítka aktualizace a zrušit. Výchozí hodnota `EditItemTemplate` generovaný sady Visual Studio obsahuje popisek pro všechna pole s automatickým krokem (`ProductID`), textového pole pro každé pole hodnota – datový typ Boolean a zaškrtávací políčko u každého pole, logická hodnota. Toto chování je velmi podobný BoundFields automaticky generované v ovládacích prvcích ovládacími prvky GridView a prvku DetailsView.

> [!NOTE]
> Jeden menšímu problému se ovládacího prvku FormView automatické generování `EditItemTemplate` je, že se vykresluje textové pole webové ovládací prvky pro těchto polí, která jsou jen pro čtení, například `CategoryName` a `SupplierName`. Uvidíme, jak to za chvíli.


Ovládací prvky v textovém poli `EditItemTemplate` mají jejich `Text` vlastnost vázána na hodnoty jejich odpovídajících dat pole pomocí *dvousměrnou datovou vazbou*. Obousměrná vazba dat udávají `<%# Bind("dataField") %>`, provádí vázání dat i při vytváření vazby dat k šabloně a při naplňování ObjectDataSource parametry pro vložení nebo úprava záznamů. To znamená, když uživatel klepne na tlačítko Upravit z `ItemTemplate`, `Bind()` metoda vrátí hodnotu zadané datové pole. Až uživatel provede jejich změny a klikne na tlačítko aktualizace, hodnoty odeslána zpět, která odpovídají datových polí zadat pomocí `Bind()` aplikují i na ObjectDataSource `UpdateParameters`. Můžete také jednosměrnou vázání dat udávají `<%# Eval("dataField") %>`, pouze načte pole hodnoty dat při vytváření vazby dat k šabloně a nemá *není* návratové hodnoty uživatel zadal pro zdroj dat parametry na zpětné volání.

Následující kód ukazuje ovládacího prvku FormView `EditItemTemplate`. Všimněte si, že `Bind()` metoda se používá v syntaxe vázání dat, obsahující ovládací prvky tlačítka Update a zrušit jejich `CommandName` vlastnosti nastavené odpovídajícím způsobem.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Naše `EditItemTemplate`, v tomto bodu, způsobí výjimku, která je vyvolána, pokud jsme pokusí ji použít. Problém je, že `CategoryName` a `SupplierName` pole vykreslovat jako ovládací prvky TextBox webového v `EditItemTemplate`. Potřebujeme buď změnit těchto textových polí na popisky nebo je úplně odeberte. Teď je zcela v jednoduše odstranit `EditItemTemplate`.

Po kliknutí na tlačítko Upravit pro Chai 23 obrázek ukazuje FormView v prohlížeči. Všimněte si, že `SupplierName` a `CategoryName` pole zobrazená v `ItemTemplate` už nejsou k dispozici, protože právě odebrali jsme z `EditItemTemplate`. Po kliknutí na tlačítko Aktualizovat pokračuje FormView přes stejnou posloupnost kroků jako ovládací prvky GridView a prvku DetailsView.


[![Ve výchozím nastavení EditItemTemplate zobrazuje jednotlivá pole upravitelné produktu jako textové pole nebo zaškrtávacího políčka](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Obrázek 23**: ve výchozím nastavení `EditItemTemplate` ukazuje, každý upravitelné pole produktu jako textové pole nebo zaškrtávacího políčka ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Při klepnutí na tlačítko Vložit ovládacího prvku FormView `ItemTemplate` vyplývá zpětného odeslání. Žádná data však je vázán na třídě FormView, protože se přidává nový záznam. `InsertItemTemplate` Rozhraní zahrnuje webové ovládací prvky pro přidání nového záznamu společně s tlačítka Vložit a zrušit. Výchozí hodnota `InsertItemTemplate` generovaný sady Visual Studio obsahuje textové pole pro každé pole hodnota – datový typ Boolean a zaškrtávací políčko pro každé pole logická hodnota, podobně jako automaticky generovaný `EditItemTemplate`v rozhraní. TextBox – ovládací prvky mají jejich `Text` vlastnost vázána na hodnotu odpovídající datové pole pomocí dvousměrnou datovou vazbou.

Následující kód ukazuje ovládacího prvku FormView `InsertItemTemplate`. Všimněte si, že `Bind()` metoda se používá v syntaxi databinding tady a vložení a zrušit tlačítko webové ovládací prvky mají jejich `CommandName` vlastnosti nastavené odpovídajícím způsobem.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Existuje subtlety pomocí ovládacího prvku FormView automatické generování `InsertItemTemplate`. Konkrétně vytvoření ovládacích prvků TextBox webového i pro těchto polí, která jsou jen pro čtení, například `CategoryName` a `SupplierName`. Jak je `EditItemTemplate`, potřebujeme k odebrání těchto textových polí z `InsertItemTemplate`.

Obrázek 24 FormView zobrazuje v prohlížeči při přidání nového produktu, Acme kávu. Všimněte si, že `SupplierName` a `CategoryName` pole zobrazená v `ItemTemplate` už nejsou k dispozici, protože jsme právě odebrali. Při kliknutí na tlačítko Vložit pokračuje FormView prostřednictvím stejné pořadí kroků jako ovládací prvek DetailsView, přidání nového záznamu do `Products` tabulky. Obrázek 25 zobrazuje podrobnosti o produktu Acme kávy ve třídě FormView po byla vložena.


[![InsertItemTemplate určuje rozhraní vložení ovládacího prvku FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Obrázek 24**: `InsertItemTemplate` určuje rozhraní vložení ovládacího prvku FormView ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Podrobnosti o nového produktu, Acme kávy, se zobrazí ve třídě FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Obrázek 25**: The podrobnosti nového produktu, Acme kávy, se zobrazí ve třídě FormView ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Tak, že oddělíte si jen pro čtení, úpravy a vložení rozhraní tři samostatné šablony FormView umožňuje jemnější stupeň kontroly nad tato rozhraní než DetailsView a ovládacího prvku GridView.

> [!NOTE]
> Například, FormView prvku DetailsView `CurrentMode` určuje vlastnost rozhraní se zobrazí a jeho `DefaultMode` vlastnost označuje režim FormView vrátí po úpravy nebo vložení bylo dokončeno.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na základní informace o vložení, úpravy a odstranění dat pomocí ovládacího prvku GridView, DetailsView a FormView. Všechny tři z těchto ovládacích prvků poskytovat určitou úroveň předdefinované datové možnosti úprav, které můžete používat, aniž byste museli napsat jediný řádek kódu na stránce ASP.NET díky webové ovládací prvky dat a ObjectDataSource. Jednoduché však bodu a klikněte na poměrně frail vykreslení techniky a naivní data změny uživatelského rozhraní. Pro ověřování, vložení hodnoty prostřednictvím kódu programu, bez výpadku zpracovávat výjimky, přizpůsobení uživatelského rozhraní a atd., budeme muset spoléhat na bevy technik, které probereme v další kurzy několik.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
