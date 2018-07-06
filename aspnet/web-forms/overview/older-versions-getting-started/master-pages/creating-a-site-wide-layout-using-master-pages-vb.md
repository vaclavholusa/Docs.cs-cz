---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Vytvoření rozložení platného pro celý web pomocí stránek předlohy (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Tomto kurzu se dozvíte základní informace o stránku předlohy. A to, co jsou hlavní stránky, jak funguje jeden vytvořit stránku předlohy, jaké jsou obsahu místo zástupné znaky, jak funguje jeden cr...
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 182f45c28dc37633b429fead333d401818299e36
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827819"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Vytvoření rozložení platného pro celý web pomocí stránek předlohy (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Tomto kurzu se dozvíte základní informace o stránku předlohy. A to, co jsou hlavní stránky, jak jeden vytvořit stránku předlohy, jaké jsou obsahu místo zástupné znaky, jak jeden vytvořit stránky ASP.NET, která používá stránku předlohy tak, jak změny stránky předlohy se automaticky projeví v jeho přidružené obsahu stránky a tak dále.


## <a name="introduction"></a>Úvod

Jeden atribut dobře navržené webové stránky je rozložení konzistentní stránky webu. Vezměme si jako příklad www.asp.net webu. V době psaní tohoto návodu každé stránky má stejný obsah v horní a dolní části stránky. Jak ukazuje obrázek 1 zobrazuje velmi horní části každé stránky šedého panelu se seznamem Communities společnosti Microsoft. Pod, který je logo společnosti, seznam jazyků, do kterých byl přeložen webu a v částech core: Home, Začínáme, přečtěte si víc, soubory ke stažení a tak dále. Dolní části stránky, obsahuje informace o reklamy na www.asp.net, prohlášení o autorských právech a odkaz na prohlášení o ochraně osobních údajů.


[![Na webu www.asp.net využívá konzistentní vzhled a chování na všech stránkách](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Obrázek 01</strong>: www.asp.net web využívá konzistentní vzhled a pocit, že mezi všechny stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Jiný atribut dobře navržené lokality je snadné, pomocí kterého lze změnit vzhled tohoto webu. Obrázek 1 ukazuje na domovskou stránku www.asp.net od března 2008, ale a publikování v tomto kurzu, mohl změnit vzhled a chování. Možná se rozbalí položky nabídky podél horního zahrnout nová část pro rozhraní MVC. Nebo možná bude unveiled radikálně nový design pro různé barvy, písma a rozložení. Při použití těchto změn celá lokalita by měla být rychlý a jednoduchý proces, který nevyžaduje úpravy tisíce webové stránky, které tvoří webu.

Vytvoření šablony webu stránky v technologii ASP.NET je možné prostřednictvím *stránky předlohy*. Řečeno v kostce, hlavní stránka je speciální typ stránky ASP.NET, která definuje značky, které jsou společné mezi všemi *stránky obsahu* a oblasti, které můžete přizpůsobit na základě obsahu stránky obsahu stránky. (Stránka obsahu je stránky ASP.NET, která je vázána na hlavní stránku.) Pokaždé, když se změní rozložení stránky předlohy nebo formátování, všechny její obsah stránky výstupu podobně okamžitě aktualizován, takže se dají aplikovat změny webu vzhled stejně snadné jako aktualizace a nasazení jednoho souboru (konkrétně, hlavní stránky).

Toto je první kurz série kurzů, které se zabývají pomocí stránek předlohy. V průběhu této sérii kurzů jsme:

- Zkontrolujte vytvoření hlavní stránky a jejich přidružené stránky obsahu
- Popisují širokou škálu tipů, triků a depeší,
- Identifikovat běžné nástrahy stránky předlohy a prozkoumat řešení
- Informace o tom, pro přístup k hlavní stránce z obsahu stránky a naopak,
- Zjistěte, jak zadat stránku obsahu stránky předlohy v době běhu a
- Další rozšířené témata stránky předlohy.

Tyto kurzy jsou zaměřené na být stručné a poskytují podrobné pokyny s dostatečným snímky obrazovky, který vás provede procesem vizuálně. Každý kurz je dostupná v C# a Visual Basic verze a nabízí ke stažení kompletní kód používá.

V tomto kurzu zahajovací začíná podívejte se na stránku předlohy základy. Jsme popisují, jak fungují stránky předlohy, podívejte se na vytváření stránky předlohy a přidružené obsahu stránky pomocí Visual Web Developer a naleznete v tématu jak na stránku předlohy se okamžitě projeví v jeho obsahu stránky. Pusťme se do práce!

## <a name="understanding-how-master-pages-work"></a>Vysvětlení, jak fungují stránky předlohy

Vytvoření webu s rozložení konzistentní stránky webu vyžaduje, aby každé webové stránky generování běžných formátování kódu kromě jeho vlastní obsah. Například i když každý kurz nebo fórum příspěvek na www.asp.net má svůj vlastní jedinečný obsah, každá z těchto stránek také vykreslit řadu běžných `<div>` prvky, které zobrazují odkazy oddílů nejvyšší úrovně: Home, Začínáme, přečtěte si víc a tak dále.

Existuje řada různých technik vytváření webových stránek s konzistentní vzhled a chování. Naivní přístupem je jednoduše zkopírujte a vložte společné značky rozložení do všech webových stránek, ale tento přístup má několik nevýhody. Pokud začínáte pokaždé, když se vytvoří nová stránka, nezapomeňte zkopírovat a vložit sdíleného obsahu do stránky. Takové zkopírováním a vložením operace jsou zralé chyby pouze podmnožinu sdílenému kódu může nechtěně zkopírujte do nové stránky. A na začátek ho, tento přístup zajišťuje, nahradíte stávající vzhled webu nový reálné problémy vzhledem k tomu, aby bylo možné používat nový vzhled a chování nutné upravit každé jedné stránky na webu.

Před ASP.NET verze 2.0, stránce vývojáři často umístit společné značky v [uživatelské ovládací prvky](https://msdn.microsoft.com/library/y6wb1a0e.aspx) a pak přidá tyto uživatelské ovládací prvky pro každou jednotlivou stránku. Tento přístup vyžaduje, že vývojář stránky nezapomeňte ručně přidat uživatelské ovládací prvky pro každou novou stránku, ale pro snazší změny webu nepovoluje, protože při aktualizaci společné značky potřeba upravit pouze uživatelské ovládací prvky. Bohužel Visual Studio .NET 2002 a 2003 – verze sady Visual Studio použít k vytvoření aplikace ASP.NET 1.x – uživatelské ovládací prvky v návrhovém zobrazení se vykresluje jako šedá pole. V důsledku toho stránky vývojářům, kteří používají tento přístup není využijte WYSIWYG návrhové prostředí.

Nedostatky používání uživatelských ovládacích prvků se zákazníky a vyřešené v technologii ASP.NET verze 2.0 a Visual Studio 2005 se zavedením *stránky předlohy*. Hlavní stránka je speciální typ stránky ASP.NET, která definuje značky webu a *oblastech* tam, kde se spojená *stránky obsahu* definovat své vlastní značky. Jak vidíte v kroku 1, tyto oblasti jsou definovány ovládací prvky ContentPlaceHolder. Ovládací prvek ContentPlaceHolder jednoduše označuje pozici v hierarchii ovládacích prvků stránky předlohy, kde můžete vloženy vlastní obsah stránky obsahu.

> [!NOTE]
> Základní koncepty a funkce stránek předlohy nebyl změněn od ASP.NET verze 2.0. Visual Studio 2008 však nabízí podporu návrhu pro vložené hlavní stránky, funkce, která chyběla v sadě Visual Studio 2005. Podíváme se na použití vložené hlavní stránky v budoucích kurzech.


Obrázek 2 ukazuje, jak může vypadat na hlavní stránce pro www.asp.net. Všimněte si, že na hlavní stránce definuje společné rozložení pro celý web - kód v horní, dolní a pravé části každé stránky – a také prvku ContentPlaceHolder na levý střed, kde je umístěn jedinečné obsah pro každou jednotlivou stránku webové.


![Stránka předlohy definuje rozložení pro celý web a upravitelnou oblastí na základě obsahu stránky obsahu stránky](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Obrázek 02**: hlavní stránku definuje rozložení pro celý web a upravitelnou oblastí na základě obsahu stránky obsahu stránky


Po definování hlavní stránky může být vázána na nové stránky ASP.NET prostřednictvím značek zaškrtávací políčko. Tyto stránky ASP.NET – volána stránky obsahu – obsahují ovládací prvek obsahu pro všechny ovládací prvky ContentPlaceHolder na hlavní stránce. Při návštěvě stránky obsahu prostřednictvím prohlížeče modulu ASP.NET vytvoří hierarchii ovládacích prvků stránky předlohy a vkládá hierarchii ovládacích prvků stránky obsahu do příslušných místech. Tato hierarchie kombinované ovládací prvek se vykreslí a výsledného souboru HTML se vrátí do prohlížeče koncového uživatele. V důsledku toho stránky obsahu vysílá společné značky definované v jeho hlavní stránky mimo ovládacích prvků ContentPlaceHolder a kód specifický pro stránku definované v rámci své vlastní ovládací prvky obsahu. Obrázek 3 ilustruje tento koncept.


[![Požadovaná stránka značek je začleněny do stránky předlohy](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Obrázek 03**: The požadované na stránce značek je začleněny do stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Teď, když máte jsme zmínili, jak fungují stránky předlohy, Pojďme se na pohled na vytváření stránky předlohy a přidružené obsahu stránky pomocí Visual Web Developer.

> [!NOTE]
> Pokud chcete oslovit širokou cílovou skupinu je to možné, webu technologie ASP.NET při sestavování v celé této sérii kurzů se vytvoří pomocí technologie ASP.NET 3.5 společnosti Microsoft bezplatnou verzi Visual Studio 2008 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Pokud jste dosud nebyly upgradovány na technologii ASP.NET 3.5, Nedělejte si starosti – Principy probírané v těchto kurzech pracovní stejně dobře s technologií ASP.NET 2.0 a Visual Studio 2005. Ale některé ukázkové aplikace mohou používat nové funkce rozhraní .NET Framework verze 3.5; Když se používají funkce specifické pro 3.5, můžu zahrnout poznámku, která popisuje, jak implementovat podobné funkce ve verzi 2.0. Mějte na paměti, která ukázkové aplikace dostupné pro stahování jednotlivých kurzů cílového rozhraní .NET Framework verze 3.5, což vede k `Web.config` soubor, který obsahuje prvky konfigurace specifické pro 3.5. Dlouhý text krátký, pokud ještě nemáte k instalaci rozhraní .NET 3.5 na počítači poté ke stažení webové aplikace nebude fungovat bez první odebrání značky specifické 3.5 z `Web.config`. Zobrazit [rozbor ASP.NET verze 3.5. `Web.config` souboru](http://www.4guysfromrolla.com/articles/121207-1.aspx) Další informace o tomto tématu.


## <a name="step-1-creating-a-master-page"></a>Krok 1: Vytvoření stránky předlohy

Než budeme můžete věnovat vytváření a použití stránky předlohy a obsahu, musíte nejprve webové stránky ASP.NET. Začněte vytvořením nového webu souboru na základě systému technologie ASP.NET. K tomu, spusťte aplikaci Visual Web Developer a pak přejděte do nabídky soubor a zvolte nový web zobrazení dialogového okna Nový web (viz obrázek 4). Výběr šablony webové stránky ASP.NET, nastavte rozevírací seznam umístění do systému souborů, vyberte složku, umístěte na webovém serveru a nastavení jazyka Visual Basic. Tím se vytvoří nový web s `Default.aspx` stránka ASP.NET `App_Data` složky a `Web.config` souboru.

> [!NOTE]
> Visual Studio podporuje dva režimy správy projektu: webové projekty a projekty webových aplikací. Webové projekty chybí soubor projektu, že projekty webových aplikací napodobuje architekturu projektu v aplikaci Visual Studio .NET 2002/2003 – zahrnout soubor projektu a kompilaci zdrojového kódu v projektu do jednoho sestavení, který je umístěn do `/bin` složka. Visual Studio 2005 zpočátku pouze podporované projekty webů, i když s aktualizací Service Pack 1; byl znovuzavedeno modelu projektu webové aplikace Visual Studio 2008 nabízí oba modely projektu. Visual Web Developer 2005 a edice 2008, ale podporují pouze webové projekty. Používám modelu projektu webové stránky Moje ukázky v této sérii kurzů. Pokud používáte jiné Express edition a chcete použít [modelu projektu webové aplikace](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) místo toho můžete tak učinit, ale mějte na paměti, že mohou být některé nesrovnalosti mezi zobrazí na obrazovce a kroky musíte provést porovnání Zobrazí snímky obrazovky a pokyny uvedené v následujících kurzech.


[![Vytvoření nového souboru na základě systému webového serveru](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Obrázek 04**: vytvoření webu New File System-Based ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Dále přidejte na stránku předlohy k lokalitě v kořenovém adresáři pravým tlačítkem myši na název projektu, výběrem přidat novou položku a výběr šablony stránky předlohy. Všimněte si, že hlavní stránky končit příponou `.master`. Název této nové stránky předlohy `Site.master` a klikněte na tlačítko Přidat.


[![Přidat stránku předlohy s názvem Site.master na web](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Obrázek 05**: přidejte název stránky předlohy `Site.master` na web ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Přidání nového souboru hlavní stránky prostřednictvím Visual Web Developer vytvoří hlavní stránku s následující kód:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

První řádek v deklarativním označení [ `@Master` směrnice](https://msdn.microsoft.com/library/ms228176.aspx). `@Master` – Direktiva je podobný [ `@Page` – direktiva](https://msdn.microsoft.com/library/ydy4x04a.aspx) , který se zobrazí na stránkách ASP.NET. Definuje jazyk na straně serveru (VB) a informace o umístění a dědičnost třídy modelu code-behind na hlavní stránce.

`DOCTYPE` a deklarativní stránky se zobrazí pod `@Master` směrnice. Stránka obsahuje statický kód HTML spolu s čtyři ovládací prvky na straně serveru:

- **Webový formulář ( `<form runat="server">`)** – protože všechny stránky ASP.NET obvykle mají webového formuláře – a protože hlavní stránka může obsahovat webové ovládací prvky, které musí být uvedena v rámci webového formuláře – je potřeba přidat webový formulář do hlavní stránky (místo přidávání webového formuláře do e stránka ACH obsahu).
- **Ovládací prvek ContentPlaceHolder s názvem `ContentPlaceHolder1`**  -tento ovládací prvek ContentPlaceHolder se zobrazí v rámci webového formuláře a slouží jako oblast obsahu stránky uživatelského rozhraní.
- **Na straně serveru `<head>` element** – `<head>` element má `runat="server"` atribut zpřístupnění prostřednictvím kódu na straně serveru. `<head>` Element je implementovaná tímto způsobem tak, aby název stránky a další `<head>`-týkající se kód může přidat nebo upravit prostřednictvím kódu programu. Například nastavení stránky ASP.NET `Title` změny vlastností `<title>` element vykreslen metodou `<head>` serverový ovládací prvek.
- **Ovládací prvek ContentPlaceHolder s názvem `head`**  -tento ovládací prvek ContentPlaceHolder se zobrazí v rámci `<head>` server řízení a můžou používat deklarativně přidání obsahu do `<head>` elementu.

Tato výchozí značka deklarativní stránky předlohy slouží jako výchozí bod pro vytvoření vlastní stránky předlohy. Nebojte se, k úpravě HTML nebo přidání dalších webových ovládacích prvcích nebo prvků ContentPlaceHolder na hlavní stránku.

> [!NOTE]
> Při návrhu stránky předlohy Ujistěte se, že webový formulář obsahuje stránky předlohy a alespoň jeden prvek ContentPlaceHolder se zobrazí v rámci tohoto webového formuláře.


### <a name="creating-a-simple-site-layout"></a>Vytvoření rozložení platného pro jednoduché lokality

Umožňuje rozbalit `Site.master`na výchozí deklarativní, chcete-li vytvořit rozložení webu, kde všechny stránky sdílet: společné hlavičky; levý sloupec s navigace, zprávy a další obsah webu; a přidáme zápatí, který zobrazuje ikonu "S využitím pomocí technologie Microsoft ASP.NET". Obrázek 6 zobrazuje konečný výsledek stránky předlohy, když jeden z jeho obsahu stránky je zobrazit pomocí prohlížeče. Červené v kroužku oblast na obrázku 6 je specifické pro právě navštívené stránky (`Default.aspx`); další obsah je na hlavní stránce definován a proto konzistentní vzhledem k aplikacím na všech stránkách obsahu.


[![Stránky předlohy se stránkou definuje značky pro horní, doleva a dolní části](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Obrázek 06**: stránky předlohy se stránkou definuje značky pro horní, doleva a dolní části ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Pokud chcete dosáhnout rozložení webu je znázorněno na obrázku 6, začněte tím, že aktualizace `Site.master` stránku předlohy tak, aby obsahoval následující deklarativní:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Rozložení stránky předlohy je definován pomocí řady `<div>` elementů HTML. `topContent` `<div>` Obsahuje kód, který se zobrazí v horní části každé stránky, zatímco `mainContent`, `leftContent`, a `footerContent` `<div>` s slouží k zobrazení obsahu na stránce, v levém sloupci a "s využitím ve společnosti Microsoft Ikona technologie ASP.NET", v uvedeném pořadí. Kromě přidání těchto `<div>` prvky, jsem také přejmenovat `ID` vlastnost primární ovládací prvek ContentPlaceHolder z `ContentPlaceHolder1` k `MainContent`.

Pravidla formátování a rozložení pro tyto roztříděné `<div>` elementů je států v [šablony stylů CSS (Cascading)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) souboru `Styles.css`, které je zadáno pomocí `<link>` prvek stránky předlohy `<head>`elementu. Tyto různé pravidla definují vzhled a chování každého `<div>` element bylo uvedeno výše. Například `topContent` `<div>` má element, který zobrazí text "Hlavní stránky kurzy" a odkaz, jeho formátování pravidla stanovená dokumentem `Styles.css` následujícím způsobem:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Pokud postupujete na počítač, budete muset stáhnout související kód v tomto kurzu a přidat `Styles.css` soubor do projektu. Podobně, budete také muset vytvořit složku s názvem `Images` a zkopírujte na ikonu "S využitím pomocí technologie Microsoft ASP.NET" ze staženého ukázkovému webu do vašeho projektu.

> [!NOTE]
> Informace o formátování webové stránky a šablony stylů CSS je nad rámec tohoto článku. Další informace o CSS, podívejte se [šablon stylů CSS kurzy](http://www.w3schools.com/css/default.asp) na [W3Schools.com](http://www.w3schools.com/). Také neváhejte si stáhnout související kód v tomto kurzu a pohrát si s nastavením šablony stylů CSS v `Styles.css` abyste viděli efekt různá pravidla formátování.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Vytvoření pomocí existující šablonu návrhu stránky předlohy

V průběhu let jsem začlenění několik webových aplikací ASP.NET pro (krátkodobé používání) – pro střední firmy. Některé z mých klientů má existující lokality rozložení, které chtěli používat; ostatní pověřili příslušné grafický Návrhář. Několik svěřených mi návrh webu rozložení. Jak se dá zjistit podle obrázku 6, tasking programátorovi, aby návrh rozložení tohoto webu je obvykle vhodné tak, že má váš účetní provádět open-heart surgery, zatímco vaše lékař nemá daně.

Naštěstí innumerous weby, které nabízejí bezplatné šablony návrhu ve formátu HTML – Google vrátil více než 6 milionů výsledky pro hledaný termín "šablony bezplatné webu." Moje Oblíbené struktur je [OpenDesigns.org](http://opendesigns.org/). Jakmile najdete šablonu webu, který rádi používáte, přidejte do projektu webu souborů šablon stylů CSS a bitových kopií a integrovat šablony HTML stránky předlohy.

> [!NOTE]
> Microsoft taky nabízí řadu [bez šablony ASP.NET návrhu Start Kit](https://msdn.microsoft.com/asp.net/aa336613.aspx) , které integrují do dialogového okna Nový web v sadě Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Krok 2: Vytvoření související stránky obsahu

S hlavní stránkou vytvořili budeme připravení začít vytvářet stránky technologie ASP.NET, které jsou vázány na hlavní stránku. Tyto stránky se označují jako *stránky obsahu*.

Umožňuje přidat nové stránky ASP.NET do projektu a vytvořte mu vazbu k `Site.master` stránky předlohy. Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte možnost Přidat novou položku. Vyberte šablonu Webový formulář, zadejte název `About.aspx`a potom zaškrtněte políčko "Vybrat hlavní stránku", jak je znázorněno na obrázku 7. Tím se zobrazí Select dialogového okna stránky předlohy pole (viz obrázek 8) z kde můžete vybrat hlavní stránku.

> [!NOTE]
> Pokud jste vytvořili pomocí modelu projektu webové aplikace namísto modelu projektu webové stránky webu ASP.NET neuvidíte zaškrtávací políčko "Vybrat hlavní stránku" v dialogovém okně Přidat novou položku je znázorněno na obrázku 7. K vytvoření obsahu musí stránku při projekt webové aplikace pomocí modelu vyberte šablonu Webový formulář obsahu místo šabloně webového formuláře. Po výběrem webový formulář obsahu šablony a kliknutím na Přidat, vyberte stejný hlavní stránky se zobrazí dialogové okno je znázorněno na obrázku 8.


[![Přidejte novou stránku obsahu](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Obrázek 07**: přidejte novou stránku obsahu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Vyberte na stránce předlohy Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Obrázek 08**: vyberte `Site.master` stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Jak ukazuje následující kód obsahuje nové stránky obsahu `@Page` směrnice, odkazuje zpátky na jeho hlavní stránky a ovládací prvek obsahu pro všechny ovládací prvky ContentPlaceHolder na hlavní stránce.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> V části "Vytvoření jednoduché rozložení webu" v kroku 1 můžu přejmenovat `ContentPlaceHolder1` k `MainContent`. Pokud jste tento ovládací prvek ContentPlaceHolder nepřejmenovali `ID` stejným způsobem, budou mírně lišit deklarativní obsahu stránky v označení uvedené výše. Konkrétně, druhý obsahu ovládacího prvku `ContentPlaceHolderID` projeví `ID` odpovídajícího prvku ContentPlaceHolder v ovládacím prvku stránky předlohy.


Při generování obsahu stránky, musíte modul ASP.NET prověřování na stránce ovládací prvky s ovládacími prvky ContentPlaceHolder jeho hlavní stránky obsahu. Modul ASP.NET zjistí hlavní stránce stránky obsahu z `@Page` direktivy `MasterPageFile` atribut. Jak ukazuje výše uvedené značky, tuto stránku obsahu je vázán na `~/Site.master`.

Vzhledem k tomu, že hlavní stránka má dva ovládací prvky ContentPlaceHolder - `head` a `MainContent` -Visual Web Developer vygenerované dva ovládací prvky obsahu. Každý ovládací prvek odkazuje na konkrétní ContentPlaceHolder prostřednictvím jeho `ContentPlaceHolderID` vlastnost.

Kde je hlavní stránky lesku za předchozí postupy šablony webu s jejich podpory během návrhu. Obrázek 9 ukazuje `About.aspx` stránky obsahu, když zobrazit pomocí zobrazení návrhu Visual Web Developer. Mějte na paměti, zatímco je zobrazen obsah stránky předlohy, je zobrazena šedě a nelze ji změnit. Ovládací prvky obsahu odpovídajících prvků ContentPlaceHolder stránky předlohy se, ale upravovat. A stejně jako s jakoukoli jinou stránku ASP.NET můžete vytvořit stránku obsahu rozhraní tak, že přidáte webové ovládací prvky prostřednictvím zobrazení Zdroj nebo návrhu.


[![Návrhové zobrazení obsahu stránky zobrazí obsah specifický pro stránku a hlavní stránky](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Obrázek 09**: The obsahu návrhové zobrazení zobrazí i konkrétní stránky a stránky obsahu stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Přidání značky a ovládací prvky webové stránky obsahu

Za chvíli nějaký obsah pro vytvoření `About.aspx` stránky. Jak je vidět na obrázku 10, zadali nadpisů "O autorovi" a pár odstavců text, ale teď můžete přidat ovládací prvky webového příliš. Po vytvoření tohoto rozhraní, přejděte `About.aspx` stránky prostřednictvím prohlížeče.


[![Na stránce About.aspx prostřednictvím prohlížeče](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Obrázek 10**: přejděte `About.aspx` stránku prostřednictvím prohlížeči ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


Je důležité pochopit, že požadovaná stránka obsahu a jeho přidružené hlavní stránky jsou začleněny a se vykresluje jako celek zcela na webovém serveru. Koncový uživatel prohlížeče odeslaný výsledný, roztaveného HTML. Chcete-li to ověřit, zobrazení kódu HTML přijatých prohlížeče tak, že přejdete do nabídky zobrazení a zvolíte zdroj. Všimněte si, že neexistují žádné snímky nebo jiných specializovaných techniky pro zobrazení dvou různých webových stránek v jednom okně.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Vytvoření vazby hlavní stránky na stávající stránku ASP.NET

Jak jsme viděli v tomto kroku, přidání nové stránky obsahu do webové aplikace ASP.NET je stejně jednoduché jako zaškrtnete políčko "Vybrat hlavní stránku" a vybrat hlavní stránku. Bohužel převod stávající stránku ASP.NET na stránku předlohy není snadné.

Vytvořit vazbu na stránku předlohy na stávající stránku ASP.NET je třeba provést následující kroky:

1. Přidat `MasterPageFile` atribut na stránku ASP.NET `@Page` směrnice, odkazuje na příslušnou stránku předlohy.
2. Přidání ovládacích prvků obsahu všech prvků ContentPlaceHolder na stránce předlohy.
3. Selektivně vyjmout a vložit existující obsah stránky ASP.NET do odpovídající ovládací prvky obsahu. Řeknu "selektivně" zde, protože stránka technologie ASP.NET pravděpodobně obsahuje kód, který je již vyjádřena stránky předlohy, jako `DOCTYPE`, `<html>` elementu a webového formuláře.

Podrobné pokyny k tomuto procesu spolu se snímky obrazovky, projděte si [Scott Guthrie](https://weblogs.asp.net/scottgu/)společnosti [pomocí stránek předlohy a navigace na webu](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) kurzu. "Aktualizace `Default.aspx` a `DataSample.aspx` použití stránky předlohy se stránkou" část podrobně popisuje tyto kroky.

Protože je snazší vytvářet nové stránky obsahu, než je můžete převést stávající stránky technologie ASP.NET na stránky obsahu, doporučuje se, že při každém vytvoření nového webu ASP.NET na stránku předlohy do lokality přidat. Vytvořit vazbu všech nových stránek ASP.NET na tuto stránku předlohy. Nedělejte si starosti, pokud počáteční stránky předlohy je velmi jednoduché nebo prostý; na hlavní stránce můžete aktualizovat později.

> [!NOTE]
> Při vytváření nové aplikace ASP.NET, aplikace Visual Web Developer přidá `Default.aspx` stránky, který není vázán na hlavní stránku. Pokud si chcete Postup převedení stávající stránku ASP.NET na stránku obsahu, pokračujte a činit se `Default.aspx`. Alternativně můžete odstranit `Default.aspx` a potom je znovu přidat, ale tentokrát zaškrtnete políčko "Vyberte stránka předlohy".


## <a name="step-3-updating-the-master-pages-markup"></a>Krok 3: Aktualizace kódu stránky předlohy

Jedním z hlavních výhod hlavní stránky je, že jedna stránka předlohy slouží k definování celkového rozložení pro mnoho stránky na webu. Proto se aktualizace v lokalitě vzhled a chování vyžaduje aktualizaci jednoho souboru – stránka předlohy.

Pro toto chování ilustrují, aktualizaci Pojďme naší hlavní stránky, aby zahrnovala aktuální datum v horní části v levém sloupci. Přidejte popisek s názvem `DateDisplay` k `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Dále vytvořte `Page_Load` obslužné rutiny události pro hlavní stránky a přidejte následující kód:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Ve výše uvedeném kódu nastaví popisku `Text` vlastnosti na aktuální datum a čas ve formátu den v týdnu, název měsíce a dne dvěma číslicemi (viz obrázek 11). Díky této změně návštěvě jeden z obsahu stránky. Jak ukazuje obrázek 11, výsledný kód okamžitě aktualizován zahrnout změnit na stránce předlohy.


[![Změny na stránku předlohy se projeví při prohlížení stránku obsahu](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Obrázek 11**: The změny stránky předlohy se stránkou se projeví při prohlížení stránku obsahu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Jak ukazuje tento příklad, hlavní stránky mohou obsahovat webové serverové ovládací prvky, kód a obslužné rutiny událostí.


## <a name="summary"></a>Souhrn

Stránky předlohy povolit vývojáře využívající technologii ASP.NET a navrhněte rozložení konzistentní webu, který je možné snadno aktualizovat. Vytvoření hlavní stránky a jejich přidružené stránky obsahu je snadné – stačí vytvořit standardní stránky technologie ASP.NET, jako Visual Web Developer nabízí bohatou podporu návrhu.

Příklad stránky předlohy, který jsme vytvořili v tomto kurzu má dva ovládací prvky ContentPlaceHolder, head a MainContent. Jsme zadali pouze značky pro ovládací prvek MainContent ContentPlaceHolder na naší stránce obsahu, ale. V dalším kurzu se podíváme na použití více obsahu ovládacích prvků na stránce obsahu. Můžeme také zjistit, jak definovat výchozí kód pro obsah ovládací prvky v rámci stránky předlohy a jak přepínat mezi pomocí výchozího kódu definované v hlavní stránky a poskytuje vlastní značky ze stránky obsahu.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Technologie ASP.NET pro profesionální návrháře využívající: bezplatná šablony návrhu a pokyny na vytváření webů ASP.NET pomocí webových standardů](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Přehled ASP.NET hlavní stránky](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Šablony kaskádových šablony stylů (CSS) kurzů](http://www.w3schools.com/css/default.asp)
- [Použije dynamické nastavení názvu stránky](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Stránky předlohy v ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Kurzy rychlý start stránky předlohy](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](nested-master-pages-cs.md)
> [další](multiple-contentplaceholders-and-default-content-vb.md)
