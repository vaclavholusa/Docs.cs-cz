---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: "Zobrazení dat s ObjectDataSource (C#) | Microsoft Docs"
author: rick-anderson
description: "Tento kurz se zabývá ovládacího prvku ObjectDataSource pomocí tohoto ovládacího prvku lze vázat data načtená z BLL vytvořili v předchozí kurzu bez havi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bd6534b652735e657aa71cdf07dac48f20a549c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Zobrazení dat s ObjectDataSource (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) nebo [stáhnout PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> Tento kurz se zabývá ovládacího prvku ObjectDataSource pomocí tohoto ovládacího prvku lze vázat data načtená z BLL vytvořili v předchozí kurzu bez nutnosti psaní řádek kódu!


## <a name="introduction"></a>Úvod

Naše aplikace architekturu a webu stránky rozložení dokončení jsme připraveni a začněte se seznamovat jak provést celou řadu běžných úloh a vytváření sestav-souvisejících s daty. V předchozích kurzech jsme viděli jak programově vázat data ze DAL a BLL k data ovládacího prvku webové stránky ASP.NET. Tuto syntaxi přiřazování dat ovládací prvek webu `DataSource` vlastnost, která má data k zobrazení a potom volání ovládacího prvku `DataBind()` metoda byla způsobem používaným v aplikacích ASP.NET 1.x a můžete nadále používat v aplikacích 2.0. ASP.NET 2.0 nové ovládací prvky zdroje dat však nabízí deklarativní způsob, jak pracovat s daty. Pomocí těchto ovládacích prvků můžete vázat data načtená z BLL vytvořené v [předchozí kurzu](../introduction/creating-a-business-logic-layer-cs.md) bez nutnosti psaní řádek kódu!

ASP.NET 2.0 se dodává s ovládacími prvky zdroje pět předdefinovaných dat [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [prvku](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), a [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) i když můžete vytvořit vlastní [vlastní datové zdroje ovládací prvky](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), v případě potřeby. Vzhledem k tomu, že Vyvinuli jsme architekturu pro naše aplikace, budeme používat ObjectDataSource proti naše BLL třídy.


![ASP.NET 2.0 obsahuje pět předdefinovaných dat zdroje ovládacích prvků](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Obrázek 1**: ASP.NET 2.0 obsahuje pět předdefinovaných dat zdroje ovládacích prvků


ObjectDataSource slouží jako proxy server pro práci s některý jiný objekt. Ke konfiguraci ObjectDataSource jsme tuto verzi uveďte základní objekt a jak její metody mapovány ObjectDataSource `Select`, `Insert`, `Update`, a `Delete` metody. Jakmile byl zadán tento základní objekt a její metody mapované na ObjectDataSource, jsme pak vazbu ObjectDataSource k datům ovládací prvek webu. ASP.NET se dodává s mnoha data ovládací prvky webového, včetně GridView, DetailsView, RadioButtonList a rozevírací seznam, mimo jiné. Během životního cyklu stránky, může data ovládací prvek webu vyžadovat pro přístup k datům je vázána na, který bude provést vyvoláním jeho ObjectDataSource `Select` metoda; Pokud data ovládací prvek webu podporuje vkládání, aktualizaci, nebo odstranění, může být volání provedeno na jeho Na ObjectDataSource `Insert`, `Update`, nebo `Delete` metody. Tyto volání směrují ObjectDataSource na příslušné základní objekt metody pak, jak ukazuje následující obrázek.


[![ObjectDataSource slouží jako proxy server](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Obrázek 2**: The ObjectDataSource slouží jako proxy server ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Zatímco ObjectDataSource lze použít k vyvolání metody pro vložení, aktualizaci nebo odstranění dat, můžeme právě zaměřit se na vrácení dat; budoucí kurzy bude prozkoumat pomocí ObjectDataSource a data webové ovládací prvky, které upravovat data.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Krok 1: Přidání a konfigurace ovládacího prvku ObjectDataSource

Začněte otevřením `SimpleDisplay.aspx` stránku `BasicReporting` složky, přepněte do zobrazení návrhu a poté přetáhněte ovládacího prvku ObjectDataSource z panelu nástrojů na návrhovou plochu stránky. ObjectDataSource se zobrazí jako šedé pole na návrhovou plochu, protože nevytváří žádné značky; jednoduše přistupuje k dat vyvoláním metody ze zadaného objektu. Data vrácená ObjectDataSource lze zobrazit data ovládací prvek webu, jako je například rutina GridView, DetailsView, FormView a tak dále.

> [!NOTE]
> Alternativně můžete chtít nejdřív data přidat do ovládacího prvku webové stránky a zvolte z jeho inteligentních značek &lt;nový zdroj dat&gt; možnost z rozevíracího seznamu.


Zadejte ObjectDataSource základní objekt a jak tento objekt metody mapovány ObjectDataSource, klikněte na odkaz Konfigurace zdroje dat z ObjectDataSource inteligentních značek.


[![Klikněte konfiguraci připojení zdroje dat z inteligentní značky](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Obrázek 3**: klikněte na odkaz konfiguraci datového zdroje ze inteligentní značky ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Otevře Průvodce konfigurace zdroje dat. Nejdřív jsme musíte zadat objekt, který je ObjectDataSource pro práci s. Pokud je zaškrtnuté políčko "Zobrazit pouze data komponenty", v rozevíracím seznamu na této obrazovce jsou uvedeny pouze ty objekty, které byly označených pomocí `DataObject` atribut. Aktuálně našich seznamu jsou uvedeny TableAdapters typové datové sady a BLL třídy, které jsme vytvořili v předchozí kurzu. Pokud jste zapomněli přidat `DataObject` atribut na vrstvu obchodní logiky třídy neuvidíte je v tomto seznamu. V takovém případě zrušit zaškrtnutí políčka "Zobrazit pouze data komponenty" pro zobrazení všech objektů, které by měly obsahovat BLL třídy (spolu s jiné třídy v typové datové sady DataTables, DataRows a tak dále).

Tato první obrazovce vyberte `ProductsBLL` třídy z rozevíracího seznamu a klikněte na tlačítko Další.


[![Zadejte objekt, který chcete použít pomocí ovládacího prvku ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Obrázek 4**: Zadejte objekt, který chcete použít pomocí ovládacího prvku ObjectDataSource ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


Na další obrazovce v průvodci zobrazí výzva k výběru jakou metodu ObjectDataSource by měla vyvolat. Z rozevírací nabídky uvádí tyto metody, které vracejí dat v objektu vybrali na předchozí obrazovku. Tady vidíte `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, a `GetProductsBySupplierID`. Vyberte `GetProducts` metoda z rozevíracího seznamu a klikněte na tlačítko Dokončit (Pokud jste přidali `DataObjectMethodAttribute` k `ProductBLL`na metod, jak je znázorněno v předchozích kurzu, tato možnost bude vybrán ve výchozím nastavení).


[![Vyberte metodu pro vrácení dat, na kartě vyberte](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Obrázek 5**: zvolit metodu pro vložení dat z karty vyberte ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Ruční konfigurace prvku ObjectDataSource

Průvodce konfigurací zdroje dat ObjectDataSource nabízí rychlý způsob, jak zadat objekt, který používá a přidružit, jaké metody objektu jsou vyvolány. Můžete ale nakonfigurovat ObjectDataSource prostřednictvím jeho vlastnosti, a to buď prostřednictvím okna vlastnosti, nebo přímo v deklarativní. Jednoduše nastavit `TypeName` vlastnost typu základní objekt, který chcete použít a `SelectMethod` metodu má být vyvolán při načítání dat.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

I když dáváte přednost Průvodce konfigurace zdroje dat, které mohou nastat situace, když budete muset ručně nakonfigurovat ObjectDataSource, jako průvodce jsou uvedeny pouze vývojáře vytvořené třídy. Pokud chcete vytvořit vazbu ObjectDataSource třídu v rozhraní .NET Framework, jako [třída členství](https://msdn.microsoft.com/library/system.web.security.membership.aspx), přístup k informace o uživatelském účtu, nebo [třída Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) pro práci s informace o systému souborů budete muset ručně nastavit ObjectDataSource vlastnosti.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Krok 2: Přidání ovládacího prvku webového dat a vytvoření vazby ObjectDataSource

Jakmile ObjectDataSource je přidán na stránku a nakonfigurovaná, jsme připravení přidat na stránku pro zobrazení dat vrácených ObjectDataSource data ovládací prvky webového `Select` metoda. Ovládací prvek webu žádná data mohou být vázány na ObjectDataSource; Podívejme se na zobrazení dat ObjectDataSource GridView, DetailsView a FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Vytvoření vazby GridView ObjectDataSource

Přidání ovládacího prvku GridView z panelu nástrojů `SimpleDisplay.aspx`na návrhovou plochu. Z prvku GridView inteligentních značek zvolte ovládacího prvku ObjectDataSource, kterou jsme přidali v kroku 1. Tím se automaticky vytvoří BoundField v GridView pro každou vlastnost vrácených dat z ObjectDataSource `Select` – metoda (konkrétně, vlastnosti definované DataTable produkty).


[![GridView byla přidána na stránku a vázaný k ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Obrázek 6**: A GridView byla přidána na stránku a vázaných ke ObjectDataSource ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


Pak můžete přizpůsobit, uspořádání nebo odebrání BoundFields GridView kliknutím na možnost Upravit sloupce z inteligentních značek.


[![Spravovat GridView BoundFields prostřednictvím dialogové okno Upravit sloupce](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Obrázek 7**: Správa prvku GridView BoundFields prostřednictvím sloupce dialogové okno Upravit ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


Za chvíli upravit BoundFields prvku GridView, odebrání `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` BoundFields. Jednoduše vyberte BoundField ze seznamu v levém dolním a klikněte na tlačítko Odstranit (na červené X) je odebrat. V dalším kroku Změna uspořádání BoundFields tak, aby `CategoryName` a `SupplierName` BoundFields předcházet `UnitPrice` BoundField tyto BoundFields výběrem a kliknutím na šipku nahoru. Nastavte `HeaderText` vlastnosti zbývající BoundFields k `Products`, `Category`, `Supplier`, a `Price`, v uvedeném pořadí. Potom mít `Price` BoundField formátu měny nastavením BoundField `HtmlEncode` vlastnost na hodnotu False a jeho `DataFormatString` vlastnost `{0:c}`. Nakonec vodorovně zarovnat `Price` napravo a `Discontinued` políčko v centru prostřednictvím `ItemStyle` / `HorizontalAlign` vlastnost.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Přizpůsobené BoundFields GridView.](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Obrázek 8**: The GridView BoundFields přizpůsobené ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Použití motivů pro konzistentního vzhledu

Tyto kurzy snažit odstraní veškerá nastavení řízení úrovně styl, místo toho pomocí kaskádových stylů definované v souboru externích kdykoli je to možné. `Styles.css` Soubor obsahuje `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, a `AlternatingRowStyle` třídy CSS, které se má použít k určují vzhled data webových ovládacích prvků používaná v těchto kurzech. K tomu může nastaví prvku GridView `CssClass` vlastnost `DataWebControlStyle`a jeho `HeaderStyle`, `RowStyle`, a `AlternatingRowStyle` vlastnosti `CssClass` vlastnosti odpovídajícím způsobem.

Pokud tyto nastaví `CssClass` vlastnosti na ovládací prvek webu, je potřeba pamatovat explicitně nastavit hodnoty těchto vlastností pro každou jednotlivou data webové ovládací prvek přidán na naše výukové programy. Lepší správu bitlockeru přístup je můžete definovat výchozí vlastnosti související s šablon stylů CSS, rutina GridView DetailsView, a FormView řídí motiv. Motiv je kolekce nastavení vlastností ovládacího prvku úrovni, Image a tříd CSS, které mohou být použity na stránky napříč lokalitou a vynutit běžné vzhled a chování.

Naše motiv nebude obsahovat žádné obrázky nebo soubory CSS (necháme šablony stylů `Styles.css` jako-je, definována v kořenové složce webové aplikace), ale bude obsahovat dvě vzhledy. Vzhled je soubor, který definuje výchozí vlastnosti pro ovládací prvek webu. Konkrétně jsme budete mít soubor vzhledu GridView a DetailsView ovládacích prvků, která určuje výchozí `CssClass`-souvisejících vlastností.

Začněte přidáním nového vzhledu souboru projektu s názvem `GridView.skin` pravým tlačítkem myši na název projektu v Průzkumníku řešení a zvolením přidat novou položku.


[![Přidejte soubor vzhledu s názvem GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Obrázek 9**: Přidat soubor vzhledu s názvem `GridView.skin` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Soubory vzhledu musí umístit do motiv, který se nachází ve `App_Themes` složky. Vzhledem k tomu, že ještě nemáme takové složce, Visual Studio nabídne, vytvořte ho pro nás při přidávání naše první vzhledu. Klikněte na tlačítko Ano, chcete-li vytvořit `App_Theme` složky a umístěte nové `GridView.skin` soubor existuje.


[![Nechte nástroj Visual Studio vytvořit složku App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Obrázek 10**: umožní vytvořit Visual Studio `App_Theme` složky ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Tím se vytvoří nový motiv v `App_Themes` složku s názvem GridView souborem vzhledu `GridView.skin`.


![Rutina GridView motiv má byla přidána do složky App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Obrázek 11**: motivu GridView má přidané do `App_Theme` složky


Přejmenujte motiv GridView DataWebControls (klikněte pravým tlačítkem na složku GridView ve `App_Theme` složky a vyberte přejmenování). Potom zadejte následující kód do `GridView.skin` souboru:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Definuje výchozí vlastnosti `CssClass`-souvisejících vlastností pro všechny GridView na libovolné stránce, který používá DataWebControls motivu. Umožňuje přidat jiné vzhledu pro DetailsView, data ovládací prvek webu, který budeme krátce používat. Přidání nového vzhledu do DataWebControls motiv s názvem `DetailsView.skin` a přidejte následující kód:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

S naše motiv definované posledním krokem je k uplatnění motivu na naší stránce s ASP.NET. Motiv lze použít na základě po stránkách nebo pro všechny stránky webu. Umožňuje použít tento motiv pro všechny stránky webu. K tomu, přidejte následující kód do `Web.config`na `<system.web>` části:


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

To je všechno je k němu! `styleSheetTheme` Nastavení znamená, že by měl zadány v motivu vlastnosti *není* přepsat vlastnosti zadané pro úroveň ovládacího prvku. K určení, že by měl motiv nastavení trumfové barvy nastavení řízení, použijte `theme` atribut místě `styleSheetTheme`; bohužel motiv nastavení zadané prostřednictvím `theme` atribut se nezobrazí v zobrazení návrhu Visual Studio. Odkazovat na [ASP.NET motivy a vzhledy přehled](https://msdn.microsoft.com/library/ykzx33wh.aspx) a [motivů pomocí stylů serverové](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) na motivy a vzhledy; Další informace najdete v části [postupy: použít motivy ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) Další informace o konfigurace stránky pro použití motivu.


[![GridView zobrazí název, kategorie, dodavatele, ceny a – starší formáty informace o produktu](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Obrázek 12**: GridView zobrazí název produktu, kategorie, dodavatele, ceny a zastaví informace ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Zobrazení jeden záznam v daný okamžik v ovládacím prvku DetailsView.

GridView zobrazí jeden řádek pro každý záznam vrácený ovládací prvek zdroje dat, ke kterému je vázán. Existují dobu, ale když jsme chtít zobrazit záznam jedinou nebo jenom jeden záznam současně. [Ovládací prvek DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) nabízí tuto funkci, vykreslování jako HTML `<table>` s dva sloupce a jeden řádek pro každý sloupec nebo vlastnost vázáno na ovládací prvek. DetailsView si můžete představit jako GridView s jednoho záznamu otočený 90 stupňů.

Začněte tím, že přidání ovládacího prvku DetailsView *výše* GridView v `SimpleDisplay.aspx`. V dalším kroku navázat jej do ovládacího prvku ObjectDataSource jako GridView. Jako s GridView, BoundField budou přidány do DetailsView pro každou vlastnost v objektu vrácený ObjectDataSource `Select` metoda. Jediným rozdílem je, že DetailsView BoundFields jsou nastíněny vodorovně namísto svisle.


[![Přidat DetailsView na stránku a navázat jej ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Obrázek 13**: Přidání DetailsView na stránku a navázat jej ObjectDataSource ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Jako GridView může být DetailsView BoundFields tweaked zajistit další přizpůsobení zobrazení dat vrácených ObjectDataSource. Obrázek 14 ukazuje DetailsView po jeho BoundFields a `CssClass` vlastnosti byla nakonfigurována pro zkontrolujte její vzhled, podobně jako v příkladu rutina GridView.


[![DetailsView zobrazuje jeden záznam](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Obrázek 14**: DetailsView zobrazuje jeden záznam ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Všimněte si, že DetailsView zobrazí pouze na první záznam vrácený zdrojem dat. Umožňuje uživatelům procházet všechny záznamy, jeden po druhém, jsme musíte povolit stránkování pro DetailsView. Uděláte to tak, vraťte k sadě Visual Studio a zaškrtněte políčko Povolit stránkování v ovládacím prvku DetailsView inteligentních značek.


[![Povolit stránkování v ovládacím prvku DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Obrázek 15**: Povolit stránkování v ovládacím prvku DetailsView ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![S stránkování povoleno, umožňuje DetailsView uživateli zobrazit produktů](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Obrázek 16**: S povoleno stránkování DetailsView umožňuje uživatelům zobrazit produktů ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Budeme mluvit o další informace o stránkování v budoucnosti kurzy.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Flexibilnější rozložení pro zobrazení jednotlivých záznamů

DetailsView je poměrně pevné v, jak se zobrazuje každý záznam vrácená z ObjectDataSource. Chceme může flexibilnější zobrazení data. Například místo zobrazuje název produktu, kategorie, dodavatele, ceny a – starší formáty informace na samostatném řádku, budeme chtít zobrazit název produktu a cenu ve `<h4>` záhlaví s informacemi kategorií a dodavatele, zobrazování pod názvem a cenu v menší velikost písma. A nemusí uživatelské zobrazit názvy vlastností (produktu, kategorie a tak dále) vedle hodnoty.

[FormView řízení](https://msdn.microsoft.com/library/fyf1dk77.aspx) poskytuje tuto úroveň přizpůsobení. Místo použití polí (jako jsou GridView a DetailsView provádět), FormView používá šablony, které umožňují směs ovládací prvky webového, statické HTML a [Syntaxe datové vazby](http://www.15seconds.com/issue/040630.htm). Pokud jste se seznámili s ovládacím prvku Repeater z ASP.NET 1.x, si můžete představit FormView jako opakovače pro zobrazuje jeden záznam.

Přidání ovládacího prvku FormView `SimpleDisplay.aspx` stránky návrhovou plochu. Zpočátku FormView zobrazí jako blok šedé nám oznamující, že je potřeba zadat, minimálně, ovládacího prvku `ItemTemplate`.


[![FormView musí zahrnovat ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Obrázek 17**: FormView musí začlenění `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


Můžete vázat FormView přímo na ovládací prvek zdroje dat pomocí inteligentních značek FormView, která vytvoří výchozí `ItemTemplate` automaticky (spolu s `EditItemTemplate` a `InsertItemTemplate`, pokud se ovládací prvek ObjectDatatSource `InsertMethod` a `UpdateMethod` jsou nastaveny vlastnosti). Ale v tomto příkladu budeme vazby dat k třídě FormView a zadejte jeho `ItemTemplate` ručně. Začněte tím, že nastavení FormView `DataSourceID` vlastnost, která má `ID` prvku ObjectDataSource `ObjectDataSource1`. Dále vytvořte `ItemTemplate` tak, aby se zobrazí název a cena v produktu `<h4>` elementu a kategorie a odesilatel názvy pod které v menší velikost písma.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![První produktu (Chai) se zobrazí ve formátu vlastní](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Obrázek 18**: první produktu (Chai) se zobrazí ve formátu vlastní ([Kliknutím zobrazit obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


`<%# Eval(propertyName) %>` Je syntaxe datové vazby. `Eval` Vrátí metoda hodnotu zadané vlastnosti pro aktuální objekt je vázán na ovládací prvek FormView. Podívejte se na článek Alex Homer [rozšířené vazby syntaxi Data v technologii ASP.NET 2.0 a zjednodušená](http://www.15seconds.com/issue/040630.htm) Další informace o výpisů a in datové vazby.

Jako DetailsView FormView se zobrazí pouze na první záznam vrácená z ObjectDataSource. Můžete povolit stránkování v FormView umožňující návštěvníkům krok prostřednictvím produktů jeden najednou.

## <a name="summary"></a>Souhrn

Přístup k informacím a zobrazení dat z vrstvy obchodní logiky můžete udělat bez nutnosti psaní řádek kódu díky ovládacího prvku ObjectDataSource technologii ASP.NET 2.0. ObjectDataSource vyvolá zadanou metodu třídy a vrátí výsledky. Tyto výsledky lze zobrazit v datech ovládací prvek webu, která je vázána ObjectDataSource. V tomto kurzu jsme se podívali na vytvoření vazby ovládacích prvků GridView DetailsView a FormView ObjectDataSource.

Zatím jste pouze viděli použití ObjectDataSource k volání metody bez parametrů, ale co když chceme vyvolat metodu, která očekává vstupní parametry, jako například `ProductBLL` třídy `GetProductsByCategoryID(categoryID)`? Abyste mohli volat metodu, která očekává, že jeden nebo více parametrů jsme musíte nakonfigurovat ObjectDataSource zadat hodnoty pro tyto parametry. Ukážeme, jak to provést naše [další kurz](declarative-parameters-cs.md).

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vytvořit vlastní ovládací prvky zdroje dat](https://msdn.microsoft.com/library/ms364049.aspx)
- [Rutina GridView příklady pro technologii ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Zjednodušené a rozšířené datové vazby syntaxe technologie ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Motivy technologie ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Motivů pomocí stylů na straně serveru](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Postupy: Motivů technologie ASP.NET prostřednictvím kódu programu](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](declarative-parameters-cs.md)
