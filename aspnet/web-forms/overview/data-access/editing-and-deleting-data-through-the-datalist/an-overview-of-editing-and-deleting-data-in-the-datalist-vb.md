---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: "Přehled úpravy a odstraňování dat v DataList (VB) | Microsoft Docs"
author: rick-anderson
description: "Při prvku DataList chybí předdefinované úpravy a odstraňování možnosti, v tomto kurzu ukážeme, jak vytvořit DataList, který podporuje úpravy a odstraňování o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e08b55f763677a40a03503e54a23dc77a10a34f5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Přehled úpravy a odstraňování dat v DataList (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) nebo [stáhnout PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Při prvku DataList chybí předdefinované úpravy a odstraňování možnosti, v tomto kurzu jsme zobrazí postup vytvoření DataList, který podporuje úpravy a odstraňování jeho základní data.


## <a name="introduction"></a>Úvod

V [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) kurzu jsme se podívali na tom, jak vložit, aktualizovat a odstranit data pomocí architektury aplikace, ObjectDataSource a rutina GridView DetailsView a FormView ovládací prvky. ObjectDataSource a ovládací prvky webového tyto tři datové implementace rozhraní úpravy jednoduché datové byla modulu snap a zahrnuta pouze tikání zaškrtávací políčko ze inteligentní značky. Žádný kód potřebný k zapsání.

Bohužel DataList chybí předdefinované úpravy a odstraňování možnosti vyplývajících v ovládacím prvku GridView. Tato funkce chybí je kvůli částečně skutečnost, že je DataList relic z předchozí verze technologie ASP.NET, pokud byla k dispozici ovládací prvky zdroje deklarativní data a data kód bez úprav stránky. Při DataList v technologii ASP.NET 2.0 nenabízí stejné ihned funkce úpravy dat jako GridView, jsme vám pomůže ASP.NET 1.x techniky patří takové funkce. Tento přístup vyžaduje malou část kódu, ale, jak jsme zobrazí v tomto kurzu, DataList má některé události a vlastnosti na místě a usnadňuje tento proces.

V tomto kurzu vidíte vytvoření DataList, který podporuje úpravy a odstraňování jeho základní data. Budoucí kurzy prozkoumá pokročilejší úpravy a odstraňování scénáře, včetně ověření vstupní pole, pohodlné zpracování výjimek vyvolaných z přístupu k datům nebo vrstvy obchodní logiky a tak dále.

> [!NOTE]
> Jako DataList nemá ovládacím prvku Repeater mimo funkci pole pro vložení, aktualizaci nebo odstranění. Při přidávání těchto funkcí, zahrnuje prvku DataList vlastností a událostí nebyl nalezen v Opakovači, které zjednodušují přidání například tyto funkce. Proto tento kurz a budoucí ty, které podívejte se na úpravy a odstranění se soustředí výhradně na prvku DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Krok 1: Vytváření, úpravy a odstraňování kurzy webové stránky

Než začneme zkoumat aktualizaci a odstranění dat z DataList, umožní s nejprve pozorně vytváření stránek ASP.NET v našem webu projekt, který budeme potřebovat pro tento kurz a další několik podsítí. Nejprve přidejte novou složku s názvem `EditDeleteDataList`. Dál přidejte následující stránky ASP.NET do této složky, a zkontrolujte, zda přidružit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Přidání stránky ASP.NET pro kurzů k](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Obrázek 1**: Přidání stránky ASP.NET pro kurzů k


V jiných složkách, jako `Default.aspx` v `EditDeleteDataList` složka obsahuje seznam kurzů k v jeho části. Odvolat, který `SectionLevelTutorialListing.ascx` tuto funkci zajišťuje uživatelský ovládací prvek. Proto přidat tento uživatelský ovládací prvek pro `Default.aspx` přetažením z Průzkumníka řešení na stránku s zobrazení návrhu.


[![Přidání SectionLevelTutorialListing.ascx uživatelského ovládacího prvku do Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Obrázek 2**: Přidat `SectionLevelTutorialListing.ascx` uživatelského ovládacího prvku na `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Nakonec přidejte stránky jako položky na `Web.sitemap` souboru. Konkrétně, přidejte následující kód po sestavy a podrobností s DataList a opakovače `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, pozorně Zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro DataList úpravy a odstraňování kurzy.


![Mapy webu nyní obsahuje položky pro DataList úpravy a odstraňování kurzy](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Obrázek 3**: mapy webu nyní obsahuje položky pro DataList úpravy a odstraňování kurzy


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Krok 2: Prozkoumání techniky pro aktualizaci a odstraňování dat

Úpravy a odstraňování dat s GridView je tak snadno, protože pod zahrnuje, rutina GridView a ObjectDataSource fungovat ve vzájemné součinnosti. Jak je popsáno v [zkoumání události spojené s vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) kurzu při kliknutí na tlačítko Aktualizovat řádek s GridView automaticky přiřadí jeho pole, která používá obousměrný datové vazby k `UpdateParameters` kolekce jeho ObjectDataSource a potom se vyvolá tento ObjectDataSource s `Update()` metoda.

Bohužel DataList neposkytuje žádné této integrované funkce. Je naše odpovědností zajistit přiřazení hodnoty uživatele s parametry s ObjectDataSource a že jeho `Update()` metoda je volána. Pokud chcete nám pomoci při této omezené úsilí, prvku DataList poskytuje následující vlastnosti a události:

- **[ `DataKeyField` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  při aktualizaci nebo odstranění, potřebujeme, abyste mohli jednoznačně identifikovat každou položku v prvku DataList. Nastavte tuto vlastnost na primární klíče pole zobrazená data. Díky tomu bude naplnit DataList s [ `DataKeys` kolekce](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) se zadaným `DataKeyField` hodnota pro každou položku DataList.
- **[ `EditCommand` Událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  aktivuje se v případě tlačítka, LinkButton nebo ImageButton jehož `CommandName` je nastavena na kliknutí na Upravit.
- **[ `CancelCommand` Událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  aktivuje se v případě tlačítka, LinkButton nebo ImageButton jehož `CommandName` je nastavena na kliknutí na Storno.
- **[ `UpdateCommand` Událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  aktivuje se v případě tlačítka, LinkButton nebo ImageButton jehož `CommandName` je nastavena na po kliknutí na aktualizace.
- **[ `DeleteCommand` Událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  aktivuje se v případě tlačítka, LinkButton nebo ImageButton jehož `CommandName` je nastavena na po kliknutí na odstranění.

Pomocí těchto vlastností a událostí, existují čtyři přístupy, které jsme můžete použít k aktualizaci a odstranění dat z prvku DataList:

1. **Pomocí technologie ASP.NET 1.x techniky** prvku DataList existovaly před technologii ASP.NET 2.0 a ObjectDataSources a bylo možné aktualizovat a odstraňovat data programové způsoby. Tento postup ObjectDataSource zcela kanálů a vyžaduje jsme vazby dat k prvku DataList přímo z vrstvy obchodní logiky, jak získat data k zobrazení a při aktualizaci nebo odstranění záznamu.
2. **Pomocí jednoho prvku ObjectDataSource na stránce pro výběr, aktualizace a odstranění** při prvku DataList chybí GridView s vyplývajících úpravy a odstraňování možnosti, že s žádný důvod můžeme t je přidejte v označována. S tímto přístupem můžeme používat ObjectDataSource podobně jako příklady GridView, ale musíte vytvořit obslužnou rutinu události pro DataList s `UpdateCommand` událostí, kde jsme nastavit ObjectDataSource s parametry a volání jeho `Update()` metoda.
3. **Použití ovládacího prvku ObjectDataSource zvolíte, ale aktualizace a odstranění přímo proti the BLL** při použití volby 2 musíme zápisu bit kódu `UpdateCommand` událostí, přiřazení hodnoty parametrů a tak dále. Místo toho jsme přilepit s použitím ObjectDataSource pro výběr ale volání aktualizaci a odstraňování přímo na BLL (jako jsou s možnost 1). Moje názoru aktualizace dat pomocí rozhraní přímo k BLL vede k srozumitelnější kódu než přiřazení ObjectDataSource s `UpdateParameters` a volání jeho `Update()` metoda.
4. **Pomocí deklarativní znamená prostřednictvím více ObjectDataSources** předchozí všechny tři přístupy vyžadují bit kódu. Pokud jste d místo dál používat jako mnohem deklarativní syntaxe nejdříve, je poslední možnost zahrnují více ObjectDataSources na stránce. První ObjectDataSource načítá data z BLL a váže k prvku DataList. Pro aktualizaci, je jiná ObjectDataSource přidán, ale přidat přímo v rámci DataList s `EditItemTemplate`. Zahrnout odstraňování podpory, ještě jiné ObjectDataSource by potřeba v `ItemTemplate`. S tímto přístupem tyto ObjectDataSource s použití vložených `ControlParameters` deklarativně vytvořit vazbu s parametry ObjectDataSource uživatelské vstupní ovládací prvky (místo nutnosti zadávat prostřednictvím kódu programu v DataList s `UpdateCommand` obslužné rutiny události). Tento přístup stále vyžaduje bit kódu musíme volání embedded ObjectDataSource s `Update()` nebo `Delete()` příkaz ale vyžaduje mnohem menší než k ostatním tři blíží. Zde nevýhodou je, že více ObjectDataSources zbytečných souborů stránku, zasahoval celkové čitelnost.

Pokud vynutilo vždy jen použijte jednu z těchto přístupů, I d zvolte možnost 1, protože poskytuje největší flexibilitu a protože prvku DataList byla původně navržena pro uložení tohoto vzoru. Když DataList byl rozšířené pro práci s ovládacími prvky zdroje dat ASP.NET 2.0, nemá všechny body rozšiřitelnosti nebo funkce oficiální data technologie ASP.NET 2.0 ovládací prvky webového (GridView, DetailsView a FormView). Možnosti 2 až 4 nejsou bez hodnotě, ale.

To budoucí úpravy a odstraňování kurzy použije ObjectDataSource pro načítání dat k zobrazení a přímé volání BLL k aktualizaci a odstranění dat (možnost 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Krok 3: Přidání prvku DataList a konfigurace jeho ObjectDataSource

V tomto kurzu vytvoříme DataList, který zobrazuje informace o produktu a pro každý produkt obsahuje uživatele umožňuje upravit název a ceny a úplně odstranit produktu. Konkrétně jsme načíst záznamy, které chcete zobrazit pomocí ObjectDataSource, ale provede aktualizace a odstranění akcí pomocí rozhraní přímo k BLL. Před jsme starat o implementaci úpravy a odstraňování funkce pro prvku DataList, umožní s nejdřív získat na stránce zobrazit produkty v rozhraní, které je jen pro čtení. Protože jsme sunout zkontrolován tyto kroky v předchozí kurzech I budete pokračujte je rychle.

Začněte otevřením `Basics.aspx` stránku `EditDeleteDataList` složku a ze zobrazení návrhu, přidejte na stránku DataList. Dále ze s DataList inteligentní značky, vytvořte nový ObjectDataSource. Vzhledem k tomu, že jsme pracují s daty produktu, nakonfigurujte ho na používání `ProductsBLL` třídy. Načtení *všechny* vyberte produkty, `GetProducts()` metoda na kartě vyberte.


[![Konfigurace ObjectDataSource použití třídy ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Obrázek 4**: Konfigurace ObjectDataSource pro použití `ProductsBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Vrátí informace o produktu pomocí metody GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Obrázek 5**: vrátit na informace o produktu pomocí `GetProducts()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


DataList, jako je GridView, není určené pro vložení nová data; proto vyberte (None) možnost v rozevíracím seznamu v kartě Vložení. Také (None) zvolte pro karty aktualizace a odstranění od aktualizace a odstranění bude prostřednictvím BLL provést prostřednictvím kódu programu.


[![Potvrďte nastavení rozevíracího seznamu jsou uvedeny v ObjectDataSource s příkazy INSERT, UPDATE a odstranit karty na (žádný)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Obrázek 6**: zkontrolovali, jestli rozevíracího seznamu, jsou uvedeny ve ObjectDataSource s příkazy INSERT, UPDATE a odstranit karty jsou nastavené na (žádný) ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Po dokončení konfigurace ObjectDataSource, klikněte na tlačítko Dokončit, vrácením návrháře. Jako jsme jste viděli v posledních příklady po dokončení konfigurace ObjectDataSource, Visual Studio automaticky vytvoří `ItemTemplate` pro rozevírací seznam, zobrazení každé pole data. Nahraďte ho `ItemTemplate` s jedním, který zobrazí název produktu s a ceny. Také nastavena `RepeatColumns` vlastnost na hodnotu 2.

> [!NOTE]
> Jak je popsáno v *Přehled vložení, aktualizace a odstranění dat* kurzu při změně dat pomocí ObjectDataSource Naše architektura vyžaduje, že jsme odebrat `OldValuesParameterFormatString` vlastnost z ObjectDataSource s deklarativní (nebo ho resetovat na výchozí hodnotu, `{0}`). V tomto kurzu ale používáme ObjectDataSource pouze pro načítání dat. Proto jsme nemusíte upravit ObjectDataSource s `OldValuesParameterFormatString` hodnota vlastnosti (i když ho nemá t narušit Uděláte to tak).


Po nahrazení výchozí DataList `ItemTemplate` s vlastní jeden, deklarativní na stránku by mělo vypadat jako následující:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Chcete-li zobrazit naše průběh prostřednictvím prohlížeče chvíli trvat. Jak je vidět na obrázku 7, zobrazí DataList product name a jednotka ceny pro každý produkt v dva sloupce.


[![Názvy produktů a ceny se zobrazují v DataList dva sloupce](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Obrázek 7**: názvy produktů a ceny se zobrazují v DataList dvou sloupců ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> DataList má několik vlastností, které jsou požadovány pro proces aktualizace a odstraňování a tyto hodnoty se uloží v zobrazení stavu. Proto při vytváření DataList, který podporuje úpravy nebo odstranění dat, je nezbytné, aby byl povolen stav zobrazení s DataList.  
>   
>  Astute čtečky si pamatovat, že jsme byli schopni zakázat stav zobrazení, při vytváření upravitelné GridViews, DetailsViews a FormViews. Důvodem je, že může zahrnovat ovládacích prvků technologie ASP.NET 2.0 *řízení stavu*, což je stav ukládaný napříč postback jako stav zobrazení, ale domnělého nezbytné.


Zakazuje zobrazení stavu v GridView jenom vynechá trivial stavové informace, ale zachová stav ovládacího prvku (které zahrnuje stav potřebné pro úpravy a odstranění). DataList, s byla vytvořena v určeném časovém rozmezí ASP.NET 1.x nevyužívá ovládací prvek stavu a proto musí mít stav zobrazení, které jsou povolené. V tématu [řízení stavu vs. Zobrazení stavu](https://msdn.microsoft.com/library/1whwt1k7.aspx) Další informace o účelu řízení stavu a jak se liší od stav zobrazení.

## <a name="step-4-adding-an-editing-user-interface"></a>Krok 4: Přidání úpravy uživatelského rozhraní

Ovládací prvek GridView se skládá z kolekce polí (BoundFields, CheckBoxFields, TemplateFields a tak dále). Tato pole můžete upravit své vykreslené značky v závislosti na jejich režimu. V režimu jen pro čtení, BoundField zobrazí jeho data pole hodnotu jako text; Když v režimu úprav, vykreslí TextBox webového řídit, jehož `Text` vlastnost je přiřazena hodnota pole data.

DataList, vykreslí na druhé straně jeho položek pomocí šablony. Jen pro čtení položky jsou vykreslovány pomocí `ItemTemplate` zatímco položky v režimu úprav jsou vykreslovány pomocí `EditItemTemplate`. V tomto okamžiku je naše DataList pouze `ItemTemplate`. Pro podporu funkcí úpravy úrovni položek je potřeba přidat `EditItemTemplate` obsahující kód, který se má zobrazit pro položku upravovat. V tomto kurzu používáme ovládací prvky webového textové pole pro úpravy produktu s názvem a jednotka cenu.

`EditItemTemplate` Lze vytvořit deklarativně nebo prostřednictvím návrháře (tak, že vyberete možnost Upravit šablony ze inteligentní značky DataList s). Chcete-li použít možnost Upravit šablony, nejprve klikněte na odkaz Upravit šablony v inteligentní značky a pak vyberte `EditItemTemplate` položku z rozevíracího seznamu.


[![OPT práce se sadami DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Obrázek 8**: Opt práce se sadami DataList s `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


Potom zadejte název produktu: a cena: a poté přetáhněte z panelu nástrojů do dvou ovládacích prvků textového pole `EditItemTemplate` rozhraní v designeru. Nastavení textových polí `ID` vlastnosti, které chcete `ProductName` a `UnitPrice`.


[![Přidat textové pole s názvem produktu a ceny](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Obrázek 9**: přidat textové pole s názvem produktu a ceny ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


Je potřeba vytvořit vazbu odpovídající hodnoty polí data produktu na `Text` vlastnosti dvou textových polí. Inteligentní značky textová pole, klikněte na odkaz Upravit datové vazby a pak přidružit pole příslušná data s `Text` vlastnost, jak je znázorněno na obrázku 10.

> [!NOTE]
> Při vytváření vazby `UnitPrice` datové pole na cenu textové pole s `Text` pole, je může formátovat jako hodnotu měny (`{0:C}`), Obecné číslo (`{0:N}`), nebo nechte neformátovaný tvar.


![Vytvořit vazbu ProductName a pole UnitPrice dat vlastnosti textu textových polí](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Obrázek 10**: vytvoření vazby `ProductName` a `UnitPrice` Data polí k `Text` vlastnosti textových


Všimněte si, jak funguje dialogové okno Upravit datové vazby na obrázku 10 *není* zahrnují políčka obousměrné vazby dat, která je k dispozici při úpravě TemplateField GridView nebo DetailsView nebo šablonu FormView. Funkci obousměrné vazby dat povolená hodnota zadaná do vstupní ovládací prvek webu automaticky přiřazení odpovídající ObjectDataSource s `InsertParameters` nebo `UpdateParameters` při vložení nebo aktualizaci data. DataList nepodporuje obousměrné vazby dat, jako ukážeme později v tomto kurzu po uživatel provede jí změny a je připravena aktualizovat data, budeme muset programový přístup tyto textových polí `Text` vlastnosti a předejte jí jejich hodnoty do odpovídající `UpdateProduct` metoda v `ProductsBLL` třídy.

Nakonec je potřeba přidat aktualizace tlačítka a Storno `EditItemTemplate`. Jak jsme viděli v [hlavní/podrobností s odrážkami seznamu v hlavní záznamů pomocí DataList podrobnosti](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurzu, pokud tlačítko, LinkButton nebo ImageButton jehož `CommandName` je nastavena po kliknutí na z určitého opakovače nebo DataList, Opakovače nebo DataList s `ItemCommand` událost se vyvolá. Pro DataList Pokud `CommandName` je nastavena na určitou hodnotu, může také zvýšit další událost. Speciální `CommandName` hodnoty vlastností, patří mimo jiné:

- Zrušit vyvolá `CancelCommand` událostí
- Upravit vyvolá `EditCommand` událostí
- Aktualizovat vyvolá `UpdateCommand` událostí

Mějte na paměti, že tyto události jsou vyvolány *kromě* `ItemCommand` událostí.

Přidejte do `EditItemTemplate` dvou tlačítko webových ovládacích prvků, jeden jehož `CommandName` je nastavena na aktualizace a další prostředky nastavena na Storno. Po přidání těchto dvou ovládacích prvků tlačítko webové návrháře by měl vypadat takto:


[![Přidat aktualizace tlačítka EditItemTemplate a Storno](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Obrázek 11**: zrušit tlačítek a přidat aktualizovat `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Pomocí `EditItemTemplate` dokončení DataList s deklarativní by měl vypadat podobně jako následující:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Krok 5: Přidání vložení do režimu úprav

V tomto okamžiku naše DataList má úpravy rozhraní definované pomocí jeho `EditItemTemplate`, nicméně zde s aktuálně nijak pro uživatele, kteří navštěvují naší stránce s znamenat, že chce upravit informace o produktu s. Je potřeba přidat tlačítko pro každý produkt, že při kliknutí na, vykreslí této DataList položky v režimu úprav. Začněte přidáním tlačítko pro úpravy k `ItemTemplate`, prostřednictvím návrháře nebo deklarativně. Být určitých nastavení na tlačítko Upravit s `CommandName` vlastnost pro úpravy.

Jakmile přidáte toto tlačítko Upravit pozorně zobrazíte stránku prostřednictvím prohlížeče. Pomocí tohoto přidání by měla obsahovat každého produktu seznamu tlačítko pro úpravy.


[![Přidat aktualizace tlačítka EditItemTemplate a Storno](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Obrázek 12**: zrušit tlačítek a přidat aktualizovat `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Kliknutím na tlačítko způsobí, že zpětné volání, ale nemá *není* uvést do režimu úprav dílčího produktu. Chcete-li produkt upravovat, je potřeba:

1. Nastavit DataList s [ `EditItemIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) na index `DataListItem` právě označeného jehož tlačítko Upravit.
2. Rebind data, která mají DataList. Po znovu vykreslené DataList `DataListItem` jejichž `ItemIndex` odpovídá DataList s `EditItemIndex` bude vykreslit pomocí jeho `EditItemTemplate`.

Od DataList s `EditCommand` událost je aktivována, jestliže po kliknutí na tlačítko Upravit, vytvořte `EditCommand` obslužné rutiny události s následujícím kódem:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand` Obslužné rutiny události, je předaná objekt typu `DataListCommandEventArgs` jako druhý vstupní parametr, který obsahuje odkaz na `DataListItem` označeného jehož tlačítko Upravit (`e.Item`). Obslužné rutiny události nejprve nastaví DataList s `EditItemIndex` k `ItemIndex` z upravitelné `DataListItem` a pak znovu připojí data k prvku DataList voláním DataList s `DataBind()` metoda.

Po přidání této obslužné rutiny události, otevírat stránku v prohlížeči. Kliknutím na tlačítko Upravit teď umožňuje na kliknutelnou produktu upravovat (viz obrázek 13).


[![Kliknutím na tlačítko díky upravit upravitelné produktu](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Obrázek 13**: Kliknutím na tlačítko Upravit umožňuje upravovat produktu ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Krok 6: Uložení změn s uživatele

Kliknutím na tlačítko s upravená produktu aktualizaci nebo Storno tlačítka se nic nestane. v tomto okamžiku; Chcete-li přidat tuto funkci, musíme vytváření obslužných rutin událostí pro DataList s `UpdateCommand` a `CancelCommand` události. Začněte vytvořením `CancelCommand` obslužná rutina události, která se spustí, když po kliknutí na tlačítko Storno upravená produktu s a jeho musí s vrácením prvku DataList stavu předem úpravy.

Chcete-li DataList vykreslení všechny jeho položky v režimu jen pro čtení, je potřeba:

1. Nastavit DataList s [ `EditItemIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) do indexu neexistující `DataListItem` index. `-1`je bezpečné volbou, protože `DataListItem` indexy začínají `0`.
2. Rebind data, která mají DataList. Od ne `DataListItem` `ItemIndex` es odpovídají DataList s `EditItemIndex`, budou k dispozici celý DataList v režimu jen pro čtení.

Následující kód obslužné rutiny událostí se dá udělat tyto kroky:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Pomocí tohoto přidání kliknutím na tlačítko Storno tlačítko vrátí prvku DataList předem úpravy stav.

Je poslední obslužné rutiny události je potřeba dokončit `UpdateCommand` obslužné rutiny události. Této obslužné rutiny události musí:

1. Programový přístup název zadaný uživatelem produktu a ceny, jakož i upravená produktu s `ProductID`.
2. Zahájit proces aktualizace voláním odpovídající `UpdateProduct` přetížení v `ProductsBLL` třídy.
3. Nastavit DataList s [ `EditItemIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) do indexu neexistující `DataListItem` index. `-1`je bezpečné volbou, protože `DataListItem` indexy začínají `0`.
4. Rebind data, která mají DataList. Od ne `DataListItem` `ItemIndex` es odpovídají DataList s `EditItemIndex`, budou k dispozici celý DataList v režimu jen pro čtení.

Provedení kroků 1 a 2 zodpovídají za uložení uživatele s změn; kroky 3 a 4 DataList vrátit do předem úpravy stavu po změny byly uloženy a se shoduje s postupem prováděla `CancelCommand` obslužné rutiny události.

Získat název aktualizované produktu a ceny, je potřeba použít `FindControl` metodu prostřednictvím kódu programu odkaz TextBox webové ovládací prvky v rámci `EditItemTemplate`. Také je potřeba získat upravená produktu s `ProductID` hodnotu. Pokud jsme původně vázána ObjectDataSource prvku DataList, Visual Studio přiřazen DataList s `DataKeyField` vlastnost na hodnotu primárního klíče ze zdroje dat (`ProductID`). Tato hodnota může být pak načtena z DataList s `DataKeys` kolekce. Ujistěte se, že za chvíli `DataKeyField` skutečně je vlastnost nastavena na `ProductID`.

Následující kód implementuje čtyři kroky:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Obslužné rutiny události začne čtení v upravená produktu s `ProductID` z `DataKeys` kolekce. Další, dvou textových polí v `EditItemTemplate` odkazují a jejich `Text` vlastností uložených v místní proměnné `productNameValue` a `unitPriceValue`. Používáme `Decimal.Parse()` metoda čtení hodnoty z `UnitPrice` textové pole tak, že pokud zadaná hodnota má symbolu měny, ho můžete stále správně převést na `Decimal` hodnotu.

> [!NOTE]
> Hodnoty z `ProductName` a `UnitPrice` textová pole mají přiřazenou pouze k proměnné productNameValue a unitPriceValue, pokud má hodnotu vlastnosti Text textových polí. Jinak hodnota `Nothing` se používá pro proměnné, která má za následek aktualizace dat s databází `NULL` hodnotu. To znamená, že naše kódu považuje převede prázdné řetězce k databázi `NULL` hodnoty, které je výchozí chování úpravy rozhraní v ovládacích prvcích GridView DetailsView a FormView.


Po přečtení hodnoty, `ProductsBLL` třídu s `UpdateProduct` metoda je volána, předávání v názvu produktu s, cena, a `ProductID`. Obslužné rutiny události dokončení vraťte se na stav předem úpravy pomocí přesně stejné logiky, jako v prvku DataList `CancelCommand` obslužné rutiny události.

Pomocí `EditCommand`, `CancelCommand`, a `UpdateCommand` dokončit obslužné rutiny událostí, návštěvníka můžete upravit název a cena produktu. Následující obrázky 14 až 16 zobrazit tento pracovní postup úpravy v akci.


[![Při první návštěvě stránky, všechny produkty, které jsou v režimu jen pro čtení](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Obrázek 14**: při první návštěvě stránky, všechny produkty, které jsou v režimu jen pro čtení ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![K aktualizaci produktu s název nebo cena, klikněte na tlačítko Upravit](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Obrázek 15**: Chcete-li aktualizovat produkt s názvem nebo ceny, klikněte na tlačítko Upravit ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Po změně hodnoty, kliknutím na tlačítko Aktualizovat vraťte se do režimu jen pro čtení](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Obrázek 16**: po změně hodnoty, klikněte na Aktualizovat vraťte se do režimu jen pro čtení ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Krok 7: Přidání schopností odstranění

Kroky pro přidání funkcí odstranění do DataList jsou podobná těm pro přidání možností úprav. Stručně řečeno, je potřeba přidat tlačítko Odstranit `ItemTemplate` , při kliknutí na:

1. Přečte v odpovídající produktu s `ProductID` prostřednictvím `DataKeys` kolekce.
2. Provede odstranění voláním `ProductsBLL` třídu s `DeleteProduct` metoda.
3. Znovu připojí data k prvku DataList.

Umožní s Začněte přidáním tlačítko Odstranit `ItemTemplate`.

Při kliknutí na tlačítko jejichž `CommandName` je upravit, aktualizaci nebo Storno vyvolá DataList s `ItemCommand` událostí společně s další událost (například při použití úprav `EditCommand` událost se vyvolá také). Podobně žádné tlačítko, LinkButton nebo ImageButton v prvku DataList jejichž `CommandName` je nastavena na odstranění příčin `DeleteCommand` na aktivaci události (spolu s `ItemCommand`).

Přidání tlačítka Odstranit u tlačítka Upravit v `ItemTemplate`, nastavení jeho `CommandName` vlastnost k odstranění. Po přidání toto tlačítko řízení vaší DataList s `ItemTemplate` deklarativní syntaxe by měl vypadat jako:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Dále vytvořte obslužnou rutinu události pro DataList s `DeleteCommand` událostí, pomocí následujícího kódu:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Kliknutím na tlačítko Odstranit způsobí, že zpětné volání a aktivuje DataList s `DeleteCommand` událostí. V obslužné rutině události kliknutelnou produktu s `ProductID` hodnota je k němu přistupovat z `DataKeys` kolekce. Dále je produkt odstraněno voláním `ProductsBLL` třídu s `DeleteProduct` metoda.

Po odstranění produktu, je důležité, že jsme rebind data, která mají DataList s (`DataList1.DataBind()`), jinak bude pokračovat DataList zobrazíte produkt, který byl právě odstraněn.

## <a name="summary"></a>Souhrn

Při prvku DataList chybí bodem a klikněte na tlačítko úpravy a odstraňování podporu líbilo podle GridView, s krátkou bit kódu ji můžete vylepšit zahrnout tyto funkce. V tomto kurzu jsme viděli, jak vytvořit dva sloupce seznam produktů Nepodařilo se odstranit a jejichž název a cena může jej upravit. Přidání, úpravy a odstraňování podporu je řádu včetně odpovídající ovládací prvky webového v `ItemTemplate` a `EditItemTemplate`, odpovídající obslužné rutiny událostí vytváření, čtení hodnoty klíče uživatel zadal a primární a propojování s firmy Vrstva logiky.

Když jsme přidali základní úpravy a odstraňování funkcí, které DataList, nemá pokročilejší funkce. Například neexistuje žádné vstupní pole ověření – Pokud uživatel zadá cena příliš nákladné, bude vyvolána výjimka podle `Decimal.Parse` při pokusu o převod příliš nákladné do `Decimal`. Podobně pokud došlo k potížím při aktualizaci data na obchodní logiku nebo datové vrstvy přístup uživatele zobrazí standardní chyba obrazovku. Bez jakéhokoli druhu potvrzení na tlačítko Odstranit je všechny moc pravděpodobné nechtěnému odstranění produktu.

V budoucích kurzy ukážeme, jak ke zlepšení úpravy uživatelské prostředí.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Zack Petr, Ken Pespisa a Randy Schmidt. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](customizing-the-datalist-s-editing-interface-cs.md)
[další](performing-batch-updates-vb.md)
