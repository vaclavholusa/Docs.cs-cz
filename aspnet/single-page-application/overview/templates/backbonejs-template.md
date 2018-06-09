---
uid: single-page-application/overview/templates/backbonejs-template
title: Páteřní šablony | Microsoft Docs
author: madskristensen
description: Backbone.js SPA šablony
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566308"
---
<a name="backbone-template"></a><span data-ttu-id="e3974-103">Páteřní šablony</span><span class="sxs-lookup"><span data-stu-id="e3974-103">Backbone Template</span></span>
====================
<span data-ttu-id="e3974-104">podle [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="e3974-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="e3974-105">Šablona SPA páteřní napsal Kazi Manzur Rashid</span><span class="sxs-lookup"><span data-stu-id="e3974-105">The Backbone SPA Template was written by Kazi Manzur Rashid</span></span>
> 
> [<span data-ttu-id="e3974-106">Stažení šablony SPA Backbone.js</span><span class="sxs-lookup"><span data-stu-id="e3974-106">Download the Backbone.js SPA Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=293631)


<span data-ttu-id="e3974-107">Šabloně Backbone.js SPA navržená tak, abyste mohli začít rychle vytvářet interaktivní webové klientské aplikace pomocí [Backbone.js.](http://backbonejs.org/)</span><span class="sxs-lookup"><span data-stu-id="e3974-107">The Backbone.js SPA template is designed to get you started quickly building interactive client-side web apps using [Backbone.js.](http://backbonejs.org/)</span></span>

<span data-ttu-id="e3974-108">Šablona poskytuje počáteční kostru pro vývoj aplikace Backbone.js v architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3974-108">The template provides an initial skeleton for developing a Backbone.js application in ASP.NET MVC.</span></span> <span data-ttu-id="e3974-109">Ihned poskytuje funkce základní uživatel přihlášení, včetně resetování hesla registrace, přihlášení, uživatele a potvrzení uživatele s základní e-mailových šablon.</span><span class="sxs-lookup"><span data-stu-id="e3974-109">Out of the box it provides basic user login functionality, including user sign-up, sign-in, password reset, and user confirmation with basic email templates.</span></span>

<span data-ttu-id="e3974-110">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="e3974-110">Requirements:</span></span>

- [<span data-ttu-id="e3974-111">Technologie ASP.NET a webové nástroje 2012.2 aktualizace</span><span class="sxs-lookup"><span data-stu-id="e3974-111">ASP.NET and Web Tools 2012.2 update</span></span>](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a><span data-ttu-id="e3974-112">Vytvoření projektu šablony páteřní</span><span class="sxs-lookup"><span data-stu-id="e3974-112">Create a Backbone Template Project</span></span>

<span data-ttu-id="e3974-113">Stáhněte a nainstalujte šablony kliknutím na výše uvedené tlačítko Stáhnout.</span><span class="sxs-lookup"><span data-stu-id="e3974-113">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="e3974-114">Šablona je zabalené jako soubor rozšíření Visual Studio (VSIX).</span><span class="sxs-lookup"><span data-stu-id="e3974-114">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="e3974-115">Možná budete muset restartovat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3974-115">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="e3974-116">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e3974-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e3974-117">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="e3974-117">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e3974-118">V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e3974-118">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e3974-119">Název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3974-119">Name the project and click **OK**.</span></span>

![](backbonejs-template/_static/image1.png)

<span data-ttu-id="e3974-120">V **nový projekt** průvodce, vyberte Backbone.js SPA projektu.</span><span class="sxs-lookup"><span data-stu-id="e3974-120">In the **New Project** wizard, select Backbone.js SPA Project.</span></span>

![](backbonejs-template/_static/image2.png)

<span data-ttu-id="e3974-121">Stisknutím Ctrl + F5 sestavení a spuštění aplikace bez ladění nebo stisknutím klávesy F5 spusťte s laděním.</span><span class="sxs-lookup"><span data-stu-id="e3974-121">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](backbonejs-template/_static/image3.png)

<span data-ttu-id="e3974-122">Kliknutím na tlačítko "Můj účet" zobrazíte stránku pro přihlášení:</span><span class="sxs-lookup"><span data-stu-id="e3974-122">Clicking "My Account" brings up the login page:</span></span>

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a><span data-ttu-id="e3974-123">Návod: Kód klienta</span><span class="sxs-lookup"><span data-stu-id="e3974-123">Walkthrough: Client Code</span></span>

<span data-ttu-id="e3974-124">Pojďme začíná na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e3974-124">Let's starts with the client side.</span></span> <span data-ttu-id="e3974-125">Skripty aplikace klienta jsou umístěny ve složce ~/Scripts/application.</span><span class="sxs-lookup"><span data-stu-id="e3974-125">The client application scripts are located in the ~/Scripts/application folder.</span></span> <span data-ttu-id="e3974-126">Aplikace je napsána v [TypeScript](http://www.typescriptlang.org/) (.ts soubory) které jsou zkompilovány do jazyka JavaScript (.js soubory).</span><span class="sxs-lookup"><span data-stu-id="e3974-126">The application is written in [TypeScript](http://www.typescriptlang.org/) (.ts files) which are compiled into JavaScript (.js files).</span></span>

<span data-ttu-id="e3974-127">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="e3974-127">**Application**</span></span>

<span data-ttu-id="e3974-128">`Application` je definována v application.ts.</span><span class="sxs-lookup"><span data-stu-id="e3974-128">`Application` is defined in application.ts.</span></span> <span data-ttu-id="e3974-129">Tento objekt inicializuje aplikaci a funguje jako kořenového oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="e3974-129">This object initializes the application and acts as the root namespace.</span></span> <span data-ttu-id="e3974-130">Uchovává informace o konfiguraci a stavu, který je sdílen mezi aplikace, například zda je přihlášený uživatel.</span><span class="sxs-lookup"><span data-stu-id="e3974-130">It maintains configuration and state information that is shared across the application, such as whether the user is signed in.</span></span>

<span data-ttu-id="e3974-131">`application.start` Metoda vytvoří modální zobrazení a připojí obslužné rutiny události pro události na úrovni aplikace, jako je například přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3974-131">The `application.start` method creates the modal views and attaches event handlers for application-level events, such as user sign-in.</span></span> <span data-ttu-id="e3974-132">V dalším kroku vytvoří výchozí směrovač a kontroluje, zda je zadaná adresa URL žádné straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e3974-132">Next, it creates the default router and checks whether any client-side URL is specified.</span></span> <span data-ttu-id="e3974-133">Pokud ne, je přesměrován na výchozí adresa url (#! /).</span><span class="sxs-lookup"><span data-stu-id="e3974-133">If not, it redirects to the default url (#!/).</span></span>

<span data-ttu-id="e3974-134">**Události**</span><span class="sxs-lookup"><span data-stu-id="e3974-134">**Events**</span></span>

<span data-ttu-id="e3974-135">Události jsou důležité vždy při vývoji volně doplněná součásti.</span><span class="sxs-lookup"><span data-stu-id="e3974-135">Events are always important when developing loosely coupled components.</span></span> <span data-ttu-id="e3974-136">Aplikace často provádět více operací v reakci na akci uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3974-136">Applications often perform multiple operations in response to a user action.</span></span> <span data-ttu-id="e3974-137">Páteřní poskytuje integrované události s komponenty, například modelu, kolekce a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3974-137">Backbone provides built-in events with components such as Model, Collection, and View.</span></span> <span data-ttu-id="e3974-138">Místo vytváření mezi závislosti mezi tyto součásti, šablona používá model "pub nebo sub": `events` objektu, která je definována v events.ts, funguje jako centra událostí pro publikování a přihlášení k odběru událostí aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3974-138">Instead of creating inter-dependencies among these components, the template uses a "pub/sub" model: The `events` object, defined in events.ts, acts as an event hub for publishing and subscribing to application events.</span></span> <span data-ttu-id="e3974-139">`events` Objekt je typu singleton.</span><span class="sxs-lookup"><span data-stu-id="e3974-139">The `events` object is a singleton.</span></span> <span data-ttu-id="e3974-140">Následující kód ukazuje, jak chcete objednat předplatné událost a potom aktivovat události:</span><span class="sxs-lookup"><span data-stu-id="e3974-140">The following code shows how to subscribe to an event and then trigger the event:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

<span data-ttu-id="e3974-141">**Směrovač**</span><span class="sxs-lookup"><span data-stu-id="e3974-141">**Router**</span></span>

<span data-ttu-id="e3974-142">Směrovač v Backbone.js, poskytuje metody pro směrování stránky na straně klienta a jejich připojením do akce a události.</span><span class="sxs-lookup"><span data-stu-id="e3974-142">In Backbone.js, a router provides methods for routing client-side pages and connecting them to actions and events.</span></span> <span data-ttu-id="e3974-143">Šablona definuje jeden směrovač, router.ts.</span><span class="sxs-lookup"><span data-stu-id="e3974-143">The template defines a single router, in router.ts.</span></span> <span data-ttu-id="e3974-144">Směrovači activable zobrazení vytvoří a udržuje stav při přepnutí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3974-144">The router creates the activable views and maintains the state when switching views.</span></span> <span data-ttu-id="e3974-145">(Activable zobrazení jsou popsané v další části). Na začátku projekt má dvě fiktivní zobrazení, domácích a o.</span><span class="sxs-lookup"><span data-stu-id="e3974-145">(Activable views are described in the next section.) Initially, the project has two dummy views, Home and About.</span></span> <span data-ttu-id="e3974-146">Je také zobrazení serveru, který se zobrazí, pokud není znám trasy.</span><span class="sxs-lookup"><span data-stu-id="e3974-146">It also has a NotFound view, which is displayed if the route is not known.</span></span>

<span data-ttu-id="e3974-147">**Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="e3974-147">**Views**</span></span>

<span data-ttu-id="e3974-148">Zobrazení jsou definovány v ~/Scripts/application nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3974-148">The views are defined in ~/Scripts/application/views.</span></span> <span data-ttu-id="e3974-149">Existují dva typy zobrazení, activable zobrazení a zobrazení modální dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3974-149">There are two kinds of views, activable views and modal dialog views.</span></span> <span data-ttu-id="e3974-150">Activable zobrazení jsou vyvolány směrovači.</span><span class="sxs-lookup"><span data-stu-id="e3974-150">Activable views are invoked by the router.</span></span> <span data-ttu-id="e3974-151">Když se zobrazuje i activable zobrazení, všechna activable zobrazení neaktivní.</span><span class="sxs-lookup"><span data-stu-id="e3974-151">When an activable view is shown, all other activable views become inactive.</span></span> <span data-ttu-id="e3974-152">Chcete-li vytvořit activable zobrazení, rozšířit zobrazení s `Activable` objektu:</span><span class="sxs-lookup"><span data-stu-id="e3974-152">To create an activable view, extend the view with the `Activable` object:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

<span data-ttu-id="e3974-153">Rozšíření s `Activable` přidá dva nové metody pro zobrazení, `activate` a `deactivate`.</span><span class="sxs-lookup"><span data-stu-id="e3974-153">Extending with `Activable` adds two new methods to the view, `activate` and `deactivate`.</span></span> <span data-ttu-id="e3974-154">Směrovači volá tyto metody pro aktivaci a deactive zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3974-154">The router calls these methods to activate and deactive the view.</span></span>

<span data-ttu-id="e3974-155">Modální zobrazení jsou implementované jako [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modální dialogová okna.</span><span class="sxs-lookup"><span data-stu-id="e3974-155">Modal views are implemented as [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modal dialogs.</span></span> <span data-ttu-id="e3974-156">`Membership` a `Profile` zobrazení jsou modální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3974-156">The `Membership` and `Profile` views are modal views.</span></span> <span data-ttu-id="e3974-157">Model zobrazení může vyvolat všech událostí aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3974-157">Model views can be invoked by any application events.</span></span> <span data-ttu-id="e3974-158">Například v `Navigation` zobrazení, kliknutím na odkaz "Můj účet" uvádí buď `Membership` zobrazení nebo `Profile` zobrazení, v závislosti na tom, jestli je uživatel přihlášen.</span><span class="sxs-lookup"><span data-stu-id="e3974-158">For example, in the `Navigation` view, clicking the "My Account" link shows either the `Membership` view or the `Profile` view, depending on whether the user is logged in.</span></span> <span data-ttu-id="e3974-159">`Navigation` Připojí obslužné rutiny události pro všechny podřízené elementy, které mají kliknutí `data-command` atribut.</span><span class="sxs-lookup"><span data-stu-id="e3974-159">The `Navigation` attaches click event handlers to any child elements that have the `data-command` attribute.</span></span> <span data-ttu-id="e3974-160">Zde je kód HTML:</span><span class="sxs-lookup"><span data-stu-id="e3974-160">Here is the HTML markup:</span></span>

[!code-html[Main](backbonejs-template/samples/sample3.html)]

<span data-ttu-id="e3974-161">Zde je kód v navigation.ts spojit události:</span><span class="sxs-lookup"><span data-stu-id="e3974-161">Here is the code in navigation.ts to hook up the events:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

<span data-ttu-id="e3974-162">**Modely**</span><span class="sxs-lookup"><span data-stu-id="e3974-162">**Models**</span></span>

<span data-ttu-id="e3974-163">Modely jsou definovány v ~/Scripts/application/modelů.</span><span class="sxs-lookup"><span data-stu-id="e3974-163">The models are defined in ~/Scripts/application/models.</span></span> <span data-ttu-id="e3974-164">Všechny modely mají tři základní věci: výchozí atributy, ověřovacích pravidel a koncový bod na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="e3974-164">The models all have three basic things: default attributes, validation rules, and a server-side end point.</span></span> <span data-ttu-id="e3974-165">Tady je typický příklad:</span><span class="sxs-lookup"><span data-stu-id="e3974-165">Here is a typical example:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

<span data-ttu-id="e3974-166">**Moduly plug-in**</span><span class="sxs-lookup"><span data-stu-id="e3974-166">**Plug-ins**</span></span>

<span data-ttu-id="e3974-167">Složka ~/Scripts/application/lib obsahuje několik užitečný jQuery moduly plug-in. Soubor form.ts definuje modulu plug-in pro práci s dat formuláře.</span><span class="sxs-lookup"><span data-stu-id="e3974-167">The ~/Scripts/application/lib folder contains a few handy jQuery plug-ins. The form.ts file defines a plug-in for working with form data.</span></span> <span data-ttu-id="e3974-168">Často potřebují k serializaci nebo deserializaci dat formuláře a zobrazit všechny chyby ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="e3974-168">Often you need to serialize or deserialize form data and show any model validation errors.</span></span> <span data-ttu-id="e3974-169">Modul plug-in form.ts má metody jako `serializeFields`, `deserializeFields`, a `showFieldErrors`.</span><span class="sxs-lookup"><span data-stu-id="e3974-169">The form.ts plug-in has methods such as `serializeFields`, `deserializeFields`, and `showFieldErrors`.</span></span> <span data-ttu-id="e3974-170">Následující příklad ukazuje, jak k serializaci formuláře k modelu.</span><span class="sxs-lookup"><span data-stu-id="e3974-170">The following example shows how to serialize a form to a model.</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

<span data-ttu-id="e3974-171">Modul plug-in flashbar.ts poskytuje různé druhy zprávy zpětnou vazbu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3974-171">The flashbar.ts plug-in gives various kinds of feedback messages to the user.</span></span> <span data-ttu-id="e3974-172">Metody `$.showSuccessbar`, `$.showErrorbar` a `$.showInfobar`.</span><span class="sxs-lookup"><span data-stu-id="e3974-172">The methods are `$.showSuccessbar`, `$.showErrorbar` and `$.showInfobar`.</span></span> <span data-ttu-id="e3974-173">Na pozadí používá Twitter Bootstrap výstrahy zobrazíte vhodně animovaný zprávy.</span><span class="sxs-lookup"><span data-stu-id="e3974-173">Behind the scenes, it uses Twitter Bootstrap alerts to show nicely animated messages.</span></span>

<span data-ttu-id="e3974-174">Modul plug-in confirm.ts nahrazuje prohlížeče potvrďte dialog, i když se poněkud liší rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="e3974-174">The confirm.ts plug-in replaces the browser's confirm dialog, although the API is somewhat different:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a><span data-ttu-id="e3974-175">Návod: Serverový kód</span><span class="sxs-lookup"><span data-stu-id="e3974-175">Walkthrough: Server Code</span></span>

<span data-ttu-id="e3974-176">Nyní Podíváme se na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="e3974-176">Now let's look at the server side.</span></span>

<span data-ttu-id="e3974-177">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="e3974-177">**Controllers**</span></span>

<span data-ttu-id="e3974-178">V aplikaci jednostránkové hraje serveru jenom malé roli v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e3974-178">In a single page application, the server plays only a small role in the user interface.</span></span> <span data-ttu-id="e3974-179">Obvykle serveru vykreslí úvodní stránku a pak odesílá a přijímá JSON data.</span><span class="sxs-lookup"><span data-stu-id="e3974-179">Typically, the server renders the initial page and then sends and receives JSON data.</span></span>

<span data-ttu-id="e3974-180">Šablona má dva řadiče MVC: `HomeController` vykreslí úvodní stránky a `SupportsController` se používá k potvrzení nové uživatelské účty a resetovat hesla.</span><span class="sxs-lookup"><span data-stu-id="e3974-180">The template has two MVC controllers: `HomeController` renders the initial page, and `SupportsController` is used to confirm new user accounts and reset passwords.</span></span> <span data-ttu-id="e3974-181">Všechny řadiče v šabloně jsou řadiče rozhraní ASP.NET Web API, které odesílat a přijímat JSON data.</span><span class="sxs-lookup"><span data-stu-id="e3974-181">All other controllers in the template are ASP.NET Web API controllers, which send and receive JSON data.</span></span> <span data-ttu-id="e3974-182">Ve výchozím nastavení, aby řadiče pomocí nové `WebSecurity` třída úkoly související s uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e3974-182">By default, the controllers use the new `WebSecurity` class to perform user-related tasks.</span></span> <span data-ttu-id="e3974-183">Ale mají také volitelné konstruktory, které umožňují předat delegáty pro tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="e3974-183">However, they also have optional constructors that let you pass in delegates for these tasks.</span></span> <span data-ttu-id="e3974-184">To je testování jednodušší a umožňuje vám nahradit `WebSecurity` s něco jiného, pomocí kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="e3974-184">This makes testing easier, and lets you replace `WebSecurity` with something else, by using an IoC Container.</span></span> <span data-ttu-id="e3974-185">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="e3974-185">Here is an example:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a><span data-ttu-id="e3974-186">zobrazení</span><span class="sxs-lookup"><span data-stu-id="e3974-186">Views</span></span>

<span data-ttu-id="e3974-187">Zobrazení jsou navržené jako modulární: každý oddíl stránky, má svou vlastní vyhrazené zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3974-187">The views are designed to be modular: Each section of a page has its own dedicated view.</span></span> <span data-ttu-id="e3974-188">V jednostránkové aplikace je běžné zahrnout zobrazení, které nemají žádné odpovídající kontroler.</span><span class="sxs-lookup"><span data-stu-id="e3974-188">In a single page application, it is common to include views that do not have any corresponding controller.</span></span> <span data-ttu-id="e3974-189">Zobrazení může zahrnovat voláním `@Html.Partial('myView')`, ale to získá zdlouhavé.</span><span class="sxs-lookup"><span data-stu-id="e3974-189">You can include a view by calling `@Html.Partial('myView')`, but this gets tedious.</span></span> <span data-ttu-id="e3974-190">Pro zjednodušení, že šablona definuje Pomocná metoda, `IncludeClientViews`, který vykreslí všechna zobrazení v zadané složce:</span><span class="sxs-lookup"><span data-stu-id="e3974-190">To make this easier, the template defines a helper method, `IncludeClientViews`, that renders all of the views in a specified folder:</span></span>

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

<span data-ttu-id="e3974-191">Pokud není zadán název složky, je výchozí název složky "ClientViews".</span><span class="sxs-lookup"><span data-stu-id="e3974-191">If the folder name is not specified, the default folder name is "ClientViews".</span></span> <span data-ttu-id="e3974-192">Pokud klient zobrazení také používá částečné zobrazení, název částečného zobrazení znakem podtržítka (například `_SignUp`).</span><span class="sxs-lookup"><span data-stu-id="e3974-192">If your client view also uses partial views, name the partial view with an underscore character (for example, `_SignUp`).</span></span> <span data-ttu-id="e3974-193">`IncludeClientViews` Metoda vylučuje všechna zobrazení, jejichž název začíná podtržítkem.</span><span class="sxs-lookup"><span data-stu-id="e3974-193">The `IncludeClientViews` method excludes any views whose name starts with an underscore.</span></span> <span data-ttu-id="e3974-194">Chcete-li zahrnout do zobrazení klienta částečné zobrazení, volejte `Html.ClientView('SignUp')` místo `Html.Partial('_SignUp')`.</span><span class="sxs-lookup"><span data-stu-id="e3974-194">To include a partial view in the client view, call `Html.ClientView('SignUp')` instead of `Html.Partial('_SignUp')`.</span></span>

<span data-ttu-id="e3974-195">**Odesílání e-mailu**</span><span class="sxs-lookup"><span data-stu-id="e3974-195">**Sending Email**</span></span>

<span data-ttu-id="e3974-196">K odeslání e-mailu, šablona používá [poštovní](http://aboutcode.net/postal).</span><span class="sxs-lookup"><span data-stu-id="e3974-196">To send email, the template uses [Postal](http://aboutcode.net/postal).</span></span> <span data-ttu-id="e3974-197">Ale je poštovní abstrahované od zbytku kódu pomocí `IMailer` rozhraní, takže ho můžete snadno nahradit s jinou implementaci.</span><span class="sxs-lookup"><span data-stu-id="e3974-197">However, Postal is abstracted from the rest of the code with the `IMailer` interface, so you can easily replace it with another implementation.</span></span> <span data-ttu-id="e3974-198">E-mailových šablon jsou umístěny ve složce zobrazení či E-maily.</span><span class="sxs-lookup"><span data-stu-id="e3974-198">The email templates are located in the Views/Emails folder.</span></span> <span data-ttu-id="e3974-199">E-mailová adresa odesílatele je zadaný v souboru web.config v `sender.email` klíče z **appSettings** části.</span><span class="sxs-lookup"><span data-stu-id="e3974-199">The sender's email address is specified in the web.config file, in the `sender.email` key of the **appSettings** section.</span></span> <span data-ttu-id="e3974-200">I když `debug="true"` aplikace v souboru web.config, nevyžaduje potvrzení e-mailu uživatele, pro urychlení vývoj.</span><span class="sxs-lookup"><span data-stu-id="e3974-200">Also, when `debug="true"` in web.config, the application does not require user email confirmation, to speed up development.</span></span>

## <a name="github"></a><span data-ttu-id="e3974-201">GitHub</span><span class="sxs-lookup"><span data-stu-id="e3974-201">GitHub</span></span>

<span data-ttu-id="e3974-202">Můžete také získat šabloně Backbone.js SPA na [Githubu](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span><span class="sxs-lookup"><span data-stu-id="e3974-202">You can also find the Backbone.js SPA template on [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span></span>
