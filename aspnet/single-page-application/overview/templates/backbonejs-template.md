---
uid: single-page-application/overview/templates/backbonejs-template
title: Šablona Backbone | Dokumentace Microsoftu
author: madskristensen
description: Šablona Backbone.js SPA
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 641e149155fbee2655024bec3b76dce5243e7d59
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362502"
---
<a name="backbone-template"></a><span data-ttu-id="e148a-103">Šablona Backbone</span><span class="sxs-lookup"><span data-stu-id="e148a-103">Backbone Template</span></span>
====================
<span data-ttu-id="e148a-104">podle [Autor: Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="e148a-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="e148a-105">Šablona Backbone SPA zapsal Kazi Manzur Rashid</span><span class="sxs-lookup"><span data-stu-id="e148a-105">The Backbone SPA Template was written by Kazi Manzur Rashid</span></span>
> 
> [<span data-ttu-id="e148a-106">Stáhněte si šablonu Backbone.js SPA</span><span class="sxs-lookup"><span data-stu-id="e148a-106">Download the Backbone.js SPA Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=293631)


<span data-ttu-id="e148a-107">Šablona Backbone.js SPA je navržena tak, jak začít rychle vytvářet interaktivní webové klientské aplikace s využitím [Backbone.js.](http://backbonejs.org/)</span><span class="sxs-lookup"><span data-stu-id="e148a-107">The Backbone.js SPA template is designed to get you started quickly building interactive client-side web apps using [Backbone.js.](http://backbonejs.org/)</span></span>

<span data-ttu-id="e148a-108">Šablona nabízí počáteční skeleton pro vývoj aplikací Backbone.js v architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e148a-108">The template provides an initial skeleton for developing a Backbone.js application in ASP.NET MVC.</span></span> <span data-ttu-id="e148a-109">Ihned poskytuje funkce základní uživatel přihlášení, včetně resetování hesla registrace, přihlášení, uživatele a potvrzení uživatele s základní e-mailové šablony.</span><span class="sxs-lookup"><span data-stu-id="e148a-109">Out of the box it provides basic user login functionality, including user sign-up, sign-in, password reset, and user confirmation with basic email templates.</span></span>

<span data-ttu-id="e148a-110">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="e148a-110">Requirements:</span></span>

- [<span data-ttu-id="e148a-111">Aktualizace technologie ASP.NET and Web Tools 2012.2</span><span class="sxs-lookup"><span data-stu-id="e148a-111">ASP.NET and Web Tools 2012.2 update</span></span>](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a><span data-ttu-id="e148a-112">Vytvoření projektu páteřní šablony</span><span class="sxs-lookup"><span data-stu-id="e148a-112">Create a Backbone Template Project</span></span>

<span data-ttu-id="e148a-113">Stáhněte a nainstalujte šablony kliknutím na tlačítko Stáhnout výše.</span><span class="sxs-lookup"><span data-stu-id="e148a-113">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="e148a-114">Šablona je zabalena jako soubor rozšíření aplikace Visual Studio (VSIX).</span><span class="sxs-lookup"><span data-stu-id="e148a-114">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="e148a-115">Můžete potřebovat restartovat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e148a-115">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="e148a-116">V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e148a-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e148a-117">V části **Visual C#** vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="e148a-117">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e148a-118">V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e148a-118">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e148a-119">Pojmenujte projekt a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e148a-119">Name the project and click **OK**.</span></span>

![](backbonejs-template/_static/image1.png)

<span data-ttu-id="e148a-120">V **nový projekt** průvodce, vyberte projekt SPA Backbone.js.</span><span class="sxs-lookup"><span data-stu-id="e148a-120">In the **New Project** wizard, select Backbone.js SPA Project.</span></span>

![](backbonejs-template/_static/image2.png)

<span data-ttu-id="e148a-121">Stisknutím kláves Ctrl-F5 sestavte a spusťte aplikaci bez ladění nebo stisknutím klávesy F5 spusťte ladění.</span><span class="sxs-lookup"><span data-stu-id="e148a-121">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](backbonejs-template/_static/image3.png)

<span data-ttu-id="e148a-122">Kliknutím na "Můj účet" zobrazí přihlašovací stránku:</span><span class="sxs-lookup"><span data-stu-id="e148a-122">Clicking "My Account" brings up the login page:</span></span>

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a><span data-ttu-id="e148a-123">Návod: Klientský kód</span><span class="sxs-lookup"><span data-stu-id="e148a-123">Walkthrough: Client Code</span></span>

<span data-ttu-id="e148a-124">Pojďme začíná na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e148a-124">Let's starts with the client side.</span></span> <span data-ttu-id="e148a-125">Klientské aplikace skripty jsou umístěny ve složce ~/Scripts/application.</span><span class="sxs-lookup"><span data-stu-id="e148a-125">The client application scripts are located in the ~/Scripts/application folder.</span></span> <span data-ttu-id="e148a-126">Aplikace je napsána v [TypeScript](http://www.typescriptlang.org/) (soubory .ts) které jsou kompilovány do jazyka JavaScript (.js soubory).</span><span class="sxs-lookup"><span data-stu-id="e148a-126">The application is written in [TypeScript](http://www.typescriptlang.org/) (.ts files) which are compiled into JavaScript (.js files).</span></span>

<span data-ttu-id="e148a-127">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="e148a-127">**Application**</span></span>

<span data-ttu-id="e148a-128">`Application` je definováno v application.ts.</span><span class="sxs-lookup"><span data-stu-id="e148a-128">`Application` is defined in application.ts.</span></span> <span data-ttu-id="e148a-129">Tento objekt inicializuje aplikaci a funguje jako kořenový obor názvů.</span><span class="sxs-lookup"><span data-stu-id="e148a-129">This object initializes the application and acts as the root namespace.</span></span> <span data-ttu-id="e148a-130">Udržuje informace o konfiguraci a stavu, který je sdílen mezi aplikace, jako například, jestli je uživatel přihlášený.</span><span class="sxs-lookup"><span data-stu-id="e148a-130">It maintains configuration and state information that is shared across the application, such as whether the user is signed in.</span></span>

<span data-ttu-id="e148a-131">`application.start` Metoda vytvoří modální zobrazení a připojí obslužné rutiny událostí pro události na úrovni aplikace, jako je například přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="e148a-131">The `application.start` method creates the modal views and attaches event handlers for application-level events, such as user sign-in.</span></span> <span data-ttu-id="e148a-132">Dále vytvoří výchozí směrovač a zkontroluje, zda je zadán libovolnou adresu URL na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e148a-132">Next, it creates the default router and checks whether any client-side URL is specified.</span></span> <span data-ttu-id="e148a-133">Pokud ne, přesměruje na výchozí adresu url (#! /).</span><span class="sxs-lookup"><span data-stu-id="e148a-133">If not, it redirects to the default url (#!/).</span></span>

<span data-ttu-id="e148a-134">**Události**</span><span class="sxs-lookup"><span data-stu-id="e148a-134">**Events**</span></span>

<span data-ttu-id="e148a-135">Události jsou důležité vždy při vývoji volně vázanými komponentami.</span><span class="sxs-lookup"><span data-stu-id="e148a-135">Events are always important when developing loosely coupled components.</span></span> <span data-ttu-id="e148a-136">Aplikace často provádět více operací v reakci na akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="e148a-136">Applications often perform multiple operations in response to a user action.</span></span> <span data-ttu-id="e148a-137">Páteřní poskytuje integrované události s komponentami, jako je Model, kolekci a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e148a-137">Backbone provides built-in events with components such as Model, Collection, and View.</span></span> <span data-ttu-id="e148a-138">Místo vytváření mezi závislosti mezi komponentami, šablony používá model "pub/sub": `events` objekt definovaný v events.ts, funguje jako Centrum událostí pro publikování a přihlášení k odběru událostí aplikace.</span><span class="sxs-lookup"><span data-stu-id="e148a-138">Instead of creating inter-dependencies among these components, the template uses a "pub/sub" model: The `events` object, defined in events.ts, acts as an event hub for publishing and subscribing to application events.</span></span> <span data-ttu-id="e148a-139">`events` Objekt je typu singleton.</span><span class="sxs-lookup"><span data-stu-id="e148a-139">The `events` object is a singleton.</span></span> <span data-ttu-id="e148a-140">Následující kód ukazuje, jak přihlásit odběr události a poté spuštění události:</span><span class="sxs-lookup"><span data-stu-id="e148a-140">The following code shows how to subscribe to an event and then trigger the event:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

<span data-ttu-id="e148a-141">**Směrovače**</span><span class="sxs-lookup"><span data-stu-id="e148a-141">**Router**</span></span>

<span data-ttu-id="e148a-142">Směrovač v Backbone.js, poskytuje metody pro směrování stránky na straně klienta a připojte je ke akcích a událostech.</span><span class="sxs-lookup"><span data-stu-id="e148a-142">In Backbone.js, a router provides methods for routing client-side pages and connecting them to actions and events.</span></span> <span data-ttu-id="e148a-143">Šablona definuje jeden směrovač v router.ts.</span><span class="sxs-lookup"><span data-stu-id="e148a-143">The template defines a single router, in router.ts.</span></span> <span data-ttu-id="e148a-144">Směrovač activable zobrazení vytvoří a udržuje stav při přepnutí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e148a-144">The router creates the activable views and maintains the state when switching views.</span></span> <span data-ttu-id="e148a-145">(Activable zobrazení jsou popsané v další části). Na začátku projektu má dvě fiktivní zobrazení domácích a o.</span><span class="sxs-lookup"><span data-stu-id="e148a-145">(Activable views are described in the next section.) Initially, the project has two dummy views, Home and About.</span></span> <span data-ttu-id="e148a-146">Má také NotFound zobrazení, které se zobrazí, pokud je trasa není znám.</span><span class="sxs-lookup"><span data-stu-id="e148a-146">It also has a NotFound view, which is displayed if the route is not known.</span></span>

<span data-ttu-id="e148a-147">**Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="e148a-147">**Views**</span></span>

<span data-ttu-id="e148a-148">Zobrazení jsou definovány v ~/Scripts/application a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e148a-148">The views are defined in ~/Scripts/application/views.</span></span> <span data-ttu-id="e148a-149">Existují dva druhy, activable zobrazení a zobrazení modálního dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e148a-149">There are two kinds of views, activable views and modal dialog views.</span></span> <span data-ttu-id="e148a-150">Activable zobrazení jsou vyvolány směrovačem.</span><span class="sxs-lookup"><span data-stu-id="e148a-150">Activable views are invoked by the router.</span></span> <span data-ttu-id="e148a-151">Když activable zobrazení se zobrazí, všech ostatních zobrazeních activable neaktivní.</span><span class="sxs-lookup"><span data-stu-id="e148a-151">When an activable view is shown, all other activable views become inactive.</span></span> <span data-ttu-id="e148a-152">Vytvořit activable zobrazení, rozšíření zobrazení s `Activable` objektu:</span><span class="sxs-lookup"><span data-stu-id="e148a-152">To create an activable view, extend the view with the `Activable` object:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

<span data-ttu-id="e148a-153">Rozšíření s `Activable` přidá dva nové metody pro zobrazení, `activate` a `deactivate`.</span><span class="sxs-lookup"><span data-stu-id="e148a-153">Extending with `Activable` adds two new methods to the view, `activate` and `deactivate`.</span></span> <span data-ttu-id="e148a-154">Směrovač voláním metod aktivace a deactive zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e148a-154">The router calls these methods to activate and deactive the view.</span></span>

<span data-ttu-id="e148a-155">Modální zobrazení jsou implementovány jako [architekturu Twitter Bootstrap](http://twitter.github.com/bootstrap/) modální dialogová okna.</span><span class="sxs-lookup"><span data-stu-id="e148a-155">Modal views are implemented as [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modal dialogs.</span></span> <span data-ttu-id="e148a-156">`Membership` a `Profile` zobrazení jsou modální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e148a-156">The `Membership` and `Profile` views are modal views.</span></span> <span data-ttu-id="e148a-157">Zobrazení modelu, jde vyvolat události žádné aplikace.</span><span class="sxs-lookup"><span data-stu-id="e148a-157">Model views can be invoked by any application events.</span></span> <span data-ttu-id="e148a-158">Například v `Navigation` zobrazení, kliknutím na odkaz "Můj účet" obsahuje buď `Membership` zobrazení nebo `Profile` zobrazení, v závislosti na tom, jestli je uživatel přihlášen.</span><span class="sxs-lookup"><span data-stu-id="e148a-158">For example, in the `Navigation` view, clicking the "My Account" link shows either the `Membership` view or the `Profile` view, depending on whether the user is logged in.</span></span> <span data-ttu-id="e148a-159">`Navigation` Bude k obrazci, klikněte na tlačítko obslužné rutiny událostí pro všechny podřízené prvky, které mají `data-command` atribut.</span><span class="sxs-lookup"><span data-stu-id="e148a-159">The `Navigation` attaches click event handlers to any child elements that have the `data-command` attribute.</span></span> <span data-ttu-id="e148a-160">Tady je značka jazyka HTML:</span><span class="sxs-lookup"><span data-stu-id="e148a-160">Here is the HTML markup:</span></span>

[!code-html[Main](backbonejs-template/samples/sample3.html)]

<span data-ttu-id="e148a-161">Zde je kód v navigation.ts k připojení události:</span><span class="sxs-lookup"><span data-stu-id="e148a-161">Here is the code in navigation.ts to hook up the events:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

<span data-ttu-id="e148a-162">**Modely**</span><span class="sxs-lookup"><span data-stu-id="e148a-162">**Models**</span></span>

<span data-ttu-id="e148a-163">Tyto modely jsou definovány v ~/Scripts/application/modely.</span><span class="sxs-lookup"><span data-stu-id="e148a-163">The models are defined in ~/Scripts/application/models.</span></span> <span data-ttu-id="e148a-164">Všechny modely mají tři základní akce: výchozí atributy, ověřovací pravidla a koncový bod na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="e148a-164">The models all have three basic things: default attributes, validation rules, and a server-side end point.</span></span> <span data-ttu-id="e148a-165">Tady je typickým příkladem:</span><span class="sxs-lookup"><span data-stu-id="e148a-165">Here is a typical example:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

<span data-ttu-id="e148a-166">**Moduly plug-in**</span><span class="sxs-lookup"><span data-stu-id="e148a-166">**Plug-ins**</span></span>

<span data-ttu-id="e148a-167">Složka ~/Scripts/application/lib obsahuje několik modulů plug-in jQuery po ruce. Definuje form.ts souboru modulu plug-in pro práci s daty formuláře.</span><span class="sxs-lookup"><span data-stu-id="e148a-167">The ~/Scripts/application/lib folder contains a few handy jQuery plug-ins. The form.ts file defines a plug-in for working with form data.</span></span> <span data-ttu-id="e148a-168">Často je potřeba serializovat nebo deserializovat data formuláře a zobrazit všechny chyby ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="e148a-168">Often you need to serialize or deserialize form data and show any model validation errors.</span></span> <span data-ttu-id="e148a-169">Modul plug-in form.ts, jako má metody `serializeFields`, `deserializeFields`, a `showFieldErrors`.</span><span class="sxs-lookup"><span data-stu-id="e148a-169">The form.ts plug-in has methods such as `serializeFields`, `deserializeFields`, and `showFieldErrors`.</span></span> <span data-ttu-id="e148a-170">Následující příklad ukazuje, jak k serializaci formuláře k modelu.</span><span class="sxs-lookup"><span data-stu-id="e148a-170">The following example shows how to serialize a form to a model.</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

<span data-ttu-id="e148a-171">Modul plug-in flashbar.ts poskytuje různé typy zpráv se zpětnou vazbou pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e148a-171">The flashbar.ts plug-in gives various kinds of feedback messages to the user.</span></span> <span data-ttu-id="e148a-172">Metody jsou `$.showSuccessbar`, `$.showErrorbar` a `$.showInfobar`.</span><span class="sxs-lookup"><span data-stu-id="e148a-172">The methods are `$.showSuccessbar`, `$.showErrorbar` and `$.showInfobar`.</span></span> <span data-ttu-id="e148a-173">Na pozadí používá architekturu Twitter Bootstrap výstrahy zobrazíte hezky animovaných zprávy.</span><span class="sxs-lookup"><span data-stu-id="e148a-173">Behind the scenes, it uses Twitter Bootstrap alerts to show nicely animated messages.</span></span>

<span data-ttu-id="e148a-174">Modul plug-in confirm.ts nahradí prohlížeče dialogové okno, potvrzení, i když se rozhraní API se poněkud liší:</span><span class="sxs-lookup"><span data-stu-id="e148a-174">The confirm.ts plug-in replaces the browser's confirm dialog, although the API is somewhat different:</span></span>

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a><span data-ttu-id="e148a-175">Návod: Kód serveru</span><span class="sxs-lookup"><span data-stu-id="e148a-175">Walkthrough: Server Code</span></span>

<span data-ttu-id="e148a-176">Nyní Pojďme se podívat na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="e148a-176">Now let's look at the server side.</span></span>

<span data-ttu-id="e148a-177">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="e148a-177">**Controllers**</span></span>

<span data-ttu-id="e148a-178">V jednostránkové aplikaci hraje na server pouze malé roli v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e148a-178">In a single page application, the server plays only a small role in the user interface.</span></span> <span data-ttu-id="e148a-179">Obvykle serveru vykreslí stránku pro počáteční a pak odesílá i přijímá JSON data.</span><span class="sxs-lookup"><span data-stu-id="e148a-179">Typically, the server renders the initial page and then sends and receives JSON data.</span></span>

<span data-ttu-id="e148a-180">Šablona má dva řadiče MVC: `HomeController` vykreslí stránku pro počáteční, a `SupportsController` se používá k potvrzení nové uživatelské účty a resetování hesel.</span><span class="sxs-lookup"><span data-stu-id="e148a-180">The template has two MVC controllers: `HomeController` renders the initial page, and `SupportsController` is used to confirm new user accounts and reset passwords.</span></span> <span data-ttu-id="e148a-181">Všechny řadiče v šabloně jsou řadiče rozhraní ASP.NET Web API, které odesílat a přijímat JSON data.</span><span class="sxs-lookup"><span data-stu-id="e148a-181">All other controllers in the template are ASP.NET Web API controllers, which send and receive JSON data.</span></span> <span data-ttu-id="e148a-182">Ve výchozím nastavení, pomocí nové řadiče `WebSecurity` pro provádění úlohy související s uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e148a-182">By default, the controllers use the new `WebSecurity` class to perform user-related tasks.</span></span> <span data-ttu-id="e148a-183">Však mají také volitelné konstruktory, které umožňují předat delegáty pro tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="e148a-183">However, they also have optional constructors that let you pass in delegates for these tasks.</span></span> <span data-ttu-id="e148a-184">Tím je testování jednodušší a umožňuje nahradit `WebSecurity` s něco jiného, pomocí kontejner IoC.</span><span class="sxs-lookup"><span data-stu-id="e148a-184">This makes testing easier, and lets you replace `WebSecurity` with something else, by using an IoC Container.</span></span> <span data-ttu-id="e148a-185">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="e148a-185">Here is an example:</span></span>

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a><span data-ttu-id="e148a-186">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="e148a-186">Views</span></span>

<span data-ttu-id="e148a-187">Zobrazení jsou navržené tak, aby modulární: každý oddíl na stránce má svůj vlastní vyhrazený zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e148a-187">The views are designed to be modular: Each section of a page has its own dedicated view.</span></span> <span data-ttu-id="e148a-188">V případě jednostránkové aplikace je běžné zahrnují zobrazení, které nemají žádné odpovídající kontroler.</span><span class="sxs-lookup"><span data-stu-id="e148a-188">In a single page application, it is common to include views that do not have any corresponding controller.</span></span> <span data-ttu-id="e148a-189">Zobrazení může obsahovat voláním `@Html.Partial('myView')`, ale to získá únavné.</span><span class="sxs-lookup"><span data-stu-id="e148a-189">You can include a view by calling `@Html.Partial('myView')`, but this gets tedious.</span></span> <span data-ttu-id="e148a-190">Abychom to usnadnili, Šablona definuje Pomocná metoda `IncludeClientViews`, který vykreslí všechna zobrazení v zadané složce:</span><span class="sxs-lookup"><span data-stu-id="e148a-190">To make this easier, the template defines a helper method, `IncludeClientViews`, that renders all of the views in a specified folder:</span></span>

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

<span data-ttu-id="e148a-191">Pokud není zadán název složky, je výchozí název složky "ClientViews".</span><span class="sxs-lookup"><span data-stu-id="e148a-191">If the folder name is not specified, the default folder name is "ClientViews".</span></span> <span data-ttu-id="e148a-192">Pokud klienta zobrazí také používá částečná zobrazení, název částečného zobrazení se znakem podtržítka (například `_SignUp`).</span><span class="sxs-lookup"><span data-stu-id="e148a-192">If your client view also uses partial views, name the partial view with an underscore character (for example, `_SignUp`).</span></span> <span data-ttu-id="e148a-193">`IncludeClientViews` Metoda vylučuje všechna zobrazení, jejichž název začíná podtržítkem.</span><span class="sxs-lookup"><span data-stu-id="e148a-193">The `IncludeClientViews` method excludes any views whose name starts with an underscore.</span></span> <span data-ttu-id="e148a-194">Chcete-li zahrnout do zobrazení klienta částečné zobrazení, zavolejte `Html.ClientView('SignUp')` místo `Html.Partial('_SignUp')`.</span><span class="sxs-lookup"><span data-stu-id="e148a-194">To include a partial view in the client view, call `Html.ClientView('SignUp')` instead of `Html.Partial('_SignUp')`.</span></span>

<span data-ttu-id="e148a-195">**Odesílání e-mailu**</span><span class="sxs-lookup"><span data-stu-id="e148a-195">**Sending Email**</span></span>

<span data-ttu-id="e148a-196">K odesílání e-mailová šablona používá [poštovní](http://aboutcode.net/postal).</span><span class="sxs-lookup"><span data-stu-id="e148a-196">To send email, the template uses [Postal](http://aboutcode.net/postal).</span></span> <span data-ttu-id="e148a-197">Nicméně je abstrahovaný poštovní od zbývající části kódu pomocí `IMailer` rozhraní, takže můžete snadno nahradit ho záznamem jinou implementaci.</span><span class="sxs-lookup"><span data-stu-id="e148a-197">However, Postal is abstracted from the rest of the code with the `IMailer` interface, so you can easily replace it with another implementation.</span></span> <span data-ttu-id="e148a-198">E-mailové šablony jsou umístěny ve složce zobrazení/e-mailů.</span><span class="sxs-lookup"><span data-stu-id="e148a-198">The email templates are located in the Views/Emails folder.</span></span> <span data-ttu-id="e148a-199">E-mailová adresa odesílatele je zadán v souboru web.config v `sender.email` klíč **appSettings** oddílu.</span><span class="sxs-lookup"><span data-stu-id="e148a-199">The sender's email address is specified in the web.config file, in the `sender.email` key of the **appSettings** section.</span></span> <span data-ttu-id="e148a-200">I když `debug="true"` v souboru web.config, aplikace nevyžaduje, aby uživatel e-mailové potvrzení pro urychlení vývoje.</span><span class="sxs-lookup"><span data-stu-id="e148a-200">Also, when `debug="true"` in web.config, the application does not require user email confirmation, to speed up development.</span></span>

## <a name="github"></a><span data-ttu-id="e148a-201">GitHub</span><span class="sxs-lookup"><span data-stu-id="e148a-201">GitHub</span></span>

<span data-ttu-id="e148a-202">Můžete také najít šablonu Backbone.js SPA na [Githubu](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span><span class="sxs-lookup"><span data-stu-id="e148a-202">You can also find the Backbone.js SPA template on [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).</span></span>
