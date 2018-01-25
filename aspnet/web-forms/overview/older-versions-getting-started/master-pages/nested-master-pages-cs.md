---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: "Vnořené hlavní stránky (C#) | Microsoft Docs"
author: rick-anderson
description: "Ukazuje, jak lze vnořit jednu stránku předlohy v rámci jiného."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 97513a5a6ac7a958a03626f16a328ecb0b85c03f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="nested-master-pages-c"></a>Vnořené hlavní stránky (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Ukazuje, jak lze vnořit jednu stránku předlohy v rámci jiného.


## <a name="introduction"></a>Úvod

V průběhu posledních devět kurzy jsme viděli, jak implementovat rozložení na webu s stránky předlohy. Stručně řečeno hlavní stránky nám umožňují, vývojáři stránky definovat běžné značek na hlavní stránce spolu s určitých oblastí, které se dají přizpůsobit na základě stránky obsahu stránce obsahu. Ovládací prvky ContentPlaceHolder na hlavní stránce znamenat přizpůsobitelné oblasti; vlastní kód pro obsahuje rozložení jsou definovány v obsahu stránce prostřednictvím ovládací prvky obsahu.

Stránky předlohy technik, které jsme jste prozkoumali doposud jsou skvělé, pokud budete mít jedno rozložení použít celého webu. Mnoho velkých webů však mít rozložení lokality, které je přizpůsobené přes různé části. Představte si třeba zdravotní péče aplikace měla nemocnice pracovníci slouží ke správě informací o pacientech, aktivity a fakturace. Mohou být tři typy webových stránek v této aplikaci:

- Zaměstnanci stránek specifických pro člena kde pracovníků oddělení můžete aktualizovat dostupnosti, zobrazit plány, nebo požádejte dovolená.
- Kde pracovníků oddělení umožňuje zobrazit nebo upravit informace o konkrétní pacienta pacienta konkrétní stránky.
- Fakturace konkrétní stránky, kde zkontrolovat aktuální účetní deklarací identity stavy a finanční sestavy.

Každé stránce může sdílet běžné rozložení, jako je například nabídce v horní části a řadu často používaných odkazy podél dolního. Ale zaměstnanci - pacienta- a stránek specifických pro fakturaci muset přizpůsobit toto Obecné rozložení. Například možná všech stránek specifických pracovníci by měla obsahovat seznam kalendář a úkoly zobrazuje dostupnost aktuálně přihlášeného uživatele a denní plán. Pravděpodobně muset všech stránek specifických pro pacienta zobrazit název, adresa a informací o pojištění pro pacienta upravována jejichž informace.

Je možné vytvořit takové vlastní rozložení pomocí *vnořené hlavní stránky*. K implementaci výše uvedené scénáře, jsme by začněte vytvořením stránku předlohy, definovaná na webu obsah rozložení, nabídky a zápatí s ContentPlaceHolders definování přizpůsobitelné oblasti. Vytvoříme by pak tři vnořené hlavní stránky, jednu pro každý typ webové stránky. Každé vnořené hlavní stránce nadefinovat obsahu mezi typ stránky obsahu, které používají stránky předlohy. Jinými slovy vnořené stránky předlohy pro konkrétní pacienta stránky obsahu bude zahrnovat značek a programové logiku pro zobrazení informací o pacienta Upravovaný. Při vytváření nové stránky pacienta konkrétní jsme by vazby této vnořené hlavní stránky.

V tomto kurzu se spustí zvýraznění výhod vnořené hlavní stránky. Potom ukazuje, jak vytvořit a použít vnořené hlavní stránky.

> [!NOTE]
> Vnořené hlavní stránky bylo umožněno od rozhraní .NET Framework verze 2.0. Visual Studio 2005 však nenabízely podporu návrhu pro vnořené hlavní stránky. Dobrá zpráva je, že Visual Studio 2008 nabízí bohaté prostředí návrhu pro vnořené hlavní stránky. Pokud mají zájem o pomocí vnořené hlavní stránky, ale stále používáte Visual Studio 2005, podívejte se na [Scott Guthrie](https://weblogs.asp.net/scottgu/)na položku blogu, [tipy pro vnořené hlavní stránky v době návrhu 2005 VS](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Výhody vnořené hlavní stránky

Mnohé weby mají zastřešující návrh lokality, jakož i více přizpůsobené návrhů specifická pro určité typy stránek. Například v naší ukázkové webové aplikaci jsme vytvořili elementární oddílu Správa (stránky v `~/Admin` složku). Aktuálně webové stránky v `~/Admin` složky použít stejnou stránku předlohy jako tyto stránky není v části Správa (konkrétně, `Site.master` nebo `Alternate.master`, v závislosti na výběru uživatele).

> [!NOTE]
> Prozatím se předstírají, že aby náš web má jenom jednu stránku předlohy, `Site.master`. Jsme budete adres pomocí vnořené hlavní stránky s dva (nebo více) hlavní stránky počínaje "Pomocí vnořené hlavní stránky pro části" později v tomto kurzu.


Představte si, že jsme se výzva k přizpůsobení rozložení stránky správy, které chcete zahrnout další informace nebo odkazy, které by jinak v dalších stránek v lokalitě. Existují čtyři metody k implementaci tento požadavek:

1. Ručně přidat informace specifické pro správu a odkazy na všechny stránky obsahu `~/Admin` složky.
2. Aktualizace `Site.master` o tom, jestli některé stránky Správa přístupu na základě stránky předlohy zahrnout správu části konkrétní informace a odkazy, a pak přidejte kód k hlavní stránce můžete zobrazit nebo skrýt tyto části.
3. Vytvořit novou stránku předlohy speciálně pro části, zkopírovat kód z `Site.master`, přidejte informace specifické pro oddíl správy a odkazy a pak aktualizaci obsahu stránky v `~/Admin` složku, kterou chcete použít tento nový hlavní server stránka.
4. Vytvoření vnořené hlavní stránky, která se sváže s `Site.master` a stránky obsahu v `~/Admin` složky použijte tento nový vnořené stránky předlohy. Tato vnořené hlavní stránky by mělo zahrnovat jenom Další informace a odkazy, které jsou specifické pro stránky správy a nebude potřeba opakování kódu již definována v `Site.master`.

První možností je nejméně palatable. Celý bod použití hlavní stránky je přesunout mimo museli ručně zkopírujte a vložte běžné značek pro nové stránky ASP.NET. Druhou možností je přijatelné, ale přidá méně udržovatelný, jako je bulks si hlavní stránky s kód, který se zobrazí pouze občas a vyžaduje vývojáře úpravy stránky předlohy, chcete-li vyřešit tento kód a mít na paměti, když aplikace přesně určitých zobrazení značek a když je skrytý. Tento postup by méně chráněném jako přizpůsobení z více typů webových stránek musela být obslouženo tento jedné stránky předlohy.

Odebere třetí možnost zbytečné soubory a složitost problémy surfaced se druhá možnost. Možnost pro tři hlavní nevýhodou je však vyžaduje nám zkopírujte a vložte běžné rozložení z `Site.master` na novou stránku předlohy konkrétní části Správa. Pokud jsme se později rozhodnete změnit pro celou lokalitu rozložení máme mějte na paměti, jak ho změnit na dvou místech.

Možnost čtvrtý vnořené hlavní stránky, sdělte nám nejvhodnější druhý a třetí možnosti. Informace na webu rozložení postaráno v jednom souboru - nejvyšší úrovně stránky předlohy - i specifické pro konkrétní oblasti obsahu je rozdělené na jiné soubory.

V tomto kurzu začíná podívejte se na vytvoření a použití jednoduché vnořené hlavní stránky. Vytvoříme nové hlavní stránku nejvyšší úrovně, dvě vnořené hlavní stránky a dvě obsahu stránky. Počínaje "Pomocí vnořené hlavní stránky pro části", podíváme na aktualizace našich existující architekturu stránky předlohy pro patří použití vnořené hlavní stránky. Konkrétně jsme vytvoření vnořené hlavní stránky a použít ho k zahrnovat další obsah vlastní obsahu stránky v `~/Admin` složky.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Krok 1: Vytvoření jednoduché nejvyšší úrovně stránky předlohy

Vytváření vnořené hlavní na základě jedné z existující hlavní stránky a pak aktualizuje existující stránky obsahu používat tento nový vnořené hlavní stránky namísto nejvyšší úrovně stránky předlohy zahrnuje některé složitost, protože existující stránky obsahu již očekávat určité Ovládací prvky ContentPlaceHolder definované v nejvyšší úrovně stránky předlohy. Vnořené hlavní stránce proto musíte taky zahrnout se stejnými ovládacími prvky ContentPlaceHolder se stejnými názvy. Kromě toho naše konkrétní ukázkové aplikace má dvě hlavní stránky (`Site.master` a `Alternate.master`), jsou dynamicky přiřadit stránku obsahu na základě předvoleb uživatele, který další přidá do této složitost. Aktualizace stávající aplikace pro používání vnořené hlavní stránky později v tomto kurzu se podíváme, ale můžeme vnořené první se zaměřují na jednoduchý příklad stránky předlohy.

Vytvořte novou složku s názvem `NestedMasterPages` a pak přidejte nový soubor předlohové stránky do této složky s názvem `Simple.master`. (Viz obrázek 1 pro snímek obrazovky Průzkumníka řešení po přidání tuto složku a soubor.) Přetáhněte `AlternateStyles.css` soubor stylů v Průzkumníku řešení do návrháře. Tím se přidá `<link>` soubor stylů v elementu `<head>` elementu, po jejímž uplynutí stránky předlohy `<head>` elementu značek by měl vypadat jako:


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Dál přidejte následující kód v rámci webové formu `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Tento kód zobrazí odkaz s názvem "Vnořené hlavní stránky (jednoduchý)" v horní části stránky velkými písmeny bílé na Námořnická modř pozadí. Pod kterou je `MainContent` ContentPlaceHolder. Obrázek 1 zobrazuje `Simple.master` stránky předlohy při načítání v návrháři sady Visual Studio.


[![Vnořené stránky předlohy definuje obsahu specifické pro stránky v části Správa](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Obrázek 01**: vnořené hlavní stránky definuje obsahu specifické pro stránky v části Správa ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Krok 2: Vytvoření jednoduché vnořené hlavní stránky

`Simple.master`obsahuje dvou ovládacích prvků ContentPlaceHolder: `MainContent` ContentPlaceHolder jsme přidali v rámci ve webovém formuláři spolu s `head` ContentPlaceHolder v `<head>` elementu. Pokud se k vytvoření stránky obsahu a navázat jej na `Simple.master` stránky obsahu by měla mít dva ovládací prvky obsahu odkazující na dvě ContentPlaceHolders. Podobně pokud jsme vytvoření vnořené hlavní stránky a navázat jej na `Simple.master` pak vnořené hlavní stránky bude mít dva ovládací prvky obsahu.

Umožňuje přidat nové vnořené hlavní stránky k `NestedMasterPages` složku s názvem `SimpleNested.master`. Klikněte pravým tlačítkem na `NestedMasterPages` složky a vyberte Přidat novou položku. Otevře dialogové okno Přidat novou položku, na obrázku 2. Vyberte typ šablony stránky předlohy a zadejte název nové stránky předlohy. K označení, že nová stránka předlohy by měla být vnořené hlavní stránky, zaškrtněte políčko "Vyberte stránku předlohy".

Potom klikněte na tlačítko Přidat. Bude se zobrazovat stejné vyberte stránku předlohy dialogové okno se zobrazí při vytváření vazby obsahu stránce pro stránky předlohy (viz obrázek 3). Vyberte `Simple.master` stránky předlohy v `NestedMasterPages` složky a klikněte na tlačítko OK.

> [!NOTE]
> Pokud jste vytvořili pomocí modelu projektu webové aplikace místo webový projekt modelu webu ASP.NET neuvidíte políčka "Vyberte stránku předlohy" v dialogovém okně Přidat novou položku na obrázku 2. K vytvoření vnořené hlavní stránky při použití modelu projektu webové aplikace, musíte zvolit šablonu vnořené stránky předlohy (namísto šabloně – stránka předlohy). Po výběru šablony vnořené hlavní stránky a kliknutím na tlačítko Přidat, stejné vyberte stránku předlohy, zobrazí se dialogové okno znázorněný na obrázku 3.


[![Zkontrolujte &quot;vyberte stránku předlohy&quot; zaškrtávací políčko k přidání vnořené hlavní stránky](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Obrázek 02**: Zaškrtnutím políčka "Vyberte stránku předlohy" Přidání vnořené hlavní stránky ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image6.png))


[![Vytvoření vazby vnořené hlavní stránky na stránku předlohy Simple.master](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Obrázek 03**: vytvoření vazby vnořené hlavní stránky `Simple.master` – stránka předlohy ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image9.png))


Vnořené hlavní stránky deklarativní kód ukazuje následující příklad obsahuje dva ovládací prvky obsahu odkazující na nejvyšší úrovni stránky předlohy dvou ContentPlaceHolder ovládacích prvků.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

S výjimkou `<%@ Master %>` direktivy, deklarativní počáteční vnořené hlavní stránky je stejný jako kód, který původně vytvořil při vazbě obsahu stránce na stejné nejvyšší úrovně stránky předlohy. Jako obsahu stránce `<%@ Page %>` – direktiva, `<%@ Master %>` zahrnuje direktiva sem `MasterPageFile` atribut, který určuje vnořené hlavní stránky nadřazená stránka předlohy. Hlavní rozdíl mezi vnořené hlavní stránky a stránky obsahu vázána na stejné nejvyšší úrovně stránky předlohy je, že vnořené hlavní stránky může zahrnovat ContentPlaceHolder ovládací prvky. Ovládací prvky ContentPlaceHolder vnořené hlavní stránky definovat oblastech, kde stránky obsahu můžete upravit kód.

Aktualizujte tento vnořené hlavní stránky tak, aby se zobrazí text "Hello, z SimpleNested!" v ovládacím prvku obsahu, která odpovídá `MainContent` ContentPlaceHolder řízení.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Po provedení tohoto přidání, uložte vnořené hlavní stránky a pak přidejte novou stránku obsahu, aby `NestedMasterPages` složku s názvem `Default.aspx`a navázat jej na `SimpleNested.master` stránky předlohy. Po přidání této stránky může být Překvapený obsahovat žádné ovládací prvky obsahu, (viz obrázek 4)! Stránky obsahu přístup jenom k jeho *nadřazené* hlavní stránky ContentPlaceHolders. `SimpleNested.master`neobsahuje všechny ovládací prvky ContentPlaceHolder; proto všechny stránky obsahu vázána na tuto stránku předlohy nemůže obsahovat všechny ovládací prvky obsahu.


[![Nová stránka obsahu obsahuje žádné ovládací prvky obsahu](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Obrázek 04**: nové obsahu stránce obsahuje Ne ovládací prvky obsahu ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image12.png))


Co je potřeba udělat je aktualizovat vnořené hlavní stránky (`SimpleNested.master`) zahrnout ContentPlaceHolder ovládací prvky. Obvykle budete muset svoje vnořené hlavní stránky pro zahrnutí ContentPlaceHolder každý ContentPlaceHolder definované její nadřazená stránka předlohy, a tím umožní její podřízené stránky předlohy nebo obsahu stránce pro práci s žádným z ContentPlaceHolder nejvyšší úrovně stránky předlohy ovládací prvky.

Aktualizace `SimpleNested.master` stránky předlohy mají být zahrnuty ContentPlaceHolder jeho dvou obsahu ovládacích prvků. Zadejte obsahuje rozložení stejný název jako ContentPlaceHolder ovládací prvek, který odkazuje ovládací prvek jejich obsahu. To znamená, přidání ovládacího prvku ContentPlaceHolder s názvem `MainContent` obsahu řídit ve `SimpleNested.master` odkazující `MainContent` ContentPlaceHolder v `Simple.master`. V ovládacím prvku obsahu, který odkazuje na stejnou věc udělat `head` ContentPlaceHolder.

> [!NOTE]
> Při I doporučujeme názvy ovládacích prvků ContentPlaceHolder na hlavní stránce vnořené stejná jako ContentPlaceHolders na hlavní stránce nejvyšší úrovně, tato pojmenování symetrie se nevyžaduje. Obsahuje rozložení můžete dát do vnořené hlavní stránky libovolný název, který chcete. Však lze snadno zapamatovat ContentPlaceHolders odpovídají s co oblastí stránky, pokud Moje nejvyšší úrovně stránky předlohy a vnořené hlavní stránky použijte stejné názvy.


Po provedení těchto dodatky vaší `SimpleNested.master` stránky předlohy deklarativní by mělo vypadat jako následující:


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Odstranit `Default.aspx` obsahu stránce jsme právě vytvořili a pak znovu přidat, jeho vazby `SimpleNested.master` stránky předlohy. Tentokrát Visual Studio přidá dva prvky obsah `Default.aspx`, odkazující ContentPlaceHolders nyní definována ve `SimpleNested.master` (viz obrázek 6). Přidejte text "Hello, z Default.aspx!" obsah řídit, který odkazuje `MainContent`.

Obrázek 5 ukazuje tři entity podílejí zde - `Simple.master`, `SimpleNested.master`, a `Default.aspx` – a jak se vztahují na sebe navzájem. Jak ukazuje obrázek, vnořené hlavní stránky implementuje ovládací prvky obsahu pro ContentPlaceHolder nadřazeného objektu. Pokud tyto oblasti musí být přístupné na stránku obsahu, musí vnořené hlavní stránky přidat vlastní ContentPlaceHolders na ovládací prvky obsahu.


[![Nejvyšší úrovně a vnořené hlavní stránky, určují rozložení obsahu stránky](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Obrázek 05**: nejvyšší úrovně a vnořené hlavní stránky, určují rozložení obsahu stránce ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image15.png))


Toto chování ukazuje, jak obsahu stránce nebo hlavní stránky je pouze cognizant z nadřazené stránky předlohy. Toto chování je také indikován návrháři sady Visual Studio. Obrázek 6 ukazuje návrháře pro `Default.aspx`. Při návrháře jasně ukazuje, jaké oblasti je možné upravovat ze stránky obsahu a co části nejsou, není to rozlišení upravovat oblasti jsou z vnořené hlavní stránky a jaké jsou oblasti na hlavní stránce nejvyšší úrovně.


[![Obsahu stránce nyní zahrnuje ovládací prvky obsahu pro ContentPlaceHolders vnořené stránky předlohy](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Obrázek 06**: obsahu stránce nyní zahrnuje ovládací prvky obsahu pro vnořené hlavní stránky ContentPlaceHolders ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Krok 3: Přidání druhého jednoduché vnořené hlavní stránky

Výhodou vnořené hlavní stránky je více zřejmé, pokud existuje více vnořené hlavní stránky. Příkladem této výhody, vytvořte jiný vnořené hlavní stránky v `NestedMasterPages` složky; Tento nový název vnořené hlavní stránky `SimpleNestedAlternate.master` a navázat jej na `Simple.master` stránky předlohy. Přidání ovládacích prvků ContentPlaceHolder ve dvou obsahu ovládacích prvků vnořené hlavní stránky, jako jsme provedli v kroku 2. Také přidáte text "Hello, z SimpleNestedAlternate!" v ovládacím prvku obsahu, která odpovídá nejvyšší úrovně stránky předlohy `MainContent` ContentPlaceHolder. Po provedení těchto změn vaší nové vnořené hlavní stránky deklarativní by měl vypadat takto:


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Vytvoření stránky obsahu s názvem `Alternate.aspx` v `NestedMasterPages` složky a navázat jej na `SimpleNestedAlternate.master` vnořené stránky předlohy. Přidejte text "Hello, z alternativní!" v ovládacím prvku obsahu, která odpovídá `MainContent`. Obrázek 7 znázorňuje `Alternate.aspx` při zobrazení v návrháři sady Visual Studio.


[![Alternate.aspx je vázána na stránku předlohy SimpleNestedAlternate.master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Obrázek 07**: `Alternate.aspx` je vázána `SimpleNestedAlternate.master` – stránka předlohy ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image21.png))


Porovnejte návrháře na obrázku 7 do návrháře na obrázku 6. Obě obsahu stránky sdílet se stejné rozvržení definované v nejvyšší úrovně stránky předlohy (`Simple.master`), konkrétně název "Vnořené hlavní stránky kurzu (jednoduchý)". Ještě mají odlišné obsah definované v jejich nadřazené hlavní stránky - text "Hello, z SimpleNested!" Obrázek 6 a "Hello, z SimpleNestedAlternate!" na obrázku 7. Udělena, zde tyto rozdíly jsou trivial, ale můžete rozšířit tento příklad zahrnout smysluplnější rozdíly. Například `SimpleNested.master` stránky může zahrnovat nabídka s možnostmi, které jsou specifické pro jeho stránky obsahu, zatímco `SimpleNestedAlternate.master` může mít informace týkající se stránky obsahu, které svázat s ní.

Představte si teď jsme musel provést změnu zastřešující rozložení webu. Představte si například, že jsme chtěli přidat seznam běžných odkazy na všechny stránky obsahu. K tomu budeme aktualizovat hlavní stránku nejvyšší úrovně `Simple.master`. Všechny změny existuje se okamžitě projeví v jeho vnořené hlavní stránky a rozšíření, jejich obsahu stránky.

K předvedení usnadnění, pomocí kterého jsme zastřešující lokality rozložení změnit, otevřete `Simple.master` hlavní stránky a přidejte následující kód mezi `topContent` a `mainContent` `<div>` prvky:


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Tento postup přidá dva odkazy do horní části každé stránce, která se sváže s `Simple.master`, `SimpleNested.master`, nebo `SimpleNestedAlternate.master`; tyto změny použít na všechny vnořené hlavní stránky a jejich obsahu stránky okamžitě. Obrázek 8 ukazuje `Alternate.aspx` při zobrazení prostřednictvím prohlížeče. Poznámka: Přidání odkazů v horní části stránky (ve srovnání s obrázek 7).


[![Změnit na hlavní stránku nejvyšší úrovně se okamžitě projeví v jeho vnořené hlavní stránky a jejich stránky obsahu](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Obrázek 08**: změnit na hlavní stránku nejvyšší úrovně se okamžitě projeví v jeho vnořené hlavní stránky a jejich stránky obsahu ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Použití vnořených stránky předlohy pro části

V tomto okamžiku jste prohlédli výhod vnořené hlavní stránky a viděli, jak vytvořit a použít ji v aplikaci ASP.NET. V příkladech v krocích 1, 2 a 3, ale podílejí vytváření novou stránku předlohy nejvyšší úrovně, nové vnořené hlavní stránky a nové stránky obsahu. Co o přidání nové vnořené hlavní stránky na web s existující nejvyšší úrovně stránky předlohy a stránky obsahu?

Vnořené hlavní stránky integraci do stávajícího webu a jeho přidružením k existující stránky obsahu vyžaduje trochu další úsilí než od začátku. Kroky 4, 5, 6 a 7 prozkoumat tyto problémy, jako jsme posílení naše ukázkové aplikace, aby zahrnovala nové vnořené hlavní stránky s názvem `AdminNested.master` , obsahuje pokyny pro správce a je používána stránky ASP.NET v `~/Admin` složky.

Integrace vnořené hlavní stránky do naší ukázkovou aplikaci zavádí následující mezní hodnoty:

- Existující obsah stránky v `~/Admin` složky mají určité očekávání z jejich stránky předlohy. Pro začátek očekávat určité ovládacích prvků ContentPlaceHolder nacházet. Kromě toho `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky volat veřejné stránky předlohy `RefreshRecentProductsGrid` nastavena metoda, jeho `GridMessageText` vlastnost, nebo mají obslužné rutiny události pro jeho `PricesDoubled` událostí. V důsledku toho naše vnořené hlavní stránky musíte zadat stejné ContentPlaceHolders a veřejné členy.
- V předchozím kurzu jsme rozšířené `BasePage` třída dynamicky nastavit `Page` objektu `MasterPageFile` vlastnost podle proměnné relace. Jak na podporujeme dynamické stránky předlohy při použití vnořené hlavní stránky?

Tyto dva problémy budou surface jako jsme vytvoření vnořené hlavní stránky a použít ho z existujících stránek s obsahem. Jsme budete prozkoumat a překonat tyto problémy při jejich vzniku.

## <a name="step-4-creating-the-nested-master-page"></a>Krok 4: Vytvoření vnořené stránky předlohy

Naše první krokem je vytvoření vnořené hlavní stránky má být používána stránky v části Správa. Jak jsme viděli v kroku 2, při přidání nového vnořené hlavní stránky, je potřeba zadat vnořené hlavní stránky nadřazená stránka předlohy. Dva hlavní stránky nejvyšší úrovně, ale: `Site.master` a `Alternate.master`. Odvolání, který jsme vytvořili `Alternate.master` v předchozí kurzu a napsali v kódu `BasePage` třídu, která nastavení stránky objektu `MasterPageFile` vlastnost za běhu, aby buď `Site.master` nebo `Alternate.master` v závislosti na hodnotě `MyMasterPage` Proměnné relace.

Jak jsme nakonfigurovat naše vnořené hlavní stránky tak, aby používala na příslušnou stránku nejvyšší úrovně hlavní? Máme dvě možnosti:

- Vytvořte dva vnořené hlavní stránky, `AdminNestedSite.master` a `AdminNestedAlternate.master`a navázat je na nejvyšší úrovni hlavní stránky `Site.master` a `Alternate.master`, v uvedeném pořadí. V `BasePage`, pak doporučujeme nastavit `Page` objektu `MasterPageFile` na příslušnou vnořené hlavní stránku.
- Vytvoření jednoho vnořené hlavní stránky a mít stránky obsahu používat tento konkrétní stránky předlohy. Poté, v době běhu by potřebujeme nastavení vnořené hlavní stránky `MasterPageFile` vlastnosti k odpovídající nejvyšší úrovně hlavní stránce za běhu. (Jak je nyní, může být započítáno, hlavní stránky také mít `MasterPageFile` vlastnost.)

Umožňuje použít druhou možnost. Vytvoření jednoho vnořené hlavní stránky souboru v `~/Admin` složku s názvem `AdminNested.master`. Protože obě `Site.master` a `Alternate.master` mají stejnou sadu ovládacích prvků ContentPlaceHolder, nezávisle na tom, jaké stránky předlohy, můžete vázat, i když I doporučujeme vytvořte mu vazbu k `Site.master` pro saké na konzistence.


[![Přidejte do složky ~/Admin vnořené stránky předlohy.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Obrázek 09**: přidejte vnořené hlavní stránky `~/Admin` složky. ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image27.png))


Protože vnořené hlavní stránky je vázána na hlavní stránku s čtyři ovládací prvky ContentPlaceHolder, Visual Studio. přidá čtyři ovládací prvky pro počáteční značka nový soubor vnořené hlavní stránky obsahu. Jako jsme provedli v kroku 2 a 3, přidání ovládacího prvku ContentPlaceHolder v každý ovládací prvek předá stejným názvem jako má prvek ContentPlaceHolder nejvyšší úrovně stránky předlohy. Také přidejte následující kód do ovládacího prvku obsahu, která odpovídá `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

V dalším kroku definovat `instructions` v třídy CSS `Styles.css` a `AlternateStyles.css` soubory šablon stylů CSS. Následující pravidla šablon stylů CSS způsobit elementů HTML stylem `instructions` třída zobrazuje barvu pozadí světla žlutý a černá, plnou ohraničení:


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Protože tyto značky se přidal do vnořené hlavní stránky, se zobrazí pouze v těchto stránek, které pomocí této vnořené hlavní stránky (konkrétně, stránky v části Správa).

Po provedení těchto dodatky na hlavní stránku vnořené, jeho deklarativní značek by měl vypadat takto:


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Všimněte si, že každý ovládací prvek má prvek ContentPlaceHolder a že ovládací prvky ContentPlaceHolder `ID` vlastnosti přiřazené stejné hodnoty jako odpovídající ovládacích prvků ContentPlaceHolder na hlavní stránce nejvyšší úrovně. Kromě toho se zobrazí v části konkrétní značku správy `MainContent` ContentPlaceHolder.

Obrázek 10 ukazuje `AdminNested.master` vnořené hlavní stránky při zobrazení v návrháři Visual Studio. Zobrazí se pokyny žlutý pole v horní části `MainContent` obsahu ovládacího prvku.


[![Vnořené stránky předlohy rozšiřuje nejvyšší úrovně stránky předlohy zahrnout pokyny pro správce.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Obrázek 10**: vnořené stránky předlohy rozšiřuje nejvyšší úrovně stránky předlohy zahrnout pokyny pro správce. ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Krok 5: Aktualizace existující stránky obsahu používat nové vnořené stránky předlohy

Kdykoliv přidáme do části Správa musíme vytvořte mu vazbu k nové stránky obsahu `AdminNested.master` stránky předlohy jsme právě vytvořili. Ale co o existující stránky obsahu? V současné době jsou odvozeny od všechny stránky obsahu v lokalitě `BasePage` třídy, která prostřednictvím kódu programu nastaví hlavní stránky obsahu stránce za běhu. Nejedná se o chování, které má být obsah stránky v části Správa. Místo toho chceme, aby tyto stránek obsahu vždy nutné použít `AdminNested.master` stránky. Je zodpovědností vnořené hlavní stránky zvolte právo nejvyšší úrovně obsahu stránce za běhu.

Nejlepší způsob, jak dosáhnout to požadovaných chování je vytvořte novou třídu vlastní základní stránky s názvem `AdminBasePage` který rozšiřuje `BasePage` třídy. `AdminBasePage`potom můžete přepsat `SetMasterPageFile` a nastavte `Page` objektu `MasterPageFile` na hodnotu pevně "~ / Admin/AdminNested.master". Tímto způsobem žádné stránce, která je odvozena od `AdminBasePage` použije `AdminNested.master`, zatímco žádné stránce, která je odvozena od `BasePage` bude mít jeho `MasterPageFile` vlastnost nastavit dynamicky buď "~ / Site.master" nebo "~ / Alternate.master" založené na hodnotu `MyMasterPage` Proměnné relace.

Začněte přidáním nového souboru třídy `App_Code` složku s názvem `AdminBasePage.cs`. Mít `AdminBasePage` rozšířit `BasePage` a potom přepsáním `SetMasterPageFile` metoda. V dané metody přiřadit `MasterPageFile` hodnota "~ / Admin/AdminNested.master". Po provedení těchto změn třídě soubor by měl vypadat podobně jako následující:


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

Nyní potřebujeme mít existující stránky obsahu v části Správa části odvozena od `AdminBasePage` místo `BasePage`. Přejděte na soubor třídy kódu pro každé obsahu stránce v `~/Admin` složky a tuto změnu provést. Například v `~/Admin/Default.aspx` by způsobila změnu deklaraci třídy kódu z:


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

Do:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Znázorňuje obrázek 11 jak nejvyšší úrovně stránky předlohy (`Site.master` nebo `Alternate.master`), vnořené hlavní stránky (`AdminNested.master`), a na stránkách obsahu části správy souvisí.


[![Vnořené stránky předlohy definuje obsahu specifické pro stránky v části Správa](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Obrázek 11**: vnořené hlavní stránky definuje obsahu specifické pro stránky v části Správa ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Krok 6: Zrcadlení veřejné metody a vlastnosti stránky předlohy

Odvolat, který `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky komunikovat prostřednictvím kódu programu se stránkou předlohy: `~/Admin/AddProduct.aspx` volání stránky předlohy je veřejný `RefreshRecentProductsGrid` metoda a nastaví její `GridMessageText` vlastnost; `~/Admin/Products.aspx` má obslužné rutiny události pro `PricesDoubled` událostí. V předchozím kurzu jsme vytvořili abstraktní `BaseMasterPage` třídu, která definována tyto veřejné členy.

`~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky předpokládá, že jejich hlavní stránka odvozuje od `BaseMasterPage` třídy. `AdminNested.master` Stránky, ale aktuálně rozšiřuje `System.Web.UI.MasterPage` třídy. V důsledku toho při návštěvě `~/Admin/Products.aspx` `InvalidCastException` je vyvolána se zprávou: "nelze vrátit objekt typu ' ASP.admin\_adminnested\_hlavní ' na typ 'BaseMasterPage'."

Chcete-li tomu je potřeba mít `AdminNested.master` rozšířit třídu kódem v pozadí `BaseMasterPage`. Aktualizujte deklaraci třídy kódu vnořené hlavní stránky z:


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

Do:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Máme ještě nejsou Hotovo. Protože `BaseMasterPage` je abstraktní třída, musíme přepsat `abstract` členy, `RefreshRecentProductsGrid` a `GridMessageText`. Tito členové jsou používány nejvyšší úrovně hlavní stránky k aktualizaci jejich uživatelské rozhraní. (Ve skutečnosti, jen `Site.master` stránky předlohy používá tyto metody, i když obě nejvyšší úrovně hlavní stránky implementovat tyto metody vzhledem k tomu, jak rozšířit `BaseMasterPage`.)

Při musíme implementovat tito členové v `AdminNested.master`, tyto implementace udělat stačí jednoduše volání stejného člena v nejvyšší úrovně stránky předlohy používané Vnořené stránky předlohy. Například když obsahu stránce v části Správa volání vnořené hlavní stránky `RefreshRecentProductsGrid` metodou, všechny vnořené hlavní stránky je potřeba udělat, je, pak, volejte `Site.master` nebo `Alternate.master`na `RefreshRecentProductsGrid` metoda.

Jak toho docílit, začněte přidáním následující `@MasterType` direktivy do horní části `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Odvolat, který `@MasterType` – direktiva přidá vlastnost silně typované třídy kódu s názvem `Master`. Potom přepsat `RefreshRecentProductsGrid` a `GridMessageText` členy a jednoduše delegovat volání `Master`je odpovídající metodu:


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

S tímto kódem na místě byste měli navštívit a používat stránky obsahu v části Správa. Obrázek 12 znázorňuje `~/Admin/Products.aspx` v případě, že zobrazit pomocí prohlížeče. Jak vidíte, stránka obsahuje pole pokyny správy, která je definována v vnořené stránky předlohy.


[![Obsahu stránky v části Správa obsahují pokyny v horní části každé stránce](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Obrázek 12**: obsahu stránky v pokyny zahrnují správu část v horní části každé stránky ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Krok 7: Pomocí příslušné nejvyšší úrovně stránky předlohy za běhu

Všechny stránky obsahu v části Správa jsou plně funkční, všechny používají stejný hlavní stránku nejvyšší úrovně a Ignorovat vybraný uživatelem na hlavní stránce `ChooseMasterPage.aspx`. Toto chování je skutečnost, že má vnořené hlavní stránky jeho `MasterPageFile` vlastnost staticky nastavena na hodnotu `Site.master` v jeho `<%@ Master %>` – direktiva.

Použít hlavní stránku nejvyšší úrovně, vybrané koncovým uživatelem, je potřeba nastavit `AdminNested.master`na `MasterPageFile` vlastnost na hodnotu v `MyMasterPage` proměnné relace. Vzhledem k tomu, že nastaví stránky obsahu `MasterPageFile` vlastnosti v `BasePage`, si myslíte, že doporučujeme nastavit vnořené hlavní stránky `MasterPageFile` vlastnost v `BaseMasterPage` nebo v `AdminNested.master`na třídu kódem v pozadí. Tato funkce nebude pracovat, ale protože je potřeba mít `MasterPageFile` vlastnost na konci fázi PreInit. Nejdřívější čas, který jsme prostřednictvím kódu programu můžou klepnout do životního cyklu stránky ze stránky předlohy je fázi Init (které dochází po fázi PreInit).

Proto je potřeba nastavit vnořené hlavní stránky `MasterPageFile` vlastnosti ze stránky obsahu. Jediný obsah stránky, které používají `AdminNested.master` stránky předlohy odvozena od `AdminBasePage`. Proto jsme můžou tuto logiku existuje. V kroku 5 jsme overrode `SetMasterPageFile` metodu, nastavení `Page` objektu `MasterPageFile` vlastnost "~ / Admin/AdminNested.master". Aktualizace `SetMasterPageFile` také nastavit stránky předlohy `MasterPageFile` vlastnost výsledek uložený v relaci:


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

`GetMasterPageFileFromSession` Metodu, která jsme přidali do `BasePage` – třída v předchozím kurzu vrátí cestu k souboru příslušná stránka předlohy založená na hodnotě proměnné relace.

Díky této změně zavedené stránky předlohy výběru uživatele se přenesou do části Správa. Obrázek 13 ukazuje stejné stránce jako obrázek 12, ale po změně jejich výběr stránky předlohy pro uživatele `Alternate.master`.


[![Stránka vnořené správy používá nejvyšší úrovně stránky předlohy vybrané uživatelem.](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Obrázek 13**: stránka Správa vnořené používá nejvyšší úrovně hlavní stránky vybraný uživatelem ([Kliknutím zobrazit obrázek v plné velikosti](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Souhrn

Mnoho například jak obsahu stránky můžete vázat na stránku předlohy, je možné vytvořit vnořené hlavní stránky tak, že podřízená stránka předlohy vytvořit vazbu k nadřazené stránky předlohy. Podřízená stránka předlohy pro každý z jeho nadřazeného objektu ContentPlaceHolders; může definovat ovládací prvky obsahu potom ji můžete přidat vlastní ovládací prvky ContentPlaceHolder (stejně jako ostatní značek) na tyto ovládací prvky obsahu. Vnořené hlavní stránky jsou velmi užitečné v velkých webových aplikací, kde všechny stránky sdílet zastřešující vzhled a chování, ale některé části lokality vyžadovat jedinečné přizpůsobení.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vnořené stránky předlohy technologie ASP.NET](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Tipy pro vnořené hlavní stránky a VS 2005 návrhu](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 vnořené hlavní stránky podpory](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](specifying-the-master-page-programmatically-cs.md)
[další](creating-a-site-wide-layout-using-master-pages-vb.md)
