---
title: Oblasti
author: rick-anderson
description: "Ukazuje, jak pracovat s oblastí."
keywords: "ASP.NET Core, oblasti, směrování, zobrazení"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: cd0302fa1668979df9bbd6cb36f82742d325c5e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="areas"></a><span data-ttu-id="f6872-104">Oblasti</span><span class="sxs-lookup"><span data-stu-id="f6872-104">Areas</span></span>

<span data-ttu-id="f6872-105">Podle [Dhananjay Kumar](https://twitter.com/debug_mode) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f6872-105">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f6872-106">Oblasti se o funkci ASP.NET MVC sloužící k organizování související funkce do skupiny jako samostatný obor názvů (pro směrování) a strukturu složek (pro zobrazení).</span><span class="sxs-lookup"><span data-stu-id="f6872-106">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="f6872-107">Použití oblastí vytvoří hierarchii pro účely směrování přidáním jiný parametr trasy, `area`do `controller` a `action`.</span><span class="sxs-lookup"><span data-stu-id="f6872-107">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="f6872-108">Oblasti poskytují způsob, jak oddílu velké ASP.NET Core MVC webové aplikace do menších funkční seskupení.</span><span class="sxs-lookup"><span data-stu-id="f6872-108">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="f6872-109">Oblast je efektivně strukturu MVC uvnitř aplikace.</span><span class="sxs-lookup"><span data-stu-id="f6872-109">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="f6872-110">V projektu MVC logické součásti jako Model, Kontroleru a zobrazení jsou uchovány v různých složkách a konvence pojmenování aplikace MVC používá k vytvoření vztahu mezi těmito součástmi.</span><span class="sxs-lookup"><span data-stu-id="f6872-110">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="f6872-111">Pro velké aplikace může být výhodné oddílu aplikace na samostatné vysoké úrovni oblasti funkcí.</span><span class="sxs-lookup"><span data-stu-id="f6872-111">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="f6872-112">Pro instanci elektronické obchodování aplikace s více organizačních jednotek, jako je například checkout, fakturace a vyhledávání atd. Každý z těchto jednotek mají své vlastní logickou součástí zobrazení, řadiče a modely.</span><span class="sxs-lookup"><span data-stu-id="f6872-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="f6872-113">V tomto scénáři můžete v oblasti fyzicky oddílu komponenty obchodní ve stejném projektu.</span><span class="sxs-lookup"><span data-stu-id="f6872-113">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="f6872-114">Oblast může být definován jako menší funkční jednotky v projektu ASP.NET MVC jádra s vlastní sadou řadiče, zobrazení a modely.</span><span class="sxs-lookup"><span data-stu-id="f6872-114">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="f6872-115">Zvažte použití oblastí v MVC při projektu:</span><span class="sxs-lookup"><span data-stu-id="f6872-115">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="f6872-116">Aplikace se provádí více vysoké úrovně funkčnosti komponent, které by měl být oddělený logicky</span><span class="sxs-lookup"><span data-stu-id="f6872-116">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="f6872-117">Chcete oddílu projektu MVC tak, aby každý funkční oblast bylo možné pracovat nezávisle</span><span class="sxs-lookup"><span data-stu-id="f6872-117">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="f6872-118">Oblast funkce:</span><span class="sxs-lookup"><span data-stu-id="f6872-118">Area features:</span></span>

* <span data-ttu-id="f6872-119">ASP.NET MVC základní aplikaci může mít libovolný počet oblastí</span><span class="sxs-lookup"><span data-stu-id="f6872-119">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="f6872-120">Každou oblast má svou vlastní řadiče, modely a zobrazení</span><span class="sxs-lookup"><span data-stu-id="f6872-120">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="f6872-121">Umožňuje uspořádat do několika komponent vysoké úrovně, které bylo možné pracovat nezávisle rozsáhlých projektů MVC</span><span class="sxs-lookup"><span data-stu-id="f6872-121">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="f6872-122">Podporuje více řadiče se stejným názvem - tak dlouho, dokud mají různé *oblastí*</span><span class="sxs-lookup"><span data-stu-id="f6872-122">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="f6872-123">Podívejme se na příklad znázorňující způsob vytváření a použít oblasti.</span><span class="sxs-lookup"><span data-stu-id="f6872-123">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="f6872-124">Řekněme, že máte aplikaci ze storu, která má dva odlišné seskupení kontrolery a zobrazení: produktů a služeb.</span><span class="sxs-lookup"><span data-stu-id="f6872-124">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="f6872-125">Složka obvykle struktury pro, že pomocí MVC oblasti vypadá níže:</span><span class="sxs-lookup"><span data-stu-id="f6872-125">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="f6872-126">název projektu</span><span class="sxs-lookup"><span data-stu-id="f6872-126">Project name</span></span>

  * <span data-ttu-id="f6872-127">Oblasti</span><span class="sxs-lookup"><span data-stu-id="f6872-127">Areas</span></span>

    * <span data-ttu-id="f6872-128">Produkty</span><span class="sxs-lookup"><span data-stu-id="f6872-128">Products</span></span>

      * <span data-ttu-id="f6872-129">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="f6872-129">Controllers</span></span>

        * <span data-ttu-id="f6872-130">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f6872-130">HomeController.cs</span></span>

        * <span data-ttu-id="f6872-131">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="f6872-131">ManageController.cs</span></span>

      * <span data-ttu-id="f6872-132">zobrazení</span><span class="sxs-lookup"><span data-stu-id="f6872-132">Views</span></span>

        * <span data-ttu-id="f6872-133">Domů</span><span class="sxs-lookup"><span data-stu-id="f6872-133">Home</span></span>

          * <span data-ttu-id="f6872-134">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f6872-134">Index.cshtml</span></span>

        * <span data-ttu-id="f6872-135">Správa</span><span class="sxs-lookup"><span data-stu-id="f6872-135">Manage</span></span>

          * <span data-ttu-id="f6872-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f6872-136">Index.cshtml</span></span>

    * <span data-ttu-id="f6872-137">Služby</span><span class="sxs-lookup"><span data-stu-id="f6872-137">Services</span></span>

      * <span data-ttu-id="f6872-138">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="f6872-138">Controllers</span></span>

        * <span data-ttu-id="f6872-139">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f6872-139">HomeController.cs</span></span>

      * <span data-ttu-id="f6872-140">zobrazení</span><span class="sxs-lookup"><span data-stu-id="f6872-140">Views</span></span>

        * <span data-ttu-id="f6872-141">Domů</span><span class="sxs-lookup"><span data-stu-id="f6872-141">Home</span></span>

          * <span data-ttu-id="f6872-142">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f6872-142">Index.cshtml</span></span>

<span data-ttu-id="f6872-143">Když se pokusí MVC pro vykreslení zobrazení v oblasti, ve výchozím nastavení, pokusí se hledat v následujících umístěních:</span><span class="sxs-lookup"><span data-stu-id="f6872-143">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="f6872-144">Jedná se o výchozí umístění, které je možné změnit prostřednictvím `AreaViewLocationFormats` na `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="f6872-144">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="f6872-145">Například v níže uvedeného kódu místo nutnosti název složky jako "Oblasti", byla změněna na "Kategorií".</span><span class="sxs-lookup"><span data-stu-id="f6872-145">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="f6872-146">Poznámka je, že strukturu *zobrazení* složka je jenom jeden, která je považována za důležité sem a obsah ostatních složek, jako jsou *řadiče* a *modely* nemá **není** vás.</span><span class="sxs-lookup"><span data-stu-id="f6872-146">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="f6872-147">Například nemusí mít *řadiče* a *modely* složku v všechny.</span><span class="sxs-lookup"><span data-stu-id="f6872-147">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="f6872-148">Toto funguje, protože obsah *řadiče* a *modely* je pouze kód, který získá zkompilovat do ve formátu .dll, kde je jako obsah *zobrazení* není dokud požadavek na který zobrazení byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="f6872-148">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="f6872-149">Jakmile hierarchii složek, které jste definovali, budete muset MVC říct, že každý řadič je přiřazený k oblasti.</span><span class="sxs-lookup"><span data-stu-id="f6872-149">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="f6872-150">Provedete to tak architekturu názvu řadiče s `[Area]` atribut.</span><span class="sxs-lookup"><span data-stu-id="f6872-150">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="f6872-151">Nastavte definici trasy, která funguje s vaší nově vytvořený oblasti.</span><span class="sxs-lookup"><span data-stu-id="f6872-151">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="f6872-152">[Směrování do akce Kontroleru](routing.md) článek obsahuje podrobnosti o tom, jak vytvořit definicí cesty, včetně použití konvenční trasy a trasy atributů.</span><span class="sxs-lookup"><span data-stu-id="f6872-152">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="f6872-153">V tomto příkladu použijeme konvenční trasy.</span><span class="sxs-lookup"><span data-stu-id="f6872-153">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="f6872-154">Chcete-li to provést, otevřete *Startup.cs* soubor a upravit přidáním `areaRoute` s názvem definice trasy níže.</span><span class="sxs-lookup"><span data-stu-id="f6872-154">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="f6872-155">Procházení k `http://<yourApp>/products`, `Index` metody akce `HomeController` v `Products` oblasti bude volána.</span><span class="sxs-lookup"><span data-stu-id="f6872-155">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="f6872-156">Generování odkazů</span><span class="sxs-lookup"><span data-stu-id="f6872-156">Link Generation</span></span>

* <span data-ttu-id="f6872-157">Generování odkazů z akce v rámci oblast na základě řadiče jiné akce v rámci stejného řadiče.</span><span class="sxs-lookup"><span data-stu-id="f6872-157">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="f6872-158">Řekněme, že je cesta aktuální žádosti jako`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="f6872-158">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f6872-159">HtmlHelper syntaxe:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="f6872-159">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="f6872-160">TagHelper syntaxe:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f6872-160">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="f6872-161">Všimněte si, že jsme nemusí zadat hodnoty 'oblastí' a 'controller' tady, protože je již k dispozici v kontextu aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="f6872-161">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="f6872-162">Tyto druhy hodnot, se nazývají `ambient` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6872-162">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="f6872-163">Generování odkazů z akce v rámci oblast na základě řadiče další akci na jiném řadiči</span><span class="sxs-lookup"><span data-stu-id="f6872-163">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="f6872-164">Řekněme, že je cesta aktuální žádosti jako`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="f6872-164">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f6872-165">HtmlHelper syntaxe:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="f6872-165">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="f6872-166">TagHelper syntaxe:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f6872-166">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="f6872-167">Všimněte si, tady je použita hodnota vedlejším oblasti, ale výše je výslovně zadána hodnota 'controller'.</span><span class="sxs-lookup"><span data-stu-id="f6872-167">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="f6872-168">Generování odkazů z akce v rámci oblasti řadiče další akci na základě různých řadiče a jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="f6872-168">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="f6872-169">Řekněme, že je cesta aktuální žádosti jako`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="f6872-169">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f6872-170">HtmlHelper syntaxe:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="f6872-170">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="f6872-171">TagHelper syntaxe:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f6872-171">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="f6872-172">Všimněte si, že tady jsou použity žádné vedlejším hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6872-172">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="f6872-173">Generování odkazů z akce v rámci řadič oblast na základě jinou akci na jiném řadiči a **není** v oblasti.</span><span class="sxs-lookup"><span data-stu-id="f6872-173">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="f6872-174">HtmlHelper syntaxe:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="f6872-174">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="f6872-175">TagHelper syntaxe:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f6872-175">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="f6872-176">Vzhledem k tomu, že chcete generovat odkazy na jiné oblasti na základě akce kontroleru, jsme prázdný vedlejším hodnota "plochu" sem.</span><span class="sxs-lookup"><span data-stu-id="f6872-176">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="f6872-177">Publikování oblastí</span><span class="sxs-lookup"><span data-stu-id="f6872-177">Publishing Areas</span></span>

<span data-ttu-id="f6872-178">Všechny `*.cshtml` a `wwwroot/**` soubory jsou publikovány do výstupního při `<Project Sdk="Microsoft.NET.Sdk.Web">` je součástí *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="f6872-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
