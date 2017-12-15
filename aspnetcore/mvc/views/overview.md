---
title: "Zobrazení v jádro ASP.NET MVC"
author: ardalis
description: "Zjistěte, jak pracovat zobrazení prezentace dat aplikace a interakce uživatelů v aplikaci ASP.NET MVC jádra."
keywords: "ASP.NET Core zobrazení MVC razor, viewmodel, viewdata, viewbag"
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 2562d4e5fb85159e6ccb47990f54448ddc188077
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="f9ea6-104">Zobrazení v jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f9ea6-104">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="f9ea6-105">Podle [Steve Smith](https://ardalis.com/) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f9ea6-105">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f9ea6-106">Tento dokument popisuje zobrazení použitých v aplikacích ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-106">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="f9ea6-107">Informace na stránkách Razor najdete v tématu [Úvod do stránky Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-107">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="f9ea6-108">V **M**odelu -**V**obrazit -**C**vzor ontroller (MVC), *zobrazení* zpracovává data aplikace prezentace a uživatelské interakce.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-108">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="f9ea6-109">Zobrazení je šablony HTML s vložených [kódu Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-109">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="f9ea6-110">Značka Razor je kód, který komunikuje s kódu HTML, který vytvoří webovou stránku, která je odeslána do klienta.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-110">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="f9ea6-111">V aplikaci ASP.NET MVC jádra, jsou zobrazení *.cshtml* soubory, které používají [programovací jazyk C#](/dotnet/csharp/) v kódu Razor.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-111">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="f9ea6-112">Obvykle jsou seskupené zobrazit soubory do složky s názvem pro jednotlivé aplikace [řadiče](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-112">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="f9ea6-113">Složky jsou uložené v *zobrazení* složku v kořenovém adresáři aplikace:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-113">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Zobrazení složky v řešení pro Průzkumníka Visual Studio je otevřený s otevřený a zobrazit soubory About.cshtml, Contact.cshtml a Index.cshtml Domovská složka](overview/_static/views_solution_explorer.png)

<span data-ttu-id="f9ea6-115">*Domů* je reprezentována řadiče *Domů* složky uvnitř *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-115">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="f9ea6-116">*Domů* složka obsahuje zobrazení pro *o*, *kontaktujte*, a *Index* webové stránky (domovské stránce).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-116">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="f9ea6-117">Když uživatel požádá o, jednu z těchto tří webových stránkách akce kontroleru v *Domů* řadič určit, který tři zobrazení, které se používá k sestavení a vrátí webové stránky pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-117">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="f9ea6-118">Použití [rozložení](xref:mvc/views/layout) k poskytování části konzistentní webové stránky a snížit opakování kódu.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-118">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="f9ea6-119">Rozložení často obsahují záhlaví, navigace a nabídky prvky a zápatí.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-119">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="f9ea6-120">Záhlaví a zápatí obvykle obsahuje často používaný kód pro mnoho elementy metadata a odkazy na prostředky skriptu a stylu.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-120">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="f9ea6-121">Rozložení umožňuje vyhnout se tento často používaný kód v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-121">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="f9ea6-122">[Částečná zobrazení](xref:mvc/views/partial) snížit zdvojení kódu pomocí správy opakovaně použitelné součásti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-122">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="f9ea6-123">Například je užitečné pro biografii Autor na web blogu, který se zobrazí v několik zobrazení částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-123">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="f9ea6-124">Vytváření biografii je obyčejnou zobrazení obsahu a nevyžaduje kód provést, aby bylo možné vytvořit obsahu pro webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-124">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="f9ea6-125">Autor biografii obsah není k dispozici na zobrazení ve samostatně, vazby modelu, takže použití částečné zobrazení pro tento typ obsahu je ideální.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-125">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="f9ea6-126">[Zobrazit součásti](xref:mvc/views/view-components) jsou podobná částečné zobrazení, protože umožňují snížit opakovaných kód, ale jsou vhodné pro zobrazení obsahu, který vyžaduje kód pro spuštění na serveru k vykreslení webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-126">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="f9ea6-127">Zobrazení součásti jsou užitečné při vykreslené obsah vyžaduje interakci databáze, například pro web nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-127">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="f9ea6-128">Zobrazení součásti nejsou omezena na modelu vazby k vytvoření výstupu webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-128">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="f9ea6-129">Výhody použití zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-129">Benefits of using views</span></span>

<span data-ttu-id="f9ea6-130">Zobrazení nápovědy k vytvoření [ **S**eparation **o**f **C**oncerns (SoC) návrhu](http://deviq.com/separation-of-concerns/) v aplikaci MVC oddělením kód uživatelské rozhraní z dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-130">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="f9ea6-131">Následující SoC návrhu, bude vaše aplikace modulární, který nabízí několik výhod:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-131">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="f9ea6-132">Tato aplikace je snazší správa, protože je lépe uspořádány.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-132">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="f9ea6-133">Zobrazení jsou obecně seskupené podle funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-133">Views are generally grouped by app feature.</span></span> <span data-ttu-id="f9ea6-134">Díky tomu je snazší najít související zobrazení při práci na funkce.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-134">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="f9ea6-135">Součástí aplikace jsou volně vázány.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-135">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="f9ea6-136">Můžete vytvářet a aktualizovat zobrazení aplikace odděleně od komponenty obchodní logiku a data access.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-136">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="f9ea6-137">Zobrazení aplikace můžete upravit aniž byste museli mít aktualizovat dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-137">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="f9ea6-138">Aby bylo jednodušší testování části uživatelského rozhraní aplikace, protože zobrazení jsou samostatné jednotky.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-138">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="f9ea6-139">Kvůli lepší organizaci je méně pravděpodobné, že budete ať už náhodně opakování části uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-139">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="f9ea6-140">Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-140">Creating a view</span></span>

<span data-ttu-id="f9ea6-141">Zobrazení, které jsou specifické pro určitý kontroler jsou vytvořené v *zobrazení / [ControllerName]* složky.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-141">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="f9ea6-142">Zobrazení, které jsou sdíleny mezi řadiče jsou umístěny v *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-142">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="f9ea6-143">Pokud chcete vytvořit zobrazení, přidejte nový soubor a poskytněte stejný název jako jeho přidruženému kontroleru akce s *.cshtml* příponu souboru.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-143">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="f9ea6-144">Vytvoření zobrazení, která odpovídá *o* akce v *Domů* řadič, vytvořit *About.cshtml* souboru v *zobrazení Domů*složky:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-144">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="f9ea6-145">*Syntaxe Razor* značek začíná `@` symbol.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-145">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="f9ea6-146">Spusťte příkazy jazyka C# tím, že umístíte C# kódu v rámci [bloky kódu Razor](xref:mvc/views/razor#razor-code-blocks) nastavit ve složených závorkách (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-146">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="f9ea6-147">Například najdete v sekci přiřazení "O" `ViewData["Title"]` uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-147">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="f9ea6-148">Můžete zobrazit hodnoty v kódu HTML hodnotu s odkazem jednoduše `@` symbol.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-148">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="f9ea6-149">Zobrazit obsah `<h2>` a `<h3>` prvků uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-149">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="f9ea6-150">Zobrazení obsahu uvedené výše je jenom část celé webové stránky, které je vykresleno do uživatele.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-150">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="f9ea6-151">Zbytek rozložení stránky a dalších běžných aspektů zobrazení jsou určené v jiných souborů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-151">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="f9ea6-152">Další informace najdete v tématu [rozložení tématu](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-152">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="f9ea6-153">Jak určit řadiče zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-153">How controllers specify views</span></span>

<span data-ttu-id="f9ea6-154">Zobrazení jsou obvykle vrácené z akce jako [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), který je typem [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-154">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="f9ea6-155">Můžete vytvořit a vrátí metodu akce `ViewResult` přímo, ale neprovádí běžně.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-155">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="f9ea6-156">Vzhledem k tomu, že většina řadičů dědí [řadič](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), jednoduše použijte `View` Pomocná metoda vrátit `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-156">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="f9ea6-157">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="f9ea6-157">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="f9ea6-158">Po návratu tato akce *About.cshtml* zobrazení zobrazen v poslední části vykresleno jako na následující webové stránce:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-158">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![O stránku v prohlížeči Edge](overview/_static/about-page.png)

<span data-ttu-id="f9ea6-160">`View` Pomocná metoda má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-160">The `View` helper method has several overloads.</span></span> <span data-ttu-id="f9ea6-161">Volitelně můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-161">You can optionally specify:</span></span>

* <span data-ttu-id="f9ea6-162">Explicitní zobrazení vrátit:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-162">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="f9ea6-163">A [modelu](xref:mvc/models/model-binding) předat na zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-163">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="f9ea6-164">Zobrazení a modelu:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-164">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="f9ea6-165">Zobrazení zjišťování</span><span class="sxs-lookup"><span data-stu-id="f9ea6-165">View discovery</span></span>

<span data-ttu-id="f9ea6-166">Když akce vrátí zobrazení, proces se nazývá *zobrazení zjišťování* proběhne.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-166">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="f9ea6-167">Tento proces určuje, který soubor zobrazení se používá na základě názvu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-167">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="f9ea6-168">Výchozí chování `View` – metoda (`return View();`) je lze vrátit zobrazení se stejným názvem jako metodu akce, ve kterém je volána.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-168">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="f9ea6-169">Například *o* `ActionResult` název metody řadiče se používá k hledání pro zobrazení soubor s názvem *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-169">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="f9ea6-170">Nejprve hledá modulu runtime v *zobrazení / [ControllerName]* složka pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-170">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="f9ea6-171">Pokud se nenajde odpovídající zobrazení, vyhledávání *sdílené* složka pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-171">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="f9ea6-172">Není důležité, pokud se implicitně vrátíte `ViewResult` s `return View();` nebo explicitně předat název zobrazení má `View` metoda s `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-172">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="f9ea6-173">V obou případech zobrazení zjišťování hledá odpovídající soubor zobrazení v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-173">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="f9ea6-174">*Zobrazení nebo\[ControllerName]\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="f9ea6-174">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="f9ea6-175">*Zobrazení nebo sdílené nebo\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="f9ea6-175">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="f9ea6-176">Místo názvu zobrazení se dá zadat cestu k zobrazení souboru.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-176">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="f9ea6-177">Pokud používáte absolutní cestu spouštění v kořenovém adresáři aplikace (volitelně od verze "/" nebo "~ /"), *.cshtml* musí být zadán rozšíření:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-177">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="f9ea6-178">Relativní cesta můžete použít také k určení zobrazení v různých adresářích bez *.cshtml* rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-178">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="f9ea6-179">Uvnitř `HomeController`, můžete se vrátit *Index* zobrazení vaší *spravovat* zobrazení s relativní cesta:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-179">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="f9ea6-180">Podobně můžete určit, na aktuální adresář konkrétní řadič se ". /" předpony:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-180">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="f9ea6-181">[Částečná zobrazení](xref:mvc/views/partial) a [zobrazení součásti](xref:mvc/views/view-components) použijte mechanismy pro zjišťování podobné (ale ne identické).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-181">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="f9ea6-182">Můžete přizpůsobit výchozí konvenci pro způsob zobrazení se nacházejí v rámci aplikace s použitím vlastní [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-182">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="f9ea6-183">Zobrazení zjišťování spoléhá na hledání zobrazit soubory podle názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-183">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="f9ea6-184">Pokud podkladový systém souborů je velká a malá písmena, názvy zobrazení jsou pravděpodobně velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-184">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="f9ea6-185">Pro kompatibilitu mezi operační systémy malá a velká písmena mezi kontroleru a akce a přidružené zobrazení složky a názvy souborů.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-185">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="f9ea6-186">Pokud dojde k chybě, která soubor zobrazení nebyl nalezen při práci s systém souborů malá a velká písmena, potvrďte, že mezi požadované zobrazení souboru a název souboru skutečné zobrazení odpovídá malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-186">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="f9ea6-187">Postupujte podle osvědčený postup, uspořádání strukturu souborů pro zobrazení tak, aby odrážela vztahy mezi řadiče, akce a zobrazení udržovatelnosti a srozumitelnější.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-187">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="f9ea6-188">Předávání dat zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-188">Passing data to views</span></span>

<span data-ttu-id="f9ea6-189">Můžete předat data do zobrazení pomocí několik přístupů.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-189">You can pass data to views using several approaches.</span></span> <span data-ttu-id="f9ea6-190">Většina robustní přístupu slouží k zadání [modelu](xref:mvc/models/model-binding) typu v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-190">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="f9ea6-191">Tento model se často označuje jako *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-191">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="f9ea6-192">Instance typu viewmodel předáte do zobrazení z akce.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-192">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="f9ea6-193">Použití viewmodel k předávání dat do zobrazení umožňuje zobrazit využívat výhod *silné* kontrola typu.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-193">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="f9ea6-194">*Silného typování* (nebo *silného typu*) znamená, že všechny proměnné a konstanta nemá explicitně definovaný typ (například `string`, `int`, nebo `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-194">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="f9ea6-195">V době kompilace se kontroluje platnost typy používané v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-195">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="f9ea6-196">[Visual Studio](https://www.visualstudio.com/vs/) a [Visual Studio Code](https://code.visualstudio.com/) seznamu členů silně typované třídy pomocí funkci [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-196">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="f9ea6-197">Pokud chcete zobrazit vlastnosti viewmodel, zadejte název proměnné pro viewmodel následovaný tečkou (`.`).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-197">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="f9ea6-198">To umožňuje rychlejší psaní kódu s méně chyby.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-198">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="f9ea6-199">Zadejte model pomocí `@model` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-199">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="f9ea6-200">Použití modelu s `@Model`:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-200">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="f9ea6-201">K poskytnutí modelu pro zobrazení, předává kontroleru je jako parametr:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-201">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="f9ea6-202">Neexistují žádná omezení na modelu typy, které můžete zadat do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-202">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="f9ea6-203">Doporučujeme používat **P**rostý **O**ld **C**LR **O**viewmodels bject (objektů POCO) s minimální nebo žádnou chování (metody) definované.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-203">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="f9ea6-204">Obvykle jsou viewmodel třídy buď uložena v *modely* složku nebo samostatné *ViewModels* složku v kořenovém adresáři aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-204">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="f9ea6-205">*Adresu* viewmodel použít v předchozím příkladu je objektů POCO viewmodel, uložené v souboru s názvem *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-205">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> <span data-ttu-id="f9ea6-206">Nic brání použití stejné tříd pro vaše viewmodel typy a typy modelu vaší firmy.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-206">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="f9ea6-207">Použití samostatných modelů však umožňuje zobrazení k odlišení nezávisle z obchodní logiku a data přístup částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-207">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="f9ea6-208">Oddělení modely a viewmodels také nabízí výhody zabezpečení při použití modelů [model vazby](xref:mvc/models/model-binding) a [ověření](xref:mvc/models/validation) pro data odesílaná do aplikace uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-208">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="f9ea6-209">Slabě typované data (ViewData a ViewBag)</span><span class="sxs-lookup"><span data-stu-id="f9ea6-209">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="f9ea6-210">Poznámka: `ViewBag` není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-210">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="f9ea6-211">Kromě zobrazení silného typu zobrazení mají přístup k *slabě typované* (také nazývané *volného typu*) shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-211">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="f9ea6-212">Na rozdíl od silné typy *slabé typy* (nebo *ztrátě typy*) znamená, že jste si deklarovat explicitně typu dat, který používáte.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-212">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="f9ea6-213">Shromažďování dat o slabě typované můžete použít pro předávání malé množství dat a deaktivovat kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-213">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="f9ea6-214">Předávání dat mezi...</span><span class="sxs-lookup"><span data-stu-id="f9ea6-214">Passing data between a ...</span></span>                        | <span data-ttu-id="f9ea6-215">Příklad</span><span class="sxs-lookup"><span data-stu-id="f9ea6-215">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="f9ea6-216">Řadiče a zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-216">Controller and a view</span></span>                             | <span data-ttu-id="f9ea6-217">Naplnění rozevíracího seznamu, s daty.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-217">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="f9ea6-218">Zobrazení a [rozložení zobrazení](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="f9ea6-218">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="f9ea6-219">Nastavení  **\<title >** obsahu elementu v zobrazení rozložení ze zobrazení souboru.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-219">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="f9ea6-220">[Částečné zobrazení](xref:mvc/views/partial) a zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-220">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="f9ea6-221">Pomůcku, která zobrazuje data v závislosti na webové stránky, uživatel si vyžádal.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-221">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="f9ea6-222">Tuto kolekci lze odkazovat pomocí buď `ViewData` nebo `ViewBag` vlastnosti kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-222">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="f9ea6-223">`ViewData` Vlastnost je slovník slabě typované objektů.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-223">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="f9ea6-224">`ViewBag` Vlastnost je obálku kolem `ViewData` poskytuje dynamické vlastnosti základní `ViewData` kolekce.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-224">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="f9ea6-225">`ViewData`a `ViewBag` jsou vyřešeny dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-225">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="f9ea6-226">Vzhledem k tomu, že nemáte nabízejí kontrolu typu kompilaci, jak jsou obecně více náchylné než použití viewmodel.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-226">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="f9ea6-227">Z tohoto důvodu někteří vývojáři radši chtěli použít minimálně nebo nikdy `ViewData` a `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-227">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="f9ea6-228">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="f9ea6-228">**ViewData**</span></span>

<span data-ttu-id="f9ea6-229">`ViewData`je [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) objekt přistupovat prostřednictvím `string` klíče.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-229">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="f9ea6-230">Řetězec dat lze ukládat a používat přímo bez nutnosti přetypování, ale musíte vysílat dalších `ViewData` objekt hodnoty pro konkrétní typy při extrahování je.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-230">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="f9ea6-231">Můžete použít `ViewData` předání dat z řadiče zobrazení a v zobrazeních, včetně [částečná zobrazení](xref:mvc/views/partial) a [rozložení](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-231">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="f9ea6-232">Tady je příklad, který nastavuje hodnoty pozdrav a adresy `ViewData` v akci:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-232">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="f9ea6-233">Práce s daty v zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-233">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="f9ea6-234">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="f9ea6-234">**ViewBag**</span></span>

<span data-ttu-id="f9ea6-235">Poznámka: `ViewBag` není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-235">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="f9ea6-236">`ViewBag`je [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objekt, který poskytuje dynamické přístup k objektům, které jsou uložené v `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-236">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="f9ea6-237">`ViewBag`může být pohodlnější můžete pracovat, protože nevyžaduje přetypování.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-237">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="f9ea6-238">Následující příklad ukazuje, jak používat `ViewBag` se stejné výsledky jako pomocí `ViewData` výše:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-238">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="f9ea6-239">**Pomocí ViewData a ViewBag současně**</span><span class="sxs-lookup"><span data-stu-id="f9ea6-239">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="f9ea6-240">Poznámka: `ViewBag` není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-240">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="f9ea6-241">Vzhledem k tomu `ViewData` a `ViewBag` odkazovat na stejnou základní `ViewData` kolekce, můžete je používat `ViewData` a `ViewBag` a kombinovat a párovat mezi nimi při čtení a zápis hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-241">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="f9ea6-242">Nastavit název pomocí `ViewBag` a popis pomocí `ViewData` v horní části *About.cshtml* zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-242">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="f9ea6-243">Při čtení vlastností ale reverse použití `ViewData` a `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-243">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="f9ea6-244">V *_Layout.cshtml* souboru, získat název pomocí `ViewData` a získat pomocí popis `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-244">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="f9ea6-245">Mějte na paměti, že řetězce nevyžadují přetypování pro `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-245">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="f9ea6-246">Můžete použít `@ViewData["Title"]` bez přetypování.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-246">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="f9ea6-247">Pomocí obou `ViewData` a `ViewBag` ve stejnou dobu funguje, jako nepodporuje kombinace a porovnávání čtení a zápisu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-247">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="f9ea6-248">Následující kód je vykreslen:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-248">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="f9ea6-249">**Souhrnné informace o rozdílech mezi ViewData a ViewBag**</span><span class="sxs-lookup"><span data-stu-id="f9ea6-249">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="f9ea6-250">`ViewBag`není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-250">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="f9ea6-251">Odvozená z [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), takže má slovník vlastností, které mohou být užitečné, například `ContainsKey`, `Add`, `Remove`, a `Clear`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-251">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="f9ea6-252">Klíče ve slovníku jsou řetězce, takže je povolen prázdný znak.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-252">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="f9ea6-253">Příklad:`ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="f9ea6-253">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="f9ea6-254">Jakýkoli typ jinými než `string` musíte vysílat v zobrazení použít `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-254">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="f9ea6-255">Odvozená z [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), takže umožňuje vytváření dynamických vlastností pomocí zápisu s tečkou (`@ViewBag.SomeKey = <value or object>`), a není vyžadováno žádné přetypování.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-255">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="f9ea6-256">Syntaxe `ViewBag` umožňuje rychlejší pro přidání do kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-256">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="f9ea6-257">Jednodušší zkontrolujte hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-257">Simpler to check for null values.</span></span> <span data-ttu-id="f9ea6-258">Příklad:`@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="f9ea6-258">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="f9ea6-259">**Kdy použít ViewData nebo ViewBag**</span><span class="sxs-lookup"><span data-stu-id="f9ea6-259">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="f9ea6-260">Obě `ViewData` a `ViewBag` jsou rovnoměrně platný přístupy k předání malé množství dat mezi kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-260">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="f9ea6-261">Volba používané podle preference.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-261">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="f9ea6-262">Můžete kombinovat a párovat `ViewData` a `ViewBag` objekty, ale kód je snazší ke čtení a udržovat s jedním ze způsobů použít konzistentně.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-262">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="f9ea6-263">Obou přístupů jsou přeložit dynamicky za běhu a proto náchylné k způsobuje chyby za běhu.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-263">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="f9ea6-264">Vyhněte se některé vývojové týmy je.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-264">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="f9ea6-265">Dynamické zobrazení</span><span class="sxs-lookup"><span data-stu-id="f9ea6-265">Dynamic views</span></span>

<span data-ttu-id="f9ea6-266">Typ zobrazení, která není deklarovat model pomocí `@model` , ale dostatek instanci modelu předán do je (například `return View(Address);`) může odkazovat na instanci vlastnosti dynamicky:</span><span class="sxs-lookup"><span data-stu-id="f9ea6-266">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="f9ea6-267">Tato funkce nabízí flexibilitu ale nenabízí kompilace ochrany ani IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-267">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="f9ea6-268">Pokud vlastnost neexistuje, webová stránka generování selže za běhu.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-268">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="f9ea6-269">Více zobrazení funkcí</span><span class="sxs-lookup"><span data-stu-id="f9ea6-269">More view features</span></span>

<span data-ttu-id="f9ea6-270">[Značka pomocné rutiny](xref:mvc/views/tag-helpers/intro) snadno přidat chování na straně serveru do existující značky HTML.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-270">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="f9ea6-271">Pomocí značky Pomocníci zabraňuje nemusíte psát vlastní kód nebo pomocné rutiny v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-271">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="f9ea6-272">Značka pomocníků se použijí jako atributy prvků HTML a ignorují podle editory, které nelze zpracovat.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-272">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="f9ea6-273">To umožňuje upravit a vykreslit zobrazení Kód v různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-273">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="f9ea6-274">Generování vlastní značky HTML můžete dosáhnout s mnoho předdefinovaných pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-274">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="f9ea6-275">Složitější logiku uživatelského rozhraní lze provádět pomocí [zobrazení součásti](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-275">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="f9ea6-276">Zobrazení součásti zadejte stejné SoC kterých řadiče a nabídnout zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-276">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="f9ea6-277">Se může eliminovat potřebu akcemi a zobrazeními, které pracují s daty používá společné prvky uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f9ea6-277">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="f9ea6-278">Například mnoho dalších aspektů ASP.NET Core, zobrazení podporu [vkládání závislostí](xref:fundamentals/dependency-injection), povolení services [vloženy do zobrazení](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f9ea6-278">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
