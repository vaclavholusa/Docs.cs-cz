---
title: "Použití Gulp v ASP.NET Core"
author: rick-anderson
description: "Další informace o použití Gulp v ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: f091370bc85a37eeaac1291a2fdc6ea85164f148
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>Úvod do používání Gulp v ASP.NET Core 

Podle [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [ADAM Roth](https://github.com/danroth27), a [Shayne Boyer](https://twitter.com/spboyer)

V typické moderní webové aplikace může procesu sestavení:

* Vytvořit balíček a minifikaci souborů JavaScript a CSS.
* Spuštění nástrojů pro volání úlohy sdružování a minimalizace před každé sestavení.
* Kompilace menší nebo SASS soubory do šablon stylů CSS.
* Zkompilujte CoffeeScript nebo TypeScript soubory pro jazyk JavaScript.

A *Spouštěče úloh* je nástroj, který automatizuje tyto rutiny vývoj a další úkoly. Visual Studio poskytuje integrovanou podporu pro dvě oblíbených úloh založená na JavaScript závodníky: [Gulp](https://gulpjs.com/) a [Grunt](using-grunt.md).

## <a name="gulp"></a>gulp

Gulp je bázi jazyka JavaScript streamování sestavení nástrojů pro kódu na straně klienta. Běžně slouží k vysílání datového proudu klientské soubory pomocí několika procesů, když na konkrétní událost se aktivuje v prostředí sestavení. Gulp můžete například použít k automatizaci [sdružování a minimalizace](bundling-and-minification.md) nebo čištění prostředí pro vývoj než nové sestavení.

Sadu Gulp úloh je definována v *gulpfile.js*. Následující JavaScript obsahuje Gulp modulů a určuje cesty k souborům bude odkazovat v rámci chystaný úkolů:

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

Ve výše uvedeném kódu určuje, které moduly uzlu jsou požadovány. `require` Funkce importuje každý modul tak, aby závislých úloh můžete využívat jejich funkce. Každý z importovaných modulů je přiřazený k proměnné. Moduly můžou být buď podle názvu nebo cesty. V tomto příkladu s názvem moduly `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, a `gulp-uglify` se načítají podle názvu. Kromě toho řadu cesty jsou vytvořeny tak, aby umístění souborů CSS a JavaScript můžete opakovaně používat a odkazuje v rámci úlohy. Následující tabulka obsahuje popis modulů, které jsou součástí *gulpfile.js*.

| Název modulu | Popis |
| ----------- | ----------- |
| gulp        | Systém Gulp streamování sestavení. Další informace najdete v tématu [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Modul odstranění uzlu. Další informace najdete v tématu [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp concat | Modul, který řetězí soubory podle znak nového řádku operačního systému. Další informace najdete v tématu [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp cssmin | Modul, který minifikuje soubory šablon stylů CSS. Další informace najdete v tématu [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp uglify | Modul, který minifikuje *.js* soubory. Další informace najdete v tématu [gulp uglify](https://www.npmjs.com/package/gulp-uglify). |

Jakmile požadavků moduly importují, je možné zadat úlohy. Zde jsou šesti úlohy zaregistrován, reprezentována následující kód:

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

Následující tabulka obsahuje vysvětlení úloh, zadaný ve výše uvedeném kódu:

|Název úlohy|Popis|
|--- |--- |
|vyčištění: js|Úloha, která používá modul rimraf uzlu odstranění odebrat minifikovaný verzi souboru site.js.|
|vyčištění: šablon stylů css|Úloha, která používá modul rimraf uzlu odstranění k odebrání minifikovaný verze souboru site.css.|
|Vyčištění|Úloha, která volá `clean:js` úkolu, a `clean:css` úloh.|
|min:js|Úloha, která minifikuje a zřetězí všechny .js soubory ve složce js. . Min.js soubory jsou vyloučeny.|
|min:css|Úloha, která minifikuje a zřetězí všechny .css soubory ve složce šablon stylů css. . Min.css soubory jsou vyloučeny.|
|min|Úloha, která volá `min:js` úkolu, a `min:css` úloh.|

## <a name="running-default-tasks"></a>Spuštěné úlohy výchozí

Pokud jste ještě nevytvořili novou webovou aplikaci, vytvořte nový projekt webové aplikace ASP.NET v sadě Visual Studio.

1.  Do projektu přidejte nový soubor JavaScript a pojmenujte ji *gulpfile.js*, zkopírujte následující kód.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Otevřete *package.json* souboru (Přidat není-li existuje) a přidejte následující.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na *gulpfile.js*a vyberte **Průzkumník Spouštěče úloh**.
    
    ![V Průzkumníku řešení otevřete Průzkumník Spouštěče úloh](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Průzkumník Spouštěče úloh** zobrazuje seznam Gulp úlohy. (Možná budete muset klikněte **aktualizovat** tlačítko, které se zobrazí nalevo od názvu projektu.)
    
    ![Průzkumník Spouštěče úloh](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  Pod **úlohy** v **Průzkumník Spouštěče úloh**, klikněte pravým tlačítkem na **čisté**a vyberte **spustit** z místní nabídky.

    ![Úloha vyčištění Průzkumník Spouštěče úloh](using-gulp/_static/04-TaskRunner-clean.png)

    **Průzkumník Spouštěče úloh** vytvoří novou kartu s názvem **čisté** a provádění čisté úlohy, jako je definována v *gulpfile.js*.

5.  Klikněte pravým tlačítkem myši **čisté** úloh a pak vyberte **vazby** > **před sestavení**.

    ![Průzkumník Spouštěče úloh vazby BeforeBuild](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Před sestavení** vazby nakonfiguruje úlohu vyčištění automatické spuštění před každé sestavení projektu.

Vazby nastavit s **Průzkumník Spouštěče úloh** jsou uloženy ve formě komentářů v horní části vaší *gulpfile.js* a jsou platné pouze v sadě Visual Studio. Alternativu, která nevyžaduje Visual Studio, je nakonfigurovat automatické spuštění úlohy gulp ve vaší *.csproj* souboru. Například to uvedli do vaší *.csproj* souboru:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Teď čistou úlohy je spuštěn při spuštění projektu v sadě Visual Studio nebo pomocí příkazového řádku `dotnet run` příkazu (Spustit `npm install` první).

## <a name="defining-and-running-a-new-task"></a>Definování a spuštěním nové úlohy

Chcete-li definovat nové úlohy Gulp, změňte *gulpfile.js*.

1.  Přidejte následující JavaScript na konec *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Tato úloha je s názvem `first`, a jednoduše zobrazí řetězec.

2.  Uložit *gulpfile.js*.

3.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na *gulpfile.js*a vyberte *Průzkumník Spouštěče úloh*.

4.  V **Průzkumník Spouštěče úloh**, klikněte pravým tlačítkem na **první**a vyberte **spustit**.

    ![Průzkumník Spouštěče úloh spustit první úlohu](using-gulp/_static/06-TaskRunner-First.png)

    Uvidíte, že se zobrazí text výstupu. Pokud vás zajímá v příkladech podle běžný scénář, najdete v části Gulp recepty.

## <a name="defining-and-running-tasks-in-a-series"></a>Definování a spuštění úloh v řadě

Při spuštění několika úloh úlohy spouštět souběžně ve výchozím nastavení. Ale pokud potřebujete ke spouštění úloh v určitém pořadí, musíte zadat každý úkol po dokončení, stejně jako úkoly, které závisí na dokončení jiného úkolu.

1.  Chcete-li definovat řadu úloh, které se spouští v pořadí, nahraďte `first` úlohu, která jste přidali výše v *gulpfile.js* s následující:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Nyní máte tři úkoly: `series:first`, `series:second`, a `series`. `series:second` Úloh obsahuje druhý parametr, který určuje pole úlohy ke spuštění a dokončení před `series:second` úloha spustí. Jak je uvedeno v kód výše, jenom `series:first` úloha se musí dokončit před `series:second` úloha spustí.

2.  Uložit *gulpfile.js*.

3.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na *gulpfile.js* a vyberte **Průzkumník Spouštěče úloh** Pokud již není otevřený.

4.  V **Průzkumník Spouštěče úloh**, klikněte pravým tlačítkem na **řady** a vyberte **spustit**.

    ![Průzkumník Spouštěče úloh spustit úlohu řady](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense poskytuje doplňování kódu, popisy parametrů a další funkce pro zvýšení produktivity a snížení chyby. Gulp úlohy jsou napsané v jazyce JavaScript; IntelliSense můžete tedy pomoc při vývoji. Při práci s JavaScript IntelliSense zobrazí seznam objektů, funkce, vlastnosti a parametry, které jsou k dispozici na základě vaší aktuální kontextu. Kódování možnost vyberte v rozevíracím seznamu poskytované IntelliSense pro dokončení kódu.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Další informace o technologii IntelliSense, najdete v části [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Vývoj, pracovní a provozní prostředí

Když Gulp je použít k optimalizaci soubory klienta pro pracovní a provozní, zpracované soubory se uloží do místního umístění pracovní a provozní. *_Layout.cshtml* souborů používá **prostředí** značky pomocná rutina pro zadejte dvě různé verze souborů šablon stylů CSS. Jedna verze souborů šablon stylů CSS je pro vývoj a druhou verzi je optimalizovaná pro pracovní a provozní. V aplikaci Visual Studio 2017, když změníte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí `Production`, Visual Studio bude vytvoření webové aplikace a odkaz na minimalizovaném okně soubory šablon stylů CSS. Následující kód ukazuje **prostředí** značky obsahující značky odkazu pro pomocné rutiny `Development` šablon stylů CSS souborů a minifikovaný `Staging, Production` soubory šablon stylů CSS.

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Přepínání mezi prostředími

Chcete-li přepnout mezi kompilování v různých prostředích, změňte **ASPNETCORE_ENVIRONMENT** hodnotu proměnné prostředí.

1.  V **Průzkumník Spouštěče úloh**, ověřte, zda **min** úkolu nastaveno ke spuštění **před sestavení**.

2.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **vlastnosti**.

    Zobrazí se seznam vlastností pro webovou aplikaci.

3.  Klikněte **ladění** kartě.

4.  Nastavte hodnotu **hostitelský: prostředí** proměnnou prostředí `Production`.

5.  Stiskněte klávesu **F5** ke spuštění aplikace v prohlížeči.

6.  V okně prohlížeče, klikněte pravým tlačítkem na stránku a vyberte **zobrazit zdroj** zobrazení HTML pro stránku.

    Všimněte si, že šablony stylů odkazují na minifikovaný soubory šablon stylů CSS.

7.  Zavřete prohlížeč k zastavení webové aplikace.

8.  V sadě Visual Studio, vraťte se na seznamu vlastností pro webovou aplikaci a změnit **hostitelský: prostředí** proměnnou prostředí zpět do `Development`.

9.  Stiskněte klávesu **F5** a znovu spusťte aplikaci v prohlížeči.

10. V okně prohlížeče, klikněte pravým tlačítkem na stránku a vyberte **zobrazit zdroj** zobrazíte HTML pro stránku.

    Všimněte si, že šablony stylů odkazují na nezmenšenou verze souborů šablon stylů CSS.

Další informace související s prostředím v ASP.NET Core najdete v tématu [práce s několika prostředí](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Podrobnosti úlohy a modulu

Úloha Gulp není zaregistrována název funkce. Závislosti můžete zadat, pokud jiné úlohy musí být spuštěny před aktuální úlohy. Další funkce umožňují spouštět a sledovat Gulp úlohy, a také nastavit zdroj (*src*) a cíl (*cíle*) souborů upravována. Toto jsou primární funkce Gulp rozhraní API:

|Gulp – funkce|Syntaxe|Popis|
|---   |--- |--- |
|– úloha  |`gulp.task(name[, deps], fn) { }`|`task` Funkce vytvoří úlohu. `name` Parametr definuje název úlohy. `deps` Parametr obsahuje pole potřeba provést před spuštěním této úlohy. `fn` Parametr představuje funkci zpětného volání, která provádí operace úlohy.|
|Sledování |`gulp.watch(glob [, opts], tasks) { }`|`watch` Funkce monitoruje soubory a spustí úlohy, když dojde ke změně souboru. `glob` Parametr `string` nebo `array` který určuje, které pokud chcete sledovat soubory. `opts` Parametr poskytuje další soubor možnosti sledování.|
|src   |`gulp.src(globs[, options]) { }`|`src` Funkce obsahuje soubory, které odpovídají glob hodnoty. `glob` Parametr `string` nebo `array` který určuje, které soubory číst. `options` Parametr poskytuje možnosti pro další soubory.|
|Cíle  |`gulp.dest(path[, options]) { }`|`dest` Funkce definuje umístění, do kterého lze zapisovat soubory. `path` Parametr je řetězec nebo funkci, která určuje cílovou složku. `options` Parametr je objekt, který určuje možnosti složku výstupu.|

Další informace referenční dokumentace rozhraní API Gulp najdete v tématu [Gulp dokumentace rozhraní API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Gulp recepty

Komunita Gulp poskytuje Gulp [recepty](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Tyto recepty obsahovat úlohy Gulp řeší běžné scénáře.

## <a name="additional-resources"></a>Další zdroje

* [Gulp dokumentace](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Sdružování a minimalizace v ASP.NET Core](bundling-and-minification.md)
* [Použití Grunt v ASP.NET Core](using-grunt.md)
