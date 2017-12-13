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
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="ef69e-103">Rozhraní MVVM Knockout.js Framework v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef69e-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="ef69e-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ef69e-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ef69e-105">Knockout je Oblíbené knihovny JavaScript, která zjednodušuje vytváření složitých založené na datech uživatelská rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ef69e-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="ef69e-106">Můžete použít samostatně nebo pomocí jiné knihovny, například jQuery.</span><span class="sxs-lookup"><span data-stu-id="ef69e-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="ef69e-107">Jeho primárním účelem je vytvoření vazby prvky uživatelského rozhraní na základní datový model definován jako objekt jazyka JavaScript, tak, aby při provedení změn do rozhraní, dojde k aktualizaci modelu a naopak.</span><span class="sxs-lookup"><span data-stu-id="ef69e-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="ef69e-108">Knockout usnadňuje použití vzoru Model-View-ViewModel (modelem MVVM) v chování klienta webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef69e-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="ef69e-109">Dva hlavní koncepty, kterým musí jeden další při práci s modelem MVVM implementací na Knockout jsou pozorovatelné objekty a vazeb.</span><span class="sxs-lookup"><span data-stu-id="ef69e-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ef69e-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="ef69e-110">Getting started</span></span>

<span data-ttu-id="ef69e-111">Knockout je nasazený jako jeden soubor JavaScript, takže instalaci a použití se velmi jednoduché pomocí [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="ef69e-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="ef69e-112">Za předpokladu, že už máte [bower](bower.md) a [gulp](using-gulp.md) nakonfigurovaná, otevřete *bower.json* v ASP.NET Core projektu a přidat závislost knockout, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="ef69e-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

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

<span data-ttu-id="ef69e-113">Pomocí této na místě můžete ručně spustit bower tak, že otevřete Průzkumník Spouštěče úloh (v zobrazení ‣ ‣ ostatní okna Průzkumník Spouštěče úloh) a pak v části úlohy klikněte pravým tlačítkem na bower a vyberte spustit.</span><span class="sxs-lookup"><span data-stu-id="ef69e-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="ef69e-114">Výsledek by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="ef69e-114">The result should appear similar to this:</span></span>

![bower spuštěné knockout v Průzkumník Spouštěče úloh](knockout/_static/bower-knockout.png)

<span data-ttu-id="ef69e-116">Teď, když se podíváte ve vašem projektu `wwwroot` složky, měli byste vidět knockout nainstalována ve složce lib.</span><span class="sxs-lookup"><span data-stu-id="ef69e-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![knockout nainstalována ve složce lib](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="ef69e-118">Doporučujeme, aby v provozním prostředí odkazujete knockout prostřednictvím Content Delivery Network nebo CDN, protože to zvýší pravděpodobnost, že vaši uživatelé budou už máte uložené v mezipaměti kopii souboru a proto nebude nutné ho stáhnout na všech.</span><span class="sxs-lookup"><span data-stu-id="ef69e-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="ef69e-119">Knockout je k dispozici na několika sítím CDN, včetně Microsoft Ajax CDN tady:</span><span class="sxs-lookup"><span data-stu-id="ef69e-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="ef69e-120">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="ef69e-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="ef69e-121">Chcete-li zahrnout Knockout na stránce, který bude používat, jednoduše přidejte `<script>` element odkazující na soubor z kdekoli kterou budete hostovat ho (s vaší aplikací nebo prostřednictvím název CDN):</span><span class="sxs-lookup"><span data-stu-id="ef69e-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="ef69e-122">Pozorovatelné objekty, ViewModels a jednoduchá vazba</span><span class="sxs-lookup"><span data-stu-id="ef69e-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="ef69e-123">Může být již obeznámeni s použitím jazyka JavaScript k manipulaci s prvky na webové stránce, buď prostřednictvím přímý přístup k modelu DOM nebo pomocí knihovny jako jQuery.</span><span class="sxs-lookup"><span data-stu-id="ef69e-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="ef69e-124">Tento druh chování se obvykle dosahuje psaní kódu pro přímo nastavovat hodnoty prvků v reakci na určité akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef69e-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="ef69e-125">S Knockout deklarativní způsob je převzat místo, pomocí kterého elementy na stránce je vázána na vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="ef69e-126">Místo psaní kódu k manipulaci s elementů modelu DOM, akcí uživatele jednoduše komunikují s objekt ViewModel a Knockout postará zajistit, že jsou synchronizovány prvky stránky.</span><span class="sxs-lookup"><span data-stu-id="ef69e-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="ef69e-127">Jako jednoduchý příklad zvažte níže uvedeného seznamu na stránce.</span><span class="sxs-lookup"><span data-stu-id="ef69e-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="ef69e-128">Obsahuje `<span>` element s `data-bind` atribut, který určuje, že textového obsahu by měla být vázána na authorName.</span><span class="sxs-lookup"><span data-stu-id="ef69e-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="ef69e-129">Dále v bloku jazyka JavaScript proměnné viewModel je definován s jedinou vlastností, `authorName`, nastavte na určitou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="ef69e-130">Nakonec volání `ko.applyBindings` přišla předávání v této proměnné viewModel.</span><span class="sxs-lookup"><span data-stu-id="ef69e-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

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

<span data-ttu-id="ef69e-131">Při zobrazení v prohlížeči, obsah <span> element nahrazen hodnotou proměnné viewModel:</span><span class="sxs-lookup"><span data-stu-id="ef69e-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![Jednoduchá vazba knockout](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="ef69e-133">Nyní je k dispozici pracovní jednoduché jednosměrné vazby.</span><span class="sxs-lookup"><span data-stu-id="ef69e-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="ef69e-134">Všimněte si, že nikde v kódu nebyla jsme zápisu JavaScript o přiřazení hodnoty k obsahu značku span.</span><span class="sxs-lookup"><span data-stu-id="ef69e-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="ef69e-135">Pokud jsme chtěli upravit ViewModel, jsme můžete provádět tento další krok, přidejte jazyka HTML vstupní textové pole a vytvořit vazbu na svou hodnotu, jako je třeba proto:</span><span class="sxs-lookup"><span data-stu-id="ef69e-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="ef69e-136">Opětovné načtení stránky, vidíme, že tato hodnota je skutečně vázána na vstupního pole:</span><span class="sxs-lookup"><span data-stu-id="ef69e-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![Vstupní vazba knockout](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="ef69e-138">Ale pokud jsme změnit hodnotu do textového pole, odpovídající hodnota v `<span>` element nemění.</span><span class="sxs-lookup"><span data-stu-id="ef69e-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="ef69e-139">Proč?</span><span class="sxs-lookup"><span data-stu-id="ef69e-139">Why not?</span></span>

<span data-ttu-id="ef69e-140">Problém je, že nic upozornění `<span>` , je potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ef69e-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="ef69e-141">Jednoduše aktualizace ViewModel není dostatečná, samostatně, pokud jsou vlastnosti ViewModel uzavřen do zvláštním typem.</span><span class="sxs-lookup"><span data-stu-id="ef69e-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="ef69e-142">Je potřeba použít **pozorovatelné objekty** v ViewModel pro všechny vlastnosti, které je třeba provést změny automaticky aktualizovat, protože k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="ef69e-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="ef69e-143">Změnou ViewModel používat `ko.observable("value")` místo právě "value", bude ViewModel aktualizovat všechny elementy HTML, které jsou vázány na hodnotu vždy, když dojde ke změně.</span><span class="sxs-lookup"><span data-stu-id="ef69e-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="ef69e-144">Všimněte si, že nemáte vstupních polí až do okamžiku ztráty fokus, takže neuvidíte změny vázané prvky, jako je typ aktualizaci jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ef69e-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="ef69e-145">Přidání podpory pro živé aktualizace po každé keypress se pouze přidávání `valueUpdate: "afterkeydown"` k `data-bind` obsah atributu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="ef69e-146">Toto chování můžete získat také pomocí `data-bind="textInput: authorName"` získat rychlých aktualizací hodnot.</span><span class="sxs-lookup"><span data-stu-id="ef69e-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="ef69e-147">Naše viewModel po aktualizaci ho na použití ko.observable:</span><span class="sxs-lookup"><span data-stu-id="ef69e-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="ef69e-148">Knockout podporuje mnoho různých druhů vazby.</span><span class="sxs-lookup"><span data-stu-id="ef69e-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="ef69e-149">Zatím jste viděli jak vytvořit vazbu `text` a `value`.</span><span class="sxs-lookup"><span data-stu-id="ef69e-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="ef69e-150">Také můžete vázat na všechny daný atribut.</span><span class="sxs-lookup"><span data-stu-id="ef69e-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="ef69e-151">Pro instanci, vytvoříte hypertextový odkaz s značku ukotvení `src` atributu může být vázán viewModel.</span><span class="sxs-lookup"><span data-stu-id="ef69e-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="ef69e-152">Knockout podporuje také vazbu na funkce.</span><span class="sxs-lookup"><span data-stu-id="ef69e-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="ef69e-153">Ukazuje to umožňuje aktualizovat viewModel zahrnout popisovač twitter autora a zobrazit popisovač služby twitter jako odkaz na stránku služby twitter autora.</span><span class="sxs-lookup"><span data-stu-id="ef69e-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="ef69e-154">Provedeme to ve třech fázích.</span><span class="sxs-lookup"><span data-stu-id="ef69e-154">We'll do this in three stages.</span></span>

<span data-ttu-id="ef69e-155">Nejprve přidejte HTML zobrazíte hypertextový odkaz, který jsme zobrazí v závorkách po jméno autora:</span><span class="sxs-lookup"><span data-stu-id="ef69e-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="ef69e-156">Potom aktualizujte viewModel zahrnout vlastnosti twitterUrl a twitterAlias:</span><span class="sxs-lookup"><span data-stu-id="ef69e-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

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

<span data-ttu-id="ef69e-157">Všimněte si, že v tomto okamžiku nebyly dosud aktualizovali jsme twitterUrl přejít na správnou adresu URL pro tento alias twitter – je jednotka ukazovat na twitter.com. Také Všimněte si, že používáme novou funkci Knockout, `computed`, pro twitterUrl.</span><span class="sxs-lookup"><span data-stu-id="ef69e-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com. Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="ef69e-158">Toto je pozorovatelné funkce, které oznámí všechny prvky uživatelského rozhraní, pokud se změní.</span><span class="sxs-lookup"><span data-stu-id="ef69e-158">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="ef69e-159">Ale mohla mít přístup k další vlastnosti v viewModel, musíme změnit, jak se vytváří viewModel, tak, aby každá vlastnost své vlastní prohlášení.</span><span class="sxs-lookup"><span data-stu-id="ef69e-159">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="ef69e-160">Revidované viewModel deklarace jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="ef69e-160">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="ef69e-161">Nyní je deklarován jako funkce.</span><span class="sxs-lookup"><span data-stu-id="ef69e-161">It is now declared as a function.</span></span> <span data-ttu-id="ef69e-162">Všimněte si, zda je každý vlastnost své vlastní prohlášení teď konče středníkem.</span><span class="sxs-lookup"><span data-stu-id="ef69e-162">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="ef69e-163">Všimněte si také, že přístup twitterAlias hodnotu vlastnosti, je potřeba provést, takže jeho odkaz obsahuje ().</span><span class="sxs-lookup"><span data-stu-id="ef69e-163">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

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

<span data-ttu-id="ef69e-164">Výsledek funguje podle očekávání v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="ef69e-164">The result works as expected in the browser:</span></span>

![knockout hypertextový odkaz](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="ef69e-166">Knockout podporuje také vazbu na určité události element uživatelského rozhraní, jako je například klikněte na události.</span><span class="sxs-lookup"><span data-stu-id="ef69e-166">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="ef69e-167">To umožňuje snadno a deklarativně vazby prvky uživatelského rozhraní pro funkce v rámci viewModel aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef69e-167">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="ef69e-168">Jako jednoduchý příklad, přidáme tlačítka, která po kliknutí na upraví autora twitterAlias na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ef69e-168">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="ef69e-169">Nejprve přidáme na tlačítko, události a odkazování na název funkce, které vytvoříme pro přidání do viewModel, klikněte na tlačítko vazba na tlačítko:</span><span class="sxs-lookup"><span data-stu-id="ef69e-169">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="ef69e-170">Pak přidání funkce do viewModel a propojit je upravit viewModel stavu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-170">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="ef69e-171">Všimněte si, že nastavte novou hodnotu pro vlastnost twitterAlias, jsme volat jako metodu a předat novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-171">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

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

<span data-ttu-id="ef69e-172">Spuštění kódu a kliknutím na tlačítko upraví odkaz Zobrazit podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="ef69e-172">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![velká počáteční hypertextový odkaz](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="ef69e-174">Tok řízení</span><span class="sxs-lookup"><span data-stu-id="ef69e-174">Control flow</span></span>

<span data-ttu-id="ef69e-175">Knockout zahrnuje vazby, které můžete provádět operace podmíněného a opakování.</span><span class="sxs-lookup"><span data-stu-id="ef69e-175">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="ef69e-176">Opakování operace jsou užitečné zejména pro vazby seznam dat do uživatelského rozhraní seznamů, nabídek a mřížky nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="ef69e-176">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="ef69e-177">Vazba foreach bude iterace pole.</span><span class="sxs-lookup"><span data-stu-id="ef69e-177">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="ef69e-178">Při použití s pozorovatelné pole, se automaticky aktualizuje prvky uživatelského rozhraní při položky jsou přidány nebo odebrány z pole, bez nutnosti znovu vytvářet každý element ve stromové struktuře uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ef69e-178">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="ef69e-179">Následující příklad používá nové viewModel, včetně pozorovatelné pole herní výsledky.</span><span class="sxs-lookup"><span data-stu-id="ef69e-179">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="ef69e-180">Je vázána na jednoduché tabulky s pomocí dva sloupce `foreach` vazby `<tbody>` element.</span><span class="sxs-lookup"><span data-stu-id="ef69e-180">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="ef69e-181">Každý `<tr>` v rámci `<tbody>` budou vázána na element gameResults kolekce.</span><span class="sxs-lookup"><span data-stu-id="ef69e-181">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

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

<span data-ttu-id="ef69e-182">Všimněte si, že tuto chvíli používáme ViewModel s velkým "V" protože Očekáváme, že můžete vytvořit pomocí "nové" (v applyBindings hovor).</span><span class="sxs-lookup"><span data-stu-id="ef69e-182">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="ef69e-183">Po provedení stránce výsledkem následující výstup:</span><span class="sxs-lookup"><span data-stu-id="ef69e-183">When executed, the page results in the following output:</span></span>

![model knockout zobrazení záznamu](knockout/_static/record-screenshot.png)

<span data-ttu-id="ef69e-185">K prokázání, že pozorovatelné kolekce funguje, přidejme trochu další funkce.</span><span class="sxs-lookup"><span data-stu-id="ef69e-185">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="ef69e-186">Jsme můžete zahrnout funkcí záznamu výsledky jinou hru k ViewModel a poté přidejte tlačítka a některé uživatelského rozhraní pro práci pomocí této nové funkce.</span><span class="sxs-lookup"><span data-stu-id="ef69e-186">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="ef69e-187">Nejdříve vytvoříme metodu addResult:</span><span class="sxs-lookup"><span data-stu-id="ef69e-187">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="ef69e-188">BIND – tato metoda tlačítko pomocí `click` vazby:</span><span class="sxs-lookup"><span data-stu-id="ef69e-188">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="ef69e-189">Otevřete stránku v prohlížeči a klikněte na tlačítko několika dobu, výsledkem je nový řádek tabulky s každou klikněte na:</span><span class="sxs-lookup"><span data-stu-id="ef69e-189">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Přidat výsledky](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="ef69e-191">Pro podporu přidávání nové záznamy v uživatelském rozhraní, obvykle buď vložené nebo v samostatné formuláře několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="ef69e-191">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="ef69e-192">Jsme můžete snadno upravit tabulku, kterou chcete použít textových polí a dropdownlists tak, aby celou věc je upravovat.</span><span class="sxs-lookup"><span data-stu-id="ef69e-192">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="ef69e-193">Právě změnit `<tr>` element, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="ef69e-193">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="ef69e-194">Všimněte si, že `$root` odkazuje na kořenový ViewModel, který je, kde jsou umístěny možné volby.</span><span class="sxs-lookup"><span data-stu-id="ef69e-194">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="ef69e-195">`$data`odkazuje na bez ohledu na aktuální model je v daném kontextu – v tomto případě odkazuje na element jednotlivých resultChoices pole, z nichž každý je jednoduchý řetězec.</span><span class="sxs-lookup"><span data-stu-id="ef69e-195">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="ef69e-196">Díky této změně možné upravovat celou mřížky:</span><span class="sxs-lookup"><span data-stu-id="ef69e-196">With this change, the entire grid becomes editable:</span></span>

![Upravitelné mřížky](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="ef69e-198">Pokud nebyly používáme Knockout, jsme může dosáhnout všechny informace o použití jQuery, ale s největší pravděpodobností by není možné téměř efektivní.</span><span class="sxs-lookup"><span data-stu-id="ef69e-198">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="ef69e-199">Knockout prvky uživatelského rozhraní, které sleduje vázané datových položek v ViewModel odpovídají a aktualizuje pouze ty prvky, které je třeba přidat, odebrat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ef69e-199">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="ef69e-200">By byly třeba významné úsilí dosáhnout označována pomocí jQuery nebo přímé DOM manipulaci, a dokonce i pak potom jsme chtěli zobrazit agregačních výsledků (například záznam win ztrátě) na základě dat v tabulce, by potřebujeme ještě jednou procházení ho a analyzovat Elementů HTML.</span><span class="sxs-lookup"><span data-stu-id="ef69e-200">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="ef69e-201">S Knockout zobrazení záznamu win ztrátě je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="ef69e-201">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="ef69e-202">Jsme provádění výpočtů ve ViewModel sám a zobrazit ji vazbou jednoduchý text a `<span>`.</span><span class="sxs-lookup"><span data-stu-id="ef69e-202">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="ef69e-203">K vytvoření záznamu řetězce win ztrátě, můžeme použít počítaný lze zobrazit.</span><span class="sxs-lookup"><span data-stu-id="ef69e-203">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="ef69e-204">Všimněte si, že odkazuje na pozorovatelné vlastnosti v rámci ViewModel musí být volání funkce, jinak nebude načíst hodnotu lze zobrazit (tj. `gameResults()` není `gameResults` v uvedeném kódu):</span><span class="sxs-lookup"><span data-stu-id="ef69e-204">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="ef69e-205">Tato funkce vytvořit vazbu na rozpětí v rámci `<h1>` element v horní části stránky:</span><span class="sxs-lookup"><span data-stu-id="ef69e-205">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="ef69e-206">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="ef69e-206">The result:</span></span>

![Win ztrátě](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="ef69e-208">Přidání řádků nebo úprava vybraný prvek v sloupec všechny řádky výsledků zaktualizuje záznam zobrazí v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="ef69e-208">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="ef69e-209">Kromě vazba na hodnoty, můžete taky téměř jakoukoli právní výrazu jazyka JavaScript v rámci vazbu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-209">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="ef69e-210">Například pokud prvku uživatelského rozhraní by se měla objevit jenom za určitých podmínek, například pokud hodnotu překročí určitou mez, můžete určit to logicky v rámci výrazu vazby:</span><span class="sxs-lookup"><span data-stu-id="ef69e-210">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="ef69e-211">To `<div>` je zobrazen, jen když customerValue je více než 100.</span><span class="sxs-lookup"><span data-stu-id="ef69e-211">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="ef69e-212">Šablony</span><span class="sxs-lookup"><span data-stu-id="ef69e-212">Templates</span></span>

<span data-ttu-id="ef69e-213">Knockout obsahuje podporu pro šablony, abyste mohli snadno oddělit od chování vaší uživatelské rozhraní nebo přírůstkově načíst prvky uživatelského rozhraní na velké aplikaci na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="ef69e-213">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="ef69e-214">Aktualizujeme naše předchozí příklad, aby každý řádek vlastní šablony jednoduše stahování HTML odhlašování do šablony a zadání šablony podle názvu ve volání vytvořit datovou vazbu na `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="ef69e-214">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

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

<span data-ttu-id="ef69e-215">Knockout také podporuje další ukázka stroje, například knihovny jQuery.tmpl a Underscore.js je ukázka modulu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-215">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="ef69e-216">Součásti</span><span class="sxs-lookup"><span data-stu-id="ef69e-216">Components</span></span>

<span data-ttu-id="ef69e-217">Komponenty umožňují uspořádat a opakovaně používat kód uživatelského rozhraní, obvykle spolu s daty ViewModel, na kterém závisí kód uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ef69e-217">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="ef69e-218">Pokud chcete vytvořit komponentu, musíte jednoduše zadejte její šablonu a jeho viewModel a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="ef69e-218">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="ef69e-219">To se provádí volání `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="ef69e-219">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="ef69e-220">Kromě definování šablon a vložené viewmodel, mohou být načteny z externích souborů pomocí knihovny jako *require.js*, což je velmi vyčištění a efektivní kódu.</span><span class="sxs-lookup"><span data-stu-id="ef69e-220">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="ef69e-221">Komunikaci s rozhraními API</span><span class="sxs-lookup"><span data-stu-id="ef69e-221">Communicating with APIs</span></span>

<span data-ttu-id="ef69e-222">Knockout můžete pracovat se všechna data ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ef69e-222">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="ef69e-223">Běžný způsob, jak načíst a uložení dat pomocí Knockout je s jQuery, která podporuje `$.getJSON()` funkce načíst data a `$.post()` metodu pro odeslání dat z prohlížeče na koncový bod rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ef69e-223">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="ef69e-224">Samozřejmě pokud dáváte přednost jiný způsob, jak odesílat a přijímat JSON data, Knockout se s ním pracovat také.</span><span class="sxs-lookup"><span data-stu-id="ef69e-224">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="ef69e-225">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ef69e-225">Summary</span></span>

<span data-ttu-id="ef69e-226">Knockout poskytuje jednoduchý, elegantní způsob vazby na aktuální stav aplikace klienta, definována v ViewModel prvky uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ef69e-226">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="ef69e-227">Syntaxe vazby na knockout používá atribut data-bind, použít pro elementy HTML, které mají být zpracovány.</span><span class="sxs-lookup"><span data-stu-id="ef69e-227">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="ef69e-228">Knockout je moct efektivně vykreslení a aktualizovat velké sady dat pomocí funkce sledování prvky uživatelského rozhraní a elementy pouze zpracovávat změny vliv.</span><span class="sxs-lookup"><span data-stu-id="ef69e-228">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="ef69e-229">Velké aplikace můžete rozdělit logiku uživatelského rozhraní pomocí šablony a součásti, které mohou být načteny na vyžádání z externích souborů.</span><span class="sxs-lookup"><span data-stu-id="ef69e-229">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="ef69e-230">Verze 3, Knockout se v současné době stabilní knihovna JavaScript, která může zlepšit webových aplikací, které vyžadují interaktivity plně funkčního klienta.</span><span class="sxs-lookup"><span data-stu-id="ef69e-230">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
