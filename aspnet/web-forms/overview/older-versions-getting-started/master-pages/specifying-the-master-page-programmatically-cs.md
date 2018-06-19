---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Určení stránky předlohy prostřednictvím kódu programu (C#) | Microsoft Docs
author: rick-anderson
description: Vypadá na hlavní stránce obsahu stránce prostřednictvím kódu programu prostřednictvím obslužné rutiny události PreInit nastavení.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 2294ee2e58e55901d77958e7cf45dd74fc2a1187
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891149"
---
<a name="specifying-the-master-page-programmatically-c"></a>Určení stránky předlohy prostřednictvím kódu programu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Vypadá na hlavní stránce obsahu stránce prostřednictvím kódu programu prostřednictvím obslužné rutiny události PreInit nastavení.


## <a name="introduction"></a>Úvod

Od zahajovací příklad v [ *vytváření celém webu rozložení pomocí stránky předlohy*](creating-a-site-wide-layout-using-master-pages-cs.md), veškerý obsah stránky odkazujete jejich stránky předlohy deklarativně pomocí `MasterPageFile` atribut v `@Page`– direktiva. Například následující `@Page` – direktiva odkazuje stránky obsahu na hlavní stránce `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

[ `Page` Třída](https://msdn.microsoft.com/library/system.web.ui.page.aspx) v `System.Web.UI` obor názvů zahrnuje [ `MasterPageFile` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) , vrací cestu k obsahu stránce stránky předlohy, je tato vlastnost, kterou je nastavit `@Page` – direktiva. Tuto vlastnost lze použít také prostřednictvím kódu programu určit hlavní stránku obsahu stránce. Tento přístup je užitečné, pokud chcete dynamicky přiřadit na základě na externí faktorech, například uživatele, kteří navštěvují stránce stránky předlohy.

V tomto kurzu jsme přidejte druhou stránku předlohy pro náš web a dynamicky rozhodnout, které stránky předlohy pro použití v době běhu.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Krok 1: Podívejte se na stránku životního cyklu

Vždy, když přijat požadavek na webovém serveru pro stránku ASP.NET, který je obsahu stránce, musíte modul ASP.NET pojistka stránky obsahu odpovídající ovládacích prvků ContentPlaceHolder ovládací prvky na hlavní stránku. Tato fusion vytvoří jeden ovládací prvek hierarchie, která můžete pak pokračujte životního cyklu typické stránky.

Obrázek 1 ukazuje tento fusion. Krok 1 na obrázku 1 obsahuje původní obsah a hierarchií ovládací prvek stránky předlohy. Na konci tail fázi PreInit obsah se ovládací prvky na stránce přidají do odpovídající ContentPlaceHolders na hlavní stránce (krok 2). Po této fusion stránky předlohy slouží jako kořenem hierarchie roztaveného ovládacího prvku. To začleněny řízení hierarchie se pak přidá na stránku a vytvořit hierarchii dokončené ovládacích prvků (krok 3). Net výsledkem je, že stránky hierarchii ovládací prvek obsahuje hierarchii roztaveného ovládacích prvků.


[![Stránky předlohy a stránky obsahu ovládacího prvku hierarchií jsou začleněny společně během fáze PreInit](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Obrázek 01**: stránku předlohy a stránky obsahu ovládacího prvku hierarchií jsou začleněny společně během fáze PreInit ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Krok 2: Nastavení`MasterPageFile`vlastnost z kódu

Jaké stránky předlohy partakes v této fusion závisí na hodnotu `Page` objektu `MasterPageFile` vlastnost. Nastavení `MasterPageFile` atribut `@Page` – direktiva má projeví přiřazování `Page`na `MasterPageFile` vlastnost během fáze inicializace, což je velmi první fáze životního cyklu stránky. Tuto vlastnost jsme případně lze nastavit prostřednictvím kódu programu. Je však nutné, aby tuto vlastnost nastavit před provedením fusion na obrázku 1.

Na začátku fázi PreInit `Page` objektu vyvolá jeho [ `PreInit` událostí](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) a volá jeho [ `OnPreInit` metoda](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Nastavení stránky předlohy prostřednictvím kódu programu, potom můžeme buď vytvořit obslužnou rutinu události pro `PreInit` události nebo přepsání `OnPreInit` metoda. Podívejme se na obou přístupů.

Začněte otevřením `Default.aspx.cs`, soubor třídy kódu pro náš web domovské stránky. Přidání obslužné rutiny pro stránky `PreInit` událostí zadáním následujícího kódu:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Zde jsme můžete nastavit `MasterPageFile` vlastnost. Aktualizujte kód tak, aby se přiřadí hodnota "~ / Site.master" na `MasterPageFile` vlastnost.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Pokud nastavení boru přerušení a začněte s ladění, můžete se vždy, když `Default.aspx` navštívené stránky, nebo vždy, když probíhá zpětné volání na této stránce `Page_PreInit` spustí obslužnou rutinu události a `MasterPageFile` vlastnost je přiřazena k "~ / Site.master".

Alternativně můžete přepsat `Page` třídy `OnPreInit` metoda a sadu `MasterPageFile` vlastnost existuje. V tomto příkladu budeme není nastavena na hlavní stránku konkrétní stránky, ale spíše z `BasePage`. Odvolat, že jsme vytvořili vlastní základní stránky třídu (`BasePage`) zpět v [ *zadáte název, značky Meta a ostatní hlavičky HTML na hlavní stránce* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu. Aktuálně `BasePage` přepsání `Page` třídy `OnLoadComplete` metoda, kde se nastaví na stránku `Title` vlastnost podle data mapy webu. Umožňuje aktualizovat `BasePage` také přepsat `OnPreInit` metoda prostřednictvím kódu programu určit hlavní stránku.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Protože všechny stránek s obsahem odvozena od `BasePage`, všechny nyní mají jejich stránky předlohy prostřednictvím kódu programu přiřazen. V tomto okamžiku `PreInit` obslužné rutiny událostí v `Default.aspx.cs` nadbytečné; můžete bez obav odstranit ji.

### <a name="what-about-thepagedirective"></a>Co`@Page`– direktiva?

Co může být trochu matoucí je, že stránky obsahu `MasterPageFile` vlastnosti jsou nyní specifikované na dvou místech: prostřednictvím kódu programu v `BasePage` třídy `OnPreInit` metoda také jako prostřednictvím `MasterPageFile` atribut v každé obsahu stránce `@Page` – direktiva.

První fázi v průběhu životního cyklu stránky je fáze inicializace. Během této fáze `Page` objektu `MasterPageFile` vlastnost je přiřazena hodnota `MasterPageFile` atribut `@Page` – direktiva (Pokud je poskytována). Odpovídá fázi PreInit fáze inicializace a je zde prostřednictvím kódu programu nastavení `Page` objektu `MasterPageFile` vlastnost, a tím přepsal hodnotu přiřazenou z `@Page` – direktiva. Protože se jedná `Page` objektu `MasterPageFile` vlastnost prostřednictvím kódu programu, jsme by mohl odebrat `MasterPageFile` atribut z `@Page` direktiv bez ovlivnění činnost koncového uživatele. Snaží přimět sami tohoto, pokračujte a odebrat `MasterPageFile` atribut z `@Page` direktivy v `Default.aspx` a potom navštivte stránku prostřednictvím prohlížeče. Jak by se dalo očekávat, je výstup stejná jako předtím, než byl odebrán atribut.

Jestli `MasterPageFile` prostřednictvím je hodnota nastavena `@Page` – direktiva nebo prostřednictvím kódu programu není důležitá pro činnost koncového uživatele. Ale `MasterPageFile` atribut `@Page` je použita sadou Visual Studio během návrhu pro vytvoření WYSIWYG zobrazení v návrháři. Pokud se vraťte ke `Default.aspx` v sadě Visual Studio a přejděte do návrháře se zobrazí zpráva, "Chyba stránky předlohy: stránka obsahuje ovládací prvky, které vyžadují odkazem na stránku předlohy, ale není zadaný žádný" (viz obrázek 2).

Stručně řečeno, budete muset ponechte `MasterPageFile` atribut `@Page` – direktiva, abyste mohli využívat bohaté možnosti návrhu v sadě Visual Studio.


[![Visual Studio používá @Page Atribut MasterPageFile pro direktiva k vykreslení zobrazení návrhu](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Obrázek 02**: Visual Studio použije `@Page` – direktiva na `MasterPageFile` atribut k vykreslení zobrazení návrhu ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Krok 3: Vytvoření stránky hlavního alternativní

Protože stránky předlohy stránky obsahu lze nastavit prostřednictvím kódu programu v době běhu je možné se dynamicky načíst konkrétní stránky předlohy na základě některé externí kritérií. Tato funkce může být užitečné v situacích, kde rozložení lokality je potřeba se liší podle uživatele. Pro instanci blog modul webové aplikace může povolit svým uživatelům vybrat rozložení pro jejich blog, kde každé rozložení souvisí s jinou stránku předlohy. Za běhu při návštěvníka je zobrazení blog uživatele webové aplikace by muset určit rozložení na blogu a dynamicky přidružit stránky obsahu odpovídající stránky předlohy.

Podívejme se na tom, jak dynamicky načíst stránku předlohy za běhu na základě některé externí kritérií. Náš web aktuálně obsahuje pouze jednu stránku předlohy (`Site.master`). Potřebujeme jiné stránky předlohy pro ilustraci výběr stránky předlohy za běhu. Tento krok se zaměřuje na vytvoření a konfiguraci nové stránky předlohy. Krok 4 vypadá na určení, jaké stránky předlohy pro použití v době běhu.

Vytvořit novou stránku předlohy v kořenové složce s názvem `Alternate.master`. Také přidat nové šablony stylů na web s názvem `AlternateStyles.css`.


[![Přidejte další stránky předlohy a šablon stylů CSS soubor na webu](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Obrázek 03**: přidání další stránky předlohy a souborů CSS na web ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image9.png))


I jste určená `Alternate.master` stránky předlohy tak, aby měl název zobrazí v horní části stránky, na střed a na Námořnická modř pozadí. I jste distribuován levé sloupce a přesunout tento obsah pod `MainContent` ContentPlaceHolder řízení, které nyní zahrnuje celou šířku stránky. Kromě toho nixed neuspořádaný seznam lekce a nahradí vodorovné seznamu výše `MainContent`. I taky aktualizovat písma a barvy používané stránkou předlohy (a rozšířením jeho stránky obsahu). Obrázek 4 ukazuje `Default.aspx` při použití `Alternate.master` stránky předlohy.

> [!NOTE]
> Technologie ASP.NET obsahuje schopnost definovat *motivy*. Motiv je soubor bitové kopie, soubory šablon stylů CSS a související se styly webové ovládací prvek nastavení vlastností, které můžete použít na stránce za běhu. Motivy jsou způsob, jak můžete přejít, pokud váš web rozložení liší pouze v obrázcích zobrazí a jejich pravidla šablon stylů CSS. Pokud rozložení liší více podstatně, například pomocí různých ovládací prvky webového nebo nutnosti radikálně rozložení, budete muset použít samostatné stránky předlohy. Informace naleznete v části Další čtení na konci tohoto kurzu Další informace o motivů.


[![Naše stránky obsahu teď můžete použít nový vzhled a chování](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Obrázek 04**: stránek s obsahem teď můžete použít nový vzhled a chování ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image12.png))


Když jsou začleněny hlavní a kódu stránky obsahu, `MasterPage` třídy kontroluje, ujistěte se, že každý obsah ovládacího prvku obsahu stránce odkazuje ContentPlaceHolder na hlavní stránce. Pokud je nalezen ovládací prvek obsahu, který odkazuje na neexistující ContentPlaceHolder, je vyvolána výjimka. Jinými slovy, je nutné, že mají přiřazeny stránky obsahu stránky předlohy ContentPlaceHolder pro každý ovládací prvek obsahu stránce obsahu.

`Site.master` Stránky předlohy obsahuje čtyři ovládací prvky ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Část stránky obsahu v našem webu obsahovat jen jeden nebo dva ovládací prvky obsahu; ty další zahrnují obsah ovládacího prvku pro každý dostupný ContentPlaceHolders. Pokud naší nové stránky předlohy (`Alternate.master`) může být přiřazena k obsahu stránky, které mají ovládací prvky obsahu pro všechny ContentPlaceHolders v `Site.master` , pak je nezbytné, `Alternate.master` také zahrnovat se stejnými ovládacími prvky ContentPlaceHolder jako `Site.master`.

Chcete-li získat vaše `Alternate.master` stránky předlohy podobným Moje (viz obrázek 4), začněte definováním stránky předlohy styly v `AlternateStyles.css` šablony stylů. Přidat následující pravidla do `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Dál přidejte následující deklarativní k `Alternate.master`. Jak vidíte, `Alternate.master` obsahuje čtyři ovládací prvky ContentPlaceHolder se stejným `ID` hodnoty jako ovládacích prvků ContentPlaceHolder v `Site.master`. Kromě toho zahrnuje ovládací prvek ScriptManager, který je potřebný pro tyto stránky v našem webu, které používají rozhraní ASP.NET AJAX.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testování nová stránka předlohy

K testování této nové aktualizace stránky předlohy `BasePage` třídy `OnPreInit` metoda tak, aby `MasterPageFile` vlastnost je přiřazena hodnota "~ / Alternate.master" a pak přejděte na webovou stránku. Každé stránce by měla fungovat bez chyba s výjimkou dva: `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx`. Přidání produktu DetailsView v `~/Admin/AddProduct.aspx` výsledkem `NullReferenceException` z řádku kódu, který se pokouší nastavit stránky předlohy `GridMessageText` vlastnost. Při návštěvě `~/Admin/Products.aspx` `InvalidCastException` je vyvolána při načtení stránky se zprávou: "nelze vrátit objekt typu ' ASP.alternate\_hlavní ' na typ" ASP.site\_hlavní '. "

K těmto chybám, protože `Site.master` kódu třída zahrnuje veřejné události, vlastnosti a metody, které nejsou definovány v `Alternate.master`. Mít značek část tyto dvě stránky `@MasterType` direktiva, která odkazuje `Site.master` stránky předlohy.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Navíc prvku DetailsView `ItemInserted` obslužné rutiny událostí v `~/Admin/AddProduct.aspx` zahrnuje kód, který vrhá volného typu `Page.Master` vlastností pro objekt typu `Site`. `@MasterType` – Direktiva (používá se tímto způsobem) a přetypování v `ItemInserted` obslužné rutiny události pevně spojuje `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky k `Site.master` stránky předlohy.

Pro přerušení tento úzkou párování jsme může mít `Site.master` a `Alternate.master` odvozena od obecná základní třída, která obsahuje definice pro veřejné členy. Následující, budeme aktualizovat `@MasterType` – direktiva, chcete-li tento běžné základního typu.

### <a name="creating-a-custom-base-master-page-class"></a>Vytvoření třídy hlavní vlastní základní stránky

Přidat nový soubor třídy k `App_Code` složku s názvem `BaseMasterPage.cs` a ji odvodit z `System.Web.UI.MasterPage`. Je potřeba definovat `RefreshRecentProductsGrid` metoda a `GridMessageText` vlastnost v `BaseMasterPage`, ale jsme nelze jednoduše je přesunout existuje z `Site.master` vzhledem k tomu, že tito členové pracovat webové ovládací prvky, které jsou specifické pro `Site.master` stránky předlohy ( `RecentProducts` Rutina GridView a `GridMessage` štítek).

Co je potřeba udělat je konfigurace `BaseMasterPage` tak, aby tito členové jsou definovány existuje, ale ve skutečnosti implementované `BaseMasterPage`je odvozených třídách (`Site.master` a `Alternate.master`). Tento typ dědičnosti je možné označením třídy a její členy jako `abstract`. Stručně řečeno, přidání `abstract` – klíčové slovo pro tyto dva členy oznamuje, který `BaseMasterPage` neimplementovala `RefreshRecentProductsGrid` a `GridMessageText`, ale, že se jejich odvozené třídy.

Také je potřeba definovat `PricesDoubled` událost v `BaseMasterPage` a poskytují prostředky odvozené třídy pro vyvolání události. Vzor použít v rozhraní .NET Framework pro usnadnění toto chování je vytvoření veřejnou událost v základní třídě a přidání chráněn, `virtual` metodu s názvem `OnEventName`. Odvozené třídy pak můžete volat tuto metodu pro vyvolání události nebo ji ke spouštění kódu bezprostředně před nebo po událost se vyvolá, můžete přepsat.

Aktualizace vašeho `BaseMasterPage` třídy tak, aby obsahoval následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Potom přejděte na `Site.master` kódu třídy a ji odvodit z `BaseMasterPage`. Protože `BaseMasterPage` je `abstract` musíme přednost před akcemi `abstract` členy v `Site.master`. Přidat `override` – klíčové slovo v definicích metod a vlastností. Také aktualizovat kód, který vyvolá `PricesDoubled` událost v `DoublePrice` tlačítka `Click` obslužné rutiny události s výzvou k základní třídě `OnPricesDoubled` metoda.

Po tyto úpravy `Site.master` třída kódu by měla obsahovat následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Také je potřeba aktualizovat `Alternate.master`je odvozena od třídy kódu `BaseMasterPage` a přepsat dva `abstract` členy. Ale protože `Alternate.master` neobsahuje GridView, že seznamy nejnovější produkty ani štítek, který zobrazí zprávu po novém produktu je přidáno do databáze, tyto metody není nutné provádět žádné kroky.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Odkazování na hlavní základní třídy stránky

Teď, když jsme dokončili `BaseMasterPage` třídy a naše dvě hlavní stránky je rozšíření, posledním krokem je aktualizovat `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky, které odkazují na tento typ běžné. Začněte tím, že změna `@MasterType` direktivy v obou stránky z:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Do:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Místo odkazující na cestu k souboru `@MasterType` nyní odkazuje základního typu vlastnost (`BaseMasterPage`). V důsledku toho silného typu `Master` vlastnost použitá ve třídách modelu code-behind obě stránky je nyní typu `BaseMasterPage` (místo typu `Site`). V této změně zavedené pokroku `~/Admin/Products.aspx`. Dříve, to bylo způsobeno chybu přetypování protože stránky je nakonfigurován pro použití `Alternate.master` stránky předlohy, ale `@MasterType` – direktiva odkazuje `Site.master` souboru. Ale nyní vykreslí stránku bez chyby. Důvodem je, že `Alternate.master` stránky předlohy lze převést na objekt typu `BaseMasterPage` (protože rozšiřuje ji).

Neexistuje jeden malé změny, která je potřeba provést v `~/Admin/AddProduct.aspx`. Ovládací prvek DetailsView `ItemInserted` obslužné rutiny události používá i silného typu `Master` vlastnosti a volného typu `Page.Master` vlastnost. Když aktualizovali jsme vyřešili silného typu odkaz `@MasterType` – direktiva, ale nemůžeme přesto potřebujete aktualizovat volného typu odkaz. Nahraďte následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Pomocí následující příkaz, který vrhá `Page.Master` na základní typ:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Krok 4: Určení, jaké stránky předlohy pro vazbu na stránky obsahu

Naše `BasePage` třída aktuálně nastaví všechny obsahu stránek `MasterPageFile` vlastnosti na hodnotu pevně zakódovaná ve fázi PreInit životního cyklu stránky. Aktualizujeme tento kód, aby na některé externí Multi-Factor základní stránky předlohy. Možná stránky předlohy načíst závisí na předvolby aktuálně přihlášeného uživatele. V takovém případě je potřeba psát kód `OnPreInit` metoda v `BasePage` , vyhledává aktuálně hostujícími uživatelské předvolby stránky předlohy.

Umožňuje vytvořit webovou stránku, která umožňuje uživatelům zvolit použití – stránka předlohy `Site.master` nebo `Alternate.master` - a uložte tato volba v proměnné relace. Začněte vytvořením novou webovou stránku v kořenovém adresáři s názvem `ChooseMasterPage.aspx`. Při vytváření této stránce (nebo všechny ostatní stránky obsahu od nynějška) nemusíte navázat jej na stránku předlohy, protože stránky předlohy nastavena prostřednictvím kódu programu v `BasePage`. Ale pokud není vytvořit vazbu na novou stránku na stránku předlohy pak nová stránka výchozí deklarativní obsahuje webového formuláře a další obsah, který poskytl stránky předlohy. Budete muset ručně nahraďte tento kód odpovídající ovládací prvky obsahu. Z tohoto důvodu I jednodušší pro vazbu novou stránku ASP.NET na hlavní stránku.

> [!NOTE]
> Protože `Site.master` a `Alternate.master` stejné nastavili ovládacích prvků ContentPlaceHolder nezávisle na tom, jaké stránky předlohy, zvolte při vytváření nové stránky obsahu. Konzistence, I Navrhněte pomocí `Site.master`.


[![Přidání nové stránky obsahu webu](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Obrázek 05**: Přidání nové stránky obsahu webu ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image15.png))


Aktualizace `Web.sitemap` souboru záznam pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro hlavní stránky a prvku ASP.NET AJAX lekce:


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Před přidáním jakékoli obsahu `ChooseMasterPage.aspx` stránky pozorně aktualizujte třídy kódu stránky tak, aby je odvozena z `BasePage` (místo `System.Web.UI.Page`). V dalším kroku přidat ovládací prvek rozevírací seznam na stránku, nastavte jeho `ID` vlastnost `MasterPageChoice`, a přidejte dvě položky ListItems s `Text` hodnoty "~ / Site.master" a "~ / Alternate.master".

Přidání ovládacího prvku tlačítko na stránku a nastavit jeho `ID` a `Text` vlastnosti, které chcete `SaveLayout` a "Uložit rozložení volba", v uvedeném pořadí. V tomto okamžiku vaší stránky deklarativní by měl vypadat takto:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Při první návštěvě stránce musíme zobrazit aktuálně vybrané stránky předlohy volby uživatele. Vytvoření `Page_Load` obslužné rutiny události a přidejte následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

Výše uvedený kód spustí, pouze při první návštěvě stránce (a ne na následné postback). Nejdřív zkontroluje, pokud proměnné relace `MyMasterPage` existuje. Pokud ano, pokusí se najít odpovídající ListItem v `MasterPageChoice` rozevírací seznam. Pokud je nalezen odpovídající ListItem, jeho `Selected` je nastavena na `true`.

Potřebujeme kód, který uloží volby uživatele do `MyMasterPage` proměnné relace. Vytvoření obslužné rutiny událostí `SaveLayout` tlačítka `Click` událostí a přidejte následující kód:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> V čase `Click` zpracování události je spuštěn na zpětné volání, již byla vybrána stránky předlohy. Výběr v rozevíracím seznamu uživatele proto nebude platit možné, dokud přejděte na další stránku. `Response.Redirect` Přinutí prohlížeč požadovat znovu `ChooseMasterPage.aspx`.


S `ChooseMasterPage.aspx` stránka dokončení, naše poslední je mít `BasePage` přiřadit `MasterPageFile` vlastnost podle hodnotu `MyMasterPage` proměnné relace. Pokud proměnná relace není nastavená `BasePage` výchozí `Site.master`.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Po přesunutí kód, který přiřazuje `Page` objektu `MasterPageFile` vlastnost z `OnPreInit` obslužné rutiny události a do dvou samostatných metod. Tato metoda první `SetMasterPageFile`, přiřadí `MasterPageFile` vlastnost na hodnotu vrácenou druhé metody `GetMasterPageFileFromSession`. Provedené `SetMasterPageFile` metoda `virtual` tak, aby budoucí třídy, které rozšiřují `BasePage` můžete volitelně přepsat k implementaci vlastní logiky, v případě potřeby. Ukážeme příklad přepsání `BasePage`na `SetMasterPageFile` vlastnost v dalším kurzu.


S tímto kódem na místě, přejděte `ChooseMasterPage.aspx` stránky. Standardně `Site.master` stránky předlohy je vybrané (viz obrázek 6), ale uživatel může zvolit jinou stránku předlohy z rozevíracího seznamu.


[![Zobrazí obsah stránky pomocí stránky předlohy Site.master](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Obrázek 06**: obsahu stránky se zobrazí pomocí `Site.master` – stránka předlohy ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![Stránky obsahu se nyní zobrazují pomocí Alternate.master hlavní stránky.](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Obrázek 07**: obsahu stránky se nyní zobrazí pomocí `Alternate.master` – stránka předlohy ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>Souhrn

Když je navštívené stránky obsahu, jeho ovládací prvky obsahu jsou začleněny s ovládacími prvky ContentPlaceHolder jeho hlavní stránky. Stránky předlohy obsahu stránce označuje `Page` třídy `MasterPageFile` vlastnost, která je přiřazená `@Page` – direktiva na `MasterPageFile` atribut během fáze inicializace. Jako tento kurz vám ukázal, jsme můžete přiřadit hodnotu, která `MasterPageFile` , pokud jsme tak učinit před koncem fázi PreInit vlastnost. Schopnost prostřednictvím kódu programu určit hlavní stránku otevře dveře pokročilejší scénáře, jako je například dynamické vazby stránky obsahu na stránku předlohy založené na externí faktory.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Diagram životního cyklu stránky ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Přehled životního cyklu stránky ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Motivy ASP.NET a vzhledy – přehled](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Hlavní stránky: Tipy, triky a depeše](http://www.odetocode.com/articles/450.aspx)
- [Motivy technologie ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Suchi Banerjee. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](master-pages-and-asp-net-ajax-cs.md)
> [další](nested-master-pages-cs.md)
