---
uid: single-page-application/overview/templates/emberjs-template
title: Šablona EmberJS | Microsoft Docs
author: xqiu
description: EmberJS šablony
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566344"
---
<a name="emberjs-template"></a><span data-ttu-id="c5b08-103">EmberJS šablony</span><span class="sxs-lookup"><span data-stu-id="c5b08-103">EmberJS template</span></span>
====================
<span data-ttu-id="c5b08-104">podle [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="c5b08-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="c5b08-105">Šablony MVC EmberJS se zapíše Nathan Totten, Thiago Santos a Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="c5b08-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="c5b08-106">Stažení šablony EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="c5b08-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="c5b08-107">Šablona EmberJS SPA je navržená tak, aby začít rychle vytvářet interaktivní webové klientské aplikace pomocí EmberJS.</span><span class="sxs-lookup"><span data-stu-id="c5b08-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="c5b08-108">"Jednostránkové aplikace" (SPA) je obecný termín pro webovou aplikaci, která načte jediné stránce HTML a pak aktualizuje dynamicky, místo načítání stránky, nové stránky.</span><span class="sxs-lookup"><span data-stu-id="c5b08-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="c5b08-109">Po načtení úvodní stránky SPA komunikuje se serverem prostřednictvím požadavky AJAX.</span><span class="sxs-lookup"><span data-stu-id="c5b08-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="c5b08-110">AJAX není nic nového, ale existují ještě dnes JavaScript rozhraní, které usnadňují tvorbu a udržování rozsáhlé sofistikované SPA aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5b08-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="c5b08-111">Navíc standardu HTML 5 a CSS3 jsou usnadnit vytvoření bohaté uživatelská rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5b08-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="c5b08-112">Šablona SPA EmberJS používá [členskými](http://emberjs.com/) knihovna JavaScript pro zpracování aktualizace stránky z požadavky AJAX.</span><span class="sxs-lookup"><span data-stu-id="c5b08-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="c5b08-113">Ember.js používá datové vazby k synchronizaci stránky s nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="c5b08-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="c5b08-114">Tímto způsobem, nemusíte psát kód, který provede JSON data a aktualizuje modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="c5b08-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="c5b08-115">Místo toho uveďte deklarativní atributy ve formátu HTML, který informování Ember.js jak data k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c5b08-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="c5b08-116">Na straně serveru, je téměř shodné s šabloně EmberJS [kódem KnockoutJS SPA šablony](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="c5b08-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="c5b08-117">ASP.NET MVC používá k obsluze dokumentů HTML a ASP.NET Web API pro zpracování žádostí AJAX z klienta.</span><span class="sxs-lookup"><span data-stu-id="c5b08-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="c5b08-118">Další informace o těchto aspektů šablony najdete v části [kódem KnockoutJS šablony](../introduction/knockoutjs-template.md) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c5b08-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="c5b08-119">Toto téma se zaměřuje na rozdíly mezi šablony Knockout a EmberJS šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="c5b08-120">Vytvoření projektu EmberJS SPA šablony</span><span class="sxs-lookup"><span data-stu-id="c5b08-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="c5b08-121">Stáhněte a nainstalujte šablony kliknutím na výše uvedené tlačítko Stáhnout.</span><span class="sxs-lookup"><span data-stu-id="c5b08-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="c5b08-122">Možná budete muset restartovat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5b08-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="c5b08-123">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c5b08-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c5b08-124">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="c5b08-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c5b08-125">V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="c5b08-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c5b08-126">Název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5b08-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="c5b08-127">V **nový projekt** průvodci vyberte **Ember.js SPA projektu**.</span><span class="sxs-lookup"><span data-stu-id="c5b08-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="c5b08-128">Přehled šablon SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="c5b08-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="c5b08-129">Šablona EmberJS používá kombinaci jQuery, Ember.js, Handlebars.js vytvořit smooth, interaktivní uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5b08-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="c5b08-130">Ember.js je knihovna JavaScript, která používá vzor MVC na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c5b08-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="c5b08-131">A *šablony*, napsané v jazyce ukázka Řídítka kola, popisuje uživatelské rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5b08-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="c5b08-132">V režimu vydání [Řídítka kola kompilátoru](https://github.com/Myslik/csharp-ember-handlebars) se používá k sady a kompilaci Řídítka kola šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="c5b08-133">A *modelu* ukládá data aplikací, který získá ze serveru (ToDo seznamy a položky ToDo).</span><span class="sxs-lookup"><span data-stu-id="c5b08-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="c5b08-134">A *řadič* uloží stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5b08-134">A *controller* stores application state.</span></span> <span data-ttu-id="c5b08-135">Řadiče často prezentují data modelu odpovídající šablon.</span><span class="sxs-lookup"><span data-stu-id="c5b08-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="c5b08-136">A *zobrazení* překládá primitivní události z aplikace a předá tyto řadiče.</span><span class="sxs-lookup"><span data-stu-id="c5b08-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="c5b08-137">A *směrovač* spravuje stav aplikace, udržování synchronizace adresy URL a šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="c5b08-138">Kromě toho knihovny členskými dat slouží k synchronizaci objektů JSON (získaný ze serveru prostřednictvím rozhraní RESTful API) a modely klienta.</span><span class="sxs-lookup"><span data-stu-id="c5b08-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="c5b08-139">Šablona EmberJS SPA uspořádává skripty do osm vrstvy:</span><span class="sxs-lookup"><span data-stu-id="c5b08-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="c5b08-140">webapi\_adapter.js, webapi\_serializer.js: rozšíření knihovny členskými dat pro práci s rozhraním ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="c5b08-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="c5b08-141">Scripts/Helpers.js: Definuje nový Řídítka členskými kola pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="c5b08-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="c5b08-142">Scripts/App.js: Vytvoří aplikaci a nakonfiguruje adaptéru a serializátor.</span><span class="sxs-lookup"><span data-stu-id="c5b08-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="c5b08-143">Aplikace, skripty nebomodely/\*.js: definuje modely.</span><span class="sxs-lookup"><span data-stu-id="c5b08-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="c5b08-144">Skripty nebo aplikace nebozobrazení/\*.js: definuje zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5b08-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="c5b08-145">Skripty/aplikace/řadiče nebo\*.js: definuje do řadičů.</span><span class="sxs-lookup"><span data-stu-id="c5b08-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="c5b08-146">Skripty a aplikace/směruje, Scripts/app/router.js: Definuje trasy.</span><span class="sxs-lookup"><span data-stu-id="c5b08-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="c5b08-147">Šablony /\*.hbs: definuje Řídítka kola šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="c5b08-148">Podívejme se na některé z těchto skriptů podrobněji.</span><span class="sxs-lookup"><span data-stu-id="c5b08-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="c5b08-149">Modely</span><span class="sxs-lookup"><span data-stu-id="c5b08-149">Models</span></span>

<span data-ttu-id="c5b08-150">Modely jsou definovány ve složce aplikace, skripty nebo modely.</span><span class="sxs-lookup"><span data-stu-id="c5b08-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="c5b08-151">Existují dva soubory modelu: todoItem.js a todoList.js.</span><span class="sxs-lookup"><span data-stu-id="c5b08-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="c5b08-152">**TODO.model.js** definuje modely na straně klienta (prohlížeč) pro seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="c5b08-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="c5b08-153">Existují dvě třídy modelu: todoItem a seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="c5b08-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="c5b08-154">V členskými jsou modely měly podtřídy DS. Model.</span><span class="sxs-lookup"><span data-stu-id="c5b08-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="c5b08-155">Model může mít vlastnosti s atributy:</span><span class="sxs-lookup"><span data-stu-id="c5b08-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="c5b08-156">Modely můžete definovat vztahy s jinými modely:</span><span class="sxs-lookup"><span data-stu-id="c5b08-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="c5b08-157">Modely můžete počítaný vlastnosti, které vytvořit vazbu na ostatní vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c5b08-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="c5b08-158">Modely může mít pozorovatel funkce, které jsou vyvolány, když se změní zjištěnou vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c5b08-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="c5b08-159">zobrazení</span><span class="sxs-lookup"><span data-stu-id="c5b08-159">Views</span></span>

<span data-ttu-id="c5b08-160">Zobrazení jsou definovány ve složce skripty nebo aplikace nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c5b08-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="c5b08-161">Zobrazení překládá události z aplikace uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5b08-161">A view translates events from the application UI.</span></span> <span data-ttu-id="c5b08-162">Obslužné rutiny události můžete zpětné volání pro funkce řadiče nebo jednoduše kontextu dat volat přímo.</span><span class="sxs-lookup"><span data-stu-id="c5b08-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="c5b08-163">Například následující kód je z views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="c5b08-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="c5b08-164">Definuje, událostí zpracování pro vstupní textové pole.</span><span class="sxs-lookup"><span data-stu-id="c5b08-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="c5b08-165">Řadiče</span><span class="sxs-lookup"><span data-stu-id="c5b08-165">Controller</span></span>

<span data-ttu-id="c5b08-166">Ve složce skripty/aplikace/řadiče jsou definovány v řadičích.</span><span class="sxs-lookup"><span data-stu-id="c5b08-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="c5b08-167">Představují jeden model, rozšířit `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="c5b08-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="c5b08-168">Řadič může také představovat kolekce modelů rozšířením `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="c5b08-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="c5b08-169">Například TodoListController představuje pole `todoList` objekty.</span><span class="sxs-lookup"><span data-stu-id="c5b08-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="c5b08-170">Řadičem seřadí podle ID seznamu úkolů v sestupném pořadí:</span><span class="sxs-lookup"><span data-stu-id="c5b08-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="c5b08-171">Řadičem definuje funkci s názvem `addTodoList`, který vytvoří nový seznam úkolů a přidá ji do pole.</span><span class="sxs-lookup"><span data-stu-id="c5b08-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="c5b08-172">Informace o tom, jak tuto funkci volala, otevřete soubor šablony s názvem todoListTemplate.html ve složce šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="c5b08-173">Následující kód šablony váže tlačítko `addTodoList` funkce:</span><span class="sxs-lookup"><span data-stu-id="c5b08-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="c5b08-174">Také obsahuje kontroler `error` vlastnosti, která obsahuje chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="c5b08-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="c5b08-175">Tady je kód šablony zobrazíte chybovou zprávu (také ve todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="c5b08-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="c5b08-176">Trasy</span><span class="sxs-lookup"><span data-stu-id="c5b08-176">Routes</span></span>

<span data-ttu-id="c5b08-177">Router.js definuje trasy a výchozí šablony, které chcete zobrazit, nastaví stav aplikací a odpovídá adresy URL trasy:</span><span class="sxs-lookup"><span data-stu-id="c5b08-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="c5b08-178">TodoListRoute.js načte data pro TodoListRoute přepsáním setupController funkce:</span><span class="sxs-lookup"><span data-stu-id="c5b08-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="c5b08-179">Členskými používá zásady vytváření názvů k přiřazení adres URL, názvy tras, řadiče a šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="c5b08-180">Další informace najdete v tématu [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) v dokumentaci EmberJS.</span><span class="sxs-lookup"><span data-stu-id="c5b08-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="c5b08-181">Šablony</span><span class="sxs-lookup"><span data-stu-id="c5b08-181">Templates</span></span>

<span data-ttu-id="c5b08-182">Složka šablon obsahuje čtyři šablony:</span><span class="sxs-lookup"><span data-stu-id="c5b08-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="c5b08-183">Application.HBs: výchozí šablonu, která se zobrazí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5b08-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="c5b08-184">About.HBs: Šablona pro tuto trasu "/ o".</span><span class="sxs-lookup"><span data-stu-id="c5b08-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="c5b08-185">index.HBs: šablony pro kořenovou "/" trasy.</span><span class="sxs-lookup"><span data-stu-id="c5b08-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="c5b08-186">todoList.hbs: šablonu pro "/ todo" trasy.</span><span class="sxs-lookup"><span data-stu-id="c5b08-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="c5b08-187">\_navbar.HBs: Šablona definuje navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="c5b08-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="c5b08-188">Šablona aplikací funguje jako hlavní stránky.</span><span class="sxs-lookup"><span data-stu-id="c5b08-188">The application template acts like a master page.</span></span> <span data-ttu-id="c5b08-189">Obsahuje záhlaví, zápatí a "{{výstupu}}" Chcete-li vložit další šablony v v závislosti na trasy.</span><span class="sxs-lookup"><span data-stu-id="c5b08-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="c5b08-190">Další informace o šablonách aplikace v členskými najdete v tématu [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="c5b08-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="c5b08-191">"/ TodoList" Šablona obsahuje dvou výrazů smyčky.</span><span class="sxs-lookup"><span data-stu-id="c5b08-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="c5b08-192">Mimo smyčka je `{{#each controller}}`a uvnitř smyčka je `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="c5b08-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="c5b08-193">Následující kód ukazuje integrované `Ember.Checkbox` zobrazit, přizpůsobený `App.TodoItemEditView`a odkaz s `deleteTodo` akce.</span><span class="sxs-lookup"><span data-stu-id="c5b08-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="c5b08-194">`HtmlHelperExtensions` Třídy, které jsou definované v Controllers/HtmlHelperExensions.cs, definuje pomocné rutiny funkce ukládat do mezipaměti a vložte šablonu souborů při **ladění** je nastaven na **true** v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="c5b08-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="c5b08-195">Tato funkce je volána ze souboru zobrazení ASP.NET MVC definované v Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="c5b08-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="c5b08-196">Voláno bez argumentů, vykreslí funkce všechny šablony soubory ve složce šablony.</span><span class="sxs-lookup"><span data-stu-id="c5b08-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="c5b08-197">Můžete také zadat určitá šablona souboru nebo podsložku.</span><span class="sxs-lookup"><span data-stu-id="c5b08-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="c5b08-198">Když **ladění** je **false** v souboru Web.config, aplikace obsahuje položku sady "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="c5b08-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="c5b08-199">Tuto položku sady je přidaný do BundleConfig.cs pomocí knihovny kompilátoru Řídítka kola:</span><span class="sxs-lookup"><span data-stu-id="c5b08-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
