---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Stránky předlohy a navigace na webu (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Jeden běžné charakteristiky uživatelsky přívětivé webů je, ke kterým mají konzistentní vzhledem k aplikacím, webu stránku rozložení a navigace schéma. V tomto kurzu snaží y...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ad86398122f3a47721bda689b7f5afc706047bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816493"
---
<a name="master-pages-and-site-navigation-c"></a>Stránky předlohy a navigace na webu (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) nebo [stahovat PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Jeden běžné charakteristiky uživatelsky přívětivé webů je, ke kterým mají konzistentní vzhledem k aplikacím, webu stránku rozložení a navigace schéma. V tomto kurzu se prohledá vytváření konzistentní vzhled a chování na všech stránkách, které je možné snadno aktualizovat.


## <a name="introduction"></a>Úvod

Jeden běžné charakteristiky uživatelsky přívětivé webů je, ke kterým mají konzistentní vzhledem k aplikacím, webu stránku rozložení a navigace schéma. Technologie ASP.NET 2.0 přináší dvě nové funkce, které výrazně usnadňují provádění oba webu stránku rozložení a navigace schéma: stránky předlohy a navigace na webu. Stránky předlohy umožňují vývojářům vytvářet šablony webu s určený upravitelnou oblastí. Pak lze použít tuto šablonu na stránky technologie ASP.NET v lokalitě. Tyto stránky ASP.NET potřebujete pouze poskytování obsahu pro stránku předlohy určené upravitelné oblasti na všech stránkách ASP.NET, které používají na hlavní stránce je stejný jako všechny ostatní značky na stránce předlohy. Tento model umožňuje vývojářům definovat a centralizovat rozložení stránky webu, a tím usnadníte tak jeho vytvoření konzistentního vzhledu a funkce na všech stránkách, které je možné snadno aktualizovat.

[Navigace systému lokality](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) poskytuje mechanismus pro stránku vývojářům definovat Mapa webu i rozhraní API pro lokalitu, která je namapována, aby se dalo dotazovat prostřednictvím kódu programu. Nové ovládací prvky webové navigace, který v nabídce, prvek TreeView a ovládací prvky SiteMapPath usnadňují vykreslování nebo její část mapy webu v běžných navigační prvek uživatelského rozhraní. Budeme používat výchozího webu navigace zprostředkovatele, to znamená, že naše Mapa webu budou určené v souboru ve formátu XML.

Ilustraci těchto konceptů a náš web kurzy dala lépe využít, věnujte této lekci definování rozložení stránky webu, implementace mapy webu a přidání navigační uživatelské rozhraní. Na konci tohoto kurzu máme uhlazené webu návrhu pro vytváření Náš kurz webových stránek.


[![Konečný výsledek tohoto kurzu](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Obrázek 1**: The End výsledek z v tomto kurzu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Krok 1: Vytvoření stránky předlohy

Prvním krokem je vytvoření hlavní stránky webu. V tuto chvíli našeho webu se skládá pouze zadané datové sady (`Northwind.xsd`v `App_Code` složky), BLL třídy (`ProductsBLL.cs`, `CategoriesBLL.cs`, a tak dále všechny in `App_Code` složky), databáze (`NORTHWND.MDF`v `App_Data` složka), konfigurační soubor (`Web.config`) a soubor šablony stylů CSS (`Styles.css`). Můžu vyčistil tyto stránky a soubory demonstrace pomocí vrstvy DAL a BLL z první dva kurzy od jsme přezkoumání těchto příkladů podrobněji v budoucích kurzech.


![Soubory v našem projektu](master-pages-and-site-navigation-cs/_static/image4.png)

**Obrázek 2**: soubory v našem projektu


Chcete-li vytvořit stránku předlohy, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. Potom ze seznamu šablon vyberte typ stránky předlohy a pojmenujte ho `Site.master`.


[![Přidejte novou stránku předlohy k webu](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Obrázek 3**: Přidání nové stránky předlohy k webu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image7.png))


Definování rozložení stránky webu tady na hlavní stránce. Můžete použít zobrazení návrhu a přidat libovolné rozložení webové ovládací prvky nebo potřebujete, nebo můžete ručně přidat značky můžete rozšířit ručně v zobrazení zdroje. Mé stránce předlohy používám [šablony stylů CSS](http://www.w3schools.com/css/default.asp) pro umístění a styly CSS nastavení definované v externím souboru `Style.css`. Zatímco nelze zjistit z kódu je uvedeno níže, se definují pravidla šablon stylů CSS tak, aby navigaci `<div>`jeho obsah je absolutně umístěné tak, aby se zobrazí na levé straně a má pevnou šířku 200 pixelů.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Hlavní stránka definuje statickou stránku rozložení a oblasti, které lze upravovat pomocí stránek ASP.NET, které používají stránky předlohy. Tyto obsahu upravitelné oblasti jsou označeny ovládací prvek ContentPlaceHolder, který lze zobrazit v rámci obsahu `<div>`. Naší hlavní stránka má jeden prvek ContentPlaceHolder (`MainContent`), ale hlavní stránka může obsahovat několik prvků ContentPlaceHolder.

Se značkami výše ukazuje přepnutí na zobrazení návrhu rozložení stránky předlohy. Všechny stránky technologie ASP.NET, které pomocí této hlavní stránky bude mít toto jednotné rozložení s možností určit značky pro `MainContent` oblasti.


[![Stránky předlohy se stránkou, při zobrazení v okně návrhu](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Obrázek 4**: stránku předlohy, při prohlížení prostřednictvím the návrhové zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Krok 2: Přidání domovské stránce na webu

S hlavní stránkou definované jsme připraveni pro přidání stránek technologie ASP.NET pro web. Začněme přidáním `Default.aspx`, domovská stránka našeho webu. Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. Vyberte si možnost webový formulář ze seznamu šablon a název souboru `Default.aspx`. Kromě toho zaškrtněte políčko "Vybrat hlavní stránku".


[![Přidat nový webový formulář, kontrolu vyberte zaškrtávací políčko hlavní stránky](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Obrázek 5**: přidejte nový webový formulář, kontrolu vyberte zaškrtávací políčko hlavní stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image13.png))


Po kliknutí na tlačítko OK, jsme se dotaz, rozhodnout, jaké stránky předlohy, používejte tuto novou stránku ASP.NET. I když můžete mít více stránek předloh ve vašem projektu, budeme mít pouze jeden.


[![Zvolte na stránce předlohy, kterou by měl použít tuto stránku ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Obrázek 6**: Zvolte na stránce předlohy použijte by měl stránky technologie ASP.NET ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image16.png))


Po výběru stránky předlohy, bude obsahovat nové stránky technologie ASP.NET následující kód:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

V `@Page` směrnice je odkaz na stránku předlohy soubor používá (`MasterPageFile="~/Site.master"`), a kódu stránky technologie ASP.NET obsahuje ovládací prvek obsahu pro všechny ovládací prvky ContentPlaceHolder definované na stránce předlohy, pomocí ovládacího prvku `ContentPlaceHolderID` mapování obsahu konkrétní ContentPlaceHolder pod kontrolou. Ovládací prvek obsahu je třeba umístit kód chcete zobrazit v odpovídajícího prvku ContentPlaceHolder. Nastavte `@Page` direktivy `Title` atribut na domovskou stránku a přidejte nějaký uvítacímu obsah do ovládacího prvku obsahu:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

`Title` Atribut `@Page` – direktiva umožňuje nastavit nadpis stránky ze stránky ASP.NET, i když `<title>` na hlavní stránce je definován prvek. Můžete také nastavíme názvu prostřednictvím kódu programu, použití `Page.Title`. Všimněte si také, že na hlavní stránce odkazy na šablony stylů (například `Style.css`) se automaticky aktualizují tak, aby fungovaly v jakékoli stránky technologie ASP.NET, bez ohledu na to, jaké adresář je stránka technologie ASP.NET v vzhledem k hlavní stránce.

Přepnout do zobrazení návrhu, že můžeme vidět, jak bude vypadat naši stránku v prohlížeči. Všimněte si, že v návrhu zobrazení pro stránku ASP.NET, že se upravovat pouze obsahu upravitelné oblasti značky bez ContentPlaceHolder definovány na hlavní stránce šedě.


[![Zobrazí upravitelné a neupravitelné oblasti návrhové zobrazení pro stránku ASP.NET](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Obrázek 7**: návrhovém zobrazení pro technologie ASP.NET stránku zobrazuje i upravit a Non-upravit oblasti ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image19.png))


Když `Default.aspx` navštívené stránky v prohlížeči, modul ASP.NET automaticky sloučí na stránce obsahu stránky předlohy a ASP. NET uživatele obsahu a vykreslí obsah sloučené do konečné kódu HTML, která bude zaslána požadujícího prohlížeče. Při aktualizaci obsahu stránky předlohy, budou mít všechny stránky technologie ASP.NET, které používají tuto stránku předlohy jejich obsah remerged pomocí nové stránky předlohy, obsahu, které se jsou požadovány. Stručně řečeno, hlavní stránky model umožňuje na jedné stránce rozložení šabloně definovaný (hlavní stránky), změny se projeví okamžitě celého webu.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Přidání stránky další technologie ASP.NET na web

Věnujte chvíli přidat další zástupné procedury stránky technologie ASP.NET na web, který bude nakonec obsahovat různé ukázky generování sestav. Bude existovat více než 35 ukázky celkem, tak místo všech stránek se zakázaným inzerováním umožňuje vytvoření pouze prvních vytvořit. Bude také se stát, že mnoho kategorií ukázky, přidejte si ukázky lepší kontrolou složky pro kategorie. Teď přidejte následující tři složky:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Nakonec přidejte nové soubory, jak je znázorněno v Průzkumníku řešení na obrázku 8. Při přidávání každého souboru, nezapomeňte zaškrtnout políčko "Vybrat hlavní stránku".


![Přidejte následující soubory](master-pages-and-site-navigation-cs/_static/image20.png)

**Obrázek 8**: přidejte následující soubory


## <a name="step-2-creating-a-site-map"></a>Krok 2: Vytvoření mapy webu

Jeden z problémů, správu webu se skládá z více než několik stránek poskytuje jednoduchý způsob, jak návštěvníci procházení webu. Než začneme musí být definován v lokalitě navigační strukturu. Tato struktura v dalším kroku musí přeložit na prvky navigaci uživatelského rozhraní, jako jsou nabídky nebo s popisem cesty. Nakonec tento celý proces je potřeba udržuje a aktualizuje při přidání nové stránky v lokalitě a odebrat existující aplikace. Před ASP.NET 2.0 byly vývojáři na své vlastní pro vytvoření webu navigační strukturu, údržbu a překládá na prvky navigaci uživatelského rozhraní. S prostředím ASP.NET 2.0 ale vývojáři můžou využívat velmi flexibilní vytvořené v systému lokality navigace.

Systém navigace na webu technologie ASP.NET 2.0 poskytuje prostředky pro vývojáře k definování mapy webu a k těmto informacím přistupovat prostřednictvím programových rozhraní API. Technologie ASP.NET se dodává s zprostředkovatele mapy webu, který očekává, že data mapy webu, který bude uložen do souboru XML ve formátu určitým způsobem. Ale vzhledem k tomu, že systém navigace na webu je postavená na [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) je možné rozšířit na alternativní způsoby podporu pro serializaci informace mapy webu. Jeff Prosise článku [The SQL lokality mapy zprostředkovatele je jsme bylo čekání pro](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) ukazuje, jak vytvořit zprostředkovatele mapy webu, který ukládá mapy webu v databázi serveru SQL Server; Další možností je vytvořit [na základě zprostředkovatele mapy webu strukturu systémů souborů](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

V tomto kurzu ale použijeme výchozího zprostředkovatele mapy webu, která je dodávána s prostředím ASP.NET 2.0. K vytvoření mapy webu, jednoduše klikněte pravým tlačítkem na název projektu v Průzkumníku řešení zvolte Přidat novou položku a zvolte možnost mapy webu. Nechte název tak jako `Web.sitemap` a klikněte na tlačítko Přidat.


[![Přidejte do projektu mapy webu](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Obrázek 9**: Přidání mapy webu do vašeho projektu ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image23.png))


Soubor mapy webu je soubor XML. Všimněte si, že Visual Studio poskytuje IntelliSense pro strukturu mapy webu. Soubor mapy webu musí mít `<siteMap>` jako jeho kořenový uzel, který musí obsahovat přesně jeden uzel `<siteMapNode>` podřízený element. Nejdříve `<siteMapNode>` element může obsahovat libovolný počet potomka `<siteMapNode>` elementy.

Definujte Mapa webu tak, aby napodoboval strukturu systému souborů. To znamená, přidejte `<siteMapNode>` – element pro všechny tři složky a podřízených `<siteMapNode>` elementy pro jednotlivé stránky technologie ASP.NET v těchto složek, například takto:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Mapa webu definuje webu navigační strukturu, která je hierarchie, která popisuje různé části webu. Každý `<siteMapNode>` prvek `Web.sitemap` představuje oddíl v navigační struktuře lokality.


[![Mapa webu představuje hierarchickou strukturu navigační](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Obrázek 10**: mapy webu představuje hierarchickou strukturu navigační ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image26.png))


Technologie ASP.NET poskytuje strukturu mapy webu prostřednictvím rozhraní .NET Framework [Třída SiteMap](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Tato třída obsahuje `CurrentNode` vlastnost, která vrátí informace o části návštěvy aktuálně; `RootNode` vlastnost vrátí kořen mapy webu (v našem mapy webu Home). Jak `CurrentNode` a `RootNode` návratové vlastnosti [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instancemi, jejichž vlastnosti jako `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`, a tak dále, která umožňují mapy webu hierarchie na vás.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Krok 3: Zobrazení nabídky na základě mapy webu

Přístup k datům v technologii ASP.NET 2.0 lze provést prostřednictvím kódu programu, jako jsou v technologii ASP.NET 1.x, nebo pomocí deklarace, prostřednictvím nového [ovládací prvky zdroje dat](https://msdn.microsoft.com/library/ms227679.aspx). Existuje několik předdefinovaných ovládací prvky zdroje dat jako je například ovládacím prvkem SqlDataSource pro přístup k datům relační databáze, ovládací prvek ObjectDataSource, pro přístup k datům z třídy a další. Můžete dokonce vytvořit svoje vlastní [ovládací prvky zdroje dat vlastní](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Ovládací prvky zdroje dat slouží jako proxy mezi vaší stránky technologie ASP.NET a příslušná data. Aby bylo možné zobrazit ovládací prvek zdroje dat načtená data, jsme budete obvykle přidejte další ovládací prvek webové stránky a svázat ovládací prvek zdroje dat. K vytvoření vazby ovládacího prvku webové k ovládacímu prvku zdroje dat, stačí nastavit ovládací prvek webového `DataSourceID` k hodnotě ovládacího prvku zdroje dat `ID` vlastnost.

Na podporu při práci s daty mapy webu technologie ASP.NET obsahuje ovládací prvek SiteMapDataSource, který umožňuje vytvoření vazby ovládacího prvku webové proti mapy webu našeho webu. Dvou ovládacích prvků TreeView a nabídky se běžně používají k zajištění navigační uživatelské rozhraní. Svázat data mapy webu na jednu z těchto dvou ovládacích prvků, jednoduše přidat SiteMapDataSource na stránku spolu s ovládací prvek TreeView nebo nabídky ovládací prvek, jehož `DataSourceID` je nastavena odpovídajícím způsobem. Ovládací prvek nabídky můžeme přidat například na hlavní stránku pomocí následující kód:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Pro jemnější stupeň kontroly nad emitovaný kód HTML, jsme lze svázat ovládací prvek SiteMapDataSource ovládacím prvku opakovače takto:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Ovládací prvek SiteMapDataSource vrátí úroveň hierarchie jeden mapy webu současně, počínaje kořenový uzel mapy webu (domů, v našem mapy webu), klikněte na další úroveň (základní tvorbou sestav, filtrování sestavy a přizpůsobit formátování) a tak dále. Při vytváření vazby SiteMapDataSource na Repeateru, zobrazí první úroveň vrátí a vytvoří `ItemTemplate` pro každou `SiteMapNode` instance v této první úroveň. Pro přístup k určité vlastnosti `SiteMapNode`, můžeme použít `Eval(propertyName)`, což je, jak každý získáme `SiteMapNode`společnosti `Url` a `Title` vlastnosti ovládacího prvku hypertextový odkaz.

Výše uvedený příklad Repeater bude vykreslen:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Tyto uzly mapy webu (základní tvorbou sestav, filtrování sestavy a přizpůsobit formátování) tvoří *druhý* úrovně vykreslované, ne v prvním mapy webu. Je to proto, SiteMapDataSource `ShowStartingNode` je nastavena na hodnotu False, způsobí SiteMapDataSource obejít kořenový uzel mapy webu a místo toho začít tak, že vrací druhou úroveň v hierarchii mapy webu.

K zobrazení podřízených základní tvorbou sestav, filtrování sestav, a přizpůsobit formátování `SiteMapNode` s, přidáme jiný Repeater na počáteční Repeater `ItemTemplate`. Tento druhý Repeater bude vázán k `SiteMapNode` instance `ChildNodes` vlastnosti, například takto:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Tyto dvě opakovače za následek následující značky (některé značek se odebrala pro zkrácení):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Pomocí šablon stylů CSS styly zvolené z [Rachel Andrew](http://www.rachelandrew.co.uk/)uživatele rezervuje [The Anthology šablon stylů CSS: 101 důležité tipy, triky, &amp; změní](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` a `<li>` jsou ve stylu prvků tak, aby kód vytvoří následující výstup visual:


![Zobrazí se nabídka skládá ze dvou opakovače a některé šablony stylů CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Obrázek 11**: nabídky se skládá ze dvou opakovače a některé šablony stylů CSS


Tato nabídka je na hlavní stránce a vázaný k Mapa webu, který je definován v `Web.sitemap`, to znamená, že všechny změny do mapy webu se okamžitě projeví na všech stránek, které používají `Site.master` stránky předlohy.

## <a name="disabling-viewstate"></a>Zakazuje stav zobrazení

Všechny ovládací prvky technologie ASP.NET můžete volitelně uchovávat jejich stav [zobrazit stav](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), který serializuje jako skryté pole formuláře v zobrazený HTML. Stav zobrazení používá ovládací prvky pamatovat jejich programově změnit stav postbacků, například že data svázaná s ovládacím prvkem webových dat. Při zobrazení stavu umožňuje informace, které se uloží, postbacků, jeho hodnota se zvyšuje velikost značky, které se musí odeslat na klienta a může vést k jejímu narůstání závažné stránky, pokud nebude pečlivě monitorovat. Webové ovládací prvky dat zejména GridView jsou zvláště notorious pro přidání desítky navíc kilobajtů značek na stránku. Takové zvýšení může být nepatrné pro uživatele širokopásmového připojení nebo intranetu, zobrazení stavu do odezvy pro telefonického uživatele můžete přidat několik sekund.

Chcete-li zobrazit dopadu zobrazit stav, navštivte stránku v prohlížeči a zobrazte zdroj zaslaná z webové stránky (v Internet Exploreru přejděte do zobrazení nabídky a zvolte možnost zdroje). Můžete také zapnout [trasování stránek](https://msdn.microsoft.com/library/sfbfw58f.aspx) zobrazíte zobrazit stav přidělení používaný jednotlivými ovládacích prvků na stránce. Informace o stavu zobrazení je serializován ve skryté pole formuláře s názvem `__VIEWSTATE`, který je umístěn v `<div>` ihned za úvodní prvek `<form>` značky. Stav zobrazení je trvalý pouze když je webový formulář používá; Pokud vaše stránka technologie ASP.NET není `<form runat="server">` v jeho deklarativní syntaxe, nebude existovat `__VIEWSTATE` skrytého pole formuláře vykreslované značky.

`__VIEWSTATE` Pole formuláře, které jsou generované na hlavní stránce přidá přibližně 1 800 bajtů do generovaného kódu stránky. Tato další vyhledávání je třeba primárně na ovládacím prvku opakovače jako obsah ovládacího prvku SiteMapDataSource jsou zachované v zobrazení stavu. Při další 1 800 bajtů nemusí vypadat podobně jako v podstatě zažijte to nadšení, při použití GridView s mnoha záznamy a pole, můžete zobrazit stav snadno bobtnat faktorem 10 nebo více.

Stav zobrazení je zakázat na úrovni stránky nebo ovládacího prvku tak, že nastavíte `EnableViewState` vlastnost `false`, a tím snížit velikost vykreslované značky. Od stavu zobrazení dat webové ovládací prvek zachovává data vázaná na data webový ovládací prvek postbacků při zakázání stavu zobrazení dat ovládacího prvku webového data musí být vázán na každého zpětného odeslání. Ve verzi ASP.NET 1.x tuto odpovědnost klesl se na ramena vývojář; s prostředím ASP.NET 2.0 ale webových ovládacích prvcích dat se znovu připojit k jejich ovládací prvek zdroje dat při každém postbacku v případě potřeby.

Ke snížení stav zobrazení stránky umožňuje nastavit ovládacím prvku opakovače `EnableViewState` vlastnost `false`. To můžete udělat v okně Vlastnosti v Návrháři nebo deklarativně v zobrazení zdroje. Po provedení této změny by měl vypadat Repeater deklarativní:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Po této změně stránky pro vykreslení zobrazení, že má k pouhé zmenšit velikost stavu 52 bajtů, 97 % úspory v zobrazení stav velikost! V kurzech v celé této sérii budete Zakážeme stav zobrazení dat webové ovládací prvky ve výchozím nastavení za účelem snížení velikosti vykreslované značky. Ve většině z příkladů `EnableViewState` vlastnost bude nastavena na `false` a to všechno bez pozornost. Pouze čas zobrazení probereme stavu je ve scénářích, kde musí být povolená v pořadí dat webové řídit jeho očekávaný nakonfigurovánu.

## <a name="step-4-adding-breadcrumb-navigation"></a>Krok 4: Přidání navigace s popisem cesty

K dokončení stránky předlohy, přidáme prvek uživatelského rozhraní navigace s popisem cesty na každou stránku. Tento navigační prvek určuje rychle zobrazuje uživatele své aktuální pozici v rámci hierarchie lokality. Přidání navigace s popisem cesty v technologii ASP.NET 2.0 je stejně jednoduché přidat ovládací prvky SiteMapPath ovládacího prvku na stránku. je potřeba žádný kód.

Pro náš web, přidejte tento ovládací prvek záhlaví `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Tento navigační prvek určuje ukazuje aktuální stránku návštěvníků uživatele v hierarchii mapy webu a v dané lokalitě uzel mapy "předchůdce," úplně až po kořen (v našem mapy webu Home).


![Zobrazí tento navigační prvek určuje aktuální stránku a namapujte jeho předchůdců v lokalitě hierarchie](master-pages-and-site-navigation-cs/_static/image28.png)

**Obrázek 12**: zobrazí tento navigační prvek určuje aktuální stránku a namapujte jeho předchůdců v lokalitě hierarchie


## <a name="step-5-adding-the-default-page-for-each-section"></a>Krok 5: Přidání výchozí stránky pro každý oddíl

Kurzy v našem webu jsou rozdělené do různých kategorií základní tvorbou sestav, filtrování, vlastní formátování, se složkou pro každou kategorii a odpovídající kurzy jako stránky technologie ASP.NET v rámci dané složky a tak dále. Kromě toho obsahuje každou složku `Default.aspx` stránky. Pro tuto výchozí stránku můžeme zobrazit všechny kurzy pro aktuální oddíl. To znamená pro `Default.aspx` v `BasicReporting` složky měli jsme odkazy na `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, a `ProgrammaticParams.aspx`. Tady, znovu, můžete použít `SiteMap` třídy a data webový ovládací prvek pro zobrazení těchto informací na základě mapy webu definované v `Web.sitemap`.

Umožňuje zobrazit neuspořádaný seznam pomocí Repeateru znovu, tentokrát ale jsme vám zobrazit nadpis a popis v kurzech. Protože značek a kódu k dosažení tohoto musí zopakovat pro každý `Default.aspx` stránky, budeme zapouzdření tuto logiku uživatelského rozhraní [uživatelský ovládací prvek](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Vytvořte složku na webu volá `UserControls` a přidejte do nové položky typu webový uživatelský ovládací prvek s názvem `SectionLevelTutorialListing.ascx`a přidejte následující kód:


[![Přidat nový uživatelský ovládací prvek webu do složky UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Obrázek 13**: přidejte nový ovládací prvek uživatelské Web k `UserControls` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

V předchozím příkladu Repeater jsme vázán `SiteMap` data Opakovači deklarativně; `SectionLevelTutorialListing` uživatelského ovládacího prvku, ale provádí se prostřednictvím kódu programu. V `Page_Load` obslužná rutina události, a zkontrolujte, že tato stránka s URL mapuje na uzel na mapě webu se provede kontrola. Pokud použijete tento uživatelský ovládací prvek na stránce, který nemá odpovídající `<siteMapNode>` položky `SiteMap.CurrentNode` vrátí `null` a žádná data bude vázán k opakovače. Za předpokladu, že máme `CurrentNode`, můžeme vytvořit vazbu jeho `ChildNodes` kolekce opakovače. Vzhledem k tomu, že naše mapy webu je nastavený tak, aby `Default.aspx` stránka v každé části je nadřazený uzel všechny kurzy v této části, tento kód zobrazí odkazy a popisy všech kurzů v části, jak je znázorněno v níže uvedeném snímku obrazovky.

Po vytvoření tohoto Repeater, otevřete `Default.aspx` stránky ve všech složek, přejděte do zobrazení návrhu a jednoduše přetáhněte uživatelského ovládacího prvku na návrhovou plochu v Průzkumníkovi řešení ve kterém chcete kurz seznamu se zobrazí.


[![Má uživatelský ovládací prvek byl přidán na stránku Default.aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Obrázek 14**: uživatelského ovládacího prvku má byla přidána do `Default.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image34.png))


[![Jsou uvedeny základní kurzy vytváření sestav](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Obrázek 15**: kurzy základní vytváření sestav jsou uvedené ([kliknutím ji zobrazíte obrázek v plné velikosti](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Souhrn

Mapa webu, který je definován a kompletní stránky předlohy teď máme konzistentní stránku rozložení a navigace schéma pro naše kurzy související s daty. Bez ohledu na to, kolik stránek se nám přidat na náš web aktualizace stránky webu informace navigace rozložení nebo webu je rychlý a jednoduchý proces z důvodu tyto informace se centralizované. Konkrétně informace o rozložení stránky je definován na hlavní stránce `Site.master` a lokality si namapují `Web.sitemap`. Neměli musíme napsat *jakékoli* kódu k dosažení tohoto mechanismu rozložení a navigace stránky webu a budeme uchovávat úplné WYSIWYG podpora návrháře v sadě Visual Studio.

Po dokončení vrstvy přístupu k datům a vrstvu obchodní logiky a potíže konzistentní stránku rozložení a navigace na webu definované, jsme připraveni začít s průzkumem běžných vzorů pro vytváření sestav. V [Další](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tří kurzů podíváme na základní úlohy vytváření sestav zobrazení dat načtených z knihoven BLL v prvku GridView, DetailsView, a řídí FormView.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přehled ASP.NET hlavní stránky](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Stránky předlohy v technologii ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [Šablony ASP.NET 2.0 návrhu](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Přehled navigace webu ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Zkoumání ASP.NET 2.0 je navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funkce navigace 2.0 webu technologie ASP.NET](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Principy stav zobrazení ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Postupy: Povolení trasování pro stránku ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Uživatelské ovládací prvky ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Liz Shulok, Dennis Patterson a Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-a-business-logic-layer-cs.md)
> [další](creating-a-data-access-layer-vb.md)
