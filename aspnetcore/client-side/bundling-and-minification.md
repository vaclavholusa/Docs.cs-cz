---
title: "Sdružování a minimalizace v ASP.NET Core"
author: scottaddie
description: "Informace o optimalizaci statické prostředky ve webové aplikaci ASP.NET Core použitím sdružování a minimalizace techniky."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: 6c233d0957ce9974adbc6112e6194c072aab0b41
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="bundling-and-minification"></a>Sdružování a minimalizace

Podle [Scott Addie](https://twitter.com/Scott_Addie)

Tento článek vysvětluje výhody použití sdružování a minimalizace, včetně použití těchto funkcí s webovými aplikacemi ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Co je sdružování a minimalizace?

Sdružování a minimalizace jsou dvě odlišné výkonu optimalizace, které můžete použít ve webové aplikaci. Použít společně, sdružování a minimalizace vylepšit výkon, snižuje počet požadavků serveru a zmenšení velikosti požadovaných prostředků statické.

Sdružování a minimalizace především zlepšit první čas načítání stránky požadavku. Jakmile požádal na webové stránce v prohlížeči ukládá do mezipaměti statické prostředky (JavaScript, CSS a bitové kopie). V důsledku toho sdružování a minimalizace nemáte výkon při požadavku stejné stránce nebo stránky, na stejném webu požaduje stejné prostředky. Pokud vyprší platnost hlavičky není správně nastaven na prostředky a pokud se nepoužívá sdružování a minimalizace, heuristiky aktuálnosti prohlížeče označit prostředky zastaralé po několik dní. Kromě toho v prohlížeči vyžaduje žádost o ověření pro každý prostředek. V takovém případě sdružování a minimalizace poskytnout zlepšování výkonu i po první požadavek na stránku.

### <a name="bundling"></a>Sdružování

Sdružování kombinuje několik souborů do jediného souboru. Sdružování snižuje počet požadavků serveru, které jsou nezbytné k vykreslení webové prostředku, například na webové stránce. Můžete vytvořit libovolný počet jednotlivých sad speciálně pro šablon stylů CSS, JavaScript, atd. Méně souborů znamená méně požadavky HTTP z prohlížeče na server nebo služba poskytující vaší aplikace. Výsledkem zvýšení výkonu zatížení první stránky.

### <a name="minification"></a>Minimalizace

Minimalizace odstraní nepotřebné znaky z kódu beze změny funkce. Výsledkem je snížení významné velikosti požadovaných prostředků (například šablon stylů CSS, Image a soubory JavaScript). Běžné důsledky minimalizace zahrnují zkrátit názvy proměnných jeden znak a odebírání komentářů a nepotřebné prázdné znaky.

Vezměte v úvahu následující funkce JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimalizace snižuje funkce takto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Kromě odebírání komentářů a nepotřebné prázdné znaky, byly následující názvy parametrů a proměnnou přejmenovat následujícím způsobem:

Původní | Přejmenovat
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Dopad sdružování a minimalizace

Následující tabulka popisuje rozdíly mezi jednotlivě načítání prostředků a pomocí sdružování a minimalizace:

Akce | S B/M | Bez B/M | Změna
--- | :---: | :---: | :---:
Požadavky na soubory  | 7   | 18     | 157%
Přenést KB | 156 | 264.68 | 70%
Čas načítání (ms) | 885 | 2360   | 167%

Prohlížeče jsou poměrně podrobné s ohledem na hlavičkách žádosti protokolu HTTP. Celkový počet bajtů odeslaných metrika viděli k výraznému snížení při sdružování. Čas načítání ukazuje výrazným vylepšením, ale v tomto příkladu byla spuštěna místně. Větší zvýšení výkonu jsou realizovány při použití sdružování a minimalizace s prostředky přenést přes síť.

## <a name="choose-a-bundling-and-minification-strategy"></a>Volba strategie sdružování a minimalizace

Šablony projektů MVC a stránky Razor poskytují out-of-the-box řešení pro sdružování a minimalizace skládající se z konfiguračního souboru JSON. Nástroje třetích stran, jako [Gulp](xref:client-side/using-gulp) a [Grunt](xref:client-side/using-grunt) úkolů závodníky, provádění stejných úloh s trochu složitější. Nástroj třetí strany je skvělým přizpůsobení, pokud vaše pracovní postup vývoje vyžaduje zpracování nad rámec sdružování a minimalizace&mdash;například optimalizace linting a bitové kopie. Pomocí sdružování a minimalizace návrhu se vytváří minifikovaný soubory před nasazením aplikace. Sdružování a minifikace před nasazením poskytuje výhodu v podobě zatížení serveru. Ale je důležité vědět, že návrhu sdružování a minimalizace zvyšuje složitost sestavení a funguje jenom se statické soubory.

## <a name="configure-bundling-and-minification"></a>Konfigurace sdružování a minimalizace

Projektu šablony MVC a stránky syntaxe Razor poskytují *bundleconfig.json* konfigurační soubor, který definuje možnosti pro každou sadu. Ve výchozím nastavení, je konfigurace jedné sadě definován pro vlastní jazyk JavaScript (*wwwroot/js/site.js*) a šablony stylů (*wwwroot/css/site.css*) souborů:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Možnosti konfigurace patří:

* `outputFileName`: Název souboru kompletu na výstup. Může obsahovat relativní cestu z *bundleconfig.json* souboru. **požadované**
* `inputFiles`: Pole soubory sady společně. Jedná se o relativní cesty k souboru konfigurace. **volitelné**, * prázdnou hodnotu výsledkem prázdná výstupního souboru. [režim expanze](http://www.tldp.org/LDP/abs/html/globbingref.html) vzory jsou podporovány.
* `minify`: Možnosti minimalizace typ výstupu. **volitelné**, *výchozí –`minify: { enabled: true }`*
  * Možnosti konfigurace jsou k dispozici na typ výstupního souboru.
    * [Minifikátor šablon stylů CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minifikátor JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minifikátor HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Příznak určující, zda má-li přidat vygenerované soubory do souboru projektu. **volitelné**, *výchozí - false.*
* `sourceMap`: Příznak, který udává, jestli se má Generovat mapu zdroj pro soubor připojené. **volitelné**, *výchozí - false.*
* `sourceMapRootPath`: Kořenovou cestu pro ukládání vytvořeném zdrojovém souboru mapy.

## <a name="build-time-execution-of-bundling-and-minification"></a>Sestavení – doba provádění sdružování a minimalizace

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) balíček NuGet umožňuje provádění sdružování a minimalizace v čase vytvoření buildu. Vloží balíček [cíle MSBuild](/visualstudio/msbuild/msbuild-targets) kterého spouští v sestavení a vyčištění čas. *Bundleconfig.json* analýzy souboru procesem sestavení nevytvořila výstupní soubory na základě definovaných konfigurace.

> [!NOTE]
> BuildBundlerMinifier patří do komunitou vytvářený projektu na Githubu, pro které společnost Microsoft poskytuje bez podpory. Podává problémy by měl být [zde](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Přidat *BuildBundlerMinifier* balíčku do projektu.

Sestavte projekt. Tímto se zobrazí v okně výstupu:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Vyčistěte projekt. Tímto se zobrazí v okně výstupu:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli) 

Přidat *BuildBundlerMinifier* balíčku do projektu:

```console
dotnet add package BuildBundlerMinifier
```

Pokud používáte ASP.NET základní 1.x, obnovení nově přidaného balíčku:

```console
dotnet restore
```

Sestavení projektu:

```console
dotnet build
```

Zobrazí se následující:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Vyčistěte projekt:

```console
dotnet clean
```

Zobrazí se následující výstup:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Ad hoc provádění sdružování a minimalizace

Je možné spustit úlohy sdružování a minimalizace na základě ad hoc bez vytváření projektu. Přidat [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) balíček NuGet do projektu:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core patří do komunitou vytvářený projektu na Githubu, pro které společnost Microsoft poskytuje bez podpory. Podává problémy by měl být [zde](https://github.com/madskristensen/BundlerMinifier/issues).

Tento balíček rozšiřuje rozhraní příkazového řádku .NET Core zahrnout *dotnet sady* nástroj. V okně konzoly Správce balíčků (PMC) nebo v příkazovém řádku, mohou být provedeny následující příkaz:

```console
dotnet bundle
```

> [!IMPORTANT]
> Správce balíčků NuGet přidá do souboru *.csproj jako závislosti `<PackageReference />` uzlů. `dotnet bundle` Příkaz je zaregistrovaná pomocí rozhraní příkazového řádku .NET Core pouze tehdy, když `<DotNetCliToolReference />` uzel se používá. Upravte soubor *.csproj odpovídajícím způsobem.

## <a name="add-files-to-workflow"></a>Přidání souborů do pracovního postupu

Zvažte příklad, ve kterém Další *custom.css* se přidá soubor připomínající následující:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

K minifikaci *custom.css* a její sady *site.css* do *site.min.css* soubor, přidejte relativní cestu k *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Alternativně může následující vzor expanze názvů:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Tento vzor expanze názvů odpovídá všechny soubory šablon stylů CSS a vyloučí vzor minifikovaný souborů.

Sestavení aplikace. Otevřete *site.min.css* a Všimněte si obsah *custom.css* je připojen na konec souboru.

## <a name="environment-based-bundling-and-minification"></a>Na základě prostředí sdružování a minimalizace

Jako osvědčený postup připojené a minifikovaný soubory aplikace by měla lze použít v produkčním prostředí. Během vývoje zkontrolujte původní soubory pro snazší ladění aplikace.

Zadejte soubory, které chcete zahrnout do vaší stránky pomocí [pomocná značku prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) v zobrazení. Pomocník značku prostředí pouze vykreslí obsah při spuštění v konkrétní [prostředí](xref:fundamentals/environments).

Následující `environment` značky vykreslí nezpracované soubory šablon stylů CSS při spuštění v `Development` prostředí:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

Následující `environment` značky vykreslí souborů CSS připojené a minifikovaný při spuštění v prostředí s jiným než `Development`. Například běžet `Production` nebo `Staging` aktivuje vykreslování těchto šablon:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Využívat bundleconfig.json z Gulp

Existují případy, ve kterých aplikace sdružování a minimalizace pracovní postup vyžaduje další zpracování. Mezi příklady patří Optimalizace bitové kopie, nejnovějších mezipaměti a zpracování asset CDN. Splňovat tyto požadavky, můžete převést sdružování a minimalizace pracovní postup použít Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Použití rozšíření instalující další produkty & Minifikátor

Visual Studio [instalující další produkty & Minifikátor](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozšíření zpracovává převod Gulp.

> [!NOTE]
> Rozšíření instalující další produkty & Minifikátor patří do komunitou vytvářený projektu na Githubu, pro které společnost Microsoft poskytuje bez podpory. Podává problémy by měl být [zde](https://github.com/madskristensen/BundlerMinifier/issues).

Klikněte pravým tlačítkem myši *bundleconfig.json* soubor v Průzkumníku řešení a vyberte **instalující další produkty & Minifikátor** > **převést na Gulp...** :

![Převést na Gulp položky kontextové nabídky](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* a *package.json* soubory budou přidány do projektu. V podporu [npm](https://www.npmjs.com/) balíčky uvedené v *package.json* souboru `devDependencies` části jsou nainstalovány.

Spusťte následující příkaz v okně pomocí PMC nainstalovat rozhraní příkazového řádku Gulp jako globální závislost:

```console
npm i -g gulp-cli
```

*Gulpfile.js* souboru čtení *bundleconfig.json* soubor pro vstupy, výstupy a nastavení.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Převést ručně

Pokud Visual Studio a rozšíření instalující další produkty & Minifikátor nejsou k dispozici, převeďte ručně.

Přidat *package.json* soubor s následující `devDependencies`, do kořenového adresáře projektu:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Nainstalujte závislosti spuštěním následujícího příkazu na stejné úrovni jako *package.json*:

```console
npm i
```

Nainstalujte rozhraní příkazového řádku Gulp jako globální závislost:

```console
npm i -g gulp-cli
```

Kopírování *gulpfile.js* souboru do kořenového adresáře projektu níže:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Spuštění úloh Gulp

Spustit úlohu minimalizace Gulp před sestavení projektu v sadě Visual Studio, přidejte následující [MSBuild cíl](/visualstudio/msbuild/msbuild-targets) *.csproj souboru:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

V tomto příkladu jsou všechny úlohy definované v rámci `MyPreCompileTarget` cílových spouštění před předdefinovanou `Build` cíl. V okně výstupu sady Visual Studio zobrazí se výstup podobný následujícímu:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Průzkumník Spouštěče úloh sady Visual Studio lze případně vázat Gulp úlohy k událostem konkrétní sadě Visual Studio. V tématu [spuštěné úkoly výchozí](xref:client-side/using-gulp#running-default-tasks) pokyny učinit.

## <a name="additional-resources"></a>Další zdroje

* [Použití nástroje Gulp](xref:client-side/using-gulp)
* [Použití nástroje Grunt](xref:client-side/using-grunt)
* [Práce s několika prostředí](xref:fundamentals/environments)
* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
