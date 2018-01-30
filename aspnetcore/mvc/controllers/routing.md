---
title: "Směrování do akce Kontroleru"
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/routing
ms.openlocfilehash: ca4f40766b4ec3d59587109ec9d5a412e834e3ce
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="23d8c-102">Směrování do akce Kontroleru</span><span class="sxs-lookup"><span data-stu-id="23d8c-102">Routing to Controller Actions</span></span>

<span data-ttu-id="23d8c-103">Podle [Ryan Nowak](https://github.com/rynowak) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23d8c-103">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="23d8c-104">Jádro ASP.NET MVC používá směrování [middleware](../../fundamentals/middleware.md) odpovídající adresy URL příchozích požadavků a jejich namapování na akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-104">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="23d8c-105">Trasy jsou definovány v spuštění kódu nebo atributy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-105">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="23d8c-106">Postupy popisují, jak cest URL by měla odpovídat akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-106">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="23d8c-107">Trasy se také používají k vygenerování adres URL (pro odkazy) odeslaná v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="23d8c-107">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="23d8c-108">Akce se buď obvykle směrují nebo směrován atribut.</span><span class="sxs-lookup"><span data-stu-id="23d8c-108">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="23d8c-109">Uvedení trasu na kontroler nebo akce umožňuje atribut směrovat.</span><span class="sxs-lookup"><span data-stu-id="23d8c-109">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="23d8c-110">V tématu [ve smíšeném směrování](#routing-mixed-ref-label) Další informace.</span><span class="sxs-lookup"><span data-stu-id="23d8c-110">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="23d8c-111">Tento dokument vysvětluje interakce mezi MVC a směrování a jak typické zpřístupnění aplikace MVC použití funkce směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-111">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="23d8c-112">V tématu [směrování](xref:fundamentals/routing) podrobnosti o pokročilé směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-112">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="23d8c-113">Nastavení směrování middlewaru</span><span class="sxs-lookup"><span data-stu-id="23d8c-113">Setting up Routing Middleware</span></span>

<span data-ttu-id="23d8c-114">Ve vaší *konfigurace* metoda mohou se zobrazit podobné kódu:</span><span class="sxs-lookup"><span data-stu-id="23d8c-114">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="23d8c-115">Uvnitř volání `UseMvc`, `MapRoute` se používá k vytvoření jedné trasy, které budete označujeme jako `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-115">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="23d8c-116">Většinu aplikací MVC použije trasu pomocí šablony podobně jako `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-116">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="23d8c-117">Šablona trasy `"{controller=Home}/{action=Index}/{id?}"` může odpovídat cestu adresy URL jako `/Products/Details/5` , který extrahuje hodnoty trasy `{ controller = Products, action = Details, id = 5 }` podle tokenizaci cestu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-117">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="23d8c-118">MVC se pokusí najít řadič s názvem `ProductsController` a spuštěním akce `Details`:</span><span class="sxs-lookup"><span data-stu-id="23d8c-118">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="23d8c-119">Upozorňujeme, že v tomto příkladu by vazby modelu použít hodnotu `id = 5` nastavit `id` parametru `5` při vyvolání této akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-119">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="23d8c-120">Najdete v článku [vazby modelu](../models/model-binding.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="23d8c-120">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="23d8c-121">Pomocí `default` trasy:</span><span class="sxs-lookup"><span data-stu-id="23d8c-121">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="23d8c-122">Šablona trasy:</span><span class="sxs-lookup"><span data-stu-id="23d8c-122">The route template:</span></span>

* <span data-ttu-id="23d8c-123">`{controller=Home}`definuje `Home` jako výchozí`controller`</span><span class="sxs-lookup"><span data-stu-id="23d8c-123">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="23d8c-124">`{action=Index}`definuje `Index` jako výchozí`action`</span><span class="sxs-lookup"><span data-stu-id="23d8c-124">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="23d8c-125">`{id?}`definuje `id` jako volitelná</span><span class="sxs-lookup"><span data-stu-id="23d8c-125">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="23d8c-126">Výchozí a parametry volitelné trasy nemusí být k dispozici v cestě adresy URL pro shodu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-126">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="23d8c-127">V tématu [odkaz na šablonu trasy](../../fundamentals/routing.md#route-template-reference) podrobný popis syntaxe šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-127">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="23d8c-128">`"{controller=Home}/{action=Index}/{id?}"`Cesta adresy URL se může shodovat `/` a hodnoty trasy způsobí `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-128">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="23d8c-129">Hodnoty pro `controller` a `action` Zkontrolujte použití výchozí hodnoty, `id` neobsahuje hodnotu vzhledem k tomu, že neexistuje žádný odpovídající segment cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-129">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="23d8c-130">MVC využije tyto hodnoty trasy k vyberte `HomeController` a `Index` akce:</span><span class="sxs-lookup"><span data-stu-id="23d8c-130">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="23d8c-131">Pomocí této definici řadiče a šablonu trasy `HomeController.Index` akce by byl proveden v žádném z následující cesty adresy URL:</span><span class="sxs-lookup"><span data-stu-id="23d8c-131">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="23d8c-132">Metoda zvýšení pohodlí `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="23d8c-132">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="23d8c-133">Lze použít k nahrazení:</span><span class="sxs-lookup"><span data-stu-id="23d8c-133">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="23d8c-134">`UseMvc`a `UseMvcWithDefaultRoute` přidá instanci `RouterMiddleware` k middlewaru v řadě.</span><span class="sxs-lookup"><span data-stu-id="23d8c-134">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="23d8c-135">MVC nebude pracovat přímo s middleware a používá směrování pro zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="23d8c-135">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="23d8c-136">MVC je připojený k trasy prostřednictvím instance `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-136">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="23d8c-137">Kód uvnitř `UseMvc` je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="23d8c-137">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="23d8c-138">`UseMvc`všechny trasy přímo nedefinuje ho přidá do kolekce tras pro zástupný symbol `attribute` trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-138">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="23d8c-139">Přetížení `UseMvc(Action<IRouteBuilder>)` můžete přidat vlastní trasy a také podporuje směrováním atributů.</span><span class="sxs-lookup"><span data-stu-id="23d8c-139">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="23d8c-140">`UseMvc`a jeho změn přidá zástupný symbol pro atribut trasy – atribut směrování je vždy k dispozici bez ohledu na to, jak nakonfigurujete `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-140">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="23d8c-141">`UseMvcWithDefaultRoute`Definuje výchozí trasu a podporuje atribut směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-141">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="23d8c-142">[Směrováním atributů](#attribute-routing-ref-label) část obsahuje podrobné informace o směrování atribut.</span><span class="sxs-lookup"><span data-stu-id="23d8c-142">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="23d8c-143">Konvenční směrování</span><span class="sxs-lookup"><span data-stu-id="23d8c-143">Conventional routing</span></span>

<span data-ttu-id="23d8c-144">`default` Trasy:</span><span class="sxs-lookup"><span data-stu-id="23d8c-144">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="23d8c-145">Příkladem je *konvenční směrování*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-145">is an example of a *conventional routing*.</span></span> <span data-ttu-id="23d8c-146">Říkáme tento styl *konvenční směrování* protože navazuje *konvence* pro cesty adresy URL:</span><span class="sxs-lookup"><span data-stu-id="23d8c-146">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="23d8c-147">první segment cesty se mapuje na název řadiče</span><span class="sxs-lookup"><span data-stu-id="23d8c-147">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="23d8c-148">druhý mapuje název akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-148">the second maps to the action name.</span></span>

* <span data-ttu-id="23d8c-149">třetí segmentu se používá pro volitelné `id` slouží k mapování na modelu entity</span><span class="sxs-lookup"><span data-stu-id="23d8c-149">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="23d8c-150">Pomocí této `default` trasy, cesty URL `/Products/List` se mapuje `ProductsController.List` akce, a `/Blog/Article/17` mapuje `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-150">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="23d8c-151">Toto mapování je založena na názvy kontroleru a akce **pouze** a není založena na obory názvů, umístění zdrojových souborů nebo parametry metody.</span><span class="sxs-lookup"><span data-stu-id="23d8c-151">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="23d8c-152">Použití konvenční směrování s výchozí trasu umožňuje rychle vytvářet aplikace bez nutnosti spolu s novou vzor adresy URL pro každou akci, kterou definujete.</span><span class="sxs-lookup"><span data-stu-id="23d8c-152">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="23d8c-153">Pro aplikace s akcemi CRUD styl s konzistence pro adresy URL napříč řadičů pomůžou zjednodušit kódu a stát předvídatelnější uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="23d8c-153">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="23d8c-154">`id` Je definován jako volitelné šablonou trasy, což znamená, že vaše akce můžete provést bez zadané ID jako část adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-154">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="23d8c-155">Obvykle co se stane, pokud `id` je vynechán z adresy URL je, že bude nastavena pro `0` vazby modelu a v důsledku toho odpovídajícím databáze bude nalezena žádná entita `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-155">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="23d8c-156">Atribut směrování vám může jemně odstupňovanou kontrolu, aby se ID požadované pro některé akce a nikoli pro ostatní uživatele.</span><span class="sxs-lookup"><span data-stu-id="23d8c-156">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="23d8c-157">Podle konvence dokumentace bude obsahovat volitelné parametry jako `id` když jsou pravděpodobně uvedena správné použití.</span><span class="sxs-lookup"><span data-stu-id="23d8c-157">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="23d8c-158">Víc tras</span><span class="sxs-lookup"><span data-stu-id="23d8c-158">Multiple routes</span></span>

<span data-ttu-id="23d8c-159">Můžete přidat víc tras uvnitř `UseMvc` přidáním další volání `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-159">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="23d8c-160">Díky tomu můžete zadat více názvů nebo přidat konvenční trasy, které jsou vyhrazené pro určité akci, jako například:</span><span class="sxs-lookup"><span data-stu-id="23d8c-160">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="23d8c-161">`blog` Trasa sem *vyhrazené konvenční trasy*, což znamená, že používá konvenční směrování systému, ale je vyhrazen pro určité akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-161">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="23d8c-162">Vzhledem k tomu `controller` a `action` nezobrazí v šabloně trasy jako parametry, budou mít pouze výchozí hodnoty a proto bude tato trasa vždy mapovat akce `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-162">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="23d8c-163">Trasy do kolekce tras se seřadí a budou zpracovány v pořadí, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="23d8c-163">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="23d8c-164">Ano v tomto příkladu `blog` vyzkouší se trasy před `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-164">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-165">*Vyhrazené konvenční trasy* často používají parametry trasy catch-all jako `{*article}` k zachycení zbývající část cesty URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-165">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="23d8c-166">Může být trasu 'příliš typu greedy, což znamená, že se shoduje adresy URL, které byste chtěli odpovídala jiným trasám.</span><span class="sxs-lookup"><span data-stu-id="23d8c-166">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="23d8c-167">Uveďte, typu greedy' trasy později ve směrovací tabulce, chcete-li tento problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="23d8c-167">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="23d8c-168">Záložní volba</span><span class="sxs-lookup"><span data-stu-id="23d8c-168">Fallback</span></span>

<span data-ttu-id="23d8c-169">V rámci zpracování žádosti se MVC ověří, že hodnoty trasy můžete použít k vyhledání kontroleru a akce v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-169">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="23d8c-170">Pokud se hodnoty trasy neshodují akce pak není trasy považovány za shodné a vyzkouší se další trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-170">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="23d8c-171">To se označuje jako *záložní*, a je určen ke zjednodušení případech, kde konvenční trasy překrývat.</span><span class="sxs-lookup"><span data-stu-id="23d8c-171">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="23d8c-172">Nejednoznačnosti akce</span><span class="sxs-lookup"><span data-stu-id="23d8c-172">Disambiguating actions</span></span>

<span data-ttu-id="23d8c-173">Při dvě akce odpovídat prostřednictvím směrování, musí na zvolte "nejlepší" candidate, jinak se vyvolat výjimku rozlišení MVC.</span><span class="sxs-lookup"><span data-stu-id="23d8c-173">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="23d8c-174">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d8c-174">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="23d8c-175">Tento řadič definuje dvě akce, které by odpovídalo cesty URL `/Products/Edit/17` a data trasy `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-175">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="23d8c-176">Toto je typický vzor MVC řadiče kde `Edit(int)` zobrazí formulář upravit produktu, a `Edit(int, Product)` zpracovává odeslaného formuláře.</span><span class="sxs-lookup"><span data-stu-id="23d8c-176">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="23d8c-177">Chcete-li to možné, bude muset zvolit MVC `Edit(int, Product)` po požadavku HTTP `POST` a `Edit(int)` když příkaz protokolu HTTP je jakýkoli jiný.</span><span class="sxs-lookup"><span data-stu-id="23d8c-177">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="23d8c-178">`HttpPostAttribute` ( `[HttpPost]` ) Je implementací `IActionConstraint` se povolit jenom akce, která má být vybrána při HTTP je `POST`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-178">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="23d8c-179">Přítomnost `IActionConstraint` umožňuje `Edit(int, Product)` shodovat lépe než `Edit(int)`, takže `Edit(int, Product)` vyzkouší se napřed.</span><span class="sxs-lookup"><span data-stu-id="23d8c-179">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="23d8c-180">Je potřeba jenom psaní vlastních `IActionConstraint` implementace v specializované scénáře, ale je důležité si uvědomit, roli atributů, například `HttpPostAttribute` -podobné atributy jsou definovány pro jiné akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="23d8c-180">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="23d8c-181">V konvenční směrování je běžné akce, které používají stejný název akce, pokud jsou součástí `show form -> submit form` pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-181">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="23d8c-182">Výhodou tohoto vzoru se stane po zkontrolování více zřejmá [IActionConstraint Principy](#understanding-iactionconstraint) části.</span><span class="sxs-lookup"><span data-stu-id="23d8c-182">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="23d8c-183">Pokud odpovídají víc tras a MVC nemůžete najít, nejlepší' směrování, vyvolá výjimku `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-183">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="23d8c-184">Názvy tras</span><span class="sxs-lookup"><span data-stu-id="23d8c-184">Route names</span></span>

<span data-ttu-id="23d8c-185">Řetězce `"blog"` a `"default"` v následujících příkladech jsou názvy tras:</span><span class="sxs-lookup"><span data-stu-id="23d8c-185">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="23d8c-186">Názvy tras pojmenujte trasy logické tak, aby pojmenovanou trasu lze použít pro generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-186">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="23d8c-187">To výrazně zjednodušuje vytvoření adresy URL při řazení tras se ověřte komplikovanější generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-187">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="23d8c-188">Názvy tras musí být jedinečný celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-188">Route names must be unique application-wide.</span></span>

<span data-ttu-id="23d8c-189">Názvy tras nemají žádný vliv na adresu URL odpovídající nebo zpracování požadavků; použijí se jenom pro generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-189">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="23d8c-190">[Směrování](xref:fundamentals/routing) obsahuje podrobnější informace o generování adresy URL včetně generování adresy URL v pomocné rutiny specifické pro MVC.</span><span class="sxs-lookup"><span data-stu-id="23d8c-190">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="23d8c-191">Atribut směrování</span><span class="sxs-lookup"><span data-stu-id="23d8c-191">Attribute routing</span></span>

<span data-ttu-id="23d8c-192">Atribut směrování používá sadu atributů k mapování akcí přímo na šablon trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-192">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="23d8c-193">V následujícím příkladu `app.UseMvc();` je používán `Configure` je předán metoda a žádná trasa.</span><span class="sxs-lookup"><span data-stu-id="23d8c-193">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="23d8c-194">`HomeController` Bude shodovat s sadu podobná jaké výchozí trasu adresy URL `{controller=Home}/{action=Index}/{id?}` odpovídá:</span><span class="sxs-lookup"><span data-stu-id="23d8c-194">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="23d8c-195">`HomeController.Index()` Akce se provede v žádném z cest URL `/`, `/Home`, nebo `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-195">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-196">V tomto příkladu jsou vysvětlené klíče programovací rozdíl mezi směrováním atributů a standardní metody směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-196">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="23d8c-197">Atribut směrování vyžaduje další vstup k určení trasu; konvenční výchozí trasu zpracovává více stručně trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-197">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="23d8c-198">Ale směrováním atributů umožňuje (a vyžaduje) přesné řízení, které šablon trasy použijí na každou akci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-198">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="23d8c-199">S atributem směrování názvu kontroleru a akce názvy přehrání **žádné** role, ve kterém je vybraná akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-199">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="23d8c-200">V tomto příkladu bude odpovídat stejné adresy URL jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-200">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="23d8c-201">Výše uvedené šablon trasy nemusíte definovat parametry trasy pro `action`, `area`, a `controller`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-201">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="23d8c-202">Tyto parametry trasy ve skutečnosti nejsou povoleny v atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-202">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="23d8c-203">Vzhledem k tomu, že šablona trasy je již přidružen akci, by nedávalo smysl analyzovat název akce z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-203">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="23d8c-204">Atribut směrování s atributy Http [akce]</span><span class="sxs-lookup"><span data-stu-id="23d8c-204">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="23d8c-205">Atribut směrování provést také použít `Http[Verb]` atributy, jako `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-205">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="23d8c-206">Všechny tyto atributy mohou přijímat šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-206">All of these attributes can accept a route template.</span></span> <span data-ttu-id="23d8c-207">Tento příklad ukazuje dvě akce, které odpovídají stejnou šablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="23d8c-207">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="23d8c-208">Pro cestu adresy URL jako `/products` `ProductsApi.ListProducts` akce se provede, pokud je příkazem HTTP příkaz `GET` a `ProductsApi.CreateProduct` bude proveden, pokud je příkazem HTTP příkaz `POST`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-208">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="23d8c-209">Atribut směrování nejprve odpovídá adrese URL pro sadu šablon trasy definované atributy trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-209">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="23d8c-210">Jakmile odpovídá šablonu trasy, `IActionConstraint` omezení se použijí k určení, jaké akce lze provádět.</span><span class="sxs-lookup"><span data-stu-id="23d8c-210">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="23d8c-211">Při sestavování rozhraní REST API, navíc není obvyklé, že budete chtít použít `[Route(...)]` na metodu akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-211">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="23d8c-212">Je vhodnější použít další konkrétní `Http*Verb*Attributes` být přesně určit, co vaše rozhraní API podporuje.</span><span class="sxs-lookup"><span data-stu-id="23d8c-212">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="23d8c-213">Očekává se, že klienti rozhraní REST API vědět, co cesty a HTTP se mapují na konkrétní logické operace.</span><span class="sxs-lookup"><span data-stu-id="23d8c-213">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="23d8c-214">Vzhledem k tomu, že atribut trasy se vztahuje na určité akci, je usnadňuje parametrů požadovaných jako součást definice šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-214">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="23d8c-215">V tomto příkladu `id` je vyžadován jako součást cesty URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-215">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="23d8c-216">`ProductsApi.GetProduct(int)` Akce se provede pro cestu adresy URL jako `/products/3` , ale ne pro cestu adresy URL jako `/products`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-216">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="23d8c-217">V tématu [směrování](../../fundamentals/routing.md) úplný popis šablon trasy a související možnosti.</span><span class="sxs-lookup"><span data-stu-id="23d8c-217">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="23d8c-218">Název trasy</span><span class="sxs-lookup"><span data-stu-id="23d8c-218">Route Name</span></span>

<span data-ttu-id="23d8c-219">Následující kód definuje *název trasy* z `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="23d8c-219">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="23d8c-220">Názvy tras slouží ke generování adresy URL na základě konkrétní trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-220">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="23d8c-221">Názvy tras nemají žádný vliv na porovnávání chování směrování adres URL a používají jenom pro generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-221">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="23d8c-222">Názvy tras musí být jedinečný celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-222">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-223">Rozdíl oproti to běžné *výchozí trasu*, která definuje `id` parametr jako volitelný (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="23d8c-223">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="23d8c-224">Tato schopnost přesněji určit rozhraní API má výhod, například umožníte `/products` a `/products/5` aby byly odeslány různé akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-224">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="23d8c-225">Kombinování trasy</span><span class="sxs-lookup"><span data-stu-id="23d8c-225">Combining routes</span></span>

<span data-ttu-id="23d8c-226">Chcete-li směrováním atributů méně opakují, atributů tras na řadiči spolu se atributů tras na jednotlivé akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-226">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="23d8c-227">Všechny šablony trasy definované v řadiči se přidá jako předpona k šablon trasy na akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-227">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="23d8c-228">Umístění atribut trasy na řadiči díky **všechny** akce v kontroleru využívají směrováním atributů.</span><span class="sxs-lookup"><span data-stu-id="23d8c-228">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="23d8c-229">V tomto příkladu cesty URL `/products` může odpovídat `ProductsApi.ListProducts`a cesty URL `/products/5` může odpovídat `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-229">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="23d8c-230">Obě tyto akce HTTP odpovídá pouze `GET` vzhledem k tomu, že označených pomocí `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-230">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="23d8c-231">Směrovat šablony použít akci, která začínají `/` nemáte získat v kombinaci s šablon trasy použijí pro kontroler.</span><span class="sxs-lookup"><span data-stu-id="23d8c-231">Route templates applied to an action that begin with a `/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="23d8c-232">Tento příklad odpovídá cesty adresy URL, podobně jako *výchozí trasu*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-232">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="23d8c-233">Pořadí trasy atributů</span><span class="sxs-lookup"><span data-stu-id="23d8c-233">Ordering attribute routes</span></span>

<span data-ttu-id="23d8c-234">Konvenční trasy, který se spustí v zadaném pořadí, na rozdíl od směrováním atributů vytvoří strom a současně odpovídá všechny trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-234">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="23d8c-235">To se chová jako – Pokud položky trasy byly umístěny v ideální řazení; Většina konkrétní trasy mít příležitost se provést před obecnější trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-235">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="23d8c-236">Například trasu typu `blog/search/{topic}` je specifičtější než trasu jako `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-236">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="23d8c-237">Logicky mluvení `blog/search/{topic}` trasy ", nejprve ve výchozím nastavení spustí, protože se jedná pouze rozumný řazení.</span><span class="sxs-lookup"><span data-stu-id="23d8c-237">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="23d8c-238">Použití konvenční směrování, vývojář je odpovědná za uvedení do požadovaného pořadí trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-238">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="23d8c-239">Atribut trasy můžete nakonfigurovat pořadí, pomocí `Order` vlastnost všechny atributy rámec poskytovaný trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-239">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="23d8c-240">Trasy se zpracovávají podle vzestupném seřadit na `Order` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23d8c-240">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="23d8c-241">Výchozí pořadí je `0`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-241">The default order is `0`.</span></span> <span data-ttu-id="23d8c-242">Nastavení směrování pomocí `Order = -1` se spustit před spuštěním tras, které není nastavený pořadí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-242">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="23d8c-243">Nastavení směrování pomocí `Order = 1` se spustí po změně pořadí trasy výchozí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-243">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="23d8c-244">Vyhněte se v závislosti na `Order`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-244">Avoid depending on `Order`.</span></span> <span data-ttu-id="23d8c-245">Pokud váš prostor adresy URL vyžaduje, aby explicitní pořadí hodnoty trasy správně, je pravděpodobně matoucí také klientům.</span><span class="sxs-lookup"><span data-stu-id="23d8c-245">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="23d8c-246">Obecně se směrováním atributů vyberte správné směrování na odpovídající adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-246">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="23d8c-247">Pokud není výchozí pořadí používá ke generování adresy URL pro funguje, pomocí názvu trasy, protože přepsání je většinou jednodušší než použití `Order` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23d8c-247">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="23d8c-248">Token nahrazení v šablonách tras ([řadič], [akce] [Oblast])</span><span class="sxs-lookup"><span data-stu-id="23d8c-248">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="23d8c-249">Pro usnadnění práce trasy atributů podporují *token nahrazení* uzavřením token do hranaté závorky (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="23d8c-249">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="23d8c-250">Tokeny `[action]`, `[area]`, a `[controller]` nahradí hodnoty názvu akce, názvu oblasti a názvu kontroleru z akce, kde je definován trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-250">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="23d8c-251">V tomto příkladu může akce odpovídat cesty adresy URL, jak je popsáno v komentářích:</span><span class="sxs-lookup"><span data-stu-id="23d8c-251">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="23d8c-252">Jako poslední krok sestavování tras atributů dojde k nahrazení tokenu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-252">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="23d8c-253">Výše uvedeném příkladu bude chovají stejně jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="23d8c-253">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="23d8c-254">Trasy atributů může být spojen s dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="23d8c-254">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="23d8c-255">To je zvláště efektivní, v kombinaci s tokenu nahrazení.</span><span class="sxs-lookup"><span data-stu-id="23d8c-255">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="23d8c-256">Token nahrazení platí i pro názvy trasy definované trasy atributů.</span><span class="sxs-lookup"><span data-stu-id="23d8c-256">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="23d8c-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]`vygeneruje trasy jedinečný název pro každou akci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="23d8c-258">Tak, aby odpovídaly oddělovač literálu tokenu nahrazení `[` nebo `]`, vyhnuli opakováním znak (`[[` nebo `]]`).</span><span class="sxs-lookup"><span data-stu-id="23d8c-258">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="23d8c-259">Víc tras</span><span class="sxs-lookup"><span data-stu-id="23d8c-259">Multiple Routes</span></span>

<span data-ttu-id="23d8c-260">Atribut směrování podporuje definování víc tras, které dosáhnout stejné akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-260">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="23d8c-261">Nejběžnější využití tohoto objektu je tak, aby napodoboval chování *výchozí trasu konvenční* jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="23d8c-261">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="23d8c-262">Vložení několika atributů tras na řadiči znamená, že každé z nich bude kombinovat k jednotlivým atributům trasy na metody akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-262">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="23d8c-263">Při několika atributů tras (které implementují `IActionConstraint`) jsou umístěny na akce, poté každý akce omezení spojuje se šablona trasy z atributu, který je definován.</span><span class="sxs-lookup"><span data-stu-id="23d8c-263">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="23d8c-264">Při použití více tras na akce může vypadat výkonné, je lepší jednoduché a dobře definovaný zachovat místo adresy URL vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="23d8c-264">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="23d8c-265">Akce při použijte víc tras, pouze v případě potřeby, třeba zajistit podporu pro existující klienty.</span><span class="sxs-lookup"><span data-stu-id="23d8c-265">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="23d8c-266">Volitelné parametry atribut trasy, výchozí hodnoty a omezení</span><span class="sxs-lookup"><span data-stu-id="23d8c-266">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="23d8c-267">Trasy atributů podporují stejnou syntaxi vložených jako konvenční trasy k určení volitelné parametry, výchozí hodnoty a omezení.</span><span class="sxs-lookup"><span data-stu-id="23d8c-267">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="23d8c-268">V tématu [odkaz na šablonu trasy](../../fundamentals/routing.md#route-template-reference) podrobný popis syntaxe šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-268">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="23d8c-269">Vlastní trasy atributů s použitím`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="23d8c-269">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="23d8c-270">Všechny atributy trasy zadaný v rozhraní framework ( `[Route(...)]`, `[HttpGet(...)]` atd) implementovat `IRouteTemplateProvider` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="23d8c-270">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="23d8c-271">MVC hledá atributy na řadič třídy a metody akce při spuštění a použije parametry, které implementují aplikace `IRouteTemplateProvider` k vytvoření počáteční sadu trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-271">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="23d8c-272">Můžete implementovat `IRouteTemplateProvider` definovat vlastní atributy trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-272">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="23d8c-273">Každý `IRouteTemplateProvider` umožňuje definovat jednu trasu pomocí šablony vlastní trasy, řazení a název:</span><span class="sxs-lookup"><span data-stu-id="23d8c-273">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="23d8c-274">Atribut z výše uvedeném příkladu automaticky nastaví `Template` k `"api/[controller]"` při `[MyApiController]` platí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-274">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="23d8c-275">Přizpůsobení trasy atributů pomocí aplikačního modelu</span><span class="sxs-lookup"><span data-stu-id="23d8c-275">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="23d8c-276">*Aplikačního modelu* je model objektu vytvořeny při spuštění se všemi metadat používané MVC trasy a spouštět vaše akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-276">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="23d8c-277">*Aplikačního modelu* zahrnuje všechna data shromážděná z atributů tras (prostřednictvím `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="23d8c-277">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="23d8c-278">Můžete napsat *konvence* k úpravě aplikačního modelu v čase spuštění k přizpůsobení chování směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-278">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="23d8c-279">Tato část ukazuje jednoduchý příklad přizpůsobení směrování pomocí aplikačního modelu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-279">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="23d8c-280">Ve smíšeném směrování: atribut směrování vs konvenční směrování</span><span class="sxs-lookup"><span data-stu-id="23d8c-280">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="23d8c-281">Aplikace MVC můžete kombinovat použití konvenční směrování a směrováním atributů.</span><span class="sxs-lookup"><span data-stu-id="23d8c-281">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="23d8c-282">Je typické používat běžné postupy pro řadiče obsluhující stránky HTML pro prohlížeče, a atribut směrování pro řadiče obsluhující rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="23d8c-282">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="23d8c-283">Akce se buď obvykle směrují nebo směrován atribut.</span><span class="sxs-lookup"><span data-stu-id="23d8c-283">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="23d8c-284">Uvedení trasu na kontroler nebo akce umožňuje atribut směrovat.</span><span class="sxs-lookup"><span data-stu-id="23d8c-284">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="23d8c-285">Akce, které definují trasy atributů není dostupný prostřednictvím konvenční trasy a naopak.</span><span class="sxs-lookup"><span data-stu-id="23d8c-285">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="23d8c-286">**Všechny** atribut trasy na řadiči díky všechny akce v kontroleru atributu směrovat.</span><span class="sxs-lookup"><span data-stu-id="23d8c-286">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-287">Co rozlišuje dva typy systémů směrování je proces, použijí po adresu URL odpovídá šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-287">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="23d8c-288">V konvenční směrování, hodnoty trasy z shody lze vybírat vyhledávací tabulky všechny běžné směrované akcí akce a kontroler.</span><span class="sxs-lookup"><span data-stu-id="23d8c-288">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="23d8c-289">V atributu směrování, každé šablony je již přidružen akci a je potřeba žádné další vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="23d8c-289">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="23d8c-290">Generování adresy URL</span><span class="sxs-lookup"><span data-stu-id="23d8c-290">URL Generation</span></span>

<span data-ttu-id="23d8c-291">Aplikace MVC můžete používat směrování na Funkce generování adresy URL pro generování odkazů URL na akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-291">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="23d8c-292">Generování adres URL eliminuje hardcoding adresy URL, provádění kódu robustnější a udržovatelný.</span><span class="sxs-lookup"><span data-stu-id="23d8c-292">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="23d8c-293">Tato část se zaměřuje na Funkce generování adresy URL poskytované MVC a pouze popisuje základní informace o tom, jak funguje generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-293">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="23d8c-294">V tématu [směrování](../../fundamentals/routing.md) podrobný popis generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-294">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="23d8c-295">`IUrlHelper` Rozhraní je základní část infrastruktury mezi MVC a směrování pro generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-295">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="23d8c-296">Zjistíte instanci `IUrlHelper` k dispozici prostřednictvím `Url` vlastnost řadiče, zobrazení a zobrazení součásti.</span><span class="sxs-lookup"><span data-stu-id="23d8c-296">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="23d8c-297">V tomto příkladu `IUrlHelper` rozhraní se používá prostřednictvím `Controller.Url` vlastnost ke generování adresy URL pro další akci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-297">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="23d8c-298">Pokud aplikace používá konvenční výchozí směrování, hodnota `url` proměnná bude řetězec cesty adresy URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-298">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="23d8c-299">Tato cesta adresy URL je vytvořený směrování kombinací hodnoty trasy z aktuální žádosti (vedlejším hodnoty), s hodnotami předaný `Url.Action` a nahraďte těmito hodnotami do šablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="23d8c-299">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="23d8c-300">Každý parametr trasy v šabloně trasy má svou hodnotu nahrazena odpovídající názvy s hodnotami a vedlejším hodnoty.</span><span class="sxs-lookup"><span data-stu-id="23d8c-300">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="23d8c-301">Parametr trasy, který nemá hodnotu můžete použít výchozí hodnotu, pokud má jednu, nebo vynechána, pokud je volitelné (stejně jako u `id` v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="23d8c-301">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="23d8c-302">Generování adresy URL se nezdaří, pokud parametr všechny požadované trasy nemá odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-302">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="23d8c-303">Pokud se nezdaří generování adresy URL pro trasu, zkusí se další trasy, dokud vyzkoušeny všechny trasy nebo je nalezena shoda.</span><span class="sxs-lookup"><span data-stu-id="23d8c-303">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="23d8c-304">Jako příklad `Url.Action` vyšší předpokládá konvenční směrování, ale adresa URL generování funguje podobně jako se směrováním atributů, i když koncepty se liší.</span><span class="sxs-lookup"><span data-stu-id="23d8c-304">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="23d8c-305">S konvenční směrování, hodnoty trasy slouží k rozbalte šablony a hodnoty trasy pro `controller` a `action` obvykle zobrazují v této šabloně – to funguje, protože dodržovat odpovídala směrování adres URL *konvence*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-305">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="23d8c-306">V atributu směrování, hodnoty trasy pro `controller` a `action` nejsou povoleny se objeví v šabloně – místo toho se použije pro vyhledávání kterou šablonu použít.</span><span class="sxs-lookup"><span data-stu-id="23d8c-306">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="23d8c-307">Tento příklad používá směrováním atributů:</span><span class="sxs-lookup"><span data-stu-id="23d8c-307">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="23d8c-308">MVC sestavení vyhledávací tabulky všech akcí atribut směrovat a bude odpovídat `controller` a `action` hodnoty a vyberte šablonu trasy sloužící ke generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-308">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="23d8c-309">V ukázce výše `custom/url/to/destination` se vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="23d8c-309">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="23d8c-310">Generování adres URL pomocí názvu akce</span><span class="sxs-lookup"><span data-stu-id="23d8c-310">Generating URLs by action name</span></span>

<span data-ttu-id="23d8c-311">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="23d8c-311">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="23d8c-312">`Action`) a všechny související přetížení všechny jsou založeny na tento nápad chcete určit, co odkazujete na zadáním názvu akce a názvu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="23d8c-312">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-313">Při použití `Url.Action`, aktuální trasy hodnoty pro `controller` a `action` jsou určené pro vás – hodnota `controller` a `action` jsou součástí obě *vedlejším hodnoty* **a** *hodnoty*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-313">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="23d8c-314">Metoda `Url.Action`, vždy používá aktuální hodnoty `action` a `controller` a vygeneruje cestu adresy URL, který směruje na aktuální akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-314">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="23d8c-315">Směrování pokusy o použití hodnot v vedlejším hodnoty vyplnit informace, které neposkytli při generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-315">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="23d8c-316">Použití trasy jako `{a}/{b}/{c}/{d}` a vedlejším hodnoty `{ a = Alice, b = Bob, c = Carol, d = David }`, směrování má dostatek informací ke generování adresy URL bez jakékoli další hodnoty – vzhledem k tomu, že všechny trasy parametry mají hodnotu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-316">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="23d8c-317">Pokud jste přidali hodnota `{ d = Donovan }`, hodnota `{ d = David }` by být ignorovány, a vygenerovaný adresa URL by obsahovat `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-317">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="23d8c-318">Jsou hierarchické cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-318">URL paths are hierarchical.</span></span> <span data-ttu-id="23d8c-319">V příkladu výše, pokud jste přidali hodnota `{ c = Cheryl }`, obě hodnoty `{ c = Carol, d = David }` by ignorovány.</span><span class="sxs-lookup"><span data-stu-id="23d8c-319">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="23d8c-320">V takovém případě již máme hodnotu `d` a generování adresy URL se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="23d8c-320">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="23d8c-321">Museli byste zadejte požadovanou hodnotu `c` a `d`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-321">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="23d8c-322">Můžete očekávat, že dosáhl tento problém se výchozí trasa (`{controller}/{action}/{id?}`)- ale zřídka narazíte na toto chování v praxi jako `Url.Action` bude vždy explicitně zadáte `controller` a `action` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-322">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="23d8c-323">Delší přetížení `Url.Action` také provést další *hodnot trasy* objekt, který má jiné než zadejte hodnoty pro parametry trasy `controller` a `action`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-323">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="23d8c-324">Můžete to použít s nejčastěji zobrazí `id` jako `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-324">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="23d8c-325">Podle konvence *hodnot trasy* objektu je obvykle objekt anonymního typu, ale také může být `IDictionary<>` nebo *prostý původního objektu .NET*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-325">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="23d8c-326">V řetězci dotazu jsou vloženy hodnoty žádné další trasy, které neodpovídají parametry trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-326">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="23d8c-327">Chcete-li vytvořit absolutní adresu URL, použijte přetížení, které přijímá `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="23d8c-327">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="23d8c-328">Generování adres URL pomocí trasy</span><span class="sxs-lookup"><span data-stu-id="23d8c-328">Generating URLs by route</span></span>

<span data-ttu-id="23d8c-329">Výše uvedený kód ukázán generování adresy URL předáním v názvu kontroleru a akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-329">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="23d8c-330">`IUrlHelper`také poskytuje `Url.RouteUrl` řady metod.</span><span class="sxs-lookup"><span data-stu-id="23d8c-330">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="23d8c-331">Tyto metody jsou podobná `Url.Action`, ale jejich nemáte zkopírovat aktuální hodnoty `action` a `controller` hodnoty trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-331">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="23d8c-332">Nejběžnější využití slouží k zadání názvu trasy používat konkrétní trasy pro generování adresy URL, obecně *bez* zadejte název a kontroler nebo akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-332">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="23d8c-333">Generování adres URL ve formátu HTML</span><span class="sxs-lookup"><span data-stu-id="23d8c-333">Generating URLs in HTML</span></span>

<span data-ttu-id="23d8c-334">`IHtmlHelper`poskytuje `HtmlHelper` metody `Html.BeginForm` a `Html.ActionLink` ke generování `<form>` a `<a>` elementy v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-334">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="23d8c-335">Tyto metody používat `Url.Action` metoda ke generování adresy URL a přijímají podobně jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="23d8c-335">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="23d8c-336">`Url.RouteUrl` Companions pro `HtmlHelper` jsou `Html.BeginRouteForm` a `Html.RouteLink` které mají podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-336">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="23d8c-337">TagHelpers vygenerování adres URL pomocí `form` TagHelper a `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="23d8c-337">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="23d8c-338">Obě tyto použít `IUrlHelper` pro jejich implementaci.</span><span class="sxs-lookup"><span data-stu-id="23d8c-338">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="23d8c-339">V tématu [práci s formuláři](../views/working-with-forms.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="23d8c-339">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="23d8c-340">Uvnitř zobrazení `IUrlHelper` je k dispozici prostřednictvím `Url` vlastnost pro generování adresy URL všech ad-hoc nevztahuje výše.</span><span class="sxs-lookup"><span data-stu-id="23d8c-340">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="23d8c-341">Generování adres URL ve výsledcích akce</span><span class="sxs-lookup"><span data-stu-id="23d8c-341">Generating URLS in Action Results</span></span>

<span data-ttu-id="23d8c-342">Výše uvedených příkladech ukázaly, pomocí `IUrlHelper` v kontroleru, zatímco nejběžnější využití v kontroleru je ke generování adresy URL jako součást výsledek akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-342">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="23d8c-343">`ControllerBase` a `Controller` základní třídy zprostředkovávají usnadňující metody pro výsledky akce, které odkazují jiné akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-343">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="23d8c-344">Jeden typickému využití je přesměrování po přijetí vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="23d8c-344">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="23d8c-345">Metody vytváření výsledky akce podle podobný Princip metody `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-345">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="23d8c-346">Zvláštním případem pro vyhrazené konvenční trasy</span><span class="sxs-lookup"><span data-stu-id="23d8c-346">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="23d8c-347">Konvenční směrování můžete použít zvláštní druh názvem definice trasy *vyhrazené konvenční trasy*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-347">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="23d8c-348">V následujícím příkladu s názvem trasy `blog` je vyhrazené konvenční trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-348">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="23d8c-349">Pomocí těchto definicí cesty `Url.Action("Index", "Home")` vygeneruje cesty URL `/` s `default` trasy, ale proč?</span><span class="sxs-lookup"><span data-stu-id="23d8c-349">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="23d8c-350">Hodnoty trasy může uhodnout `{ controller = Home, action = Index }` by být dost pro generování adres URL pomocí `blog`, a výsledkem bude `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-350">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="23d8c-351">Vyhrazené konvenční trasy spoléhají na zvláštní chování výchozí hodnoty, které nemají odpovídající parametr trasy, která trasy, která brání v dodržení "příliš chamtivého" s generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-351">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="23d8c-352">V takovém případě výchozí hodnoty jsou `{ controller = Blog, action = Article }`a ani `controller` ani `action` se zobrazí jako parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-352">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="23d8c-353">Když směrování provede generování adresy URL, zadané hodnoty musí odpovídat výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="23d8c-353">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="23d8c-354">Pomocí generování adresy URL `blog` selže, protože hodnoty `{ controller = Home, action = Index }` neodpovídají `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-354">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="23d8c-355">Směrování pak spadne zpět na akci `default`, což je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="23d8c-355">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="23d8c-356">Oblasti</span><span class="sxs-lookup"><span data-stu-id="23d8c-356">Areas</span></span>

<span data-ttu-id="23d8c-357">[Oblasti](areas.md) se o funkci MVC sloužící k organizování související funkce do skupiny jako samostatné směrování – obor názvů (pro akce kontroleru) a strukturu složek (pro zobrazení).</span><span class="sxs-lookup"><span data-stu-id="23d8c-357">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="23d8c-358">Použití oblastí umožňuje aplikaci používat více řadičů se stejným názvem – tak dlouho, dokud mají různé *oblasti*.</span><span class="sxs-lookup"><span data-stu-id="23d8c-358">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="23d8c-359">Použití oblastí vytvoří hierarchii pro účely směrování přidáním jiný parametr trasy, `area` k `controller` a `action`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-359">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="23d8c-360">V této části se popisují, jak směrování komunikuje s oblastí - najdete v části [oblasti](areas.md) podrobnosti o použití oblastí se zobrazeními.</span><span class="sxs-lookup"><span data-stu-id="23d8c-360">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="23d8c-361">Následující příklad konfiguruje MVC používat výchozí trasu konvenční a *oblasti trasy* pro oblast s názvem `Blog`:</span><span class="sxs-lookup"><span data-stu-id="23d8c-361">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="23d8c-362">Při kontrole shody cesty adresy URL jako `/Manage/Users/AddUser`, první trasy vygeneruje hodnoty trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-362">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="23d8c-363">`area` Hodnota trasy je produkovaný výchozí hodnotu pro `area`, ve skutečnosti trasy vytvořené `MapAreaRoute` je ekvivalentní k následujícímu:</span><span class="sxs-lookup"><span data-stu-id="23d8c-363">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="23d8c-364">`MapAreaRoute`Vytvoří trasu pomocí výchozí hodnotu a omezení pro `area` pomocí názvu oblasti zadaný v tomto případě `Blog`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-364">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="23d8c-365">Výchozí hodnota zajistí, že trasy vždy vytvoří `{ area = Blog, ... }`, omezení vyžaduje hodnotu `{ area = Blog, ... }` pro generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-365">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="23d8c-366">Konvenční směrování je závislá na pořadí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-366">Conventional routing is order-dependent.</span></span> <span data-ttu-id="23d8c-367">Obecně platí tras se oblasti musí být umístěny dříve v tabulce směrování, jako jsou podrobnější než cesty bez oblast.</span><span class="sxs-lookup"><span data-stu-id="23d8c-367">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="23d8c-368">Pomocí výše uvedeném příkladu, hodnoty trasy odpovídá následující akce:</span><span class="sxs-lookup"><span data-stu-id="23d8c-368">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="23d8c-369">`AreaAttribute` Je co označuje řadič v rámci oblasti, říkáme, zda je tento řadič v `Blog` oblasti.</span><span class="sxs-lookup"><span data-stu-id="23d8c-369">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="23d8c-370">Řadiče bez `[Area]` atribut nejsou členy žádné oblasti a bude **není** shodovat, kdy `area` hodnota trasy zajišťuje směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-370">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="23d8c-371">V následujícím příkladu může odpovídat pouze první řadič uvedené hodnoty trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-371">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="23d8c-372">Obor názvů každého řadiče se zde zobrazí pro úplnost – jinak by se řadiče mít názvy konfliktu a vygenerována chyba kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="23d8c-372">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="23d8c-373">Obory názvů třídy nemají vliv na směrování MVC.</span><span class="sxs-lookup"><span data-stu-id="23d8c-373">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="23d8c-374">První dva řadiče jsou členy oblasti a odpovídá pouze při jejich název příslušné oblasti zajišťuje `area` směrování hodnotu.</span><span class="sxs-lookup"><span data-stu-id="23d8c-374">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="23d8c-375">Třetí řadič není členem žádné oblasti a může pouze shody, pokud žádná hodnota pro `area` zajišťuje směrování.</span><span class="sxs-lookup"><span data-stu-id="23d8c-375">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-376">Z hlediska odpovídající *žádná hodnota*, chybí `area` hodnota je stejná jako hodnota `area` měla hodnotu null nebo prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="23d8c-376">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="23d8c-377">Při provádění akce uvnitř oblasti, hodnoty trasy pro `area` budou k dispozici jako *vedlejším hodnota* pro směrování sloužící ke generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-377">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="23d8c-378">To znamená, že ve výchozím nastavení oblasti fungovat *trvalé* pro generování adresy URL, jak je předvedeno podle následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="23d8c-378">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="23d8c-379">Principy IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="23d8c-379">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="23d8c-380">Tato část se přímý informace na interní informace o framework a způsobu, jakým MVC vybírá akci, kterou chcete provést.</span><span class="sxs-lookup"><span data-stu-id="23d8c-380">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="23d8c-381">Typická aplikace nebudete potřebovat vlastní`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="23d8c-381">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="23d8c-382">Pravděpodobně už jste použili `IActionConstraint` i v případě, že nejste obeznámení s rozhraním.</span><span class="sxs-lookup"><span data-stu-id="23d8c-382">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="23d8c-383">`[HttpGet]` Atribut a je to podobné `[Http-VERB]` atributy implementovat třídu `IActionConstraint` za účelem omezení provádění metody akce.</span><span class="sxs-lookup"><span data-stu-id="23d8c-383">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="23d8c-384">Za předpokladu, že výchozí konvenční trasu cesty URL `/Products/Edit` byste mohli vytvořit hodnoty `{ controller = Products, action = Edit }`, který odpovídá **i** z akcí, které jsou tady uvedené.</span><span class="sxs-lookup"><span data-stu-id="23d8c-384">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="23d8c-385">V `IActionConstraint` terminologie by říkáme, že obě tyto akce jsou považovány za kandidáty – obě shodný data trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-385">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="23d8c-386">Když `HttpGetAttribute` provede, se dozvíte, který *Edit()* odpovídá *získat* a není nalezena shoda pro jiné akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="23d8c-386">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="23d8c-387">`Edit(...)` Akce nemá žádné omezení a proto bude shodovat s všechny akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="23d8c-387">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="23d8c-388">Proto za předpokladu, že `POST` – pouze `Edit(...)` odpovídá.</span><span class="sxs-lookup"><span data-stu-id="23d8c-388">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="23d8c-389">Ale pro `GET` obě akce mohou stále odpovídat - však akce s `IActionConstraint` je považováno za *lepší* než akce bez.</span><span class="sxs-lookup"><span data-stu-id="23d8c-389">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="23d8c-390">Proto protože `Edit()` má `[HttpGet]` je považován za zvláštní a bude vybrána, pokud obě akce se může shodovat.</span><span class="sxs-lookup"><span data-stu-id="23d8c-390">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="23d8c-391">Koncepčně `IActionConstraint` je forma *přetížení*, ale místo přetížení metody se stejným názvem, je přetížení mezi akce, které odpovídají stejnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="23d8c-391">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="23d8c-392">Atribut směrování také používá `IActionConstraint` a může mít za následek akce z různých řadičů obou za kandidáty.</span><span class="sxs-lookup"><span data-stu-id="23d8c-392">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="23d8c-393">Implementace IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="23d8c-393">Implementing IActionConstraint</span></span>

<span data-ttu-id="23d8c-394">Nejjednodušší způsob, jak implementovat `IActionConstraint` je vytvoření třídy odvozené od `System.Attribute` a umístěte ji na vaše akce a kontrolery.</span><span class="sxs-lookup"><span data-stu-id="23d8c-394">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="23d8c-395">MVC bude automaticky zjistit některé `IActionConstraint` , se použijí jako atributy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-395">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="23d8c-396">Můžete použít model aplikace použít omezení a jedná se pravděpodobně nejvíce flexibilní přístup, protože ji umožňuje metaprogram jak uplatňují.</span><span class="sxs-lookup"><span data-stu-id="23d8c-396">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="23d8c-397">V následujícím příkladu omezení vybere akci založenou na *kód země* z dat trasy.</span><span class="sxs-lookup"><span data-stu-id="23d8c-397">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="23d8c-398">[Úplné ukázku na Githubu](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="23d8c-398">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="23d8c-399">Jste zodpovědní za implementaci `Accept` metoda a výběr 'Pořadí' pro omezení provést.</span><span class="sxs-lookup"><span data-stu-id="23d8c-399">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="23d8c-400">V takovém případě `Accept` metoda vrátí `true` k označení akce je nalezena shoda při `country` směrovat hodnota odpovídá.</span><span class="sxs-lookup"><span data-stu-id="23d8c-400">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="23d8c-401">To se liší od `RouteValueAttribute` , neboť umožňuje záložní s-s atributy akcí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-401">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="23d8c-402">Ukázka ukazuje, že pokud byste `en-US` akce pak kód země, jako je `fr-FR` přejde zpět do více obecné řadiče, který nemá `[CountrySpecific(...)]` použít.</span><span class="sxs-lookup"><span data-stu-id="23d8c-402">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="23d8c-403">`Order` Vlastnost rozhoduje, které *fáze* omezení je součástí.</span><span class="sxs-lookup"><span data-stu-id="23d8c-403">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="23d8c-404">Akce omezení spustit ve skupinách, na základě `Order`.</span><span class="sxs-lookup"><span data-stu-id="23d8c-404">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="23d8c-405">Například všechny rozhraní zadané atributy metody HTTP používají stejné `Order` hodnotu tak, aby spouštět ve fázi stejné.</span><span class="sxs-lookup"><span data-stu-id="23d8c-405">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="23d8c-406">Může mít libovolný počet fází, jako je nutné implementovat požadovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="23d8c-406">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="23d8c-407">Při rozhodování o hodnotu `Order` přemýšlení o tom, zda bude použito vaše omezení před metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="23d8c-407">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="23d8c-408">Nižší čísla se spouští jako první.</span><span class="sxs-lookup"><span data-stu-id="23d8c-408">Lower numbers run first.</span></span>
