---
title: "Použití Gulp v ASP.NET Core"
author: rick-anderson
description: "Další informace o použití Gulp v ASP.NET Core."
keywords: ASP.NET Core, Gulp
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68f6838889cfb830f2c5a1976b3140ae5d94ac25
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="4b81f-104">Úvod do používání Gulp v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b81f-104">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="4b81f-105">Podle [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [ADAM Roth](https://github.com/danroth27), a [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="4b81f-105">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="4b81f-106">V typické moderní webové aplikace může procesu sestavení:</span><span class="sxs-lookup"><span data-stu-id="4b81f-106">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="4b81f-107">Vytvořit balíček a minifikaci souborů JavaScript a CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-107">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="4b81f-108">Spuštění nástrojů pro volání úlohy sdružování a minimalizace před každé sestavení.</span><span class="sxs-lookup"><span data-stu-id="4b81f-108">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="4b81f-109">Kompilace menší nebo SASS soubory do šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-109">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="4b81f-110">Zkompilujte CoffeeScript nebo TypeScript soubory pro jazyk JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4b81f-110">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="4b81f-111">A *Spouštěče úloh* je nástroj, který automatizuje tyto rutiny vývoj a další úkoly.</span><span class="sxs-lookup"><span data-stu-id="4b81f-111">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="4b81f-112">Visual Studio poskytuje integrovanou podporu pro dvě oblíbených úloh založená na JavaScript závodníky: [Gulp](https://gulpjs.com/) a [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="4b81f-112">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="4b81f-113">Gulp</span><span class="sxs-lookup"><span data-stu-id="4b81f-113">Gulp</span></span>

<span data-ttu-id="4b81f-114">Gulp je bázi jazyka JavaScript streamování sestavení nástrojů pro kódu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4b81f-114">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="4b81f-115">Běžně slouží k vysílání datového proudu klientské soubory pomocí několika procesů, když na konkrétní událost se aktivuje v prostředí sestavení.</span><span class="sxs-lookup"><span data-stu-id="4b81f-115">It is commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="4b81f-116">Gulp můžete například použít k automatizaci [sdružování a minimalizace](bundling-and-minification.md) nebo čištění prostředí pro vývoj než nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="4b81f-116">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="4b81f-117">Sadu Gulp úloh je definována v *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-117">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="4b81f-118">Následující JavaScript obsahuje Gulp modulů a určuje cesty k souborům bude odkazovat v rámci chystaný úkolů:</span><span class="sxs-lookup"><span data-stu-id="4b81f-118">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="4b81f-119">Ve výše uvedeném kódu určuje, které moduly uzlu jsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="4b81f-119">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="4b81f-120">`require` Funkce importuje každý modul tak, aby závislých úloh můžete využívat jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="4b81f-120">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="4b81f-121">Každý z importovaných modulů je přiřazený k proměnné.</span><span class="sxs-lookup"><span data-stu-id="4b81f-121">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="4b81f-122">Moduly můžou být buď podle názvu nebo cesty.</span><span class="sxs-lookup"><span data-stu-id="4b81f-122">The modules can be located either by name or path.</span></span> <span data-ttu-id="4b81f-123">V tomto příkladu s názvem moduly `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, a `gulp-uglify` se načítají podle názvu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-123">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="4b81f-124">Kromě toho řadu cesty jsou vytvořeny tak, aby umístění souborů CSS a JavaScript můžete opakovaně používat a odkazuje v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-124">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="4b81f-125">Následující tabulka obsahuje popis modulů, které jsou součástí *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-125">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

|<span data-ttu-id="4b81f-126">Název modulu</span><span class="sxs-lookup"><span data-stu-id="4b81f-126">Module Name</span></span>|<span data-ttu-id="4b81f-127">Popis</span><span class="sxs-lookup"><span data-stu-id="4b81f-127">Description</span></span>|
|---|---|
|<span data-ttu-id="4b81f-128">gulp</span><span class="sxs-lookup"><span data-stu-id="4b81f-128">gulp</span></span>|<span data-ttu-id="4b81f-129">Systém Gulp streamování sestavení.</span><span class="sxs-lookup"><span data-stu-id="4b81f-129">The Gulp streaming build system.</span></span> <span data-ttu-id="4b81f-130">Další informace najdete v tématu [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="4b81f-130">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span>|
|<span data-ttu-id="4b81f-131">rimraf</span><span class="sxs-lookup"><span data-stu-id="4b81f-131">rimraf</span></span>|<span data-ttu-id="4b81f-132">Modul odstranění uzlu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-132">A Node deletion module.</span></span> <span data-ttu-id="4b81f-133">Další informace najdete v tématu [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="4b81f-133">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span>|
|<span data-ttu-id="4b81f-134">gulp concat</span><span class="sxs-lookup"><span data-stu-id="4b81f-134">gulp-concat</span></span>|<span data-ttu-id="4b81f-135">Modul, který řetězí soubory podle znak nového řádku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4b81f-135">A module that concatenates files based on the operating system’s newline character.</span></span> <span data-ttu-id="4b81f-136">Další informace najdete v tématu [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="4b81f-136">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span>|
|<span data-ttu-id="4b81f-137">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="4b81f-137">gulp-cssmin</span></span>|<span data-ttu-id="4b81f-138">Modul, který minifikuje soubory šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-138">A module that minifies CSS files.</span></span> <span data-ttu-id="4b81f-139">Další informace najdete v tématu [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="4b81f-139">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span>|
|<span data-ttu-id="4b81f-140">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="4b81f-140">gulp-uglify</span></span>|<span data-ttu-id="4b81f-141">Modul, který minifikuje *.js* soubory.</span><span class="sxs-lookup"><span data-stu-id="4b81f-141">A module that minifies *.js* files.</span></span> <span data-ttu-id="4b81f-142">Další informace najdete v tématu [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="4b81f-142">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span>|

<span data-ttu-id="4b81f-143">Jakmile požadavků moduly importují, je možné zadat úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-143">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="4b81f-144">Zde jsou šesti úlohy zaregistrován, reprezentována následující kód:</span><span class="sxs-lookup"><span data-stu-id="4b81f-144">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="4b81f-145">Následující tabulka obsahuje vysvětlení úloh, zadaný ve výše uvedeném kódu:</span><span class="sxs-lookup"><span data-stu-id="4b81f-145">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="4b81f-146">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="4b81f-146">Task Name</span></span>|<span data-ttu-id="4b81f-147">Popis</span><span class="sxs-lookup"><span data-stu-id="4b81f-147">Description</span></span>|
|--- |--- |
|<span data-ttu-id="4b81f-148">vyčištění: js</span><span class="sxs-lookup"><span data-stu-id="4b81f-148">clean:js</span></span>|<span data-ttu-id="4b81f-149">Úloha, která používá modul rimraf uzlu odstranění odebrat minifikovaný verzi souboru site.js.</span><span class="sxs-lookup"><span data-stu-id="4b81f-149">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="4b81f-150">vyčištění: šablon stylů css</span><span class="sxs-lookup"><span data-stu-id="4b81f-150">clean:css</span></span>|<span data-ttu-id="4b81f-151">Úloha, která používá modul rimraf uzlu odstranění k odebrání minifikovaný verze souboru site.css.</span><span class="sxs-lookup"><span data-stu-id="4b81f-151">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="4b81f-152">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="4b81f-152">clean</span></span>|<span data-ttu-id="4b81f-153">Úloha, která volá `clean:js` úkolu, a `clean:css` úloh.</span><span class="sxs-lookup"><span data-stu-id="4b81f-153">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="4b81f-154">min:js</span><span class="sxs-lookup"><span data-stu-id="4b81f-154">min:js</span></span>|<span data-ttu-id="4b81f-155">Úloha, která minifikuje a zřetězí všechny .js soubory ve složce js.</span><span class="sxs-lookup"><span data-stu-id="4b81f-155">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="4b81f-156">. Min.js soubory jsou vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="4b81f-156">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="4b81f-157">min:CSS</span><span class="sxs-lookup"><span data-stu-id="4b81f-157">min:css</span></span>|<span data-ttu-id="4b81f-158">Úloha, která minifikuje a zřetězí všechny .css soubory ve složce šablon stylů css.</span><span class="sxs-lookup"><span data-stu-id="4b81f-158">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="4b81f-159">. Min.css soubory jsou vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="4b81f-159">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="4b81f-160">min</span><span class="sxs-lookup"><span data-stu-id="4b81f-160">min</span></span>|<span data-ttu-id="4b81f-161">Úloha, která volá `min:js` úkolu, a `min:css` úloh.</span><span class="sxs-lookup"><span data-stu-id="4b81f-161">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="4b81f-162">Spuštěné úlohy výchozí</span><span class="sxs-lookup"><span data-stu-id="4b81f-162">Running default tasks</span></span>

<span data-ttu-id="4b81f-163">Pokud jste ještě nevytvořili novou webovou aplikaci, vytvořte nový projekt webové aplikace ASP.NET v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b81f-163">If you haven’t already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="4b81f-164">Do projektu přidejte nový soubor JavaScript a pojmenujte ji *gulpfile.js*, zkopírujte následující kód.</span><span class="sxs-lookup"><span data-stu-id="4b81f-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="4b81f-165">Otevřete *package.json* souboru (Přidat není-li existuje) a přidejte následující.</span><span class="sxs-lookup"><span data-stu-id="4b81f-165">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="4b81f-166">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *gulpfile.js*a vyberte **Průzkumník Spouštěče úloh**.</span><span class="sxs-lookup"><span data-stu-id="4b81f-166">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![V Průzkumníku řešení otevřete Průzkumník Spouštěče úloh](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="4b81f-168">**Průzkumník Spouštěče úloh** zobrazuje seznam Gulp úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-168">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="4b81f-169">(Možná budete muset klikněte **aktualizovat** tlačítko, které se zobrazí nalevo od názvu projektu.)</span><span class="sxs-lookup"><span data-stu-id="4b81f-169">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Průzkumník Spouštěče úloh](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="4b81f-171">Pod **úlohy** v **Průzkumník Spouštěče úloh**, klikněte pravým tlačítkem na **čisté**a vyberte **spustit** z místní nabídky.</span><span class="sxs-lookup"><span data-stu-id="4b81f-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Úloha vyčištění Průzkumník Spouštěče úloh](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="4b81f-173">**Průzkumník Spouštěče úloh** vytvoří novou kartu s názvem **čisté** a provádění čisté úlohy, jako je definována v *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it is defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="4b81f-174">Klikněte pravým tlačítkem myši **čisté** úloh a pak vyberte **vazby** > **před sestavení**.</span><span class="sxs-lookup"><span data-stu-id="4b81f-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Průzkumník Spouštěče úloh vazby BeforeBuild](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="4b81f-176">**Před sestavení** vazby nakonfiguruje úlohu vyčištění automatické spuštění před každé sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="4b81f-177">Vazby nastavit s **Průzkumník Spouštěče úloh** jsou uloženy ve formě komentářů v horní části vaší *gulpfile.js* a jsou platné pouze v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b81f-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="4b81f-178">Alternativu, která nevyžaduje Visual Studio, je nakonfigurovat automatické spuštění úlohy gulp ve vaší *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="4b81f-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="4b81f-179">Například to uvedli do vaší *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="4b81f-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="4b81f-180">Teď čistou úlohy je spuštěn při spuštění projektu v sadě Visual Studio nebo pomocí příkazového řádku `dotnet run` příkazu (Spustit `npm install` první).</span><span class="sxs-lookup"><span data-stu-id="4b81f-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the `dotnet run` command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="4b81f-181">Definování a spuštěním nové úlohy</span><span class="sxs-lookup"><span data-stu-id="4b81f-181">Defining and running a new task</span></span>

<span data-ttu-id="4b81f-182">Chcete-li definovat nové úlohy Gulp, změňte *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="4b81f-183">Přidejte následující JavaScript na konec *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="4b81f-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="4b81f-184">Tato úloha je s názvem `first`, a jednoduše zobrazí řetězec.</span><span class="sxs-lookup"><span data-stu-id="4b81f-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="4b81f-185">Uložit *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="4b81f-186">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *gulpfile.js*a vyberte *Průzkumník Spouštěče úloh*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="4b81f-187">V **Průzkumník Spouštěče úloh**, klikněte pravým tlačítkem na **první**a vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4b81f-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Průzkumník Spouštěče úloh spustit první úlohu](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="4b81f-189">Uvidíte, že se zobrazí text výstupu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-189">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="4b81f-190">Pokud vás zajímá v příkladech podle běžný scénář, najdete v části Gulp recepty.</span><span class="sxs-lookup"><span data-stu-id="4b81f-190">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="4b81f-191">Definování a spuštění úloh v řadě</span><span class="sxs-lookup"><span data-stu-id="4b81f-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="4b81f-192">Při spuštění několika úloh úlohy spouštět souběžně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4b81f-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="4b81f-193">Ale pokud potřebujete ke spouštění úloh v určitém pořadí, musíte zadat každý úkol po dokončení, stejně jako úkoly, které závisí na dokončení jiného úkolu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="4b81f-194">Chcete-li definovat řadu úloh, které se spouští v pořadí, nahraďte `first` úlohu, která jste přidali výše v *gulpfile.js* s následující:</span><span class="sxs-lookup"><span data-stu-id="4b81f-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="4b81f-195">Nyní máte tři úkoly: `series:first`, `series:second`, a `series`.</span><span class="sxs-lookup"><span data-stu-id="4b81f-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="4b81f-196">`series:second` Úloh obsahuje druhý parametr, který určuje pole úlohy ke spuštění a dokončení před `series:second` úloha spustí.</span><span class="sxs-lookup"><span data-stu-id="4b81f-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span>  <span data-ttu-id="4b81f-197">Jak je uvedeno v kód výše, jenom `series:first` úloha se musí dokončit před `series:second` úloha spustí.</span><span class="sxs-lookup"><span data-stu-id="4b81f-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="4b81f-198">Uložit *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b81f-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="4b81f-199">V **Průzkumníku řešení**, klikněte pravým tlačítkem na *gulpfile.js* a vyberte **Průzkumník Spouštěče úloh** Pokud již není otevřený.</span><span class="sxs-lookup"><span data-stu-id="4b81f-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn’t already open.</span></span>

4.  <span data-ttu-id="4b81f-200">V **Průzkumník Spouštěče úloh**, klikněte pravým tlačítkem na **řady** a vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4b81f-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Průzkumník Spouštěče úloh spustit úlohu řady](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="4b81f-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="4b81f-202">IntelliSense</span></span>

<span data-ttu-id="4b81f-203">IntelliSense poskytuje doplňování kódu, popisy parametrů a další funkce pro zvýšení produktivity a snížení chyby.</span><span class="sxs-lookup"><span data-stu-id="4b81f-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="4b81f-204">Gulp úlohy jsou napsané v jazyce JavaScript; IntelliSense můžete tedy pomoc při vývoji.</span><span class="sxs-lookup"><span data-stu-id="4b81f-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="4b81f-205">Při práci s JavaScript IntelliSense zobrazí seznam objektů, funkce, vlastnosti a parametry, které jsou k dispozici na základě vaší aktuální kontextu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="4b81f-206">Kódování možnost vyberte v rozevíracím seznamu poskytované IntelliSense pro dokončení kódu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="4b81f-208">Další informace o technologii IntelliSense, najdete v části [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="4b81f-208">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="4b81f-209">Vývoj, pracovní a provozní prostředí</span><span class="sxs-lookup"><span data-stu-id="4b81f-209">Development, staging, and production environments</span></span>

<span data-ttu-id="4b81f-210">Když Gulp je použít k optimalizaci soubory klienta pro pracovní a provozní, zpracované soubory se uloží do místního umístění pracovní a provozní.</span><span class="sxs-lookup"><span data-stu-id="4b81f-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="4b81f-211">*_Layout.cshtml* souborů používá **prostředí** značky pomocná rutina pro zadejte dvě různé verze souborů šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="4b81f-212">Jedna verze souborů šablon stylů CSS je pro vývoj a druhou verzi je optimalizovaná pro pracovní a provozní.</span><span class="sxs-lookup"><span data-stu-id="4b81f-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="4b81f-213">V aplikaci Visual Studio 2017, když změníte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí `Production`, Visual Studio bude vytvoření webové aplikace a odkaz na minimalizovaném okně soubory šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="4b81f-214">Následující kód ukazuje **prostředí** značky obsahující značky odkazu pro pomocné rutiny `Development` šablon stylů CSS souborů a minifikovaný `Staging, Production` soubory šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="4b81f-215">Přepínání mezi prostředími</span><span class="sxs-lookup"><span data-stu-id="4b81f-215">Switching between environments</span></span>

<span data-ttu-id="4b81f-216">Chcete-li přepnout mezi kompilování v různých prostředích, změňte **ASPNETCORE_ENVIRONMENT** hodnotu proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="4b81f-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="4b81f-217">V **Průzkumník Spouštěče úloh**, ověřte, zda **min** úkolu nastaveno ke spuštění **před sestavení**.</span><span class="sxs-lookup"><span data-stu-id="4b81f-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="4b81f-218">V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4b81f-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="4b81f-219">Zobrazí se seznam vlastností pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b81f-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="4b81f-220">Klikněte **ladění** kartě.</span><span class="sxs-lookup"><span data-stu-id="4b81f-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="4b81f-221">Nastavte hodnotu **hostitelský: prostředí** proměnnou prostředí `Production`.</span><span class="sxs-lookup"><span data-stu-id="4b81f-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="4b81f-222">Stiskněte klávesu **F5** ke spuštění aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4b81f-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="4b81f-223">V okně prohlížeče, klikněte pravým tlačítkem na stránku a vyberte **zobrazit zdroj** zobrazení HTML pro stránku.</span><span class="sxs-lookup"><span data-stu-id="4b81f-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="4b81f-224">Všimněte si, že šablony stylů odkazují na minifikovaný soubory šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="4b81f-225">Zavřete prohlížeč k zastavení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b81f-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="4b81f-226">V sadě Visual Studio, vraťte se na seznamu vlastností pro webovou aplikaci a změnit **hostitelský: prostředí** proměnnou prostředí zpět do `Development`.</span><span class="sxs-lookup"><span data-stu-id="4b81f-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="4b81f-227">Stiskněte klávesu **F5** a znovu spusťte aplikaci v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4b81f-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="4b81f-228">V okně prohlížeče, klikněte pravým tlačítkem na stránku a vyberte **zobrazit zdroj** zobrazíte HTML pro stránku.</span><span class="sxs-lookup"><span data-stu-id="4b81f-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="4b81f-229">Všimněte si, že šablony stylů odkazují na nezmenšenou verze souborů šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4b81f-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="4b81f-230">Další informace související s prostředím v ASP.NET Core najdete v tématu [práce s několika prostředí](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="4b81f-230">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="4b81f-231">Podrobnosti úlohy a modulu</span><span class="sxs-lookup"><span data-stu-id="4b81f-231">Task and module details</span></span>

<span data-ttu-id="4b81f-232">Úloha Gulp není zaregistrována název funkce.</span><span class="sxs-lookup"><span data-stu-id="4b81f-232">A Gulp task is registered with a function name.</span></span>  <span data-ttu-id="4b81f-233">Závislosti můžete zadat, pokud jiné úlohy musí být spuštěny před aktuální úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="4b81f-234">Další funkce umožňují spouštět a sledovat Gulp úlohy, a také nastavit zdroj (*src*) a cíl (*cíle*) souborů upravována.</span><span class="sxs-lookup"><span data-stu-id="4b81f-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="4b81f-235">Toto jsou primární funkce Gulp rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="4b81f-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="4b81f-236">Gulp – funkce</span><span class="sxs-lookup"><span data-stu-id="4b81f-236">Gulp Function</span></span>|<span data-ttu-id="4b81f-237">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="4b81f-237">Syntax</span></span>|<span data-ttu-id="4b81f-238">Popis</span><span class="sxs-lookup"><span data-stu-id="4b81f-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="4b81f-239">– úloha</span><span class="sxs-lookup"><span data-stu-id="4b81f-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="4b81f-240">`task` Funkce vytvoří úlohu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-240">The `task` function creates a task.</span></span> <span data-ttu-id="4b81f-241">`name` Parametr definuje název úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="4b81f-242">`deps` Parametr obsahuje pole potřeba provést před spuštěním této úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="4b81f-243">`fn` Parametr představuje funkci zpětného volání, která provádí operace úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b81f-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="4b81f-244">Sledování</span><span class="sxs-lookup"><span data-stu-id="4b81f-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="4b81f-245">`watch` Funkce monitoruje soubory a spustí úlohy, když dojde ke změně souboru.</span><span class="sxs-lookup"><span data-stu-id="4b81f-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="4b81f-246">`glob` Parametr `string` nebo `array` který určuje, které pokud chcete sledovat soubory.</span><span class="sxs-lookup"><span data-stu-id="4b81f-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="4b81f-247">`opts` Parametr poskytuje další soubor možnosti sledování.</span><span class="sxs-lookup"><span data-stu-id="4b81f-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="4b81f-248">src</span><span class="sxs-lookup"><span data-stu-id="4b81f-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="4b81f-249">`src` Funkce obsahuje soubory, které odpovídají glob hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4b81f-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="4b81f-250">`glob` Parametr `string` nebo `array` který určuje, které soubory číst.</span><span class="sxs-lookup"><span data-stu-id="4b81f-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="4b81f-251">`options` Parametr poskytuje možnosti pro další soubory.</span><span class="sxs-lookup"><span data-stu-id="4b81f-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="4b81f-252">Cíle</span><span class="sxs-lookup"><span data-stu-id="4b81f-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="4b81f-253">`dest` Funkce definuje umístění, do kterého lze zapisovat soubory.</span><span class="sxs-lookup"><span data-stu-id="4b81f-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="4b81f-254">`path` Parametr je řetězec nebo funkci, která určuje cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="4b81f-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="4b81f-255">`options` Parametr je objekt, který určuje možnosti složku výstupu.</span><span class="sxs-lookup"><span data-stu-id="4b81f-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="4b81f-256">Další informace referenční dokumentace rozhraní API Gulp najdete v tématu [Gulp dokumentace rozhraní API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="4b81f-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="4b81f-257">Gulp recepty</span><span class="sxs-lookup"><span data-stu-id="4b81f-257">Gulp recipes</span></span>

<span data-ttu-id="4b81f-258">Komunita Gulp poskytuje Gulp [recepty](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="4b81f-258">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="4b81f-259">Tyto recepty obsahovat úlohy Gulp řeší běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="4b81f-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b81f-260">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4b81f-260">Additional resources</span></span>

* [<span data-ttu-id="4b81f-261">Gulp dokumentace</span><span class="sxs-lookup"><span data-stu-id="4b81f-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="4b81f-262">Sdružování a minimalizace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b81f-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="4b81f-263">Použití Grunt v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b81f-263">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
