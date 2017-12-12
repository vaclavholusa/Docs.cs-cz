---
title: "Vytváření Krásný přizpůsobivý weby s Bootstrap"
author: ardalis
description: 
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bd27832c-2877-4b7b-9337-e009361d845f
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bootstrap
ms.openlocfilehash: f89ad584600c3f12a936599de27f931aff0cd4b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Vytváření Krásný přizpůsobivý weby s Bootstrap

<a name="bootstrap-index"></a>

Podle [Steve Smith](https://ardalis.com/)

Bootstrap je aktuálně nejoblíbenější webové rozhraní pro vývoj přizpůsobivý webových aplikací. Nabízí několik funkce a výhody, které může zvýšit vaši uživatelé zkušenosti s webu, ať už jste začínající na front-endu návrh a vývoj nebo odborníka. Bootstrap je nasazený jako sada souborů CSS a JavaScript a slouží k efektivní Nápověda škálování váš web nebo aplikaci z telefonů tablety pro stolní počítače.

## <a name="getting-started"></a>Začínáme

Existuje několik způsobů, jak začít s Bootstrap. Pokud začínáte novou webovou aplikaci v sadě Visual Studio, můžete výchozí šablona starter pro ASP.NET Core, ve kterém budou pocházet případu Bootstrap předem nainstalovaná:

![V zobrazení řešení šablona starter Bootstrap](bootstrap/_static/bootstrap-in-starter-template.png)

Přidání Bootstrap ASP.NET Core projektu se pouze záležitost jejím přidáním do *bower.json* jako závislost:

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Toto je doporučeným způsobem, jak přidat Bootstrap do projektu ASP.NET Core.

Můžete také nainstalovat bootstrap pomocí jedné z několika správce balíčku, například Bower, npm nebo NuGet. V každém případě proces je v podstatě stejný:

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> Doporučený způsob, jak nainstalovat klienta závislosti, jako je Bootstrap v ASP.NET Core prostřednictvím Bower (pomocí *bower.json*, jak je uvedeno výše). Použití npm/NuGet se zobrazují ukazují, jak snadno lze přidat Bootstrap na jiné typy webových aplikací, včetně starší verze technologie ASP.NET.

Pokud jste odkazující na vlastní místní verze Bootstrap, budete muset na ně odkazovat ve všech stránek, které budou používat. V produkčním prostředí by měl odkazovat bootstrap pomocí název CDN. Ve výchozí šablona webu ASP.NET *_Layout.cshtml* souboru tak, jako tento:

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Pokud se chystáte používat žádné z modulů plug-in jQuery Bootstrap společnosti, musíte se také tak, aby odkazovaly jQuery.

## <a name="basic-templates-and-features"></a>Základní šablony a funkce

Nejzákladnější Bootstrap šablony vypadá hodně podobá *_Layout.cshtml* souboru ukazuje vyšší a jednoduše obsahuje základní nabídky pro navigaci a místo pro vykreslení zbývající části stránky.

### <a name="basic-navigation"></a>Základní navigaci

Výchozí šablona používá sadu `<div>` elementy pro vykreslení horní navigační panel a hlavní část stránky. Pokud používáte HTML5, můžete nahradit první `<div>` značku s `<nav>` značky získat stejného efektu, ale s přesnější sémantiku.  V rámci této první `<div>` se zobrazí několik i další. První, `<div>` s třídou "kontejneru" a pak v rámci této, dva další `<div>` elementy: "navbar záhlaví" a "navbar sbalit".  Navigační panel záhlaví div obsahuje tlačítko, které se zobrazí, pokud je na obrazovce níže určité minimální šířky, zobrazující 3 vodorovné čáry (takzvané "hamburger ikonu"). Ikona je vykreslen pomocí čistý HTML a CSS; je vyžadován žádný obrázek. Toto je kód, který se zobrazí ikona s jednotlivými <span> značky vykreslování mezi bílé řádky:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Zahrnuje také název aplikace, která se zobrazí v levé horní části.  Hlavní navigační nabídce je vykreslen metodou `<ul>` v rámci druhého div a obsahuje odkazy na Domů, o a obraťte se na. Další odkazy pro registrace a přihlášení se přidá řádek _LoginPartial v řádku 29. Níže navigaci, hlavní části každé stránce vykreslením v jiném `<div>`označené jako s třídami "kontejner" a "obsah". V souboru _Layout jednoduché výchozí vidět tady jsou obsahu stránce vykreslen zobrazením konkrétní přidružené stránce a pak jednoduchou `<footer>` se přidá na konec `<div>` elementu.  Uvidíte, jak zobrazuje integrované o stránku pomocí této šablony:

![O stránku](bootstrap/_static/about-page-wide.png)

Sbalené navigační panel s tlačítkem "hamburger" v horním pravém, se zobrazí, když okno klesne pod určitou:

![o stránce s hamburger nabídky](bootstrap/_static/about-page-hamburger.png)

Kliknutím na ikonu zjistí položky nabídky svislé zásuvce, který snímky dolů z horní části stránky:

![o stránce s hamburger nabídky rozšířit](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Typografie a odkazy

Bootstrap nastaví základní typografii v lokalitě, barvy a odkaz formátování ve svém souboru šablon stylů CSS. Tento soubor CSS zahrnuje výchozí styly pro tabulky, tlačítka, elementy form, obrázky a další ([Další](http://getbootstrap.com/css/)). Jeden užitečné funkce je systému rozložení mřížky, popsané dále.

### <a name="grids"></a>Mřížky

Jednou z nejčastěji používané funkce Bootstrap je jeho systém rozložení mřížky. Moderních webových aplikací byste neměli používat `<table>` značky pro rozložení, místo toho omezení použití tohoto elementu k skutečné tabulková data. Místo toho sloupců a řádků můžete rozloženy pomocí řady `<div>` elementy a příslušné třídy CSS. Existuje několik výhod tohoto přístupu, včetně možnosti nastavit rozložení mřížky zobrazíte svisle na úzkých obrazovkách, například na telefonech.

[Systém rozložení mřížky na Bootstrap](http://getbootstrap.com/css/#grid) je založena na dvanáct sloupce. Toto číslo jste vybrali, protože je možné rozdělit rovnoměrně do 1, 2, 3 nebo 4 sloupce a šířku sloupců můžete k pohybovat v rozmezí 1/12 svislé šířky na obrazovce. Pokud chcete začít používat systém rozložení mřížky, měli byste začít s kontejner `<div>` a poté přidejte řádek `<div>`, jak je vidět tady:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

V dalším kroku přidejte další `<div>` prvky pro každý sloupec a zadat počet sloupců, která `<div>` by měla zabírat (mimo 12) jako součást třídy CSS počínaje "sloupec - md –". Například pokud chcete jednoduše mít dva sloupce stejné velikosti, by použití třídy "sloupec md – 6" pro každé z nich. V tomto případě "md" je zkratka pro "střední" a odkazuje na standardní velikosti stolní počítač velikosti zobrazení. Existují čtyři různé možnosti, které můžete vybrat z, a každý se použije pro vyšší šířky přepsána (takže pokud chcete rozložení opravit bez ohledu na šířku obrazovky, můžete zadat jenom xs třídy).

Předpona šablon stylů CSS – třída | Vrstva zařízení | Šířka
:---: | :---: | :---:
sloupec-xs - | Telefony | < 768px
sloupec-sm - | Tablety | > = 768px
sloupec-md – | Stolní počítače | > = 992px
sloupec-kontaktní skupina - | Zobrazí větší plochy | > = 1200px

Při zadávání dva sloupce obou se "sloupec md – 6" výsledné rozložení bude dva sloupce plochy rozlišení, ale tyto dva sloupce se svisle zásobníku při vykreslení v menší zařízení (nebo užší okno prohlížeče na ploše), což umožňuje uživatelům snadno zobrazit obsah, aniž by bylo nutné vodorovný posun.

Bootstrap bude vždy výchozí rozložení jednoho sloupce, takže je třeba zadat sloupce, pokud chcete více než jeden sloupec. Jenom jednou, můžete chtít explicitně určit, že `<div>` trvat až 12 všechny sloupce by k potlačení chování větší vrstvy zařízení. Při zadávání více tříd zařízení vrstvy, můžete resetovat vykreslování sloupec v určitých bodech. Přidání div clearfix, který se zobrazí v rámci určitých zobrazení může dosáhnout, jak je vidět tady:

![omezený a široké zobrazení mřížky](bootstrap/_static/narrow-and-wide-viewport-grid.png)

V předchozím příkladu jeden a dva sdílet řádek v rozložení "md", zatímco dva a tři sdílet v řádku v rozložení "xs". Bez clearfix `<div>`, druhý a třetí nezobrazují správně v zobrazení "xs" (Všimněte si, že se zobrazují pouze jeden, čtyři a pět):

![mřížky bez použití clearfix](bootstrap/_static/grid-without-clearfix.png)

V tomto příkladu pouze jeden řádek `<div>` byl použit, a zavedení stále většinou nebyla správné věci s ohledem na rozložení a překrývání sloupců. Obvykle byste měli zadat řádek `<div>` pro každý řádek vodorovné vyžaduje vaše rozložení a samozřejmě můžete vnořovat Bootstrap mřížky v rámci jednu na druhou. Když to uděláte, bude každé vnořené mřížky zabírat 100 % šířky elementu, ve kterém je umístěn, který lze potom rozdělit pomocí třídy sloupce.

### <a name="jumbotron"></a>Jumbotron

Pokud jste použili výchozí šablony ASP.NET MVC v sadě Visual Studio 2012 nebo 2013, pravděpodobně vidíte Jumbotron v akci. Odkazuje na velká část stránky, který umožňuje zobrazit obrázek na pozadí velké volání akce, rotator nebo podobným elementům plnou šířkou. Přidat jumbotron na stránku, jednoduše přidat `<div>` a pojmenujte ho třídu "jumbotron", pak umístit kontejner `<div>` uvnitř a přidejte svůj obsah.  Jsme můžete snadno upravit standardní o stránce sloužící jumbotron pro hlavní názvy sloupců, které se zobrazí:

![Příklad jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Tlačítka

Výchozí tlačítko třídy a jejich barvy jsou uvedené v následující obrázek.

![motivu tlačítka](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Odznaky

Odznaky naleznete malé, obvykle číselné popisky vedle položky navigace. Můžete indikuje počet zpráv nebo oznámení čekání nebo přítomnosti aktualizací. Určení takové odznaky je jednoduché, přidávání <span> obsahující text, s třídou "BADGE":

![odznaky s motivu](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Upozornění

Potřebujete zobrazí nějaké oznámení, upozornění nebo chybovou zprávu uživatelům vaší aplikace. To je, kde jsou užitečné standardní třídy výstrahy.  Existují čtyři různé úrovně závažnosti s přidružené barevná schémata:

![motivu výstrahy](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Navbars a nabídek

Naše rozložení už obsahuje standardní navigační panel, ale motivu spuštění, který podporuje možnosti dalšími styly. Nemůžeme se můžete také snadno rozhodnout pro zobrazení navigační panel svisle místo vodorovně Pokud, která obsahuje upřednostňované, také přidání dílčí navigační se položky v nabídkách plovoucím panelem. Jednoduché navigační nabídky, jako je karta proužky jsou postavený na <ul> elementy. Ty lze vytvořit velmi jednoduše tak, že právě poskytovat jim třídy CSS "nav" a "nav karty":

![motivu tabstrips](bootstrap/_static/theme-tabstrips.png)

Navbars jsou vytvořeny podobně, ale trochu složitější.  Jsou při spuštění systému `<nav>` nebo `<div>` s třídou "navbar", ve kterém div kontejner obsahuje zbytek elementy. Naše stránka obsahuje navigační panel v hlavičce již – následující jednoduše rozbalí na to, přidání podpory pro rozevírací nabídce:

![motivu navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Další prvky

Výchozí motiv lze také k dispozici ve stylu vhodně formátovaná, včetně podpory pro prokládané zobrazení tabulek HTML. Existují popisky se styly, které jsou podobné jako u tlačítka. Můžete vytvořit vlastní rozevírací nabídky, které podporují dalšími styly možnosti nad rámec standardních HTML `<select>` element společně s Navbars stejný, jako je naše výchozí úvodní lokality již používá. Pokud potřebujete indikátor průběhu, existuje několik styly můžete vybírat, a také seznam skupin a panely, které obsahují název a obsahu.  Prozkoumejte další možnosti v standardní Bootstrap motivu, který zde:

[http://getbootstrap.com/Examples/Theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Další motivy

Standardní motivu spuštění, který můžete rozšířit přepsáním některé nebo všechny jeho CSS, jak přizpůsobit barvy a styly, aby odpovídaly potřebám vaší vlastní aplikaci. Pokud chcete spustit z připravených motiv, existuje několik motiv galerie dostupných online, specializují na Bootstrap motivy, jako je například WrapBootstrap.com (což je mnoho motivů komerční) a Bootswatch.com (který nabízí bezplatné motivy).  Některé placené šablony dostupné poskytují značnou část funkce nad rámec základní motivu spuštění, který, jako je například bohatou podporu pro správu nabídek a řídicích panelů s bohatou grafů a ukazatelů. Příklad oblíbených placené šablony Inspinia, je aktuálně pro prodej pro $18, který obsahuje šablonu ASP.NET MVC5 kromě AngularJS a statické verze HTML. Snímek obrazovky ukázkové jsou uvedeny níže.

![Příklad motiv inspinia](bootstrap/_static/theme-inspinia.png)

Pokud chcete Změna motivu spuštění, umístí *bootstrap.css* soubor pro motiv, který chcete v **wwwroot/css** složky a změňte odkazy v *_Layout.cshtml* a nasměrovat ho.  Změna odkazy pro všechna prostředí:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Pokud chcete sestavit vlastní řídicí panel, můžete spustit z volného příkladu, které jsou k dispozici zde: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Součásti

Kromě těchto elementů již popsané, Bootstrap zahrnuje podporu pro celou řadu [integrované komponenty uživatelského rozhraní](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap obsahuje ikonu sady z Glyphicons ([http://glyphicons.com](http://glyphicons.com)), s více než 200 ikonami volně dostupných pro použití v rámci Bootstrap povolené webové aplikace. Tady je jenom malé ukázkové:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Vstupní skupiny

Vstupní skupin povolit sdružování další text nebo tlačítka s input element tak uživatelům více intuitivní prostředí:

![vstupní skupiny](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Popis cesty

Cesta ke stránce jsou běžné součást uživatelského rozhraní používá k zobrazení uživatele jejich nedávné historii nebo hloubka v rámci hierarchie navigace webu. Snadno přidat použitím třídy "s popisem cesty" na všechny `<ol>` element seznamu. Zahrnout integrovanou podporu pro stránkování pomocí třídy "stránkování" na `<ul>` v rámci `<nav>`. Přidání přizpůsobivý embedded prezentace a video s použitím `<iframe>`, `<embed>`, `<video>`, nebo `<object>` prvky, které budou automaticky styl Bootstrap. Zadejte konkrétní poměr stran pomocí určité třídy, jako je "Vložit přizpůsobivý 16by9".

## <a name="javascript-support"></a>Podpora jazyka JavaScript

Knihovna JavaScript Bootstrap na zahrnuje podporu rozhraní API pro zahrnuté součásti, vám umožní ovládat jejich chování prostřednictvím kódu programu v rámci vaší aplikace. Kromě toho *bootstrap.js* obsahuje více než tucet detekce (aktualizace styly, podle které má uživatel přešli v dokumentu), posuňte se vlastní jQuery modulů plug-in, že poskytuje další funkce, například přechody, modálních dialogových oken Sbalit chování, karusely a vyznačení nabídky do okna, takže není přejděte z obrazovky. Není dostatek místa na všechny doplňky JavaScript součástí Bootstrap – Další informace naleznete [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Souhrn

Bootstrap poskytuje webové rozhraní, které lze použít k rychlé a efektivní rozvržení a stylu širokou škálu webům a aplikacím. Její základní typografii a styly poskytují příjemný vzhled a chování, která lze snadno manipulovat prostřednictvím podpory vlastní motiv, který můžete ručně vytvořené nebo komerčně zakoupili. Podporuje hostitel webové komponenty, které v minulosti by vyžadovaly nákladné ovládací prvky třetích stran k dosažení, spolu s podporou moderní a otevřete webové standardy.
