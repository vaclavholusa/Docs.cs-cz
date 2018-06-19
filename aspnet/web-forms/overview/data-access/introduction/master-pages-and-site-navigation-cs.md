---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Hlavní stránky a navigace na webu (C#) | Microsoft Docs
author: rick-anderson
description: Jeden společným znakem uživatelsky přívětivý webů je, ke kterým mají konzistentní, na webu stránky rozložení a navigační schématu. V tomto kurzu vypadá o tom, jak y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7bf834373c6f44272a9c4c816e649fefe3247a4e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887597"
---
<a name="master-pages-and-site-navigation-c"></a>Hlavní stránky a navigace na webu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) nebo [stáhnout PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Jeden společným znakem uživatelsky přívětivý webů je, ke kterým mají konzistentní, na webu stránky rozložení a navigační schématu. V tomto kurzu vypadá na tom, jak můžete vytvořit konzistentní vzhled a chování na všech stránkách, které můžete snadno aktualizovat.


## <a name="introduction"></a>Úvod

Jeden společným znakem uživatelsky přívětivý webů je, ke kterým mají konzistentní, na webu stránky rozložení a navigační schématu. Technologie ASP.NET 2.0 přináší dvě nové funkce, které výrazně usnadňují implementaci obě stránky na webu rozložení a navigace schématu: navigace na webu a stránky předlohy. Stránky předlohy umožňují vývojářům vytvářet šablony na webu s určenou upravitelné oblasti. Pak lze použít tuto šablonu na stránky ASP.NET v této lokalitě. Tyto stránek ASP.NET stačí pouze zadat obsah pro stránky předlohy je zadané upravitelné oblasti všechny značky v hlavní stránky je stejný jako na všech stránkách ASP.NET, které používají stránky předlohy. Tento model umožňuje vývojářům definovat a centralizovat rozložení stránky na webu, a tím usnadnit vytvoření konzistentní vzhled a chování napříč všechny stránky, které můžete snadno aktualizovat.

[Navigační systému lokality](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) poskytuje mechanismus pro vývojáře stránky k definování mapy webu i rozhraní API pro tuto mapu lokality má být dotazován prostřednictvím kódu programu. Nové ovládací prvky webové navigace, které nabídky, TreeView a SiteMapPath usnadňují vykreslit nebo její část mapy webu v běžné navigační prvek uživatelského rozhraní. Budeme používat výchozí zprostředkovatele služby site navigace, což znamená, že naše mapy webu bude definována v souboru formátu XML.

Pro ilustraci tyto koncepty a bylo víc použít web naše kurzy, věnujte této lekci definování rozložení stránky na webu, implementace mapy webu a přidání uživatelské rozhraní. Na konci tohoto kurzu máme web dokonalý návrhu pro sestavení naše kurz webové stránky.


[![Konečný výsledek tohoto kurzu](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Obrázek 1**: End výsledek z tento kurz ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Krok 1: Vytvoření stránky předlohy

Prvním krokem je vytvoření stránky předlohy pro lokalitu. Aktuálně náš web se skládá pouze typové datové sady (`Northwind.xsd`v `App_Code` složky), BLL třídy (`ProductsBLL.cs`, `CategoriesBLL.cs`a tak dále všechny v `App_Code` složky), databáze (`NORTHWND.MDF`v `App_Data` složka), konfigurační soubor (`Web.config`) a soubor šablony stylů CSS (`Styles.css`). Vyčistit I tyto stránek a demonstraci pomocí DAL a BLL z první dva kurzy vzhledem k tomu, že jsme přezkoumání tyto příklady podrobněji v budoucnu kurzech soubory.


![Soubory v našem projektu](master-pages-and-site-navigation-cs/_static/image4.png)

**Obrázek 2**: soubory v našem projektu


Chcete-li vytvořit stránku předlohy, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. Pak vyberte typ stránky předlohy ze seznamu šablon a pojmenujte ji `Site.master`.


[![Přidání nové stránky předlohy webu](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Obrázek 3**: Přidat stránku předlohy na web ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image7.png))


Zadejte sem rozložení stránky na webu na hlavní stránce. Můžete použít zobrazení návrhu a přidat libovolnou rozložení a webové kontroly je nutné, nebo můžete ručně přidat kód ručně v zobrazení zdroje. Moje hlavní stránce použít [kaskádových stylů](http://www.w3schools.com/css/default.asp) pro umístění a stylů CSS nastavení definované v souboru externích `Style.css`. Při nelze zjistit z značek vidíte níže, jsou definovaná pravidla šablon stylů CSS tak, aby navigaci `<div>`na obsah je absolutně nastavený tak, aby se zobrazí na levé straně a má pevnou šířku 200 pixelů.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Na hlavní stránce definuje rozložení statické stránky a oblastí, které lze upravit pomocí stránek ASP.NET, které používají stránky předlohy. Tyto obsahu upravitelné oblasti jsou označeny ContentPlaceHolder řízení, které jsou viditelné v rámci obsah `<div>`. Naše stránka předlohy má jeden ContentPlaceHolder (`MainContent`), ale stránky předlohy mohou mít více ContentPlaceHolders.

Pomocí výše uvedených kód přepnutí do návrhového zobrazení ukazuje rozložení stránky předlohy. Všechny stránek ASP.NET, které používají tuto stránku předlohy bude mít toto uniform rozložení možnost zadejte kód pro `MainContent` oblast.


[![Stránky předlohy, při zobrazení v zobrazení návrhu](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Obrázek 4**: stránku předlohy, při prohlížení prostřednictvím zobrazení návrhu ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Krok 2: Přidání domovské stránky na web

Se stránkou předlohy definované jsme připraveni pro přidání stránek ASP.NET pro web. Začněme přidáním `Default.aspx`, náš web domovské stránky. Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. Vyberte možnost webového formuláře ze seznamu šablon a název souboru `Default.aspx`. Kromě toho zaškrtněte políčko "Vyberte stránku předlohy".


[![Přidat nový webový formulář, kontrola vyberte stránku předlohy zaškrtávací políčko](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Obrázek 5**: přidejte nový webový formulář, kontrola vyberte stránku předlohy políčko ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image13.png))


Po kliknutí na tlačítko OK, jsme se výzva k výběru jaké stránky předlohy měli používat tuto novou stránku ASP.NET. I když můžete mít více hlavní stránky ve vašem projektu, budeme mít jenom jeden.


[![Vyberte stránku předlohy, kterou by měl použít tuto stránku ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Obrázek 6**: Vyberte stránku předlohy použití by měl tento ASP.NET stránky ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image16.png))


Po výběru stránky předlohy, bude obsahovat nové stránky ASP.NET následující kód:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

V `@Page` existuje direktiva je odkaz na soubor stránky předlohy použitý (`MasterPageFile="~/Site.master"`), a značky stránky ASP.NET obsahuje ovládací prvek obsahu pro každou z ovládacích prvků ContentPlaceHolder definované na hlavní stránce s ovládacím prvkem `ContentPlaceHolderID` mapování obsahu pro konkrétní ContentPlaceHolder řízení. Ovládací prvek obsahu je třeba umístit značku bude zobrazen v odpovídající ContentPlaceHolder. Nastavte `@Page` – direktiva na `Title` atribut na domovskou stránku a přidejte nějaký uvítacímu obsah do ovládacího prvku obsahu:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

`Title` Atribut `@Page` – direktiva umožňuje nastavit nadpis stránky ze stránky ASP.NET, i když `<title>` element je definována v stránky předlohy. Jsme můžete také nastavit nadpis programově pomocí `Page.Title`. Všimněte si také, že hlavní stránce odkazy na šablony stylů (například `Style.css`) se automaticky aktualizují, aby fungují jakoukoli stránku ASP.NET, bez ohledu na to, jaké adresáře stránky ASP.NET je v vzhledem k hlavní stránce.

Přepnutí do návrhového zobrazení, že jsme najdete v části Vzhled naši stránku v prohlížeči. Všimněte si, že v návrhu zobrazení pro stránku ASP.NET, aby se pouze obsahu upravitelné oblasti upravovat kód bez ContentPlaceHolder na hlavní stránce definován šedě.


[![Zobrazení návrhu pro stránku ASP.NET zobrazuje upravitelný a upravovat oblastí](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Obrázek 7**: zobrazení návrhu pro ASP.NET stránky ukazuje, jak upravit a oblastí, Non-upravit ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image19.png))


Když `Default.aspx` prohlížečem je navštívené stránky, modul ASP.NET automaticky sloučí obsah stránky hlavní stránky a ASP. NET obsah a vykreslí sloučené obsah do konečné kódu HTML, která bude zaslána požadujícího prohlížeče. Při aktualizaci obsahu stránky předlohy, bude mít všechny stránky ASP.NET, které používají tuto stránku předlohy remerged s novou stránku předlohy obsahu při příštím vyžádání obsah. Stručně řečeno, umožňuje modelu stránky předlohy pro jednu stránku rozložení šablona být definována (stránky předlohy) jejíž změny jsou okamžitě projeví celého webu.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Přidání další ASP.NET stránky na web

Umožňuje přidat další zástupných procedur stránka ASP.NET k lokalitě, která bude obsahovat nakonec různé sestavy ukázky chvíli trvat. Budou existovat více než 35 ukázky celkem, takže místo vytvoření všechny stránky se zakázaným inzerováním umožňuje právě vytvořit první několik. Vzhledem k tomu, že bude také mnoho kategorií ukázky, přidejte ukázky lépe spravovat složku pro kategorií. Přidejte následující tři složky teď:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Nakonec přidejte nové soubory, jak je znázorněno v Průzkumníku řešení na obrázku 8. Při přidávání jednotlivých souborů, nezapomeňte zaškrtnutím políčka "Vyberte stránku předlohy".


![Přidejte následující soubory](master-pages-and-site-navigation-cs/_static/image20.png)

**Obrázek 8**: přidejte následující soubory


## <a name="step-2-creating-a-site-map"></a>Krok 2: Vytvoření mapy webu

Jedním z problémů správy webu skládá z více než několik stránek poskytuje přímý způsob pro návštěvníky procházet webu. Musí být definovány navigační strukturu webu. Tato struktura v dalším kroku musí přeložit na elementy navigaci uživatelského rozhraní, jako jsou nabídky nebo cesta ke stránce. Nakonec je třeba udržovat a aktualizovat, protože se přidají nové stránky v lokalitě a existující odebrat tento celý proces. Před aplikaci ASP.NET 2.0 se vývojáři na své vlastní pro vytvoření webu navigační strukturu, údržbu ho a překladu do elementy navigaci uživatelského rozhraní. S prostředím ASP.NET 2.0 ale vývojáři můžou velmi flexibilní integrovaný systém navigace webu.

Systém navigace na webu technologie ASP.NET 2.0 poskytuje prostředky pro vývojáře k definování mapy webu a potom k těmto informacím přístup prostřednictvím programovací rozhraní API. ASP.NET se dodává s poskytovatel mapy webu, která očekává data mapy webu k uložení do souboru XML, který je naformátovaný určitým způsobem. Ale vzhledem k tomu, že systém navigace na webu je založený na [modelu poskytovatelů](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) ho může být rozšířené k podpoře alternativní způsoby pro serializaci mapy informace o webovém serveru. Článek Jeff Prosise [SQL lokality mapu zprostředkovatele jste jsme byla čekání pro](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) ukazuje, jak vytvořit poskytovatele mapy webu, ukládá mapy webu v databázi systému SQL Server; Další možností je vytvořit [na základě poskytovatel mapy webu strukturu systému souborů](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

V tomto kurzu ale můžeme použít výchozí poskytovatel mapy webu, který se dodává s prostředím ASP.NET 2.0. K vytvoření mapy webu, jednoduše klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, vyberte Přidat novou položku a zvolte možnost mapy webu. Ponechejte pole název jako `Web.sitemap` a klikněte na tlačítko Přidat.


[![Do projektu přidejte mapy webu](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Obrázek 9**: Přidání mapy webu do vašeho projektu ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image23.png))


Souboru mapy webu je soubor XML. Všimněte si, že Visual Studio poskytuje IntelliSense pro strukturu mapy webu. Souboru s mapováním lokality musí mít `<siteMap>` uzlu jako kořenový uzel, který musí obsahovat přesně jeden `<siteMapNode>` podřízený element. Tento první `<siteMapNode>` element může obsahovat libovolný počet potomka `<siteMapNode>` elementy.

Definujte mapy webu tak, aby napodoboval strukturu systému souborů. To znamená, přidejte `<siteMapNode>` element pro všechny tři složky a podřízených `<siteMapNode>` elementy pro každou stránku ASP.NET v těchto složek, například takto:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Mapy webu definuje navigační strukturu webu, která je hierarchie, která popisuje různé části webu. Každý `<siteMapNode>` element v `Web.sitemap` reprezentuje oddíl v navigační struktuře daného webu.


[![Hierarchická struktura navigační představuje mapy webu](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Obrázek 10**: mapy webu představuje hierarchická struktura navigační ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image26.png))


Technologie ASP.NET poskytuje struktury mapy webu prostřednictvím rozhraní .NET Framework [Třída SiteMap](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Tato třída obsahuje `CurrentNode` vlastnost, která vrací informace o části návštěvy aktuálně; `RootNode` vlastnost vrací kořenu mapy webu (v našem mapy webu domácí). Jak `CurrentNode` a `RootNode` vlastnosti vrátit [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instancí, které mají vlastnosti jako `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`a tak dále, které umožňují, aby mapy webu hierarchie být proveden pro vaši firmu.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Krok 3: Zobrazení nabídky založené na mapy webu

Přístup k datům v technologii ASP.NET 2.0 lze provést prostřednictvím kódu programu, stejně jako v nástroji ASP.NET 1.x, nebo deklarativně, prostřednictvím nové [ovládací prvky zdroje dat](https://msdn.microsoft.com/library/ms227679.aspx). Existuje několik předdefinovaných ovládací prvky zdroje dat například prvku SqlDataSource pro přístup k datům relační databáze, ovládacího prvku ObjectDataSource pro přístup k datům z třídy a dalších. Můžete například vytvořit vlastní [vlastní datové zdroje ovládací prvky](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Ovládací prvky zdroje dat slouží jako server proxy mezi stránku ASP.NET a základní data. Chcete-li zobrazit ovládací prvek zdroje dat načtená data, jsme budete obvykle přidejte další ovládací prvek webové stránky a navázat jej data zdrojového kódu. Chcete-li vytvořit vazbu ovládacího prvku webového do ovládacího prvku zdroje dat, jednoduše nastavte ovládacího prvku webového `DataSourceID` vlastnost na hodnotu ovládacího prvku zdroje dat `ID` vlastnost.

Technologie ASP.NET a usnadňuje práci s daty mapy webu, obsahuje ovládací prvek SiteMapDataSource, který umožňuje vytvořit vazbu ovládacího prvku webového proti mapy webu náš web. Dva ovládacích prvků TreeView a nabídky se běžně používají k nabízí uživatelské rozhraní, navigace. Pokud chcete vytvořit vazbu data mapy webu na jednu z těchto dvou ovládacích prvků, jednoduše přidat SiteMapDataSource na stránku společně s elementu TreeView nebo nabídky řídit, jehož `DataSourceID` vlastnost je nastavena odpovídajícím způsobem. Například může přidáme ovládacího prvku nabídka na hlavní stránku pomocí následující kód:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Pro lepší stupeň kontroly nad emitovaného HTML, jsme vazbu ovládacího prvku SiteMapDataSource do ovládacího prvku opakovače takto:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Ovládací prvek SiteMapDataSource vrátí úrovni hierarchie jeden mapy webu současně, počínaje kořenového uzlu mapy webu (domácí, v našem mapy webu), pak další úroveň (základní vytváření sestav, filtrování sestavy a přizpůsobit formátování) a tak dále. Při vytváření vazby SiteMapDataSource k prvku Repeater, výčtu první úroveň vrátí a vytvoří `ItemTemplate` pro každou `SiteMapNode` instance v této první úroveň. Pro přístup k určité vlastnosti `SiteMapNode`, můžeme použít `Eval(propertyName)`, což je, jak se nám získat každý `SiteMapNode`na `Url` a `Title` vlastností ovládacího prvku hypertextový odkaz.

V předchozím příkladu opakovače vykreslí následující kód:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Tvoří tyto uzly mapy webu (základní vytváření sestav, filtrování sestavy a přizpůsobit formátování) *druhý* mapy webu vykreslované není první úrovně. Důvodem je, že SiteMapDataSource `ShowStartingNode` vlastnost nastavena na hodnotu False, způsobuje SiteMapDataSource vynechat kořenového uzlu mapy webu a místo toho spustíte vrácením druhé úrovně v hierarchii této lokality mapy.

K zobrazení podřízených objektů pro vytváření základních sestav, filtrování sestavy a přizpůsobit formátování `SiteMapNode` s, přidáme jiné opakovače k počáteční opakovače `ItemTemplate`. Tento druhý opakovače bude navázán `SiteMapNode` instance `ChildNodes` vlastnost takto:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Tyto dvě opakovače mít za následek následující kód (zápis pomocí značek, byla odebrána jako stručný výtah):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Pomocí šablon stylů CSS styly zvolený z [Rachel Andrew](http://www.rachelandrew.co.uk/)je sešit [Anthology šablon stylů CSS: 101 důležité tipy, triky, &amp; útoky zvenku](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` a `<li>` elementy jsou ve tak, aby kód vytvoří následující visual výstup:


![Z nabídky se skládá ze dvou opakovače a některé šablon stylů CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Obrázek 11**: nabídky se skládá ze dvou opakovače a některé šablon stylů CSS


Tato nabídka je na hlavní stránce a vázaný k mapy webu definované v `Web.sitemap`znamená, že všechny změny do mapy webu se ihned projeví na všechny stránky, které používají `Site.master` stránky předlohy.

## <a name="disabling-viewstate"></a>Zakázání ViewState

Všechny ovládací prvky ASP.NET můžete volitelně zachovat jejich stavu [zobrazení stavu](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), který serializuje jako skryté pole formuláře v vykreslené kódu HTML. Stav zobrazení se používá z ovládacích prvků pamatovat jejich stavu prostřednictvím kódu programu změnit napříč postback, například data svázán s ovládacím Web data. Při zobrazení stavu umožňuje informace, které mít na paměti, napříč postback, jeho hodnota se zvyšuje velikost značky, které musí být odeslat klientovi a může vést k tomu závažné stránky není-li byste měli pečlivě monitorovat. Ovládací prvky webového dat zejména GridView jsou zvláště notorious pro přidání desítek navíc kilobajtů značek na stránku. O zvýšení limitu může být nepatrné pro uživatele, širokopásmové připojení nebo intranetu, zobrazení stavu do doby odezvy pro uživatele telefonického připojení můžete přidat několik sekund.

Chcete-li zobrazit dopad zobrazení stavu, najdete na stránce v prohlížeči a potom zobrazte zdroj odeslaných webovou stránku (v Internet Exploreru přejděte do nabídky Zobrazit a vybrat možnost zdroje). Můžete také zapnout [trasování stránek](https://msdn.microsoft.com/library/sfbfw58f.aspx) zobrazíte přidělení zobrazení stavu jednotlivých ovládacích prvků na stránce používá. Informace o stavu zobrazení je serializována v skryté pole formuláře s názvem `__VIEWSTATE`, který je umístěn v `<div>` element ihned po otevření `<form>` značky. Zobrazení stavu uchovávána pouze po webového formuláře používá; Pokud stránku ASP.NET neobsahuje `<form runat="server">` v jeho deklarativní syntaxi nebude dostupný `__VIEWSTATE` skryté pole formuláře v vykreslované značky.

`__VIEWSTATE` Pole formuláře, které jsou generované stránky předlohy přidá přibližně 1 800 bajtů generovaných značek. Tato další tomu je kvůli především v ovládacím prvku Repeater jako obsah ovládacího prvku SiteMapDataSource jsou nastavené jako trvalé stavu zobrazení. Když navíc 1 800 bajtů nemusí jevit jako mnohem získat nadšení o, při použití GridView s mnoha pole a záznamy, stav zobrazení můžete snadno bobtnat faktorem, 10 nebo víc.

Stav zobrazení lze vypnout na úrovni stránka nebo ovládací prvek nastavením `EnableViewState` vlastnost `false`, čímž snižuje velikost vykreslované značky. Od stav zobrazení pro data webové ovládací prvek zachovává data vázaná na ovládací prvek webu dat napříč postback při zakázání ke stavu zobrazení dat ovládacího prvku webového data musí být vázána na každé zpětné volání. Ve verzi ASP.NET 1.x tuto odpovědnost klesl na pleci stránky vývojáře; s prostředím ASP.NET 2.0 ale ovládací prvky webového dat bude rebind k jejich řízení zdrojů dat na každé zpětné volání v případě potřeby.

Ke snížení stav zobrazení stránky umožňuje nastavit ovládacím prvku Repeater `EnableViewState` vlastnost `false`. To lze provést prostřednictvím okna vlastnosti v Návrháři nebo deklarativně v zobrazení zdroje. Po provedení této změny opakovače deklarativní by měl vypadat podobně jako:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Po této změně stránky pro vykreslení zobrazení, má zmenšit pouhé velikost stavu 52 bajtů, 97 % úspory v zobrazení stav velikost! V kurzech v rámci této série budete Zakážeme stav zobrazení dat ovládací prvky webového ve výchozím nastavení za účelem snížení velikosti vykreslované značky. Ve většině v příkladech `EnableViewState` vlastnost bude nastavena pro `false` a nevytvořili bez zmínky. Jenom čas, kdy zobrazení, probereme stavu je ve scénářích, kde musí být povoleno v pořadí pro data webové řízení k poskytnutí očekávané funkcí.

## <a name="step-4-adding-breadcrumb-navigation"></a>Krok 4: Přidání navigace s popisem cesty

K dokončení stránky předlohy, přidejme prvku s popisem cesty navigační uživatelského rozhraní na každou stránku. Uživatelé s popisem cesty rychle ukazuje své aktuální pozici v hierarchii této lokality. Přidání zobrazení cesty v technologii ASP.NET 2.0 je stejně snadné přidání ovládacího prvku SiteMapPath na stránku. je potřeba žádný kód.

Pro náš web přidat tento ovládací prvek záhlaví `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

S popisem cesty zobrazuje stránku aktuální uživatel je v hierarchii lokality mapy, jakož i lokality uzel mapy "nadřazených," návštěvou až do kořenové (v našem mapy webu domácí).


![Zobrazí aktuální stránky s popisem cesty a jeho předchůdců v lokalitě mapování hierarchie](master-pages-and-site-navigation-cs/_static/image28.png)

**Obrázek 12**: Zobrazí aktuální stránky s popisem cesty a jeho předchůdců v lokalitě mapování hierarchie


## <a name="step-5-adding-the-default-page-for-each-section"></a>Krok 5: Přidání výchozí stránku pro každý oddíl

Kurzy v našem webu jsou rozděleny do různých kategorií sestav základní filtrování, formátování vlastní, a tak dále se složkou pro každou kategorii a odpovídající kurzy jako stránky ASP.NET v této složce. Kromě toho obsahuje každou složku `Default.aspx` stránky. Pro tuto stránku výchozí umožňuje zobrazit všechna kurzy pro aktuální oddíl. To znamená, že pro `Default.aspx` v `BasicReporting` složky nám odkazy na `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, a `ProgrammaticParams.aspx`. Zde znovu, můžeme použít `SiteMap` třídy a data ovládací prvek webu zobrazit tyto informace na základě mapy webu definované v `Web.sitemap`.

Umožňuje zobrazit neuspořádaný seznam znovu, ale tentokrát jsme vám zobrazí název a popis kurzů k pomocí prvku Repeater. Vzhledem k tomu, že značek a kódu k provádění budou muset opakovat pro každý `Default.aspx` stránky, jsme zapouzdření tuto logiku uživatelského rozhraní [uživatelský ovládací prvek](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Vytvořte složku na webu s názvem `UserControls` a přidejte do nové položky typu uživatelský ovládací prvek webu s názvem `SectionLevelTutorialListing.ascx`a přidejte následující kód:


[![Přidat nový uživatelský ovládací prvek webu do složky UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Obrázek 13**: Přidání nového uživatele ovládacího prvku na `UserControls` složky ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

V předchozím příkladu opakovače jsme vázaný `SiteMap` data opakovače deklarativně; `SectionLevelTutorialListing` uživatelský ovládací prvek, ale nemá tak prostřednictvím kódu programu. V `Page_Load` obslužné rutiny události, je prověřit, ujistěte se, že tato stránka s URL přiřazena k uzlu v mapě webu. Pokud se používá tento uživatelský ovládací prvek na stránce, která nemá odpovídající `<siteMapNode>` položky `SiteMap.CurrentNode` vrátí `null` a žádná data budou vázána na opakovače. Za předpokladu, že máme `CurrentNode`, jsme vazby jeho `ChildNodes` kolekce opakovače. Vzhledem k tomu, že naše mapy webu je nastavený tak, aby `Default.aspx` stránka v každé části je nadřazený uzel všech kurzy v této části, tento kód zobrazí odkazy a popisy všech v části Podrobné pokyny, jak je znázorněno v následující kopie obrazovky.

Po vytvoření této opakovače, otevřete `Default.aspx` stránky v každé ze složek, přejděte do zobrazení návrhu a jednoduše přetažením uživatelského ovládacího prvku v Průzkumníku řešení na návrhovou plochu místo kurz seznamu se objeví.


[![Uživatelský ovládací prvek je přidaný do Default.aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Obrázek 14**: uživatelského ovládacího prvku má přidané do `Default.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image34.png))


[![Jsou uvedeny základní kurzů k vytváření sestav](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Obrázek 15**: základní kurzy pro generování sestav jsou uvedeny ([Kliknutím zobrazit obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Souhrn

Mapa webu, který je definovaný a dokončení stránky předlohy máme teď konzistentní stránky rozložení a navigační schématu pro naše kurzy souvisejících s daty. Bez ohledu na to, kolik stránek přidáme na náš web aktualizaci informací na webu stránky rozložení nebo lokality navigace je rychlý a jednoduchý proces z důvodu tyto informace se centralizované. Konkrétně je definována informace rozložení stránky na hlavní stránce `Site.master` a webu mapování v `Web.sitemap`. Nebyla musíme zápisu *žádné* kód k dosažení tento mechanismus rozložení a navigace stránky na webu a jsme zachovat úplné WYSIWYG podpora návrháře v sadě Visual Studio.

Po dokončení Data Access Layer a vrstvu obchodní logiky a s konzistentní stránky rozložení a lokality navigaci definované, máme připraven k započetí zkoumat obecné vzory pro generování sestav. V [Další](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tři kurzy podíváme základní úlohy vytváření sestav zobrazení data načtená z BLL v GridView, DetailsView, a FormView ovládací prvky.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přehled stránky předlohy technologie ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Stránky předlohy v technologii ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 návrh šablony](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Přehled Navigační stránka technologie ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Zkoumání ASP.NET 2.0 je navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funkce navigace technologie ASP.NET 2.0 lokality](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Principy ASP.NET zobrazení stavu](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Postupy: Povolení trasování pro stránku ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Uživatelské ovládací prvky ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Liz Shulok společnosti Dennis Patterson a Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-a-business-logic-layer-cs.md)
> [další](creating-a-data-access-layer-vb.md)
