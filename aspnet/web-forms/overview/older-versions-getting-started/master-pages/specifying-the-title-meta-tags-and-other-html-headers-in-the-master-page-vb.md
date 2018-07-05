---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Zadání názvu, metaznaček a dalších hlaviček HTML na stránce předlohy (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Vyhledá v různých postupů pro definování roztříděné &lt;head&gt; elementy na stránce předlohy ze stránky obsahu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 139fe0f67ddc067e0e23eed99569d7f6de533482
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380704"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Zadání názvu, metaznaček a dalších hlaviček HTML na stránce předlohy (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Vyhledá v různých postupů pro definování roztříděné &lt;head&gt; elementy na stránce předlohy ze stránky obsahu.


## <a name="introduction"></a>Úvod

Nové hlavní stránky vytvořené v sadě Visual Studio 2008 mají ve výchozím nastavení, dva ovládací prvky ContentPlaceHolder: jeden s názvem `head`a je umístěn v `<head>` element; a jeden s názvem `ContentPlaceHolder1`, umístěné v rámci webového formuláře. Účelem `ContentPlaceHolder1` je definování oblasti webového formuláře, kterou lze přizpůsobit na základě stránku po stránce. `head` ContentPlaceHolder umožňuje přidávat vlastní obsah stránky `<head>` oddílu. (Samozřejmě těchto dvou prvků ContentPlaceHolder může změnit ani odebrat, a další prvky ContentPlaceHolder lze přidat na stránku předlohy. Naši stránku předlohy `Site.master`, aktuálně obsahuje čtyři ovládací prvky ContentPlaceHolder.)

Kód HTML `<head>` prvek slouží jako úložiště pro informace o dokumentu webové stránky, které nejsou součástí samotného dokumentu. Jedná se o informace, jako je název webové stránky, meta informace používané vyhledávače nebo interní prohledávací moduly a odkazy na externí prostředky, jako je například kanály RSS, JavaScript a CSS soubory. Některé z těchto informací může být relevantní pro všechny stránky na webu. Můžete například chtít globálně importovat stejná pravidla šablon stylů CSS a soubory jazyka JavaScript na každou stránku ASP.NET. Existují však části `<head>` element, který je specifický pro stránku. Typickým příkladem je název stránky.

V tomto kurzu Zkoumáme, jak definovat globální a specifické pro stránku `<head>` část značky na stránce předlohy a stránky jeho obsahu.

## <a name="examining-the-master-pagesheadsection"></a>Zkoumání stránky předlohy`<head>`oddílu

Výchozí soubor předlohové stránky daného vytvořené pomocí sady Visual Studio 2008 obsahuje následující kód v jeho `<head>` části:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Všimněte si, že `<head>` obsahuje element `runat="server"` atribut, který označuje, že se jedná serverový ovládací prvek (spíše než statický kód HTML). Všechny stránky technologie ASP.NET jsou odvozeny z [ `Page` třídy](https://msdn.microsoft.com/library/system.web.ui.page.aspx), který se nachází v `System.Web.UI` oboru názvů. Tato třída obsahuje [ `Header` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) , která poskytuje přístup na stránku `<head>` oblasti. Použití `Header` vlastnost můžeme nastavit nadpis stránky technologie ASP.NET nebo přidejte další značky vygenerované `<head>` oddílu. Je pak možné úprava obsahu stránky `<head>` element napsáním hodně kódu na stránce `Page_Load` obslužné rutiny události. Zkoumáme, jak prostřednictvím kódu programu nastavit nadpis stránky v kroku 1.

Kód ukazuje `<head>` element výše také obsahuje ovládací prvek ContentPlaceHolder s názvem `head`. Tento ovládací prvek ContentPlaceHolder není nezbytné, protože obsah stránky můžete přidat vlastní obsah `<head>` element prostřednictvím kódu programu. Je užitečné, ale v situacích, kde musí stránku obsahu přidat statická značky `<head>` prvek jako statický kód lze přidat pomocí deklarace na odpovídající ovládací prvek obsahu místo prostřednictvím kódu programu.

Kromě `<title>` elementu a `head` ContentPlaceHolder, stránky předlohy `<head>` element by měl obsahovat všechny `<head>`-úrovně značky, které jsou společné pro všechny stránky. V našem webu všechny stránky pomocí pravidel šablon stylů CSS definovaných v `Styles.css` souboru. V důsledku toho jsme aktualizovali `<head>` prvek [ *vytvoření rozložení platného pro celý web pomocí stránek předlohy* ](creating-a-site-wide-layout-using-master-pages-vb.md) kurzu zahrnují odpovídající `<link>` element. Naše `Site.master` na aktuální stránku předlohy `<head>` značek je uveden níže.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Krok 1: Nastavení názvu stránky obsahu

Název webové stránky je zadáno pomocí `<title>` elementu. Je důležité nastavit název každé stránky na odpovídající hodnotu. Při návštěvě stránky, zobrazí se jeho název v záhlaví okna prohlížeče. Kromě toho při vytváření záložek na stránce, prohlížečů použít název stránky jako navrhovaný název záložky. Mnoho vyhledávací weby také zobrazit název stránky při zobrazení výsledků hledání.

> [!NOTE]
> Ve výchozím nastavení, aplikace Visual Studio nastaví `<title>` element na stránce předlohy "Bez názvu stránku". Podobně nové stránky ASP.NET mají jejich `<title>` příliš nastavena na "Stránka bez názvu". Vzhledem k tomu může být snadné zapomenout nastavit nadpis stránky na odpovídající hodnotu, existuje mnoho stránek na Internetu s názvem "Stránka bez názvu". Hledání Google pro webové stránky s tímto názvem vrátí výsledky, přibližně 2,460,000. Ani Microsoft je náchylný k publikování webových stránek s názvem "Stránka bez názvu". V době psaní tohoto návodu hledání Google hlášené 236 těchto webových stránek v doméně Microsoft.com.


Stránky technologie ASP.NET můžete zadat jeho název v jednom z následujících způsobů:

- Tak, že hodnota přímo v rámci `<title>` – element
- Použití `Title` atribut `<%@ Page %>` – direktiva
- Programové nastavení na stránce `Title` pomocí kódu jako vlastnost `Page.Title="title"` nebo `Page.Header.Title="title"`.

Obsah stránky nemají `<title>` element, protože je definován na hlavní stránce. Proto se nastavit nadpis stránky obsahu, můžete použít `<%@ Page %>` direktivy `Title` atribut nebo nastavení prostřednictvím kódu programu.

### <a name="setting-the-pages-title-declaratively"></a>Nastavení deklarativně název stránky

Název obsahu stránky můžete nastavit pomocí deklarace až `Title` atribut [ `<%@ Page %>` směrnice](https://msdn.microsoft.com/library/ydy4x04a.aspx). Tuto vlastnost lze nastavit tak, že upravíte přímo `<%@ Page %>` nebo prostřednictvím okna Vlastnosti. Podívejme se na oba přístupy.

Ze zobrazení zdroje, vyhledejte `<%@ Page %>` – direktiva, což je v horní části deklarativním označení stránky. `<%@ Page %>` Směrnice pro `Default.aspx` následující:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>` Direktiva Určuje atributy specifické pro stránku používá modul ASP.NET při analýze a kompilaci stránky. To zahrnuje jeho soubor předlohové stránky, umístění svůj soubor kódu a jeho název, mimo jiné informace.

Ve výchozím nastavení při vytváření nové stránky obsahu aplikace Visual Studio nastaví `Title` atribut "Stránka bez názvu". Změna `Default.aspx`společnosti `Title` atribut z "Stránka bez názvu" na "Stránka předlohy výukové programy" a zobrazte stránku prostřednictvím prohlížeče. Obrázek 1 ukazuje záhlaví okna prohlížeče, která odráží nový nadpis stránky.


![Záhlaví okna prohlížeče nyní zobrazuje &quot;hlavní stránky kurzy&quot; místo &quot;stránka bez názvu&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Obrázek 01**: "Hlavní stránky kurzy" místo "Bez názvu stránky" v záhlaví prohlížeče nyní nachází


Název stránky může být také nastaven v okně Vlastnosti. V okně Vlastnosti vyberte dokument z rozevíracího seznamu na vlastnosti zatížení úrovni stránky, které zahrnuje `Title` vlastnost. Obrázek 2 ukazuje v okně Vlastnosti po `Title` byla nastavena na "Kurzy stránka předlohy".


![Název v okně Vlastnosti mohou nakonfigurovat příliš](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Obrázek 02**: název v okně Vlastnosti mohou nakonfigurovat příliš


### <a name="setting-the-pages-title-programmatically"></a>Název stránky nastavení prostřednictvím kódu programu

Stránky předlohy `<head runat="server">` značek je přeložit na [ `HtmlHead` třídy](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instance modulem ASP.NET vykreslením stránky. `HtmlHead` Třída nemá [ `Title` vlastnost](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) jehož hodnota se projeví ve vygenerované `<title>` elementu. Tato vlastnost je přístupná ze stránky technologie ASP.NET použití modelu code-behind třídy přes `Page.Header.Title`; tomto stejné vlastnosti lze rovněž přistupovat prostřednictvím `Page.Title`.

Cvičení nastavení název stránky prostřednictvím kódu programu, přejděte na `About.aspx` kódu stránky třídy a vytvořit obslužnou rutinu události pro danou stránku `Load` událostí. Dále nastavte název stránky "hlavní stránky kurzy:: o:: *datum*", kde *datum* je aktuální datum. Po přidání tohoto kódu vaší `Page_Load` obslužné rutiny události by měl vypadat nějak takto:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Obrázek 3 ukazuje záhlaví okna prohlížeče při návštěvě `About.aspx` stránky.


![Název stránky je prostřednictvím kódu programu nastavit a obsahuje aktuální datum](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Obrázek 03**: Nadpis na stránku prostřednictvím kódu programu nastavit a obsahuje aktuální datum


## <a name="step-2-automatically-assigning-a-page-title"></a>Krok 2: Přiřazení automaticky nadpis stránky

Jak jsme viděli v kroku 1, je možné nastavit nadpis stránky deklarativně nebo prostřednictvím kódu programu. Pokud zapomenete explicitně změňte nadpis na výstižněji, ale vaše stránka bude mít výchozí název, "Stránka bez názvu". V ideálním případě název stránky se nastavuje automaticky pro nás v případě, že jsme explicitně nezadávejte jeho hodnotu. Pokud v době běhu je název stránky "Stránka bez názvu", budeme chtít mít název automaticky aktualizují na stejný jako název souboru stránky ASP.NET. Dobrou zprávou je, že stačí nepatrné dost práce, kterou je možné mít název automaticky přiřadí.

Všechna rozhraní ASP.NET web pages odvozovat `Page` třídy v oboru názvů System.Web.UI. `Page` Třída definuje minimální funkce vyžadované stránky technologie ASP.NET a poskytuje základní vlastnosti, jako je `IsPostBack`, `IsValid`, `Request`, a `Response`, mezi mnoha dalších. Každé stránky ve webové aplikaci často, vyžaduje další funkce ani funkcionalita. Běžný způsob poskytování to je pro vytvoření vlastní stránku základní třídy. Vlastní stránku základní třídy je třída vytvoříte, která je odvozena z `Page` třídy a obsahuje další funkce. Po vytvoření této základní třídy, může mít vaše stránky technologie ASP.NET jsou odvozeny z něj (místo `Page` třídy), a tím nabídky Rozšířené funkce pro stránky ASP.NET.

V tomto kroku vytvoříme základní stránku, která se automaticky nastaví název stránky pro stránku ASP.NET název souboru, pokud název nebyla nastavena v opačném případě explicitně. Krok 3 zjistí nastavení název stránky, na základě mapy webu.

> [!NOTE]
> Důkladné posouzení, vytvoření a použití vlastní základní třídy je nad rámec této sérii kurzů. Další informace najdete v článku [pomocí vlastní základní třída pro třídy vaše stránky technologie ASP.NET použití modelu Code-Behind](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Vytvoření základní třídy stránky

Naše první úloha je pro vytvoření třídy základní stránku, což je třída, která rozšiřuje `Page` třídy. Začněte přidáním `App_Code` složku pro váš projekt pravým tlačítkem myši na název projektu v Průzkumníku řešení, výběrem přidat složku ASP.NET a následným výběrem `App_Code`. Pak klikněte pravým tlačítkem na `App_Code` složky a přidejte novou třídu s názvem `BasePage.vb`. Průzkumník řešení po vidět na obrázku 4 `App_Code` složky a `BasePage.vb` třídy byly přidány.


![Přidat složku App_Code a třídu s názvem BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Obrázek 04**: Přidejte `App_Code` složky a třídu s názvem `BasePage`


> [!NOTE]
> Visual Studio podporuje dva režimy správy projektu: webové projekty a projekty webových aplikací. `App_Code` Složka je určená pro použití s modelem projektu webové stránky. Pokud použijete model projektu webové aplikace, umístěte `BasePage.vb` třídy ve složce pojmenovali jinak než `App_Code`, jako například `Classes`. Další informace o tomto tématu najdete [migrace webového projektu do projektu webové aplikace](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Protože vlastní základní stránky slouží jako základní třída pro třídy modelu code-behind stránek ASP.NET, je potřeba rozšířit `Page` třídy.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Pokaždé, když je požadováno stránky ASP.NET pokračuje přes řadu fází s ukončené požadovaná stránka vykreslované do kódu HTML. Můžeme tak, že přepíšete pronikněte do fází `Page` třídy `OnEvent` metody. Pro naši základní stránce teď automaticky nastavení názvu, pokud ho nebyl explicitně zadán pomocí `LoadComplete` fáze (které, jak vám může mít uhodnout, dojde poté, co `Load` fáze).

Chcete-li to provést, přepsat `OnLoadComplete` metoda a zadejte následující kód:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete` Metoda začíná ve zjištění, zda `Title` ještě nebyla nastavena vlastnost explicitně. Pokud `Title` vlastnost `Nothing`, prázdný řetězec, nebo má hodnotu "Stránka bez názvu" je přiřazena k názvu souboru požadované stránky technologie ASP.NET. Fyzická cesta k požadované stránce technologie ASP.NET – `C:\MySites\Tutorial03\Login.aspx`, je třeba - přístupné přes `Request.PhysicalPath` vlastnost. `Path.GetFileNameWithoutExtension` Metoda se používá k vytáhnout jen část názvu souboru a tento název je pak přiřazen `Page.Title` vlastnost.

> [!NOTE]
> Můžu pozvat, abyste si vylepšit tuto logiku ke zlepšení formát názvu. Například, pokud je název souboru na stránce `Company-Products.aspx`, výše uvedený kód vytvoří název "Produktů podniku", ale v ideálním případě by měl být čárka nahrazen mezerou, stejně jako v "Produktů společnosti". Zvažte také přidat mezeru pokaždé, když dojde ke změně velikosti písmen. To znamená, zvažte přidání kódu, která transformuje souboru `OurBusinessHours.aspx` a místo něj z "náš pracovní doby".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Uspořádání obsahu stránek dědit základní třídy stránky

Nyní potřebujeme aktualizovat stránky technologie ASP.NET v našem webu se odvodit z vlastní základní stránky (`BasePage`) místo `Page` třídy. K provedení přejděte ke každé třídě použití modelu code-behind a změně deklarace třídy z:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Do:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Až to uděláte, přejděte na web prostřednictvím prohlížeče. Pokud navštívíte stránku, jehož název je explicitně nastaveno, jako například `Default.aspx` nebo `About.aspx`, výslovně zadaný název se používá. Pokud však najdete na stránce, jejichž název nebylo změněno z výchozí hodnoty ("bez názvu stránka"), třída základní stránky nastaví nadpis na stránce název souboru.

Obrázek 5 ukazuje, `MultipleContentPlaceHolders.aspx` stránce při prohlížení prostřednictvím prohlížeče. Všimněte si, že název je přesně na stránce název souboru (méně rozšíření), "MultipleContentPlaceHolders".


[![Pokud název není explicitně zadán, název souboru stránky je automaticky použít](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Obrázek 05**: je-li název explicitně nezadáte, název souboru stránky je automaticky použít ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Krok 3: Vytvoření nadpis stránky na základě mapy webu

ASP.NET nabízí robustní lokality architekturu mapy, která umožňuje vývojářům definovat mapa hierarchické webu v externí prostředek (například XML soubor nebo databáze tabulku) spolu s ovládacích prvků pro zobrazení informací o mapy webu (jako je například ovládací prvky SiteMapPath, stránka Nabídky a ovládacích prvků TreeView).

Struktura mapy webu je také možné programově přistupovat ze stránky technologie ASP.NET použití modelu code-behind třídy. Tímto způsobem můžete automaticky nastavíme nadpis na stránce název jeho odpovídající uzel na mapě webu. Umožňuje zvýšit `BasePage` třídy tak, aby tato funkce nabízí vytvořili v kroku 2. Ale nejdřív je potřeba vytvořit mapy webu pro náš web.

> [!NOTE]
> Tento kurz předpokládá, že čtečky je již znáte ASP. Funkce mapy webu vaší sítě. Pro další informace o používání mapy webu najdete Moje série několika článků, [zkoumání ASP. Navigace na webu společnosti NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Vytváření mapy webu

Mapa systému lokality je vytvořeným na základě [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), který odděluje mapy webu rozhraní API od logiky, který serializuje informací o lokalitě mapování mezi pamětí a trvalé úložiště. Rozhraní .NET Framework se dodává s [ `XmlSiteMapProvider` třída](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), což je výchozí zprostředkovatele mapy webu. Jak již název napovídá, `XmlSiteMapProvider` používá soubor XML jako své úložiště mapy webu. Použijeme tohoto zprostředkovatele pro definování naše mapy webu.

Začněte vytvořením souboru mapy webu v kořenové složce webu s názvem `Web.sitemap`. Chcete-li to provést, klikněte pravým tlačítkem na název webu v Průzkumníku řešení, zvolte možnost Přidat novou položku a vyberte šablonu mapy webu. Ujistěte se, že je soubor s názvem `Web.sitemap` a klikněte na tlačítko Přidat.


[![Přidejte soubor s názvem Web.sitemap do kořenové složky webu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Obrázek 06**: Přidat soubor s názvem `Web.sitemap` do kořenové složky webu ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Přidejte následující kód XML do `Web.sitemap` souboru:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Tato konfigurace XML definuje strukturu mapy hierarchické lokality je znázorněno na obrázku 7.


![Mapa webu je aktuálně skládající se z webu mapy uzly](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Obrázek 07**: The mapy webu je aktuálně skládající se z webu mapy uzly


Budeme aktualizovat mapu struktury webu v budoucích kurzech, protože budeme přidávat nové příklady.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualizace stránky předlohy, aby zahrnovala webové ovládací prvky navigace

Teď, když máme mapy webu definované, můžeme aktualizovat hlavní stránky, aby zahrnovala webové ovládací prvky navigace. Konkrétně můžeme přidat ovládací prvek ListView do levého sloupce v sekci lekcí, který vykreslí Neseřazený seznam s položku seznamu pro každý uzel definovaný v mapě webu.

> [!NOTE]
> Ovládací prvek ListView je nová technologie ASP.NET, verze 3.5. Pokud používáte předchozí verzi technologie ASP.NET, použijte místo toho ovládacím prvku opakovače. Další informace o ovládacím prvku ListView, naleznete v tématu [pomocí technologie ASP.NET 3.5 prvku ListView a ovládací prvky prvek DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Začněte tím, že odebrání existující značky neuspořádaný seznam z část lekce. V dalším kroku přetáhněte ovládací prvek ListView z panelu nástrojů a umístěte pod lekcí záhlaví. ListView se nachází v části dat na panelu nástrojů, společně s další ovládací prvky zobrazení: ovládacího prvku GridView, DetailsView a FormView. Nastavte ListView `ID` vlastnost `LessonsList`.

Z Průvodce konfigurací zdroje dat zvolte nový ovládací prvek SiteMapDataSource s názvem vytvořit vazbu ListView `LessonsDataSource`. Ovládací prvek SiteMapDataSource vrátí hierarchickou strukturu ze systému lokality mapy.


[![Vytvoření vazby ovládacího prvku SiteMapDataSource do ovládacího prvku ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Obrázek 08**: vytvoření vazby ovládacího prvku SiteMapDataSource do ovládacího prvku ListView LessonsList ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Po vytvoření ovládacího prvku SiteMapDataSource, musíme definovat šablony prvku ListView tak, aby se vykresluje Neseřazený seznam s položku seznamu pro každý uzel vrácený SiteMapDataSource ovládacího prvku. To lze provést pomocí následující šablony značky:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate` Vygeneruje kód pro neuspořádaný seznam (`<ul>...</ul>`) zatímco `ItemTemplate` vykreslí každé položky vrácené SiteMapDataSource jako položku seznamu (`<li>`), která obsahuje odkaz na konkrétní lekce.

Po dokončení konfigurace šablony prvku ListView, přejděte na webovou stránku. Jak je vidět na obrázku 9, lekce oddíl obsahuje seznamy s odrážkami položky, domovská stránka. Kde se o a použití ovládacích prvků ContentPlaceHolder více lekce? SiteMapDataSource slouží k vrácení hierarchické sady dat, ale ovládací prvek ListView může zobrazit pouze jednu úroveň v hierarchii. V důsledku toho se zobrazí pouze první úroveň vrácené SiteMapDataSource uzly mapy webu.


[![Lekce oddíl obsahuje jednu položku seznamu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Obrázek 09**: lekce oddíl obsahuje jednu položku seznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Chcete-li zobrazit více úrovní jsme může vnořit více zobrazení v rámci `ItemTemplate`. Tato technika je zkontrolován v [ *stránky předlohy a navigace na webu* kurzu](../../data-access/introduction/master-pages-and-site-navigation-vb.md) z mé [práce s Data sérii](../../data-access/index.md). Pro tuto řadu kurzů však Mapa webu bude obsahovat pouze dvě úrovně: Home (prvek nejvyšší úrovně); a každá lekce jako podřízený objekt Domovská stránka. Místo vytváření vnořených ListView, jsme můžete místo toho dáte pokyn, aby SiteMapDataSource počáteční uzel není vrátit tak, že nastavíte její [ `ShowStartingNode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) k `False`. Výsledkem je, že SiteMapDataSource začne vrácením druhé vrstvy uzly mapy webu.

Díky této změně ListView zobrazí položky s odrážkami o a pomocí ovládacích prvků více ContentPlaceHolder lekcích, ale vynechá odrážky položky pro domácí. Chcete-li to napravit, jsme explicitně přidat položku odrážky pro domácí v `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Poznatky oddílu konfigurace SiteMapDataSource chcete vynechat, nechte počáteční uzel a explicitně přidáním položky odrážky Domovská stránka, teď zobrazuje zamýšlený výstup.


[![Lekce oddíl obsahuje položku odrážky pro domácnosti a každý podřízený uzel](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Obrázek 10**: lekce oddíl obsahuje položku odrážky pro domácnosti a každý podřízený uzel ([kliknutím ji zobrazíte obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Nastavení názvu na základě mapy webu

Pomocí mapy webu na místě, abychom mohli aktualizovat naše `BasePage` třídu použít název zadaný v mapě webu. Jako jsme to udělali v kroku 2, chceme jenom Pokud název stránky není nastavený explicitně vývojářem, použijte název uzel mapy webu. Pokud na stránce žádá nemá nastavenou explicitně název stránky a nebyl nalezen v objektu map lokality potom nám budete vrátí k používání filename požadovaná stránka (méně rozšíření), jako jsme to udělali v kroku 2. Obrázek 11 znázorňuje tento proces rozhodnutí.


![Chybí explicitně nastavit nadpis stránky se používá Title odpovídající uzel mapy webu.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Obrázek 11**: chybí explicitně nastavit nadpis stránky se používá Title odpovídající uzel mapy webu.


Aktualizace `BasePage` třídy `OnLoadComplete` tak, aby zahrnoval následující kód:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Stejně jako předtím `OnLoadComplete` metoda začíná tak, že určíte, zda byla explicitně nastavit nadpis stránky. Pokud `Page.Title` je `Nothing`, prázdný řetězec, nebo je přiřazena hodnota "Stránka bez názvu", pak kód automaticky přiřadí hodnotu k `Page.Title`.

Určit název, kód spustí odkazováním [ `SiteMap` třídy](https://msdn.microsoft.com/library/system.web.sitemap.aspx)společnosti [ `CurrentNode` vlastnost](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Vrátí [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instance v mapě webu, který odpovídá aktuálně požadovanou stránku. Za předpokladu, že aktuálně požadovanou stránku se nachází v rámci mapy webu `SiteMapNode`společnosti `Title` vlastnost je přiřazen název stránky. Pokud aktuálně požadovaná stránka není v mapě webu `CurrentNode` vrátí `Nothing` a název souboru požadovaná stránka se používá jako název (jak tomu bylo v kroku 2).

Obrázek 12 se zobrazí `MultipleContentPlaceHolders.aspx` stránce při prohlížení prostřednictvím prohlížeče. Protože název této stránky není explicitně nastavena, její odpovídající uzel mapy webu název se používá místo toho.


![Název MultipleContentPlaceHolders.aspx stránky se načítají z mapy webu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Obrázek 12**: název The MultipleContentPlaceHolders.aspx stránky se načítají z mapy webu


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Krok 4: Přidání další kód specifický pro stránku na`<head>`oddílu

Kroky 1, 2 a 3 podívali se na přizpůsobení `<title>` element na základě stránku po stránce. Kromě `<title>`, `<head>` oddíl může obsahovat `<meta>` elementy a `<link>` elementy. Jak je uvedeno výše v tomto kurzu `Site.master`společnosti `<head>` část obsahuje `<link>` elementu `Styles.css`. Protože toto `<link>` element je definována v rámci stránky předlohy, je součástí `<head>` oddílu pro všechny stránky obsahu. Ale jak bychom postupovali při přidávání `<meta>` a `<link>` prvků na základě stránku po stránce?

Nejjednodušší způsob, jak přidat obsah specifický pro stránku `<head>` části, je vytvoření ovládacího prvku ContentPlaceHolder na stránce předlohy. Už máme takové ContentPlaceHolder (s názvem `head`). Proto chcete-li přidat vlastní `<head>` značek, vytvoří odpovídající ovládací prvek na stránce obsahu a umístěte kód existuje.

Pro ilustraci přidání vlastní `<head>` značek na stránku, můžeme zahrnout `<meta>` popis – element pro naše aktuální sadu obsahu stránky. `<meta>` Element popis obsahuje stručný popis o webovou stránku; většinu vyhledávacích zahrnout tyto informace v nějaké podobě při zobrazení výsledků hledání.

A `<meta>` description element má následující formát:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Chcete-li přidat tento kód na stránku obsahu, přidejte výše uvedený text do ovládacího prvku obsahu, který se mapuje na hlavní stránku `head` ContentPlaceHolder. Například, chcete-li definovat `<meta>` popis – element pro `Default.aspx`, přidejte následující kód:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Vzhledem k tomu, `head` ContentPlaceHolder není v těle stránky HTML, poznámky přidané do ovládacího prvku obsahu se nezobrazí v zobrazení Návrh. Pokud chcete zobrazit `<meta>` najdete popis – element `Default.aspx` prostřednictvím prohlížeče. Po načtení stránky, zobrazení zdroje a Všimněte si, `<head>` část obsahuje kód zadaný v obsahu prvku.

Za chvíli přidat `<meta>` popis prvků, které mají `About.aspx`, `MultipleContentPlaceHolders.aspx`, a `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programové přidání značky`<head>`oblasti

`head` ContentPlaceHolder nám umožňuje deklarativně přidat vlastní značky k hlavní stránce `<head>` oblasti. Vlastní kód se dá přidat i prostřednictvím kódu programu. Vzpomeňte si, že `Page` třídy `Header` vrátí vlastnost `HtmlHead` instance definované na stránce předlohy ( `<head runat="server">`).

Schopnost programově přidat obsah tak, aby `<head>` oblast je užitečné, když je dynamický obsah, který chcete přidat. Možná je založen na uživatele na stránce; Možná je právě načetli z databáze. Bez ohledu na to z důvodu, můžete přidat obsah tak, aby `HtmlHead` tak, že přidáte ovládací prvky pro jeho `Controls` kolekce takto:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Ve výše uvedeném kódu přidá `<meta>` element klíčová slova `<head>` oblast, která obsahuje seznam oddělený čárkami klíčová slova, která popisují, na stránce. Všimněte si, že chcete přidat `<meta>` značku vytvoříte [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) instance, nastavte jeho `Name` a `Content` vlastnosti a pak ho přidat do `Header`společnosti `Controls` kolekce. Podobně programově přidat `<link>` elementu, vytvořit [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) objektu, nastavte jeho vlastnosti a pak ho přidat do `Header`společnosti `Controls` kolekce.

> [!NOTE]
> Přidat libovolné značky, vytvořit [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) instance, nastavte jeho `Text` vlastnost a pak ho přidat do `Header`společnosti `Controls` kolekce.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na celou řadu způsobů, jak přidat `<head>` oblast značky na základě stránku po stránce. Hlavní stránka by měla obsahovat `HtmlHead` instance (`<head runat="server">`) s ContentPlaceHolder. `HtmlHead` Instance umožňuje programově přístup k obsahu stránek `<head>` oblasti a deklarativní a programová nastavení stránky v nadpisu; ovládací prvek ContentPlaceHolder umožňuje přidat do vlastní značky `<head>` část deklarativně pomocí ovládacího prvku obsahu.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Dynamické nastavení název stránky v ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Zkoumání ASP. Navigace na webu společnosti NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Jak používat HTML metaznaček](http://searchenginewatch.com/showPage.html?page=2167931)
- [Stránky předlohy v ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Pomocí technologie ASP.NET 3.5 prvku ListView a ovládací prvky prvek DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Použití vlastní základní třída pro třídy modelu Code-Behind stránek ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Zack Jones a Suchi Banerjee. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](multiple-contentplaceholders-and-default-content-vb.md)
> [další](urls-in-master-pages-vb.md)
