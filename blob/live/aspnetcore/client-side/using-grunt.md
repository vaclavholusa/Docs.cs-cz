---
title: "Použití Grunt v ASP.NET Core"
author: rick-anderson
description: 
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 471112e9-2c33-454b-96fc-32916102ce73
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 8ae50514ce24c7f9e3bb1e347d5d860e1de43c5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="f390d-103">Použití Grunt v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f390d-103">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="f390d-104">Podle [Noel rýže](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="f390d-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="f390d-105">Grunt je Spouštěče úloh JavaScript, který automatizuje minimalizaci skriptů, kompilaci TypeScript, nástrojů pro "hadříkem" kvality kódu, před procesory šablon stylů CSS a téměř žádné opakovaných případě vyžadující, chcete-li podporu vývoje klienta.</span><span class="sxs-lookup"><span data-stu-id="f390d-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="f390d-106">Grunt je plně podporovaný v sadě Visual Studio, i když šablony projektu ASP.NET ve výchozím nastavení používá Gulp (viz [pomocí Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="f390d-106">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="f390d-107">Tento příklad používá prázdný projekt ASP.NET Core jako výchozí bod, jak k automatizaci procesu sestavení klienta od začátku.</span><span class="sxs-lookup"><span data-stu-id="f390d-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="f390d-108">Dokončení příklad vyčistí cílový adresář nasazení, spojuje soubory JavaScript, zkontroluje kvality kódu, zestruční obsah souboru JavaScript a nasadí do kořenového adresáře vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f390d-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="f390d-109">Budeme používat následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="f390d-109">We will use the following packages:</span></span>

* <span data-ttu-id="f390d-110">**grunt**: The Grunt úloh runner balíček.</span><span class="sxs-lookup"><span data-stu-id="f390d-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="f390d-111">**Vyčištění contrib grunt**: modul plug-in, který odstraní soubory nebo adresáře.</span><span class="sxs-lookup"><span data-stu-id="f390d-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="f390d-112">**grunt. contrib jshint**: modul plug-in, který kontroluje kvality kódu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f390d-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="f390d-113">**grunt. contrib concat**: modul plug-in, který spojuje soubory do jediného souboru.</span><span class="sxs-lookup"><span data-stu-id="f390d-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="f390d-114">**grunt contrib uglify**: modul plug-in, který minifikuje JavaScript ke snížení velikosti.</span><span class="sxs-lookup"><span data-stu-id="f390d-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="f390d-115">**grunt. contrib sledovat**: modul plug-in, který sleduje aktivity souborů.</span><span class="sxs-lookup"><span data-stu-id="f390d-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="f390d-116">Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="f390d-116">Preparing the application</span></span>

<span data-ttu-id="f390d-117">Pokud chcete začít, nastavte novou prázdnou webovou aplikaci a přidejte soubory TypeScript příklad.</span><span class="sxs-lookup"><span data-stu-id="f390d-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="f390d-118">Soubory typeScript jsou automaticky zkompilovány do jazyka JavaScript pomocí výchozího nastavení sady Visual Studio a bude naše suroviny ke zpracování pomocí Grunt.</span><span class="sxs-lookup"><span data-stu-id="f390d-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="f390d-119">V sadě Visual Studio vytvořte novou `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="f390d-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="f390d-120">V **nový projekt ASP.NET** dialogovém okně, vyberte ASP.NET Core **prázdný** šablonu a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="f390d-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="f390d-121">V Průzkumníku řešení přečtěte si strukturu projektu.</span><span class="sxs-lookup"><span data-stu-id="f390d-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="f390d-122">`\src` Složka obsahuje prázdný `wwwroot` a `Dependencies` uzly.</span><span class="sxs-lookup"><span data-stu-id="f390d-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![prázdný webového řešení](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="f390d-124">Přidejte novou složku s názvem `TypeScript` do vašeho adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="f390d-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="f390d-125">Před přidáním jakékoli soubory, Pojďme zajistit, že Visual Studio má možnost "zkompilovat na Uložit ' pro soubory TypeScript se změnami.</span><span class="sxs-lookup"><span data-stu-id="f390d-125">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="f390d-126">*Nástroje > Možnosti > textový Editor > Typescript > projektu*</span><span class="sxs-lookup"><span data-stu-id="f390d-126">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![Možnosti nastavení automatického compliation TypeScript souborů](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="f390d-128">Klikněte pravým tlačítkem myši `TypeScript` adresář a zaškrtněte možnost **Přidat > Nová položka** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f390d-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="f390d-129">Vyberte **soubor JavaScript** položky a název souboru *Tastes.ts* (Poznámka: \*.ts rozšíření).</span><span class="sxs-lookup"><span data-stu-id="f390d-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="f390d-130">Zkopírujte na řádek kódu TypeScript níže do souboru (po uložení, nový *Tastes.js* souboru se zobrazí s zdroji JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f390d-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="f390d-131">Přidat druhý soubor, který chcete **TypeScript** adresáře a pojmenujte ji `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="f390d-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="f390d-132">Zkopírujte následující kód do souboru.</span><span class="sxs-lookup"><span data-stu-id="f390d-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="f390d-133">Konfigurace NPM</span><span class="sxs-lookup"><span data-stu-id="f390d-133">Configuring NPM</span></span>

<span data-ttu-id="f390d-134">V dalším kroku nakonfigurujte NPM ke stažení grunt a grunt úlohy.</span><span class="sxs-lookup"><span data-stu-id="f390d-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="f390d-135">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **Přidat > Nová položka** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f390d-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="f390d-136">Vyberte **konfigurační soubor NPM** položky, ponechte výchozí název *package.json*a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f390d-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="f390d-137">V *package.json* souboru uvnitř `devDependencies` objektu složené závorky, zadejte "grunt".</span><span class="sxs-lookup"><span data-stu-id="f390d-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="f390d-138">Vyberte `grunt` z Intellisense seznamu a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="f390d-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="f390d-139">Visual Studio bude quote grunt název balíčku a přidat dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="f390d-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="f390d-140">Napravo od dvojtečkou vyberte nejnovější stabilní verze balíčku z horní části seznamu Intellisense (stiskněte `Ctrl-Space` Pokud není uvedené Intellisense).</span><span class="sxs-lookup"><span data-stu-id="f390d-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense does not appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="f390d-142">Používá NPM [sémantické verze](http://semver.org/) k uspořádání závislosti.</span><span class="sxs-lookup"><span data-stu-id="f390d-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="f390d-143">Sémantické verze, také známé jako SemVer identifikuje balíčky s schématu číslování <major>.<minor>. <patch>. IntelliSense zjednodušuje tím, že zobrazuje pouze několik běžné volby sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="f390d-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="f390d-144">Horní položku v seznamu Intellisense (0.4.5 v předchozím příkladu), je považován za nejnovější stabilní verze balíčku.</span><span class="sxs-lookup"><span data-stu-id="f390d-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="f390d-145">Symbol šipka nahoru (^) odpovídá nejnovější hlavní verzi a znak tilda (~) odpovídá nejnovější dílčí verzi.</span><span class="sxs-lookup"><span data-stu-id="f390d-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="f390d-146">Najdete v článku [NPM semver verze analyzátor odkazu](https://www.npmjs.com/package/semver) jako vodítko k úplné expressivity, která poskytuje SemVer.</span><span class="sxs-lookup"><span data-stu-id="f390d-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="f390d-147">Přidat další závislosti načíst grunt-contrib -\* balíčky pro *čisté*, *jshint*, *concat*, *uglify*a *sledovat* jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="f390d-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="f390d-148">Verze nemusíte odpovídaly příkladu.</span><span class="sxs-lookup"><span data-stu-id="f390d-148">The versions do not need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="f390d-149">Uložit *package.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="f390d-149">Save the *package.json* file.</span></span>

<span data-ttu-id="f390d-150">Balíčky pro každou položku devDependencies stáhne společně s všechny soubory, které vyžaduje každý balíček.</span><span class="sxs-lookup"><span data-stu-id="f390d-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="f390d-151">Můžete najít soubory v balíčku `node_modules` adresář povolením **zobrazit všechny soubory** tlačítko v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="f390d-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="f390d-153">Pokud potřebujete, můžete ručně obnovit závislostí v Průzkumníku řešení kliknete pravým tlačítkem na `Dependencies\NPM` a výběrem **obnovení balíčků** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="f390d-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![obnovení balíčků](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="f390d-155">Konfigurace Grunt</span><span class="sxs-lookup"><span data-stu-id="f390d-155">Configuring Grunt</span></span>

<span data-ttu-id="f390d-156">Grunt je konfigurován pomocí manifestu s názvem *Gruntfile.js* který definuje, načte a zaregistruje úlohy, které lze spustit ručně nebo nakonfigurovat tak, aby spustit automaticky v závislosti na události v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f390d-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="f390d-157">Klikněte pravým tlačítkem na projekt a vyberte **Přidat > Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f390d-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="f390d-158">Vyberte **Grunt konfigurační soubor** možnost, ponechte výchozí název *Gruntfile.js*a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f390d-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="f390d-159">Počáteční kód obsahuje definici modulu a `grunt.initConfig()` metoda.</span><span class="sxs-lookup"><span data-stu-id="f390d-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="f390d-160">`initConfig()` Se používá k nastavení možností pro každý balíček a bude zbytek modul načíst a zaregistrovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="f390d-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="f390d-161">Uvnitř `initConfig()` metoda, možnosti pro přidání `clean` úkolů, jak je znázorněno v příkladu *Gruntfile.js* níže.</span><span class="sxs-lookup"><span data-stu-id="f390d-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="f390d-162">Úloha vyčištění přijímá pole řetězců adresáře.</span><span class="sxs-lookup"><span data-stu-id="f390d-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="f390d-163">Tato úloha odebere soubory z wwwroot/lib a odebere celý/dočasného adresáře.</span><span class="sxs-lookup"><span data-stu-id="f390d-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="f390d-164">Pod metodu initConfig(), přidejte volání `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="f390d-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="f390d-165">Bude úloha spustitelného ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f390d-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="f390d-166">Uložit *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="f390d-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="f390d-167">Soubor by měl vypadat podobně jako na následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f390d-167">The file should look something like the screenshot below.</span></span>

    ![Počáteční gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="f390d-169">Klikněte pravým tlačítkem na *Gruntfile.js* a vyberte **Průzkumník Spouštěče úloh** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f390d-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="f390d-170">Otevře se okno Průzkumník Spouštěče úloh.</span><span class="sxs-lookup"><span data-stu-id="f390d-170">The Task Runner Explorer window will open.</span></span>

    ![nabídky Průzkumníka Spouštěče úloh](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="f390d-172">Ověřte, že `clean` zobrazuje v části **úlohy** v Průzkumník Spouštěče úloh.</span><span class="sxs-lookup"><span data-stu-id="f390d-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![seznam úloh Průzkumníka Spouštěče úloh](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="f390d-174">Klikněte pravým tlačítkem na úlohu vyčištění a vyberte **spustit** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f390d-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="f390d-175">Příkazové okno zobrazí průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="f390d-175">A command window displays progress of the task.</span></span>

    ![Úloha runner spustit Průzkumníka čistou úloh](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="f390d-177">Neexistují žádné soubory nebo adresáře zatím vyčistit.</span><span class="sxs-lookup"><span data-stu-id="f390d-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="f390d-178">Pokud chcete můžete vytvořit ručně v Průzkumníku řešení a potom spusťte úlohu vyčištění jako testu.</span><span class="sxs-lookup"><span data-stu-id="f390d-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="f390d-179">V metodě initConfig(), přidejte záznam pro `concat` pomocí kódu níže.</span><span class="sxs-lookup"><span data-stu-id="f390d-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="f390d-180">`src` Pole vlastnosti obsahuje soubory, které chcete kombinovat, v pořadí, že by měl být společně.</span><span class="sxs-lookup"><span data-stu-id="f390d-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="f390d-181">`dest` Vlastnost přiřadí kombinované soubor, který vytváří cestu.</span><span class="sxs-lookup"><span data-stu-id="f390d-181">The `dest` property assigns the path to the combined file that is produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="f390d-182">`all` Vlastnost ve výše uvedeném kódu je název cíl.</span><span class="sxs-lookup"><span data-stu-id="f390d-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="f390d-183">Cíle se používají v některé úlohy Grunt umožňuje prostředí s více sestavení.</span><span class="sxs-lookup"><span data-stu-id="f390d-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="f390d-184">Můžete zobrazit předdefinovaných cílů pomocí Intellisense nebo přiřadit vlastního.</span><span class="sxs-lookup"><span data-stu-id="f390d-184">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="f390d-185">Přidat `jshint` úloh pomocí kódu níže.</span><span class="sxs-lookup"><span data-stu-id="f390d-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="f390d-186">Spustí nástroj jshint kvality kódu pro každý soubor JavaScript najde v adresáři temp.</span><span class="sxs-lookup"><span data-stu-id="f390d-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="f390d-187">Možnost "-W069" je chybu produkovaný jshint při JavaScript používá závorka syntaxe přiřazení vlastnosti místo zápisu s tečkou, tj. `Tastes["Sweet"]` místo `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="f390d-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="f390d-188">Možnost vypne upozornění umožňující zbytek procesu pokračovat.</span><span class="sxs-lookup"><span data-stu-id="f390d-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="f390d-189">Přidat `uglify` úloh pomocí kódu níže.</span><span class="sxs-lookup"><span data-stu-id="f390d-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="f390d-190">Minifikuje úlohu *combined.js* soubor najde v adresáři temp a vytvoří soubor s výsledky v wwwroot/lib následující standardní zásady vytváření názvů  *\<název souboru\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="f390d-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="f390d-191">V části grunt.loadNpmTasks() volání, který načítá vyčistit contrib grunt zahrnují stejné volání pro jshint, concat a uglify pomocí kódu níže.</span><span class="sxs-lookup"><span data-stu-id="f390d-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="f390d-192">Uložit *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="f390d-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="f390d-193">Soubor by měl vypadat podobně jako v příkladu níže.</span><span class="sxs-lookup"><span data-stu-id="f390d-193">The file should look something like the example below.</span></span>

    ![Příklad souboru dokončení grunt](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="f390d-195">Všimněte si, že seznam úloh Průzkumník Spouštěče úloh zahrnuje `clean`, `concat`, `jshint` a `uglify` úlohy.</span><span class="sxs-lookup"><span data-stu-id="f390d-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="f390d-196">Spusťte každý úkol v pořadí a pozorovat výsledky v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="f390d-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="f390d-197">Každý úkol se budou spouštět bez chyby.</span><span class="sxs-lookup"><span data-stu-id="f390d-197">Each task should run without errors.</span></span>
    
    ![Průzkumník Spouštěče úloh spouštět každý úkol](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="f390d-199">Vytvoří novou úlohu concat *combined.js* souboru a umístí jej do dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="f390d-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="f390d-200">Úloha jshint jednoduše spustí a neobsahuje výstup.</span><span class="sxs-lookup"><span data-stu-id="f390d-200">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="f390d-201">Vytvoří novou úlohu uglify *combined.min.js* souboru a umístí jej do wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="f390d-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="f390d-202">Při dokončení řešení by měl vypadat podobně jako na následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f390d-202">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![Průzkumník řešení po všech úloh.](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="f390d-204">Další informace o možnostech pro každý balíček, najdete v článku [https://www.npmjs.com/](https://www.npmjs.com/) a vyhledávací název balíčku do vyhledávacího pole na hlavní stránce.</span><span class="sxs-lookup"><span data-stu-id="f390d-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="f390d-205">Například můžete vyhledat grunt contrib vyčistit balíčku k načtení dokumentace odkaz, který vysvětluje všechny její parametry.</span><span class="sxs-lookup"><span data-stu-id="f390d-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="f390d-206">Nyní je vše pohromadě</span><span class="sxs-lookup"><span data-stu-id="f390d-206">All together now</span></span>

<span data-ttu-id="f390d-207">Použít Grunt `registerTask()` metodu pro spuštění řadu úloh v určitém pořadí.</span><span class="sxs-lookup"><span data-stu-id="f390d-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="f390d-208">Například pro spuštění příkladu výše uvedené kroky v pořadí čistou -> concat -> jshint -> uglify, přidejte následující kód do modulu.</span><span class="sxs-lookup"><span data-stu-id="f390d-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="f390d-209">Kód musí být přidaní do stejné úrovni jako loadNpmTasks() volání, mimo initConfig.</span><span class="sxs-lookup"><span data-stu-id="f390d-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="f390d-210">Nová úloha se zobrazí v Průzkumník Spouštěče úloh v části úlohy Alias.</span><span class="sxs-lookup"><span data-stu-id="f390d-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="f390d-211">Můžete klikněte pravým tlačítkem myši a spustit ho stejně jako další úlohy.</span><span class="sxs-lookup"><span data-stu-id="f390d-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="f390d-212">`all` Úloha spustí `clean`, `concat`, `jshint` a `uglify`, v pořadí.</span><span class="sxs-lookup"><span data-stu-id="f390d-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![Alias grunt úlohy](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="f390d-214">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="f390d-214">Watching for changes</span></span>

<span data-ttu-id="f390d-215">A `watch` úkolů udržuje přehled o souborů a adresářů.</span><span class="sxs-lookup"><span data-stu-id="f390d-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="f390d-216">Hodinek aktivuje úlohy automaticky, pokud zjistí změny.</span><span class="sxs-lookup"><span data-stu-id="f390d-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="f390d-217">Přidejte do initConfig můžete sledovat změny kód níže \*.js souborů v adresáři TypeScript.</span><span class="sxs-lookup"><span data-stu-id="f390d-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="f390d-218">Pokud se změnil soubor JavaScript, `watch` se spustí `all` úloh.</span><span class="sxs-lookup"><span data-stu-id="f390d-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="f390d-219">Přidejte volání `loadNpmTasks()` zobrazíte `watch` úloha v Průzkumník Spouštěče úloh.</span><span class="sxs-lookup"><span data-stu-id="f390d-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="f390d-220">Klikněte pravým tlačítkem na úlohu sledovat v Průzkumník Spouštěče úloh a v místní nabídce vyberte možnost spustit.</span><span class="sxs-lookup"><span data-stu-id="f390d-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="f390d-221">Příkazové okno zobrazující úlohu sledovat systémem zobrazí "Čekajících..."</span><span class="sxs-lookup"><span data-stu-id="f390d-221">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="f390d-222">Zpráva.</span><span class="sxs-lookup"><span data-stu-id="f390d-222">message.</span></span> <span data-ttu-id="f390d-223">Otevřete jeden ze souborů TypeScript, přidejte mezeru a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="f390d-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="f390d-224">To bude aktivovat sledování úkolů a aktivuje další úlohy ke spuštění v pořadí.</span><span class="sxs-lookup"><span data-stu-id="f390d-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="f390d-225">Následující snímek obrazovky ukazuje ukázka spustit.</span><span class="sxs-lookup"><span data-stu-id="f390d-225">The screenshot below shows a sample run.</span></span>

![spuštění úlohy výstup](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="f390d-227">Vytvoření vazby na události v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f390d-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="f390d-228">Pokud chcete ručně spustit vaše úkoly pokaždé, když pracujete v sadě Visual Studio, můžete vázat úlohy k **před sestavení**, **po sestavení**, **Vyčistit**, a  **Otevřete projekt** události.</span><span class="sxs-lookup"><span data-stu-id="f390d-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="f390d-229">Umožňuje vytvořit vazbu `watch` tak, aby běžel pokaždé, když se otevře Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f390d-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="f390d-230">V Průzkumník Spouštěče úloh, klikněte pravým tlačítkem na úlohu sledovat a vyberte **vazby > otevření projektu** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f390d-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![vázat úlohy k otevření projektu](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="f390d-232">A opětovném načtení projektu.</span><span class="sxs-lookup"><span data-stu-id="f390d-232">Unload and reload the project.</span></span> <span data-ttu-id="f390d-233">Pokud projekt znovu načte, sledování úkolů začne automaticky spuštěn.</span><span class="sxs-lookup"><span data-stu-id="f390d-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="f390d-234">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f390d-234">Summary</span></span>

<span data-ttu-id="f390d-235">Grunt je runner výkonné úlohy, které je možné automatizovat většinu úloh build klienta.</span><span class="sxs-lookup"><span data-stu-id="f390d-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="f390d-236">Grunt využívá NPM zajistit jeho balíčky a funkce tooling integrace se sadou Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f390d-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="f390d-237">Průzkumník Spouštěče úloh sady Visual Studio rozpoznává změny konfigurační soubory a poskytuje pohodlné rozhraní pro spuštění úlohy, zobrazit spuštěné úlohy a vázat úlohy k událostem v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f390d-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f390d-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f390d-238">Additional resources</span></span>

   * [<span data-ttu-id="f390d-239">Pomocí Gulp</span><span class="sxs-lookup"><span data-stu-id="f390d-239">Using Gulp</span></span>](using-gulp.md)
