---
title: Oblasti
author: rick-anderson
description: "Ukazuje, jak pracovat s oblastí."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 1ade49de3f6c58edc4ea7b06bc593b3db797081c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="areas"></a><span data-ttu-id="bd123-103">Oblasti</span><span class="sxs-lookup"><span data-stu-id="bd123-103">Areas</span></span>

<span data-ttu-id="bd123-104">Podle [Dhananjay Kumar](https://twitter.com/debug_mode) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bd123-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bd123-105">Oblasti se o funkci ASP.NET MVC sloužící k organizování související funkce do skupiny jako samostatný obor názvů (pro směrování) a strukturu složek (pro zobrazení).</span><span class="sxs-lookup"><span data-stu-id="bd123-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="bd123-106">Použití oblastí vytvoří hierarchii pro účely směrování přidáním jiný parametr trasy, `area`do `controller` a `action`.</span><span class="sxs-lookup"><span data-stu-id="bd123-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="bd123-107">Oblasti poskytují způsob, jak oddílu velké ASP.NET Core MVC webové aplikace do menších funkční seskupení.</span><span class="sxs-lookup"><span data-stu-id="bd123-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="bd123-108">Oblast je efektivně strukturu MVC uvnitř aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd123-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="bd123-109">V projektu MVC logické součásti jako Model, Kontroleru a zobrazení jsou uchovány v různých složkách a konvence pojmenování aplikace MVC používá k vytvoření vztahu mezi těmito součástmi.</span><span class="sxs-lookup"><span data-stu-id="bd123-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="bd123-110">Pro velké aplikace může být výhodné oddílu aplikace na samostatné vysoké úrovni oblasti funkcí.</span><span class="sxs-lookup"><span data-stu-id="bd123-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="bd123-111">Pro instanci elektronické obchodování aplikace s více organizačních jednotek, jako je například checkout, fakturace a vyhledávání atd. Každý z těchto jednotek mají své vlastní logickou součástí zobrazení, řadiče a modely.</span><span class="sxs-lookup"><span data-stu-id="bd123-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="bd123-112">V tomto scénáři můžete v oblasti fyzicky oddílu komponenty obchodní ve stejném projektu.</span><span class="sxs-lookup"><span data-stu-id="bd123-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="bd123-113">Oblast může být definován jako menší funkční jednotky v projektu ASP.NET MVC jádra s vlastní sadou řadiče, zobrazení a modely.</span><span class="sxs-lookup"><span data-stu-id="bd123-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="bd123-114">Zvažte použití oblastí v MVC při projektu:</span><span class="sxs-lookup"><span data-stu-id="bd123-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="bd123-115">Aplikace se provádí více vysoké úrovně funkčnosti komponent, které by měl být oddělený logicky</span><span class="sxs-lookup"><span data-stu-id="bd123-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="bd123-116">Chcete oddílu projektu MVC tak, aby každý funkční oblast bylo možné pracovat nezávisle</span><span class="sxs-lookup"><span data-stu-id="bd123-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="bd123-117">Oblast funkce:</span><span class="sxs-lookup"><span data-stu-id="bd123-117">Area features:</span></span>

* <span data-ttu-id="bd123-118">ASP.NET MVC základní aplikaci může mít libovolný počet oblastí</span><span class="sxs-lookup"><span data-stu-id="bd123-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="bd123-119">Každou oblast má svou vlastní řadiče, modely a zobrazení</span><span class="sxs-lookup"><span data-stu-id="bd123-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="bd123-120">Umožňuje uspořádat do několika komponent vysoké úrovně, které bylo možné pracovat nezávisle rozsáhlých projektů MVC</span><span class="sxs-lookup"><span data-stu-id="bd123-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="bd123-121">Podporuje více řadiče se stejným názvem - tak dlouho, dokud mají různé *oblastí*</span><span class="sxs-lookup"><span data-stu-id="bd123-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="bd123-122">Podívejme se na příklad znázorňující způsob vytváření a použít oblasti.</span><span class="sxs-lookup"><span data-stu-id="bd123-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="bd123-123">Řekněme, že máte aplikaci ze storu, která má dva odlišné seskupení kontrolery a zobrazení: produktů a služeb.</span><span class="sxs-lookup"><span data-stu-id="bd123-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="bd123-124">Složka obvykle struktury pro, že pomocí MVC oblasti vypadá níže:</span><span class="sxs-lookup"><span data-stu-id="bd123-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="bd123-125">název projektu</span><span class="sxs-lookup"><span data-stu-id="bd123-125">Project name</span></span>

  * <span data-ttu-id="bd123-126">Oblasti</span><span class="sxs-lookup"><span data-stu-id="bd123-126">Areas</span></span>

    * <span data-ttu-id="bd123-127">Produkty</span><span class="sxs-lookup"><span data-stu-id="bd123-127">Products</span></span>

      * <span data-ttu-id="bd123-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="bd123-128">Controllers</span></span>

        * <span data-ttu-id="bd123-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="bd123-129">HomeController.cs</span></span>

        * <span data-ttu-id="bd123-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="bd123-130">ManageController.cs</span></span>

      * <span data-ttu-id="bd123-131">zobrazení</span><span class="sxs-lookup"><span data-stu-id="bd123-131">Views</span></span>

        * <span data-ttu-id="bd123-132">Domů</span><span class="sxs-lookup"><span data-stu-id="bd123-132">Home</span></span>

          * <span data-ttu-id="bd123-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bd123-133">Index.cshtml</span></span>

        * <span data-ttu-id="bd123-134">Správa</span><span class="sxs-lookup"><span data-stu-id="bd123-134">Manage</span></span>

          * <span data-ttu-id="bd123-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bd123-135">Index.cshtml</span></span>

    * <span data-ttu-id="bd123-136">Služby</span><span class="sxs-lookup"><span data-stu-id="bd123-136">Services</span></span>

      * <span data-ttu-id="bd123-137">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="bd123-137">Controllers</span></span>

        * <span data-ttu-id="bd123-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="bd123-138">HomeController.cs</span></span>

      * <span data-ttu-id="bd123-139">zobrazení</span><span class="sxs-lookup"><span data-stu-id="bd123-139">Views</span></span>

        * <span data-ttu-id="bd123-140">Domů</span><span class="sxs-lookup"><span data-stu-id="bd123-140">Home</span></span>

          * <span data-ttu-id="bd123-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bd123-141">Index.cshtml</span></span>

<span data-ttu-id="bd123-142">Když se pokusí MVC pro vykreslení zobrazení v oblasti, ve výchozím nastavení, pokusí se hledat v následujících umístěních:</span><span class="sxs-lookup"><span data-stu-id="bd123-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="bd123-143">Jedná se o výchozí umístění, které je možné změnit prostřednictvím `AreaViewLocationFormats` na `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="bd123-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="bd123-144">Například v níže uvedeného kódu místo nutnosti název složky jako "Oblasti", byla změněna na "Kategorií".</span><span class="sxs-lookup"><span data-stu-id="bd123-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="bd123-145">Poznámka je, že strukturu *zobrazení* složka je jenom jeden, která je považována za důležité sem a obsah ostatních složek, jako jsou *řadiče* a *modely* nemá **není** vás.</span><span class="sxs-lookup"><span data-stu-id="bd123-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="bd123-146">Například nemusí mít *řadiče* a *modely* složku v všechny.</span><span class="sxs-lookup"><span data-stu-id="bd123-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="bd123-147">Toto funguje, protože obsah *řadiče* a *modely* je pouze kód, který získá zkompilovat do ve formátu .dll, kde je jako obsah *zobrazení* není dokud požadavek na který zobrazení byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="bd123-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="bd123-148">Jakmile hierarchii složek, které jste definovali, budete muset MVC říct, že každý řadič je přiřazený k oblasti.</span><span class="sxs-lookup"><span data-stu-id="bd123-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="bd123-149">Provedete to tak architekturu názvu řadiče s `[Area]` atribut.</span><span class="sxs-lookup"><span data-stu-id="bd123-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="bd123-150">Nastavte definici trasy, která funguje s vaší nově vytvořený oblasti.</span><span class="sxs-lookup"><span data-stu-id="bd123-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="bd123-151">[Směrování do akce Kontroleru](routing.md) článek obsahuje podrobnosti o tom, jak vytvořit definicí cesty, včetně použití konvenční trasy a trasy atributů.</span><span class="sxs-lookup"><span data-stu-id="bd123-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="bd123-152">V tomto příkladu použijeme konvenční trasy.</span><span class="sxs-lookup"><span data-stu-id="bd123-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="bd123-153">Chcete-li to provést, otevřete *Startup.cs* soubor a upravit přidáním `areaRoute` s názvem definice trasy níže.</span><span class="sxs-lookup"><span data-stu-id="bd123-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="bd123-154">Procházení k `http://<yourApp>/products`, `Index` metody akce `HomeController` v `Products` oblasti bude volána.</span><span class="sxs-lookup"><span data-stu-id="bd123-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="bd123-155">Generování odkazů</span><span class="sxs-lookup"><span data-stu-id="bd123-155">Link Generation</span></span>

* <span data-ttu-id="bd123-156">Generování odkazů z akce v rámci oblast na základě řadiče jiné akce v rámci stejného řadiče.</span><span class="sxs-lookup"><span data-stu-id="bd123-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="bd123-157">Řekněme, že je cesta aktuální žádosti jako`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="bd123-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="bd123-158">HtmlHelper syntaxe:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="bd123-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="bd123-159">TagHelper syntaxe:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="bd123-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="bd123-160">Všimněte si, že jsme nemusí zadat hodnoty 'oblastí' a 'controller' tady jsou již k dispozici v kontextu aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd123-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="bd123-161">Tyto druhy hodnot, se nazývají `ambient` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bd123-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="bd123-162">Generování odkazů z akce v rámci oblast na základě řadiče další akci na jiném řadiči</span><span class="sxs-lookup"><span data-stu-id="bd123-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="bd123-163">Řekněme, že je cesta aktuální žádosti jako`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="bd123-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="bd123-164">HtmlHelper syntaxe:`@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="bd123-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="bd123-165">TagHelper syntaxe:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="bd123-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="bd123-166">Všimněte si, tady je použita hodnota vedlejším oblasti, ale výše je výslovně zadána hodnota 'controller'.</span><span class="sxs-lookup"><span data-stu-id="bd123-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="bd123-167">Generování odkazů z akce v rámci oblasti řadiče další akci na základě různých řadiče a jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="bd123-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="bd123-168">Řekněme, že je cesta aktuální žádosti jako`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="bd123-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="bd123-169">HtmlHelper syntaxe:`@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="bd123-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="bd123-170">TagHelper syntaxe:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="bd123-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="bd123-171">Všimněte si, že tady jsou použity žádné vedlejším hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bd123-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="bd123-172">Generování odkazů z akce v rámci řadič oblast na základě jinou akci na jiném řadiči a **není** v oblasti.</span><span class="sxs-lookup"><span data-stu-id="bd123-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="bd123-173">HtmlHelper syntaxe:`@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="bd123-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="bd123-174">TagHelper syntaxe:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="bd123-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="bd123-175">Vzhledem k tomu, že chcete generovat odkazy na jiné oblasti na základě akce kontroleru, jsme prázdný vedlejším hodnota "plochu" sem.</span><span class="sxs-lookup"><span data-stu-id="bd123-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="bd123-176">Publikování oblastí</span><span class="sxs-lookup"><span data-stu-id="bd123-176">Publishing Areas</span></span>

<span data-ttu-id="bd123-177">Všechny `*.cshtml` a `wwwroot/**` soubory jsou publikovány do výstupního při `<Project Sdk="Microsoft.NET.Sdk.Web">` je součástí *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="bd123-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
