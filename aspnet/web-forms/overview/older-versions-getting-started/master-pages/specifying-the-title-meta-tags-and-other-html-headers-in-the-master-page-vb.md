---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Zadáte název, značky Meta a dalších hlavičky HTML stránka předlohy (VB) | Microsoft Docs
author: rick-anderson
description: Vypadá na různých techniky pro definování roztříděné &lt;head&gt; prvky na hlavní stránce ze stránky obsahu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b8bf9d32eee3e35ffc84521f7f82f7beecc99a0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891442"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Zadáte název, značky Meta a dalších hlavičky HTML stránka předlohy (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Vypadá na různých techniky pro definování roztříděné &lt;head&gt; prvky na hlavní stránce ze stránky obsahu.


## <a name="introduction"></a>Úvod

Nové hlavní stránky vytvořené v sadě Visual Studio 2008 mají ve výchozím nastavení dvou ovládacích prvků ContentPlaceHolder: jednu s názvem `head`a je umístěn v `<head>` element; a s názvem `ContentPlaceHolder1`, umístěné v rámci webového formuláře. Účelem `ContentPlaceHolder1` je určit oblasti v webového formuláře, kterou lze přizpůsobit na základě po stránkách. `head` ContentPlaceHolder umožňuje přidat vlastní obsah na stránkách `<head>` části. (Samozřejmě tyto dvě ContentPlaceHolders můžete změnit ani odebrat, a další ContentPlaceHolder mohou být přidány do stránky předlohy. Naše stránky předlohy `Site.master`, aktuálně má čtyři ovládací prvky ContentPlaceHolder.)

HTML `<head>` element slouží jako úložiště pro informace o dokumentu webové stránky, který není součástí samotného dokumentu. To zahrnuje informace, jako je název webové stránky, meta informace používané vyhledávače nebo interní prohledávací moduly a odkazy na externí prostředky, třeba informační kanály RSS, JavaScript a CSS soubory. Některé z těchto informací může být důležité pro všechny stránky webu. Například můžete chtít globálně importovat stejná pravidla šablon stylů CSS a JavaScript soubory na každé stránce ASP.NET. Existují však části `<head>` elementu, které jsou specifické pro stránku. Typickým příkladem je název stránky.

V tomto kurzu jsme zkontrolujte jak definovat globální a specifické pro stránku `<head>` části značek na hlavní stránce a v jeho obsahu stránky.

## <a name="examining-the-master-pagesheadsection"></a>Zkoumání stránky předlohy`<head>`části

Výchozí soubor předlohové stránky vytvořené Visual Studio 2008 obsahuje následující kód v jeho `<head>` části:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Všimněte si, že `<head>` obsahuje element `runat="server"` atribut, který označuje, že je pro ovládací prvek serveru (nikoli statické HTML). Všechny stránky ASP.NET je odvozena od [ `Page` třída](https://msdn.microsoft.com/library/system.web.ui.page.aspx), který se nachází ve `System.Web.UI` oboru názvů. Tato třída obsahuje [ `Header` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) , který poskytuje přístup na stránku `<head>` oblast. Pomocí `Header` vlastnost nemůžeme nastavit název stránky ASP.NET nebo přidejte další značky vygenerované `<head>` části. Je možné, pak, chcete-li přizpůsobit stránky obsahu `<head>` element napsáním bit kódu na stránce `Page_Load` obslužné rutiny události. Jsme zkontrolujte jak programově nastavit nadpis stránky v kroku 1.

Kód ukazuje `<head>` element výše také obsahuje ovládací prvek ContentPlaceHolder s názvem `head`. Tento ovládací prvek ContentPlaceHolder není nutné, jako stránky obsahu můžete přidat vlastní obsah `<head>` element prostřednictvím kódu programu. Je užitečné, ale v situacích, kde musí stránku obsahu přidat statické značek k `<head>` element jako statické značka můžete přidat deklarativně do ovládacího prvku odpovídající obsahu místo prostřednictvím kódu programu.

Kromě `<title>` elementu a `head` ContentPlaceHolder, stránky předlohy pro `<head>` element by měl obsahovat žádné `<head>`-úrovně značky, které jsou společné pro všechny stránky. V našem webu všechny stránky použít pravidla šablon stylů CSS definované v `Styles.css` souboru. V důsledku toho byl aktualizován `<head>` element v [ *vytvoření rozložení na webu pomocí stránky předlohy* ](creating-a-site-wide-layout-using-master-pages-vb.md) kurzu zahrnout odpovídající `<link>` elementu. Naše `Site.master` je aktuální stránky předlohy `<head>` značek jsou uvedeny níže.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Krok 1: Nastavení názvu stránky obsahu

Je zadán název webové stránky prostřednictvím `<title>` elementu. Je důležité nastavit název každé stránky na odpovídající hodnotu. Při návštěvě stránky, jeho název je zobrazen v záhlaví okna prohlížeče. Kromě toho při záložky na stránce, prohlížečů použijte nadpis stránky jako navrhovaný název záložky. Navíc mnoho vyhledávací weby zobrazit název stránky při zobrazování výsledků vyhledávání.

> [!NOTE]
> Ve výchozím nastavení, Visual Studio nastaví `<title>` element na hlavní stránce "Bez názvu stránku". Podobně mají nové stránky ASP.NET jejich `<title>` příliš nastavte na "Bez názvu stránky". Vzhledem k tomu může být snadno zapomněli nastavit nadpis stránky na odpovídající hodnotu, existuje mnoho stránek na Internetu s názvem "Bez názvu stránky". Hledání Google pro webové stránky s tímto názvem vrátí zhruba 2,460,000 výsledky. I společnosti Microsoft je ohrožena útoky založenými na publikování webové stránky s názvem "Bez názvu stránky". V době psaní tohoto textu hledání Google hlášené 236 těchto webových stránek v doméně Microsoft.com.


Stránky ASP.NET můžete určit její název v jednom z následujících způsobů:

- Tím, že na hodnotu přímo v rámci `<title>` – element
- Pomocí `Title` atribut `<%@ Page %>` – direktiva
- Prostřednictvím kódu programu stránce nastavení `Title` vlastnost pomocí kódu jako `Page.Title="title"` nebo `Page.Header.Title="title"`.

Obsahu stránky nemají `<title>` elementu, protože je definována v stránky předlohy. Proto nastavit název stránky obsahu můžete použít `<%@ Page %>` – direktiva na `Title` atribut, nebo ho nastavit prostřednictvím kódu programu.

### <a name="setting-the-pages-title-declaratively"></a>Nastavení nadpisu stránky deklarativně

Název stránky obsahu můžete nastavit deklarativně pomocí `Title` atribut [ `<%@ Page %>` – direktiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Tuto vlastnost lze nastavit přímou úpravou `<%@ Page %>` nebo prostřednictvím okna Vlastnosti. Podívejme se na obou přístupů.

Ze zobrazení zdroje, vyhledejte `<%@ Page %>` – direktiva, což je v horní části stránky deklarativní značky. `<%@ Page %>` Direktivy pro `Default.aspx` následuje:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>` – Direktiva Určuje atributy specifické pro stránku používá stroj ASP.NET při analýze a kompilaci stránky. To zahrnuje její soubor předlohové stránky, jeho kód souboru a jeho název, mimo jiné informace o umístění.

Ve výchozím nastavení při vytváření nové stránky obsahu sady Visual Studio nastaví `Title` atribut "Bez názvu stránky". Změna `Default.aspx`na `Title` atribut z "Bez názvu stránky" na "Stránka předlohy výukové programy" a zobrazte stránku prostřednictvím prohlížeče. Obrázek 1 zobrazuje v prohlížeči záhlaví, která odráží nový název stránky.


![Záhlaví prohlížeče nyní zobrazuje &quot;hlavní stránky kurzy&quot; místo &quot;bez názvu stránky&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Obrázek 01**: záhlaví prohlížeče zobrazí "Hlavní stránky kurzy" namísto "Bez názvu stránky"


Název stránky může být také nastaven v okně Vlastnosti. V okně Vlastnosti vyberte dokument z rozevíracího seznamu zatížení úrovně stránky vlastností, které zahrnuje `Title` vlastnost. Obrázek 2 ukazuje okno vlastností po `Title` byla nastavena na "Kurzy stránka předlohy".


![Můžete nakonfigurovat název v okně Vlastnosti příliš](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Obrázek 02**: můžete nakonfigurovat název v okně Vlastnosti příliš


### <a name="setting-the-pages-title-programmatically"></a>Nastavení nadpisu stránky prostřednictvím kódu programu

Stránky předlohy `<head runat="server">` značek je přeložit na [ `HtmlHead` třída](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instance při vykreslení stránky ASP.NET modul. `HtmlHead` Třída má [ `Title` vlastnost](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) jejichž hodnoty se odrazí v vygenerované `<title>` elementu. Tato vlastnost je přístupná ze třídy kódu stránky ASP.NET prostřednictvím `Page.Header.Title`; tento stejné vlastnosti lze také přistupovat prostřednictvím `Page.Title`.

Chcete provádět nastavení nadpis stránky prostřednictvím kódu programu, přejděte `About.aspx` kódu stránky třídy a vytvoření obslužné rutiny události pro danou stránku `Load` událostí. Dále nastavte nadpis stránky "hlavní stránky kurzy:: o:: *datum*", kde *datum* je aktuální datum. Po přidání tohoto kódu vaší `Page_Load` obslužné rutiny události by měl vypadat podobně jako následující:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Obrázek 3 ukazuje záhlaví prohlížeče při návštěvě `About.aspx` stránky.


![Nadpis stránky programově nastavit a zahrnuje aktuální datum](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Obrázek 03**: název stránky nastavování a zahrnuje aktuální datum


## <a name="step-2-automatically-assigning-a-page-title"></a>Krok 2: Přiřazení automaticky název stránky

Jak jsme viděli v kroku 1, název stránky lze nastavit deklarativně nebo prostřednictvím kódu programu. Pokud zapomenete explicitně změnit na něco víc popisný název, ale stránka bude mít výchozí název, "Bez názvu stránky". V ideálním případě nadpis stránky se nastavuje automaticky pro nám v případě, že jsme explicitně nezadávejte jeho hodnotu. Pokud v době běhu je nadpis stránky "Bez názvu stránka", jsme chtít mít název automaticky aktualizují na být stejný jako název souboru stránky ASP.NET. Dobrá zpráva je, že s chvilku předem práce, kterou je možné, že automaticky přiřazen název.

Všechny webové stránky ASP.NET je odvozena od `Page` – třída v oboru názvů System.Web.UI. `Page` Třída definuje minimální funkce vyžadované stránku ASP.NET a zveřejňuje nezbytné vlastnosti, například `IsPostBack`, `IsValid`, `Request`, a `Response`, mezi mnohé další. Každé stránky ve webové aplikaci často, vyžaduje další funkce nebo funkce. Běžný způsob poskytování to je pro vytvoření třídy vlastní základní stránky. Třída vlastní základní stránky je vytvoříte třídu odvozenou od `Page` třídy a zahrnuje další funkce. Po vytvoření této základní třídy, může mít stránkách ASP.NET, která je odvozena z něj (místo `Page` třída), a tím nabídky Rozšířené funkce pro stránky ASP.NET.

V tomto kroku vytvoříme základní stránky, který se automaticky nastaví titulek k názvu souboru stránky ASP.NET, pokud název nebyla nastavena jinak explicitně. Krok 3 zjistí nastavení nadpis stránky na základě lokality mapy.

> [!NOTE]
> Důkladné posouzení vytváření a používání vlastní základní třídy je nad rámec tohoto kurzu řady. Další informace najdete v tématu [použití vlastní základní třídy pro třídy kódu vaše stránky ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Vytvoření základní třídy stránky

Naše první úloha je pro vytvoření třídy základní stránky, která je třída, která rozšiřuje `Page` třídy. Začněte přidáním `App_Code` složky do projektu pravým tlačítkem myši na název projektu v Průzkumníku řešení, výběr přidat složku ASP.NET a potom výběrem `App_Code`. Pak klikněte pravým tlačítkem na `App_Code` složky a přidejte novou třídu s názvem `BasePage.vb`. Obrázek 4 ukazuje Průzkumník řešení po `App_Code` složky a `BasePage.vb` třídy byly přidány.


![Přidat složku App_Code a třídy s názvem BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Obrázek 04**: Přidejte `App_Code` složku a třídy s názvem `BasePage`


> [!NOTE]
> Visual Studio podporuje dva režimy řízení projektu: webové projekty a projekty webových aplikací. `App_Code` Složky je určen k použití s modelem webový projekt. Pokud používáte model projektu webové aplikace, umístit `BasePage.vb` třídy ve složce pojmenovali jinak než `App_Code`, jako například `Classes`. Další informace v tomto tématu najdete v části [migrace webový projekt na projekt webové aplikace](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Protože vlastní základní stránky slouží jako základní třída pro třídy kódu stránky ASP.NET, je potřeba rozšířit `Page` třídy.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Vždy, když se požaduje stránku ASP.NET pokračuje prostřednictvím řady fázích, ukončeno k požadované stránce vykreslované do kódu HTML. Jsme může klepnout do fáze přepsáním `Page` třídy `OnEvent` metoda. Pro naše základní stránky umožňuje automaticky nastaví název, pokud ji nebyl zadán explicitně pomocí `LoadComplete` fázi (který, jak vám může mít uhádnout, dojde po `Load` fáze).

Chcete-li dosáhnout, přepište `OnLoadComplete` metoda a zadejte následující kód:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete` Metoda spustí na určení toho, jestli `Title` ještě nebyla nastavena vlastnost explicitně. Pokud `Title` vlastnost je `Nothing`, prázdný řetězec, nebo má hodnotu "Bez názvu stránka" je přiřazena k názvu souboru požadované stránky ASP.NET. Fyzická cesta k požadované stránce ASP.NET - `C:\MySites\Tutorial03\Login.aspx`, například - je přístupná prostřednictvím `Request.PhysicalPath` vlastnost. `Path.GetFileNameWithoutExtension` Metoda se používá k vysunout pouze část názvu souboru, a tento název souboru je poté přiřazen `Page.Title` vlastnost.

> [!NOTE]
> I pozvat vás k vylepšení této logiky ke zlepšení formát názvu. Například, pokud je název souboru stránky `Company-Products.aspx`, výše uvedený kód vytvoří název "Společnosti-produkty", ale v ideálním případě by čárka vyměnit mezerami, jako "Produkty společnosti". Zvažte také přidání mezeru vždy, když dojde ke změně případu. To znamená, zvažte přidání kód, který transformuje název souboru `OurBusinessHours.aspx` k titulu z "naše pracovní dobu".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Uspořádání obsahu stránek dědění základní třídy stránky

Nyní potřebujeme k aktualizaci stránky ASP.NET v našem webu k odvozování z vlastní základní stránky (`BasePage`) místo `Page` třídy. Provedení této, přejděte na každý třída kódu a změnit deklaraci třídy z:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Do:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Až to uděláte, přejděte na web prostřednictvím prohlížeče. Pokud navštívíte stránku, jehož název je explicitně nastavit, jako například `Default.aspx` nebo `About.aspx`, explicitně zadaný název se používá. Pokud však navštívit stránku, jejichž název nebyl změněn z výchozího ("bez názvu stránka"), nastaví třídu základní stránky title k názvu souboru stránky.

Obrázek 5 ukazuje `MultipleContentPlaceHolders.aspx` v případě, že zobrazit pomocí prohlížeče. Upozorňujeme, že název je přesněji stránky filename (méně rozšíření), "MultipleContentPlaceHolders".


[![Pokud je název nejsou explicitně zadané, název souboru stránky je automaticky použita](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Obrázek 05**: Pokud je název nejsou explicitně zadané, název souboru stránky je automaticky použijí ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Krok 3: Vytvoření název stránky na základě mapy webu

Technologie ASP.NET poskytuje robustní lokality mapy rozhraní, které umožňuje vývojářům stránky k definování mapu hierarchické webu v externí prostředek (například soubor nebo databáze Tabulka XML) společně s ovládacích prvků pro zobrazení informací o mapy webu (například SiteMapPath, Nabídky a ovládacích prvků TreeView).

Struktury mapy webu je také přístupný prostřednictvím kódu programu ze třídy kódu stránky ASP.NET. Tímto způsobem jsme v mapě webu automaticky nastaví na stránce název na název svého odpovídající uzlu. Umožňuje zvýšit `BasePage` třída vytvořili v kroku 2, takže nabízí tuto funkci. Ale je potřeba nejdřív vytvoření mapy webu pro náš web.

> [!NOTE]
> Tento kurz předpokládá, že čtečka je již obeznámeni s prostředím ASP. Funkce mapy NET na webu. Další informace o použití mapy webu, naleznete Moje řady více částech článku [zkoumání ASP. Navigace na webu na NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Vytváření mapy webu

Mapa systému lokality je vytvořené na [modelu poskytovatelů](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), který odděluje mapy webu rozhraní API z logiky, která serializuje informace mapy webu mezi paměti a trvalé úložiště. Rozhraní .NET Framework se dodává s verzí [ `XmlSiteMapProvider` třída](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), což je výchozí poskytovatel mapy webu. Jak již název napovídá, `XmlSiteMapProvider` používá soubor XML jako své úložiště mapy webu. Pojďme tohoto zprostředkovatele použijte pro definování naše mapy webu.

Začněte vytvořením souboru mapy webu v kořenové složce webu s názvem `Web.sitemap`. K tomu, klikněte pravým tlačítkem na název webu v Průzkumníku řešení, vyberte Přidat novou položku a vyberte šablonu, mapy webu. Ujistěte se, že je soubor s názvem `Web.sitemap` a klikněte na tlačítko Přidat.


[![Přidejte soubor s názvem Web.sitemap do kořenové složky webu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Obrázek 06**: Přidat soubor s názvem `Web.sitemap` do kořenové složky webu ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Přidejte následující XML tak, aby `Web.sitemap` souboru:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Tato konfigurace XML definuje strukturu mapy hierarchické lokality, který je vidět na obrázku 7.


![Mapa lokality je aktuálně skládá z tři lokality uzlů mapy](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Obrázek 07**: mapy webu je aktuálně skládá z tři lokality uzlů mapy


Struktury mapy webu bude aktualizován v budoucnu kurzech jako přidáme nové příklady.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualizace stránky předlohy zahrnout ovládací prvky pro navigaci Web

Teď, když máme mapy webu definované, můžeme aktualizovat zahrnout ovládací prvky pro navigaci webové stránky předlohy. Umožňuje přidat konkrétně ovládacím prvku ListView na levém sloupci v části lekce, který vykreslí neuspořádaný seznam s položku seznamu pro každý uzel definované v mapě webu.

> [!NOTE]
> Ovládacího prvku ListView je nová technologii ASP.NET verze 3.5. Pokud používáte předchozí verzi technologie ASP.NET, použijte místo toho ovládacím prvku opakovače. Další informace o prvku ListView najdete v tématu [pomocí technologie ASP.NET 3.5 je prvky ListView a DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Spusťte odebráním existující neuspořádaný seznam značek z části lekce. Přetáhněte ovládacím prvku ListView z panelu nástrojů a umístěte jej pod poznatky záhlaví. ListView se nachází v části datové sady nástrojů, spolu s další zobrazení ovládacích prvků: rutina GridView, DetailsView a FormView. Nastavte ListView `ID` vlastnost `LessonsList`.

Z Průvodce konfigurací zdroje dat vyberte k vytvoření vazby ListView nový ovládací prvek SiteMapDataSource s názvem `LessonsDataSource`. Ovládací prvek SiteMapDataSource vrátí hierarchická struktura ze systému lokality mapy.


[![Vytvoření vazby ovládacího prvku SiteMapDataSource do ovládacího prvku LessonsList ListView](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Obrázek 08**: vytvoření vazby ovládacího prvku SiteMapDataSource do ovládacího prvku ListView LessonsList ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Po vytvoření ovládacího prvku SiteMapDataSource, je potřeba definovat šablony ListView tak, aby vykreslením neuspořádaný seznam s položku seznamu pro každý uzel vrácený prvek SiteMapDataSource. Můžete to provést pomocí následující šablony kód:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate` Vygeneruje kód pro neuspořádaný seznam (`<ul>...</ul>`) při `ItemTemplate` vykreslí jednotlivých položek vrácených SiteMapDataSource jako položky seznamu (`<li>`) obsahující odkaz na konkrétní lekce.

Po dokončení konfigurace ListView šablony, přejděte na webovou stránku. Jak je vidět na obrázku 9, lekce oddíl obsahuje jednu položku s odrážkami, Home. Kde jsou o a pomocí ovládacích prvků více ContentPlaceHolder lekce? SiteMapDataSource slouží k vrátit hierarchickou sadu dat, ale ovládacího prvku ListView může zobrazit pouze jednu úroveň v hierarchii. V důsledku toho se zobrazí pouze první úroveň vrácený SiteMapDataSource uzlů mapy webu.


[![Lekce k oddíl obsahuje jednu položku seznamu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Obrázek 09**: lekce oddíl obsahuje jednu položku seznamu ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Chcete-li zobrazit více úrovní jsme může vnořit více ListViews v rámci `ItemTemplate`. Tato technika byl zkontrolován v [ *hlavní stránky a navigace na webu* kurzu](../../data-access/introduction/master-pages-and-site-navigation-vb.md) z mé [práce s kurz datové řady](../../data-access/index.md). Pro tento kurz řady však naše mapy webu bude obsahovat pouze dvě úrovně: Home (nejvyšší úroveň); a každý lekce jako podřízenou domovské. Místo věnujte vnořené ListView, jsme místo toho určit, aby SiteMapDataSource není vrátit počáteční uzel nastavením jeho [ `ShowStartingNode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) k `False`. Net efekt je, že SiteMapDataSource začne vrácením druhé vrstvy mapy uzlů lokality.

Díky této změně ListView zobrazí položky s odrážkami o a pomocí ovládacích prvků více ContentPlaceHolder lekci, ale vynechá položku odrážka pro domovskou. Chcete-li to opravit, jsme explicitně přidat položku odrážka pro domovskou v `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Nakonfigurováním SiteMapDataSource vynechat počáteční uzel a explicitně přidání položky odrážka Home v části lekce teď zobrazuje určený výstup.


[![Lekce k oddíl obsahuje položku odrážka pro Home a každý podřízený uzel](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Obrázek 10**: lekce oddíl obsahuje položku odrážka pro Home a každý podřízený uzel ([Kliknutím zobrazit obrázek v plné velikosti](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Nastavení názvu podle mapy webu

S mapy webu na místě, budeme aktualizovat naše `BasePage` třídu se má použít název zadaný v mapě webu. Jako jsme to udělali v kroku 2, chceme jenom použijte název uzel mapy webu, pokud Nadpis stránky není nastaveno explicitně vývojářem stránky. Pokud stránka nemá explicitně nastavit název stránky a nebyl nalezen v mapě webu, pak jsme budete vrátit k použití se požadovaná stránka filename (méně rozšíření), jako jsme to udělali v kroku 2. Obrázek 11 znázorňuje tento proces rozhodnutí.


![Chybí explicitně zadat název stránky se používá název odpovídající lokality mapy uzlu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Obrázek 11**: chybí explicitně zadat název stránky, se používá název odpovídající lokality mapy uzlu


Aktualizace `BasePage` třídy `OnLoadComplete` tak, aby zahrnoval následující kód:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Jako předtím `OnLoadComplete` metoda spustí tak, že určíte, zda byla explicitně nastavit nadpis stránky. Pokud `Page.Title` je `Nothing`, prázdný řetězec, nebo je přiřadit hodnotu "Bez názvu stránka", pak kód automaticky přiřadí hodnota `Page.Title`.

K určení názvu používat, kód spustí odkazem [ `SiteMap` třída](https://msdn.microsoft.com/library/system.web.sitemap.aspx)na [ `CurrentNode` vlastnost](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Vrátí [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instance na mapě webu, která odpovídá aktuálně požadované stránky. Za předpokladu, že se aktuálně požadovaná stránka nachází v rámci mapy webu `SiteMapNode`na `Title` vlastnost je přiřazená k titulu stránky. Pokud aktuálně požadované stránky není v mapě webu `CurrentNode` vrátí `Nothing` a název souboru k požadované stránce slouží jako název (jako tomu bylo v kroku 2).

Obrázek 12 znázorňuje `MultipleContentPlaceHolders.aspx` v případě, že zobrazit pomocí prohlížeče. Vzhledem k tomu, že název této stránky není explicitně nastavena, bude místo něj použita jeho odpovídající uzel mapy webu na název.


![Název stránky MultipleContentPlaceHolders.aspx pocházejí z mapy webu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Obrázek 12**: název MultipleContentPlaceHolders.aspx stránky pocházejí z mapy webu


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Krok 4: Přidání další značky specifické pro stránku, aby`<head>`části

Kroky 1, 2 a 3 zvážení přizpůsobení `<title>` element na základě po stránkách. Kromě `<title>`, `<head>` může obsahovat části `<meta>` elementy a `<link>` elementy. Jak jsme uvedli dříve v tomto kurzu `Site.master`na `<head>` část obsahuje `<link>` element `Styles.css`. Protože to `<link>` element je definována v rámci stránky předlohy, je součástí `<head>` části pro všechny stránky obsahu. Ale jak do jsme přejděte o přidání `<meta>` a `<link>` elementů na základě po stránkách?

Nejjednodušší způsob, jak přidat specifické pro stránku obsahu `<head>` části, je vytvoření ovládacího prvku ContentPlaceHolder na hlavní stránce. Připravili jsme takové ContentPlaceHolder (s názvem `head`). Proto přidat vlastní `<head>` značek, vytvoří odpovídající ovládací prvek na stránce obsahu a umístit značku existuje.

Pro ilustraci přidání vlastní `<head>` značek na stránku, umožňuje zahrnout `<meta>` description – element naše aktuální sadu stránky obsahu. `<meta>` Element popis obsahuje stručný popis o webovou stránku; většina vyhledávacích webů obsahovat tyto informace v některé formuláře při zobrazování výsledků vyhledávání.

A `<meta>` description – element má následující formát:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Pokud chcete přidat tento kód na stránku obsahu, přidejte výše uvedený text do ovládacího prvku obsahu, který se mapuje na hlavní stránce `head` ContentPlaceHolder. Například můžete definovat `<meta>` description – element pro `Default.aspx`, přidejte následující kód:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Protože `head` ContentPlaceHolder není v textu stránky HTML, značka přidána do ovládacího prvku obsahu se nezobrazí v zobrazení návrhu. Pokud chcete zobrazit `<meta>` najdete popis element `Default.aspx` prostřednictvím prohlížeče. Po načtení stránky zobrazení zdroje a Všimněte si, že `<head>` část obsahuje kód uvedený v obsahu ovládacího prvku.

Za chvíli přidat `<meta>` popis elementů `About.aspx`, `MultipleContentPlaceHolders.aspx`, a `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programové přidání značek k`<head>`oblast

`head` ContentPlaceHolder umožňuje nám deklarativně přidat vlastní značky k hlavní stránce `<head>` oblast. Vlastní kód lze přidat také prostřednictvím kódu programu. Odvolat, který `Page` třídy `Header` vlastnost vrátí `HtmlHead` instance na hlavní stránce definován ( `<head runat="server">`).

Schopnost programově přidat obsah `<head>` oblast je užitečné, když je dynamický obsah, který chcete přidat. Možná je založena na uživatele, kteří navštěvují stránce; Možná je právě vyžádat z databáze. Bez ohledu na důvod, proč můžete přidat obsah `HtmlHead` přidáním ovládacích prvků k jeho `Controls` kolekce takto:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Výše uvedený kód přidá `<meta>` element klíčová slova `<head>` oblast, která poskytuje čárkami oddělený seznam klíčových slov, která popisují stránky. Všimněte si, že chcete přidat `<meta>` značku vytvoříte [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) instance, nastavte jeho `Name` a `Content` vlastnosti a přidat jej do `Header`na `Controls` kolekce. Podobně programově přidat `<link>` elementu, vytvořit [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) objektu, nastavte jeho vlastnosti a přidat jej do `Header`na `Controls` kolekce.

> [!NOTE]
> Přidejte libovolné značky, vytvořte [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) instance, nastavte jeho `Text` vlastnost a přidat jej do `Header`na `Controls` kolekce.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na celou řadu způsobů, jak přidat `<head>` značek oblast na základě po stránkách. Hlavní stránka by měla obsahovat `HtmlHead` instance (`<head runat="server">`) s ContentPlaceHolder. `HtmlHead` Instance umožňuje stránky obsahu na přístup prostřednictvím kódu programu `<head>` oblasti a je title deklarativně a programově nastavení stránky, ovládací prvek ContentPlaceHolder umožňuje vlastní kód pro přidání do `<head>` část deklarativně pomocí ovládacího prvku obsahu.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Dynamicky nadpis stránky nastavení technologie ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Zkoumání ASP. Navigace na webu na NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Jak používat značky HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Stránky předlohy technologie ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Použití technologie ASP.NET 3.5 je DataPager prvky ListView a](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Použití vlastní základní třída pro třídy kódu stránky ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly Zack Petr a Suchi Banerjee. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](multiple-contentplaceholders-and-default-content-vb.md)
> [další](urls-in-master-pages-vb.md)
