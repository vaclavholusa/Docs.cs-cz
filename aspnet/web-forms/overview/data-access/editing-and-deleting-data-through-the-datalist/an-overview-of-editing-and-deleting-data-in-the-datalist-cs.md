---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Přehled úprav a odstraňování dat v ovládacím prvku DataList (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Zatímco prvku DataList nemá vestavěné úpravy a odstranění funkce, v tomto kurzu jsme ukážeme, jak k vytvoření ovládacích prvků DataList, který podporuje úpravy a odstranění o...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: a9be3e332ec19f78c4dcc2e78d3dd4609c27fddf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810482"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Přehled úprav a odstraňování dat v ovládacím prvku DataList (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) nebo [stahovat PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Zatímco prvku DataList nemá vestavěné úpravy a odstranění funkce, v tomto kurzu uvidíme vytvoření ovládacích prvků DataList, který podporuje úpravy a odstraňování podkladová data.


## <a name="introduction"></a>Úvod

V [přehled o vložení, aktualizace a odstranění dat](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) kurzu jsme se podívali na tom, jak vkládat, aktualizovat a odstraňovat data pomocí architektury aplikace, ObjectDataSource a GridView DetailsView a FormView ovládací prvky. ObjectDataSource a webových ovládacích prvcích tyto tři dat implementace rozhraní jednoduché datové úpravy se stává hračkou a zahrnuty pouze zaškrtávacím z inteligentních značek. Žádný kód potřebný k zapsání.

Bohužel chybí prvku DataList integrovaných úpravy a odstranění možnosti spočívající v ovládacím prvku GridView. Tato funkce chybí je v části by se skutečnost, že je prvku DataList relic z předchozí verze technologie ASP.NET, pokud ovládací prvky deklarativní data zdroje a úprava stránky dat bez použití kódu byla k dispozici. Zatímco DataList v technologii ASP.NET 2.0 nenabízí stejné předdefinované možnosti úprav data jako prvku GridView, jsme techniky 1.x technologie ASP.NET použijte k zahrnutí těchto funkcí. Tento přístup vyžaduje hodně kódu, ale jak uvidíme v tomto kurzu, prvku DataList má některé události a vlastnosti na místě, které vám pomůže tento proces.

V tomto kurzu uvidíme, jak vytvořit ovládacích prvků DataList, který podporuje úpravy a odstraňování podkladová data. Další kurzy prozkoumá pokročilejší úpravy a odstranění scénářů, včetně ověřování vstupních polí, bez výpadku zpracování výjimky vyvolané z přístupu k datům nebo vrstvy obchodní logiky a tak dále.

> [!NOTE]
> Stejně jako prvku DataList chybí ovládacím prvku opakovače mimo pole funkce pro vkládání, aktualizace nebo odstranění. Zatímco tato funkce je možné přidat, prvku DataList obsahuje vlastnosti a události nebyl nalezen v Opakovači, které zjednodušují přidání těchto funkcí. Proto v tomto kurzu, podívejte se na úpravy a odstranění budoucí které se zaměřuje výhradně na ovládacím prvku DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Krok 1: Vytváření, úpravy a odstraňování kurzy webových stránek

Než začneme, aktualizaci a odstranění dat z a v prvku DataList zkoumání, umožní s nejdřív využít k vytvoření stránky technologie ASP.NET v našem projektu webu, které potřebujeme pro tento kurz a další několik předpon. Začněte přidáním novou složku s názvem `EditDeleteDataList`. Dále přidejte následující stránky ASP.NET do této složky, nezapomeňte přiřadit každou stránku s `Site.master` hlavní stránky:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Přidání stránky technologie ASP.NET pro kurzy](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Obrázek 1**: Přidání stránky technologie ASP.NET pro kurzy


V jiných složkách, jako jsou `Default.aspx` v `EditDeleteDataList` složka obsahuje seznam kurzů v příslušném oddílu. Vzpomeňte si, že `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek tuto funkci poskytuje. Proto přidat tento uživatelský ovládací prvek `Default.aspx` přetažením v Průzkumníku řešení na stránku s návrhové zobrazení.


[![Přidat na stránku Default.aspx SectionLevelTutorialListing.ascx uživatelského ovládacího prvku](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Obrázek 2**: Přidejte `SectionLevelTutorialListing.ascx` uživatelský ovládací prvek `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


A konečně, přidejte na stránkách jako položky `Web.sitemap` souboru. Konkrétně, přidejte následující kód za záznamů Master/Detail sestavy ovládacími prvky DataList a Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Po aktualizaci `Web.sitemap`, věnujte chvíli zobrazit kurzy web prostřednictvím prohlížeče. V nabídce na levé straně teď obsahuje položky pro DataList úpravy a odstranění kurzy.


![Mapa webu nyní obsahuje záznamy pro DataList úpravy a odstranění kurzy](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Obrázek 3**: mapy webu nyní obsahuje záznamy pro DataList úpravy a odstranění kurzy


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Krok 2: Zkoumání techniky pro aktualizaci a odstraňování dat

Úpravy a odstraňování dat ovládacím prvkem prvku GridView. je tak snadné, protože na pozadí ovládacího prvku GridView a ObjectDataSource fungovat společně. Jak je popsáno v [zkoumání události spojené s vložení, aktualizace a odstranění](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) kurzu při kliknutí na tlačítko Aktualizovat řádek s prvku GridView automaticky přiřadí polí, které používají dvousměrnou datovou vazbou k `UpdateParameters` kolekce jeho ObjectDataSource a potom vyvolá tohoto prvku ObjectDataSource s `Update()` metody.

Bohužel prvku DataList neposkytuje žádné této vestavěné funkce. Je našich povinností ujistit se, že uživatel s hodnoty jsou přiřazeny k prvku ObjectDataSource s parametry a že jeho `Update()` metoda je volána. Pokud chcete nám pomoci při tomto zařízeními, prvku DataList poskytuje následující vlastnosti a události:

- **[ `DataKeyField` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  při aktualizaci nebo odstranění, musíme být schopni jednoznačně identifikovat každou položku v ovládacím prvku DataList. Nastavte tuto vlastnost na pole primárního klíče jsou data zobrazená. Tím se vyplní DataList s [ `DataKeys` kolekce](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) se zadaným `DataKeyField` hodnotu pro každou položku v prvku DataList.
- **[ `EditCommand` Události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  je vyvoláno, když tlačítko, odkazem (LinkButton) nebo ImageButton jehož `CommandName` je nastavena na kliknutí na Upravit.
- **[ `CancelCommand` Události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  je vyvoláno, když tlačítko, odkazem (LinkButton) nebo ImageButton jehož `CommandName` je nastavena na kliknutí na zrušit.
- **[ `UpdateCommand` Události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  je vyvoláno, když tlačítko, odkazem (LinkButton) nebo ImageButton jehož `CommandName` je nastavena na dojde ke kliknutí na aktualizace.
- **[ `DeleteCommand` Události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  je vyvoláno, když tlačítko, odkazem (LinkButton) nebo ImageButton jehož `CommandName` je nastavena na kliknutí na odstranit.

Pomocí těchto vlastností a událostí, existují čtyři přístupy, které můžeme použít k aktualizaci a odstraňování dat v ovládacím prvku DataList:

1. **Pomocí technik 1.x ASP.NET** prvku DataList existovaly před ASP.NET 2.0 a ObjectDataSources a bylo možné aktualizovat a odstraňovat data programové způsoby. Tato technika kanálů ObjectDataSource úplně a vyžaduje, že jsme svázat data prvku DataList přímo z vrstvy obchodní logiky, jak při načítání dat pro zobrazení a při aktualizaci nebo odstranění záznamu.
2. **Pomocí jednoho ovládacího prvku ObjectDataSource na stránce pro výběr, aktualizace a odstranění** při prvku DataList chybí GridView s vlastní úpravy a odstranění možnosti, existovat s bez důvodu, můžeme t je přidejte v sami. S tímto přístupem můžeme používat prvku ObjectDataSource stejně jako příklady ovládacího prvku GridView, ale musíte vytvořit obslužnou rutinu události pro DataList s `UpdateCommand` událostí, kde jsme si nastavili prvku ObjectDataSource s parametry a volání jeho `Update()` metoda.
3. **Použití ovládacího prvku ObjectDataSource pro výběr, ale aktualizace a odstranění přímo proti BLL** při použití možnosti 2, musíme psát hodně kódu v `UpdateCommand` události přiřazení hodnoty parametrů a tak dále. Místo toho jsme můžete zůstat u pomocí ObjectDataSource pro výběr, ale volání aktualizaci a odstraňování přímo proti BLL (podobně jako s možnost 1). Moje názoru aktualizace dat pomocí rozhraní přímo k BLL vede k kód čitelnější než přiřazení ObjectDataSource s `UpdateParameters` a volání jeho `Update()` metoda.
4. **Deklarativní způsobem prostřednictvím více ObjectDataSources** předchozích všechny tři přístupů vyžadují hodně kódu. Pokud vám d spíše dál používat jako mnohem deklarativní syntaxe nejvíce, posledním způsobem je zahrnout více ObjectDataSources na stránce. První prvek ObjectDataSource načte data z knihoven BLL a provádí vazbu na ovládacím prvku DataList. Aktualizace, je jiný prvek ObjectDataSource přidán, ale přidat přímo v prvku DataList s `EditItemTemplate`. Pokud chcete zahrnout, odstranění podpory, ještě další prvek ObjectDataSource, bylo by potřeba v `ItemTemplate`. S tímto přístupem tyto vložené pomocí prvku ObjectDataSource s `ControlParameters` deklarativně vytvořit vazbu s parametry ObjectDataSource uživatelské ovládací prvky vstupu (místo nutnosti zadávat programově v ovládacím prvku DataList s `UpdateCommand` obslužná rutina události). Tento přístup i nadále vyžaduje hodně kódu, které potřebujeme pro volání vloženého prvku ObjectDataSource s `Update()` nebo `Delete()` příkazu, ale vyžaduje mnohem menší než k ostatním tři přístupů. Tady nevýhodou je, že více ObjectDataSources zatěžovat stránce zasahoval celkové čitelnost.

Pokud se musí použít vždy jen jeden z těchto přístupů, můžu d výběrem možnosti 1 vzhledem k tomu, že nabízí největší flexibilitu a prvku DataList byla původně určena k přizpůsobení tohoto modelu. Když prvku DataList se rozšířilo pro práci s ovládací prvky zdroje dat ASP.NET 2.0, nemá všechny body rozšiřitelnosti nebo funkce oficiální data technologie ASP.NET 2.0 webové ovládací prvky (ovládacího prvku GridView, DetailsView a FormView). 2 až 4 nejsou možnosti bez naučit, i když.

To budoucí úpravy a odstranění kurzy použije prvku ObjectDataSource pro načítání dat pro zobrazení a přímé volání BLL aktualizovat a odstraňovat data (možnost 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Krok 3: Přidání prvku DataList a konfigurace jeho prvku ObjectDataSource

V tomto kurzu vytvoříme DataList, který obsahuje informace o produktu a pro jednotlivé produkty, nabídne uživateli možnost upravit název a cena a úplně odstranit produkt. Konkrétně jsme načte záznamy, které chcete zobrazit, pomocí ObjectDataSource, ale provést aktualizaci a odstranit některé akce podle propojení přímo s BLL. Předtím, než jsme se starat o implementaci možnosti úprav a odstranění prvku DataList, umožní s nejdřív získat na stránce zobrazit produkty v rozhraní jen pro čtení. Protože jsme ve prozkoumat tyto kroky v předchozích kurzech se bude pokračovat jejich prostřednictvím rychle.

Začněte otevřením `Basics.aspx` stránku `EditDeleteDataList` složku a v návrhovém zobrazení, přidat na stránku a v prvku DataList. Potom z inteligentních značek v prvku DataList s vytvořte nový prvek ObjectDataSource. Protože Pracujeme s daty produktu, nakonfigurujte ho na použití `ProductsBLL` třídy. K načtení *všechny* produkty, zvolte `GetProducts()` metoda v kartě vyberte.


[![Konfigurace ObjectDataSource pomocí třídy ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Obrázek 4**: Konfigurace ObjectDataSource k použití `ProductsBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Vrátí informace o produktu pomocí GetProducts() – metoda](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Obrázek 5**: vrátit informací pomocí produktu `GetProducts()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList, jako je prvku GridView, není určená pro vkládání nových dat; proto vyberte (žádný) možnost z rozevíracího seznamu na kartě Vložení. Také (žádný) zvolte pro karty UPDATE a DELETE od aktualizace a odstranění se provádí prostřednictvím kódu programu prostřednictvím BLL.


[![Potvrďte, že rozevírací seznamy v prvku ObjectDataSource s vložení, aktualizace a odstranění karty jsou nastaveny na (žádný)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Obrázek 6**: Ověřte, že rozevírací seznamy v prvku ObjectDataSource s INSERT, UPDATE a odstranit karty jsou nastaveny na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


Po dokončení konfigurace ObjectDataSource, klikněte na tlačítko Dokončit, vrátí do návrháře. Jako jsme viděli v posledních příklady po dokončení konfigurace prvek ObjectDataSource, Visual Studio automaticky odebrat vytvoří `ItemTemplate` pro DropDownList, zobrazení všech datových polí. Nahraďte `ItemTemplate` úlohou, která se zobrazí jenom produkt s názvem a ceny. Také nastavit, `RepeatColumns` vlastnost na 2.

> [!NOTE]
> Jak je popsáno v *Přehled vložení, aktualizace a odstranění dat* kurzu při upravování dat pomocí prvku ObjectDataSource Naše architektura vyžaduje, jsme odebrali `OldValuesParameterFormatString` vlastnost z prvku ObjectDataSource s deklarativní (nebo ho resetovat na výchozí hodnotu, `{0}`). V tomto kurzu ale použijeme ObjectDataSource pouze pro načítání dat. Proto jsme není potřeba upravovat ObjectDataSource s `OldValuesParameterFormatString` hodnotu vlastnosti (i když to narušit t kódu k tomu).


Po nahrazení výchozí DataList `ItemTemplate` vlastní sadou deklarativní na stránce by měl vypadat podobně jako následujícím:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Chcete-li zobrazit náš postup prostřednictvím prohlížeče chvíli trvat. Obrázek 7 znázorňuje, zobrazuje prvku DataList produktu název a Jednotková cena pro jednotlivé produkty ve dvou sloupcích.


[![V prvku DataList dvěma sloupci se zobrazují názvy produktů a ceny](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Obrázek 7**: The názvy produktů a ceny jsou zobrazeny v prvku DataList dvěma sloupci ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> Má několik vlastností, které jsou požadovány pro proces aktualizace a odstranění prvku DataList a tyto hodnoty jsou uloženy v zobrazení stavu. Proto se při sestavování a v prvku DataList, která podporuje úpravy nebo odstranění dat, je nezbytné povolit stav zobrazení v prvku DataList s.  
>   
>  Bystří čtenáři mohou si možná Vzpomínáte, že jsme byli schopni zakázat stavu zobrazení při vytváření upravitelné prvků GridViews DetailsViews a FormViews. Důvodem je, že může obsahovat ovládacích prvků technologie ASP.NET 2.0 *stav ovládacích prvků*, což je stav ukládaný postbacků jako stav zobrazení, ale domnělého nezbytné.


Zakazuje zobrazení stavu v prvku GridView pouze vynechá informace o stavu triviální, ale zachová stav ovládacího prvku (které zahrnuje stav nezbytné pro úpravy a odstranění). Prvku DataList s byly vytvořeny v časovém rámci 1.x technologie ASP.NET, nevyužívá stav ovládacího prvku a proto musí mít stav zobrazení povolený. Zobrazit [vs stav ovládacího prvku. Zobrazení stavu](https://msdn.microsoft.com/library/1whwt1k7.aspx) Další informace o účelu stav ovládacího prvku a jak se liší od zobrazení stavu.

## <a name="step-4-adding-an-editing-user-interface"></a>Krok 4: Přidáním úpravy uživatelského rozhraní

Ovládací prvek GridView se skládá z kolekce polí (BoundFields CheckBoxFields, vlastností TemplateField a tak dále). Tato pole můžete upravit jejich vykreslované značky v závislosti na jejich režimu. V režimu jen pro čtení, vlastnost BoundField zobrazí jeho hodnoty datového pole jako text. v režimu úprav se vykresluje textové pole webové ovládací prvek, jehož `Text` vlastnost je přiřazena hodnota pole data.

Prvku DataList vykreslí na druhé straně jeho položek pomocí šablony. Jen pro čtení položky jsou vykreslovány pomocí `ItemTemplate` že položky v režimu úprav jsou vykreslovány pomocí `EditItemTemplate`. V tomto okamžiku naše DataList má pouze `ItemTemplate`. K podpoře funkce na úrovni položek pro úpravy je potřeba přidat `EditItemTemplate` , která obsahuje kód, který má být zobrazen pro upravovat položky. Pro účely tohoto kurzu používáme TextBox webových ovládacích prvcích pro úpravy produkt s názvem a Jednotková cena.

`EditItemTemplate` Lze vytvořit pomocí deklarace nebo prostřednictvím návrháře (tak, že vyberete možnost Upravit šablony ovládacích prvků DataList s inteligentním). Chcete-li použít možnost Upravit šablony, nejprve klikněte na odkaz Upravit šablony v inteligentní značky a pak vyberte `EditItemTemplate` položku z rozevíracího seznamu.


[![Optimalizované pro práci s DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Obrázek 8**: optimalizované pro práci s DataList s `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Potom zadejte název produktu: a cena: a pak přetáhněte z panelu nástrojů do dvou ovládacích prvků textového pole `EditItemTemplate` rozhraní v návrháři. Nastavení textových polí `ID` vlastností `ProductName` a `UnitPrice`.


[![Přidat textové pole s názvem produktu a ceny](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Obrázek 9**: Přidání textového pole pro produkt s názvem a cena ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


Potřebujeme vytvořit vazbu odpovídající produktu pole hodnoty dat `Text` vlastnosti dvě textová pole. Z textových polí inteligentní značky, klikněte na odkaz Upravit datové vazby a přidružte příslušné datové pole s `Text` vlastnost, jak je znázorněno na obrázku 10.

> [!NOTE]
> Při vytváření vazby `UnitPrice` datové pole s cenou při textové pole s `Text` pole, které může naformátujeme ho jako hodnotu měny (`{0:C}`), Obecné číslo (`{0:N}`), nebo nechte neformátovaném tvaru.


![Pole ProductName a UnitPrice Data svázat vlastnosti Text textových polí](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Obrázek 10**: vytvoření vazby `ProductName` a `UnitPrice` do pole Data `Text` vlastnosti textových polí


Všimněte si, jak se dialogové okno Upravit datové vazby na obrázku 10 *není* patří Obousměrná vazba dat zaškrtávacího políčka, která je k dispozici při úpravách TemplateField v prvku GridView nebo prvku DetailsView nebo šablony ve třídě FormView. Funkce Obousměrná vazba dat povolené hodnoty zadané do vstupní ovládací prvek webového má být automaticky přiřazena k odpovídající prvku ObjectDataSource s `InsertParameters` nebo `UpdateParameters` při vkládání nebo aktualizaci data. Prvku DataList nepodporuje Obousměrná vazba dat, jak uvidíme dále v tomto kurzu se po uživatel provede své změny a je připravený k aktualizaci dat, budeme muset programový přístup k těmto textových polí `Text` vlastnosti a předejte jí jejich hodnot tak, odpovídající `UpdateProduct` metodu `ProductsBLL` třídy.

Nakonec musíme přidat aktualizace tlačítka a Storno `EditItemTemplate`. Jak jsme viděli v [Master/Detail, použití odrážkovém seznamu v hlavních záznamů s podrobnostmi v prvku DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) kurz, když tlačítko, odkazem (LinkButton) nebo ImageButton jehož `CommandName` je nastavena dojde ke kliknutí na z v rámci, v prvku DataList a Repeater Repeater nebo DataList s `ItemCommand` událost se vyvolá. Pro DataList Pokud `CommandName` je nastavena na určitou hodnotu, také mohou být vyvolány další události. Speciální `CommandName` hodnoty vlastnosti zahrnují mimo jiné:

- Zrušit vyvolá `CancelCommand` událostí
- Upravit vyvolá `EditCommand` událostí
- Aktualizovat vyvolá `UpdateCommand` událostí

Mějte na paměti, že tyto události jsou vyvolány *kromě* `ItemCommand` událostí.

Přidat `EditItemTemplate` dvě tlačítka webové ovládací prvky, jehož `CommandName` je nastavena na aktualizace a další prostředky nastavena na Storno. Po přidání těchto dvou ovládacích prvků tlačítko webové návrháře by měl vypadat nějak takto:


[![Přidat aktualizace tlačítka EditItemTemplate a Storno](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Obrázek 11**: zrušit tlačítek a přidat aktualizovat `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


S `EditItemTemplate` kompletní DataList s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Krok 5: Přidání vložení do režimu úprav

V tomto okamžiku naše DataList má úpravy rozhraní definované prostřednictvím jeho `EditItemTemplate`, nicméně tam s aktuálně žádný způsob, jak uživatel na naši stránku k označení, že chce upravit informace o produktu s. Potřebujeme přidat tlačítko pro úpravy u každého produktu, že po kliknutí na vykreslení tohoto prvku DataList položky v režimu úprav. Začněte tím, že přidáte tlačítko pro úpravy k `ItemTemplate`, prostřednictvím návrháře nebo deklarativně. Tlačítko pro úpravy s nastavena `CommandName` vlastnost na hodnotu upravit.

Po přidání tohoto tlačítka Upravit, věnujte chvíli zobrazíte stránku prostřednictvím prohlížeče. Uveďte by měly zahrnovat každý výpis tlačítko pro úpravy.


[![Přidat aktualizace tlačítka EditItemTemplate a Storno](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Obrázek 12**: zrušit tlačítek a přidat aktualizovat `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Kliknutím na tlačítko vyvolá zpětné volání, ale nemá *není* přineste produkt výpis do režimu úprav. Chcete-li upravit produktu, potřebujeme:

1. Nastavení ovládacích prvků DataList s [ `EditItemIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) k indexu `DataListItem` právě kliknutí na tlačítko jehož upravit.
2. Obnovení vazby dat k ovládacím prvku DataList. Po opětovné vykreslené prvku DataList `DataListItem` jehož `ItemIndex` odpovídá prvku DataList s `EditItemIndex` se vykreslit pomocí jeho `EditItemTemplate`.

Od prvku DataList s `EditCommand` událost je aktivována při kliknutí na tlačítko pro úpravy, vytváření `EditCommand` obslužné rutiny události s následujícím kódem:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

`EditCommand` Obslužná rutina události je předán objekt typu `DataListCommandEventArgs` jako svůj druhý vstupní parametr, který obsahuje odkaz na `DataListItem` došlo ke kliknutí na jehož tlačítko Upravit (`e.Item`). Obslužná rutina události nejprve nastaví DataList s `EditItemIndex` k `ItemIndex` z upravitelnou `DataListItem` a pak znovu připojí data k prvku DataList voláním DataList s `DataBind()` metoda.

Po přidání této obslužné rutiny události, otevírat stránku v prohlížeči. Teď kliknutím na tlačítko Upravit umožňuje kliknutí na produkt upravitelné (viz obrázek 13).


[![Kliknutím na tlačítko umožňuje úpravy upravitelné produktu](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Obrázek 13**: Kliknutím na tlačítko Upravit umožňuje upravovat produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Krok 6: Ukládá změny s uživatelů

Kliknutím na upravený produktu s aktualizací nebo tlačítka Storno nemá žádný účinek, v tuto chvíli; Chcete-li přidat tuto funkci, která potřebujeme k vytvoření obslužné rutiny událostí pro DataList s `UpdateCommand` a `CancelCommand` události. Začněte vytvořením `CancelCommand` obslužnou rutinu události, která se spustí, když dojde ke kliknutí na tlačítko Storno upravených produktu s a za úkol ji vrací do stavu před úpravy prvku DataList.

Pokud chcete, aby DataList vykreslit všechny položky v režimu jen pro čtení, potřebujeme:

1. Nastavení ovládacích prvků DataList s [ `EditItemIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) index neexistující `DataListItem` indexu. `-1` od té doby je bezpečná volba `DataListItem` indexy začínají `0`.
2. Obnovení vazby dat k ovládacím prvku DataList. Od ne `DataListItem` `ItemIndex` es odpovídají DataList s `EditItemIndex`, zobrazí se celá DataList v režimu jen pro čtení.

Tyto kroky můžete provést pomocí následující kód obslužné rutiny události:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Uveďte kliknutím na vrátí tlačítko Storno prvku DataList do předem úprav stavu.

Je poslední obslužnou rutinou události musíme provést `UpdateCommand` obslužné rutiny události. Tato obslužná rutina události je potřeba:

1. Programový přístup k názvu uživatel zadal produktu a cena, jakož i upravených produktu s `ProductID`.
2. Zahájení aktualizace voláním příslušné `UpdateProduct` přetížení v `ProductsBLL` třídy.
3. Nastavení ovládacích prvků DataList s [ `EditItemIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) index neexistující `DataListItem` indexu. `-1` od té doby je bezpečná volba `DataListItem` indexy začínají `0`.
4. Obnovení vazby dat k ovládacím prvku DataList. Od ne `DataListItem` `ItemIndex` es odpovídají DataList s `EditItemIndex`, zobrazí se celá DataList v režimu jen pro čtení.

Kroky 1 a 2 zodpovídají za uložení uživatele s změny. kroky 3 a 4 vrátit prvku DataList do předem úprav stavu po změny se uložily a jsou stejné jako v postupu `CancelCommand` obslužné rutiny události.

Pokud chcete získat název aktualizace produktu a ceny, musíme použít `FindControl` metodu programový odkaz webové TextBox – ovládací prvky v rámci `EditItemTemplate`. Je také potřeba získat upravený produktu s `ProductID` hodnotu. Když jsme původně vazba ObjectDataSource na ovládacím prvku DataList, Visual Studio přiřazené DataList s `DataKeyField` nastavte na hodnotu primárního klíče ze zdroje dat (`ProductID`). Tuto hodnotu můžete pak získat z prvku DataList s `DataKeys` kolekce. Ujistěte se, že za chvíli `DataKeyField` je skutečně nastavena na `ProductID`.

Následující kód implementuje čtyři kroky:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Spustí obslužnou rutinu události, přečtěte si téma v upravených produktu s `ProductID` z `DataKeys` kolekce. Další dvě textová pole v `EditItemTemplate` odkazují a jejich `Text` vlastnosti uložené v místních proměnných, `productNameValue` a `unitPriceValue`. Používáme `Decimal.Parse()` metodu za účelem čtení hodnoty z `UnitPrice` textové pole tak, že pokud zadaná hodnota obsahuje symbol měny, se můžete stále správně převést na `Decimal` hodnotu.

> [!NOTE]
> Hodnoty z `ProductName` a `UnitPrice` textová pole jsou pouze přiřazeny proměnné productNameValue a unitPriceValue Pokud vlastnosti Text textová pole mají hodnotu zadat. V opačném případě hodnota `Nothing` se používá pro proměnné, která má vliv na aktualizace dat pomocí databáze `NULL` hodnotu. To znamená, náš kód zpracovává převede prázdné řetězce k databázi `NULL` hodnoty, které je výchozí chování úpravy rozhraní v ovládacích prvcích ovládacího prvku GridView, DetailsView a FormView.


Po přečtení hodnoty `ProductsBLL` třída s `UpdateProduct` metoda je volána, předávání v názvu produktu s, ceny a `ProductID`. Obslužná rutina události dokončení tak, že vrací stavu předem úpravy pomocí přesně stejné logiky jako v prvku DataList `CancelCommand` obslužné rutiny události.

S `EditCommand`, `CancelCommand`, a `UpdateCommand` dokončení obslužné rutiny událostí, návštěvník můžete upravit název a cena produktu. Obrázky 14 – 16 zobrazit tento pracovní postup úpravy v akci.


[![Při první návštěvě stránky, všechny produkty jsou v režimu jen pro čtení](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Obrázek 14**: při první návštěvě stránky, všechny produkty jsou v režimu jen pro čtení ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Aktualizace produktu s názvem ani cena, klikněte na tlačítko Upravit](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Obrázek 15**: Aktualizujte produkt s názvem ani cena, kliknutím na tlačítko Upravit ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Po změně hodnoty, klikněte na tlačítko Aktualizovat pro návrat do režimu jen pro čtení](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Obrázek 16**: po změně hodnoty, klikněte na Aktualizovat vrátit do režimu jen pro čtení ([kliknutím ji zobrazíte obrázek v plné velikosti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Krok 7: Přidání možností odstranit

Postup přidání odstranit možnosti a v prvku DataList jsou podobná těm pro přidání funkce pro úpravy. Stručně řečeno, je potřeba přidat tlačítko pro odstranění k `ItemTemplate` , při kliknutí na:

1. Přečte v odpovídající produkt s `ProductID` prostřednictvím `DataKeys` kolekce.
2. Provede odstranění voláním `ProductsBLL` třída s `DeleteProduct` metody.
3. Znovu připojí data k prvku DataList.

Umožní s spustit tak, že přidáte tlačítko pro odstranění k `ItemTemplate`.

Po kliknutí na tlačítko jehož `CommandName` znamená úpravy, aktualizaci nebo Storno vyvolá DataList s `ItemCommand` události společně s další události (například při použití úprav `EditCommand` událost se vyvolá také). Podobně jakékoli tlačítko, odkazem (LinkButton) nebo ImageButton v ovládacím prvku DataList jehož `CommandName` je nastavena na odstranění příčiny `DeleteCommand` na aktivaci události (spolu s `ItemCommand`).

Přidejte tlačítko pro odstranění vedle tlačítka Upravit v `ItemTemplate`a nastavte jeho `CommandName` vlastnosti k odstranění. Po přidání tohoto tlačítka, ovládací prvek třídy DataList s `ItemTemplate` deklarativní syntaxe by měl vypadat takto:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Dále vytvořte obslužnou rutinu události pro DataList s `DeleteCommand` událostí, pomocí následujícího kódu:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Kliknutím na tlačítko Odstranit vyvolá zpětné volání a aktivuje se v ovládacím prvku DataList s `DeleteCommand` událostí. V obslužné rutině události kliknutí na produkt s `ProductID` hodnotu přistupuje z `DataKeys` kolekce. V dalším kroku se odstraní produktu voláním `ProductsBLL` třída s `DeleteProduct` metody.

Po odstranění produktu, je důležité, že jsme obnovení vazby dat k ovládacím prvku DataList (`DataList1.DataBind()`), jinak prvku DataList nadále zobrazovat produktu, který byl právě odstraněn.

## <a name="summary"></a>Souhrn

Zatímco prvku DataList chybí bodu a klikněte na možnost úprav a odstranění podpory líbila podle prvku GridView, s krátkou bitového kódu se dá vylepšit zahrnout tyto funkce. V tomto kurzu jsme viděli, jak vytvořit dva sloupce seznam produktů, které není možné odstranit a může upravovat jehož názvu a ceny. Přidání, úpravy a odstranění podpory je otázkou včetně příslušné webové ovládací prvky `ItemTemplate` a `EditItemTemplate`vytváření odpovídající obslužné rutiny událostí, čtení hodnot zadaných uživatelem a primárního klíče a propojení s obchodní Vrstvy logiky.

Když jsme přidali základní úpravy a odstranění možnosti prvku DataList, nemá další pokročilé funkce. Například neexistuje žádné ověřování vstupních polí – Pokud uživatel zadá cenu příliš drahé, výjimky budou vyvolány metodou `Decimal.Parse` při pokusu o převod příliš drahé do `Decimal`. Podobně, pokud dojde k potížím při aktualizaci dat na obchodní logiku nebo vrstvy přístupu k datům uživateli zobrazí obrazovky pro standardní chybu. Bez jakéhokoli druhu potvrzení na tlačítku pro odstranění nechtěného odstranění produktu je příliš málo pravděpodobné.

V budoucích kurzech uvidíme, jak vylepšit uživatelské úpravy prostředí.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Zack Jones, Ken Pespisa a Randy Schmidt. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](performing-batch-updates-cs.md)
