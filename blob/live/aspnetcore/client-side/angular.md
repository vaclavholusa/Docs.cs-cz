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
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="c5228-104">Pomocí AngularJS pro jednostránkové aplikace (SPA) s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5228-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="c5228-105">Podle [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c5228-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c5228-106">V tomto článku se dozvíte, jak vytvořit aplikaci ASP.NET SPA stylu pomocí AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="c5228-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c5228-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="c5228-108">Co je AngularJS?</span><span class="sxs-lookup"><span data-stu-id="c5228-108">What is AngularJS?</span></span>

<span data-ttu-id="c5228-109">[AngularJS](https://angularjs.org/) je moderní architekturu JavaScript z Google běžně používané k práci s jednostránkové aplikace (SPA).</span><span class="sxs-lookup"><span data-stu-id="c5228-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="c5228-110">AngularJS je open source v rámci licencí MIT a vývoj průběh AngularJS platí pro [jeho úložiště GitHub](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="c5228-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="c5228-111">Knihovny se nazývá úhlová, protože HTML používá úhlová ve tvaru závorky.</span><span class="sxs-lookup"><span data-stu-id="c5228-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="c5228-112">AngularJS není knihovnu DOM manipulaci jako jQuery, ale používá podmnožinu názvem jQLite jQuery.</span><span class="sxs-lookup"><span data-stu-id="c5228-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="c5228-113">AngularJS je primárně založená na deklarativní atributy HTML, které můžete přidat do značek HTML.</span><span class="sxs-lookup"><span data-stu-id="c5228-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="c5228-114">Můžete zkusit AngularJS v prohlížeči pomocí [školní kód webu](https://www.codeschool.com/courses/shaping-up-with-angularjs) nebo [W3Schools webu](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="c5228-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="c5228-115">Tento článek se zaměřuje na AngularJS s některé poznámky, na kterém je úhlová záhlaví.</span><span class="sxs-lookup"><span data-stu-id="c5228-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="c5228-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="c5228-116">Getting started</span></span>

<span data-ttu-id="c5228-117">Pokud chcete začít používat AngularJS v aplikaci ASP.NET, musíte ji nainstalovat jako součást projektu nebo na ni odkazujte z síti pro doručování obsahu (CDN).</span><span class="sxs-lookup"><span data-stu-id="c5228-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="c5228-118">Instalace</span><span class="sxs-lookup"><span data-stu-id="c5228-118">Installation</span></span>

<span data-ttu-id="c5228-119">Existuje několik způsobů, jak přidat AngularJS do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5228-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="c5228-120">Pokud začínáte novou webovou aplikaci ASP.NET Core v sadě Visual Studio, můžete přidat pomocí integrovaných AngularJS [Bower](bower.md) podporovat.</span><span class="sxs-lookup"><span data-stu-id="c5228-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="c5228-121">Otevřete *bower.json*a přidáním položky `dependencies` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c5228-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="c5228-122">Při ukládání *bower.json* soubor, úhlová bude nainstalován ve vašem projektu *wwwroot/lib* složky.</span><span class="sxs-lookup"><span data-stu-id="c5228-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="c5228-123">Kromě toho bude zobrazeno v rámci `Dependencies/Bower` složky.</span><span class="sxs-lookup"><span data-stu-id="c5228-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="c5228-124">Najdete na následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="c5228-124">See the screenshot below.</span></span>

![Průzkumník řešení s projektem AngularJS](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="c5228-126">Dál přidejte `<script>` odkaz na konci `<body>` část stránky HTML nebo *_Layout.cshtml* souboru, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="c5228-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="c5228-127">Doporučuje se, že výrobní aplikace využívat sítím CDN pro běžné knihovny jako AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="c5228-128">AngularJS, můžete odkazovat z jednoho z několika sítím CDN, jako je tato:</span><span class="sxs-lookup"><span data-stu-id="c5228-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="c5228-129">Až budete mít odkaz na *angular.js* souboru skriptu, jste připraveni začít používat AngularJS na webových stránkách.</span><span class="sxs-lookup"><span data-stu-id="c5228-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="c5228-130">Klíčové komponenty</span><span class="sxs-lookup"><span data-stu-id="c5228-130">Key components</span></span>

<span data-ttu-id="c5228-131">AngularJS zahrnuje několik hlavní součásti, jako například *direktivy*, *šablony*, *opakovače*, *moduly*,  *řadiče*, *součásti*, *součásti směrovače* a další.</span><span class="sxs-lookup"><span data-stu-id="c5228-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="c5228-132">Podívejme se, jak tyto součásti spolupracují přidat chování na webové stránky.</span><span class="sxs-lookup"><span data-stu-id="c5228-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="c5228-133">Direktivy</span><span class="sxs-lookup"><span data-stu-id="c5228-133">Directives</span></span>

<span data-ttu-id="c5228-134">Používá AngularJS [direktivy](https://docs.angularjs.org/guide/directive) rozšíření HTML s vlastní atributy a prvky.</span><span class="sxs-lookup"><span data-stu-id="c5228-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="c5228-135">Direktivy AngularJS jsou definovány pomocí `data-ng-*` nebo `ng-*` předpony (`ng` je zkratka pro úhlová).</span><span class="sxs-lookup"><span data-stu-id="c5228-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="c5228-136">Existují dva typy direktiv AngularJS:</span><span class="sxs-lookup"><span data-stu-id="c5228-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="c5228-137">**Primitivní direktivy**: těchto předdefinovaných úhlová tým a jsou součástí rozhraní AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="c5228-138">**Vlastní direktivy**: Jedná se o vlastní direktivy, které můžete definovat.</span><span class="sxs-lookup"><span data-stu-id="c5228-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="c5228-139">Jeden z primitivní direktivy použity ve všech aplikacích AngularJS je `ng-app` – direktiva, což bootstraps aplikace AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="c5228-140">Tato direktiva lze použít `<body>` značka nebo podřízený element obsahu.</span><span class="sxs-lookup"><span data-stu-id="c5228-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="c5228-141">Podíváme se na příklad v akci.</span><span class="sxs-lookup"><span data-stu-id="c5228-141">Let's see an example in action.</span></span> <span data-ttu-id="c5228-142">Za předpokladu, že jste v projektu ASP.NET, můžete buď přidat do souboru HTML `wwwroot` složky, nebo přidejte nové akce kontroleru a přidružené zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="c5228-143">V tomto případě byly přidány nové `Directives` metody akce k `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="c5228-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="c5228-144">Zobrazí se zde přidružené zobrazení:</span><span class="sxs-lookup"><span data-stu-id="c5228-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="c5228-145">Chcete-li tyto ukázky nezávisle na sobě, nepoužívám soubor sdílený rozložení.</span><span class="sxs-lookup"><span data-stu-id="c5228-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="c5228-146">Uvidíte, že jsme dekorované značky body s `ng-app` – direktiva označíte, tato stránka je aplikace AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="c5228-147">`{{2+2}}` Je výraz vazby úhlová dat, který se dozvíte více o za chvíli.</span><span class="sxs-lookup"><span data-stu-id="c5228-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="c5228-148">Tady je výsledek, pokud spustíte tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c5228-148">Here is the result if you run this application:</span></span>

![Jednoduché úhlová – direktiva](angular/_static/simple-directive.png)

<span data-ttu-id="c5228-150">Další primitivní direktivy v AngularJS patří:</span><span class="sxs-lookup"><span data-stu-id="c5228-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="c5228-151">`ng-controller`Určuje, který řadič JavaScript je vázán k zobrazení, které.</span><span class="sxs-lookup"><span data-stu-id="c5228-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="c5228-152">`ng-model`Určuje model, ke kterému jsou vázány hodnoty vlastností HTML element.</span><span class="sxs-lookup"><span data-stu-id="c5228-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="c5228-153">`ng-init`Použitý k inicializaci data aplikací v podobě výrazu v aktuálním oboru.</span><span class="sxs-lookup"><span data-stu-id="c5228-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="c5228-154">`ng-if`Odebere nebo daného elementu HTML v modelu DOM podle truthiness výrazu zadat znovu.</span><span class="sxs-lookup"><span data-stu-id="c5228-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="c5228-155">`ng-repeat`Daný blok HTML v rámci sady dat se opakuje.</span><span class="sxs-lookup"><span data-stu-id="c5228-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="c5228-156">`ng-show`Zobrazí nebo skryje daného elementu HTML na základě zadané výrazu.</span><span class="sxs-lookup"><span data-stu-id="c5228-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="c5228-157">Úplný seznam všech primitivní direktivy v AngularJS podporovány, naleznete [části direktivy dokumentaci na webu dokumentace AngularJS](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="c5228-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="c5228-158">Datová vazba</span><span class="sxs-lookup"><span data-stu-id="c5228-158">Data binding</span></span>

<span data-ttu-id="c5228-159">Poskytuje AngularJS [datová vazba](https://docs.angularjs.org/guide/databinding) podporu out-of-the-box buď pomocí `ng-bind` – direktiva nebo data, jako vazby syntaxe výrazu `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="c5228-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="c5228-160">AngularJS podporuje obousměrný datové vazby, kde je v synchronizaci se šablonu zobrazení po celou dobu uchovávat data z modelu.</span><span class="sxs-lookup"><span data-stu-id="c5228-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="c5228-161">Všechny změny do zobrazení se automaticky projeví v modelu.</span><span class="sxs-lookup"><span data-stu-id="c5228-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="c5228-162">Stejně tak všechny změny v modelu se projeví v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="c5228-163">Vytvořit soubor HTML nebo akce kontroleru s doprovodné zobrazením s názvem `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="c5228-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="c5228-164">V zobrazení zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="c5228-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="c5228-165">Všimněte si, že můžete zobrazit pomocí vazby direktivy nebo data hodnoty modelu (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="c5228-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="c5228-166">Výsledná stránka by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c5228-166">The resulting page should look like this:</span></span>

![Jednoduché datové vazby](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="c5228-168">Šablony</span><span class="sxs-lookup"><span data-stu-id="c5228-168">Templates</span></span>

<span data-ttu-id="c5228-169">[Šablony](https://docs.angularjs.org/guide/templates) v AngularJS jsou jen prostý stránky HTML označených pomocí direktivy AngularJS a artefakty.</span><span class="sxs-lookup"><span data-stu-id="c5228-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="c5228-170">Šablona v AngularJS je směs direktivy, výrazy, filtrů a ovládací prvky, které spojují s HTML k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="c5228-171">Přidání jiného zobrazení za účelem ukázky šablon a přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="c5228-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="c5228-172">Šablona má direktivy AngularJS jako `ng-app`, `ng-init`, `ng-model` a syntaxe výrazů vazby dat k vytvoření vazby `personName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c5228-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="c5228-173">Spuštěný v prohlížeči, zobrazení vypadá na následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c5228-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Jednoduchý příklad šablony 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="c5228-175">Pokud změníte název zadáním vstupní pole, zobrazí se text vedle vstupní pole dynamické aktualizace, zobrazující úhlová obousměrný datové vazby v akci.</span><span class="sxs-lookup"><span data-stu-id="c5228-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Jednoduchý příklad šablony 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="c5228-177">Výrazy</span><span class="sxs-lookup"><span data-stu-id="c5228-177">Expressions</span></span>

<span data-ttu-id="c5228-178">[Výrazy](https://docs.angularjs.org/guide/expression) v AngularJS jsou fragmenty kódu jazyka JavaScript, které jsou zapsány uvnitř `{{ expression }}` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="c5228-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="c5228-179">Data z těchto výrazů je vázána na HTML stejným způsobem jako `ng-bind` direktivy.</span><span class="sxs-lookup"><span data-stu-id="c5228-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="c5228-180">Hlavní rozdíl mezi AngularJS výrazy a regulární výrazy jazyka JavaScript je AngularJS, že se vyhodnotí výrazy proti `$scope` objekt v AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="c5228-181">Výrazy AngularJS v ukázce níže vazby `personName` a jednoduchý JavaScript vypočítány výrazu:</span><span class="sxs-lookup"><span data-stu-id="c5228-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="c5228-182">Příklad spouštění v prohlížeči zobrazí `personName` dat a výsledky výpočet:</span><span class="sxs-lookup"><span data-stu-id="c5228-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Jednoduché výrazy](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="c5228-184">Opakovače</span><span class="sxs-lookup"><span data-stu-id="c5228-184">Repeaters</span></span>

<span data-ttu-id="c5228-185">Opakující se ve AngularJS se provádí prostřednictvím direktivu primitivní názvem `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="c5228-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="c5228-186">`ng-repeat` – Direktiva opakuje daného elementu HTML v zobrazení přes délka opakující se pole data.</span><span class="sxs-lookup"><span data-stu-id="c5228-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="c5228-187">Opakovače v AngularJS, můžete opakovat přes pole řetězců nebo objekty.</span><span class="sxs-lookup"><span data-stu-id="c5228-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="c5228-188">Tady je příklad použití opakovaných přes pole řetězců:</span><span class="sxs-lookup"><span data-stu-id="c5228-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="c5228-189">[Repeat – direktiva](https://docs.angularjs.org/api/ng/directive/ngRepeat) výstupy řadu položky seznamu v neuspořádaný seznam, jak můžete vidět v nástrojích pro vývojáře zobrazí na tomto snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c5228-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Příklad opakovače](angular/_static/repeater.png)

<span data-ttu-id="c5228-191">Tady je příklad, který bude opakovat přes pole objektů.</span><span class="sxs-lookup"><span data-stu-id="c5228-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="c5228-192">`ng-init` Směrnice vytváří `names` pole, kde každý prvek je objekt, který obsahuje první a poslední názvy.</span><span class="sxs-lookup"><span data-stu-id="c5228-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="c5228-193">`ng-repeat` Přiřazení, `name in names`, výstupy položku seznamu pro každý element pole.</span><span class="sxs-lookup"><span data-stu-id="c5228-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="c5228-194">Výstup v tomto případě je stejný jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c5228-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="c5228-195">Úhlová poskytuje některé další direktivy, které můžou poskytnout chování na základě toho, kde smyčky v jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="c5228-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="c5228-196">Použití `$index` v `ng-repeat` smyčka k určení, který index pozice vaší smyčky aktuálně je na.</span><span class="sxs-lookup"><span data-stu-id="c5228-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="c5228-197">`$even`a`$odd`</span><span class="sxs-lookup"><span data-stu-id="c5228-197">`$even` and `$odd`</span></span>

<span data-ttu-id="c5228-198">Použití `$even` v `ng-repeat` opakování ve smyčce bude-li určit, zda je aktuální index ve vaší smyčce i indexované řádek.</span><span class="sxs-lookup"><span data-stu-id="c5228-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="c5228-199">Podobně, použijte `$odd` k určení, pokud je aktuální index liché indexované řádkem.</span><span class="sxs-lookup"><span data-stu-id="c5228-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="c5228-200">`$first`a`$last`</span><span class="sxs-lookup"><span data-stu-id="c5228-200">`$first` and `$last`</span></span>

<span data-ttu-id="c5228-201">Použití `$first` v `ng-repeat` opakování ve smyčce bude-li určit, zda je aktuální index ve vaší smyčce první řádek.</span><span class="sxs-lookup"><span data-stu-id="c5228-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="c5228-202">Podobně, použijte `$last` k určení, zda je aktuální index poslední řádek.</span><span class="sxs-lookup"><span data-stu-id="c5228-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="c5228-203">Zde je ukázka, která zobrazuje `$index`, `$even`, `$odd`, `$first`, a `$last` v akci:</span><span class="sxs-lookup"><span data-stu-id="c5228-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="c5228-204">Zde je výsledný výstup:</span><span class="sxs-lookup"><span data-stu-id="c5228-204">Here is the resulting output:</span></span>

![Příklad opakovače 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="c5228-206">$scope</span><span class="sxs-lookup"><span data-stu-id="c5228-206">$scope</span></span>

<span data-ttu-id="c5228-207">`$scope`je objekt jazyka JavaScript, který funguje jako spojovací mezi zobrazením (šablony) a řadiče (vysvětlení níže).</span><span class="sxs-lookup"><span data-stu-id="c5228-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="c5228-208">Zobrazit šablonu v AngularJS zná pouze hodnoty, které jsou připojené k `$scope` objektu v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="c5228-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="c5228-209">Na světě modelem MVVM `$scope` objekt v AngularJS je často definován jako ViewModel.</span><span class="sxs-lookup"><span data-stu-id="c5228-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="c5228-210">Týmem AngularJS odkazuje `$scope` objektu jako datový Model.</span><span class="sxs-lookup"><span data-stu-id="c5228-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="c5228-211">[Další informace o oborech v AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="c5228-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="c5228-212">Níže je jednoduchý příklad znázorňující postup nastavení vlastnosti na `$scope` v samostatném souboru JavaScript, *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="c5228-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="c5228-213">Sledovat `$scope` parametr předaný řadičem na řádek 2.</span><span class="sxs-lookup"><span data-stu-id="c5228-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="c5228-214">Tento objekt je zobrazení ví o.</span><span class="sxs-lookup"><span data-stu-id="c5228-214">This object is what the view knows about.</span></span> <span data-ttu-id="c5228-215">Na řádku 3 jsme se nastavení vlastnost s názvem "název" na "Marie Jana".</span><span class="sxs-lookup"><span data-stu-id="c5228-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="c5228-216">Co se stane, když konkrétní vlastnost nebyla nalezena v zobrazení?</span><span class="sxs-lookup"><span data-stu-id="c5228-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="c5228-217">Zobrazení definovaná níže odkazuje na vlastnosti "název" a "Stáří":</span><span class="sxs-lookup"><span data-stu-id="c5228-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="c5228-218">Všimněte si na řádku 9 vás žádáme úhlová zobrazíte vlastnost "název" pomocí syntaxe výrazu.</span><span class="sxs-lookup"><span data-stu-id="c5228-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="c5228-219">Řádek 10 se pak odkazuje na "Stáří", vlastnosti, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c5228-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="c5228-220">Spuštěné příklad ukazuje název nastaven "Marie Jana" a nic pro stáří.</span><span class="sxs-lookup"><span data-stu-id="c5228-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="c5228-221">Chybí vlastnosti jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="c5228-221">Missing properties are ignored.</span></span>

![Příklad oboru](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="c5228-223">Moduly</span><span class="sxs-lookup"><span data-stu-id="c5228-223">Modules</span></span>

<span data-ttu-id="c5228-224">A [modulu](https://docs.angularjs.org/guide/module) v AngularJS je kolekce řadiče, služby, direktivy atd. `angular.module()` Volání funkce slouží k vytváření, registraci a načtení modulů v AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="c5228-225">Všechny moduly, včetně těch, které dodaný výrobcem AngularJS tým a knihoven jiných dodavatelů, měly by být zapsány pomocí `angular.module()` funkce.</span><span class="sxs-lookup"><span data-stu-id="c5228-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="c5228-226">Níže je fragment kódu, který ukazuje, jak vytvořit nový modul v AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="c5228-227">První parametr je název modulu.</span><span class="sxs-lookup"><span data-stu-id="c5228-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="c5228-228">Druhý parametr definuje závislosti na dalších modulů.</span><span class="sxs-lookup"><span data-stu-id="c5228-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="c5228-229">Později v tomto článku jsme zobrazující jak předat těchto závislostí nelze `angular.module()` volání metody.</span><span class="sxs-lookup"><span data-stu-id="c5228-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="c5228-230">Použití `ng-app` – direktiva představují modul AngularJS na stránce.</span><span class="sxs-lookup"><span data-stu-id="c5228-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="c5228-231">Chcete použít modul, přiřadit název modulu, `personApp` v tomto příkladu k `ng-app` direktivy v našem šablony.</span><span class="sxs-lookup"><span data-stu-id="c5228-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="c5228-232">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="c5228-232">Controllers</span></span>

<span data-ttu-id="c5228-233">[Řadiče](https://docs.angularjs.org/guide/controller) AngularJS je první bod položky kódu.</span><span class="sxs-lookup"><span data-stu-id="c5228-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="c5228-234">`<module name>.controller()` Volání funkce slouží k vytvoření a registrace řadiče v AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="c5228-235">`ng-controller` – Direktiva se používá k reprezentování řadič AngularJS na stránce HTML.</span><span class="sxs-lookup"><span data-stu-id="c5228-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="c5228-236">Role správce v úhlová je nastavit stav a chování datového modelu (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="c5228-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="c5228-237">Řadiče není vhodné používat k manipulaci s modelu DOM přímo.</span><span class="sxs-lookup"><span data-stu-id="c5228-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="c5228-238">Zde je fragment kódu, který zaregistruje nový řadič.</span><span class="sxs-lookup"><span data-stu-id="c5228-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="c5228-239">`personApp` Proměnné v tomto fragmentu kódu odkazuje úhlová modul, který je definovaný na řádek 2.</span><span class="sxs-lookup"><span data-stu-id="c5228-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="c5228-240">Pomocí zobrazení `ng-controller` – direktiva přiřadí názvu řadiče:</span><span class="sxs-lookup"><span data-stu-id="c5228-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="c5228-241">Stránce se zobrazuje "Marie" a "Jana", která odpovídají `firstName` a `lastName` vlastnosti připojené k `$scope` objektu:</span><span class="sxs-lookup"><span data-stu-id="c5228-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Příklad řadiče](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="c5228-243">Součásti</span><span class="sxs-lookup"><span data-stu-id="c5228-243">Components</span></span>

<span data-ttu-id="c5228-244">[Součásti](https://docs.angularjs.org/guide/component) v úhlová 1.5.x povolit pro zapouzdření a možnosti vytváření jednotlivých elementů HTML.</span><span class="sxs-lookup"><span data-stu-id="c5228-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="c5228-245">V úhlová 1.4.x můžete dosáhnout pomocí metody .directive() stejné funkce.</span><span class="sxs-lookup"><span data-stu-id="c5228-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="c5228-246">Pomocí metody .component() je jednodušší vývoj získání funkce direktiva a kontroleru.</span><span class="sxs-lookup"><span data-stu-id="c5228-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="c5228-247">Mezi další výhody patří; izolace oboru, osvědčené postupy jsou vyplývajících a migrace úhlová 2 bude snazší úloh.</span><span class="sxs-lookup"><span data-stu-id="c5228-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="c5228-248">`<module name>.component()` Volání funkce slouží k vytvoření a registraci komponenty v AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="c5228-249">Zde je fragment kódu, který registruje novou součást.</span><span class="sxs-lookup"><span data-stu-id="c5228-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="c5228-250">`personApp` Proměnné v tomto fragmentu kódu odkazuje úhlová modul, který je definovaný na řádek 2.</span><span class="sxs-lookup"><span data-stu-id="c5228-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="c5228-251">Zobrazení, které jsme se zobrazení vlastního elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="c5228-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="c5228-252">Přidružené šablony používána komponentou:</span><span class="sxs-lookup"><span data-stu-id="c5228-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="c5228-253">Stránce se zobrazuje "Aftab" a "Ansari", která odpovídají `firstName` a `lastName` vlastnosti připojené k `vm` objektu:</span><span class="sxs-lookup"><span data-stu-id="c5228-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Příklad součásti](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="c5228-255">Služby</span><span class="sxs-lookup"><span data-stu-id="c5228-255">Services</span></span>

<span data-ttu-id="c5228-256">[Služby](https://docs.angularjs.org/guide/services) v AngularJS běžně se používají pro sdílené kód, který je rychle abstrahované do souboru, který můžete použít po celou dobu úhlová aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5228-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="c5228-257">Služby líné instance, což znamená, že nebudou existovat instance služby jedině v případě získá používá komponenty, která závisí na službě.</span><span class="sxs-lookup"><span data-stu-id="c5228-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="c5228-258">Objekty Factory jsou například služby, který se používá v aplikacích AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="c5228-259">Objekty Factory se vytvářejí pomocí `myApp.factory()` funkce volání, kde `myApp` je modul.</span><span class="sxs-lookup"><span data-stu-id="c5228-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="c5228-260">Dole je příklad, který ukazuje způsob použití objektů Factory v AngularJS:</span><span class="sxs-lookup"><span data-stu-id="c5228-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="c5228-261">Chcete-li zavolat tento objekt faktory kontroleru, předat `personFactory` jako parametr, který se `controller` funkce:</span><span class="sxs-lookup"><span data-stu-id="c5228-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="c5228-262">Obraťte se na koncový bod REST pomocí služby</span><span class="sxs-lookup"><span data-stu-id="c5228-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="c5228-263">Níže je příklad začátku do konce pomocí služby v AngularJS k interakci s koncovým bodem webové rozhraní API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5228-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="c5228-264">V příkladu získá data z rozhraní Web API a zobrazí data v šabloně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="c5228-265">Začneme zobrazení nejdřív:</span><span class="sxs-lookup"><span data-stu-id="c5228-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="c5228-266">V tomto zobrazení máme úhlová modul s názvem `PersonsApp` řadič s názvem `personController`.</span><span class="sxs-lookup"><span data-stu-id="c5228-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="c5228-267">Používáme `ng-repeat` Iterujte přes seznam osob.</span><span class="sxs-lookup"><span data-stu-id="c5228-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="c5228-268">Jsme odkazují tři vlastní soubory JavaScript na řádky 17-19.</span><span class="sxs-lookup"><span data-stu-id="c5228-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="c5228-269">*PersonApp.js* soubor se používá k registraci `PersonsApp` modulu; a syntaxe je podobná předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="c5228-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="c5228-270">Používáme `angular.module` funkce vytvořit novou instanci třídy modul, který Pracujeme s.</span><span class="sxs-lookup"><span data-stu-id="c5228-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="c5228-271">Podívejme se na *personFactory.js*, níže.</span><span class="sxs-lookup"><span data-stu-id="c5228-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="c5228-272">Voláme na modulu `factory` metoda vytvořte objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="c5228-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="c5228-273">Řádek 12 znázorňuje předdefinované úhlová `$http` služby načítání informací o osoby z webové služby.</span><span class="sxs-lookup"><span data-stu-id="c5228-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="c5228-274">V *personController.js*, voláme modulu `controller` metodu pro vytvoření kontroleru.</span><span class="sxs-lookup"><span data-stu-id="c5228-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="c5228-275">`$scope` Objektu `people` vlastnost je přiřazen s daty vrácenými ze personFactory (řádku 13).</span><span class="sxs-lookup"><span data-stu-id="c5228-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="c5228-276">Podívejme rychlý přehled webového rozhraní API a model za ní.</span><span class="sxs-lookup"><span data-stu-id="c5228-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="c5228-277">`Person` Modelu je POCO (prostý původní objekt CLR) s `Id`, `FirstName`, a `LastName` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c5228-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="c5228-278">`Person` Řadiče vrátí seznam formátu JSON `Person` objekty:</span><span class="sxs-lookup"><span data-stu-id="c5228-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="c5228-279">Podívejme se na aplikaci v akci:</span><span class="sxs-lookup"><span data-stu-id="c5228-279">Let's see the application in action:</span></span>

![Výsledky řadiče zobrazení REST](angular/_static/rest-bound.png)

<span data-ttu-id="c5228-281">Můžete [zobrazení aplikace struktura na Githubu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="c5228-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="c5228-282">Další informace o vytváření struktury aplikace AngularJS, najdete v části [úhlová průvodci správným stylem Jan Papa](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="c5228-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="c5228-283">Pokud chcete vytvořit modul AngularJS, řadič, vytváření, směrnice a zobrazit soubory snadno, ujistěte se, rezervovat Sayed Hashimi [SideWaffle pack šablony pro sadu Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="c5228-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="c5228-284">SideWaffle šablony jsou považovány za standardní zlatý SAYED Hashimi je vedoucí manažer programu na Visual Studio Team Web společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c5228-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="c5228-285">V době psaní tohoto textu je k dispozici pro sadu Visual Studio 2012, 2013 a 2015 SideWaffle.</span><span class="sxs-lookup"><span data-stu-id="c5228-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="c5228-286">Směrování a více zobrazení</span><span class="sxs-lookup"><span data-stu-id="c5228-286">Routing and multiple views</span></span>

<span data-ttu-id="c5228-287">AngularJS má integrovanou trasy zprostředkovatele pro zpracování SPA (jedné stránky aplikace) na základě navigace.</span><span class="sxs-lookup"><span data-stu-id="c5228-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="c5228-288">Chcete-li pracovat s směrování v AngularJS, je nutné přidat `angular-route` knihovny pomocí Bower.</span><span class="sxs-lookup"><span data-stu-id="c5228-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="c5228-289">Se zobrazí [bower.json](#angular-bower-json) soubor odkazuje na začátku tohoto článku, že jsme již na něj odkazují v našem projektu.</span><span class="sxs-lookup"><span data-stu-id="c5228-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="c5228-290">Po instalaci balíčku, přidat odkaz na skript (*úhlová route.js*) do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="c5228-291">Nyní Podívejme aplikaci osoby mít byla vytváření a do ní přidejte navigace.</span><span class="sxs-lookup"><span data-stu-id="c5228-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="c5228-292">Nejprve budeme kopie aplikace tak, že vytvoříte novou `PeopleController` akci s názvem `Spa` a odpovídající `Spa.cshtml` zobrazení tak, že zkopírujete Index.cshtml zobrazení v `People` složky.</span><span class="sxs-lookup"><span data-stu-id="c5228-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="c5228-293">Přidat odkaz na skript `angular-route` (viz řádek 11).</span><span class="sxs-lookup"><span data-stu-id="c5228-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="c5228-294">Také přidat `div` označené jako `ng-view` – direktiva (viz řádek 6) jako zástupný symbol pro umístění zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="c5228-295">Budeme používat několik dalších *.js* soubory, které odkazují na řádky 13-16.</span><span class="sxs-lookup"><span data-stu-id="c5228-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="c5228-296">Podívejme se na *personModule.js* souboru chcete zobrazit, jak jsme se vytváření instancí modul se směrováním.</span><span class="sxs-lookup"><span data-stu-id="c5228-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="c5228-297">Jsme předávání `ngRoute` jako knihovnu do modulu.</span><span class="sxs-lookup"><span data-stu-id="c5228-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="c5228-298">Tento modul zpracuje směrování v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c5228-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="c5228-299">*PersonRoutes.js* souboru níže, definuje trasy založené na zprostředkovateli trasy.</span><span class="sxs-lookup"><span data-stu-id="c5228-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="c5228-300">Řádky 4-7 definují navigační vyslovením efektivně, pokud adresa URL s `/persons` je požadována, použijte šablonu názvem `partials/personlist` projdete `personListController`.</span><span class="sxs-lookup"><span data-stu-id="c5228-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="c5228-301">Na stránce podrobností s parametrem trasy označují čáry 8 11 `personId`.</span><span class="sxs-lookup"><span data-stu-id="c5228-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="c5228-302">Pokud adresu URL neodpovídá jeden vzoru, úhlová použije se výchozí hodnota `/persons` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="c5228-303">`personlist.html` Soubor je částečné zobrazení obsahující pouze HTML, které jsou potřebné k zobrazení seznamu osoby.</span><span class="sxs-lookup"><span data-stu-id="c5228-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="c5228-304">Kontroler se definuje pomocí modulu `controller` fungovat v *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="c5228-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="c5228-305">Pokud jsme spuštění této aplikace a přejděte do `people/spa#/persons` adresu URL, jsme se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="c5228-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Zobrazení seznamu osob](angular/_static/spa-persons.png)

<span data-ttu-id="c5228-307">Pokud jsme přejděte na stránku podrobností, například `people/spa#/persons/2`, vidíme částečné zobrazení podrobností:</span><span class="sxs-lookup"><span data-stu-id="c5228-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Zobrazení podrobností osoba](angular/_static/spa-persons-2.png)

<span data-ttu-id="c5228-309">Můžete zobrazit úplný zdrojový a všechny soubory v tomto článku není uvedeno na [Githubu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="c5228-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="c5228-310">Obslužné rutiny událostí</span><span class="sxs-lookup"><span data-stu-id="c5228-310">Event Handlers</span></span>

<span data-ttu-id="c5228-311">Existuje řada direktiv v AngularJS, který přidat funkce zpracování událostí vstupní elementům ve vašem HTML DOM</span><span class="sxs-lookup"><span data-stu-id="c5228-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="c5228-312">Níže je seznam událostí, které jsou integrovány do AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-312">Below is a list of the events that are built into AngularJS.</span></span>

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
> <span data-ttu-id="c5228-313">Můžete přidat svoje vlastní obslužné rutiny událostí pomocí [vlastní direktivy funkci v AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="c5228-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="c5228-314">Podíváme, jak `ng-click` událostí je drátové nahoru.</span><span class="sxs-lookup"><span data-stu-id="c5228-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="c5228-315">Vytvořte nový soubor JavaScript s názvem *eventHandlerController.js*a přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="c5228-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="c5228-316">Všimněte si nové `sayName` fungovat v `eventHandlerController` na řádku 5 výše.</span><span class="sxs-lookup"><span data-stu-id="c5228-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="c5228-317">Všechny metody provádí pro nyní se zobrazuje upozornění na JavaScript pro uživatele s uvítací zprávy.</span><span class="sxs-lookup"><span data-stu-id="c5228-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="c5228-318">Zobrazení níže váže řadiče k událost AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="c5228-319">Řádek 9 obsahuje tlačítko, na kterém `ng-click` úhlová direktiva byl použit.</span><span class="sxs-lookup"><span data-stu-id="c5228-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="c5228-320">Zavolá naše `sayName` funkce, který je připojen k `$scope` byl předán objekt v tomto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5228-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="c5228-321">Spuštěné příklad ukazuje, který kontroleru `sayName` funkce je automaticky volána při kliknutí na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5228-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Click – událost](angular/_static/events.png)

<span data-ttu-id="c5228-323">Další podrobnosti o AngularJS předdefinované událostí obslužnou rutinu direktivy, nezapomeňte head k [webu dokumentace](https://docs.angularjs.org/api/ng/directive/ngClick) z AngularJS.</span><span class="sxs-lookup"><span data-stu-id="c5228-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c5228-324">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c5228-324">Additional resources</span></span>

* [<span data-ttu-id="c5228-325">Úhlová dokumentace</span><span class="sxs-lookup"><span data-stu-id="c5228-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="c5228-326">Informace o úhlová 2</span><span class="sxs-lookup"><span data-stu-id="c5228-326">Angular 2 Info</span></span>](https://angular.io/)
