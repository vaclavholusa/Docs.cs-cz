---
title: "Rozhraní MVVM Knockout.js Framework v ASP.NET Core"
author: ardalis
description: 
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Rozhraní MVVM Knockout.js Framework v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Knockout je Oblíbené knihovny JavaScript, která zjednodušuje vytváření složitých založené na datech uživatelská rozhraní. Můžete použít samostatně nebo pomocí jiné knihovny, například jQuery. Jeho primárním účelem je vytvoření vazby prvky uživatelského rozhraní na základní datový model definován jako objekt jazyka JavaScript, tak, aby při provedení změn do rozhraní, dojde k aktualizaci modelu a naopak. Knockout usnadňuje použití vzoru Model-View-ViewModel (modelem MVVM) v chování klienta webové aplikace. Dva hlavní koncepty, kterým musí jeden další při práci s modelem MVVM implementací na Knockout jsou pozorovatelné objekty a vazeb.

## <a name="getting-started"></a>Začínáme

Knockout je nasazený jako jeden soubor JavaScript, takže instalaci a použití se velmi jednoduché pomocí [bower](bower.md). Za předpokladu, že už máte [bower](bower.md) a [gulp](using-gulp.md) nakonfigurovaná, otevřete *bower.json* v ASP.NET Core projektu a přidat závislost knockout, jak je vidět tady:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

Pomocí této na místě můžete ručně spustit bower tak, že otevřete Průzkumník Spouštěče úloh (v zobrazení ‣ ‣ ostatní okna Průzkumník Spouštěče úloh) a pak v části úlohy klikněte pravým tlačítkem na bower a vyberte spustit. Výsledek by měl vypadat přibližně takto:

![bower spuštěné knockout v Průzkumník Spouštěče úloh](knockout/_static/bower-knockout.png)

Teď, když se podíváte ve vašem projektu `wwwroot` složky, měli byste vidět knockout nainstalována ve složce lib.

![knockout nainstalována ve složce lib](knockout/_static/wwwroot-knockout.png)

Doporučujeme, aby v provozním prostředí odkazujete knockout prostřednictvím Content Delivery Network nebo CDN, protože to zvýší pravděpodobnost, že vaši uživatelé budou už máte uložené v mezipaměti kopii souboru a proto nebude nutné ho stáhnout na všech. Knockout je k dispozici na několika sítím CDN, včetně Microsoft Ajax CDN tady:

[http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Chcete-li zahrnout Knockout na stránce, který bude používat, jednoduše přidejte `<script>` element odkazující na soubor z kdekoli kterou budete hostovat ho (s vaší aplikací nebo prostřednictvím název CDN):

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Pozorovatelné objekty, ViewModels a jednoduchá vazba

Může být již obeznámeni s použitím jazyka JavaScript k manipulaci s prvky na webové stránce, buď prostřednictvím přímý přístup k modelu DOM nebo pomocí knihovny jako jQuery. Tento druh chování se obvykle dosahuje psaní kódu pro přímo nastavovat hodnoty prvků v reakci na určité akce uživatele. S Knockout deklarativní způsob je převzat místo, pomocí kterého elementy na stránce je vázána na vlastnosti objektu. Místo psaní kódu k manipulaci s elementů modelu DOM, akcí uživatele jednoduše komunikují s objekt ViewModel a Knockout postará zajistit, že jsou synchronizovány prvky stránky.

Jako jednoduchý příklad zvažte níže uvedeného seznamu na stránce. Obsahuje `<span>` element s `data-bind` atribut, který určuje, že textového obsahu by měla být vázána na authorName. Dále v bloku jazyka JavaScript proměnné viewModel je definován s jedinou vlastností, `authorName`, nastavte na určitou hodnotu. Nakonec volání `ko.applyBindings` přišla předávání v této proměnné viewModel.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

Při zobrazení v prohlížeči, obsah <span> element nahrazen hodnotou proměnné viewModel:

![Jednoduchá vazba knockout](knockout/_static/simple-binding-screenshot.png)

Nyní je k dispozici pracovní jednoduché jednosměrné vazby. Všimněte si, že nikde v kódu nebyla jsme zápisu JavaScript o přiřazení hodnoty k obsahu značku span. Pokud jsme chtěli upravit ViewModel, jsme můžete provádět tento další krok, přidejte jazyka HTML vstupní textové pole a vytvořit vazbu na svou hodnotu, jako je třeba proto:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Opětovné načtení stránky, vidíme, že tato hodnota je skutečně vázána na vstupního pole:

![Vstupní vazba knockout](knockout/_static/input-binding-screenshot.png)

Ale pokud jsme změnit hodnotu do textového pole, odpovídající hodnota v `<span>` element nemění. Proč?

Problém je, že nic upozornění `<span>` , je potřeba aktualizovat. Jednoduše aktualizace ViewModel není dostatečná, samostatně, pokud jsou vlastnosti ViewModel uzavřen do zvláštním typem. Je potřeba použít **pozorovatelné objekty** v ViewModel pro všechny vlastnosti, které je třeba provést změny automaticky aktualizovat, protože k nim dojde. Změnou ViewModel používat `ko.observable("value")` místo právě "value", bude ViewModel aktualizovat všechny elementy HTML, které jsou vázány na hodnotu vždy, když dojde ke změně. Všimněte si, že nemáte vstupních polí až do okamžiku ztráty fokus, takže neuvidíte změny vázané prvky, jako je typ aktualizaci jejich hodnoty.

> [!NOTE]
> Přidání podpory pro živé aktualizace po každé keypress se pouze přidávání `valueUpdate: "afterkeydown"` k `data-bind` obsah atributu. Toto chování můžete získat také pomocí `data-bind="textInput: authorName"` získat rychlých aktualizací hodnot. 

Naše viewModel po aktualizaci ho na použití ko.observable:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Knockout podporuje mnoho různých druhů vazby. Zatím jste viděli jak vytvořit vazbu `text` a `value`. Také můžete vázat na všechny daný atribut. Pro instanci, vytvoříte hypertextový odkaz s značku ukotvení `src` atributu může být vázán viewModel. Knockout podporuje také vazbu na funkce. Ukazuje to umožňuje aktualizovat viewModel zahrnout popisovač twitter autora a zobrazit popisovač služby twitter jako odkaz na stránku služby twitter autora. Provedeme to ve třech fázích.

Nejprve přidejte HTML zobrazíte hypertextový odkaz, který jsme zobrazí v závorkách po jméno autora:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Potom aktualizujte viewModel zahrnout vlastnosti twitterUrl a twitterAlias:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Všimněte si, že v tomto okamžiku nebyly dosud aktualizovali jsme twitterUrl přejít na správnou adresu URL pro tento alias twitter – je jednotka ukazovat na twitter.com. Také Všimněte si, že používáme novou funkci Knockout, `computed`, pro twitterUrl. Toto je pozorovatelné funkce, které oznámí všechny prvky uživatelského rozhraní, pokud se změní. Ale mohla mít přístup k další vlastnosti v viewModel, musíme změnit, jak se vytváří viewModel, tak, aby každá vlastnost své vlastní prohlášení.

Revidované viewModel deklarace jsou uvedeny níže. Nyní je deklarován jako funkce. Všimněte si, zda je každý vlastnost své vlastní prohlášení teď konče středníkem. Všimněte si také, že přístup twitterAlias hodnotu vlastnosti, je potřeba provést, takže jeho odkaz obsahuje ().

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

Výsledek funguje podle očekávání v prohlížeči:

![knockout hypertextový odkaz](knockout/_static/hyperlink-screenshot.png)

Knockout podporuje také vazbu na určité události element uživatelského rozhraní, jako je například klikněte na události. To umožňuje snadno a deklarativně vazby prvky uživatelského rozhraní pro funkce v rámci viewModel aplikace. Jako jednoduchý příklad, přidáme tlačítka, která po kliknutí na upraví autora twitterAlias na velká písmena.

Nejprve přidáme na tlačítko, události a odkazování na název funkce, které vytvoříme pro přidání do viewModel, klikněte na tlačítko vazba na tlačítko:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Pak přidání funkce do viewModel a propojit je upravit viewModel stavu. Všimněte si, že nastavte novou hodnotu pro vlastnost twitterAlias, jsme volat jako metodu a předat novou hodnotu.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

Spuštění kódu a kliknutím na tlačítko upraví odkaz Zobrazit podle očekávání:

![velká počáteční hypertextový odkaz](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Tok řízení

Knockout zahrnuje vazby, které můžete provádět operace podmíněného a opakování. Opakování operace jsou užitečné zejména pro vazby seznam dat do uživatelského rozhraní seznamů, nabídek a mřížky nebo tabulky. Vazba foreach bude iterace pole. Při použití s pozorovatelné pole, se automaticky aktualizuje prvky uživatelského rozhraní při položky jsou přidány nebo odebrány z pole, bez nutnosti znovu vytvářet každý element ve stromové struktuře uživatelského rozhraní. Následující příklad používá nové viewModel, včetně pozorovatelné pole herní výsledky. Je vázána na jednoduché tabulky s pomocí dva sloupce `foreach` vazby `<tbody>` element. Každý `<tr>` v rámci `<tbody>` budou vázána na element gameResults kolekce.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

Všimněte si, že tuto chvíli používáme ViewModel s velkým "V" protože Očekáváme, že můžete vytvořit pomocí "nové" (v applyBindings hovor). Po provedení stránce výsledkem následující výstup:

![model knockout zobrazení záznamu](knockout/_static/record-screenshot.png)

K prokázání, že pozorovatelné kolekce funguje, přidejme trochu další funkce. Jsme můžete zahrnout funkcí záznamu výsledky jinou hru k ViewModel a poté přidejte tlačítka a některé uživatelského rozhraní pro práci pomocí této nové funkce.  Nejdříve vytvoříme metodu addResult:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

BIND – tato metoda tlačítko pomocí `click` vazby:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Otevřete stránku v prohlížeči a klikněte na tlačítko několika dobu, výsledkem je nový řádek tabulky s každou klikněte na:

![Přidat výsledky](knockout/_static/record-addresult-screenshot.png)

Pro podporu přidávání nové záznamy v uživatelském rozhraní, obvykle buď vložené nebo v samostatné formuláře několika způsoby. Jsme můžete snadno upravit tabulku, kterou chcete použít textových polí a dropdownlists tak, aby celou věc je upravovat. Právě změnit `<tr>` element, jak je znázorněno:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Všimněte si, že `$root` odkazuje na kořenový ViewModel, který je, kde jsou umístěny možné volby. `$data`odkazuje na bez ohledu na aktuální model je v daném kontextu – v tomto případě odkazuje na element jednotlivých resultChoices pole, z nichž každý je jednoduchý řetězec.

Díky této změně možné upravovat celou mřížky:

![Upravitelné mřížky](knockout/_static/editable-grid-screenshot.png)

Pokud nebyly používáme Knockout, jsme může dosáhnout všechny informace o použití jQuery, ale s největší pravděpodobností by není možné téměř efektivní. Knockout prvky uživatelského rozhraní, které sleduje vázané datových položek v ViewModel odpovídají a aktualizuje pouze ty prvky, které je třeba přidat, odebrat nebo aktualizovat. By byly třeba významné úsilí dosáhnout označována pomocí jQuery nebo přímé DOM manipulaci, a dokonce i pak potom jsme chtěli zobrazit agregačních výsledků (například záznam win ztrátě) na základě dat v tabulce, by potřebujeme ještě jednou procházení ho a analyzovat Elementů HTML.  S Knockout zobrazení záznamu win ztrátě je jednoduchá. Jsme provádění výpočtů ve ViewModel sám a zobrazit ji vazbou jednoduchý text a `<span>`.

K vytvoření záznamu řetězce win ztrátě, můžeme použít počítaný lze zobrazit. Všimněte si, že odkazuje na pozorovatelné vlastnosti v rámci ViewModel musí být volání funkce, jinak nebude načíst hodnotu lze zobrazit (tj. `gameResults()` není `gameResults` v uvedeném kódu):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Tato funkce vytvořit vazbu na rozpětí v rámci `<h1>` element v horní části stránky:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

Výsledek:

![Win ztrátě](knockout/_static/record-winloss-screenshot.png)

Přidání řádků nebo úprava vybraný prvek v sloupec všechny řádky výsledků zaktualizuje záznam zobrazí v horní části okna.

Kromě vazba na hodnoty, můžete taky téměř jakoukoli právní výrazu jazyka JavaScript v rámci vazbu. Například pokud prvku uživatelského rozhraní by se měla objevit jenom za určitých podmínek, například pokud hodnotu překročí určitou mez, můžete určit to logicky v rámci výrazu vazby:

```html
<div data-bind="visible: customerValue > 100"></div>
```

To `<div>` je zobrazen, jen když customerValue je více než 100.

## <a name="templates"></a>Šablony

Knockout obsahuje podporu pro šablony, abyste mohli snadno oddělit od chování vaší uživatelské rozhraní nebo přírůstkově načíst prvky uživatelského rozhraní na velké aplikaci na vyžádání. Aktualizujeme naše předchozí příklad, aby každý řádek vlastní šablony jednoduše stahování HTML odhlašování do šablony a zadání šablony podle názvu ve volání vytvořit datovou vazbu na `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Knockout také podporuje další ukázka stroje, například knihovny jQuery.tmpl a Underscore.js je ukázka modulu.

## <a name="components"></a>Součásti

Komponenty umožňují uspořádat a opakovaně používat kód uživatelského rozhraní, obvykle spolu s daty ViewModel, na kterém závisí kód uživatelského rozhraní. Pokud chcete vytvořit komponentu, musíte jednoduše zadejte její šablonu a jeho viewModel a pojmenujte ho. To se provádí volání `ko.components.register()`. Kromě definování šablon a vložené viewmodel, mohou být načteny z externích souborů pomocí knihovny jako *require.js*, což je velmi vyčištění a efektivní kódu.

## <a name="communicating-with-apis"></a>Komunikaci s rozhraními API

Knockout můžete pracovat se všechna data ve formátu JSON. Běžný způsob, jak načíst a uložení dat pomocí Knockout je s jQuery, která podporuje `$.getJSON()` funkce načíst data a `$.post()` metodu pro odeslání dat z prohlížeče na koncový bod rozhraní API. Samozřejmě pokud dáváte přednost jiný způsob, jak odesílat a přijímat JSON data, Knockout se s ním pracovat také.

## <a name="summary"></a>Souhrn

Knockout poskytuje jednoduchý, elegantní způsob vazby na aktuální stav aplikace klienta, definována v ViewModel prvky uživatelského rozhraní. Syntaxe vazby na knockout používá atribut data-bind, použít pro elementy HTML, které mají být zpracovány. Knockout je moct efektivně vykreslení a aktualizovat velké sady dat pomocí funkce sledování prvky uživatelského rozhraní a elementy pouze zpracovávat změny vliv. Velké aplikace můžete rozdělit logiku uživatelského rozhraní pomocí šablony a součásti, které mohou být načteny na vyžádání z externích souborů. Verze 3, Knockout se v současné době stabilní knihovna JavaScript, která může zlepšit webových aplikací, které vyžadují interaktivity plně funkčního klienta.
