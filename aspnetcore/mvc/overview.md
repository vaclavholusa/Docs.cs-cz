---
title: "Přehled ASP.NET Core MVC"
author: ardalis
description: "Zjistěte, jak je bohaté rozhraní pro vytváření webových aplikací ASP.NET MVC jádra a rozhraní API pomocí Model-View-Controller návrh vzor."
manager: wpickett
ms.author: riande
ms.date: 01/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/overview
ms.openlocfilehash: 16fd1b5e71cde4364f02640f504d42218ed680df
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="ca4e9-103">Přehled ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ca4e9-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="ca4e9-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ca4e9-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ca4e9-105">Jádro ASP.NET MVC je bohaté rozhraní pro vytváření webových aplikací a rozhraní API pomocí Model-View-Controller návrh vzor.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="ca4e9-106">Co je vzor MVC?</span><span class="sxs-lookup"><span data-stu-id="ca4e9-106">What is the MVC pattern?</span></span>

<span data-ttu-id="ca4e9-107">Vzor architektury Model-View-Controller (MVC) rozděluje aplikace do tří hlavních skupin součástí: modely, zobrazení a Kontrolery.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="ca4e9-108">Tento vzor pomáhá zajistit [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="ca4e9-109">Pomocí tohoto vzoru, se směrují požadavky uživatelů na řadič, která zajišťuje pro práci s modelem provádět akce uživatele nebo načíst výsledky dotazů.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="ca4e9-110">Řadičem vybere zobrazení pro uživatele a poskytuje Model data, která vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="ca4e9-111">Následující diagram znázorňuje třemi hlavními komponentami a ty, které odkazují jiné:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Vzor MVC](overview/_static/mvc.png)

<span data-ttu-id="ca4e9-113">Tato vymezení odpovědnosti umožňuje škálovat aplikaci z hlediska složitost, protože je jednodušší code, ladit a testovat (model, zobrazení nebo řadič) něčeho, co má jediné úlohy (a odpovídá [jeden zásady odpovědnosti ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="ca4e9-114">Je obtížné aktualizace, testování a ladění kódu, který má závislosti rozloženy dva nebo víc z těchto tří oblastí.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="ca4e9-115">Například logiku uživatelského rozhraní se obvykle změnit častěji, než obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="ca4e9-116">Pokud kód a obchodní logiku prezentace jsou sloučeny do jednoho objektu, musíte změnit objekt obsahující obchodní logiku pokaždé, když se změní uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="ca4e9-117">To často představuje chyby a vyžaduje opakované zkoušky obchodní logiky po každé změně minimální uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="ca4e9-118">Zobrazení a kontroler závisí na modelu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="ca4e9-119">Model však závisí na zobrazení ani kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="ca4e9-120">To je jedno z klíčových výhod oddělení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="ca4e9-121">Toto rozdělení umožňuje modelu, který má být vytvořeny a testovány nezávislé vizuální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="ca4e9-122">Model odpovědnosti</span><span class="sxs-lookup"><span data-stu-id="ca4e9-122">Model Responsibilities</span></span>

<span data-ttu-id="ca4e9-123">Modelu v aplikaci MVC představuje stav aplikace a veškeré obchodní logiky nebo operace, které by měli provádět ho.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="ca4e9-124">Obchodní logika by měl zapouzdřený v modelu, společně s žádné implementační logika pro uchování stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="ca4e9-125">Zobrazení silného typu obvykle používat typy ViewModel navržený tak, aby obsahují data pro zobrazení v tomto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="ca4e9-126">Správce vytvoří a naplní tyto instance ViewModel z modelu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="ca4e9-127">K uspořádání modelu v aplikaci, která používá architekturní vzor MVC mnoha způsoby.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="ca4e9-128">Další informace o některých [různé druhy modelu typy](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="ca4e9-129">Zobrazit odpovědnosti</span><span class="sxs-lookup"><span data-stu-id="ca4e9-129">View Responsibilities</span></span>

<span data-ttu-id="ca4e9-130">Zobrazení jsou zodpovědní za prezentací obsahu v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="ca4e9-131">Používají [zobrazovací modul Razor](#razor-view-engine) pro vložení kódu .NET do kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="ca4e9-132">Musí být minimální logiku v zobrazeních a veškeré logiky v nich by se měly týkat prezentací obsah.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="ca4e9-133">Pokud zjistíte, třeba provádět značnou část logiky v zobrazení souborů, aby bylo možné zobrazit data z komplexní model, zvažte použití [zobrazení součást](views/view-components.md), ViewModel, nebo zobrazit šablonu pro zjednodušení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="ca4e9-134">Odpovědnosti řadiče</span><span class="sxs-lookup"><span data-stu-id="ca4e9-134">Controller Responsibilities</span></span>

<span data-ttu-id="ca4e9-135">Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracují s modelem a konečně vybírají vykreslené zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="ca4e9-136">V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="ca4e9-137">Ve vzoru MVC řadičem je počáteční vstupní bod a je odpovědná za výběr modelu typy pro práci s a které zobrazení k vykreslení (proto jeho název – se určuje, jak aplikaci reagovat na daný požadavek).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="ca4e9-138">Řadiče by neměl být ztěžuje příliš moc odpovědnosti.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-138">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="ca4e9-139">Pokud chcete zachovat řadiče logiku stal příliš složité, použijte [jeden zásady odpovědnosti](http://deviq.com/single-responsibility-principle/) na obchodní logiku nabízené z kontroleru a do modelu domény.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="ca4e9-140">Pokud zjistíte, že vaše akce kontroleru často provádějí stejné typy akcí, můžete podle [nemusíte sami opakujte Princip](http://deviq.com/don-t-repeat-yourself/) přesunutím těchto běžné akce do [filtry](#filters).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="ca4e9-141">Co je technologie ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="ca4e9-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="ca4e9-142">Rozhraní MVC ASP.NET Core je zdroj lightweight, otevřete intenzivního prezentační architektura optimalizovaný pro použití s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="ca4e9-143">Jádro ASP.NET MVC poskytuje na základě vzory způsob, jak vytvářet dynamické weby, která umožňuje čistou oddělené oblasti zájmu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="ca4e9-144">Poskytuje úplnou kontrolu nad značek, podporuje vývoj TDD popisný a využívá nejnovější webové standardy.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="ca4e9-145">Funkce</span><span class="sxs-lookup"><span data-stu-id="ca4e9-145">Features</span></span>

<span data-ttu-id="ca4e9-146">Jádro ASP.NET MVC zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="ca4e9-147">Směrování</span><span class="sxs-lookup"><span data-stu-id="ca4e9-147">Routing</span></span>](#routing)
* [<span data-ttu-id="ca4e9-148">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="ca4e9-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="ca4e9-149">Ověření modelu</span><span class="sxs-lookup"><span data-stu-id="ca4e9-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="ca4e9-150">Vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="ca4e9-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="ca4e9-151">Filtry</span><span class="sxs-lookup"><span data-stu-id="ca4e9-151">Filters</span></span>](#filters)
* [<span data-ttu-id="ca4e9-152">Oblasti</span><span class="sxs-lookup"><span data-stu-id="ca4e9-152">Areas</span></span>](#areas)
* [<span data-ttu-id="ca4e9-153">Webová rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ca4e9-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="ca4e9-154">Možnosti testování</span><span class="sxs-lookup"><span data-stu-id="ca4e9-154">Testability</span></span>](#testability)
* [<span data-ttu-id="ca4e9-155">Zobrazovací modul Razor</span><span class="sxs-lookup"><span data-stu-id="ca4e9-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="ca4e9-156">Zobrazení se silnými typy</span><span class="sxs-lookup"><span data-stu-id="ca4e9-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="ca4e9-157">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="ca4e9-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="ca4e9-158">Zobrazení součásti</span><span class="sxs-lookup"><span data-stu-id="ca4e9-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="ca4e9-159">Směrování</span><span class="sxs-lookup"><span data-stu-id="ca4e9-159">Routing</span></span>

<span data-ttu-id="ca4e9-160">ASP.NET MVC základní je postavený na [směrování ASP.NET Core](../fundamentals/routing.md), výkonné komponenta mapování adres URL, která umožňuje vytvářet aplikace, které mají srozumitelné a dohledatelné adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="ca4e9-161">To umožňuje definovat vaší aplikace vzory mapování adres URL které fungují dobře u optimalizaci pro vyhledávací weby (SEO) a pro generování odkazů, bez ohledu na tom, jak jsou uspořádány soubory na vašem webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="ca4e9-162">Můžete definovat pomocí syntaxi šablony pohodlný trasy, která podporuje hodnotu omezení trasy, výchozích hodnot a volitelné hodnoty trasy.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="ca4e9-163">*Založené na konvenci směrování* umožňuje globálně určit adresu URL formáty, které vaše aplikace přijímá a jak každou z těchto formátů mapuje metodu konkrétní akce na zadaný kontroler.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="ca4e9-164">Po přijetí příchozího požadavku modulu Směrování analyzuje adresu URL a odpovídá jednomu z definovaných formátech adres URL a pak zavolá metodu akce přidruženého kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ca4e9-165">*Atribut směrování* umožňuje zadat informace o směrování podle architekturu řadiče a akce s atributy, které definují trasy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="ca4e9-166">To znamená, že budou vedle kontroleru a akce, ke kterému jsou přidružené umístěny definic trasy.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="ca4e9-167">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="ca4e9-167">Model binding</span></span>

<span data-ttu-id="ca4e9-168">Jádro ASP.NET MVC [model vazby](models/model-binding.md) převede na objekty, které může zpracovat řadičem dat žádostí klienta (hodnot formuláře, data trasy, parametrů řetězce dotazu, hlaviček protokolu HTTP).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="ca4e9-169">V důsledku toho řadič logiky nemá udělat práci při zjištění data na příchozí žádost; jednoduše má data jako parametry pro její metody akce.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="ca4e9-170">ověření modelu</span><span class="sxs-lookup"><span data-stu-id="ca4e9-170">Model validation</span></span>

<span data-ttu-id="ca4e9-171">ASP.NET MVC základní podporuje [ověření](models/validation.md) podle architekturu modelu objektu s atributy ověření dat poznámky.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="ca4e9-172">Atributy ověření se kontroluje na straně klienta, než hodnoty jsou odeslány na server, jakož i na serveru před akce kontroleru je volána.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="ca4e9-173">Akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-173">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="ca4e9-174">Rozhraní framework zpracovává ověřování data požadavku na klientovi i na serveru.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="ca4e9-175">Ověření logiku určenou na typech modelu, přidá se do vykreslené zobrazení jako nerušivý poznámky a je požadováno v prohlížeči s [jQuery ověření](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="ca4e9-176">Vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="ca4e9-176">Dependency injection</span></span>

<span data-ttu-id="ca4e9-177">Má integrovanou podporu pro ASP.NET Core [vkládání závislostí (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="ca4e9-178">V aplikaci ASP.NET MVC jádra [řadiče](controllers/dependency-injection.md) můžete žádost o potřebných službám pomocí jejich konstruktory, což jim umožní postupujte podle [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="ca4e9-179">Můžete také použít aplikaci [vkládání závislostí v zobrazení souborů](views/dependency-injection.md)pomocí `@inject` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="ca4e9-180">Filtry</span><span class="sxs-lookup"><span data-stu-id="ca4e9-180">Filters</span></span>

<span data-ttu-id="ca4e9-181">[Filtry](controllers/filters.md) pomoci vývojářům zapouzdření mezi vyjímání obavy, jako je zpracování výjimek nebo autorizace.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="ca4e9-182">Filtry povolit spuštění vlastní před a po zpracování logiky pro metody akce a může být nakonfigurována pro spuštění v určitých bodech, v rámci spouštěcí kanál pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="ca4e9-183">Filtry lze použít k řadiče nebo akce jako atributy (nebo můžete spustit globálně).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="ca4e9-184">Několik filtry (například `Authorize`) jsou zahrnuty v rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="ca4e9-185">Oblasti</span><span class="sxs-lookup"><span data-stu-id="ca4e9-185">Areas</span></span>

<span data-ttu-id="ca4e9-186">[Oblasti](controllers/areas.md) poskytnout způsob, jak oddílu velké ASP.NET Core MVC webové aplikace do menších funkční seskupení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="ca4e9-187">Oblast je strukturu MVC uvnitř aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="ca4e9-188">V projektu MVC logické součásti jako Model, Kontroleru a zobrazení jsou uchovány v různých složkách a konvence pojmenování aplikace MVC používá k vytvoření vztahu mezi těmito součástmi.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="ca4e9-189">Pro velké aplikace může být výhodné oddílu aplikace na samostatné vysoké úrovni oblasti funkcí.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="ca4e9-190">Pro instanci elektronické obchodování aplikace s více organizačních jednotek, jako je například checkout, fakturace a vyhledávání atd. Každý z těchto jednotek mají své vlastní logickou součástí zobrazení, řadiče a modely.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="ca4e9-191">Webová rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ca4e9-191">Web APIs</span></span>

<span data-ttu-id="ca4e9-192">Kromě toho, představuje vynikající platformu pro tvorbu webů, má technologie ASP.NET MVC základní podpory pro vytváření webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="ca4e9-193">Můžete vytvářet služby, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="ca4e9-194">Rozhraní framework zahrnuje podporu pro vyjednávání obsahu HTTP s integrovanou podporu pro [formátování dat](models/formatting.md) jako XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="ca4e9-195">Zápis [vlastní formátování](advanced/custom-formatters.md) přidání podpory pro vlastní formáty.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="ca4e9-196">Povolení podpory pro hypermédií pomocí generování odkazů.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="ca4e9-197">Snadno povolit podporu pro [(CORS) pro sdílení prostředků různého původu](http://www.w3.org/TR/cors/) tak, aby webová rozhraní API můžete sdílet mezi několika webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="ca4e9-198">Možnosti testování</span><span class="sxs-lookup"><span data-stu-id="ca4e9-198">Testability</span></span>

<span data-ttu-id="ca4e9-199">Použití rozhraní framework rozhraní a vkládání závislostí proveďte ho vhodným testování částí a funkcí (např. TestHost a InMemory zprostředkovatele Entity Framework), které zahrnuje rozhraní [testování integrace](../testing/integration-testing.md) rychlé a snadno také.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="ca4e9-200">Další informace o [testování řadiče logiku](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="ca4e9-201">Zobrazovací modul Razor</span><span class="sxs-lookup"><span data-stu-id="ca4e9-201">Razor view engine</span></span>

<span data-ttu-id="ca4e9-202">[Zobrazení ASP.NET MVC základní](views/overview.md) použít [zobrazovací modul Razor](views/razor.md) k vykreslení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="ca4e9-203">Syntaxe Razor je compact, výrazovou a plynulá práce šablony značek jazyk pro definování zobrazení pomocí vloženého kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="ca4e9-204">Syntaxe Razor je používaný k dynamickému generování webového obsahu na serveru.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="ca4e9-205">Ještě jednou je možné kombinovat kódu serveru s obsah na straně klienta a kódu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="ca4e9-206">Pomocí zobrazovací modul Razor můžete definovat [rozložení](views/layout.md), [částečná zobrazení](views/partial.md) a replaceable oddílů.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="ca4e9-207">Zobrazení se silnými typy</span><span class="sxs-lookup"><span data-stu-id="ca4e9-207">Strongly typed views</span></span>

<span data-ttu-id="ca4e9-208">Zobrazení MVC Razor můžete být silného typu podle modelu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="ca4e9-209">Řadiče můžete předat povolení zobrazení tak, aby měl kontrolu typu a podporu technologie IntelliSense zobrazení silného typu modelu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="ca4e9-210">Například následující zobrazení vykreslí modelu typu `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="ca4e9-211">Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="ca4e9-211">Tag Helpers</span></span>

<span data-ttu-id="ca4e9-212">[Značka pomocné rutiny](views/tag-helpers/intro.md) povolit kód straně serveru k účasti ve vytváření a vykreslení elementů HTML v souborech Razor.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="ca4e9-213">Značka pomocné rutiny můžete použít k definování vlastní značky (například `<environment>`) nebo chcete změnit chování stávající značky (například `<label>`).</span><span class="sxs-lookup"><span data-stu-id="ca4e9-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="ca4e9-214">Pomocníci značka vytvořit vazbu na konkrétní elementy na základě názvu elementu a jeho atributy.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="ca4e9-215">Poskytují výhod vykreslování na straně serveru při zachování stále HTML prostředí pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="ca4e9-216">Existuje mnoho předdefinovaných značky pomocníci pro běžné úlohy -, jako je například vytváření formulářů, odkazy, načítání prostředků a další - a i další dostupné ve veřejných úložišť GitHub a jako NuGet balíčky.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="ca4e9-217">Pomocníci značky vytvořené v C# a jejich cílových elementů HTML na základě název elementu, název atributu nebo nadřazené značky.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="ca4e9-218">Například předdefinovaná LinkTagHelper lze vytvořit odkaz `Login` akce `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="ca4e9-219">`EnvironmentTagHelper` Slouží k zobrazení (například nezpracovanou nebo minifikovaný) založených na prostředí runtime, jako je například vývoj, pracovním nebo produkčním zahrnout jiné skripty:</span><span class="sxs-lookup"><span data-stu-id="ca4e9-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="ca4e9-220">Pomocníci značky poskytují HTML-friendly vývojového prostředí a bohaté prostředí technologie IntelliSense pro vytvoření značky HTML a syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="ca4e9-221">Většina předdefinované značky Pomocníci cíli stávající elementy HTML a poskytuje serverové atributy elementu.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="ca4e9-222">Zobrazení součásti</span><span class="sxs-lookup"><span data-stu-id="ca4e9-222">View Components</span></span>

<span data-ttu-id="ca4e9-223">[Zobrazit součásti](views/view-components.md) umožňují balíček logiku vykreslování a opakovaně ji používat v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="ca4e9-224">Jsou podobná [částečná zobrazení](views/partial.md), ale přidružené logiky.</span><span class="sxs-lookup"><span data-stu-id="ca4e9-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
