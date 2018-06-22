---
title: Menší Sass a písma Super v ASP.NET Core
author: ardalis
description: Další informace o použití menší Sass a písma Super v aplikacích ASP.NET Core.
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275562"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Menší Sass a písma Super v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Uživatelé webových aplikací mít stále vysoké očekávání při rozhodování o stylu a celkové zkušenosti. Moderních webových aplikací často využívat bohaté nástroje a rozhraní pro definování a správu jejich vzhled a chování konzistentním způsobem. Rozhraní, jako [Bootstrap](http://getbootstrap.com/) můžete vyhledat celou směrem k definování společnou sadu možností rozložení pro weby a stylů. Většina netriviální lokality však také těžit z schopnost efektivně definovat a udržovat stylů a soubory kaskádových stylů (CSS) a také s snadný přístup k ikon bitové kopie, které pomáhají intuitivnější webového rozhraní. Kde je jazyky a nástroje, které podporují [menší](http://lesscss.org/) a [Sass](http://sass-lang.com/), a knihovny, jako jsou [písma Super](http://fontawesome.io/), mají.

## <a name="css-preprocessor-languages"></a>Jazyky preprocesoru šablon stylů CSS

Jazyky, které jsou zkompilovány do jiných jazyků, za účelem zlepšení možností práce s základní jazyk, se označují jako preprocesorů. Existují dvě oblíbených preprocesorů pro šablon stylů CSS: menší a Sass.  Tyto preprocesorů přidání funkcí do šablon stylů CSS, jako je podpora pro proměnné a vnořené pravidla, která zlepšit udržovatelnosti složitých šablony stylů. CSS jako jazyk je velmi základní postrádá podporu i pro něco stejně jednoduché jako proměnné, a to může být souborů CSS opakovaných a opakovaném. Přidávání skutečných programovací jazyk funkcí pomocí preprocesorů může pomoci snížit duplikování a poskytují lepší uspořádání pravidel stylů. Visual Studio poskytuje integrovanou podporu pro obě méně a Sass, jakož i rozšíření, která lze dále zvýšit vývojového prostředí při práci s tyto jazyky.

Jako zběžný příklad jak vylepšit preprocesorů přehlednosti a udržovatelnosti informace o stylu vezměte v úvahu tato šablon stylů CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Pomocí menší, to může být přepsána pro odstranění všech duplikace, použití *mixin* (takže s názvem, protože umožňuje "kombinovat" vlastnosti z jedné třídy nebo do jiné sady pravidel):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>menší

Spustí preprocesoru méně šablon stylů CSS pomocí Node.js. K instalaci menší použijte uzel Správce balíčků (npm) z příkazového řádku (-g znamená "globální"):

```console
npm install -g less
```

Pokud používáte Visual Studio, můžete začít pracovat s méně přidáním menší soubory do projektu a potom nakonfigurovat Gulp (nebo Grunt) pro jejich zpracování při kompilaci. Přidat *styly* složky do projektu a pak přidejte nový menší soubor s názvem *main.less* do této složky.

![Přidat menší soubor](less-sass-fa/_static/add-less-file.png)

Po přidání, struktury složky by měl vypadat přibližně takto:

![Struktura složek](less-sass-fa/_static/folder-structure.png)

Teď můžete přidat některé základní stylů do souboru, který se zkompiluje do šablon stylů CSS a nasazení do složky wwwroot Gulp.

Upravit *main.less* k obsahovat následující obsah, který vytvoří z jedné základní barvy palety barev jednoduché.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` objekt a druhá @-prefixed položky jsou proměnné. Každý z nich představuje barvu. S výjimkou `@base`, je nastavena pomocí funkcí barev: zjednodušit ztmavení a číselníku. Zjednodušit a ztmavení provést podstatě by očekávat; Otočení upraví odstín barvy se o počet stupňů (kolem barevné kolo). Menší procesoru je dostatečně inteligentní ignorovat proměnné, které se nepoužívají, takže ukazují, jak tyto proměnné fungují, potřebujeme používat místo. Třídy `.baseColor`, popisuje počítané hodnoty těchto proměnných v souboru CSS, která je vytvořena atd.

### <a name="get-started"></a>Začínáme

Vytvoření **konfigurační soubor npm** (*package.json*) ve složce projektu a upravit ho tak, aby odkazovaly `gulp` a `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Nainstalujte závislosti buď na příkazovém řádku ve složce projektu, nebo v sadě Visual Studio **Průzkumníku řešení** (**závislosti > npm > Obnovení balíčků**).

```console
npm install
```

![VS obnovení balíčků](less-sass-fa/_static/restore-packages.png)

Ve složce projektu vytvořte **Gulp konfigurační soubor** (*gulpfile.js*) k definování automatizovaného procesu.  Přidejte proměnnou v horní části souboru představují menší a menší spuštění úlohy:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Otevřete **Průzkumník Spouštěče úloh** (**zobrazení > jiných Windows > Průzkumník Spouštěče úloh**). Mezi úlohy, měli byste vidět novou úlohu s názvem `less`. Můžete chtít aktualizujte okno nástroje.

Spustit `less` úloh a v tématu výstup podobný co je znázorněno zde:

![menší Spouštěče úloh](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css* složka nyní obsahuje nový soubor *main.css*:

![vytvořit hlavní css](less-sass-fa/_static/main-css-created.png)

Otevřete *main.css* a vypadá takto:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Jednoduché stránku HTML, kterou chcete přidat *wwwroot* složku a referenční dokumentace *main.css* zobrazíte paletu barev v akci.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Uvidíte, 180 stupňů číselníku na `@base` používají k vytvoření `@background` výsledkem proti barvu barevného kola `@base`:

![Příklad méně testování](less-sass-fa/_static/less-test-screenshot.png)

Menší taky poskytuje podporu pro vnořené pravidla a také dotazy vnořené média. Například definující vnořené hierarchií, jako jsou nabídky může mít za následek podrobné pravidla šablon stylů CSS, například tyto:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

V ideálním případě všechny související stylová pravidla, která bude umístěna společně v rámci tohoto souboru CSS, ale v praxi není nic vynucování toto pravidlo s výjimkou konvence a případně komentáře bloku.

Definování těchto stejná pravidla pomocí menší vypadá takto:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Všimněte si, že se v tomto případě všechny podřízené elementy `nav` jsou obsaženy v rámci svého oboru. Již není žádné opakování nadřazené elementy (`nav`, `li`, `a`), a také klesla počet celkový počet řádků (i když některé, je výsledkem vložení hodnoty na stejné řádky v druhém příkladu). Může být velmi užitečná, organizací, pokud chcete zobrazit všechna pravidla pro daný prvek uživatelského rozhraní v rámci explicitně ohraničené oboru, v takovém případě nastavte ze zbytek souboru ve složených závorkách.

`&` Syntaxe je menší selektor funkce, s & představující aktuální nadřazené selektor. Ano, v rámci {...} blok, `&` představuje `a` značky a proto `&:link` je ekvivalentní `a:link`.

Dotazy na média, velmi užitečné při vytváření přizpůsobivý návrhů, může taky výraznou přispívat k opakování a složitost v jazyce CSS. Menší umožňuje dotazy na média se Nested v rámci třídy, aby definice celou třídu není potřeba zopakovat v rámci různých nejvyšší úrovně `@media` elementy. Zde je ukázka, šablon stylů CSS přizpůsobivý nabídky:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

To je možné lépe definovat v menší jako:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Jiné funkce méně jsme již viděli je podpora pro matematické operace a atributy stylu zkonstruovat z předdefinovaných proměnných. Díky aktualizace související styly mnohem jednodušší, protože je možné upravit proměnnou základní a všechny závislé hodnoty automaticky změnit.

Soubory šablon stylů CSS, hlavně pro velké weby (a hlavně v případě, že se používají dotazy na média), zpravidla získat poměrně značnou časem provedení práce s nimi nepraktické. Menší soubory se může definovat samostatně, pak vyžádat pomocí `@import` direktivy. Menší mohou sloužit také k importu jednotlivých souborů CSS, taky v případě potřeby.

*Mixins* může přijmout parametry a menší podporuje podmíněnou logiku ve formě mixin chrání, které poskytují deklarativní způsob, jak definovat při určitých mixins vstoupila v platnost. Běžně používá pro mixin chrání je upravit barvy založené na tom, jak slabá nebo tmavý zdrojové barvy. Zadané mixin, který přijme parametr pro barvu, mixin ochranu lze použít k úpravě mixin na základě této barvy:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Zadané naše aktuální `@base` hodnotu `#663333`, menší tento skript vytvoří následující CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Menší poskytuje celou řadu dalších funkcí, ale to by měl získali přehled napájení tohoto předběžného zpracování jazyka.

## <a name="sass"></a>Sass

Sass je podobná menší, zajištění podpory pro mnoho stejných funkcí, ale syntaxí mírně lišit. Je vytvořen pomocí Ruby místo JavaScript, a proto nemá různé instalační požadavky. Původní jazyk Sass nepoužili složené závorky nebo středníky, ale místo definuje obor pomocí mezer a odsazení. Ve verzi 3 Sass byla zavedená Nová syntaxe, **SCSS** (dále jen "Sassy CSS"). SCSS je podobná šablon stylů CSS v, že ignoruje úrovně odsazení a prázdný znak a místo toho používá středníky a složené závorky.

K instalaci Sass, obvykle můžete by nejprve nainstalujte Ruby (předem nainstalovaná v systému macOS) a spusťte:

```console
gem install sass
```

Ale pokud používáte Visual Studio, můžete začít s Sass prakticky stejným způsobem jako stejně jako s menší. Otevřete *package.json* a přidejte balíček "gulp-sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

V dalším kroku změnit *gulpfile.js* k přidání proměnné sass a úlohu pro kompilaci Sass soubory a umístěte do složky wwwroot výsledky:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Teď můžete přidat soubor Sass *main2.scss* k *styly* složku v kořenovém adresáři projektu:

![Přidejte soubor scss](less-sass-fa/_static/add-scss-file.png)

Otevřete *main2.scss* a přidejte následující:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Uložte všechny soubory. Nyní když aktualizujete **Průzkumník Spouštěče úloh**, uvidíte `sass` úloh. Spusťte ji a hledat v */wwwroot/css* složky. Nyní je *main2.css* soubor s tyto obsah:

```css
body {
    background-color: #CC0000;
}
```

Sass podporuje vnoření téměř stejný se, že menší nemá, poskytuje podobné výhody. Soubory je možné rozdělit podle funkce a zahrnuté pomocí `@import` – direktiva:

```sass
@import 'anotherfile';
```

Sass podporuje mixins taky pomocí `@mixin` – klíčové slovo se definovat a `@include` pro zahrnutí, jako v následujícím příkladě z [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Kromě mixins Sass podporuje také koncepci dědičnosti, povolení jednu třídu k jiné rozšíření. Je koncepčně podobá mixin, ale má za následek méně kódu šablon stylů CSS. Dosahuje pomocí `@extend` – klíčové slovo. Můžete vyzkoušet na mixins, přidejte následující příkaz pro vaše *main2.scss* souboru:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Zkontrolujte výstup v *main2.css* po spuštění `sass` úloh v **Průzkumník Spouštěče úloh**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Všimněte si, že jsou všechny běžné vlastnosti výstrahy mixin opakována v každé třídě. Mixin nebyla dobrou práci, což pomáhá eliminovat duplikace v době vývoje, ale je stále vytváří šablon stylů CSS s velkým množstvím duplikace v, což vede k větší než potřebné soubory šablon stylů CSS – potenciální problém výkonu.

Nyní nahraďte výstrahy mixin s `.alert` třídy a změňte `@include` k `@extend` (Nezapomeňte rozšířit `.alert`, nikoli `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Spusťte Sass ještě jednou a zkontrolujte výsledné CSS:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Nyní jsou definovány vlastnosti jenom jako podle potřeby a lépe se vygeneruje šablon stylů CSS.

Sass také obsahuje funkce a operace podmíněnou logiku, podobně jako menší. Ve skutečnosti jsou velmi podobné možnosti dvou jazyků.

## <a name="less-or-sass"></a>Menší nebo Sass?

Je stále shody o tom, jestli je obecně lepší použít menší nebo Sass, (nebo i jestli se má přednost původní Sass nebo novější syntaxe SCSS v rámci Sass). Pravděpodobně je nejdůležitější rozhodnutí **použijte jednu z těchto nástrojů**, nikoli jen kódování souborů CSS dlaně. Jakmile jste provedli, které rozhodnutím, jak méně a Sass se velmi dobře hodí.

## <a name="font-awesome"></a>Skvělá písma

Kromě preprocesorů šablon stylů CSS je to Super písma jiné skvělým zdrojem pro stylů moderních webových aplikací. Písmo Super je sada nástrojů, který poskytuje více než 500 škálovatelné vektoru ikony, které můžete volně používat ve vaší webové aplikace. Byl původně navržený na práci s Bootstrap však nemá žádné závislosti na dané platformy nebo na žádné knihovny jazyka JavaScript.

Nejjednodušší způsob, jak začít pracovat s písma Super je přidat odkaz na něj pomocí veřejného doručování obsahu (CDN) síťové umístění:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Můžete také ho přidat do projektu sady Visual Studio přidejte jej do "závislosti" v *bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Až budete mít odkaz na písmo Super na stránce, můžete přidat ikony do vaší aplikace s použitím písma Super třídy, obvykle předponu "DM-" pro vložené HTML elementy (například `<span>` nebo `<i>`).  Například můžete přidat ikony jednoduché seznamy a nabídky pomocí kódu takto:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Tímto se vytvoří následující v prohlížeči – Poznámka: na ikonu vedle jednotlivých položek:

![ikony v seznamu](less-sass-fa/_static/list-icons-screenshot.png)

Můžete zobrazit úplný seznam dostupných ikon:

http://fontawesome.io/icons/

## <a name="summary"></a>Souhrn

Moderní webové aplikace stále potřebují reakce, plynulá práce návrhy, které jsou čistá, intuitivní a snadno použít z různých zařízení. Správa složitosti šablony stylů CSS, potřebné k dosažení těchto cílů se nejlépe provádí pomocí preprocesoru jako menší nebo Sass. Kromě toho sad nástrojů, jako je písmo Super rychle zadejte dobře známé ikony pro textovou navigační nabídky a tlačítka, zvýšení celkové uživatelské prostředí vaší aplikace.
