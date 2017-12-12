---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: "Vytvoření webu rozložení pomocí stránky předlohy (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu se zobrazí základy stránky předlohy. Konkrétně, jaké jsou hlavní stránky, jak funguje jeden vytvořit stránku předlohy, jaké jsou držitelé obsahu místní, jak funguje jeden cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e3507acda4958fa7caa4a22fec7603deaec73c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-site-wide-layout-using-master-pages-c"></a>Vytvoření webu rozložení pomocí stránky předlohy (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> V tomto kurzu se zobrazí základy stránky předlohy. Konkrétně, jaké jsou hlavní stránky, jak jeden vytvořit stránku předlohy, jaké jsou držitelé obsahu místní, jak jeden vytvořit stránku ASP.NET, která používá stránku předlohy, jak změny hlavní stránce se projeví automaticky v jeho přidružené obsahu stránky a tak dále.


## <a name="introduction"></a>Úvod

Jeden atribut dobře navrženou webu je rozložení konzistentní stránky na webu. Například trvat www.asp.net webu. V době psaní tohoto textu každé stránce má stejný obsah v horní a dolní části stránky. Jak ukazuje obrázek 1, velmi horní části každé stránky zobrazí šedého panelu seznam Communities společnosti Microsoft. Pod tedy logo webu, seznam jazyků, do kterých byl přeložen webu a v částech základní: Home, Začínáme, další informace, stahování a tak dále. Dolní části stránky, obsahuje informace o inzerování na www.asp.net, prohlášení o autorských právech a odkaz na prohlášení o ochraně osobních údajů.


[![Web www.asp.net využívá konzistentní vzhled a chování na všech stránkách](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

**Obrázek 01**: www.asp.net webu využívá konzistentní vzhled a myslíte, že mezi všechny stránky ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))


Jiný atribut dobře navrženou lokality je snadné, pomocí kterého lze změnit vzhled daného webu. Obrázek 1 zobrazuje na domovské stránce www.asp.net z března 2008, ale mezi teď a publikace v tomto kurzu se mohl změnit vzhled a chování. Možná bude rozšiřovat položek nabídky v horní zahrnout nová část pro rozhraní MVC. Nebo možná bude unveiled radikálně nový design pro různé barvy, písma a rozložení. Při použití těchto změn celá lokalita by měla být rychlý a jednoduchý proces, který nevyžaduje úprava webové stránky, které tvoří webu tisíců.

Vytvoření šablony stránky na webu technologie ASP.NET je možné prostřednictvím *hlavní stránky*. Stručně řečeno, hlavní stránky je zvláštní druh stránku ASP.NET, která definuje značky, které jsou společné mezi všemi *obsahu stránky* a oblastí, které jsou přizpůsobitelné na základě obsahu stránce podle obsahu stránce. (Stránky obsahu je stránku ASP.NET, který je vázaný na hlavní stránku.) Vždy, když je ke změně rozložení stránky předlohy nebo formátování, všechny jeho obsahu stránky výstupu se podobně okamžitě aktualizuje, takže použití změny na webu vzhled stejně snadná jako aktualizace a nasazení jednoho souboru (konkrétně, stránky předlohy).

Toto je první kurz v řadě návodů, které prozkoumat pomocí stránky předlohy. V průběhu této série kurzu jsme:

- Zkontrolujte vytváření hlavní stránky a jejich přidružené obsahu stránky
- Popisují různé tipy, triky a depeší,
- Identifikovat běžné nástrahy stránky předlohy a prozkoumat řešení,
- Informace o tom, pro přístup k hlavní stránce ze stránky obsahu a naopak naopak,
- Zjistěte, jak určit hlavní stránku obsahu stránce za běhu, a
- Jiné rozšířené témata stránky předlohy.

Tyto kurzy jsou zaměřeny na se stručným a poskytují podrobné pokyny s dostatkem snímky obrazovky vizuálně provedou celým procesem. Každý kurzu je k dispozici v C# a Visual Basic verze a zahrnuje stahování kód dokončení použít.

V tomto kurzu zahajovací začíná podívejte se na základy stránky předlohy. Jsme popisují, jak fungují stránky předlohy, podívejte se na vytvoření stránky předlohy a přidružené stránky obsahu pomocí Visual Web Developer a zjistit, jak na hlavní stránku se okamžitě projeví v jeho obsahu stránky. Můžeme začít!

## <a name="understanding-how-master-pages-work"></a>Pochopení, jak fungují stránky předlohy

Vytváření webu s rozložení konzistentní stránky na webu vyžaduje, aby každý webové stránky emitování běžné formátování značek kromě jeho vlastní obsah. Například při každé kurzu nebo fórum odeslání na www.asp.net mít vlastní jedinečné obsah, každý z těchto stránkách také vykreslení řadu běžné `<div>` prvky, které zobrazí nejvyšší úrovně části odkazy: Home, Začínáme, přečtěte si a tak dále.

Existují různé metody pro vytváření webových stránek s konzistentní vzhled a chování. Naïve přístupem je jednoduše zkopírujte a vložte kód běžné rozložení do všech webových stránek, ale tento přístup má počet downsides. Pro začátek se pokaždé, když se vytvoří nová stránka, nezapomeňte zkopírovat a Vložit sdílený obsah na stránku. Takové kopírování a vkládání operace jsou zralé došlo k chybě jako pouze podmnožinu sdílené značek může nechtěně zkopírujte do nové stránky. A na začátek ho, tento přístup zajišťuje, nahraďte existující vzhled celý web novou problémové reálné číslo, protože každé jedné stránky webu musí upravit, aby bylo možné používat nový vzhled a chování.

Před technologii ASP.NET verze 2.0, stránka vývojáři často umístit běžné značek v [uživatelské ovládací prvky](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx) a pak přidá tyto uživatelské ovládací prvky na každé stránce. Tento přístup vyžaduje vývojář stránky nezapomeňte ručně přidat uživatelské ovládací prvky pro každou novou stránku, že povolená pro snazší úpravy celý web, protože při aktualizaci značek běžné potřeby pouze uživatelské ovládací prvky má být změněn. Visual Studio .NET 2002 a 2003 - verze sady Visual Studio, bohužel použít k vytvoření aplikace ASP.NET 1.x - uživatelské ovládací prvky v zobrazení návrhu se vykresluje jako šedá pole. V důsledku toho stránky vývojáře, kteří používají tento přístup není získejte WYSIWYG prostředí návrhu.

Nedostatků použití uživatelských ovládacích prvků byla provedena v technologii ASP.NET verze 2.0 a Visual Studio 2005 se zavedením *hlavní stránky*. Hlavní stránka je speciální typ stránky ASP.NET, která definuje kód na webu a *oblasti* kde přidružené *obsahu stránky* definovat své vlastní značky. Jak jsme uvidí v kroku 1, jsou tyto oblasti definované ContentPlaceHolder ovládací prvky. Ovládací prvek ContentPlaceHolder jednoduše označuje pozici v hierarchii řízení stránky předlohy, kde může vlastní obsah vložit podle obsahu stránky.

> [!NOTE]
> Základní koncepty a funkce stránky předlohy nezměnil od technologii ASP.NET verze 2.0. Visual Studio 2008 však nabízí návrhu podporu pro vnořené hlavní stránky, funkce, která má v sadě Visual Studio 2005. Podíváme se na použití vnořené hlavní stránky v budoucnu kurzu.


Obrázek 2 ukazuje, jak může vypadat stránky předlohy pro www.asp.net. Všimněte si, že stránky předlohy definuje běžné na webu rozložení - kód v horní, dolní a pravé každé stránky - i ContentPlaceHolder v střední levé, kde je umístěn obsah jedinečný pro každé jednotlivé webové stránky.


![Stránky předlohy definuje rozložení na webu a upravovat oblasti na základě obsahu stránce stránky obsahu](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Obrázek 02**: stránky předlohy definuje rozložení na webu a upravovat oblasti na základě obsahu stránce stránky obsahu


Po definování na hlavní stránce může být vázána na nové stránky ASP.NET pomocí značek zaškrtávací políčko. Tyto stránek ASP.NET - názvem stránky obsahu - zahrnují obsah ovládacího prvku pro každý ovládací prvky ContentPlaceHolder stránky předlohy. Při návštěvě stránky obsahu prostřednictvím prohlížeče modul ASP.NET vytvoří hlavní stránky hierarchii ovládacích prvků a vloží hierarchii ovládacích prvků obsahu stránce do na příslušná místa. Tato kombinovaná řízení hierarchie je vykreslen a výsledný HTML se vrátí do prohlížeče koncového uživatele. V důsledku toho obsahu stránce vysílá běžné značek definovaný v jeho stránky předlohy mimo obsahuje rozložení a kód specifické pro stránku definované v rámci vlastní ovládací prvky obsahu. Obrázek 3 znázorňuje tento koncept.


[![K požadované stránce značek je začleněny do stránky předlohy](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Obrázek 03**: značek požadované stránky je začleněny do stránky předlohy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))


Teď, když budeme mít popsané, jak fungují stránky předlohy, Podívejme se při vytváření stránky předlohy a přidružené stránky obsahu pomocí Visual Web Developer.

> [!NOTE]
> Aby bylo dosaženo nejširší možnou cílovou skupinou, webu ASP.NET jsme sestavení v celé řady tento kurz se vytvoří pomocí technologie ASP.NET 3.5 společnosti Microsoft bezplatnou verzi Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Pokud jste dosud nebyly upgradovány na technologii ASP.NET 3.5, nemusíte si dělat starosti – Principy probírané v těchto kurzech pracovní stejně dobře s technologií ASP.NET 2.0 a Visual Studio 2005. Ale některé ukázkové aplikace mohou používat nové funkce rozhraní .NET Framework verze 3.5; Pokud se používá funkce specifické pro 3.5, zahrnují I poznámka, která popisuje, jak implementovat podobné funkce ve verzi 2.0. Mějte na paměti, které ukázkových aplikací, které jsou k dispozici pro stáhnou z každé kurz cílové rozhraní .NET Framework verze 3.5, což vede `Web.config` soubor, který zahrnuje 3.5 konkrétní konfigurační prvky a odkazy na obory názvů specifické 3.5 v `using` příkazů v kódu třídy stránky ASP.NET. Dlouhý text short, pokud máte ještě instalace rozhraní .NET 3.5 v počítači pak ke stažení webové aplikace nebude fungovat bez první odebrání značek 3.5 specifické z `Web.config`. V tématu [Rozvěrače ASP.NET verze 3.5 pro `Web.config` souboru](http://www.4guysfromrolla.com/articles/121207-1.aspx) Další informace v tomto tématu. Budete také muset odebrat `using` příkazy, které odkazují na obory názvů specifické 3.5.


## <a name="step-1-creating-a-master-page"></a>Krok 1: Vytvoření stránky předlohy

Před jsme můžete prozkoumat vytváření a používání stránky, potřebujeme nejprve webu ASP.NET. Začněte vytvořením webu ASP.NET na základě systému nový soubor. K tomu, spusťte aplikaci Visual Web Developer a pak přejděte do nabídky soubor a vyberte nový web zobrazení dialogového okna Nový web (viz obrázek 4). Výběr šablony webu ASP.NET, nastavte rozevírací seznam umístění do systému souborů, vyberte složku pro umístění na webu a nastavení jazyka C#. Tím se vytvoří nový web s `Default.aspx` stránka ASP.NET `App_Data` složku a `Web.config` souboru.

> [!NOTE]
> Visual Studio podporuje dva režimy řízení projektu: webové projekty a projekty webových aplikací. Webové projekty chybí soubor projektu, zatímco projekty webových aplikací napodobovat architektuře projektu ve Visual Studio .NET 2002 nebo 2003 – se můžete zahrnout soubor projektu a kompilace zdrojového kódu projektu do jednoho sestavení, který je umístěn do `/bin` složka. Projekty Visual Studio 2005 původně pouze podporované webové stránky, i když [modelu projektu webové aplikace](https://msdn.microsoft.com/en-us/library/aa730880(vs.80).aspx) bylo znovu zavedeno s aktualizací Service Pack 1; Visual Studio 2008 nabízí obou modelů projektu. Visual Web Developer 2005 a edice 2008, ale podporují pouze webové projekty. Můžu použít webový projekt modelu pro moje ukázky z této série kurzu. Pokud používáte jiný Express edition a chcete místo toho použít model projektu webové aplikace, zaregistrované, můžete tak učinit, ale uvědomte si, že mohou být některé rozdíly mezi zobrazené na obrazovce a kroky, které je nutné provést porovnání snímky obrazovky ukazuje a instructio zadaná v těchto kurzech NS.


[![Vytvoření nového souboru na základě systému webového serveru](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Obrázek 04**: vytvoření webu New File System-Based ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))


V dalším kroku na hlavní stránce do lokality přidáte v kořenovém adresáři kliknutím pravým tlačítkem myši na název projektu, výběr přidat novou položku a výběrem šablony stránky předlohy. Všimněte si, že hlavní stránky končit příponou `.master`. Název této nové stránky předlohy `Site.master` a klikněte na tlačítko Přidat.


[![Přidat stránku předlohy Site.master na web s názvem](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Obrázek 05**: Přidat hlavní stránce pojmenované `Site.master` na web ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))


Přidání nového souboru hlavní stránky prostřednictvím Visual Web Developer vytvoří na hlavní stránce s následující kód:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

V prvním řádku deklarativní je [ `@Master` – direktiva](https://msdn.microsoft.com/en-us/library/ms228176.aspx). `@Master` – Direktiva je podobná [ `@Page` – direktiva](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) které se zobrazí na stránkách ASP.NET. Určuje jazyk na straně serveru (C#) a informace o umístění a dědičnost třídy kódu stránky předlohy.

`DOCTYPE` a deklarativní stránky se zobrazí pod `@Master` – direktiva. Stránka obsahuje statický HTML spolu s čtyři serverové ovládací prvky:

- **Webového formuláře ( `<form runat="server">`)** – protože všechny stránky ASP.NET obvykle mají webového formuláře - a protože stránky předlohy může zahrnovat webové ovládací prvky, které musí být v rámci webové formuláře – nezapomeňte přidat webového formuláře k vaší stránky předlohy (místo přidávání webového formuláře do e aby obsahu stránce).
- **Ovládací prvek ContentPlaceHolder s názvem `ContentPlaceHolder1`**  -tento ovládací prvek ContentPlaceHolder se zobrazí ve webovém formuláři a slouží jako oblast obsahu stránce uživatelského rozhraní.
- **Straně serveru `<head>` element** – `<head>` má element `runat="server"` atribut zpřístupnění prostřednictvím kódu na straně serveru. `<head>` Element je implementována tímto způsobem, aby nadpis stránky a dalších `<head>`-související značek může přidat nebo upravit prostřednictvím kódu programu. Například nastavení stránky ASP.NET `Title` změny vlastností `<title>` element vykreslen metodou `<head>` prvku serveru.
- **Ovládací prvek ContentPlaceHolder s názvem `head`**  -tento ovládací prvek ContentPlaceHolder se zobrazí v rámci `<head>` serveru řídit a slouží k deklarativně přidat obsah na `<head>` element.

Tato výchozí značka deklarativní stránky předlohy slouží jako počáteční bod pro návrh vlastní stránky předlohy. Nebojte se, že úpravy kódu HTML nebo přidat další ovládací prvky webového nebo ContentPlaceHolders k hlavní stránce.

> [!NOTE]
> Při navrhování stránky předlohy Ujistěte se, že stránka předlohy obsahuje webového formuláře a že alespoň jeden prvek ContentPlaceHolder se zobrazí v rámci této webového formuláře.


### <a name="creating-a-simple-site-layout"></a>Vytvoření jednoduchého lokality rozložení

Umožňuje rozšířit `Site.master`na výchozí deklarativní značek pro vytvoření rozložení webu, kde všechny stránky sdílet: běžné záhlaví; levém sloupci s navigace, novinky a další obsah na webu; a zápatí stránky, která zobrazuje ikonu "Zapnuté pomocí technologie Microsoft ASP.NET". Obrázek 6 zobrazuje konečný výsledek stránky předlohy, když jeden z jeho stránky obsahu je zobrazit pomocí prohlížeče. Red v kroužku oblast na obrázku 6 je specifická pro probíhá navštívené stránky (`Default.aspx`); jiný obsah je definovaný na hlavní stránce a je proto konzistentní přes všechny stránky obsahu.


[![Stránky předlohy definuje značky pro horní, doleva a dolní části](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Obrázek 06**: stránky předlohy definuje značky pro horní, doleva a dolní části ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))


Chcete-li dosáhnout rozložení webu vidíte na obrázku 6, začněte tím, že aktualizace `Site.master` hlavní stránky tak, aby obsahoval následující kód:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

Rozložení stránky předlohy je definován pomocí řady `<div>` elementů HTML. `topContent` `<div>` Obsahuje kód, který se zobrazí v horní části každé stránce, když `mainContent`, `leftContent`, a `footerContent` `<div>` s slouží k zobrazení obsahu stránky, v levém sloupci a "zapnuté pomocí Microsoft Ikona ASP.NET"v uvedeném pořadí. Kromě přidání těchto `<div>` elementů I taky přejmenován `ID` vlastnost prvku primární ContentPlaceHolder z `ContentPlaceHolder1` k `MainContent`.

Pravidla formátování a rozložení pro tyto roztříděné `<div>` elementy je uvedeno v aplikaci [kaskádových stylů (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) souboru `Styles.css`, která je určena prostřednictvím &lt;odkaz&gt; element v stránky předlohy &lt;head&gt; elementu. Tato různá pravidla definujte vzhled a chování jednotlivých `<div>` element uvedených výše. Například `topContent` `<div>` má element, který zobrazí text "Hlavní stránky kurzy" a odkaz, jeho formátování pravidla stanovená dokumentem `Styles.css` následujícím způsobem:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Pokud postupujete podle na váš počítač, budete muset stáhnout doprovodné kód v tomto kurzu a přidat `Styles.css` souboru do projektu. Podobně také musíte vytvořit složku s názvem bitové kopie a zkopírovat na ikonu "Zapnuté pomocí technologie Microsoft ASP.NET" z webu stažené ukázku do projektu.

> [!NOTE]
> Popis šablon stylů CSS a formátování webové stránky je nad rámec tohoto článku. Další informace o šablon stylů CSS, podívejte se [šablon stylů CSS kurzy](http://www.w3schools.com/css/default.asp) v [W3Schools.com](http://www.w3schools.com/). I taky doporučujeme, abyste stáhnout doprovodné kód v tomto kurzu- and -play se nastavení šablon stylů CSS v `Styles.css` zobrazíte důsledky různá pravidla formátování.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Vytvoření stránky předlohy pomocí stávající šablonu návrhu

Během let I sestavili počet webové aplikace ASP.NET pro malé - pro střední společnosti. Část Moje klienty měl existující rozložení lokality, které chtěli používat; ostatní pronajaty formou příslušné grafiky návrháře. Několik svěřených mi návrh rozložení webu. Jak se dá zjistit podle obrázku 6, tasking programátory návrhu rozložení tohoto webu je většinou vhodné tak, že má vaše účetní provádět open-heart surgery, zatímco vaše doctor nemá vaše daně.

Naštěstí innumerous weby, které nabízejí šablony bezplatnou návrhu HTML – Google vrátil více než půl milionů výsledky pro hledaný termín "šablony bezplatnou webu." Jeden z mých Oblíbené ty, které jsou je [OpenDesigns.org](http://opendesigns.org/). Po nalezení webu šablonu, kterou chcete přidat soubory šablon stylů CSS a bitové kopie do projektu webu a integrovat šablony HTML do vaší stránky předlohy.

> [!NOTE]
> Microsoft také nabízí řadu [bez spuštění šablony Kit aplikace ASP.NET návrhu](https://msdn.microsoft.com/en-us/asp.net/aa336613.aspx) , integrovat do dialogu Nový web v sadě Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Krok 2: Vytvoření související stránky obsahu

Se stránkou předlohy vytvořit je připraven k zahájení vytváření stránek ASP.NET, které jsou vázány na hlavní stránce. Tyto stránek se označují jako *obsahu stránky*.

Umožňuje přidat novou stránku ASP.NET do projektu a navázat jej na `Site.master` stránky předlohy. Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte možnost Přidat novou položku. Vyberte šablonu Webový formulář, zadejte název `About.aspx`a potom zaškrtněte políčko "Vyberte stránku předlohy", jak je znázorněno na obrázku 7. Díky tomu zobrazí Select stránky předlohy dialogové okno pole (viz obrázek 8) z kde si můžete vybrat stránku předlohy k použití.

> [!NOTE]
> Pokud jste vytvořili pomocí modelu projektu webové aplikace místo webový projekt modelu webu ASP.NET neuvidíte políčka "Vyberte stránku předlohy" v dialogovém okně Přidat novou položku vidět na obrázku 7. Vytvoření obsahu musí při použití projektu webové aplikace modelu zvolte šablonu webového obsahu formuláře namísto šablonu Webový formulář. Poté, co vyberete šablonu Webový formulář obsahu a kliknete na Přidat, stejné vybrat stránku předlohy, zobrazí se dialogové okno ukazuje obrázek 8.


[![Přidat nové stránky obsahu](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Obrázek 07**: Přidání nové stránky obsahu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))


[![Vyberte stránku předlohy Site.master](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Obrázek 08**: vyberte `Site.master` – stránka předlohy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))


Jak ukazuje následující deklarativní, obsahuje nové stránky obsahu `@Page` – direktiva, odkazuje zpátky na jeho hlavní stránky a ovládací prvek obsahu pro každou z ovládacích prvků ContentPlaceHolder stránky předlohy.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> V části "Vytvoření jednoduché rozložení webu" v kroku 1 přejmenované `ContentPlaceHolder1` k `MainContent`. Pokud přejmenujete není tento ovládací prvek ContentPlaceHolder `ID` stejným způsobem, budou mírně lišit obsahu stránce deklarativní od značek uvedené výše. Konkrétně, druhý obsahu ovládacího prvku `ContentPlaceHolderID` se projeví `ID` odpovídajícího prvku ContentPlaceHolder řídit ve vaší stránky předlohy.


Při vykreslování obsahu stránce, musíte modul ASP.NET pojistka stránky ovládací prvky s ovládacími prvky ContentPlaceHolder jeho hlavní stránky obsahu. Modul ASP.NET určuje stránky předlohy obsahu stránce z `@Page` – direktiva na `MasterPageFile` atribut. Jako výše uvedený kód ukazuje, je vázána této obsahu stránce `~/Site.master`.

Vzhledem k tomu, že stránka předlohy má dvou ovládacích prvků ContentPlaceHolder - `head` a `MainContent` -Visual Web Developer generované dvou ovládacích prvků obsahu. Každý ovládací prvek odkazuje na konkrétní ContentPlaceHolder přes jeho `ContentPlaceHolderID` vlastnost.

Kde je hlavní stránky nám přes předchozí techniky šablony na webu s jejich podpoře v době návrhu. Obrázek 9 ukazuje `About.aspx` obsahu stránce zobrazí prostřednictvím zobrazení návrhu Visual Web Developer. Všimněte si, že při obsah hlavní stránky se zobrazí, je zobrazena šedě a nemůže být upraven. Ovládací prvky obsahu odpovídající stránky předlohy ContentPlaceHolders se, ale upravovat. A stejně jako s jakoukoli jinou stránku ASP.NET, můžete vytvořit obsahu stránce rozhraní přidáním ovládací prvky webového prostřednictvím zobrazení zdroje nebo návrh.


[![Zobrazení návrhu obsahu stránce zobrazuje specifické pro stránky a hlavní stránky obsahu](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Obrázek 09**: obsahu návrh zobrazení zobrazí oba konkrétní stránky a stránky obsahu stránky předlohy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Přidání značky a ovládací prvky webové stránky obsahu

Za chvíli vytvořit některé obsah `About.aspx` stránky. Jak vidíte na obrázku 10, přešla záhlaví "O autorovi" a několika odstavců text, ale můžete přidat ovládací prvky webového, příliš chování. Po vytvoření tohoto rozhraní, přejděte `About.aspx` stránku prostřednictvím prohlížeče.


[![Navštívit stránku About.aspx prostřednictvím prohlížeče](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Obrázek 10**: přejděte `About.aspx` stránku prostřednictvím prohlížeči ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))


Je důležité si uvědomit, že se požadovaná stránka obsahu a jeho přidružené stránku předlohy jsou začleněny a se vykresluje jako celek zcela na webovém serveru. Prohlížeč koncového uživatele se pak odešlou výsledné, roztaveného HTML. Chcete-li to ověřit, zobrazte HTML v prohlížeči přijatých přechodem do nabídky Zobrazit a vybrat zdroj. Všimněte si, že neexistují žádné snímky nebo jiné speciální techniky pro zobrazení dva různé webové stránky v jednom okně.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Vytvoření vazby na hlavní stránce na existující stránku ASP.NET

Jak jsme viděli v tomto kroku, přidání nové stránky obsahu do webové aplikace ASP.NET je stejně snadná jako kontroluje políčka "Vyberte stránku předlohy" a výběr stránky předlohy. Bohužel převod existující stránky ASP.NET na hlavní stránku není stejně snadná.

Chcete-li vytvořit vazbu na hlavní stránce na existující stránku ASP.NET proveďte následující kroky:

1. Přidat `MasterPageFile` atribut na stránku ASP.NET `@Page` – direktiva, přejdete na příslušnou stránku předlohy.
2. Přidání ovládacích prvků obsahu pro každou z ContentPlaceHolders na hlavní stránce.
3. Selektivně kopírování a vkládání existujícího obsahu stránce ASP.NET do příslušné ovládací prvky obsahu. Říci "selektivně" zde, protože stránka technologie ASP.NET pravděpodobně obsahuje kód, který je již vyjádřená hlavní stránky, jako `DOCTYPE`, `<html>` elementu a ve webovém formuláři.

Podrobné pokyny pro tento postup, spolu se snímky obrazovky, podívejte se na [Scott Guthrie](https://weblogs.asp.net/scottgu/)na [pomocí hlavní stránky a webové navigace](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) kurzu. "Aktualizace `Default.aspx` a `DataSample.aspx` používat stránky předlohy" části Podrobnosti o těchto kroků.

Protože je mnohem snazší vytvářet nové stránky obsahu, než je převést existující stránky ASP.NET na stránky obsahu, doporučuje se, že vždy, když vytvoříte nový web ASP.NET na hlavní stránce do lokality přidat. Tato stránka předlohy svázat všechny nové stránky ASP.NET. Nemusíte si dělat starosti, pokud je velmi jednoduchý, nebo nešifrovaná; počáteční stránka předlohy hlavní stránce můžete aktualizovat později.

> [!NOTE]
> Při vytváření nové aplikace ASP.NET, přidá aplikace Visual Web Developer `Default.aspx` stránky, která není vázaná na hlavní stránku. Pokud chcete postupem převodu existující stránky ASP.NET stránky obsahu, pokračujte a tak se `Default.aspx`. Alternativně můžete odstranit `Default.aspx` a pak znovu přidat, ale tentokrát kontrola políčka "Vyberte stránku předlohy".


## <a name="step-3-updating-the-master-pages-markup"></a>Krok 3: Aktualizace značek stránky předlohy

Jedním z primární výhody stránky předlohy je, že jedna stránka předlohy může použít k definování celkové rozložení pro mnoho stránky na webu. Proto aktualizace v lokalitě vzhled a chování vyžaduje aktualizaci jednoho souboru – stránka předlohy.

Pro ilustraci toto chování, můžeme aktualizovat naše stránky předlohy zahrnout aktuální datum v horní části levém sloupci. Přidání popisku s názvem `DateDisplay` k `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Dále vytvořte `Page_Load` obslužné rutiny události pro hlavní stránky a přidejte následující kód:

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

Ve výše uvedeném kódu nastaví jmenovky `Text` vlastnost na aktuální datum a čas formátu den v týdnu, měsíce a dne letopočty názvu (viz obrázek 11). Díky této změně pokroku jeden stránek s obsahem. Jak ukazuje obrázek 11, výsledný kód je okamžitě aktualizovalo se o změnu na hlavní stránku.


[![Změny na stránku předlohy se projeví při zobrazení stránku obsahu](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Obrázek 11**: změny stránky předlohy se projeví při zobrazení stránku obsahu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))


> [!NOTE]
> Jak ukazuje tento příklad hlavní stránky mohou obsahovat webové serverové ovládací prvky, kódu a obslužné rutiny událostí.


## <a name="summary"></a>Souhrn

Stránky předlohy umožňují vývojářům ASP.NET návrh konzistentního rozložení celý web, který je možné snadno aktualizovat. Vytvoření stránky předlohy a jejich přidružené obsahu stránky je stejně jednoduché jako vytváření standardní stránek ASP.NET, jako Visual Web Developer nabízí bohatou podporu návrhu.

Stránky předlohy příkladu jsme vytvořili v tomto kurzu měl dvou ovládacích prvků ContentPlaceHolder `head` a `MainContent`. Jsme zadali pouze kód pro `MainContent` ContentPlaceHolder řízení v naší obsahu stránce, ale. V dalším kurzu se podíváme na to použitím více obsahu ovládacích prvků obsahu stránce. Můžeme také zjistit, jak definovat výchozí kód pro obsah ovládací prvky v rámci stránky předlohy, a jak k přepnutí mezi pomocí výchozího značek definovaný v hlavní stránky a poskytování vlastních značek ze stránky obsahu.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Technologie ASP.NET pro návrháře: Uvolněte návrh šablony a pokyny o vytváření webů ASP.NET pomocí webové standardy](https://msdn.microsoft.com/en-us/asp.net/aa336602.aspx)
- [Přehled stránky předlohy technologie ASP.NET](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [Kurzy kaskádové šablony stylů (CSS)](http://www.w3schools.com/css/default.asp)
- [Dynamicky nastavení nadpis stránky](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Stránky předlohy technologie ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Kurzy hlavní stránky rychlý start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Další](multiple-contentplaceholders-and-default-content-cs.md)
