---
title: "Pomocí AngularJS pro jednostránkové aplikace (SPA)"
author: rick-anderson
description: "Zjistěte, jak vytvořit aplikaci ASP.NET SPA stylu pomocí AngularJS"
keywords: ASP.NET Core, AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>Pomocí AngularJS pro jednostránkové aplikace (SPA) s ASP.NET Core


Podle [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) a [Scott Addie](https://scottaddie.com)

V tomto článku se dozvíte, jak vytvořit aplikaci ASP.NET SPA stylu pomocí AngularJS.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>Co je AngularJS?

[AngularJS](https://angularjs.org/) je moderní architekturu JavaScript z Google běžně používané k práci s jednostránkové aplikace (SPA). AngularJS je open source v rámci licencí MIT a vývoj průběh AngularJS platí pro [jeho úložiště GitHub](https://github.com/angular/angular.js). Knihovny se nazývá úhlová, protože HTML používá úhlová ve tvaru závorky.

AngularJS není knihovnu DOM manipulaci jako jQuery, ale používá podmnožinu názvem jQLite jQuery. AngularJS je primárně založená na deklarativní atributy HTML, které můžete přidat do značek HTML. Můžete zkusit AngularJS v prohlížeči pomocí [školní kód webu](https://www.codeschool.com/courses/shaping-up-with-angularjs) nebo [W3Schools webu](https://www.w3schools.com/angular/).

Tento článek se zaměřuje na AngularJS s některé poznámky, na kterém je úhlová záhlaví.

## <a name="getting-started"></a>Začínáme

Pokud chcete začít používat AngularJS v aplikaci ASP.NET, musíte ji nainstalovat jako součást projektu nebo na ni odkazujte z síti pro doručování obsahu (CDN).

### <a name="installation"></a>Instalace

Existuje několik způsobů, jak přidat AngularJS do vaší aplikace. Pokud začínáte novou webovou aplikaci ASP.NET Core v sadě Visual Studio, můžete přidat pomocí integrovaných AngularJS [Bower](bower.md) podporovat. Otevřete *bower.json*a přidáním položky `dependencies` vlastnost:

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Při ukládání *bower.json* soubor, úhlová bude nainstalován ve vašem projektu *wwwroot/lib* složky. Kromě toho bude zobrazeno v rámci `Dependencies/Bower` složky. Najdete na následující snímek obrazovky.

![Průzkumník řešení s projektem AngularJS](angular/_static/angular-solution-explorer.png)

Dál přidejte `<script>` odkaz na konci `<body>` část stránky HTML nebo *_Layout.cshtml* souboru, jak je vidět tady:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

Doporučuje se, že výrobní aplikace využívat sítím CDN pro běžné knihovny jako AngularJS. AngularJS, můžete odkazovat z jednoho z několika sítím CDN, jako je tato:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Až budete mít odkaz na *angular.js* souboru skriptu, jste připraveni začít používat AngularJS na webových stránkách.

## <a name="key-components"></a>Klíčové komponenty

AngularJS zahrnuje několik hlavní součásti, jako například *direktivy*, *šablony*, *opakovače*, *moduly*,  *řadiče*, *součásti*, *součásti směrovače* a další. Podívejme se, jak tyto součásti spolupracují přidat chování na webové stránky.

### <a name="directives"></a>Direktivy

Používá AngularJS [direktivy](https://docs.angularjs.org/guide/directive) rozšíření HTML s vlastní atributy a prvky. Direktivy AngularJS jsou definovány pomocí `data-ng-*` nebo `ng-*` předpony (`ng` je zkratka pro úhlová). Existují dva typy direktiv AngularJS:

   1. **Primitivní direktivy**: těchto předdefinovaných úhlová tým a jsou součástí rozhraní AngularJS.

   2. **Vlastní direktivy**: Jedná se o vlastní direktivy, které můžete definovat.

Jeden z primitivní direktivy použity ve všech aplikacích AngularJS je `ng-app` – direktiva, což bootstraps aplikace AngularJS. Tato direktiva lze použít `<body>` značka nebo podřízený element obsahu. Podíváme se na příklad v akci. Za předpokladu, že jste v projektu ASP.NET, můžete buď přidat do souboru HTML `wwwroot` složky, nebo přidejte nové akce kontroleru a přidružené zobrazení. V tomto případě byly přidány nové `Directives` metody akce k `HomeController.cs`. Zobrazí se zde přidružené zobrazení:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Chcete-li tyto ukázky nezávisle na sobě, nepoužívám soubor sdílený rozložení. Uvidíte, že jsme dekorované značky body s `ng-app` – direktiva označíte, tato stránka je aplikace AngularJS. `{{2+2}}` Je výraz vazby úhlová dat, který se dozvíte více o za chvíli. Tady je výsledek, pokud spustíte tuto aplikaci:

![Jednoduché úhlová – direktiva](angular/_static/simple-directive.png)

Další primitivní direktivy v AngularJS patří:

`ng-controller`Určuje, který řadič JavaScript je vázán k zobrazení, které.

`ng-model`Určuje model, ke kterému jsou vázány hodnoty vlastností HTML element.

`ng-init`Použitý k inicializaci data aplikací v podobě výrazu v aktuálním oboru.

`ng-if`Odebere nebo daného elementu HTML v modelu DOM podle truthiness výrazu zadat znovu.

`ng-repeat`Daný blok HTML v rámci sady dat se opakuje.

`ng-show`Zobrazí nebo skryje daného elementu HTML na základě zadané výrazu.

Úplný seznam všech primitivní direktivy v AngularJS podporovány, naleznete [části direktivy dokumentaci na webu dokumentace AngularJS](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Datová vazba

Poskytuje AngularJS [datová vazba](https://docs.angularjs.org/guide/databinding) podporu out-of-the-box buď pomocí `ng-bind` – direktiva nebo data, jako vazby syntaxe výrazu `{{expression}}`. AngularJS podporuje obousměrný datové vazby, kde je v synchronizaci se šablonu zobrazení po celou dobu uchovávat data z modelu. Všechny změny do zobrazení se automaticky projeví v modelu. Stejně tak všechny změny v modelu se projeví v zobrazení.

Vytvořit soubor HTML nebo akce kontroleru s doprovodné zobrazením s názvem `Databinding`. V zobrazení zahrnují následující:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Všimněte si, že můžete zobrazit pomocí vazby direktivy nebo data hodnoty modelu (`ng-bind`). Výsledná stránka by měla vypadat takto:

![Jednoduché datové vazby](angular/_static/simple-databinding.png)

### <a name="templates"></a>Šablony

[Šablony](https://docs.angularjs.org/guide/templates) v AngularJS jsou jen prostý stránky HTML označených pomocí direktivy AngularJS a artefakty. Šablona v AngularJS je směs direktivy, výrazy, filtrů a ovládací prvky, které spojují s HTML k zobrazení.

Přidání jiného zobrazení za účelem ukázky šablon a přidejte následující:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

Šablona má direktivy AngularJS jako `ng-app`, `ng-init`, `ng-model` a syntaxe výrazů vazby dat k vytvoření vazby `personName` vlastnost. Spuštěný v prohlížeči, zobrazení vypadá na následující snímek obrazovky:

![Jednoduchý příklad šablony 1](angular/_static/simple-templates-1.png)

Pokud změníte název zadáním vstupní pole, zobrazí se text vedle vstupní pole dynamické aktualizace, zobrazující úhlová obousměrný datové vazby v akci.

![Jednoduchý příklad šablony 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Výrazy

[Výrazy](https://docs.angularjs.org/guide/expression) v AngularJS jsou fragmenty kódu jazyka JavaScript, které jsou zapsány uvnitř `{{ expression }}` syntaxe. Data z těchto výrazů je vázána na HTML stejným způsobem jako `ng-bind` direktivy. Hlavní rozdíl mezi AngularJS výrazy a regulární výrazy jazyka JavaScript je AngularJS, že se vyhodnotí výrazy proti `$scope` objekt v AngularJS.

Výrazy AngularJS v ukázce níže vazby `personName` a jednoduchý JavaScript vypočítány výrazu:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

Příklad spouštění v prohlížeči zobrazí `personName` dat a výsledky výpočet:

![Jednoduché výrazy](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Opakovače

Opakující se ve AngularJS se provádí prostřednictvím direktivu primitivní názvem `ng-repeat`. `ng-repeat` – Direktiva opakuje daného elementu HTML v zobrazení přes délka opakující se pole data. Opakovače v AngularJS, můžete opakovat přes pole řetězců nebo objekty. Tady je příklad použití opakovaných přes pole řetězců:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

[Repeat – direktiva](https://docs.angularjs.org/api/ng/directive/ngRepeat) výstupy řadu položky seznamu v neuspořádaný seznam, jak můžete vidět v nástrojích pro vývojáře zobrazí na tomto snímku obrazovky:

![Příklad opakovače](angular/_static/repeater.png)

Tady je příklad, který bude opakovat přes pole objektů. `ng-init` Směrnice vytváří `names` pole, kde každý prvek je objekt, který obsahuje první a poslední názvy. `ng-repeat` Přiřazení, `name in names`, výstupy položku seznamu pro každý element pole.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

Výstup v tomto případě je stejný jako v předchozím příkladu.

Úhlová poskytuje některé další direktivy, které můžou poskytnout chování na základě toho, kde smyčky v jeho spuštění.

`$index`

Použití `$index` v `ng-repeat` smyčka k určení, který index pozice vaší smyčky aktuálně je na.

`$even`a`$odd`

Použití `$even` v `ng-repeat` opakování ve smyčce bude-li určit, zda je aktuální index ve vaší smyčce i indexované řádek. Podobně, použijte `$odd` k určení, pokud je aktuální index liché indexované řádkem.

`$first`a`$last`

Použití `$first` v `ng-repeat` opakování ve smyčce bude-li určit, zda je aktuální index ve vaší smyčce první řádek. Podobně, použijte `$last` k určení, zda je aktuální index poslední řádek.

Zde je ukázka, která zobrazuje `$index`, `$even`, `$odd`, `$first`, a `$last` v akci:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Zde je výsledný výstup:

![Příklad opakovače 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`je objekt jazyka JavaScript, který funguje jako spojovací mezi zobrazením (šablony) a řadiče (vysvětlení níže). Zobrazit šablonu v AngularJS zná pouze hodnoty, které jsou připojené k `$scope` objektu v kontroleru.

> [!NOTE]
> Na světě modelem MVVM `$scope` objekt v AngularJS je často definován jako ViewModel. Týmem AngularJS odkazuje `$scope` objektu jako datový Model. [Další informace o oborech v AngularJS](https://docs.angularjs.org/guide/scope).

Níže je jednoduchý příklad znázorňující postup nastavení vlastnosti na `$scope` v samostatném souboru JavaScript, *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Sledovat `$scope` parametr předaný řadičem na řádek 2. Tento objekt je zobrazení ví o. Na řádku 3 jsme se nastavení vlastnost s názvem "název" na "Marie Jana".

Co se stane, když konkrétní vlastnost nebyla nalezena v zobrazení? Zobrazení definovaná níže odkazuje na vlastnosti "název" a "Stáří":

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Všimněte si na řádku 9 vás žádáme úhlová zobrazíte vlastnost "název" pomocí syntaxe výrazu. Řádek 10 se pak odkazuje na "Stáří", vlastnosti, která neexistuje. Spuštěné příklad ukazuje název nastaven "Marie Jana" a nic pro stáří. Chybí vlastnosti jsou ignorovány.

![Příklad oboru](angular/_static/scope.png)

### <a name="modules"></a>Moduly

A [modulu](https://docs.angularjs.org/guide/module) v AngularJS je kolekce řadiče, služby, direktivy atd. `angular.module()` Volání funkce slouží k vytváření, registraci a načtení modulů v AngularJS. Všechny moduly, včetně těch, které dodaný výrobcem AngularJS tým a knihoven jiných dodavatelů, měly by být zapsány pomocí `angular.module()` funkce.

Níže je fragment kódu, který ukazuje, jak vytvořit nový modul v AngularJS. První parametr je název modulu. Druhý parametr definuje závislosti na dalších modulů. Později v tomto článku jsme zobrazující jak předat těchto závislostí nelze `angular.module()` volání metody.

```javascript
var personApp = angular.module('personApp', []);
```

Použití `ng-app` – direktiva představují modul AngularJS na stránce. Chcete použít modul, přiřadit název modulu, `personApp` v tomto příkladu k `ng-app` direktivy v našem šablony.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Kontrolery

[Řadiče](https://docs.angularjs.org/guide/controller) AngularJS je první bod položky kódu. `<module name>.controller()` Volání funkce slouží k vytvoření a registrace řadiče v AngularJS. `ng-controller` – Direktiva se používá k reprezentování řadič AngularJS na stránce HTML. Role správce v úhlová je nastavit stav a chování datového modelu (`$scope`). Řadiče není vhodné používat k manipulaci s modelu DOM přímo.

Zde je fragment kódu, který zaregistruje nový řadič. `personApp` Proměnné v tomto fragmentu kódu odkazuje úhlová modul, který je definovaný na řádek 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

Pomocí zobrazení `ng-controller` – direktiva přiřadí názvu řadiče:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

Stránce se zobrazuje "Marie" a "Jana", která odpovídají `firstName` a `lastName` vlastnosti připojené k `$scope` objektu:

![Příklad řadiče](angular/_static/controllers.png)

### <a name="components"></a>Součásti

[Součásti](https://docs.angularjs.org/guide/component) v úhlová 1.5.x povolit pro zapouzdření a možnosti vytváření jednotlivých elementů HTML. V úhlová 1.4.x můžete dosáhnout pomocí metody .directive() stejné funkce.

Pomocí metody .component() je jednodušší vývoj získání funkce direktiva a kontroleru. Mezi další výhody patří; izolace oboru, osvědčené postupy jsou vyplývajících a migrace úhlová 2 bude snazší úloh. `<module name>.component()` Volání funkce slouží k vytvoření a registraci komponenty v AngularJS.

Zde je fragment kódu, který registruje novou součást. `personApp` Proměnné v tomto fragmentu kódu odkazuje úhlová modul, který je definovaný na řádek 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

Zobrazení, které jsme se zobrazení vlastního elementu HTML.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

Přidružené šablony používána komponentou:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

Stránce se zobrazuje "Aftab" a "Ansari", která odpovídají `firstName` a `lastName` vlastnosti připojené k `vm` objektu:

![Příklad součásti](angular/_static/components.png)

### <a name="services"></a>Služby

[Služby](https://docs.angularjs.org/guide/services) v AngularJS běžně se používají pro sdílené kód, který je rychle abstrahované do souboru, který můžete použít po celou dobu úhlová aplikace. Služby líné instance, což znamená, že nebudou existovat instance služby jedině v případě získá používá komponenty, která závisí na službě. Objekty Factory jsou například služby, který se používá v aplikacích AngularJS. Objekty Factory se vytvářejí pomocí `myApp.factory()` funkce volání, kde `myApp` je modul.

Dole je příklad, který ukazuje způsob použití objektů Factory v AngularJS:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Chcete-li zavolat tento objekt faktory kontroleru, předat `personFactory` jako parametr, který se `controller` funkce:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Obraťte se na koncový bod REST pomocí služby

Níže je příklad začátku do konce pomocí služby v AngularJS k interakci s koncovým bodem webové rozhraní API ASP.NET Core. V příkladu získá data z rozhraní Web API a zobrazí data v šabloně zobrazení. Začneme zobrazení nejdřív:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

V tomto zobrazení máme úhlová modul s názvem `PersonsApp` řadič s názvem `personController`. Používáme `ng-repeat` Iterujte přes seznam osob. Jsme odkazují tři vlastní soubory JavaScript na řádky 17-19.

*PersonApp.js* soubor se používá k registraci `PersonsApp` modulu; a syntaxe je podobná předchozích příkladech. Používáme `angular.module` funkce vytvořit novou instanci třídy modul, který Pracujeme s.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Podívejme se na *personFactory.js*, níže. Voláme na modulu `factory` metoda vytvořte objekt pro vytváření. Řádek 12 znázorňuje předdefinované úhlová `$http` služby načítání informací o osoby z webové služby.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

V *personController.js*, voláme modulu `controller` metodu pro vytvoření kontroleru. `$scope` Objektu `people` vlastnost je přiřazen s daty vrácenými ze personFactory (řádku 13).

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Podívejme rychlý přehled webového rozhraní API a model za ní. `Person` Modelu je POCO (prostý původní objekt CLR) s `Id`, `FirstName`, a `LastName` vlastnosti:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

`Person` Řadiče vrátí seznam formátu JSON `Person` objekty:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Podívejme se na aplikaci v akci:

![Výsledky řadiče zobrazení REST](angular/_static/rest-bound.png)

Můžete [zobrazení aplikace struktura na Githubu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Další informace o vytváření struktury aplikace AngularJS, najdete v části [úhlová průvodci správným stylem Jan Papa](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Pokud chcete vytvořit modul AngularJS, řadič, vytváření, směrnice a zobrazit soubory snadno, ujistěte se, rezervovat Sayed Hashimi [SideWaffle pack šablony pro sadu Visual Studio](http://sidewaffle.com/). SideWaffle šablony jsou považovány za standardní zlatý SAYED Hashimi je vedoucí manažer programu na Visual Studio Team Web společnosti Microsoft. V době psaní tohoto textu je k dispozici pro sadu Visual Studio 2012, 2013 a 2015 SideWaffle.

### <a name="routing-and-multiple-views"></a>Směrování a více zobrazení

AngularJS má integrovanou trasy zprostředkovatele pro zpracování SPA (jedné stránky aplikace) na základě navigace. Chcete-li pracovat s směrování v AngularJS, je nutné přidat `angular-route` knihovny pomocí Bower. Se zobrazí [bower.json](#angular-bower-json) soubor odkazuje na začátku tohoto článku, že jsme již na něj odkazují v našem projektu.

Po instalaci balíčku, přidat odkaz na skript (*úhlová route.js*) do zobrazení.

Nyní Podívejme aplikaci osoby mít byla vytváření a do ní přidejte navigace. Nejprve budeme kopie aplikace tak, že vytvoříte novou `PeopleController` akci s názvem `Spa` a odpovídající `Spa.cshtml` zobrazení tak, že zkopírujete Index.cshtml zobrazení v `People` složky. Přidat odkaz na skript `angular-route` (viz řádek 11). Také přidat `div` označené jako `ng-view` – direktiva (viz řádek 6) jako zástupný symbol pro umístění zobrazení. Budeme používat několik dalších *.js* soubory, které odkazují na řádky 13-16.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Podívejme se na *personModule.js* souboru chcete zobrazit, jak jsme se vytváření instancí modul se směrováním. Jsme předávání `ngRoute` jako knihovnu do modulu. Tento modul zpracuje směrování v naší aplikaci.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

*PersonRoutes.js* souboru níže, definuje trasy založené na zprostředkovateli trasy. Řádky 4-7 definují navigační vyslovením efektivně, pokud adresa URL s `/persons` je požadována, použijte šablonu názvem `partials/personlist` projdete `personListController`. Na stránce podrobností s parametrem trasy označují čáry 8 11 `personId`. Pokud adresu URL neodpovídá jeden vzoru, úhlová použije se výchozí hodnota `/persons` zobrazení.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

`personlist.html` Soubor je částečné zobrazení obsahující pouze HTML, které jsou potřebné k zobrazení seznamu osoby.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

Kontroler se definuje pomocí modulu `controller` fungovat v *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Pokud jsme spuštění této aplikace a přejděte do `people/spa#/persons` adresu URL, jsme se zobrazí:

![Zobrazení seznamu osob](angular/_static/spa-persons.png)

Pokud jsme přejděte na stránku podrobností, například `people/spa#/persons/2`, vidíme částečné zobrazení podrobností:

![Zobrazení podrobností osoba](angular/_static/spa-persons-2.png)

Můžete zobrazit úplný zdrojový a všechny soubory v tomto článku není uvedeno na [Githubu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Obslužné rutiny událostí

Existuje řada direktiv v AngularJS, který přidat funkce zpracování událostí vstupní elementům ve vašem HTML DOM Níže je seznam událostí, které jsou integrovány do AngularJS.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> Můžete přidat svoje vlastní obslužné rutiny událostí pomocí [vlastní direktivy funkci v AngularJS](https://docs.angularjs.org/guide/directive).

Podíváme, jak `ng-click` událostí je drátové nahoru. Vytvořte nový soubor JavaScript s názvem *eventHandlerController.js*a přidejte následující:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Všimněte si nové `sayName` fungovat v `eventHandlerController` na řádku 5 výše. Všechny metody provádí pro nyní se zobrazuje upozornění na JavaScript pro uživatele s uvítací zprávy.

Zobrazení níže váže řadiče k událost AngularJS. Řádek 9 obsahuje tlačítko, na kterém `ng-click` úhlová direktiva byl použit. Zavolá naše `sayName` funkce, který je připojen k `$scope` byl předán objekt v tomto zobrazení.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

Spuštěné příklad ukazuje, který kontroleru `sayName` funkce je automaticky volána při kliknutí na tlačítko.

![Click – událost](angular/_static/events.png)

Další podrobnosti o AngularJS předdefinované událostí obslužnou rutinu direktivy, nezapomeňte head k [webu dokumentace](https://docs.angularjs.org/api/ng/directive/ngClick) z AngularJS.

## <a name="additional-resources"></a>Další zdroje

* [Úhlová dokumentace](https://docs.angularjs.org)

* [Informace o úhlová 2](https://angular.io/)
