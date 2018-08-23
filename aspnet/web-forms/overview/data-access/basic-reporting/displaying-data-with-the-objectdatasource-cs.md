---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Zobrazení dat ovládacím prvkem ObjectDataSource (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz se zabývá ovládacího prvku ObjectDataSource pomocí tohoto ovládacího prvku lze svázat data načtená z knihoven BLL vytvořili v předchozím kurzu bez havi...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 45f72cfb06cdd4ee624f9930a360d5a85a98a9c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756947"
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Zobrazení dat ovládacím prvkem ObjectDataSource (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) nebo [stahovat PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> Tento kurz se zabývá ovládacího prvku ObjectDataSource pomocí tohoto ovládacího prvku lze svázat data načtená z knihoven BLL vytvořili v předchozím kurzu bez nutnosti psaní jediného řádku kódu!


## <a name="introduction"></a>Úvod

S naší aplikace architektury a webu rozložení stránky dokončeno jsme připraveni začít prozkoumávat tom, jak provádět řadu běžných úloh a generování sestav – související s daty. V předchozích kurzech jste zaznamenali jak prostřednictvím kódu programu vytvořit vazbu dat z knihoven BLL a DAL k datům webový ovládací prvek na stránce ASP.NET. Tato syntaxe přiřazování dat webový ovládací prvek `DataSource` vlastnost k datům zobrazení a následným voláním ovládacího prvku `DataBind()` metoda byla používaným v aplikacích ASP.NET 1.x a můžete i nadále používat ve svých aplikacích 2.0. ASP.NET 2.0 nové ovládací prvky zdroje dat ale nabízí deklarativní způsob, jak pracovat s daty. Pomocí těchto ovládacích prvků lze svázat data načtená z BLL vytvořené v [předchozí kurz o službě](../introduction/creating-a-business-logic-layer-cs.md) bez nutnosti psaní jediného řádku kódu.

ASP.NET 2.0 se dodává s pěti ovládací prvky předdefinované datové zdroje [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [prvku AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), a [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) i když můžete vytvořit vlastní [ovládací prvky zdroje dat vlastní](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), v případě potřeby. Protože jsme vyvinuli architekturu pro naše aplikace, budeme používat prvku ObjectDataSource proti našich knihoven BLL tříd.


![Technologie ASP.NET 2.0 obsahuje pět ovládací prvky předdefinované datové zdroje](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Obrázek 1**: technologie ASP.NET 2.0 obsahuje pět ovládací prvky předdefinované datové zdroje


Prvku ObjectDataSource slouží jako proxy server pro práci s některý jiný objekt. Ke konfiguraci ObjectDataSource jsme tuto verzi uveďte základní objekt a jak se jeho metody mapují na ObjectDataSource `Select`, `Insert`, `Update`, a `Delete` metody. Jakmile byl zadán tento základní objekt a jeho metody namapované na ObjectDataSource, jsme vazbu pak lze vytvořit prvku ObjectDataSource dat webový ovládací prvek. Technologie ASP.NET se dodává s mnoha dat webové ovládací prvky, včetně ovládacího prvku GridView, DetailsView, RadioButtonList a DropDownList, mimo jiné. Během životního cyklu stránky dat webový ovládací prvek může vyžadovat pro přístup k datům je vázán na, který bude provádět vyvoláním jeho ObjectDataSource `Select` metoda; Pokud data webový ovládací prvek podporuje, vkládání, aktualizaci, nebo odstraníte, volání se dají vytvořit na jeho ObjectDataSource `Insert`, `Update`, nebo `Delete` metody. Tato volání směrují ObjectDataSource na příslušné základní objekt metody pak, jak ukazuje následující diagram.


[![Prvek ObjectDataSource slouží jako proxy server](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Obrázek 2**: prvku ObjectDataSource slouží jako proxy server ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Když prvku ObjectDataSource můžete použít k vyvolání metody pro vkládání, aktualizaci nebo odstraňování dat, právě zaměřme se na vrácení dat; Další kurzy prozkoumá pomocí prvku ObjectDataSource a data webové ovládací prvky, které upravují data.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Krok 1: Přidání a konfigurace ovládacího prvku ObjectDataSource

Začněte otevřením `SimpleDisplay.aspx` stránku `BasicReporting` složku, přepněte do zobrazení návrhu a pak přetáhněte ovládací prvek ObjectDataSource z panelu nástrojů na plochu návrhu na stránce. Prvku ObjectDataSource se zobrazuje jako šedé pole na návrhové ploše, protože nevytváří žádné značky ho jednoduše přistupuje k datům pomocí volání metody ze zadaného objektu. Data vrácená prvku ObjectDataSource lze zobrazit data webový ovládací prvek, jako je například ovládacího prvku GridView, DetailsView, FormView a tak dále.

> [!NOTE]
> Alternativně můžete nejdřív přidat data webový ovládací prvek na stránce a pak zvolte z inteligentních značek, &lt;nový zdroj dat&gt; možnost z rozevíracího seznamu.


Určit základní objekt a jak se tento objekt metody mapují na ObjectDataSource ObjectDataSource, klikněte na odkaz Konfigurovat zdroj dat z ObjectDataSource inteligentních značek.


[![Klikněte konfigurovat propojení na zdroj dat z inteligentních značek](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Obrázek 3**: klikněte konfigurovat propojení na zdroj dat z inteligentních značek ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Tím se zobrazí v Průvodci zdroje dat konfigurace. Nejprve jsme musíte zadat objekt, který je prvku ObjectDataSource pro práci s. Pokud je zaškrtnuto políčko "Zobrazit pouze datové součásti", rozevíracího seznamu na této obrazovce vypíše pouze ty objekty, které byly vybaveny `DataObject` atribut. Náš seznam aktuálně obsahuje objekty TableAdapter v datové sadě zadán a BLL třídy, které jsme vytvořili v předchozím kurzu. Pokud jste zapomněli přidat `DataObject` atributů na třídy vrstvy obchodní logiky ji neuvidíte v tomto seznamu. V takovém případě zrušit zaškrtnutí políčka "Zobrazit pouze datové součásti" k zobrazení všech objektů, které by měl obsahovat BLL třídy (společně s další třídy v zadané datové sady DataTables, DataRows a tak dále).

V této první obrazovky zvolte `ProductsBLL` třídy z rozevíracího seznamu a klikněte na tlačítko Další.


[![Zadejte objekt, který chcete použít s ovládacím prvkem ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Obrázek 4**: Zadejte objekt, který chcete použít s ovládacím prvkem ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


V průvodci na další obrazovce vás vyzve k výběru jakou metodu ObjectDataSource by měla vyvolat. Rozevírací seznam uvádí tyto metody, které nevracejí data v objektu vybrali na předchozí obrazovku. Tady vidíte `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, a `GetProductsBySupplierID`. Vyberte `GetProducts` metodu z rozevíracího seznamu a klikněte na tlačítko Dokončit (Pokud jste přidali `DataObjectMethodAttribute` k `ProductBLL`pro metody, jak je znázorněno v předchozím kurzu, tato možnost bude vybrána ve výchozím nastavení).


[![Zvolit metodu pro vrácená Data z vyberte kartu](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Obrázek 5**: Zvolte metodu, vrací data z karty vyberte ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Ruční konfigurace prvku ObjectDataSource

Průvodce konfigurací zdroje dat prvku ObjectDataSource nabízí rychlý způsob, jak zadat objekt, který používá a přidružení, jaké metody objektu jsou vyvolány. Můžete ale nakonfigurovat ObjectDataSource prostřednictvím její vlastnosti pomocí okna vlastnosti nebo přímo v deklarativním označení. Stačí nastavit `TypeName` vlastnost typ základního objektu, kterou chcete použít a `SelectMethod` na metodu, která má být vyvolán při načítání dat.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

I v případě, že chcete průvodce Konfigurace zdroje dat, které mohou nastat situace, kdy je potřeba ručně nakonfigurovat prvek ObjectDataSource, jako průvodce jsou uvedeny pouze třídy vytvořené pro vývojáře. Pokud chcete vytvořit vazbu ObjectDataSource na třídu v rozhraní .NET Framework, jako [třída členství](https://msdn.microsoft.com/library/system.web.security.membership.aspx), přístup k informacím o účtu uživatele, nebo [třída Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) pro práci s informace o systému souborů bude nutné ručně nastavit vlastnosti ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Krok 2: Přidání ovládacího prvku webových dat a vytvoříte jejich vazbu na ObjectDataSource

Když prvku ObjectDataSource byl přidán na stránku a nakonfigurované, jsme připraveni přidat data ovládací prvky webové stránky k zobrazení dat vrácených ObjectDataSource `Select` metody. Žádná data webový ovládací prvek může být vázaný na ObjectDataSource; Podívejme se na zobrazení prvku ObjectDataSource dat v prvku GridView, DetailsView a FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Vazba na ObjectDataSource GridView

Přidání ovládacího prvku GridView ze sady nástrojů `SimpleDisplay.aspx`na návrhové ploše. V prvku GridView inteligentních značek Zvolte ovládací prvek ObjectDataSource, kterou jsme přidali v kroku 1. Tím se automaticky vytvoří vlastnost BoundField v prvku GridView. pro každou vlastnost data vrácená ObjectDataSource `Select` – metoda (konkrétně vlastnosti určené DataTable produkty).


[![GridView byl přidán na stránku a vázaný k ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Obrázek 6**: A GridView byl přidán na stránku a mez ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


Pak můžete přizpůsobit, uspořádání nebo odebrání prvku GridView BoundFields klepnutím na možnost Upravit sloupce z inteligentních značek.


[![Správa prvku GridView BoundFields přes dialogové okno Upravit sloupce](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Obrázek 7**: dialogového okna Spravovat prvku GridView BoundFields prostřednictvím upravit sloupce ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


Využít k úpravě BoundFields prvku GridView, odebrání `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, a `ReorderLevel` BoundFields. Jednoduše vyberte ze seznamu v levém dolním Vlastnost BoundField a klikněte na tlačítko Odstranit (červené X) je odebrat. V dalším kroku změnit uspořádání BoundFields tak, aby `CategoryName` a `SupplierName` BoundFields předcházet `UnitPrice` Vlastnost BoundField výběrem těchto BoundFields a kliknutím na šipku nahoru. Nastavte `HeaderText` vlastnosti zbývající BoundFields k `Products`, `Category`, `Supplier`, a `Price`v uvedeném pořadí. Dále `Price` Vlastnost BoundField formátované jako měnu nastavení vlastnost BoundField `HtmlEncode` vlastnost na hodnotu False a jeho `DataFormatString` vlastnost `{0:c}`. Nakonec zarovnat vodorovně `Price` napravo a `Discontinued` zaškrtávacího políčka v centru prostřednictvím `ItemStyle` / `HorizontalAlign` vlastnost.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Přizpůsobené BoundFields prvku GridView.](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Obrázek 8**: byl přizpůsoben GridView BoundFields ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Použití motivů pro konzistentního vzhledu

Tyto kurzy snažit odebrat jakékoli nastavení řízení úrovně stylu, namísto použití šablony stylů CSS definované v souboru externích kdykoli je to možné. `Styles.css` Soubor obsahuje `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, a `AlternatingRowStyle` třídy CSS, které by měla sloužit k určovat vzhled data webových ovládacích prvků používaná v těchto kurzech. K tomu může nastavíme prvku GridView `CssClass` vlastnost `DataWebControlStyle`a jeho `HeaderStyle`, `RowStyle`, a `AlternatingRowStyle` vlastností `CssClass` vlastnosti odpovídajícím způsobem.

Pokud tyto nastavíme `CssClass` ovládací prvek přidán do našich kurzů webu vlastnosti na ovládací prvek webového by musíme nezapomeňte explicitně nastavit hodnoty těchto vlastností pro každou jednotlivou data. Můžete definovat výchozí vlastnosti související s CSS prvku GridView, DetailsView, je přístup založený na snadněji zpracovatelné a FormView řídí motiv. Motiv je kolekce nastavení vlastností ovládacího prvku úrovni, obrázky a třídy CSS, které lze použít u stránek přes síť, kterou chcete vynutit běžné vzhled a chování.

Naše motiv nezahrnuje všechny obrázky nebo soubory šablon stylů CSS (necháme šablony stylů `Styles.css` jako – je definován v kořenové složce webové aplikace), ale bude obsahovat dva skinů v šablonách. Vzhled je soubor, který definuje výchozí vlastnosti pro webový ovládací prvek. Konkrétně jsme budete mít soubor skinu pro ovládací prvky GridView a DetailsView určující výchozí `CssClass`-souvisejících vlastností.

Začněte tím, že přidáte nový soubor vzhledu do projektu s názvem `GridView.skin` pravým tlačítkem myši na název projektu v Průzkumníku řešení a zvolením přidat novou položku.


[![Přidat soubor vzhledu GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Obrázek 9**: přidání souboru skinu s názvem `GridView.skin` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Souborech skinů potřeba umístit do motiv, který se nachází v `App_Themes` složky. Vzhledem k tomu, že zatím nemáme takovou složku, Visual Studio nabídne prosím nějaké nám při přidávání naši první vzhledu. Klikněte na tlačítko Ano. Pokud chcete vytvořit `App_Theme` složky a umístí nové `GridView.skin` soubor existuje.


[![Nechte Visual Studio vytvořte složku App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Obrázek 10**: umožní Visual Studio vytvořit `App_Theme` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Tím se vytvoří nový motiv v `App_Themes` složku s názvem GridView s soubor vzhledu `GridView.skin`.


![Motiv GridView má byl přidán do složky App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Obrázek 11**: prvku GridView motiv má byla přidána do `App_Theme` složky


Přejmenování ovládacího prvku GridView motiv DataWebControls (klikněte pravým tlačítkem na složku GridView v `App_Theme` složky a zvolte Přejmenovat). Pak zadejte následující kód do `GridView.skin` souboru:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Definuje výchozí vlastnosti `CssClass`-souvisejících vlastností pro libovolný prvek GridView na libovolné stránce, která používá DataWebControls motiv. Přidejme prvku DetailsView data webový ovládací prvek, který budeme za chvíli používat jiný vzhled. Přidat nový vzhled DataWebControls motiv s názvem `DetailsView.skin` a přidejte následující kód:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Naše motivu definován je posledním krokem je použít motiv na stránku ASP.NET. Motiv lze použít na základě stránku po stránce nebo pro všechny stránky webu. Pro všechny stránky webu s použitím tento motiv. Chcete-li to provést, přidejte následující kód k `Web.config`společnosti `<system.web>` části:


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

To je všechno je to! `styleSheetTheme` Nastavení znamená, že by byly zadány v motivu vlastnosti *není* přepsat vlastnosti zadaný na úrovni ovládacího prvku. K určení, že by měl trumfové barvy motivu nastavení nastavení, použijte `theme` atribut místo `styleSheetTheme`; bohužel určené pomocí nastavení motiv `theme` atribut se nezobrazí v zobrazení pro Visual Studio Design. Odkazovat na [motivů ASP.NET a přehled vzhledy](https://msdn.microsoft.com/library/ykzx33wh.aspx) a [motivů pomocí stylů na straně serveru](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) Další informace o používání motivů a vzhledy; naleznete v tématu [postupy: použití motivů ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) pro další informace o konfigurace stránky pro použití motivu.


[![Název produktu, kategorie, Dodavatel, ceny a ukončená informace se zobrazí prvku GridView.](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Obrázek 12**: prvku GridView zobrazí název produktu, kategorie, Dodavatel, ceny a vyřazuje informace ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Zobrazení jednoho záznamu v daný okamžik v ovládacím prvku DetailsView.

GridView zobrazí jeden řádek pro každý záznam vrácený ovládací prvek zdroje dat, ke kterému je vázán. Existují však situace, kdy může být vhodné zobrazí jediný záznam nebo jenom jeden záznam v čase. [Prvku DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) nabízí tyto funkce vykreslení jako HTML `<table>` se dvěma sloupci a jeden řádek pro každý sloupec nebo vlastnost svázána s ovládacím prvkem. Ovládacím prvku DetailsView si můžete představit jako prvek GridView s jednoho záznamu otočenou o 90 stupňů.

Začněte přidáním ovládacího prvku DetailsView *nad* GridView v `SimpleDisplay.aspx`. V dalším kroku vázat na stejný ovládací prvek ObjectDataSource jako prvku GridView. Například s použitím prvku GridView, vlastnost BoundField se přidají do ovládacího prvku DetailsView pro každou vlastnost v objektu vrácený ObjectDataSource `Select` metody. Jediným rozdílem je, že ovládacím prvku DetailsView BoundFields jsou rozloženy vodorovně namísto svisle.


[![Přidat na stránku DetailsView a připnout ho ke ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Obrázek 13**: DetailsView přidat na stránku a připnout ho ke ObjectDataSource ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Jako prvku GridView může být k poskytování více přizpůsobit zobrazení dat vrácených ObjectDataSource tweaked BoundFields ovládacím prvku DetailsView. Obrázek 14 ukazuje ovládacím prvku DetailsView. po jeho BoundFields a `CssClass` vlastnosti nastavené provést její vzhled podobně jako v příkladu ovládacího prvku GridView.


[![Zobrazuje jeden záznam, ovládacím prvku DetailsView.](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Obrázek 14**: jeden záznam zobrazí ovládacím prvku DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Všimněte si, že ovládacím prvku DetailsView zobrazí pouze první záznam vrácených zdrojem dat. Aby uživatel mohl procházet všechny záznamy, postupně, můžeme musíte povolit stránkování prvku DetailsView. Uděláte to tak, vraťte se do sady Visual Studio a zaškrtněte políčko Povolit stránkování v ovládacím prvku DetailsView inteligentních značek.


[![Povolit stránkování v ovládacím prvku DetailsView.](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Obrázek 15**: Povolit stránkování v ovládacím prvku DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![S povoleno stránkování, ovládacím prvku DetailsView. umožňuje uživateli zobrazit všechny produkty](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Obrázek 16**: S povoleno stránkování, ovládacím prvku DetailsView. umožňuje uživateli zobrazit všechny produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Budeme se jimi více o stránkování v budoucích kurzech.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Flexibilnější rozložení pro zobrazení jeden záznam v čase

Je hodně strnulý způsob, jakým zobrazuje jednotlivých záznamů vrácených z ObjectDataSource ovládacím prvku DetailsView. Flexibilnější zobrazení dat může být vhodné. Například místo zobrazení název produktu, kategorie, Dodavatel, ceny a ukončená informace na samostatném řádku, může chceme zobrazit název produktu a ceny v `<h4>` záhlaví s informacemi kategorie a dodavatele, povolí pod názvem a ceny v menší velikost písma. A nezapomeňte jsme nemusí zobrazit názvy vlastností (produktu, kategorie a tak dále) vedle hodnoty.

[Ovládacího prvku FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) poskytuje tuto úroveň přizpůsobení. Místo použití pole (jako jsou ovládacími prvky GridView a DetailsView provádět), třídě FormView používá šablony, které umožňují kombinaci webové ovládací prvky, statický kód HTML a [databinding syntaxe](http://www.15seconds.com/issue/040630.htm). Pokud jste obeznámeni s ovládacím prvkem Repeater z ASP.NET 1.x, si můžete představit FormView jako Repeater pro zobrazení jeden záznam.

Přidání ovládacího prvku FormView `SimpleDisplay.aspx` stránky návrhovou plochu. Otevření FormView zobrazí jako blok šedé nás informuje, že potřebujeme pro poskytování alespoň ovládacího prvku `ItemTemplate`.


[![Šablona ItemTemplate obsahovat musí FormView](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Obrázek 17**: FormView musí začlenění `ItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


FormView mohl vytvořit vazbu přímo k ovládacímu prvku zdroje dat pomocí ovládacího prvku FormView inteligentní značky, který se vytvoří výchozí `ItemTemplate` automaticky (spolu s `EditItemTemplate` a `InsertItemTemplate`, pokud ovládací prvek ObjectDatatSource `InsertMethod` a `UpdateMethod` jsou nastaveny vlastnosti). Ale pro účely tohoto příkladu můžeme data svázat FormView a zadejte jeho `ItemTemplate` ručně. Začněte tím, že nastavení ovládacího prvku FormView `DataSourceID` vlastnost `ID` ovládacího prvku ObjectDataSource `ObjectDataSource1`. Dále vytvořte `ItemTemplate` tak, aby zobrazil název produktu a cena v `<h4>` elementu a názvy kategorií a shipper pod, který v menší velikost písma.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![Zobrazí se první produktu (Chai) ve formátu vlastní](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Obrázek 18**: The první produktu (Chai) se zobrazí ve formátu vlastní ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


`<%# Eval(propertyName) %>` Je syntaxe datové vazby. `Eval` Metoda vrátí hodnotu zadané vlastnosti pro aktuální objekt svázaný s ovládacím prvku FormView. Podívejte se na článek Alex Homer [zjednodušené a rozšířené vazby syntaxe dat v technologii ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) Další informace o výhodami a nevýhodami datové vazby.

Jako ovládacím prvku DetailsView FormView zobrazuje pouze první záznam vrácená ObjectDataSource. Můžete povolit stránkování ve třídě FormView umožňující návštěvníci, chcete-li si produkty jeden po druhém.

## <a name="summary"></a>Souhrn

Přístup k a zobrazení dat z vrstvy obchodní logiky můžete udělat bez nutnosti psaní jediného řádku kódu díky ovládacího prvku ObjectDataSource ASP.NET 2.0. Prvku ObjectDataSource vyvolá zadanou metodu třídy a vrátí výsledky. Tyto výsledky lze zobrazit v datech webový ovládací prvek, který je vázán na ObjectDataSource. V tomto kurzu jsme se podívali na vazby ovládacích prvků ovládacího prvku GridView, DetailsView a FormView ObjectDataSource.

Zatím jsme viděli pouze postupy použití ovládacího prvku ObjectDataSource volání metody bez parametrů, ale co když chcete vyvolat metodu, která očekává, že vstupní parametry, jako `ProductBLL` třídy `GetProductsByCategoryID(categoryID)`? Aby bylo možné volat metodu, která očekává, že jeden nebo více parametrů musí nakonfigurujeme ObjectDataSource zadání hodnot těchto parametrů. Uvidíme, jak toho dosáhnout v našich [další kurz](declarative-parameters-cs.md).

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vytvořit vlastní ovládací prvky zdroje dat](https://msdn.microsoft.com/library/ms364049.aspx)
- [Příklady GridView pro technologii ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Zjednodušená a rozšířené datové vazby syntaxe v technologii ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Motivy v technologii ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Motivů pomocí stylů na straně serveru](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Postupy: Použití motivů ASP.NET prostřednictvím kódu programu](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](declarative-parameters-cs.md)
