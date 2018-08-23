---
title: Vytváření atraktivních, interaktivních webů pomocí Bootstrap a ASP.NET Core
author: ardalis
description: Další informace o použití Bootstrap pro vývoj, interaktivních webových aplikací pomocí ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755513"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Vytváření atraktivních, interaktivních webů pomocí Bootstrap a ASP.NET Core

<a name="bootstrap-index"></a>

Podle [Steve Smith](https://ardalis.com/)

Bootstrap je aktuálně nejoblíbenější webové rozhraní pro vývoj přizpůsobivé webové aplikace. Nabízí řadu funkcí a výhod, které může zlepšit zkušenosti vašich uživatelů s webem, ať už jste začínající na front-endu návrh a vývoj nebo odborník. Bootstrap je nasazená jako sady souborů CSS a JavaScript a efektivně pomůže škálovací webu nebo aplikaci z telefonů na tablety pro stolní počítače.

## <a name="get-started"></a>Začínáme

Existuje několik způsobů, jak začít pracovat s Bootstrap. Pokud začínáte novou webovou aplikaci v sadě Visual Studio, můžete výchozí úvodní šablona pro ASP.NET Core, ve kterém budou přicházet případu Bootstrap předinstalované:

![Spuštění v zobrazení pro úvodní šablona řešení](bootstrap/_static/bootstrap-in-starter-template.png)

Přidání Bootstrap do ASP.NET Core projektu je jednoduše otázkou jejím přidáním na *bower.json* jako závislost:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Toto je doporučený postup pro přidání do projektu aplikace ASP.NET Core Bootstrap.

Můžete také nainstalovat pomocí jedné z několika Správce balíčků, jako je například Bower, npm a NuGet bootstrap. V obou případech procesu je v podstatě stejný:

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
> Doporučený způsob instalace závislostí na straně klienta jako Bootstrap v ASP.NET Core je prostřednictvím Bower (pomocí *bower.json*, jak je uvedeno výše). Použijte npm a NuGet se zobrazí na ukazují, jak snadno Bootstrap lze přidat do jiných typů webových aplikací, včetně starších verzí prostředí ASP.NET.

Pokud už odkazuje na vaše vlastní místní verze Bootstrap, musíte odkazovat v jakékoli stránky, které ji používají. V produkčním prostředí by měly odkazovat bootstrap používání sítě CDN. Ve výchozí šablona webu ASP.NET Core *_Layout.cshtml* soubor tak tímto způsobem:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Pokud se chystáte používat některý z modulů plug-in jQuery pro spuštění, musíte také odkazovat jQuery.

## <a name="basic-templates-and-features"></a>Základní šablony a funkce

Nejzákladnější Bootstrap šablona vypadá velmi podobně jako *_Layout.cshtml* souboru uvedené výše a jednoduše zahrnuje základní nabídky pro navigaci a místo, kde k vykreslení zbytku stránky.

### <a name="basic-navigation"></a>Základní navigace

Výchozí šablona používá sadu `<div>` prvky k vykreslení v horním navigačním panelu a hlavní stránky. Pokud používáte HTML5, můžete nahradit první `<div>` označit `<nav>` značka, které docílíte stejného efektu, ale s přesnější sémantiku. V rámci první `<div>` naleznete v tématu existuje několik dalších. První, `<div>` s třídou "kontejneru" a v rámci, které dvou dalších `<div>` elementy: "navbar-header" a "navbar sbalit". Div navigačním panelu záhlaví obsahuje tlačítko, které se zobrazí, když na obrazovce je nižší než určité minimální šířku, zobrazuje 3 vodorovné čárky (takzvané "ikonu" hamburgerové""). Ikona je vykreslen pomocí čistý HTML a CSS; vyžaduje se žádný obrázek. Toto je kód, který se zobrazí ikona s jednotlivými <span> značky jeden z pruhů bílé vykreslování:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Zahrnuje také název aplikace, které se zobrazí v levé horní části. Hlavní navigační nabídku je vykreslen metodou `<ul>` elementu ve druhém div a obsahuje odkazy na domácí o programu a kontaktujte. Dole na navigačním panelu hlavní jednotlivých stránek je vykreslen v jiné `<div>`, označená "kontejneru" a "obsah" třídy. V jednoduchých výchozí \_ukázali, obsah stránky jsou generovány s konkrétní zobrazení spojené s stránky a jednoduchý soubor rozložení `<footer>` se přidá na konec objektu `<div>` elementu. Uvidíte jak předdefinované o stránce se zobrazí pomocí této šablony:

![O stránku](bootstrap/_static/about-page-wide.png)

Sbalený navigační panel s "hamburgerové" tlačítko vpravo, nahoře se zobrazí, když v okně klesne pod určitou:

![o stránku "hamburgerové" nabídky](bootstrap/_static/about-page-hamburger.png)

Kliknutím na ikonu odhaluje položky nabídky v svislý zásobník, který snímky dolů z horní části stránky:

![rozšířit o stránku "hamburgerové" nabídky](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Týkající se Typografie nebo odkazy

Spuštění se nastavuje základní Typografie, barev a formátování v souboru šablony stylů CSS odkazu webu. Tento soubor CSS obsahuje výchozí styly pro tabulky, tlačítka, prvky formuláře, obrázky a další ([Další](http://getbootstrap.com/css/)). Jeden užitečné funkce je systém rozložení mřížky, popsané dále.

### <a name="grids"></a>Mřížky

Jedním z nejoblíbenějších funkce Bootstrap je jeho systém rozložení mřížky. Moderní webové aplikace byste neměli používat `<table>` značky pro rozložení, místo toho omezení použití tohoto elementu na skutečné tabulková data. Místo toho sloupce a řádky můžou rozloží pomocí řady `<div>` elementy a příslušné třídy šablony stylů CSS. Existuje několik výhod tohoto přístupu, včetně možnosti upravit rozložení mřížky zobrazit svisle na úzkých obrazovkách, třeba na telefonech.

[Systém rozložení mřížky na Bootstrap](http://getbootstrap.com/css/#grid) vychází z 12 sloupců. Toto číslo byla vybrána, protože je možné rozdělit rovnoměrně do 1, 2, 3 nebo 4 sloupce a šířky sloupců se může lišit pro v rámci 1/12 svislé šířka obrazovky. Pokud chcete začít používat systém rozložení mřížky, měli byste začít s kontejnerem `<div>` a potom přidejte řádek `<div>`, jak je znázorněno zde:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

V dalším kroku přidejte další `<div>` prvky pro jednotlivé sloupce a zadat počet sloupců, která `<div>` by měla zabírat (mimo 12) jako součást třídu šablony stylů CSS, počínaje "sloupec - md –". Například pokud chcete jednoduše musí mít dva sloupce stejnou velikost, použijete třídu "sloupec md – 6" pro každé z nich. V tomto případě "md" je zkratka pro "střední" a odkazuje na velikosti zobrazení standardní velikosti stolního počítače. Existují čtyři různé možnosti, které můžete vybrat z, a každá se bude používat pro vyšší šířku, pokud přepsána (takže pokud chcete opravit bez ohledu na to šířka obrazovky rozložení, vám stačí zadat třídy xs).

Předpona třídy šablony stylů CSS | Úroveň zařízení | Šířka
:---: | :---: | :---:
col-xs- | Telefony | < 768px
sloupec-sm - | Tablety | >= 768px
sloupec-md – | Stolní počítače | >= 992px
sloupec – lg - | Zobrazí větší klasické pracovní plochy | >= 1200px

Při zadávání dva sloupce i s "ve sloupci md – 6" výsledný rozložení bude mít dva sloupce na klasické pracovní plochy rozlišení, ale tyto dva sloupce se svisle zásobníku při vykreslení na menších zařízeních (nebo užší okno prohlížeče na stolní počítač), umožňuje uživatelům snadno zobrazit obsah, aniž byste museli posouvat vodorovně.

Proto je potřeba jenom určit sloupce, když má více než jeden sloupec bude vždy Bootstrap výchozí rozložení jedním sloupcem. Jediný čas, byste měli explicitně určit, že `<div>` aplikace zkuste si všechny 12 sloupců je chování vyšší úroveň zařízení přepsat. Při zadávání více tříd úroveň zařízení, může být potřeba resetovat sloupce vykreslování v určitých bodech. Přidání clearfix div, která je jenom viditelné v rámci určité zobrazení může dosáhnout tím, jak je znázorněno zde:

![úzké a široké zobrazení mřížky](bootstrap/_static/narrow-and-wide-viewport-grid.png)

V příkladu výše 1 a 2 sdílet řádek v rozložení "md", dva a tři sdílet řádek v rozložení "x". Bez clearfix `<div>`, dva a tři nejsou správně zobrazeny v zobrazení "xs" (Všimněte si, že jsou zobrazeny pouze jeden, čtyři a pěti):

![mřížky bez použití clearfix](bootstrap/_static/grid-without-clearfix.png)

V tomto příkladu, pouze jeden řádek `<div>` byl použit, a zavedení stále většinou postupovaly správně s ohledem na rozložení a skládání sloupců. Obvykle byste měli zadat řádek `<div>` pro každý řádek vodorovné rozložení vyžaduje a samozřejmě můžete vnořit Bootstrap mřížky v rámci mezi sebou. Pokud ano, každý vnořené tabulky budou zaměstnávat 100 % šířky elementu ve kterém je umístí, které se pak dají rozdělit pomocí tříd sloupce.

### <a name="jumbotron"></a>Jumbotron

Pokud jste použili výchozí šablony ASP.NET Core MVC v sadě Visual Studio 2012 nebo 2013, pravděpodobně jste viděli Jumbotron v akci. Odkazuje na velké části plnou šířkou, který slouží k zobrazení obrázku na pozadí velké, volání akce, rotator nebo podobné prvky stránky. Přidat na stránku jumbotron, stačí přidat `<div>` a poskytněte třídu "jumbotron" a pak umístit kontejner `<div>` uvnitř a přidat vlastní obsah. Jsme můžete snadno upravit standard o stránku jumbotron pro hlavní názvy sloupců, které se zobrazí:

![Příklad jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Tlačítka

Na obrázku níže jsou uvedeny výchozí tlačítko třídy a jejich barvami.

![tlačítka s motivem](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Odznáčků

Odznáčky odkazovat na malé, obvykle číselné popisky vedle položky navigace. Můžete indikuje počet zpráv nebo oznámení, čekání nebo přítomnost aktualizace. Zadání těchto oznámení je stejně jednoduché jako přidání `<span>` obsahující text, s třídou "oznámení"BADGE "":

![s motivem odznáčků](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Upozornění

Budete muset zobrazit nějaký druh oznámení, upozornění nebo chybovou zprávu pro uživatele vaší aplikace. To je, kde jsou užitečné standardní třídy výstrahy. Existují čtyři různé úrovně závažnosti s přidružené barevná schémata:

![s motivem výstrahy](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Nabídky a Navbars

Naše rozložení již obsahuje standardní navigační panel, ale motivu spuštění, který podporuje používání stylů pro další možnosti. Můžeme také snadno rozhodnout zobrazit na navigačním panelu svisle, spíše než vodorovně Pokud, která obsahuje upřednostňované, stejně jako přidávání dílčí položky navigace v nabídkách Kontextová nabídka. Jednoduché navigační nabídky, jako je karta pruhy, jsou zabudovány nad `<ul>` elementy. Ty lze vytvořit velmi jednoduše a poskytovat jim jenom pomocí šablon stylů CSS třídy "nav" a "navigační karty":

![s motivem tabstrips](bootstrap/_static/theme-tabstrips.png)

Navbars jsou sestaveny podobně, ale o něco složitější. Začínají `<nav>` nebo `<div>` s třídou "navbar", ve kterém div kontejner obsahuje zbývající prvky. Naši stránku obsahuje navigačním panelu v hlavičce již – následující jednoduše rozbalí na to, přidání podpory do rozevírací nabídky:

![s motivem navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Další elementy

Výchozí motiv lze také představovat tabulky HTML ve stylu přehledně naformátovaná, včetně podpory pro prokládané zobrazení. Existují popisky se styly, které jsou podobné jako u tlačítek. Můžete vytvořit vlastní rozevíracích nabídek, které podporují používání stylů pro další možnosti nad rámec standardních HTML `<select>` element spolu s Navbars jako ten náš web starter výchozí už používá. Pokud potřebujete indikátor průběhu, existuje několik stylů můžete vybírat z, a také seznam skupin a panelů, které obsahují název a obsah. Prozkoumejte další možnosti v rámci standardní Bootstrap motivu tady:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Další motivy

Standardní motivu spuštění, který můžete rozšířit tak, že přepíšete některé nebo všechny jeho šablon stylů CSS, nastavení barvy a styly podle potřeb své aplikaci. Pokud chcete spustit z předem připravených motivu, existuje několik motiv Galerie k dispozici online, který specializují na spuštění motivy, jako je například WrapBootstrap.com (který obsahuje širokou škálu obchodních motivy) a Bootswatch.com (která nabízí bezplatné motivy). Některé z placených šablon, které jsou k dispozici poskytují spoustu funkce nad rámec základní motivu spuštění, který, jako je například bohatou podporu pro správce nabídek a řídicí panely s formátovaným grafy a měřidla. Je například šablonu oblíbených placené Inspinia, která je znázorněna na následujícím snímku obrazovky:

![Příklad inspinia motiv](bootstrap/_static/theme-inspinia.png)

Pokud chcete změnit motivu spuštění, umístěte *bootstrap.css* soubor motivu, které chcete v **wwwroot/css** složky a změňte odkazy v *_Layout.cshtml* Chcete-li bod. Změňte odkazy pro všechna prostředí:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Pokud chcete vytvořit vlastní řídicí panel, můžete spustit z bezplatné příklad, který je k dispozici zde: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Součásti

Kromě těchto prvků již probírali, Bootstrap, zahrnuje podporu pro širokou škálu [integrované komponenty uživatelského rozhraní](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap zahrnuje sady ikon z Glyphicons ([http://glyphicons.com](http://glyphicons.com)), s více než 200 ikonami volně k dispozici pro použití v rámci povoleno spuštění webové aplikace. Tady je jen malý vzorek:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Vstupní skupiny

Vstupní skupiny povolit sdružování další textu nebo tlačítka s input element tak uživatelům v mnohem intuitivnější prostředí:

![vstupní skupiny](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>S popisem cesty

Popis cesty jsou běžné komponenta uživatelského prostředí slouží k zobrazení uživatele své informace o nedávné historii nebo hloubce navigační hierarchie lokality. Snadno přidat použitím třídy "s popisem cesty" k libovolnému `<ol>` element seznamu. Pomocí třídy "stránkování" na obsahují integrovanou podporu pro stránkování `<ul>` element v rámci `<nav>`. Přidat vložený interaktivní prezentace a video s použitím `<iframe>`, `<embed>`, `<video>`, nebo `<object>` prvky, které budou automaticky stylu Bootstrap. Zadejte konkrétní poměr stran pomocí konkrétní třídy typu "Vložit responzivní 16by9".

## <a name="javascript-support"></a>Podpora jazyka JavaScript

Knihovny jazyka JavaScript společnosti Bootstrap zahrnuje podporu rozhraní API pro zahrnuté komponenty umožňuje řídit jejich chování prostřednictvím kódu programu v rámci vaší aplikace. Kromě toho *bootstrap.js* obsahuje více než deseti detekce (aktualizuje se styly podle, ve kterém se uživatel posunul v dokumentu), posuňte se moduly plug-in vlastní jQuery, poskytuje další funkce, jako je přechody, modální dialogová okna Sbalit chování, Karusel a vyznačení nabídky do okna, takže se nemusíte používat posuvník. Není dostatek místa pro pokrytí všech doplňků jazyka JavaScript součástí Bootstrap – Další informace najdete [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Souhrn

Bootstrap poskytuje webové rozhraní, které lze použít k rychlému a produktivní rozložení a stylů širokou škálu webů a aplikací. Styly a základní Typografie poskytují příjemný vzhled a chování, které lze snadno ovládat díky podpoře vlastní motiv, který můžete ručně vytvořený nebo zakoupit komerční. Podporuje se řada webových komponent, které v minulosti by jste požádali nákladné ovládací prvky třetích stran k provedení při současné podpoře moderní a otevřené webové standardy.
