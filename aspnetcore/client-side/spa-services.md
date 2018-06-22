---
title: K vytvoření jednostránkové aplikace v ASP.NET Core použijte JavaScriptServices
author: scottaddie
description: Seznamte se s výhodami JavaScriptServices vytvořit jedné stránce aplikace (SPA) zajišťoval ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: c3f454ddd91fadf94e4ee4faa8930d8a89d13833
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279620"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>K vytvoření jednostránkové aplikace v ASP.NET Core použijte JavaScriptServices

Podle [Scott Addie](https://github.com/scottaddie) a [Fiyaz Hasan](http://fiyazhasan.me/)

Jedné stránce aplikace (SPA) je typu oblíbené webové aplikace z důvodu jeho vyplývajících bohatých zkušeností uživatelů. Integrace architektury SPA klienta nebo knihovny, například [úhlová](https://angular.io/) nebo [reagovat](https://facebook.github.io/react/), pomocí rozhraní na straně serveru jako ASP.NET Core může být obtížné. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) byla vyvinuta ke snížení třecí procesu integrace. Umožňuje bezproblémové operace mezi různých klientských a serverových sad technologie.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>Co je JavaScriptServices?

JavaScriptServices je kolekce technologií na straně klienta pro ASP.NET Core. Jeho cílem je pozice ASP.NET Core jako vývojáře upřednostňované serverovou platformu pro sestavování SPA.

JavaScriptServices se skládá ze tří různých balíčků NuGet:
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Tyto balíčky jsou užitečné, pokud je:
* Spuštění JavaScriptu na serveru
* Použít SPA framework nebo knihovny
* Prostředky na straně klienta s Webpack sestavení

Velká část fokusu v tomto článku je umístěn na pomocí balíčku SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Co je SpaServices?

SpaServices byl vytvořen na pozici jako vývojáře upřednostňované serverovou platformu pro sestavování SPA ASP.NET Core. SpaServices není vyžadován k vývoji SPA s ASP.NET Core a nebude vám nutit konkrétní klienta framework.

SpaServices poskytuje užitečné infrastruktury, jako:
* [Dokončení fáze před vykreslením na straně serveru](#server-prerendering)
* [Middleware Webpack vývojářů](#webpack-dev-middleware)
* [Nahrazení aktivního modulu](#hot-module-replacement)
* [Směrování pomocné rutiny](#routing-helpers)

Tyto součástí infrastruktury zvýšit souhrnně, pracovní postup vývoje a prostředí runtime. Součástí může být přijata jednotlivě.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Předpoklady pro použití SpaServices

Chcete-li pracovat s SpaServices, nainstalujte následující:
* [Node.js](https://nodejs.org/) (verze 6 nebo novější) s npm
  * Pokud chcete ověřit tyto součásti jsou nainstalovány a možné najít, spusťte z příkazového řádku následující:

    ```console
    node -v && npm -v
    ```

Poznámka: Pokud nasazujete webovou stránku Azure, nemusíte dělat nic zde &mdash; Node.js je nainstalovaná a k dispozici v serverových prostředích.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Pokud jste v systému Windows pomocí Visual Studio 2017, sada SDK je nainstalován výběrem **vývoj pro různé platformy .NET Core** zatížení.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) balíček NuGet

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Dokončení fáze před vykreslením na straně serveru

Univerzální aplikace (také označované jako isomorphic) je aplikace JavaScript, může spustit jak na serveru a klienta. Úhlová reagují a dalších oblíbených rozhraní poskytují univerzální platformu pro vývoj styl této aplikace. Na nápad je nejdřív vykreslení součástí framework na serveru pomocí Node.js a následně delegovat další provádění klientovi.

ASP.NET Core [značky Pomocníci](xref:mvc/views/tag-helpers/intro) poskytované SpaServices zjednodušení implementace modulů serverové dokončení fáze před vykreslením vyvoláním funkce jazyka JavaScript na serveru.

### <a name="prerequisites"></a>Požadavky

Nainstalujte následující:
* [ASPNET-dokončení fáze před vykreslením](https://www.npmjs.com/package/aspnet-prerendering) balíčku npm:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Konfigurace

Pomocníci značky jsou vytvářeny zjistitelný prostřednictvím registrace oboru názvů v projektu *_ViewImports.cshtml* souboru:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Tyto značky Pomocníci abstraktní rychle rozbor všech komunikaci přímo s nižší úrovně rozhraní API s využitím syntaxe jazyka HTML v zobrazení syntaxe Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module` Značky pomocné rutiny

`asp-prerender-module` Značky pomocné rutiny, používá v předchozím příkladu kódu, provede *ClientApp/dist/main-server.js* na serveru pomocí Node.js. Pro přehlednost na saké *hlavní server.js* je artefakt úloha transpilation TypeScript JavaScript v souboru [Webpack](http://webpack.github.io/) proces sestavení. Definuje vstupní bod zástupce Webpack `main-server`; a traversal graf závislostí pro tento alias začíná na *ClientApp nebo spouštěcí server.ts* souboru:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

V následujícím příkladu úhlová *ClientApp nebo spouštěcí server.ts* využívá soubor `createServerRenderer` funkce a `RenderResult` typ `aspnet-prerendering` npm balíček ke konfiguraci serveru vykreslování prostřednictvím Node.js. Značka jazyka HTML, které jsou určené pro vykreslování na straně serveru je předána volání funkce, vyřešte, které je zabalena v JavaScriptu silného typu `Promise` objektu. `Promise` Násobek objektu je, že asynchronně poskytuje kód HTML na stránku pro vkládání v elementu DOM na zástupný symbol.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data` Značky pomocné rutiny

Při kombinaci s `asp-prerender-module` značky pomocné rutiny, `asp-prerender-data` značky pomocná lze předat kontextové informace ze zobrazení syntaxe Razor JavaScript na straně serveru. Například následující kód předá data uživatele `main-server` modul:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Přijatého `UserName` argument serializován pomocí předdefinovaných serializátor JSON a je uložen v `params.data` objektu. V následujícím příkladu úhlová, data se používají k vytvoření přizpůsobené pozdravu v rámci `h1` element:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Poznámka: Názvy vlastností předaná Pomocníci značky jsou reprezentované pomocí **PascalCase** zápis. Který kontrastu jazyka JavaScript, kde jsou reprezentované stejné názvy vlastností s **camelCase**. Výchozí konfigurace serializace JSON je zodpovědná za tento rozdíl.

Rozšířit na předchozí příklad kódu, data mohou být předána ze serveru do zobrazení pomocí hydrating `globals` vlastnost poskytnuté `resolve` funkce:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` Pole definované uvnitř `globals` objekt je připojený k prohlížeče globální `window` objektu. Tato proměnná zvedání do globálního oboru eliminuje zdvojení úsilí, zejména v jak, se vztahuje k načítání stejná data jednou na serveru a znovu na straně klienta.

![globální proměnné postList připojený k objektu okna](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Middleware Webpack vývojářů

[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) zavádí zjednodušenou vývoj pracovního postupu při němž Webpack sestavení prostředky na vyžádání. Middleware automaticky zkompiluje a slouží prostředky na straně klienta, když je znovu na stránce v prohlížeči. Alternativní způsob je ručně vyvolání Webpack pomocí skriptu buildu npm projektu při změně závislost třetích stran nebo vlastní kód. Npm sestavení skriptu *package.json* souboru je znázorněno v následujícím příkladu:

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Požadavky

Nainstalujte následující:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) balíčku npm:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Konfigurace

Middleware Webpack vývojářů je zaregistrovat do kanálu požadavku HTTP přes následující kód do *Startup.cs* souboru `Configure` metoda:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware` Před musí být volána metoda rozšíření [registrace hostování statických souborů](xref:fundamentals/static-files) prostřednictvím `UseStaticFiles` metoda rozšíření. Z bezpečnostních důvodů se zaregistrujte middleware jenom v případě, že aplikace běží v režimu pro vývoj.

*Webpack.config.js* souboru `output.publicPath` vlastnost informuje middleware můžete sledovat `dist` složku pro změny:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Nahrazení aktivního modulu

Představte si, že je Webpack [aktivního modulu nahrazení](https://webpack.js.org/concepts/hot-module-replacement/) funkce (HMR) jako vývoje [Webpack Dev Middleware](#webpack-dev-middleware). HMR zavádí stejné výhody, ale další zjednodušuje pracovní postup vývoje tím, že automaticky aktualizuje obsah stránky po kompilování změny. Nepleťte si to s aktualizaci prohlížeče, což by narušilo aktuální stav v paměti a ladicí relace SPA. Je aktivní propojení mezi službou Middleware Webpack vývojářů a prohlížeče, což znamená, že změny budou přesunuty do prohlížeče.

### <a name="prerequisites"></a>Požadavky

Nainstalujte následující:
* [webpack horkou middleware](https://www.npmjs.com/package/webpack-hot-middleware) balíčku npm:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Konfigurace

Součást HMR musí zaregistrovat do kanálu požadavku HTTP MVC v `Configure` metoda:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Stejně jako se platí [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` před musí být volána metoda rozšíření `UseStaticFiles` metoda rozšíření. Z bezpečnostních důvodů se zaregistrujte middleware jenom v případě, že aplikace běží v režimu pro vývoj.

*Webpack.config.js* musí definovat souboru `plugins` pole, i když je ponechán prázdný:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Po načtení aplikace v prohlížeči, karta konzoly nástroje developer tools poskytuje potvrzení HMR aktivace:

![Aktivní modulu nahrazení připojené zpráv](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Směrování pomocné rutiny

Ve většině SPA založené na ASP.NET Core budete muset klienta směrování kromě směrování na straně serveru. Systémy směrování SPA a MVC může nezávisle pracovat bez narušení. Existuje, ale jeden okraj případu autority vyzve: Identifikace odpovědi HTTP 404.

Představte si situaci, ve kterém bez přípony trasa `/some/page` se používá. Předpokládejme, požadavek nebude vzor match trasy na straně serveru, ale jeho vzor odpovídat trasy na straně klienta. Teď se podíváme příchozí žádosti pro `/images/user-512.png`, které se obecně očekává najít soubor obrázku na serveru. Pokud této cestě požadovaný prostředek se neshoduje se všechny serverové trasy nebo statický soubor, není pravděpodobné, že aplikace na straně klienta by zpracovat – obecně chcete vrátit 404 stavový kód HTTP.

### <a name="prerequisites"></a>Požadavky

Nainstalujte následující:
* Balíček klienta směrování npm. Jako příklad použijeme úhlová:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Konfigurace

Metody rozšíření s názvem `MapSpaFallbackRoute` je používán `Configure` metoda:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Tip: Trasy jsou vyhodnocovány v pořadí, ve kterém máte nakonfigurované. V důsledku toho `default` trasy v předchozím příkladu kódu je použita jako první porovnávání vzorů.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Vytvoření nového projektu

JavaScriptServices poskytuje šablony předem nakonfigurovaných aplikací. SpaServices se používá v těchto šablon ve spojení s jinou architektury a knihovny, například úhlová, reagují a Redux.

Tyto šablony lze nainstalovat prostřednictvím rozhraní příkazového řádku .NET Core spuštěním následujícího příkazu:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Zobrazí se seznam dostupných šablon SPA:

| Šablony                                 | Krátký název | Jazyk | Značky        |
|:------------------------------------------|:-----------|:---------|:------------|
| Jádro ASP.NET MVC s úhlová             | úhlová    | [C#]     | Web/MVC/SPA |
| Jádro ASP.NET MVC s React.js            | react      | [C#]     | Web/MVC/SPA |
| Jádro ASP.NET MVC s React.js a – obnovení  | reactredux | [C#]     | Web/MVC/SPA |

Pokud chcete vytvořit nový projekt pomocí jedné z šablon SPA, obsahovat **krátký název** šablony v [dotnet nové](/dotnet/core/tools/dotnet-new) příkaz. Následující příkaz vytvoří úhlová aplikace s ASP.NET MVC základní nakonfigurovaná na straně serveru:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Nastavit režim konfigurace modulu runtime

Existují dva režimy primární runtime konfigurace:
* **Vývoj**:
    * Obsahuje mapování zdroje k usnadnění ladění.
    * Není optimalizovat kód klienta pro výkon.
* **Produkční**:
    * Vyloučí zdroj mapy.
    * Optimalizuje kód klienta prostřednictvím sdružování a minimalizace.

ASP.NET Core používá proměnnou prostředí s názvem `ASPNETCORE_ENVIRONMENT` k uložení konfigurace režimu. V tématu **[nastavení prostředí](xref:fundamentals/environments#setting-the-environment)** Další informace.

### <a name="running-with-net-core-cli"></a>Spuštění s .NET Core rozhraní příkazového řádku

Spuštěním následujícího příkazu v kořenovém adresáři projektu obnovte požadované NuGet a npm balíčky:

```console
dotnet restore && npm i
```

Sestavení a spuštění aplikace:

```console
dotnet run
```

Spuštění aplikace na místním hostiteli podle požadavků [režim konfigurace runtime](#runtime-config-mode). Přejdete na `http://localhost:5000` v prohlížeči zobrazí cílová stránka.

### <a name="running-with-visual-studio-2017"></a>Běh Visual Studio 2017

Otevřete *.csproj* souboru vygenerované [dotnet nové](/dotnet/core/tools/dotnet-new) příkaz. Požadované balíčky NuGet a npm se automaticky obnoví po projektem otevřeným. Tento proces obnovení může trvat několik minut a aplikace je připraven ke spuštění po dokončení. Kliknutím na zelené tlačítko spustit nebo klikněte na tlačítko `Ctrl + F5`, a prohlížeči se otevře na cílovou stránku aplikace. Spuštění aplikace na místním hostiteli podle požadavků [režimu runtime konfigurace](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Testování aplikace

SpaServices šablony jsou předem nakonfigurované ke spuštění testů na straně klienta pomocí [Karma](https://karma-runner.github.io/1.0/index.html) a [Jasmine](https://jasmine.github.io/). Jasmine je Oblíbené jednotku testování framework pro JavaScript, zatímco Karma je testu pro tyto testy. Karma je nakonfigurováno pro práci s [Webpack Dev Middleware](#webpack-dev-middleware) tak, že vývojář není vyžadován k zastavení a spuštění testu pokaždé, když jsou provedeny změny. Jestli je kód spuštěn před testovacího případu nebo testovací případ, samotné, test se spouští automaticky.

Jako příklad použijeme úhlová aplikace, dvě Jasmine testovacích případů již pro jsou k dispozici `CounterComponent` v *counter.component.spec.ts* souboru:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Otevřete příkazový řádek v *ClientApp* adresáře. Spusťte následující příkaz:

```console
npm test
```

Skript spustí nástroj test runner Karma, který čte definované v nastavení *karma.conf.js* souboru. Kromě jiných nastavení *karma.conf.js* identifikuje testovacích souborů, které mají být provedeny prostřednictvím jeho `files` pole:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Publikování aplikace

Kombinování generovaného klienta prostředky a publikovaná ASP.NET Core artefakty do balíčku připravená k nasazení může být náročná. Naštěstí SpaServices orchestruje procesu celé publikace s vlastní MSBuild cíl s názvem `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Cíl MSBuild má následující zodpovědnosti:
1. Obnovení balíčků npm
1. Vytvořit sestavení produkční úrovni prostředků třetích stran, na straně klienta
1. Vytvořit produkční úrovni sestavení vlastní prostředků klienta
1. Kopírování generované Webpack prostředky do složky publikování

Cíle MSBuild je volána při spuštění:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Další zdroje

* [Úhlová dokumentace](https://angular.io/docs)
