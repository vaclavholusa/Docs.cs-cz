---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: "Přehled vkládání, aktualizaci a odstraňování dat (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu ukážeme, jak namapovat ObjectDataSource Insert(), Update(), a metody Delete() metody BLL třídy a také jak configu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 99d6b98bb7efa2f63e0c19b8623fd42ed92bdbaf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Přehled vkládání, aktualizaci a odstraňování dat (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) nebo [stáhnout PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> V tomto kurzu ukážeme, jak namapovat ObjectDataSource Insert(), Update(), a metody Delete() metody BLL třídy a také konfiguraci ovládacích prvků GridView DetailsView a FormView zajistit možnosti úprav data.


## <a name="introduction"></a>Úvod

V posledních několika kurzy jste jsme se zaměřili jak zobrazit data na stránku ASP.NET pomocí ovládacích prvků GridView DetailsView a FormView. Tyto ovládací prvky se pouze pracovat s daty zadaný na ně. Tyto ovládací prvky běžně, přístup k datům prostřednictvím ovládací prvek zdroje dat, jako je například ObjectDataSource. Jste viděli, jak ObjectDataSource funguje jako server proxy mezi stránku ASP.NET a základní data. Když GridView potřebuje k zobrazení dat, vyvolá jeho ObjectDataSource `Select()` metodu, která zase volá metodu z našich obchodní logiky vrstvy (BLL), která volá metodu v příslušné Data přístup vrstvy (DAL) TableAdapter, které následně odešle `SELECT` dotaz pro databázi Northwind.

Odvolat, když jsme vytvořili TableAdapters v DAL v [naše první kurz](../introduction/creating-a-data-access-layer-cs.md), Visual Studio automaticky přidá metody pro vložení, aktualizaci, a odstraňuje data ze základní tabulky databáze. Kromě toho v [vytváření vrstvu obchodní logiky](../introduction/creating-a-business-logic-layer-vb.md) jsme chtěli metody v BLL, který volá do těchto metod úpravy DAL data.

Kromě jeho `Select()` metody ObjectDataSource má také `Insert()`, `Update()`, a `Delete()` metody. Podobně jako `Select()` metoda, tyto tři metody lze mapovat na metody v základní objekt. Při konfiguraci vložit, aktualizovat nebo odstranit data, ovládací prvky GridView DetailsView a FormView nabízí uživatelské rozhraní pro úpravy základní data. Toto uživatelské rozhraní volá `Insert()`, `Update()`, a `Delete()` ObjectDataSource, metody, které pak vyvolání základní objekt přidruženému metody (viz obrázek 1).


[![ObjectDataSource Insert(), Update() a Delete() metody slouží jako proxy server do BLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Obrázek 1**: The ObjectDataSource `Insert()`, `Update()`, a `Delete()` metody slouží jako proxy server do BLL ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


V tomto kurzu ukážeme, jak namapovat ObjectDataSource `Insert()`, `Update()`, a `Delete()` metody pro metody třídy BLL také s postupy pro konfiguraci ovládacích prvků GridView DetailsView a FormView zajistit úprava dat Možnosti.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Krok 1: Vytvoření Insert, Update a Delete kurzy webové stránky

Jsme mohli začít seznamovat vložení, aktualizaci a odstranění dat, můžeme nejprve za chvíli vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro tento kurz a další několik podsítí. Nejprve přidejte novou složku s názvem `EditInsertDelete`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Přidání stránky ASP.NET pro kurzy týkající se úpravy dat](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**Obrázek 2**: Přidání stránky ASP.NET pro kurzy týkající se úpravy dat


V jiných složkách, jako `Default.aspx` v `EditInsertDelete` složky zobrazí seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení do zobrazení návrhu stránky.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Obrázek 3**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


Nakonec přidejte stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po přizpůsobit formátování `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro úpravy, vkládání a odstraňování kurzy.


![Mapy webu nyní obsahuje položky pro úpravy, vkládání a odstraňování kurzy](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Obrázek 4**: mapy webu nyní obsahuje položky pro úpravy, vkládání a odstraňování kurzy


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Krok 2: Přidání a konfigurace ovládacího prvku ObjectDataSource

Protože rutina GridView DetailsView a FormView každý se liší v jejich funkce úpravu dat a rozložení Podívejme se na každé z nich jednotlivě. Místo mít každý ovládací prvek pomocí vlastní ObjectDataSource, ale právě vytvoříme jednoho prvku ObjectDataSource, můžete sdílet všechny příklady tři ovládacích prvků.

Otevřete `Basics.aspx` stránky, přetáhněte ObjectDataSource z panelu nástrojů na návrháře a klikněte na odkaz Konfigurace zdroje dat z jeho inteligentních značek. Vzhledem k tomu `ProductsBLL` je jediná třída BLL, která poskytuje, úprava, vkládání a odstraňování metody, nakonfigurujte ObjectDataSource k použití této třídy.


[![Konfigurace ObjectDataSource použití třídy ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


Na další obrazovce určíme jaké metody `ProductsBLL` třídy jsou namapované na ObjectDataSource `Select()`, `Insert()`, `Update()`, a `Delete()` vyberte příslušnou kartu a vybrat metodu, která z rozevíracího seznamu. Obrázek 6, která by měla vypadat povědomě nyní, se mapuje ObjectDataSource `Select()` metodu `ProductsBLL` třídy `GetProducts()` metoda. `Insert()`, `Update()`, A `Delete()` metody lze nakonfigurovat tak, že vyberete příslušnou kartu ze seznamu podél horního.


[![Máte ObjectDataSource vrátit všechny produkty](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Obrázek 6**: mít v ObjectDataSource vrátit všechny produkty ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


Obrázky 7, 8 a 9 zobrazit UPDATE, INSERT a DELETE ObjectDataSource karty. Konfigurace těchto karet tak, aby `Insert()`, `Update()`, a `Delete()` vyvolání metody `ProductsBLL` třídy `UpdateProduct`, `AddProduct`, a `DeleteProduct` metody, v uvedeném pořadí.


[![Map – Metoda Update() ObjectDataSource metodě UpdateProduct ProductBLL – třída](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Obrázek 7**: mapovat ObjectDataSource `Update()` metodu `ProductBLL` třídy `UpdateProduct` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![Map – metoda Insert() ObjectDataSource metodě AddProduct ProductBLL – třída](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Obrázek 8**: mapovat ObjectDataSource `Insert()` metodu `ProductBLL` třídy přidat `Product` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![Map – Metoda Delete() ObjectDataSource metodě DeleteProduct ProductBLL – třída](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Obrázek 9**: mapování ObjectDataSource `Delete()` metodu `ProductBLL` třídy `DeleteProduct` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


Jste si všimli, rozevírací seznamy na kartách UPDATE, INSERT a DELETE již obsahuje tyto metody vybrané. Toto je díky naše používání `DataObjectMethodAttribute` , upraví metody `ProducstBLL`. Například metoda DeleteProduct má následující podpis:


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute` Atribut určuje účel jednotlivých metod toho, jestli je pro výběr, vkládání, aktualizaci, nebo odstranit a zda je výchozí hodnota. Pokud se tyto atributy se vynechá, při vytváření tříd BLL, je budete potřebovat ručně vybrat metodu z aktualizace, Vložit a odstranit karty.

Po zajištění, že odpovídající `ProductsBLL` metody jsou namapované na ObjectDataSource `Insert()`, `Update()`, a `Delete()` metody, klikněte na tlačítko Dokončit dokončete průvodce.

## <a name="examining-the-objectdatasources-markup"></a>Zkoumání kódu ObjectDataSource

Po dokončení konfigurace ObjectDataSource pomocí jeho průvodce, přejděte do zobrazení zdroje prozkoumat vygenerovaný kód. `<asp:ObjectDataSource>` Značky určuje základní objekt a metody k vyvolání. Kromě toho existují `DeleteParameters`, `UpdateParameters`, a `InsertParameters` , mapování na vstupní parametry `ProductsBLL` třídy `AddProduct`, `UpdateProduct`, a `DeleteProduct` metody:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource zahrnuje parametr pro každý vstupní parametry pro jeho související metody, stejně jako seznam `SelectParameter` je přítomen, pokud je nakonfigurován ObjectDataSource k volání vyberte metodu, která očekává, že vstupní parametr (například `GetProductsByCategoryID(categoryID)`). Jak jsme krátce, zobrazí hodnoty pro tyto `DeleteParameters`, `UpdateParameters`, a `InsertParameters` automaticky nastaveny tak, že rutina GridView, DetailsView a FormView před vyvoláním ObjectDataSource `Insert()`, `Update()`, nebo `Delete()` Metoda. Tyto hodnoty lze také nastavit prostřednictvím kódu programu, podle potřeby jako probereme v budoucnu kurzu.

Pomocí Průvodce nakonfigurovat tak, aby ObjectDataSource jeden vedlejším účinkem je, že nastaví Visual Studio [OldValuesParameterFormatString vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) k `original_{0}`. Hodnota této vlastnosti se používá k obsahovat původní hodnoty dat Upravovaný a je užitečná ve dvou scénářích:

- Pokud při úpravě záznam, mohou uživatelé změnit hodnotu primárního klíče. V takovém případě novou hodnotu primárního klíče a původní hodnotu primárního klíče je třeba zadat tak, aby záznam s původní hodnotu primárního klíče můžete najít a jeho hodnota příslušným způsobem aktualizuje.
- Při použití optimistickou metodu souběžného. Optimistickou metodu souběžného zpracování je technika zajistit, aby dva současně připojených uživatelů není přepsat své změny a je v tématu pro budoucí kurz.

`OldValuesParameterFormatString` Vlastnost určuje název vstupní parametry v základní objekt aktualizace a odstranění metody pro původní hodnoty. Když jsme prozkoumat optimistickou metodu souběžného najdete zde popis této vlastnosti a jeho účel podrobněji. I zprovoznit ho nyní, ale protože naše BLL metody Nečekejte původní hodnoty, a proto je důležité, že jsme odebrat tuto vlastnost. Ponechat `OldValuesParameterFormatString` vlastností nastavenou na jakoukoli jinou hodnotu než výchozí (`{0}`) dojde k chybě, když se data ovládací prvek webu pokusí vyvolání ObjectDataSource `Update()` nebo `Delete()` metody vzhledem k tomu, že bude ObjectDataSource Pokus o předání v obou `UpdateParameters` nebo `DeleteParameters` zadaný a také parametry s původní hodnotou.

Pokud to není terribly zrušte v této situaci řešit, nemusíte si dělat starosti, podíváme tuto vlastnost a její nástroj budoucí kurzu. Prozatím se právě být některé z deklarativní syntaxi zcela odebrat tuto deklaraci vlastnost nebo nastavte hodnotu na výchozí hodnotu ({0}).

> [!NOTE]
> Pokud zrušíte jednoduše `OldValuesParameterFormatString` hodnotu vlastnosti z okna vlastností v zobrazení návrhu, vlastnost bude stále existovat v deklarativní syntaxi, ale nastavit na prázdný řetězec. Bohužel stále výsledkem bude ke stejnému problému výše popsané. Proto, buď odeberte vlastnost zcela od deklarativní syntaxi nebo, v okně Vlastnosti, nastavte hodnotu na výchozí, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Krok 3: Přidání ovládacího prvku webového dat a nakonfigurovat jej pro úpravu dat

Jakmile ObjectDataSource je přidán na stránku a nakonfigurovaná, jsme připravení přidat data ovládací prvky webového na stránku a jak zobrazit data a obsahují prostředky pro koncovému uživateli umožňují upravit. Podíváme GridView, DetailsView a FormView samostatně, protože tyto ovládací prvky webového dat se liší v jejich funkce úpravu dat a konfigurace.

Jak jsme se zobrazí ve zbývající části tohoto článku, přidání velmi základní úpravy, vkládání a odstraňování podpory prostřednictvím rutina GridView DetailsView, a řídí FormView je pouhé kontrola několik zaškrtávacích políček. Existuje mnoho odlišnosti a hraniční případů v reálného prostředí, které poskytuje tyto funkce složitější než právě bodu a klikněte na tlačítko. Tento kurz se zaměřuje však výhradně na prokázání možnosti úprav zneužívající vlastností prohlížeče data. Budoucí kurzy prozkoumá otázky, které se vynoří nepochybně v reálných nastavení.

## <a name="deleting-data-from-the-gridview"></a>Odstranění dat z GridView

Spustit tak, že přetáhnete GridView z panelu nástrojů na návrháře. V dalším kroku vytvořit vazbu ObjectDataSource GridView vyberete v rozevíracím seznamu v prvku GridView inteligentních značek. V tomto okamžiku rutina GridView deklarativní bude:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

Vytvoření vazby GridView ObjectDataSource prostřednictvím jeho inteligentních značek má dvě výhody:

- BoundFields a CheckBoxFields se vytvářejí automaticky pro každé pole vrácené ObjectDataSource. Kromě toho BoundField a pro vlastnost CheckBoxField vlastnosti jsou nastaveny na základě pole podkladové metadat. Například `ProductID`, `CategoryName`, a `SupplierName` pole jsou označeny jako jen pro čtení v `ProductsDataTable` a proto by nemělo být aktualizovat při úpravě. Aby bylo možné ošetřit tento, tyto BoundFields' [vlastnosti jen pro čtení](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) jsou nastaveny na `True`.
- [Vlastnost DataKeyNames](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) je přiřazena k primárního klíče podkladového objektu. To je nezbytné při použití GridView pro úpravy nebo odstranění dat, protože tato vlastnost určuje pole (nebo sady polí) to jedinečný identifikuje každý záznam. Další informace o `DataKeyNames` vlastnost odkazovat zpět [hlavní/podrobností volitelný GridView hlavní pomocí DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurzu.

Při GridView mohou být vázány na ObjectDataSource prostřednictvím vlastnosti – okno nebo deklarativní syntaxe, tak vyžaduje, abyste ručně přidejte příslušné BoundField a `DataKeyNames` značek.

Ovládací prvek GridView poskytuje integrovanou podporu pro úpravy na úrovni řádků a odstranění. Rutina GridView pro podporu odstranění konfigurace přidá sloupec tlačítek odstranit. Když koncový uživatel klikne na tlačítko Odstranit pro konkrétního řádku, vyplývá zpětné volání a GridView provede následující kroky:

1. ObjectDataSource `DeleteParameters` přiřazené hodnoty
2. ObjectDataSource `Delete()` metoda je volána, odstraňování zadaný záznam
3. GridView znovu připojí samotné k ObjectDataSource vyvoláním jeho `Select()` – metoda

Hodnot přiřazených `DeleteParameters` jsou hodnoty `DataKeyNames` polí pro řádek označeného jehož tlačítko Odstranit. Proto je důležité, rutina GridView `DataKeyNames` vlastnost správně nastavit. Pokud není nalezena, `DeleteParameters` přiřadí hodnotu `Nothing` v kroku 1, pak nebude důsledkem toho odstraněné záznamy v kroku 2.

> [!NOTE]
> `DataKeys` Kolekce je uložen v GridView s řízení stavu, znamená to, že `DataKeys` hodnoty se uloží napříč zpětné volání, i v případě, že stav zobrazení s GridView byla zakázána. Je však velmi důležité, aby zůstala stav zobrazení povolený pro GridViews, které podporují úpravy nebo odstranění (výchozí nastavení). Pokud jste nastavili GridView s `EnableViewState` vlastnost `false`, úprav a odstraňování chování se bez problémů fungují pro jednoho uživatele, ale pokud existují souběžných uživatelů odstranění dat, existuje možnost náhodně může tyto souběžných uživatelů odstranit nebo upravit záznamy, že je dodán t hodlají. Najdete v části Moje položku blogu [upozornění: souběžnosti problém s ASP.NET 2.0 GridViews nebo DetailsView/FormViews tento podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázána](http://scottonwriting.net/sowblog/posts/10054.aspx), další informace.


Toto upozornění stejné platí také pro DetailsViews a FormViews.

Pro přidání možností odstraňování do GridView, jednoduše přejděte do jeho inteligentních značek a zaškrtněte políčko Povolit odstraňování.


![Zaškrtněte políčko Povolit odstraňování zaškrtávací políčko](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Obrázek 10**: Zkontrolujte Povolit odstraňování zaškrtávací políčko


Kontrola políčka Povolit odstranění z inteligentních značek přidá CommandField GridView. CommandField vykreslí sloupec v GridView pomocí tlačítka pro provádění jeden nebo více z následujících úloh: výběr záznam, úprava záznamů a odstraňování záznamu. Jsme viděli dříve CommandField v akci s vybírání záznamů v [hlavní/podrobností volitelný GridView hlavní pomocí DetailView podrobnosti](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) kurzu.

CommandField obsahuje řadu `ShowXButton` označují, jaké řadu tlačítek se zobrazují v CommandField. Zaškrtnutím políčka Povolit odstraňování CommandField jejichž `ShowDeleteButton` vlastnost je `True` byla přidána do kolekce sloupců prvku GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

V tomto okamžiku believe to nebo ne jsme hotovi s přidání odstraňování podpory do GridView! Jak ukazuje obrázek 11, při návštěvě této stránky prostřednictvím prohlížeče sloupec odstranit tlačítek není k dispozici.


[![CommandField přidá sloupec odstranit tlačítek](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Obrázek 11**: CommandField přidá sloupce z odstranění tlačítka ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


Pokud jste jste byla vytváření tohoto kurzu od základů sami, při testování tuto stránku kliknutím na tlačítko Odstranit vyvolá výjimku. Pokračujte ve čtení informace, proč byly vyvolány tyto výjimky a jejich řešení.

> [!NOTE]
> Pokud jste podle společně pomocí stahování doprovázející aplikaci tohoto kurzu, již byla zahrnutí pro tyto problémy. Ale I doporučujeme, abyste si přečíst podrobnosti, aby bylo možné identifikovat problémy, které může způsobit a vhodné řešení uvedené níže.


Pokud při pokusu o odstranění produktu, dojde k výjimce, jejichž zprávy je podobná "*ObjectDataSource 'ObjectDataSource1' nelze najít neobecnou metodu 'DeleteProduct', který obsahuje parametry: productID, původní\_ ProductID*, "pravděpodobně jste zapomněli odebrat `OldValuesParameterFormatString` vlastnost z ObjectDataSource. S `OldValuesParameterFormatString` vlastnost určena, ObjectDataSource pokusí předat v obou `productID` a `original_ProductID` vstupní parametry, které `DeleteProduct` metoda. `DeleteProduct`, ale přijímá pouze jeden vstupní parametr, proto výjimku. Odebrání `OldValuesParameterFormatString` vlastnost (nebo jeho nastavení na hodnotu `{0}`) dá pokyn ObjectDataSource pokoušet není předávat původní vstupní parametr.


[![Ujistěte se, že vlastnost OldValuesParameterFormatString byl vymazán](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Obrázek 12**: Zkontrolujte, zda `OldValuesParameterFormatString` vlastnost má byl vymazán Out ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


I v případě, že odebrali `OldValuesParameterFormatString` vlastnost, stále proběhne výjimku při pokusu o odstranění produktu se zprávou: "*příkaz DELETE konflikt s omezením referenční ' Cizíklíč\_pořadí\_podrobnosti \_Produkty, které se*. " Databáze Northwind obsahuje omezení cizího klíče mezi `Order Details` a `Products` tabulce, což znamená, že produkt ze systému nelze odstranit, pokud je jeden nebo více záznamů pro něj v `Order Details` tabulky. Protože každý produktu v databázi Northwind má alespoň jeden záznam v `Order Details`, nelze odstranit všechny produkty, dokud jsme nejprve odstranit záznamy podrobnosti o produktu přidružené pořadí.


[![Omezení cizího klíče znemožňuje odstranění produkty](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Obrázek 13**: odstranění produkty zakáže omezení pro cizí klíč ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


V našem kurzu budeme právě odstranit všechny záznamy z `Order Details` tabulky. V reálné aplikaci byste musíme buď:

- Spravovat informace o objednávce podrobnosti jiné obrazovku.
- Posílení `DeleteProduct` tak, aby zahrnoval logiku odstranit podrobnosti pořadí zadaného produktu
- Změňte dotaz SQL používané TableAdapter zahrnout odstranění podrobnosti pořadí zadaného produktu

Pojďme právě odstranit všechny záznamy z `Order Details` tabulky k obcházení omezení cizího klíče. Přejděte do Průzkumníka serveru v sadě Visual Studio, klikněte pravým tlačítkem na `NORTHWND.MDF` uzel a vyberte nový dotaz. Potom v okně dotazu spusťte následující příkaz SQL:`DELETE FROM [Order Details]`


[![Odstraňte všechny záznamy z tabulky Podrobnosti o pořadí](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Obrázek 14**: Odstraňte všechny záznamy z `Order Details` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


Po vymazání `Order Details` tabulky kliknutím na tlačítko Odstranit odstraníte produktu bez chyby. Pokud kliknete na tlačítko Odstranit nedojde k odstranění produktu, zkontrolujte, ujistěte se, že rutina GridView `DataKeyNames` je nastavena na pole primárního klíče (`ProductID`).

> [!NOTE]
> Když kliknete na tlačítko Odstranit vyplývá zpětné volání a záznam se odstraní. To může být nebezpečné, protože se snadno omylem, klepněte na tlačítko Odstranit nesprávných řádků. V budoucích kurzu vidíte postup přidání klienta potvrzení při odstraňování záznamu.


## <a name="editing-data-with-the-gridview"></a>Úpravy dat s GridView

Společně s odstraněním, prvek GridView také poskytuje integrovanou podporu pro úpravy úrovni řádků. Rutina GridView podporovat úpravy konfigurace přidá sloupec tlačítka Upravit. Z hlediska koncového uživatele, kliknutím na řádku upravit tlačítko příčin, které řádek stane upravitelné vypnutí buněk do textových polí obsahující existující hodnoty a nahrazení tlačítka Storno a tlačítko Upravit s aktualizací. Po provedení jejich požadované změny, koncový uživatel kliknutím na tlačítko Aktualizovat k provedení změn nebo na tlačítko Zrušit je zahodit. V obou případech po kliknutí na tlačítko Aktualizovat nebo zrušit vrátí rutina GridView předem úpravy stav.

Z našich perspektivy jako vývojář stránky Pokud koncový uživatel klikne na tlačítko Upravit pro konkrétního řádku, vyplývá zpětné volání a GridView provede následující kroky:

1. Rutina GridView na `EditItemIndex` vlastnost je přiřazena k index řádku, jehož tlačítko Upravit označeného kliknutím
2. GridView znovu připojí samotné k ObjectDataSource vyvoláním jeho `Select()` – metoda
3. Index řádku, který odpovídá `EditItemIndex` je vykreslen v "režimu úprav." V tomto režimu tlačítko Upravit nahrazuje aktualizace a zrušit tlačítka a BoundFields jejichž `ReadOnly` vlastnosti jsou NEPRAVDA (výchozí) vykreslují jako textové pole webové ovládací prvky, jejichž `Text` vlastnosti přiřazené hodnoty datová pole.

Kód v tomto okamžiku je vrácen do prohlížeči, aby koncový uživatel žádné změny dat řádek. Když uživatel klikne na tlačítko Aktualizovat, dojde k zpětné volání a GridView provede následující kroky:

1. ObjectDataSource `UpdateParameters` hodnoty jsou přiřazeny hodnot zadaných GridView úpravy rozhraní koncovým uživatelem
2. ObjectDataSource `Update()` metoda je volána, aktualizace zadaný záznam
3. GridView znovu připojí samotné k ObjectDataSource vyvoláním jeho `Select()` – metoda

Přiřazené hodnot primárního klíče `UpdateParameters` v kroku 1 pocházet z hodnoty zadané v `DataKeyNames` vlastnost, zatímco neprimárního klíče hodnoty pocházejí z textu v ovládacích prvcích TextBox webového upravená řádku. Stejně jako u odstraňování, je důležité, rutina GridView `DataKeyNames` vlastnost správně nastavit. Pokud není nalezena, `UpdateParameters` hodnotu primárního klíče bude přiřazena hodnota `Nothing` v kroku 1, pak nebude důsledkem toho aktualizaci všech záznamů v kroku 2.

Úpravy funkce může být aktivovaný jednoduše zaškrtnutím políčka Povolit úpravy v prvku GridView inteligentních značek.


![Zaškrtněte políčko Povolit úpravy zaškrtávací políčko](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Obrázek 15**: Zkontrolujte povolit úpravy zaškrtávací políčko


Kontrola políčko Povolit úpravy přidá CommandField (v případě potřeby) a sady jeho `ShowEditButton` vlastnost `True`. Jak jsme viděli dříve, CommandField obsahuje řadu `ShowXButton` označují, jaké řadu tlačítek se zobrazují v CommandField. Kontrola políčko Povolit úpravy přidá `ShowEditButton` vlastnost, která má existující CommandField:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

To je vše ohledně přidávání elementární úpravy podpory. Jako Figure16 ukazuje úpravy rozhraní je spíš hrubých každý BoundField jejichž `ReadOnly` je nastavena na `False` (výchozí), které je vykresleno jako textové pole. To zahrnuje i pole jako `CategoryID` a `SupplierID`, které jsou klíčů s jinými tabulkami.


[![Kliknutím na tlačítko Upravit s Chai zobrazí řádek v režimu úprav](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Obrázek 16**: s Chai kliknutím na tlačítko Upravit zobrazí řádek v režimu úprav ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


Kromě požádá uživatele Chcete-li upravit hodnoty cizího klíče přímo, není splněn úpravy rozhraní rozhraní následujícími způsoby:

- Pokud uživatel zadá `CategoryID` nebo `SupplierID` v databázi, která neexistuje `UPDATE` nesplňují omezení cizího klíče, která způsobila výjimku má být aktivována.
- Úpravy rozhraní neobsahuje žádné ověření. Pokud nezadáte požadované hodnoty (například `ProductName`), nebo zadejte hodnotu řetězce, kde je očekávána je číselná hodnota (například zadání "Příliš mnoho!" do `UnitPrice` textového pole), bude vyvolána výjimka. Budoucí kurzu bude zkontrolujte postup přidání ovládacích prvků ověření do uživatelského rozhraní pro úpravy.
- V současné době *všechny* pole produktů, které nejsou jen pro čtení musí být součástí GridView. Pokud nám chcete pole odebrat z GridView vyslovení `UnitPrice`, aktualizace dat GridView nebude nastavena `UnitPrice` `UpdateParameters` hodnotu, která by způsobila změnu záznam databáze `UnitPrice` k `NULL` hodnotu. Podobně, pokud povinné pole, například `ProductName`, se odebere z GridView, se aktualizace nezdaří se stejným "*'ProductName' sloupec nepovoluje hodnoty Null*" výjimka uvedených výše.
- Úpravy rozhraní formátování zanechává mnoho k být potřeby. `UnitPrice` Se zobrazí s čtyři desetinných míst. V ideálním případě `CategoryID` a `SupplierID` hodnoty by obsahovat DropDownLists, který zobrazí seznam kategorií a všichni dodavatelé v systému.

Toto jsou všechny nedostatky, které jsme budete muset za provozu se nyní, ale bude vyřešen v budoucí kurzy.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Vkládání, úpravy a odstraňování dat s DetailsView

Jak jsme viděli v dřívější kurzy, DetailsView ovládací prvek zobrazí jeden záznam v čase a jako GridView, umožňuje úpravy a odstraňování aktuálně zobrazený záznam. Obě koncového uživatele zkušenosti s úpravy a odstraňování položek ze DetailsView a pracovní postup ze strany ASP.NET je stejná jako prvku GridView. Kde DetailsView se liší od GridView je, že také poskytuje integrovanou podporu vkládání.

K předvedení funkce úpravy dat prvku GridView, začněte přidáním DetailsView k `Basics.aspx` stránky nad existující GridView a navázat jej existující ObjectDataSource pomocí inteligentních značek DetailsView. Další, zrušte na DetailsView `Height` a `Width` vlastnosti a zkontrolujte možnost Povolit stránkování inteligentních značek. Pokud chcete povolit, úpravy, vkládání a odstraňování podpory, jednoduše zkontrolujte povolit úpravy, Povolit vložení a Povolit odstranění políček v inteligentní značky.


![Konfigurace DetailsView na podporu, úprava, vkládání a odstraňování](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Obrázek 17**: Konfigurace DetailsView na podporu, úprava, vkládání a odstraňování


Jako s GridView, přidání, úpravy, vložením či odstraněním podporu přidá CommandField DetailsView, jak ukazuje následující deklarativní syntaxe:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

Všimněte si, že pro DetailsView CommandField se zobrazuje na konec kolekce sloupců ve výchozím nastavení. Vzhledem k tomu, že DetailsView pole vykreslovat jako řádky, CommandField zobrazovat jako řádek s příkazy Insert, upravovat a odstraňovat tlačítka v dolní části DetailsView.


[![Konfigurace DetailsView na podporu, úprava, vkládání a odstraňování](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Obrázek 18**: Konfigurace DetailsView pro podporu úprava, vkládání a odstraňování ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


Kliknutím na tlačítko Odstranit spustí stejný posloupnost událostí stejně jako u GridView: a postback; Následuje DetailsView naplnění jeho ObjectDataSource `DeleteParameters` na základě `DataKeyNames` hodnoty a bylo dokončeno s volání jeho ObjectDataSource `Delete()` metodu, která ve skutečnosti odebere produktu z databáze. Úpravy v ovládacím prvku DetailsView funguje taky způsobem, který je stejná jako prvku GridView.

Pro vkládání, koncový uživatel předloží nová tlačítka, která po kliknutí na vykreslí DetailsView v "režimu vkládání." S "insert režim" na tlačítko Nový nahrazuje tlačítka Vložit a Storno a pouze ty BoundFields jejichž `InsertVisible` je nastavena na `True` (výchozí) jsou zobrazeny. Tato datová pole identifikovat jako automatického přírůstku pole, jako například `ProductID`, mají jejich [InsertVisible vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) nastavena na `False` při vytváření vazby DetailsView ke zdroji dat pomocí inteligentních značek.

Při vytváření vazby zdroje dat pro DetailsView pomocí inteligentních značek, Visual Studio nastaví `InsertVisible` vlastnost `False` pouze u polí s automatickým krokem. Pole jen pro čtení, například `CategoryName` a `SupplierName`, se zobrazí v uživatelském rozhraní "insert režim", pokud jejich `InsertVisible` je explicitně nastavena na `False`. Za chvíli nastavit tyto dvě pole `InsertVisible` vlastnosti, které chcete `False`, buď pomocí deklarativní syntaxe prvku DetailsView nebo upravit pole na odkaz v inteligentní značky. 19 obrázek ukazuje nastavení `InsertVisible` vlastnosti, které chcete `False` kliknutím na Upravit pole na odkaz.


[![Northwind Traders teď nabízí čaj: MSN](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Obrázek 19**: Northwind Traders teď nabízí pokusná čaj ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


Po nastavení `InsertVisible` vlastnosti, zobrazení `Basics.aspx` stránku v prohlížeči a klikněte na tlačítko Nový. Obrázek 20 ukazuje DetailsView při přidávání nové nápoj pokusná čaj řadu našich produktů.


[![Northwind Traders teď nabízí čaj: MSN](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Obrázek 20**: Northwind Traders teď nabízí pokusná čaj ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


Po zadání podrobností pokusná čaj a kliknutím na tlačítko Vložit, zpětné volání vyplývá a se přidá nový záznam `Products` databázové tabulky. Vzhledem k tomu, že tento DetailsView seznamy produktů, v pořadí, ke které existují v tabulce databáze, jsme musí na poslední stránce produktu Chcete-li zobrazit nového produktu.


[![Podrobnosti: MSN čaj](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Obrázek 21**: Podrobnosti o pokusná čaj ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> DetailsView [CurrentMode vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) znamená rozhraní zobrazení a může být jedna z následujících hodnot: `Edit`, `Insert`, nebo `ReadOnly`. [Vlastnost DefaultMode](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) Určuje režim DetailsView vrátí po upravíte nebo vložit byla dokončena a jsou užitečné pro zobrazení DetailsView, trvale v upravit nebo vložit režimu.


Bod a klikněte na tlačítko vkládání a úpravy funkce DetailsView trpí stejná omezení jako GridView: uživatel musí zadat existující `CategoryID` a `SupplierID` hodnoty prostřednictvím textové pole; chybí rozhraní žádné ověřovací logiku; všechny pole produktu, které neumožňují `NULL` hodnoty nebo nemají výchozí hodnota zadaná na úrovni databáze musí být součástí vkládání rozhraní a tak dále.

Technik vyzkoušíme pro rozšíření a zlepšení prvku GridView úpravy rozhraní v budoucnosti články lze použít pro ovládací prvek DetailsView úpravy a vkládání také rozhraní.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Pomocí FormView pro uživatelské rozhraní flexibilnější úpravy dat

FormView má integrovanou podporu pro vkládání, úpravy a odstraňování dat, ale protože používá šablony místo pole neexistuje žádné místo, kde můžete přidat BoundFields nebo CommandField používá k zajištění data GridView a DetailsView ovládací prvky úpravy rozhraní. Místo toho toto rozhraní ovládací prvky webového pro shromažďování uživatele vstup při přidání nové položky nebo úpravou existující společně s novou, upravit, odstranit, Insert, Update a stornovací tlačítka musí ručně přidat do příslušných šablon. Naštěstí vytvoří sada Visual Studio automaticky rozhraní potřebné při vazbě FormView ke zdroji dat pomocí rozevíracího seznamu v jeho inteligentních značek.

Pro ilustraci tyto postupy, začněte přidáním FormView k `Basics.aspx` stránky a ze třídy FormView inteligentní značky, ho navázat ObjectDataSource již vytvořeny. Tím se vygeneruje `EditItemTemplate`, `InsertItemTemplate`, a `ItemTemplate` pro FormView s ovládacími prvky webového textové pole pro shromažďování vstupu uživatele a ovládací prvky webového tlačítko pro nový, upravit, odstranit, Insert, Update a stornovací tlačítka. Kromě toho třídy FormView `DataKeyNames` je nastavena na pole primárního klíče (`ProductID`) objektů vrácené objektem ObjectDataSource. Nakonec zkontrolujte možnost Povolit stránkování v třídě FormView inteligentních značek.

Následující ukazuje deklarativní pro třídy FormView `ItemTemplate` po FormView byla svázána se ObjectDataSource. Ve výchozím nastavení, je každé pole produktu-logická hodnota hodnota vázána `Text` vlastností ovládacího prvku popisek webové při každé pole logická hodnota (`Discontinued`) je vázána `Checked` vlastnost zakázané ovládací prvek zaškrtávací políčko webu. Aby tlačítka Nový, Edit a Delete pro určité FormView chování při kliknutí na aktivaci, je nutné, jejich `CommandName` nastavit hodnoty na `New`, `Edit`, a `Delete`, v uvedeném pořadí.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

Obrázek 22 ukazuje FormView `ItemTemplate` při zobrazení prostřednictvím prohlížeče. Pomocí tlačítka Nový, Edit a Delete v dolní části je uveden každé pole produktu.


[![ItemTemplate Defaut FormView obsahuje seznam všech polí produktu spolu s nové, upravovat a odstraňovat tlačítka](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Obrázek 22**: The Defaut FormView `ItemTemplate` uvádí každý produkt pole společně s novou, upravit a odstranit tlačítka ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


Jako s GridView a DetailsView, kliknutím na tlačítko Odstranit nebo žádné tlačítko, LinkButton nebo ImageButton jejichž `CommandName` vlastnost je nastavena na odstranění příčin zpětné volání, naplní ObjectDataSource `DeleteParameters` na FormViewnazákladě`DataKeyNames`hodnota a vyvolá ObjectDataSource `Delete()` metoda.

Při kliknutí na tlačítko Upravit vyplývá zpětné volání a dat je odrážejí na `EditItemTemplate`, která je zodpovědná za vykreslování úpravy rozhraní. Toto rozhraní obsahuje webové ovládací prvky pro úpravy dat společně s tlačítka aktualizace a zrušit. Výchozí hodnota `EditItemTemplate` generované Visual Studio obsahuje štítek pro všechna pole automatického přírůstku (`ProductID`), textové pole pro každé pole hodnota logická hodnota a zaškrtávací políčko pro každé pole logická hodnota. Toto chování je velmi podobný BoundFields automaticky vygenerované v ovládacích prvcích GridView a DetailsView.

> [!NOTE]
> Malé jedním z problémů s FormView pro automatické generování `EditItemTemplate` je, že vykreslením TextBox webové ovládací prvky pro tyto pole, které jsou jen pro čtení, například `CategoryName` a `SupplierName`. Ukážeme, jak pro tento účet za chvíli.


Ovládacích prvků do textového pole `EditItemTemplate` mít jejich `Text` vlastnost vázána na hodnoty jejich odpovídajících dat pole pomocí *obousměrné vazby dat*. Obousměrné vazby dat, odlišené `<%# Bind("dataField") %>`, provádí vazby dat i při vytváření vazby dat v šabloně a při naplňování ObjectDataSource parametry pro vložení nebo úprava záznamů. To znamená, když uživatel klikne na tlačítko Upravit z `ItemTemplate`, `Bind()` metoda vrátí hodnotu pole Zadaná data. Jakmile uživatel provede jejich změny a na aktualizace, hodnoty odeslány zpět, která odpovídají datových polí zadat pomocí `Bind()` použijí ObjectDataSource `UpdateParameters`. Alternativně jednosměrné vazby dat, odlišené `<%# Eval("dataField") %>`, pouze při vytváření vazby dat v šabloně načte pole hodnoty dat a nemá *není* návratové hodnoty zadanou uživatelem na parametry zdroj dat na zpětné volání.

Následující kód ukazuje FormView `EditItemTemplate`. Všimněte si, že `Bind()` metoda se používá v syntaxi datové vazby sem a zda mají ovládací prvky tlačítko Update a zrušit jejich `CommandName` odpovídajícím způsobem nastaveny vlastnosti.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

Naše `EditItemTemplate`, na tomto bodu, způsobí výjimku vyvolat, když jsme pokusí ji použít. Problém je, že `CategoryName` a `SupplierName` pole vykreslovat jako textové pole webových ovládacích prvků do `EditItemTemplate`. Buď musíme změnit popisky na těchto textových polí nebo je úplně odebrat. Můžeme jednoduše odstraníte z zcela `EditItemTemplate`.

Po kliknutí na tlačítko Upravit pro Chai 23 obrázek ukazuje FormView v prohlížeči. Všimněte si, že `SupplierName` a `CategoryName` pole ukazuje `ItemTemplate` již neexistují, jak jsme právě odebrali z `EditItemTemplate`. Při kliknutí na tlačítko Aktualizovat FormView pokračuje prostřednictvím stejné pořadí kroků jako GridView a DetailsView ovládacích prvků.


[![Ve výchozím nastavení zobrazuje EditItemTemplate každé pole upravitelné produktu jako textové pole nebo zaškrtávací políčko](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Obrázek 23**: ve výchozím nastavení `EditItemTemplate` zobrazuje každou upravitelné produktu pole jako textové pole nebo zaškrtávací políčko ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


Při po kliknutí na tlačítko Vložit na FormView `ItemTemplate` vyplývá zpětné volání. Ale žádná data je vázána FormView vzhledem k tomu, že se přidává nový záznam. `InsertItemTemplate` Rozhraní obsahuje ovládací prvky webového pro přidávání nového záznamu společně s tlačítka Vložit a Storno. Výchozí hodnota `InsertItemTemplate` generované Visual Studio obsahuje textové pole pro každé pole hodnota logická hodnota a zaškrtávací políčko pro každé pole logická hodnota, podobně jako automaticky generovaný `EditItemTemplate`je rozhraní. Ovládací prvky textové pole mají jejich `Text` vlastnost vázána na hodnotu odpovídající datové pole pomocí obousměrné vazby dat.

Následující kód ukazuje FormView `InsertItemTemplate`. Všimněte si, že `Bind()` metoda se používá v syntaxi datové vazby sem a zda mají ovládací prvky Insert a webové tlačítko zrušit jejich `CommandName` odpovídajícím způsobem nastaveny vlastnosti.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

Je subtlety s FormView pro automatické generování `InsertItemTemplate`. Konkrétně ovládací prvky webového TextBox vytvářejí i pro tato pole, které jsou jen pro čtení, například `CategoryName` a `SupplierName`. Upozorňujeme `EditItemTemplate`, je potřeba odebrat tyto textových polí z `InsertItemTemplate`.

Při přidání nového produktu, pokusná kávy 24 obrázek ukazuje FormView v prohlížeči. Všimněte si, že `SupplierName` a `CategoryName` pole ukazuje `ItemTemplate` již neexistují, protože jsme právě odebrat. Při kliknutí na tlačítko Vložit bude pokračovat FormView prostřednictvím stejné pořadí kroků jako ovládací prvek DetailsView, přidávání nového záznamu do `Products` tabulky. Obrázek 25 obsahuje pokusná kávy produktu podrobnosti ve třídě FormView po vložení.


[![InsertItemTemplate stanoví FormView vkládání rozhraní](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Obrázek 24**: `InsertItemTemplate` stanoví FormView vkládání rozhraní ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![Podrobnosti o nového produktu, pokusná kávy, jsou zobrazeny v třídě FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Obrázek 25**: The podrobnosti nového produktu, pokusná kávy, jsou zobrazeny v třídě FormView ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


Oddělením se jen pro čtení, úpravám a vkládání rozhraní do tří samostatných šablony, FormView umožňuje jemnějšího stupeň kontroly nad tato rozhraní než DetailsView a rutina GridView.

> [!NOTE]
> Jako, FormView prvku DetailsView `CurrentMode` určuje vlastnost rozhraní zobrazily a jeho `DefaultMode` vlastnost označuje režim FormView vrátí k za upravíte nebo insert se dokončila.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na základní informace o vložení, úpravy a odstraňování dat pomocí GridView, DetailsView a FormView. Všechny tři z těchto prvků zadejte určité úrovně vestavěných datových úpravy funkce, které můžete použít bez nutnosti napsat jediný řádek kódu na stránce ASP.NET díky ovládací prvky webového dat a ObjectDataSource. Jednoduché však bod a klikněte na techniky vykreslení poměrně frail a naïve data úpravy uživatelské rozhraní. Pro ověřování, vložit programový hodnoty, pohodlné zpracování výjimek, přizpůsobení uživatelského rozhraní a tak dále, budeme potřebovat spoléhají na bevy technik, které budou popsané v další kurzy několik.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Předchozí](limiting-data-modification-functionality-based-on-the-user-cs.md)
[další](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
