---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Vnořené hlavní stránky (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Ukazuje, jak vnořit do jiného jednu stránku předlohy.
ms.author: aspnetcontent
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e58eef87afce7d1b7e29445bdbf48baf5f65d3e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836259"
---
<a name="nested-master-pages-vb"></a>Vložené hlavní stránky (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Ukazuje, jak vnořit do jiného jednu stránku předlohy.


## <a name="introduction"></a>Úvod

V průběhu posledních 9 kurzů jsme viděli, jak implementovat rozložení platného pro celý web pomocí stránek předlohy. Řečeno v kostce hlavní stránky nám umožňují, vývojář definovat společné značky na stránce předlohy spolu s konkrétní oblasti, které je možné přizpůsobit na základě obsahu stránky obsahu stránky. Upravitelné oblasti; označení ovládacích prvků ContentPlaceHolder na hlavní stránce vlastní značky pro ovládací prvky ContentPlaceHolder jsou definovány na stránce obsahu prostřednictvím ovládacích prvků obsahu.

Stránky předlohy techniky, které jste doposud Prozkoumali jsme se skvěle hodí, pokud máte jedno rozložení použít v rámci celé lokality. Mnoho využití rozsáhlých webů však mít lokality rozložení, který je přizpůsobený v různých oddílů. Představte si třeba zdravotní péče aplikace nemocniční institucí, která zaměstnanci používají ke správě informací o pacientech, aktivity a fakturace. Může existovat tři typy webových stránek v této aplikaci:

- Pracovníci člen konkrétní stránky kde zaměstnanci můžou aktualizovat dostupnosti, zobrazit plány nebo žádost o dovolenou.
- Kde zaměstnanci umožňuje zobrazit nebo upravit informace o konkrétní pacienta pacienta konkrétní stránky.
- Specifické pro fakturaci stránky, kde zkontrolovat aktuální accountants deklarací identity, stavy a finanční sestavy.

Každé stránky může sdílet společný rozložení, jako je například nabídce v horní části nebo řady často používané odkazů v dolní části. Ale zaměstnance, pacienta – a stránek specifických pro fakturaci může být třeba přizpůsobit této obecné rozložení. Například možná stránky pro všechny zaměstnance konkrétní by měl obsahovat kalendář a úkoly seznamu zobrazuje dostupnost aktuálně přihlášeného uživatele a denní plán. Například všechny pacienta konkrétní stránky se musí zobrazit jméno, adresu a informací o pojištění pacienta, jehož informace se právě upravuje.

Je možné vytvořit tyto přizpůsobené rozložení pomocí *vnořené hlavní stránky*. Provádět výše popsaném scénáři bude začneme vytvořením stránku předlohy, která definované rozložení, nabídky a zápatí obsah webu pomocí prvků ContentPlaceHolder definování přizpůsobitelné oblastech. Vytvoříme by pak tři vnořené hlavní stránky, jeden pro každý typ webové stránky. Každá vnořená hlavní stránka byste definovali obsah mezi typem obsahu stránky, které používají stránky předlohy. Jinými slovy vnořené stránce předlohy pro pacienta konkrétní stránky obsahu bude zahrnovat značky a programovou logiku pro zobrazování informací o pacienta, který právě upravujete. Při vytvoření nového pacienta konkrétní stránky jsme by ho vytvořit vazbu k této vnořené hlavní stránky.

V tomto kurzu se spustí zvýraznění výhody vložené hlavní stránky. Následně ukazuje, jak vytvořit a používat vložené hlavní stránky.

> [!NOTE]
> Od verze rozhraní .NET Framework verze 2.0 bylo umožněno vložené hlavní stránky. Visual Studio 2005 však nenabízely podporu návrhu pro vložené hlavní stránky. Dobrou zprávou je, že Visual Studio 2008 nabízí celou řadu možností návrhu pro vložené hlavní stránky. Pokud jste chtěli použít vnořené hlavní stránky, ale stále používají Visual Studio 2005, projděte si [Scott Guthrie](https://weblogs.asp.net/scottgu/)na blogu, [tipy pro vložené hlavní stránky v době návrhu VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Výhody vložené hlavní stránky

Počet webů mají zastřešujícího návrh lokality, jakož i více přizpůsobené návrhy specifické pro určité typy stránek. Například v naší ukázkové webové aplikaci vytvořili jsme základní části Administration (stránek `~/Admin` složky). Aktuálně webových stránek v `~/Admin` složky použijte stejnou stránku předlohy jako tyto stránky není v části administration (konkrétně `Site.master` nebo `Alternate.master`, v závislosti na výběru uživatele).

> [!NOTE]
> Prozatím předstírají, že, že náš web má jenom jednu stránku předlohy, `Site.master`. Řešíme pomocí vložené hlavní stránky pomocí dvou (nebo více) stránky předlohy počínaje "Pomocí vnořené hlavní stránky pro the správu oddíl" dále v tomto kurzu.


Představte si, že jsme byly požádáni o přizpůsobení rozložení stránky správy, které obsahují další informace a odkazy, které by jinak k dispozici na jiných stránkách na webu. Existují čtyři metody k implementaci tohoto požadavku:

1. Ručně přidat odkazy a informace specifické pro správu na všechny stránky obsahu `~/Admin` složky.
2. Aktualizace `Site.master` o tom, jestli některý stránky pro správu přístupu na základě stránku předlohy, která obsahují informace specifické pro oddíl správy a odkazy a poté přidejte kód k hlavní stránce můžete zobrazit nebo skrýt těchto oddílů.
3. Vytvořit novou stránku předlohy speciálně pro části Správa zkopírovat kód z `Site.master`, přidejte odkazy a informace specifické pro oddíl správy a pak aktualizujte obsah stránky v `~/Admin` složku, kterou chcete použít tento nový vzor stránka.
4. Vytvoření vnořené stránce předlohy s vazbou na `Site.master` a obsahu stránek v `~/Admin` složky použijte tento nový vnořené hlavní stránky. Tato vnořená hlavní stránka bude zahrnovat jenom Další informace a odkazy, které jsou specifické pro stránky pro správu a nebude muset opakovat kódu již definována v `Site.master`.

První možnost je nejméně palatable. Smyslem pomocí stránek předloh je přestat používat museli ručně zkopírujte a vložte společné značky na nové stránky technologie ASP.NET. Druhou možností je přijatelná, ale díky menší údržba je bulks nahoru stránky předlohy s kód, který se zobrazí jenom občas a vyžaduje vývojáři úpravách stránky předlohy, chcete-li tento kód a mít na paměti, kdy aplikace přesně některé poznámky se zobrazí a když je skrytý. Tento přístup může být méně chráněném jako vlastní nastavení z více typů museli být obslouženo této jedné stránky předlohy webových stránek.

Třetí možnost odebere zbytečné prvky a problémy s druhou možností surfaced složitost. Možnost pro tři hlavní nevýhodou však je, že vyžaduje nám zkopírovat a vložit Obecné rozložení z `Site.master` na novou stránku předlohy konkrétní části Správa. Pokud později rozhodneme změnit rozložení pro celý web máme, nezapomeňte změnit na dvou místech.

Čtvrtá možnost vnořené hlavní stránky a sdělte nám to nejlepší z druhé a třetí možnost. Informace o rozložení pro celý web se udržuje v jednom souboru – hlavní stránky nejvyšší úrovně – zatímco specifické pro konkrétní oblasti obsahu je rozdělen do různých souborech.

Tento kurz pracuje s pohled na vytváření a používání jednoduchých vnořené stránce předlohy. Vytvoříme zcela nový hlavní stránku nejvyšší úrovně, dvě vnořené hlavní stránky a dvě obsahu stránky. Počínaje "Pomocí vnořené hlavní stránky pro the správu oddíl", podíváme na aktualizaci naší architektury existující stránky předlohy zahrnout použití vložené hlavní stránky. Konkrétně jsme vytvoření vnořené hlavní stránky a umožňuje přidat další vlastní obsah stránky obsahu `~/Admin` složky.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Krok 1: Vytvoření jednoduché nejvyšší úrovně stránky předlohy

Vytvoření vnořené hlavní na základě jedné z existující hlavní stránky a následně aktualizovat existující stránku obsahu, aby pomocí této nové vnořené hlavní stránky místo hlavní stránky nejvyšší úrovně zahrnuje některé složitost, protože existující stránky obsahu již očekávat určité Ovládací prvky ContentPlaceHolder definovány na hlavní stránce nejvyšší úrovně. Vnořená hlavní stránka proto musí obsahovat stejné ovládací prvky ContentPlaceHolder se stejnými názvy. Kromě toho naše konkrétní ukázkové aplikace má dvě hlavní stránky (`Site.master` a `Alternate.master`), která se dynamicky přiřadí k obsahu stránky na základě zálib uživatelů, která přidá další Tato složitost. Aktualizuje se existující aplikaci, aby používala vložené hlavní stránky později v tomto kurzu se podíváme, ale můžeme vnořené první se zaměřují na jednoduchý příklad stránky předlohy.

Vytvořte novou složku s názvem `NestedMasterPages` a pak přidejte nový soubor předlohové stránky do této složky s názvem `Simple.master`. (Viz obrázek 1 pro snímek obrazovky Průzkumníka řešení po přidání této složky a souboru.) Přetáhněte `AlternateStyles.css` soubor stylů z Průzkumníka řešení do návrháře. Tento postup přidá `<link>` elementu do souboru list stylu v `<head>` elementu, po jejímž uplynutí stránky předlohy `<head>` elementu značek by měl vypadat takto:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

V dalším kroku přidejte následující kód v rámci webového formuláře z `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Tento kód zobrazí odkaz s názvem "Vnořené hlavní stránky (jednoduchý)" v horní části stránky velkými písmeny bílé na navy na pozadí. Pod, který je `MainContent` ContentPlaceHolder. Obrázek 1 ukazuje `Simple.master` stránky předlohy, když se načte v návrháři Visual Studio.


[![Vnořená hlavní stránka definuje konkrétní obsahu do stránky v části Správa](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Obrázek 01**: The vnořené hlavní stránky definuje obsahu specifické pro stránky v části Administration ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Krok 2: Vytvoření jednoduché vnořené hlavní stránky

`Simple.master` obsahuje dva ovládací prvky ContentPlaceHolder: `MainContent` ContentPlaceHolder jsme přidali v rámci webového formuláře spolu s `head` prvku ContentPlaceHolder `<head>` elementu. Pokud bychom měli vytvořit stránku obsahu a vytvořte mu vazbu k `Simple.master` stránky obsahu bude mít dva ovládací prvky obsahu odkazující na dvou prvků ContentPlaceHolder. Podobně pokud vytvoříme vnořené hlavní stránky a vytvořte mu vazbu k `Simple.master` vnořená hlavní stránka bude mít dva ovládací prvky obsahu.

Přidejme novou vnořené stránce předlohy k `NestedMasterPages` složku s názvem `SimpleNested.master`. Klikněte pravým tlačítkem na `NestedMasterPages` složky a zvolte Přidat novou položku. Otevře dialogové okno Přidat novou položku je znázorněno na obrázku 2. Vyberte typ šablony stránky předlohy a zadejte název nové stránky předlohy. K označení, že nové stránky předlohy by měl být vnořené stránce předlohy, zaškrtněte políčko "Vyberte stránka předlohy".

Potom klikněte na tlačítko Přidat. Zobrazí se stejné vyberte stránku předlohy dialogové okno se zobrazí při vytváření vazby obsahu stránky na stránku předlohy (viz obrázek 3). Zvolte `Simple.master` stránky předlohy v `NestedMasterPages` složky a klikněte na tlačítko OK.

> [!NOTE]
> Pokud jste vytvořili pomocí modelu projektu webové aplikace namísto modelu projektu webové stránky webu ASP.NET neuvidíte zaškrtávací políčko "Vybrat hlavní stránku" v dialogovém okně Přidat novou položku je znázorněno na obrázku 2. Vytvoření vnořené stránce předlohy při použití modelu projektu webové aplikace musíte zvolit šablonu vnořená hlavní stránka (a nikoli hlavní stránku šablony projektu). Po výběrem vnořenou hlavní stránku šablony a kliknutím na Přidat, vyberte stejný hlavní stránky se zobrazí dialogové okno je znázorněno na obrázku 3.


[![Zkontrolujte, &quot;vybrat hlavní stránku&quot; zaškrtávací políčko a přidáním vnořenou hlavní stránku](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Obrázek 02**: Zaškrtnutím políčka "Vybrat hlavní stránku" přidat vnořenou hlavní stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image6.png))


[![Vnořená hlavní stránka svázat Simple.master hlavní stránky](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Obrázek 03**: vnořenou hlavní stránku k vytvoření vazby `Simple.master` stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image9.png))


Vnořená hlavní stránka deklarativní, ukazuje následující příklad obsahuje dva ovládací prvky obsahu odkazující na nejvyšší úrovni stránky předlohy dvou ovládacích prvků ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

S výjimkou `<%@ Master %>` direktiv, deklarativní počáteční vnořená hlavní stránka je stejný jako kód, který je původně vytvořil při vytváření vazby stránku obsahu na stejnou stránku předlohy nejvyšší úrovně. Stránky obsahu, jako jsou `<%@ Page %>` směrnice, `<%@ Master %>` obsahuje direktivy tady `MasterPageFile` atribut, který určuje vnořené stránce předlohy nadřazené stránky předlohy. Hlavní rozdíl mezi vnořené stránce předlohy a stránky obsahu vázán na stejnou stránku předlohy nejvyšší úrovně je, že vnořená hlavní stránka může obsahovat ovládací prvky ContentPlaceHolder. Ovládací prvky ContentPlaceHolder vnořená hlavní stránka definovat v oblastech, kde obsahu stránky můžete upravit značky.

Aktualizace této vnořené hlavní stránky tak, aby zobrazil text "Hello, z SimpleNested!" v ovládacím prvku obsahu, který odpovídá `MainContent` prvek ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Po provedení tohoto sčítání, uložení vnořené hlavní stránky a pak přidejte novou stránku obsahu, aby `NestedMasterPages` složku s názvem `Default.aspx`a vytvořte mu vazbu k `SimpleNested.master` stránky předlohy. Při přidání tuto stránku budete překvapení, pokud chcete zobrazit, že neobsahuje žádné ovládací prvky obsahu, (viz obrázek 4)! Stránka obsahu přístup jenom k jeho *nadřazené* prvků ContentPlaceHolder na stránce předlohy. `SimpleNested.master` neobsahuje žádné ovládací prvky ContentPlaceHolder; stránka obsahu vázán na tuto stránku předlohy proto nemůže obsahovat žádné ovládací prvky obsahu.


[![Nová stránka obsahu neobsahuje žádné ovládací prvky obsahu](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Obrázek 04**: The nového obsahu stránky obsahuje Ne ovládacích prvků obsahu ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image12.png))


Co musíme udělat je aktualizovat vnořené stránce předlohy (`SimpleNested.master`) Chcete-li přidat ovládací prvky ContentPlaceHolder. Obvykle je vhodné vaše vnořené hlavní stránky zahrnout ContentPlaceHolder pro každý ContentPlaceHolder určené její nadřazené stránky předlohy a tím umožní její podřízené stránky předlohy nebo stránku obsahu a spolupracovat s kterýmkoli ContentPlaceHolder nejvyšší úrovně stránky předlohy ovládací prvky.

Aktualizace `SimpleNested.master` stránky předlohy mají být zahrnuty ContentPlaceHolder jeho dvou obsahu ovládacích prvků. Zadejte ovládací prvky ContentPlaceHolder stejný název jako ovládací prvek ContentPlaceHolder, na který odkazuje prvek jejich obsahu. To znamená, přidejte ovládací prvek ContentPlaceHolder s názvem `MainContent` k obsahu v ovládacím prvku `SimpleNested.master` odkazující `MainContent` prvku ContentPlaceHolder `Simple.master`. To samé udělá v ovládacím prvku obsahu, který odkazuje `head` ContentPlaceHolder.

> [!NOTE]
> Zatímco je můžu jenom doporučit názvy ovládacích prvků ContentPlaceHolder vnořené stránce předlohy, stejně jako prvků ContentPlaceHolder na stránce předlohy nejvyšší úrovně, toto pojmenování symetrie se nevyžaduje. Ovládací prvky ContentPlaceHolder můžete dát vaše vnořené stránce předlohy libovolný název, který rádi používáte. Ale můžu jednodušší mějte na paměti prvků ContentPlaceHolder nekorespondují s jaké oblasti na stránce Moje hlavní stránky nejvyšší úrovně a vložené hlavní stránky použití stejné názvy.


Po provedení tyto doplňky vaší `SimpleNested.master` deklarativním označení stránky předlohy by měl vypadat nějak takto:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Odstranit `Default.aspx` obsahu stránky, které jsme právě vytvořili a pak jej znovu přidat, jeho pro vazbu `SimpleNested.master` stránky předlohy. Tentokrát Visual Studio přidá dvě pro ovládací prvky obsahu `Default.aspx`, odkazující prvků ContentPlaceHolder nyní definována ve `SimpleNested.master` (viz obrázek 6). Přidejte text "Hello, z Default.aspx!" v obsahu ovládací prvek, na který odkazuje `MainContent`.

Obrázek 5 ukazuje tři entity podílejí zde - `Simple.master`, `SimpleNested.master`, a `Default.aspx` – a jejich vzájemných vztazích. Protože diagram znázorňuje, vnořená hlavní stránka implementuje ovládacích prvků obsahu pro ContentPlaceHolder jeho nadřazeného objektu. Pokud tyto oblasti musí být přístupné na stránku obsahu, musí vnořené stránce předlohy přidejte vlastní prvků ContentPlaceHolder na ovládací prvky obsahu.


[![Na stránkách nejvyšší úrovně a vnořené hlavní diktování rozložení obsahu stránky](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Obrázek 05**: nejvyšší úrovně a vnořené hlavní stránky diktování obsahu stránky rozložení ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image15.png))


Toto chování ukazuje, jak stránky předlohy a stránky obsahu se pouze cognizant z nadřazené stránky předlohy. Toto chování je také označena Návrhář Visual Studio. Návrhář pro znázorňuje obrázek 6 `Default.aspx`. Zatímco návrháře jasně ukazuje, jaké oblasti se upravovat ze stránky obsahu a co není částí, není to rozlišení jako neupravovatelné oblastech jsou z vnořené hlavní stránky a oblastí se z nejvyšší úrovně stránky předlohy.


[![Obsah stránky teď obsahuje ovládací prvky obsahu pro prvků ContentPlaceHolder vnořená hlavní stránka](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Obrázek 06**: The obsahu stránky teď obsahuje ovládací prvky obsahu pro prvků ContentPlaceHolder vnořené hlavní stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Krok 3: Přidání druhé jednoduché vnořené hlavní stránky

Výhodou vložené hlavní stránky je více zřejmé, pokud existuje více vložené hlavní stránky. Pro ilustraci tuto výhodu, vytvořte jiný vnořená hlavní stránka v `NestedMasterPages` složka; Tento nový název vnořené hlavní stránky `SimpleNestedAlternate.master` a vytvořte mu vazbu k `Simple.master` stránky předlohy. Přidání ovládacích prvků ContentPlaceHolder ve vnořené stránce předlohy ovládacích prvků obsahu dvě, jak jsme to udělali v kroku 2. Také přidáte text "Hello, z SimpleNestedAlternate!" v ovládacím prvku obsahu, který odpovídá nejvyšší úrovně stránky předlohy `MainContent` ContentPlaceHolder. Po provedení těchto změn deklarativní vaší nové vnořené hlavní stránky by měl vypadat nějak takto:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Vytvoření stránky obsahu s názvem `Alternate.aspx` v `NestedMasterPages` složky a vytvořte mu vazbu k `SimpleNestedAlternate.master` vnořené stránce předlohy. Přidejte text "Hello, z alternativní!" v ovládacím prvku obsahu, který odpovídá `MainContent`. Obrázek 7 znázorňuje `Alternate.aspx` při prohlížení prostřednictvím návrháře Visual Studio.


[![Alternate.aspx je vázán na stránce předlohy SimpleNestedAlternate.master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Obrázek 07**: `Alternate.aspx` je vázán `SimpleNestedAlternate.master` stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image21.png))


Porovnejte návrháře na obrázku 7 do návrháře na obrázku 6. Obě obsahu stránky sdílet stejné rozložení, které jsou definovány na hlavní stránce nejvyšší úrovně (`Simple.master`), konkrétně název "Vnořené hlavní stránky kurzu (jednoduchý)". Ještě mají odlišné obsahem definován v jejich nadřazené hlavní stránky - text "Hello, z SimpleNested!" Obrázek 6 a "Hello, z SimpleNestedAlternate!" na obrázku 7. Udělena, zde tyto rozdíly jsou jednoduché, ale můžete rozšířit tohoto příkladu a zahrnují lépe vystihuje rozdíly. Například `SimpleNested.master` stránka může obsahovat nabídka s možnostmi, které jsou specifické pro jeho obsahu stránky, zatímco `SimpleNestedAlternate.master` může mít informace, které jsou relevantní pro stránky obsahu, které spojit.

Nyní Představte si, že jsme potřebovali měnit rozložení zastřešujícího webu. Představte si například, že jsme chtěli přidat seznam běžných odkazy na všechny stránky obsahu. K tomu budeme aktualizovat hlavní stránky nejvyšší úrovně `Simple.master`. Všechny změny se okamžitě projeví v jeho vnořené hlavní stránky a při rozšíření i pro jejich obsahu stránky.

Abychom si předvedli snadné, pomocí kterého můžete změnit zastřešujícího rozložení webu, otevřete `Simple.master` stránku předlohy a přidejte následující kód mezi `topContent` a `mainContent` `<div>` prvky:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Tento postup přidá dva odkazy do horní části každé stránky, která vytvoří vazbu na `Simple.master`, `SimpleNested.master`, nebo `SimpleNestedAlternate.master`; tyto změny se aplikují na všechny vnořené hlavní stránky a jejich obsahu stránky okamžitě. Obrázek 8 ukazuje `Alternate.aspx` při prohlížení prostřednictvím prohlížeče. Poznámka: přidávání odkazů v horní části stránky (ve srovnání se obrázek 7).


[![Změnit na stránce předlohy nejvyšší úrovně se okamžitě projeví v jeho vnořené hlavní stránky a jejich obsahu stránky](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Obrázek 08**: změnit na stránce předlohy nejvyšší úrovně se okamžitě projeví v jeho vnořené hlavní stránky a jejich obsahu stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Použití vnořená hlavní stránka pro části Správa

V tomto okamžiku se podívat na výhody vnořené hlavní stránky a viděli, jak vytvořit a použít je v aplikaci technologie ASP.NET. V příkladech v krocích 1, 2 a 3, ale součástí vytváření nové hlavní stránku nejvyšší úrovně, nové vložené hlavní stránky a nové stránky obsahu. A co přidání nové vnořené stránce předlohy na web s existující nejvyšší úrovně stránky předlohy a stránky obsahu?

Vnořená hlavní stránka integrace do existujícího webu a jeho přidružením k existující stránky obsahu vyžaduje trochu větší úsilí než vytváříte od začátku. Kroky 4, 5, 6 a 7 prozkoumat tyto problémy, jako jsme posílení naší ukázkové aplikaci a vložte nový vnořená hlavní stránka s názvem `AdminNested.master` , který obsahuje pokyny pro správce a používá na stránkách ASP.NET `~/Admin` složky.

Vnořená hlavní stránka integrace do naší ukázkovou aplikaci zavádí následující mezní hodnoty:

- Existující obsah stránky v `~/Admin` složky mají určité očekávání z jejich stránky předlohy. Pokud začínáte očekávají některé ovládací prvky ContentPlaceHolder nacházet. Kromě toho `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky volat na hlavní stránce veřejného `RefreshRecentProductsGrid` metody, nastavte jeho `GridMessageText` vlastnost, nebo mají obslužné rutiny události pro jeho `PricesDoubled` událostí. V důsledku toho naše vnořené stránce předlohy musíte zadat stejné prvků ContentPlaceHolder a veřejné členy.
- V předchozím kurzu jsme vylepšili `BasePage` třídy dynamicky nastavovat `Page` objektu `MasterPageFile` nastavenou na proměnné relace. Jak do podporujeme dynamické stránky předlohy při použití vložené hlavní stránky?

Tyto dvě výzvy bude přinášet vnořená hlavní stránka sestavení a použít ji z existujících stránek s obsahem. Vytvoříme prozkoumat a tyto problémy překonat, jakmile nějaké nastanou.

## <a name="step-4-creating-the-nested-master-page"></a>Krok 4: Vytvoření vnořené stránce předlohy

Naše první úloha je vytvoření vnořené stránce předlohy používané stránky v části Správa. Jak jsme viděli v kroku 2, při přidání nového vnořená hlavní stránka potřebujeme zadat vnořené stránce předlohy nadřazené stránky předlohy. Ale máme k dispozici dvě stránky předlohy nejvyšší úrovně: `Site.master` a `Alternate.master`. Vzpomínáte, kterou jsme vytvořili `Alternate.master` v předchozím kurzu a napsali kód v `BasePage` třídy této sady `Page` objektu `MasterPageFile` vlastnost za běhu, abyste buď `Site.master` nebo `Alternate.master` v závislosti na hodnotě `MyMasterPage` Proměnné relace.

Jak jsme nakonfigurovat naše vnořené stránce předlohy tak, aby používal odpovídající nejvyšší úrovně hlavní stránku? Máme dvě možnosti:

- Vytvořte dvě vnořené hlavní stránky, `AdminNestedSite.master` a `AdminNestedAlternate.master`a svázat ho s nejvyšší úrovně stránky předlohy `Site.master` a `Alternate.master`v uvedeném pořadí. V `BasePage`, pak doporučujeme nastavit `Page` objektu `MasterPageFile` k příslušné vnořené stránce předlohy.
- Vytvořte jeden vnořené stránce předlohy a obsahu stránky pomocí této konkrétní stránky předlohy. Pak za běhu, musíme nastavit vnořená hlavní stránka `MasterPageFile` vlastnost na příslušnou stránku nejvyšší úrovně hlavní za běhu. (Protože může mít naplánujete nyní, hlavní stránky mají navíc `MasterPageFile` vlastnosti.)

Můžeme použít druhou možnost. Vytvoření jednoho vnořené hlavní stránky souboru v `~/Admin` složku s názvem `AdminNested.master`. Protože obě `Site.master` a `Alternate.master` mají stejnou sadu ovládacích prvků ContentPlaceHolder, nebude vadit, jaké stránky předlohy, můžete svázat, i když neváhejte se vytvořte mu vazbu k `Site.master` pro saké společnosti konzistence.


[![Vnořená hlavní stránka přidáte do složky ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Obrázek 09**: vnořená hlavní stránka k přidání `~/Admin` složky. ([Kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image27.png))


Protože vnořená hlavní stránka je vázán na hlavní stránku s čtyři ovládací prvky ContentPlaceHolder, sada Visual Studio přidá čtyři ovládací prvky na počáteční značky nový soubor vnořené hlavní stránky obsahu. Jak jsme to udělali v krocích 2 a 3, přidejte ovládací prvek ContentPlaceHolder v každý ovládací prvek, předá se stejným názvem jako ovládací prvek ContentPlaceHolder nejvyšší úrovně hlavní stránky. Také přidejte následující kód do ovládacího prvku obsahu, který odpovídá `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Dále definujte `instructions` třídy šablony stylů CSS v `Styles.css` a `AlternateStyles.css` soubory šablon stylů CSS. Následující pravidla šablon stylů CSS způsobit elementů HTML stylem `instructions` třídy zobrazuje světle žlutá pozadí a ohraničení černého, plné:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Vzhledem k tomu, že tento kód je přidaný do vnořené stránce předlohy, se zobrazí pouze na těchto stránkách, které používají tento vnořené stránce předlohy (konkrétně stránky v části Administration).

Po provedení těchto dodatky k vaší vnořené stránce předlohy, jeho deklarativním označení by měl vypadat nějak takto:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Všimněte si, že každý ovládací prvek má ovládací prvek ContentPlaceHolder a, který ovládací prvek ContentPlaceHolder prvky `ID` vlastností jsou přiřazeny stejné hodnoty jako odpovídající ovládací prvky ContentPlaceHolder na stránce předlohy nejvyšší úrovně. Kromě toho se zobrazí v kódu konkrétní části Správa `MainContent` ContentPlaceHolder.

Obrázek 10 ukazuje `AdminNested.master` vnořené stránce předlohy při prohlížení prostřednictvím návrháře aplikace Visual Studio. Zobrazí se pokyny žlutá pole v horní části `MainContent` ovládacího prvku obsahu.


[![Vnořená hlavní stránka rozšiřuje nejvyšší úrovně stránky předlohy, aby zahrnovala pokyny pro správce.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Obrázek 10**: vnořená hlavní stránka rozšiřuje nejvyšší úrovně stránky předlohy, aby zahrnovala pokyny pro správce. ([Kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Krok 5: Aktualizuje se existující stránky obsahu používat nové vnořená hlavní stránka

Kdykoli můžeme přidat nové stránky obsahu do části Správa, musíme vytvořte mu vazbu k `AdminNested.master` hlavní stránky, které jsme právě vytvořili. Ale co stávající stránky obsahu? V současné době všechny stránky obsahu v lokalitě odvozovat `BasePage` třídu, která prostřednictvím kódu programu za běhu nastaví hlavní stránce stránky obsahu. Toto není chování, které chceme pro stránky obsahu v části Správa. Místo toho chceme, aby tyto stránky obsahu vždy používat `AdminNested.master` stránky. Je zodpovědností vnořené stránce předlohy zvolit obsahu stránky přímo nejvyšší úrovně v době běhu.

Nejlepší způsob, jak dosáhnout to požadovaného chování je vytvoření nové vlastní stránku základní třídu s názvem `AdminBasePage` , který rozšiřuje `BasePage` třídy. `AdminBasePage` potom můžete přepsat `SetMasterPageFile` a nastavit `Page` objektu `MasterPageFile` pevně zakódované hodnotu "~ / Admin/AdminNested.master". Tímto způsobem všechny stránky, která je odvozena z `AdminBasePage` použije `AdminNested.master`, zatímco jakékoli stránky, která je odvozena z `BasePage` bude mít jeho `MasterPageFile` nastavenou dynamicky buď "~ / Site.master" nebo "~ / Alternate.master" podle hodnoty `MyMasterPage` Proměnné relace.

Začněte přidáním nového souboru třídy `App_Code` složku s názvem `AdminBasePage.vb`. Mít `AdminBasePage` rozšířit `BasePage` a potom přepsat `SetMasterPageFile` metody. V této metodě přiřadit `MasterPageFile` hodnotu "~ / Admin/AdminNested.master". Po provedení těchto změn třídu soubor by měl vypadat nějak takto:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Nyní potřebujeme mít existující stránky obsahu ve správě jsou odvozeny z část `AdminBasePage` místo `BasePage`. Přejděte k souboru modelu code-behind třídy pro každou stránku obsahu `~/Admin` složky a provést tuto změnu. Například v `~/Admin/Default.aspx` byste změnili deklaraci třídy modelu code-behind od:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Do:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Znázorňuje obrázek 11 jak nejvyšší úrovně stránky předlohy (`Site.master` nebo `Alternate.master`), vnořené stránce předlohy (`AdminNested.master`), a na stránkách obsahu bodu správy k sobě vztahují.


[![Vnořená hlavní stránka definuje konkrétní obsahu do stránky v části Správa](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Obrázek 11**: The vnořené hlavní stránky definuje obsahu specifické pro stránky v části Administration ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Krok 6: Zrcadlení veřejné metody a vlastnosti stránky předlohy

Vzpomeňte si, že `~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky prostřednictvím kódu programu pracovat s hlavní stránkou: `~/Admin/AddProduct.aspx` volání na hlavní stránce vaší veřejné `RefreshRecentProductsGrid` metoda a nastaví jeho `GridMessageText` vlastnosti; `~/Admin/Products.aspx` má obslužná rutina události `PricesDoubled` událostí. V předchozím kurzu jsme vytvořili `MustInherit` `BaseMasterPage` třídu, která definované tyto veřejné členy.

`~/Admin/AddProduct.aspx` a `~/Admin/Products.aspx` stránky se předpokládá, že jejich hlavní stránka odvozuje od `BaseMasterPage` třídy. `AdminNested.master` Stránky, ale nyní rozšiřuje `System.Web.UI.MasterPage` třídy. V důsledku toho při návštěvě `~/Admin/Products.aspx` `InvalidCastException` je vyvolána výjimka s touto zprávou: "nelze přetypovat objekt typu" ASP.admin\_adminnested\_hlavní ' na typ "BaseMasterPage". "

Chcete-li vyřešit to budeme muset mít `AdminNested.master` rozšíření třídy modelu code-behind `BaseMasterPage`. Vnořená hlavní stránka použití modelu code-behind deklarace třídy z aktualizace:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Do:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Jsme ještě neskončili. Musíme přepsat členy označené jako `MustOverride`, jmenovitě `RefreshRecentProductsGrid` a `GridMessageText`. Tyto členy jsou používány nejvyšší úrovně stránky předlohy k aktualizaci jejich uživatelských rozhraní. (Ve skutečnosti, pouze `Site.master` stránky předlohy používat tyto metody, i když oba hlavní stránky nejvyšší úrovně implementovat tyto metody, protože obě rozšíření `BaseMasterPage`.)

Zatímco budeme muset implementovat tyto členy v `AdminNested.master`, tyto implementace provedete stačí jednoduše zavoláte na stejný člen v nejvyšší úrovni stránky předlohy používá vnořené stránce předlohy. Například když stránku obsahu v části Správa volá vnořené stránce předlohy `RefreshRecentProductsGrid` metoda, všechny vnořené stránce předlohy potřebuje pro výkon, pak je volání `Site.master` nebo `Alternate.master`společnosti `RefreshRecentProductsGrid` metody.

Můžete toho dosáhnout, začněte přidáním následujícího kódu `@MasterType` direktiv k hornímu okraji `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Vzpomeňte si, že `@MasterType` směrnice přidá k použití modelu code-behind třídu s názvem vlastnost silného typu `Master`. Potom přepsat `RefreshRecentProductsGrid` a `GridMessageText` členy a jednoduše delegovat volání `Master`odpovídající metodu:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

S tímto kódem na místě byste měli navštívit a použít na stránkách obsahu v části Správa. Obrázek 12 se zobrazí `~/Admin/Products.aspx` stránce při prohlížení prostřednictvím prohlížeče. Jak je vidět, stránka obsahuje pole pokyny správy, který je definován v vnořené stránce předlohy.


[![Obsah stránky v části Správa obsahují pokyny v horní části každé stránky](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Obrázek 12**: stránkách obsahu v pokyny zahrnují správu části v horní části každé stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Krok 7: Pomocí příslušné nejvyšší úrovně stránky předlohy se stránkou za běhu

Všechny stránky obsahu v části Správa jsou plně funkční, všechny používat stejné nejvyšší úrovni stránky předlohy a ignorovat stránky předlohy vybraných uživatelem v `ChooseMasterPage.aspx`. Toto chování je skutečnost, že má vnořená hlavní stránka jeho `MasterPageFile` staticky nastavenou na `Site.master` v jeho `<%@ Master %>` směrnice.

Použít hlavní stránky nejvyšší úrovně, vybráno koncovým uživatelem, musíme nastavit `AdminNested.master`společnosti `MasterPageFile` vlastnost na hodnotu v `MyMasterPage` proměnné relace. Vzhledem k tomu, že jsme si nastavili obsahu stránky `MasterPageFile` vlastnosti v `BasePage`, může si myslíte, že doporučujeme nastavit vnořená hlavní stránka `MasterPageFile` vlastnost v `BaseMasterPage` nebo v `AdminNested.master`na použití modelu code-behind třídy. Tato funkce nebude pracovat, ale protože musíme mít nastavená `MasterPageFile` vlastnost na konci fáze PreInit. Nejdřívější čas, který jsme můžete programově pronikněte do životního cyklu stránky ze stránky předlohy je fáze inicializace (které dojde po fázi PreInit).

Proto musíme nastavit vnořené stránce předlohy `MasterPageFile` vlastnost z obsahu stránky. Pouze obsah stránky, které používají `AdminNested.master` hlavní stránky odvozen od `AdminBasePage`. Proto jsme můžete tuto logiku umístit existuje. V kroku 5 jsme overrode `SetMasterPageFile` metoda nastavení objektu Page `MasterPageFile` vlastnost "~ / Admin/AdminNested.master". Aktualizace `SetMasterPageFile` také nastavit na hlavní stránce `MasterPageFile` vlastnost výsledek uložený v relaci:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession` Metody, které jsme přidali do `BasePage` třídy v předchozím kurzu, vrátí cestu k souboru příslušná stránka předlohy založena na hodnotě proměnné relace.

Díky této změně na místě hlavní stránky výběru uživatele se přenesou do části Správa. Obrázek 13 zobrazuje stejné stránce jako obrázek 12, ale po výběru stránky předlohy k uživateli `Alternate.master`.


[![Stránka vnořené správy používá nejvyšší úrovně stránky předlohy se stránkou vybraných uživatelem.](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Obrázek 13**: používá vnořené stránce pro správu nejvyšší úrovně hlavní stránky vybraných uživatelem ([kliknutím ji zobrazíte obrázek v plné velikosti](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Souhrn

Mnoho like jak obsahu stránky můžete vázat na stránku předlohy, je možné vytvořit vnořené hlavní stránky tak, že podřízené stránky předlohy připojení k nadřazené stránky předlohy. Podřízená stránka předlohy pro každý z prvků ContentPlaceHolder jeho nadřazeného objektu; může definovat ovládacích prvků obsahu potom ji můžete přidat vlastní ovládací prvky ContentPlaceHolder (stejně jako ostatní značky) do těchto ovládacích prvků obsahu. Vložené hlavní stránky jsou velmi užitečné ve velkých webových aplikací, kde všechny stránky sdílet zastřešujícího vzhled a chování, ale některé části webu jedinečný přizpůsobení.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vnořené stránek předloh ASP.NET](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Tipy pro vložené hlavní stránky a VS 2005 návrhu](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 vnořená hlavní stránka podpory](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](specifying-the-master-page-programmatically-vb.md)
