---
title: "Použití Grunt v ASP.NET Core"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 527373829754757e52ab84b64e04702d649e9062
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="using-grunt-in-aspnet-core"></a>Použití Grunt v ASP.NET Core 

Podle [Noel rýže](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt je Spouštěče úloh JavaScript, který automatizuje minimalizaci skriptů, kompilaci TypeScript, nástrojů pro "hadříkem" kvality kódu, před procesory šablon stylů CSS a téměř žádné opakovaných případě vyžadující, chcete-li podporu vývoje klienta. Grunt je plně podporovaný v sadě Visual Studio, i když šablony projektu ASP.NET ve výchozím nastavení používá Gulp (viz [pomocí Gulp](using-gulp.md)).

Tento příklad používá prázdný projekt ASP.NET Core jako výchozí bod, jak k automatizaci procesu sestavení klienta od začátku.

Dokončení příklad vyčistí cílový adresář nasazení, spojuje soubory JavaScript, zkontroluje kvality kódu, zestruční obsah souboru JavaScript a nasadí do kořenového adresáře vaší webové aplikace. Budeme používat následující balíčky:

* **grunt**: The Grunt úloh runner balíček.

* **Vyčištění contrib grunt**: modul plug-in, který odstraní soubory nebo adresáře.

* **grunt. contrib jshint**: modul plug-in, který kontroluje kvality kódu JavaScript.

* **grunt. contrib concat**: modul plug-in, který spojuje soubory do jediného souboru.

* **grunt contrib uglify**: modul plug-in, který minifikuje JavaScript ke snížení velikosti.

* **grunt. contrib sledovat**: modul plug-in, který sleduje aktivity souborů.

## <a name="preparing-the-application"></a>Příprava aplikace

Pokud chcete začít, nastavte novou prázdnou webovou aplikaci a přidejte soubory TypeScript příklad. Soubory typeScript jsou automaticky zkompilovány do jazyka JavaScript pomocí výchozího nastavení sady Visual Studio a bude naše suroviny ke zpracování pomocí Grunt.

1.  V sadě Visual Studio vytvořte novou `ASP.NET Web Application`.

2.  V **nový projekt ASP.NET** dialogovém okně, vyberte ASP.NET Core **prázdný** šablonu a klikněte na tlačítko OK.

3.  V Průzkumníku řešení přečtěte si strukturu projektu. `\src` Složka obsahuje prázdný `wwwroot` a `Dependencies` uzly.

    ![prázdný webového řešení](using-grunt/_static/grunt-solution-explorer.png)

4.  Přidejte novou složku s názvem `TypeScript` do vašeho adresáře projektu.

5.  Před přidáním jakékoli soubory, Pojďme zajistit, že Visual Studio má možnost "zkompilovat na Uložit ' pro soubory TypeScript se změnami. *Nástroje > Možnosti > textový Editor > Typescript > projektu*

    ![Možnosti nastavení automatického compliation TypeScript souborů](using-grunt/_static/typescript-options.png)

6.  Klikněte pravým tlačítkem myši `TypeScript` adresář a zaškrtněte možnost **Přidat > Nová položka** v místní nabídce. Vyberte **soubor JavaScript** položky a název souboru *Tastes.ts* (Poznámka: \*.ts rozšíření). Zkopírujte na řádek kódu TypeScript níže do souboru (po uložení, nový *Tastes.js* souboru se zobrazí s zdroji JavaScript).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  Přidat druhý soubor, který chcete **TypeScript** adresáře a pojmenujte ji `Food.ts`. Zkopírujte následující kód do souboru.

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

## <a name="configuring-npm"></a>Konfigurace NPM

V dalším kroku nakonfigurujte NPM ke stažení grunt a grunt úlohy.

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **Přidat > Nová položka** v místní nabídce. Vyberte **konfigurační soubor NPM** položky, ponechte výchozí název *package.json*a klikněte na tlačítko **přidat** tlačítko.

2. V *package.json* souboru uvnitř `devDependencies` objektu složené závorky, zadejte "grunt". Vyberte `grunt` z Intellisense seznamu a stiskněte klávesu Enter. Visual Studio bude quote grunt název balíčku a přidat dvojtečkou. Napravo od dvojtečkou vyberte nejnovější stabilní verze balíčku z horní části seznamu Intellisense (stiskněte `Ctrl-Space` nezobrazí Intellisense).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > Používá NPM [sémantické verze](http://semver.org/) k uspořádání závislosti. Sémantické verze, také známé jako SemVer identifikuje balíčky s schématu číslování <major>.<minor>. <patch>. IntelliSense zjednodušuje tím, že zobrazuje pouze několik běžné volby sémantické verze. Horní položku v seznamu Intellisense (0.4.5 v předchozím příkladu), je považován za nejnovější stabilní verze balíčku. Symbol šipka nahoru (^) odpovídá nejnovější hlavní verzi a znak tilda (~) odpovídá nejnovější dílčí verzi. Najdete v článku [NPM semver verze analyzátor odkazu](https://www.npmjs.com/package/semver) jako vodítko k úplné expressivity, která poskytuje SemVer.

3. Přidat další závislosti načíst grunt-contrib -\* balíčky pro *čisté*, *jshint*, *concat*, *uglify*a *sledovat* jak je znázorněno v následujícím příkladu. Verze nepotřebují tak, aby odpovídaly v příkladu.

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

4. Uložit *package.json* souboru.

Balíčky pro každou položku devDependencies stáhne společně s všechny soubory, které vyžaduje každý balíček. Můžete najít soubory v balíčku `node_modules` adresář povolením **zobrazit všechny soubory** tlačítko v Průzkumníku řešení.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Pokud potřebujete, můžete ručně obnovit závislostí v Průzkumníku řešení kliknete pravým tlačítkem na `Dependencies\NPM` a výběrem **obnovení balíčků** možnost nabídky.

![obnovení balíčků](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Konfigurace Grunt

Grunt je konfigurován pomocí manifestu s názvem *Gruntfile.js* který definuje, načte a zaregistruje úlohy, které lze spustit ručně nebo nakonfigurovat tak, aby spustit automaticky v závislosti na události v sadě Visual Studio.

1.  Klikněte pravým tlačítkem na projekt a vyberte **Přidat > Nová položka**. Vyberte **Grunt konfigurační soubor** možnost, ponechte výchozí název *Gruntfile.js*a klikněte na tlačítko **přidat** tlačítko.

    Počáteční kód obsahuje definici modulu a `grunt.initConfig()` metoda. `initConfig()` Se používá k nastavení možností pro každý balíček a bude zbytek modul načíst a zaregistrovat úlohy.
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. Uvnitř `initConfig()` metoda, možnosti pro přidání `clean` úkolů, jak je znázorněno v příkladu *Gruntfile.js* níže. Úloha vyčištění přijímá pole řetězců adresáře. Tato úloha odebere soubory z wwwroot/lib a odebere celý/dočasného adresáře.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Pod metodu initConfig(), přidejte volání `grunt.loadNpmTasks()`. Bude úloha spustitelného ze sady Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Uložit *Gruntfile.js*. Soubor by měl vypadat podobně jako na následující snímek obrazovky.

    ![Počáteční gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. Klikněte pravým tlačítkem na *Gruntfile.js* a vyberte **Průzkumník Spouštěče úloh** v místní nabídce. Otevře se okno Průzkumník Spouštěče úloh.

    ![nabídky Průzkumníka Spouštěče úloh](using-grunt/_static/task-runner-explorer-menu.png)

6. Ověřte, že `clean` zobrazuje v části **úlohy** v Průzkumník Spouštěče úloh.

    ![seznam úloh Průzkumníka Spouštěče úloh](using-grunt/_static/task-runner-explorer-tasks.png)

7. Klikněte pravým tlačítkem na úlohu vyčištění a vyberte **spustit** v místní nabídce. Příkazové okno zobrazí průběh úlohy.

    ![Úloha runner spustit Průzkumníka čistou úloh](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > Neexistují žádné soubory nebo adresáře zatím vyčistit. Pokud chcete můžete vytvořit ručně v Průzkumníku řešení a potom spusťte úlohu vyčištění jako testu.
    
8. V metodě initConfig(), přidejte záznam pro `concat` pomocí kódu níže.

    `src` Pole vlastnosti obsahuje soubory, které chcete kombinovat, v pořadí, že by měl být společně. `dest` Vlastnost přiřadí kombinované soubor, který vytváří cestu.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > `all` Vlastnost ve výše uvedeném kódu je název cíl. Cíle se používají v některé úlohy Grunt umožňuje prostředí s více sestavení. Můžete zobrazit předdefinovaných cílů pomocí Intellisense nebo přiřadit vlastního.
    
9. Přidat `jshint` úloh pomocí kódu níže.

    Spustí nástroj jshint kvality kódu pro každý soubor JavaScript najde v adresáři temp.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > Možnost "-W069" je chybu produkovaný jshint při JavaScript používá závorka syntaxe přiřazení vlastnosti místo zápisu s tečkou, tj. `Tastes["Sweet"]` místo `Tastes.Sweet`. Možnost vypne upozornění umožňující zbytek procesu pokračovat.

10.  Přidat `uglify` úloh pomocí kódu níže.

    Minifikuje úlohu *combined.js* soubor najde v adresáři temp a vytvoří soubor s výsledky v wwwroot/lib následující standardní zásady vytváření názvů  *\<název souboru\>. min.js*.
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. V části grunt.loadNpmTasks() volání, který načítá vyčistit contrib grunt zahrnují stejné volání pro jshint, concat a uglify pomocí kódu níže.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Uložit *Gruntfile.js*. Soubor by měl vypadat podobně jako v příkladu níže.

    ![Příklad souboru dokončení grunt](using-grunt/_static/gruntfile-js-complete.png)

13. Všimněte si, že seznam úloh Průzkumník Spouštěče úloh zahrnuje `clean`, `concat`, `jshint` a `uglify` úlohy. Spusťte každý úkol v pořadí a pozorovat výsledky v Průzkumníku řešení. Každý úkol se budou spouštět bez chyby.
    
    ![Průzkumník Spouštěče úloh spouštět každý úkol](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Vytvoří novou úlohu concat *combined.js* souboru a umístí jej do dočasné složky. Úloha jshint jednoduše spustí a neobsahuje výstup. Vytvoří novou úlohu uglify *combined.min.js* souboru a umístí jej do wwwroot/lib. Při dokončení řešení by měl vypadat podobně jako na následující snímek obrazovky:
    
    ![Průzkumník řešení po všech úloh.](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Další informace o možnostech pro každý balíček, najdete v článku [https://www.npmjs.com/](https://www.npmjs.com/) a vyhledávací název balíčku do vyhledávacího pole na hlavní stránce. Například můžete vyhledat grunt contrib vyčistit balíčku k načtení dokumentace odkaz, který vysvětluje všechny její parametry.

### <a name="all-together-now"></a>Nyní je vše pohromadě

Použít Grunt `registerTask()` metodu pro spuštění řadu úloh v určitém pořadí. Například pro spuštění příkladu výše uvedené kroky v pořadí čistou -> concat -> jshint -> uglify, přidejte následující kód do modulu. Kód musí být přidaní do stejné úrovni jako loadNpmTasks() volání, mimo initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

Nová úloha se zobrazí v Průzkumník Spouštěče úloh v části úlohy Alias. Můžete klikněte pravým tlačítkem myši a spustit ho stejně jako další úlohy. `all` Úloha spustí `clean`, `concat`, `jshint` a `uglify`, v pořadí.

![Alias grunt úlohy](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Sledování změn

A `watch` úkolů udržuje přehled o souborů a adresářů. Hodinek aktivuje úlohy automaticky, pokud zjistí změny. Přidejte do initConfig můžete sledovat změny kód níže \*.js souborů v adresáři TypeScript. Pokud se změnil soubor JavaScript, `watch` se spustí `all` úloh.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Přidejte volání `loadNpmTasks()` zobrazíte `watch` úloha v Průzkumník Spouštěče úloh.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Klikněte pravým tlačítkem na úlohu sledovat v Průzkumník Spouštěče úloh a v místní nabídce vyberte možnost spustit. Příkazové okno zobrazující úlohu sledovat systémem zobrazí "Čekajících..." Zpráva. Otevřete jeden ze souborů TypeScript, přidejte mezeru a pak soubor uložte. To bude aktivovat sledování úkolů a aktivuje další úlohy ke spuštění v pořadí. Následující snímek obrazovky ukazuje ukázka spustit.

![spuštění úlohy výstup](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Vytvoření vazby na události v sadě Visual Studio

Pokud chcete ručně spustit vaše úkoly pokaždé, když pracujete v sadě Visual Studio, můžete vázat úlohy k **před sestavení**, **po sestavení**, **Vyčistit**, a  **Otevřete projekt** události.

Umožňuje vytvořit vazbu `watch` tak, aby běžel pokaždé, když se otevře Visual Studio. V Průzkumník Spouštěče úloh, klikněte pravým tlačítkem na úlohu sledovat a vyberte **vazby > otevření projektu** v místní nabídce.

![vázat úlohy k otevření projektu](using-grunt/_static/bindings-project-open.png)

A opětovném načtení projektu. Pokud projekt znovu načte, sledování úkolů začne automaticky spuštěn.

## <a name="summary"></a>Souhrn

Grunt je runner výkonné úlohy, které je možné automatizovat většinu úloh build klienta. Grunt využívá NPM zajistit jeho balíčky a funkce tooling integrace se sadou Visual Studio. Průzkumník Spouštěče úloh sady Visual Studio rozpoznává změny konfigurační soubory a poskytuje pohodlné rozhraní pro spuštění úlohy, zobrazit spuštěné úlohy a vázat úlohy k událostem v sadě Visual Studio.

## <a name="additional-resources"></a>Další zdroje

   * [Použití nástroje Gulp](using-gulp.md)
