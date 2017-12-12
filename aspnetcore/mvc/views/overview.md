---
title: "Zobrazení v jádro ASP.NET MVC"
author: ardalis
description: "Zjistěte, jak pracovat zobrazení prezentace dat aplikace a interakce uživatelů v aplikaci ASP.NET MVC jádra."
keywords: "ASP.NET Core zobrazení MVC razor, viewmodel, viewdata, viewbag"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 4530d2f500dd887bf649a753283fb3e4af995322
ms.sourcegitcommit: c2f6c593d81fbd90e6ddd672fe0a5636d06b615a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="875f5-104">Zobrazení v jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="875f5-104">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="875f5-105">Podle [Steve Smith](https://ardalis.com/) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="875f5-105">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="875f5-106">V **M**odelu -**V**obrazit -**C**vzor ontroller (MVC), *zobrazení* zpracovává data aplikace prezentace a uživatelské interakce.</span><span class="sxs-lookup"><span data-stu-id="875f5-106">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="875f5-107">Zobrazení je šablony HTML s vložených [kódu Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="875f5-107">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="875f5-108">Značka Razor je kód, který komunikuje s kódu HTML, který vytvoří webovou stránku, která je odeslána do klienta.</span><span class="sxs-lookup"><span data-stu-id="875f5-108">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="875f5-109">V aplikaci ASP.NET MVC jádra, jsou zobrazení *.cshtml* soubory, které používají [programovací jazyk C#](/dotnet/csharp/) v kódu Razor.</span><span class="sxs-lookup"><span data-stu-id="875f5-109">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="875f5-110">Obvykle jsou seskupené zobrazit soubory do složky s názvem pro jednotlivé aplikace [řadiče](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="875f5-110">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="875f5-111">Složky jsou uložené v *zobrazení* složku v kořenovém adresáři aplikace:</span><span class="sxs-lookup"><span data-stu-id="875f5-111">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Zobrazení složky v řešení pro Průzkumníka Visual Studio je otevřený s otevřený a zobrazit soubory About.cshtml, Contact.cshtml a Index.cshtml Domovská složka](overview/_static/views_solution_explorer.png)

<span data-ttu-id="875f5-113">*Domů* je reprezentována řadiče *Domů* složky uvnitř *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="875f5-113">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="875f5-114">*Domů* složka obsahuje zobrazení pro *o*, *kontaktujte*, a *Index* webové stránky (domovské stránce).</span><span class="sxs-lookup"><span data-stu-id="875f5-114">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="875f5-115">Když uživatel požádá o, jednu z těchto tří webových stránkách akce kontroleru v *Domů* řadič určit, který tři zobrazení, které se používá k sestavení a vrátí webové stránky pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="875f5-115">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="875f5-116">Použití [rozložení](xref:mvc/views/layout) k poskytování části konzistentní webové stránky a snížit opakování kódu.</span><span class="sxs-lookup"><span data-stu-id="875f5-116">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="875f5-117">Rozložení často obsahují záhlaví, navigace a nabídky prvky a zápatí.</span><span class="sxs-lookup"><span data-stu-id="875f5-117">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="875f5-118">Záhlaví a zápatí obvykle obsahuje často používaný kód pro mnoho elementy metadata a odkazy na prostředky skriptu a stylu.</span><span class="sxs-lookup"><span data-stu-id="875f5-118">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="875f5-119">Rozložení umožňuje vyhnout se tento často používaný kód v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-119">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="875f5-120">[Částečná zobrazení](xref:mvc/views/partial) snížit zdvojení kódu pomocí správy opakovaně použitelné součásti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-120">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="875f5-121">Například je užitečné pro biografii Autor na web blogu, který se zobrazí v několik zobrazení částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-121">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="875f5-122">Vytváření biografii je obyčejnou zobrazení obsahu a nevyžaduje kód provést, aby bylo možné vytvořit obsahu pro webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="875f5-122">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="875f5-123">Autor biografii obsah není k dispozici na zobrazení ve samostatně, vazby modelu, takže použití částečné zobrazení pro tento typ obsahu je ideální.</span><span class="sxs-lookup"><span data-stu-id="875f5-123">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="875f5-124">[Zobrazit součásti](xref:mvc/views/view-components) jsou podobná částečné zobrazení, protože umožňují snížit opakovaných kód, ale jsou vhodné pro zobrazení obsahu, který vyžaduje kód pro spuštění na serveru k vykreslení webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="875f5-124">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="875f5-125">Zobrazení součásti jsou užitečné při vykreslené obsah vyžaduje interakci databáze, například pro web nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="875f5-125">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="875f5-126">Zobrazení součásti nejsou omezena na modelu vazby k vytvoření výstupu webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="875f5-126">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="875f5-127">Výhody použití zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-127">Benefits of using views</span></span>

<span data-ttu-id="875f5-128">Zobrazení nápovědy k vytvoření [ **S**eparation **o**f **C**oncerns (SoC) návrhu](http://deviq.com/separation-of-concerns/) v aplikaci MVC oddělením kód uživatelské rozhraní z dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f5-128">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="875f5-129">Následující SoC návrhu, bude vaše aplikace modulární, který nabízí několik výhod:</span><span class="sxs-lookup"><span data-stu-id="875f5-129">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="875f5-130">Tato aplikace je snazší správa, protože je lépe uspořádány.</span><span class="sxs-lookup"><span data-stu-id="875f5-130">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="875f5-131">Zobrazení jsou obecně seskupené podle funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f5-131">Views are generally grouped by app feature.</span></span> <span data-ttu-id="875f5-132">Díky tomu je snazší najít související zobrazení při práci na funkce.</span><span class="sxs-lookup"><span data-stu-id="875f5-132">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="875f5-133">Součástí aplikace jsou volně vázány.</span><span class="sxs-lookup"><span data-stu-id="875f5-133">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="875f5-134">Můžete vytvářet a aktualizovat zobrazení aplikace odděleně od komponenty obchodní logiku a data access.</span><span class="sxs-lookup"><span data-stu-id="875f5-134">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="875f5-135">Zobrazení aplikace můžete upravit aniž byste museli mít aktualizovat dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f5-135">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="875f5-136">Aby bylo jednodušší testování části uživatelského rozhraní aplikace, protože zobrazení jsou samostatné jednotky.</span><span class="sxs-lookup"><span data-stu-id="875f5-136">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="875f5-137">Kvůli lepší organizaci je méně pravděpodobné, že budete ať už náhodně opakování části uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="875f5-137">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="875f5-138">Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-138">Creating a view</span></span>

<span data-ttu-id="875f5-139">Zobrazení, které jsou specifické pro určitý kontroler jsou vytvořené v *zobrazení / [ControllerName]* složky.</span><span class="sxs-lookup"><span data-stu-id="875f5-139">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="875f5-140">Zobrazení, které jsou sdíleny mezi řadiče jsou umístěny v *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="875f5-140">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="875f5-141">Pokud chcete vytvořit zobrazení, přidejte nový soubor a poskytněte stejný název jako jeho přidruženému kontroleru akce s *.cshtml* příponu souboru.</span><span class="sxs-lookup"><span data-stu-id="875f5-141">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="875f5-142">Vytvoření zobrazení, která odpovídá *o* akce v *Domů* řadič, vytvořit *About.cshtml* souboru v *zobrazení Domů*složky:</span><span class="sxs-lookup"><span data-stu-id="875f5-142">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="875f5-143">*Syntaxe Razor* značek začíná `@` symbol.</span><span class="sxs-lookup"><span data-stu-id="875f5-143">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="875f5-144">Spusťte příkazy jazyka C# tím, že umístíte C# kódu v rámci [bloky kódu Razor](xref:mvc/views/razor#razor-code-blocks) nastavit ve složených závorkách (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="875f5-144">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="875f5-145">Například najdete v sekci přiřazení "O" `ViewData["Title"]` uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="875f5-145">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="875f5-146">Můžete zobrazit hodnoty v kódu HTML hodnotu s odkazem jednoduše `@` symbol.</span><span class="sxs-lookup"><span data-stu-id="875f5-146">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="875f5-147">Zobrazit obsah `<h2>` a `<h3>` prvků uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="875f5-147">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="875f5-148">Zobrazení obsahu uvedené výše je jenom část celé webové stránky, které je vykresleno do uživatele.</span><span class="sxs-lookup"><span data-stu-id="875f5-148">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="875f5-149">Zbytek rozložení stránky a dalších běžných aspektů zobrazení jsou určené v jiných souborů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-149">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="875f5-150">Další informace najdete v tématu [rozložení tématu](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="875f5-150">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="875f5-151">Jak určit řadiče zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-151">How controllers specify views</span></span>

<span data-ttu-id="875f5-152">Zobrazení jsou obvykle vrácené z akce jako [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), který je typem [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="875f5-152">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="875f5-153">Můžete vytvořit a vrátí metodu akce `ViewResult` přímo, ale neprovádí běžně.</span><span class="sxs-lookup"><span data-stu-id="875f5-153">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="875f5-154">Vzhledem k tomu, že většina řadičů dědí [řadič](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), jednoduše použijte `View` Pomocná metoda vrátit `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="875f5-154">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="875f5-155">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="875f5-155">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="875f5-156">Po návratu tato akce *About.cshtml* zobrazení zobrazen v poslední části vykresleno jako na následující webové stránce:</span><span class="sxs-lookup"><span data-stu-id="875f5-156">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![O stránku v prohlížeči Edge](overview/_static/about-page.png)

<span data-ttu-id="875f5-158">`View` Pomocná metoda má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="875f5-158">The `View` helper method has several overloads.</span></span> <span data-ttu-id="875f5-159">Volitelně můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="875f5-159">You can optionally specify:</span></span>

* <span data-ttu-id="875f5-160">Explicitní zobrazení vrátit:</span><span class="sxs-lookup"><span data-stu-id="875f5-160">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="875f5-161">A [modelu](xref:mvc/models/model-binding) předat na zobrazení:</span><span class="sxs-lookup"><span data-stu-id="875f5-161">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="875f5-162">Zobrazení a modelu:</span><span class="sxs-lookup"><span data-stu-id="875f5-162">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="875f5-163">Zobrazení zjišťování</span><span class="sxs-lookup"><span data-stu-id="875f5-163">View discovery</span></span>

<span data-ttu-id="875f5-164">Když akce vrátí zobrazení, proces se nazývá *zobrazení zjišťování* proběhne.</span><span class="sxs-lookup"><span data-stu-id="875f5-164">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="875f5-165">Tento proces určuje, který soubor zobrazení se používá na základě názvu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-165">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="875f5-166">Výchozí chování `View` – metoda (`return View();`) je lze vrátit zobrazení se stejným názvem jako metodu akce, ve kterém je volána.</span><span class="sxs-lookup"><span data-stu-id="875f5-166">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="875f5-167">Například *o* `ActionResult` název metody řadiče se používá k hledání pro zobrazení soubor s názvem *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="875f5-167">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="875f5-168">Nejprve hledá modulu runtime v *zobrazení / [ControllerName]* složka pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-168">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="875f5-169">Pokud se nenajde odpovídající zobrazení, vyhledávání *sdílené* složka pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-169">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="875f5-170">Není důležité, pokud se implicitně vrátíte `ViewResult` s `return View();` nebo explicitně předat název zobrazení má `View` metoda s `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="875f5-170">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="875f5-171">V obou případech zobrazení zjišťování hledá odpovídající soubor zobrazení v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="875f5-171">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="875f5-172">*Zobrazení nebo\[ControllerName]\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="875f5-172">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="875f5-173">*Zobrazení nebo sdílené nebo\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="875f5-173">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="875f5-174">Místo názvu zobrazení se dá zadat cestu k zobrazení souboru.</span><span class="sxs-lookup"><span data-stu-id="875f5-174">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="875f5-175">Pokud používáte absolutní cestu spouštění v kořenovém adresáři aplikace (volitelně od verze "/" nebo "~ /"), *.cshtml* musí být zadán rozšíření:</span><span class="sxs-lookup"><span data-stu-id="875f5-175">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="875f5-176">Relativní cesta můžete použít také k určení zobrazení v různých adresářích bez *.cshtml* rozšíření.</span><span class="sxs-lookup"><span data-stu-id="875f5-176">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="875f5-177">Uvnitř `HomeController`, můžete se vrátit *Index* zobrazení vaší *spravovat* zobrazení s relativní cesta:</span><span class="sxs-lookup"><span data-stu-id="875f5-177">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="875f5-178">Podobně můžete určit, na aktuální adresář konkrétní řadič se ". /" předpony:</span><span class="sxs-lookup"><span data-stu-id="875f5-178">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="875f5-179">[Částečná zobrazení](xref:mvc/views/partial) a [zobrazení součásti](xref:mvc/views/view-components) použijte mechanismy pro zjišťování podobné (ale ne identické).</span><span class="sxs-lookup"><span data-stu-id="875f5-179">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="875f5-180">Můžete přizpůsobit výchozí konvenci pro způsob zobrazení se nacházejí v rámci aplikace s použitím vlastní [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="875f5-180">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="875f5-181">Zobrazení zjišťování spoléhá na hledání zobrazit soubory podle názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="875f5-181">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="875f5-182">Pokud podkladový systém souborů je velká a malá písmena, názvy zobrazení jsou pravděpodobně velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="875f5-182">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="875f5-183">Pro kompatibilitu mezi operační systémy malá a velká písmena mezi kontroleru a akce a přidružené zobrazení složky a názvy souborů.</span><span class="sxs-lookup"><span data-stu-id="875f5-183">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="875f5-184">Pokud dojde k chybě, která soubor zobrazení nebyl nalezen při práci s systém souborů malá a velká písmena, potvrďte, že mezi požadované zobrazení souboru a název souboru skutečné zobrazení odpovídá malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="875f5-184">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="875f5-185">Postupujte podle osvědčený postup, uspořádání strukturu souborů pro zobrazení tak, aby odrážela vztahy mezi řadiče, akce a zobrazení udržovatelnosti a srozumitelnější.</span><span class="sxs-lookup"><span data-stu-id="875f5-185">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="875f5-186">Předávání dat zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-186">Passing data to views</span></span>

<span data-ttu-id="875f5-187">Můžete předat data do zobrazení pomocí několik přístupů.</span><span class="sxs-lookup"><span data-stu-id="875f5-187">You can pass data to views using several approaches.</span></span> <span data-ttu-id="875f5-188">Většina robustní přístupu slouží k zadání [modelu](xref:mvc/models/model-binding) typu v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-188">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="875f5-189">Tento model se často označuje jako *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="875f5-189">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="875f5-190">Instance typu viewmodel předáte do zobrazení z akce.</span><span class="sxs-lookup"><span data-stu-id="875f5-190">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="875f5-191">Použití viewmodel k předávání dat do zobrazení umožňuje zobrazit využívat výhod *silné* kontrola typu.</span><span class="sxs-lookup"><span data-stu-id="875f5-191">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="875f5-192">*Silného typování* (nebo *silného typu*) znamená, že všechny proměnné a konstanta nemá explicitně definovaný typ (například `string`, `int`, nebo `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="875f5-192">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="875f5-193">V době kompilace se kontroluje platnost typy používané v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-193">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="875f5-194">[Visual Studio](https://www.visualstudio.com/vs/) a [Visual Studio Code](https://code.visualstudio.com/) seznamu členů silně typované třídy pomocí funkci [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="875f5-194">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="875f5-195">Pokud chcete zobrazit vlastnosti viewmodel, zadejte název proměnné pro viewmodel následovaný tečkou (`.`).</span><span class="sxs-lookup"><span data-stu-id="875f5-195">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="875f5-196">To umožňuje rychlejší psaní kódu s méně chyby.</span><span class="sxs-lookup"><span data-stu-id="875f5-196">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="875f5-197">Zadejte model pomocí `@model` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="875f5-197">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="875f5-198">Použití modelu s `@Model`:</span><span class="sxs-lookup"><span data-stu-id="875f5-198">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="875f5-199">K poskytnutí modelu pro zobrazení, předává kontroleru je jako parametr:</span><span class="sxs-lookup"><span data-stu-id="875f5-199">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="875f5-200">Neexistují žádná omezení na modelu typy, které můžete zadat do zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-200">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="875f5-201">Doporučujeme používat **P**rostý **O**ld **C**LR **O**viewmodels bject (objektů POCO) s minimální nebo žádnou chování (metody) definované.</span><span class="sxs-lookup"><span data-stu-id="875f5-201">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="875f5-202">Obvykle jsou viewmodel třídy buď uložena v *modely* složku nebo samostatné *ViewModels* složku v kořenovém adresáři aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f5-202">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="875f5-203">*Adresu* viewmodel použít v předchozím příkladu je objektů POCO viewmodel, uložené v souboru s názvem *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="875f5-203">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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
> <span data-ttu-id="875f5-204">Nic brání použití stejné tříd pro vaše viewmodel typy a typy modelu vaší firmy.</span><span class="sxs-lookup"><span data-stu-id="875f5-204">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="875f5-205">Použití samostatných modelů však umožňuje zobrazení k odlišení nezávisle z obchodní logiku a data přístup částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f5-205">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="875f5-206">Oddělení modely a viewmodels také nabízí výhody zabezpečení při použití modelů [model vazby](xref:mvc/models/model-binding) a [ověření](xref:mvc/models/validation) pro data odesílaná do aplikace uživatelem.</span><span class="sxs-lookup"><span data-stu-id="875f5-206">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="875f5-207">Slabě typované data (ViewData a ViewBag)</span><span class="sxs-lookup"><span data-stu-id="875f5-207">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="875f5-208">Poznámka: `ViewBag` není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="875f5-208">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="875f5-209">Kromě zobrazení silného typu zobrazení mají přístup k *slabě typované* (také nazývané *volného typu*) shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="875f5-209">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="875f5-210">Na rozdíl od silné typy *slabé typy* (nebo *ztrátě typy*) znamená, že jste si deklarovat explicitně typu dat, který používáte.</span><span class="sxs-lookup"><span data-stu-id="875f5-210">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="875f5-211">Shromažďování dat o slabě typované můžete použít pro předávání malé množství dat a deaktivovat kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-211">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="875f5-212">Předávání dat mezi...</span><span class="sxs-lookup"><span data-stu-id="875f5-212">Passing data between a ...</span></span>                        | <span data-ttu-id="875f5-213">Příklad</span><span class="sxs-lookup"><span data-stu-id="875f5-213">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="875f5-214">Řadiče a zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-214">Controller and a view</span></span>                             | <span data-ttu-id="875f5-215">Naplnění rozevíracího seznamu, s daty.</span><span class="sxs-lookup"><span data-stu-id="875f5-215">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="875f5-216">Zobrazení a [rozložení zobrazení](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="875f5-216">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="875f5-217">Nastavení  **\<title >** obsahu elementu v zobrazení rozložení ze zobrazení souboru.</span><span class="sxs-lookup"><span data-stu-id="875f5-217">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="875f5-218">[Částečné zobrazení](xref:mvc/views/partial) a zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-218">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="875f5-219">Pomůcku, která zobrazuje data v závislosti na webové stránky, uživatel si vyžádal.</span><span class="sxs-lookup"><span data-stu-id="875f5-219">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="875f5-220">Tuto kolekci lze odkazovat pomocí buď `ViewData` nebo `ViewBag` vlastnosti kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-220">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="875f5-221">`ViewData` Vlastnost je slovník slabě typované objektů.</span><span class="sxs-lookup"><span data-stu-id="875f5-221">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="875f5-222">`ViewBag` Vlastnost je obálku kolem `ViewData` poskytuje dynamické vlastnosti základní `ViewData` kolekce.</span><span class="sxs-lookup"><span data-stu-id="875f5-222">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="875f5-223">`ViewData`a `ViewBag` jsou vyřešeny dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="875f5-223">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="875f5-224">Vzhledem k tomu, že nemáte nabízejí kontrolu typu kompilaci, jak jsou obecně více náchylné než použití viewmodel.</span><span class="sxs-lookup"><span data-stu-id="875f5-224">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="875f5-225">Z tohoto důvodu někteří vývojáři radši chtěli použít minimálně nebo nikdy `ViewData` a `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="875f5-225">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="875f5-226">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="875f5-226">**ViewData**</span></span>

<span data-ttu-id="875f5-227">`ViewData`je [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) objekt přistupovat prostřednictvím `string` klíče.</span><span class="sxs-lookup"><span data-stu-id="875f5-227">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="875f5-228">Řetězec dat lze ukládat a používat přímo bez nutnosti přetypování, ale musíte vysílat dalších `ViewData` objekt hodnoty pro konkrétní typy při extrahování je.</span><span class="sxs-lookup"><span data-stu-id="875f5-228">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="875f5-229">Můžete použít `ViewData` předání dat z řadiče zobrazení a v zobrazeních, včetně [částečná zobrazení](xref:mvc/views/partial) a [rozložení](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="875f5-229">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="875f5-230">Tady je příklad, který nastavuje hodnoty pozdrav a adresy `ViewData` v akci:</span><span class="sxs-lookup"><span data-stu-id="875f5-230">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="875f5-231">Práce s daty v zobrazení:</span><span class="sxs-lookup"><span data-stu-id="875f5-231">Work with the data in a view:</span></span>

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

<span data-ttu-id="875f5-232">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="875f5-232">**ViewBag**</span></span>

<span data-ttu-id="875f5-233">Poznámka: `ViewBag` není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="875f5-233">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="875f5-234">`ViewBag`je [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objekt, který poskytuje dynamické přístup k objektům, které jsou uložené v `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="875f5-234">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="875f5-235">`ViewBag`může být pohodlnější můžete pracovat, protože nevyžaduje přetypování.</span><span class="sxs-lookup"><span data-stu-id="875f5-235">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="875f5-236">Následující příklad ukazuje, jak používat `ViewBag` se stejné výsledky jako pomocí `ViewData` výše:</span><span class="sxs-lookup"><span data-stu-id="875f5-236">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

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

<span data-ttu-id="875f5-237">**Pomocí ViewData a ViewBag současně**</span><span class="sxs-lookup"><span data-stu-id="875f5-237">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="875f5-238">Poznámka: `ViewBag` není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="875f5-238">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="875f5-239">Vzhledem k tomu `ViewData` a `ViewBag` odkazovat na stejnou základní `ViewData` kolekce, můžete je používat `ViewData` a `ViewBag` a kombinovat a párovat mezi nimi při čtení a zápis hodnoty.</span><span class="sxs-lookup"><span data-stu-id="875f5-239">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="875f5-240">Nastavit název pomocí `ViewBag` a popis pomocí `ViewData` v horní části *About.cshtml* zobrazení:</span><span class="sxs-lookup"><span data-stu-id="875f5-240">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="875f5-241">Při čtení vlastností ale reverse použití `ViewData` a `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="875f5-241">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="875f5-242">V *_Layout.cshtml* souboru, získat název pomocí `ViewData` a získat pomocí popis `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="875f5-242">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="875f5-243">Mějte na paměti, že řetězce nevyžadují přetypování pro `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="875f5-243">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="875f5-244">Můžete použít `@ViewData["Title"]` bez přetypování.</span><span class="sxs-lookup"><span data-stu-id="875f5-244">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="875f5-245">Pomocí obou `ViewData` a `ViewBag` ve stejnou dobu funguje, jako nepodporuje kombinace a porovnávání čtení a zápisu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="875f5-245">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="875f5-246">Následující kód je vykreslen:</span><span class="sxs-lookup"><span data-stu-id="875f5-246">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="875f5-247">**Souhrnné informace o rozdílech mezi ViewData a ViewBag**</span><span class="sxs-lookup"><span data-stu-id="875f5-247">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="875f5-248">`ViewBag`není k dispozici na stránkách Razor.</span><span class="sxs-lookup"><span data-stu-id="875f5-248">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="875f5-249">Odvozená z [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), takže má slovník vlastností, které mohou být užitečné, například `ContainsKey`, `Add`, `Remove`, a `Clear`.</span><span class="sxs-lookup"><span data-stu-id="875f5-249">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="875f5-250">Klíče ve slovníku jsou řetězce, takže je povolen prázdný znak.</span><span class="sxs-lookup"><span data-stu-id="875f5-250">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="875f5-251">Příklad:`ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="875f5-251">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="875f5-252">Jakýkoli typ jinými než `string` musíte vysílat v zobrazení použít `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="875f5-252">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="875f5-253">Odvozená z [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), takže umožňuje vytváření dynamických vlastností pomocí zápisu s tečkou (`@ViewBag.SomeKey = <value or object>`), a není vyžadováno žádné přetypování.</span><span class="sxs-lookup"><span data-stu-id="875f5-253">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="875f5-254">Syntaxe `ViewBag` umožňuje rychlejší pro přidání do kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-254">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="875f5-255">Jednodušší zkontrolujte hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="875f5-255">Simpler to check for null values.</span></span> <span data-ttu-id="875f5-256">Příklad:`@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="875f5-256">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="875f5-257">**Kdy použít ViewData nebo ViewBag**</span><span class="sxs-lookup"><span data-stu-id="875f5-257">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="875f5-258">Obě `ViewData` a `ViewBag` jsou rovnoměrně platný přístupy k předání malé množství dat mezi kontrolery a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-258">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="875f5-259">Volba používané podle preference.</span><span class="sxs-lookup"><span data-stu-id="875f5-259">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="875f5-260">Můžete kombinovat a párovat `ViewData` a `ViewBag` objekty, ale kód je snazší ke čtení a udržovat s jedním ze způsobů použít konzistentně.</span><span class="sxs-lookup"><span data-stu-id="875f5-260">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="875f5-261">Obou přístupů jsou přeložit dynamicky za běhu a proto náchylné k způsobuje chyby za běhu.</span><span class="sxs-lookup"><span data-stu-id="875f5-261">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="875f5-262">Vyhněte se některé vývojové týmy je.</span><span class="sxs-lookup"><span data-stu-id="875f5-262">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="875f5-263">Dynamické zobrazení</span><span class="sxs-lookup"><span data-stu-id="875f5-263">Dynamic views</span></span>

<span data-ttu-id="875f5-264">Typ zobrazení, která není deklarovat model pomocí `@model` , ale dostatek instanci modelu předán do je (například `return View(Address);`) může odkazovat na instanci vlastnosti dynamicky:</span><span class="sxs-lookup"><span data-stu-id="875f5-264">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="875f5-265">Tato funkce nabízí flexibilitu ale nenabízí kompilace ochrany ani IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="875f5-265">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="875f5-266">Pokud vlastnost neexistuje, webová stránka generování selže za běhu.</span><span class="sxs-lookup"><span data-stu-id="875f5-266">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="875f5-267">Více zobrazení funkcí</span><span class="sxs-lookup"><span data-stu-id="875f5-267">More view features</span></span>

<span data-ttu-id="875f5-268">[Značka pomocné rutiny](xref:mvc/views/tag-helpers/intro) snadno přidat chování na straně serveru do existující značky HTML.</span><span class="sxs-lookup"><span data-stu-id="875f5-268">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="875f5-269">Pomocí značky Pomocníci zabraňuje nemusíte psát vlastní kód nebo pomocné rutiny v rámci zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-269">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="875f5-270">Značka pomocníků se použijí jako atributy prvků HTML a ignorují podle editory, které nelze zpracovat.</span><span class="sxs-lookup"><span data-stu-id="875f5-270">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="875f5-271">To umožňuje upravit a vykreslit zobrazení Kód v různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="875f5-271">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="875f5-272">Generování vlastní značky HTML můžete dosáhnout s mnoho předdefinovaných pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="875f5-272">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="875f5-273">Složitější logiku uživatelského rozhraní lze provádět pomocí [zobrazení součásti](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="875f5-273">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="875f5-274">Zobrazení součásti zadejte stejné SoC kterých řadiče a nabídnout zobrazení.</span><span class="sxs-lookup"><span data-stu-id="875f5-274">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="875f5-275">Se může eliminovat potřebu akcemi a zobrazeními, které pracují s daty používá společné prvky uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="875f5-275">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="875f5-276">Například mnoho dalších aspektů ASP.NET Core, zobrazení podporu [vkládání závislostí](xref:fundamentals/dependency-injection), povolení services [vloženy do zobrazení](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="875f5-276">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
