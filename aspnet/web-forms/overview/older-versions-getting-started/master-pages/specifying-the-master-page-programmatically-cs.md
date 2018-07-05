---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Programové určení stránky předlohy (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Vyhledá v nastavení obsahu stránky hlavní stránku prostřednictvím kódu programu přes PreInit obslužné rutiny události.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b3fbd95a8088e20382c426fcdcc6a4b6e67d44b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370387"
---
<a name="specifying-the-master-page-programmatically-c"></a>Programové určení stránky předlohy (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Vyhledá v nastavení obsahu stránky hlavní stránku prostřednictvím kódu programu přes PreInit obslužné rutiny události.


## <a name="introduction"></a>Úvod

Od zahajovací příklad v [ *vytváření webu rozložení pomocí stránek předlohy*](creating-a-site-wide-layout-using-master-pages-cs.md), veškerý obsah stránky neodkazujete jejich stránky předlohy deklarativně prostřednictvím `MasterPageFile` atribut `@Page`směrnice. Například následující `@Page` – direktiva odkazuje stránky obsahu na stránce předlohy `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

[ `Page` Třídy](https://msdn.microsoft.com/library/system.web.ui.page.aspx) v `System.Web.UI` obsahuje obor názvů [ `MasterPageFile` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) , který vrací cestu ke stránce hlavní stránky obsahu, je tato vlastnost nastavená `@Page` směrnice. Tuto vlastnost lze také programové určení stránky předlohy stránku obsahu. Tento přístup je užitečný, pokud chcete dynamicky přiřazovat hlavní stránku založenou na vnější faktory, jako je například uživatel na stránce.

V tomto kurzu přidáme hlavní strana na náš web a dynamicky rozhodnout, které stránky předlohy k použití v době běhu.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Krok 1: Podívejte se na životní cyklus stránky

Pokaždé, když je přijat požadavek na webovém serveru, který je stránku obsahu stránky ASP.NET, modul ASP.NET musí prověřování na stránce obsahu odpovídající ovládací prvky ContentPlaceHolder ovládacích prvků do stránky předlohy. Tato fusion vytvoří hierarchii jeden ovládací prvek, který lze potom pokračujte životního cyklu stránky.

Obrázek 1 znázorňuje tuto fusion. Krok 1 na obrázku 1 zobrazí původní obsah a hierarchie ovládací prvek stránky předlohy. Na konci ocáskem fázi PreInit obsah jsou ovládací prvky na stránce přidány do odpovídajících prvků ContentPlaceHolder na stránce předlohy (krok 2). Po tomto fusion stránky předlohy slouží jako kořenový hierarchii roztaveného ovládacích prvků. Tento ovládací prvek začleněny hierarchie se pak přidá do stránky a vytvoření hierarchie dokončené ovládacího prvku (krok 3). Net výsledkem je, že na stránce ovládacího prvku hierarchie obsahuje hierarchii roztaveného ovládacích prvků.


[![Stránky předlohy a stránky obsahu ovládacího prvku hierarchií jsou začleněny společně během fáze PreInit](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Obrázek 01**: stránku předlohy a stránky obsahu ovládacího prvku hierarchie jsou začleněny společně během fáze PreInit ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Krok 2: Nastavení`MasterPageFile`vlastnost z kódu

Jaké stránky předlohy partakes v tomto fusion závisí na hodnotě `Page` objektu `MasterPageFile` vlastnost. Nastavení `MasterPageFile` atribut `@Page` direktiva nemá čistým důsledkem toho přiřazení `Page`společnosti `MasterPageFile` vlastnost během fáze inicializace je úplně první fáze životního cyklu stránky. Tuto vlastnost jsme také můžete nastavit prostřednictvím kódu programu. Nicméně je nutné nastavit tuto vlastnost než fusion na obrázku 1.

Na začátku fáze PreInit `Page` objektu vyvolá jeho [ `PreInit` události](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) a volá jeho [ `OnPreInit` metoda](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Programové nastavení stránky předlohy, pak můžeme buď vytvořit obslužnou rutinu události pro `PreInit` události nebo přepsání `OnPreInit` metody. Podívejme se na oba přístupy.

Začněte otevřením `Default.aspx.cs`, soubor třídy modelu code-behind náš web domovské stránky. Přidat obslužnou rutinu události pro danou stránku `PreInit` události tak, že zadáte následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Tady jsme nastavili `MasterPageFile` vlastnost. Aktualizovat kód tak, aby přiřadí hodnotu "~ / Site.master" k `MasterPageFile` vlastnost.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Pokud nastavíte zarážku a, který začíná ladění vám zobrazí pokaždé, když `Default.aspx` navštívené stránky nebo pokaždé, když se zpětného odeslání na tuto stránku `Page_PreInit` spustí obslužnou rutinu události a `MasterPageFile` je přiřazená vlastnost "~ / Site.master".

Alternativně můžete přepsat `Page` třídy `OnPreInit` metoda a nastavte `MasterPageFile` existuje vlastnost. V tomto příkladu teď není nastavený na hlavní stránce v konkrétní stránce, ale spíše z `BasePage`. Si možná Vzpomínáte, že jsme vytvořili vlastní stránku základní třídu (`BasePage`) zpátky [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu. Aktuálně `BasePage` přepsání `Page` třídy `OnLoadComplete` metody, kde se nastaví na stránce `Title` vlastnost podle data mapy webu. Aktualizujme `BasePage` také přepsat `OnPreInit` metoda programové určení stránky předlohy.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Protože všechny naše obsahu stránky odvozen od `BasePage`, všechny nyní mají jejich hlavní stránku prostřednictvím kódu programu přiřazen. V tomto okamžiku `PreInit` obslužné rutině událostí ve `Default.aspx.cs` je nadbytečný; můžete bez obav ho odstranit.

### <a name="what-about-thepagedirective"></a>Co`@Page`– direktiva?

Co může být trochu matoucí je, že stránky obsahu `MasterPageFile` vlastnosti jsou nyní určenými na dvou místech: programově v `BasePage` třídy `OnPreInit` metoda také prostřednictvím `MasterPageFile` atribut v každé obsahu stránky `@Page` směrnice.

První fázi životního cyklu stránky je fáze inicializace. Během této fáze `Page` objektu `MasterPageFile` vlastnost je přiřazena hodnota `MasterPageFile` atribut `@Page` – direktiva (Pokud je zadáno). Fáze PreInit sleduje fáze inicializace a je zde kde jsme programově nastavit `Page` objektu `MasterPageFile` vlastnost, a tím přepsat hodnoty přiřazené, z `@Page` směrnice. Protože jsme nastavujete `Page` objektu `MasterPageFile` vlastnost prostřednictvím kódu programu, může Odebereme `MasterPageFile` atribut z `@Page` direktiv, aniž by to ovlivnilo činnost koncového uživatele. Sami sobě tohoto přesvědčit, pokračujte a odebrat `MasterPageFile` atribut z `@Page` direktiv v `Default.aspx` a pak na stránce prostřednictvím prohlížeče. Dle očekávání, výstup je stejný jako, než byl odebrán atribut.

Zda `MasterPageFile` je nastavena prostřednictvím `@Page` – direktiva nebo prostřednictvím kódu programu je bezvýznamnými činnost koncového uživatele. Ale `MasterPageFile` atribut `@Page` – direktiva se používá sada Visual Studio v době návrhu, vytvoří WYSIWYG zobrazení v návrháři. Pokud se vrátíte do `Default.aspx` v sadě Visual Studio a přejděte do návrháře se zobrazí zpráva, "chybová stránka předlohy: stránka obsahuje ovládací prvky, které vyžadují odkaz na stránku předlohy, ale není zadaný žádný" (viz obrázek 2).

Stručně řečeno, je potřeba nechat `MasterPageFile` atribut `@Page` směrnice mohli využívat bohaté možnosti návrhu v sadě Visual Studio.


[![Použití sady Visual Studio @Page atribut direktivy MasterPageFile k vykreslení zobrazení návrhu](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Obrázek 02**: Visual Studio používá `@Page` direktivy `MasterPageFile` atribut k vykreslení zobrazení Návrh ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Krok 3: Vytvoření stránky předlohy alternativní

Protože stránce hlavní stránky obsahu lze nastavit programově v době běhu je možné dynamicky načíst konkrétní stránky předlohy podle kritérií externí. Tato funkce může být užitečné v situacích, kde rozložení tohoto webu je potřeba se liší podle uživatele. Například blogu modul webové aplikace mohou povolit jeho uživatelům si vybrat rozložení pro jejich blogu, kde každý rozložení souvisí s jinou stránku předlohy. Za běhu když návštěvník prohlíží blog uživatele webové aplikace potřebuje určit rozložení na blogu a dynamicky přidružit odpovídající stránky předlohy se stránkou obsahu.

Podívejme se na tom, jak dynamicky načíst stránku předlohy za běhu na základě některé externí kritérií. Náš web aktuálně obsahuje pouze jednu stránku předlohy (`Site.master`). Budeme potřebovat jiné stránky předlohy pro ilustraci zvolíte stránku předlohy za běhu. Tento krok se zaměřuje na vytvoření a konfigurace nové stránky předlohy. Krok 4 zjistí určující, jaké stránky předlohy k použití v době běhu.

Vytvoření nové stránky předlohy v kořenové složce s názvem `Alternate.master`. Přidat nové šablony stylů také na web s názvem `AlternateStyles.css`.


[![Přidejte další stránky předlohy a šablon stylů CSS soubor na web](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Obrázek 03**: přidat jiné stránky předlohy a souborů CSS na web ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image9.png))


Jsme navrhli tak `Alternate.master` stránku předlohy, která mají název zobrazený v horní části stránky, na střed a na navy na pozadí. Jsme distribuován levého sloupce a přesunout obsah pod `MainContent` ovládací prvek ContentPlaceHolder, který teď pokrývá celou šířku stránky. Kromě toho nixed neuspořádaný seznam poznatky a nahradili jsme ho s vodorovné seznamu výše `MainContent`. Jsem také aktualizovat písma a barvy použité na hlavní stránce (a při rozšíření i pro její obsahu stránky). Obrázek 4 ukazuje `Default.aspx` při použití `Alternate.master` stránky předlohy.

> [!NOTE]
> Technologie ASP.NET obsahuje schopnost definovat *motivy*. Motiv je kolekce obrázků, souborů CSS a styl související webové nastavení vlastností ovládacího prvku, které se můžou uplatnit na stránky v době běhu. Motivy jsou způsob, jak přejít, pokud váš web rozložení se liší pouze v obrázcích zobrazí a jejich pravidla šablon stylů CSS. V případě rozložení více podstatně liší například pomocí jiné webové ovládací prvky nebo s výrazně jiné rozložení, je potřeba používat samostatné stránky předlohy. Na konci tohoto kurzu pro další informace o motivech najdete v části Další čtení.


[![Stránek s obsahem teď můžete použít nový vzhled a chování](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Obrázek 04**: stránek s obsahem teď můžete použít nový vzhled a chování ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image12.png))


Když jsou začleněny předlohy a obsahu stránek kódu, `MasterPage` třídy kontroluje, ujistěte se, že každý obsah ovládacího prvku obsahu stránce odkazuje ContentPlaceHolder na stránce předlohy. Pokud je nalezen ovládací prvek obsahu, který odkazuje na neexistující ContentPlaceHolder, je vyvolána výjimka. Jinými slovy, je nutné, že stránka předlohy přiřazení obsahu stránky mít ContentPlaceHolder pro každý ovládací prvek na stránce obsahu obsahu.

`Site.master` Hlavní stránka obsahuje čtyři ovládací prvky ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Stránky obsahu v našem webu patří jen jeden nebo dva ovládací prvky obsahu; pro každý dostupný prvků ContentPlaceHolder ty další zahrnují ovládací prvek obsahu. Pokud naše nové stránky předlohy (`Alternate.master`) může být přiřazeno obsahu stránek, které mají ovládací prvky obsahu u všech prvků ContentPlaceHolder v `Site.master` je nezbytné, který `Alternate.master` obsahovat také stejné ovládací prvky ContentPlaceHolder jako `Site.master`.

Chcete-li získat vaše `Alternate.master` stránku předlohy, která vypadat podobně jako dolovat (viz obrázek 4), začněte tím, že definice stylů stránky předlohy v `AlternateStyles.css` šablony stylů. Přidat následující pravidla do `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

V dalším kroku přidejte následující deklarativní k `Alternate.master`. Jak je vidět, `Alternate.master` obsahuje čtyři ovládací prvky ContentPlaceHolder se stejným `ID` hodnoty jako ovládací prvky ContentPlaceHolder `Site.master`. Kromě toho obsahuje ovládací prvek ScriptManager, což je nezbytné pro tyto stránky na našem webu, které používají rozhraní ASP.NET AJAX framework.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testování nová stránka předlohy

K otestování této nové aktualizace stránky předlohy `BasePage` třídy `OnPreInit` metodu tak, aby `MasterPageFile` vlastnost je přiřazena hodnota "~ / Alternate.master" a potom přejděte na webovou stránku. Každé stránky by mělo fungovat bez chyb s výjimkou dvou: `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx`. Přidání produktu do ovládacího prvku DetailsView v `~/Admin/AddProduct.aspx` vede `NullReferenceException` z řádku kódu, který se pokouší nastavit na hlavní stránce `GridMessageText` vlastnost. Při návštěvě `~/Admin/Products.aspx` `InvalidCastException` je vyvolána při načtení stránky s touto zprávou: "nelze přetypovat objekt typu" ASP.alternate\_hlavní ' na typ ' ASP.site\_hlavní "."

K těmto chybám, protože `Site.master` použití modelu code-behind třída zahrnuje veřejné události, vlastnosti a metody, které nejsou definovány v `Alternate.master`. Část značky tyto dvě stránky mají `@MasterType` direktiva, která odkazuje `Site.master` stránky předlohy.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Navíc prvku DetailsView `ItemInserted` obslužné rutině událostí ve `~/Admin/AddProduct.aspx` zahrnuje kód, který se vrhá volného typu `Page.Master` vlastnost na objekt typu `Site`. `@MasterType` – Direktiva (používané tímto způsobem) a přetypování v `ItemInserted` pevně spojuje obslužné rutiny události `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky `Site.master` stránky předlohy.

Přerušit tento určitou úzkou svázanost můžeme nechat `Site.master` a `Alternate.master` odvozovat obecnou základní třídu, která obsahuje definice veřejných členů. Pod, abychom mohli aktualizovat `@MasterType` směrnice odkazují na tento typ běžné základní.

### <a name="creating-a-custom-base-master-page-class"></a>Vytvoření třídy hlavního vlastní základní stránky

Přidejte nový soubor třídy do `App_Code` složku s názvem `BaseMasterPage.cs` a jeho odvozovat `System.Web.UI.MasterPage`. Budeme muset definovat `RefreshRecentProductsGrid` metoda a `GridMessageText` vlastnost v `BaseMasterPage`, ale nelze přesunout jednoduše je tam z `Site.master` vzhledem k tomu, že tyto členy, pracovat s webové ovládací prvky, které jsou specifické pro `Site.master` hlavní stránky ( `RecentProducts` GridView a `GridMessage` popisku).

Co musíme udělat, je nakonfigurovat `BaseMasterPage` tak, že jsou definované tyto členy, ale ve skutečnosti jsou implementované `BaseMasterPage`v odvozených třídách (`Site.master` a `Alternate.master`). Tento druh dědičnosti je možné označením třídy a její členy jako `abstract`. Stručně řečeno, přidání `abstract` – klíčové slovo pro tyto dva členy, které oznamuje `BaseMasterPage` neimplementoval `RefreshRecentProductsGrid` a `GridMessageText`, ale, že se její odvozené třídy.

Můžeme také muset definovat `PricesDoubled` událost v `BaseMasterPage` a poskytují prostředky z odvozených tříd pro vyvolání události. Vzor použitý v rozhraní .NET Framework pro usnadnění tohoto chování je vytvořit veřejná událost v základní třídě a přidat chráněné, `virtual` metodu s názvem `OnEventName`. Odvozené třídy provést zavoláním této metody pro vyvolání události nebo můžete přepsat spustit kód bezprostředně před nebo po vyvolání události.

Aktualizace vašeho `BaseMasterPage` třídy tak, aby obsahoval následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Dále přejděte `Site.master` použití modelu code-behind třídy a nechat ji odvodit z `BaseMasterPage`. Protože `BaseMasterPage` je `abstract` musíme mají přednost před akcemi `abstract` členy v `Site.master`. Přidat `override` – klíčové slovo do definice metody a vlastnosti. Také aktualizovat kód, který vyvolá `PricesDoubled` událost v `DoublePrice` tlačítka `Click` obslužné rutiny události pomocí volání na základní třídu `OnPricesDoubled` metody.

Po změnách `Site.master` použití modelu code-behind třída by měla obsahovat následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Musíme také aktualizovat `Alternate.master`na použití modelu code-behind třídy odvozovat z `BaseMasterPage` a přepsat dva `abstract` členy. Ale protože `Alternate.master` neobsahuje GridView, že seznamy nejnovější produkty ani popisek, který zobrazí zprávu po nového produktu je přidána do databáze, tyto metody není nutné nic dělat.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Odkazování na třídy Base hlavní stránky

Teď, když jsme dokončili `BaseMasterPage` třídy a naše dvě hlavní stránky rozšíření, posledním krokem je aktualizovat `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky, které odkazují na tento společný typ. Začněte změnou `@MasterType` direktiv v obou stránky:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Do:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Namísto odkazování na cestu k souboru `@MasterType` vlastnosti nyní odkazuje na základní typ (`BaseMasterPage`). V důsledku toho silnými typy `Master` vlastnost použitá ve třídách modelu code-behind obě stránky je nyní typu `BaseMasterPage` (místo typu `Site`). Díky této změně na místě návštěvě `~/Admin/Products.aspx`. Dříve to bylo způsobeno chybu přetypování vzhledem k tomu, že na stránce je nakonfigurován pro použití `Alternate.master` stránky předlohy, ale `@MasterType` směrnice odkazuje `Site.master` souboru. Ale nyní vykreslí stránku bez chyb. Důvodem je, že `Alternate.master` hlavní stránky může být převeden na objekt typu `BaseMasterPage` (protože ji rozšiřuje).

Je jeden menší změnu, kterou je potřeba provést v `~/Admin/AddProduct.aspx`. Ovládací prvek DetailsView `ItemInserted` obslužná rutina události používá i silných `Master` vlastnosti a volného typu `Page.Master` vlastnost. Opravili jsme silného typu odkazu, když jsme aktualizovali `@MasterType` směrnice, ale stále potřeba aktualizovat volného typu odkazu. Nahraďte následující řádek kódu:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Následujícím kódem a která přetypovává `Page.Master` základní typ:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Krok 4: Určení, jaké stránky předlohy metody Bind obsahu stránky

Naše `BasePage` třídy aktuálně nastaví všechny obsahu stránek `MasterPageFile` vlastnosti na hodnotu pevně zakódované v PreInit fázi životního cyklu stránky. Abychom mohli aktualizovat tento kód na základní stránce předlohy na některé externí faktor. Možná na hlavní stránce načíst závisí na předvolby aktuálně přihlášeného uživatele. V takovém případě budeme muset psát kód ve `OnPreInit` metoda ve `BasePage` , který vyhledá aktuálně hostujícími uživatelova stránky předlohy.

Pojďme vytvořit webovou stránku, která umožňuje uživateli zvolit hlavní na stránce, které se má použít – `Site.master` nebo `Alternate.master` – a uložte tento výběr do proměnné relace. Začněte tím, že vytvoříte novou webovou stránku v kořenovém adresáři s názvem `ChooseMasterPage.aspx`. Při vytváření této stránce (nebo jakékoli jiné stránky obsahu od nynějška) nemusíte vázat na stránku předlohy, protože nastavený prostřednictvím kódu programu na hlavní stránce `BasePage`. Ale pokud vazbu na novou stránku na stránku předlohy pak nová stránka výchozí deklarativní obsahuje webový formulář a další obsah, který poskytl stránky předlohy. Bude nutné ručně nahradit tento kód se odpovídající ovládací prvky obsahu. Z tohoto důvodu můžu jednodušší svázat nová stránka technologie ASP.NET na stránku předlohy.

> [!NOTE]
> Protože `Site.master` a `Alternate.master` mají stejnou sadu ovládacích prvků ContentPlaceHolder nebude vadit, jaké stránky předlohy, zvolte při vytváření nového obsahu stránky. Pro zajištění konzistence, můžu Navrhněte pomocí `Site.master`.


[![Přidejte novou stránku obsahu na web](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Obrázek 05**: přidejte novou stránku obsahu na web ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image15.png))


Aktualizace `Web.sitemap` soubor zahrnout položku pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro stránky předlohy a ASP.NET AJAX lekce:


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Před přidáním libovolný obsah `ChooseMasterPage.aspx` stránku využít k aktualizaci modelu code-behind třídy stránky tak, aby se odvozuje od `BasePage` (spíše než `System.Web.UI.Page`). V dalším kroku přidejte ovládací prvek DropDownList na stránku, nastavte jeho `ID` vlastnost `MasterPageChoice`, a přidejte dvě položky ListItems s `Text` hodnoty "~ / Site.master" a "~ / Alternate.master".

Přidání ovládacího prvku tlačítko Web na stránku a nastavit jeho `ID` a `Text` vlastností `SaveLayout` a "Uložit rozložení výběru", v uvedeném pořadí. Na stránce deklarativní v tuto chvíli by měl vypadat nějak takto:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Při první návštěvě stránky budeme potřebovat zobrazit aktuálně vybrané stránce předlohy podle volby uživatele. Vytvoření `Page_Load` obslužné rutiny události a přidejte následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

Výše uvedený kód provede pouze při první návštěvě stránky (a ne na následné postbacky). Nejprve zkontroluje a zjistěte, jestli proměnné relace `MyMasterPage` existuje. Pokud ano, pokusí se najít odpovídající ListItem v `MasterPageChoice` DropDownList. Pokud je nalezen odpovídající ListItem, jeho `Selected` je nastavena na `true`.

Potřebujeme kód, který uloží výběru uživatele do `MyMasterPage` proměnné relace. Vytvořte obslužnou rutinu události pro `SaveLayout` tlačítka `Click` událostí a přidejte následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> Časem `Click` spustí obslužnou rutinu události na zpětné volání, již byla vybrána na stránce předlohy. Proto rozevíracího seznamu výběru uživatele nebudou platit až do přejděte na další stránku. `Response.Redirect` Prohlížeč znovu požádat o `ChooseMasterPage.aspx`.


S `ChooseMasterPage.aspx` stránka dokončení našich posledním úkolu je, aby `BasePage` přiřadit `MasterPageFile` nastavenou na hodnotu `MyMasterPage` proměnné relace. Pokud není nastavena proměnná relace `BasePage` ve výchozím nastavení `Site.master`.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Přestěhoval jsem se kód, který se přiřadí `Page` objektu `MasterPageFile` vlastnost z celkového počtu `OnPreInit` obslužné rutiny události a do dvou samostatných metod. Tato metoda první `SetMasterPageFile`, přiřadí `MasterPageFile` vlastnost na hodnotu vrácenou příkazem druhá metoda `GetMasterPageFileFromSession`. Byla vytvořena `SetMasterPageFile` metoda `virtual` tak, že budoucí třídy, které rozšiřují `BasePage` můžete volitelně přepsat implementaci vlastní logiky, v případě potřeby. Ukážeme příklad přepsání `BasePage`společnosti `SetMasterPageFile` vlastnost v dalším kurzu.


S tímto kódem na místě, přejděte `ChooseMasterPage.aspx` stránky. Na začátku `Site.master` je vybrané (viz obrázek 6) stránka předlohy, ale uživatel může vybrat různé stránky předlohy z rozevíracího seznamu.


[![Stránky obsahu se zobrazí na stránce předlohy Site.master](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Obrázek 06**: obsahu stránky se zobrazí pomocí `Site.master` stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![Stránky obsahu se teď zobrazují používající stránku předlohy Alternate.master](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Obrázek 07**: stránky obsahu se nyní zobrazí pomocí `Alternate.master` stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>Souhrn

Při návštěvě stránky obsahu jeho ovládací prvky obsahu jsou začleněny s ovládacími prvky ContentPlaceHolder jeho hlavní stránky. Označuje symbolem stránce hlavní stránky obsahu `Page` třídy `MasterPageFile` vlastnosti, které je přiřazeno k `@Page` direktivy `MasterPageFile` atribut během fáze inicializace. Jako tento kurz vám ukázal, My je můžeme přiřadit hodnotu, která `MasterPageFile` vlastnost, dokud uděláme před koncem PreInit fázi. Schopnost programové určení stránky předlohy otevře dveře pro pokročilejší scénáře, jako je dynamické vazby obsahu stránky na stránku předlohy založené na externí faktory.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Diagram životního cyklu stránky ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Přehled životního cyklu stránky ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Přehled skinů v šablonách a motivů ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Stránky předlohy: Tipy, triky a depeše](http://www.odetocode.com/articles/450.aspx)
- [Motivy obsažené v ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Suchi Banerjee. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-pages-and-asp-net-ajax-cs.md)
> [další](nested-master-pages-cs.md)
