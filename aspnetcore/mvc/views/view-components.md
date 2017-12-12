---
title: "Zobrazení součásti"
author: rick-anderson
description: "Zobrazení součásti jsou určeny kdekoli, že máte opakovaně použitelné vykreslování logiku."
keywords: "ASP.NET Core, součásti zobrazení částečného zobrazení"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 2cf82df78c250cdfdd808d49acfc06dc2ea82f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="view-components"></a><span data-ttu-id="95d2f-104">Zobrazení součásti</span><span class="sxs-lookup"><span data-stu-id="95d2f-104">View components</span></span>

<span data-ttu-id="95d2f-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="95d2f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="95d2f-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="95d2f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="95d2f-107">Představení součásti zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-107">Introducing view components</span></span>

<span data-ttu-id="95d2f-108">Nové do architektury ASP.NET MVC jádra, zobrazení součásti jsou podobná částečné zobrazení, ale jsou mnohem silnější.</span><span class="sxs-lookup"><span data-stu-id="95d2f-108">New to ASP.NET Core MVC, view components are similar to partial views, but they are much more powerful.</span></span> <span data-ttu-id="95d2f-109">Součásti zobrazení nemáte použít modelovou vazbu a pouze závisí na data, která je zadat při volání do ní.</span><span class="sxs-lookup"><span data-stu-id="95d2f-109">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="95d2f-110">Součást zobrazení:</span><span class="sxs-lookup"><span data-stu-id="95d2f-110">A view component:</span></span>

* <span data-ttu-id="95d2f-111">Vykreslí bloku dat, nikoli celý odpověď</span><span class="sxs-lookup"><span data-stu-id="95d2f-111">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="95d2f-112">Zahrnuje stejné oddělení z otázky a výhody testovatelnosti nalezen mezi řadiče a zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-112">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="95d2f-113">Může mít parametry a obchodní logiky</span><span class="sxs-lookup"><span data-stu-id="95d2f-113">Can have parameters and business logic</span></span>
* <span data-ttu-id="95d2f-114">Obvykle je volána z rozložení stránky</span><span class="sxs-lookup"><span data-stu-id="95d2f-114">Is typically invoked from a layout page</span></span>

<span data-ttu-id="95d2f-115">Zobrazení součásti jsou určeny kdekoli, že máte opakovaně použitelné vykreslování logiky, která je příliš složitý pro částečné zobrazení, jako například:</span><span class="sxs-lookup"><span data-stu-id="95d2f-115">View components are intended anywhere you have reusable rendering logic that is too complex for a partial view, such as:</span></span>

* <span data-ttu-id="95d2f-116">Dynamické navigační nabídky</span><span class="sxs-lookup"><span data-stu-id="95d2f-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="95d2f-117">Značka cloudu (kde dotazuje databázi)</span><span class="sxs-lookup"><span data-stu-id="95d2f-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="95d2f-118">Panel přihlášení</span><span class="sxs-lookup"><span data-stu-id="95d2f-118">Login panel</span></span>
* <span data-ttu-id="95d2f-119">Nákupní košík</span><span class="sxs-lookup"><span data-stu-id="95d2f-119">Shopping cart</span></span>
* <span data-ttu-id="95d2f-120">Nedávno publikovaných článcích</span><span class="sxs-lookup"><span data-stu-id="95d2f-120">Recently published articles</span></span>
* <span data-ttu-id="95d2f-121">Obsah bočním panelu na typické blogu</span><span class="sxs-lookup"><span data-stu-id="95d2f-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="95d2f-122">Přihlášení panel, který by být vykreslen na každé stránce a zobrazovat odkazy na odhlášení nebo přihlášení, v závislosti na protokolu ve stavu uživatele</span><span class="sxs-lookup"><span data-stu-id="95d2f-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="95d2f-123">Součást zobrazení se skládá ze dvou částí: třídy (obvykle odvozené od [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) a výsledek vrátí (obvykle zobrazení).</span><span class="sxs-lookup"><span data-stu-id="95d2f-123">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="95d2f-124">Jako řadiče, může být součást zobrazení objektů POCO, ale Většina vývojářů chtít využívat výhod metody a vlastnosti, které jsou k dispozici odvozené z `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="95d2f-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="95d2f-125">Vytvoření zobrazení komponenty</span><span class="sxs-lookup"><span data-stu-id="95d2f-125">Creating a view component</span></span>

<span data-ttu-id="95d2f-126">Tato část obsahuje základní požadavky pro vytvoření zobrazení komponenty.</span><span class="sxs-lookup"><span data-stu-id="95d2f-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="95d2f-127">Dále v tomto článku jsme budete Zkontrolujte každý krok podrobně a vytvoření zobrazení komponenty.</span><span class="sxs-lookup"><span data-stu-id="95d2f-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="95d2f-128">Třídy součástí zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-128">The view component class</span></span>

<span data-ttu-id="95d2f-129">Třída součásti zobrazení lze vytvořit pomocí některé z následujících:</span><span class="sxs-lookup"><span data-stu-id="95d2f-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="95d2f-130">Odvozování z *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="95d2f-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="95d2f-131">Architekturu třídu s `[ViewComponent]` atribut nebo odvozování od třídy, se `[ViewComponent]` atribut</span><span class="sxs-lookup"><span data-stu-id="95d2f-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="95d2f-132">Vytvoření třídy, kde název končí příponou *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="95d2f-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="95d2f-133">Jako řadiče zobrazení součásti musí být veřejné, -nested a neabstraktní třídy.</span><span class="sxs-lookup"><span data-stu-id="95d2f-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="95d2f-134">Název součásti zobrazení je název třídy s příponou "ViewComponent" odebrat.</span><span class="sxs-lookup"><span data-stu-id="95d2f-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="95d2f-135">Ho je také možné explicitně zadat pomocí `ViewComponentAttribute.Name` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="95d2f-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="95d2f-136">Třídy zobrazení komponenty:</span><span class="sxs-lookup"><span data-stu-id="95d2f-136">A view component class:</span></span>

* <span data-ttu-id="95d2f-137">Plně podporuje konstruktor [vkládání závislostí](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="95d2f-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="95d2f-138">Nevyžaduje část v životním cyklu řadiče, což znamená, nemůžete použít [filtry](../controllers/filters.md) v komponentě zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-138">Does not take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="95d2f-139">Zobrazení metody součásti</span><span class="sxs-lookup"><span data-stu-id="95d2f-139">View component methods</span></span>

<span data-ttu-id="95d2f-140">Součást zobrazení definuje svou logikou v `InvokeAsync` metodu, která vrátí `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="95d2f-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="95d2f-141">Parametry pocházejí přímo z volání součásti zobrazení, nikoli z vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="95d2f-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="95d2f-142">Součást zobrazení nikdy přímo zpracuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="95d2f-142">A view component never directly handles a request.</span></span> <span data-ttu-id="95d2f-143">Obvykle komponentu zobrazení inicializuje modelu a předává je pro zobrazení pomocí volání `View` metoda.</span><span class="sxs-lookup"><span data-stu-id="95d2f-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="95d2f-144">Souhrnně zobrazení součást metody:</span><span class="sxs-lookup"><span data-stu-id="95d2f-144">In summary, view component methods:</span></span>

* <span data-ttu-id="95d2f-145">Definování `InvokeAsync` metodu, která vrátí`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="95d2f-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="95d2f-146">Obvykle inicializuje modelu a předává je pro zobrazení pomocí volání `ViewComponent` `View` – metoda</span><span class="sxs-lookup"><span data-stu-id="95d2f-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="95d2f-147">Parametry pocházejí z volání metoda HTTP není, neexistuje žádná vazba modelu</span><span class="sxs-lookup"><span data-stu-id="95d2f-147">Parameters come from the calling method, not HTTP, there is no model binding</span></span>
* <span data-ttu-id="95d2f-148">Jsou přímo jako koncový bod HTTP není dostupná, jsou vyvolány z vašeho kódu (obvykle v zobrazení).</span><span class="sxs-lookup"><span data-stu-id="95d2f-148">Are not reachable directly as an HTTP endpoint, they are invoked from your code (usually in a view).</span></span> <span data-ttu-id="95d2f-149">Součást zobrazení nikdy zpracovává žádost</span><span class="sxs-lookup"><span data-stu-id="95d2f-149">A view component never handles a request</span></span>
* <span data-ttu-id="95d2f-150">Jsou přetížené na podpis a nikoli na všechny podrobnosti, z aktuální žádosti HTTP</span><span class="sxs-lookup"><span data-stu-id="95d2f-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="95d2f-151">Zobrazení – cesta hledání</span><span class="sxs-lookup"><span data-stu-id="95d2f-151">View search path</span></span>

<span data-ttu-id="95d2f-152">Modul runtime prohledá zobrazení do následující cesty:</span><span class="sxs-lookup"><span data-stu-id="95d2f-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="95d2f-153">Zobrazení nebo\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="95d2f-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="95d2f-154">Zobrazení nebo sdílené nebo součástí nebo\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="95d2f-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="95d2f-155">Výchozí název zobrazení pro součást zobrazení je *výchozí*, což znamená, že váš soubor zobrazení se obvykle nazývá *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95d2f-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="95d2f-156">Můžete zadat název jiné zobrazení, při vytváření komponenty výsledný objekt zobrazení, nebo při volání metody `View` metoda.</span><span class="sxs-lookup"><span data-stu-id="95d2f-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="95d2f-157">Doporučujeme název souboru zobrazení *Default.cshtml* a použít *zobrazení/sdílené nebo součástí nebo\<view_component_name > /\<view_name >* cesta.</span><span class="sxs-lookup"><span data-stu-id="95d2f-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="95d2f-158">`PriorityList` Používá zobrazení součástí používanou v této ukázce *Views/Shared/Components/PriorityList/Default.cshtml* pro zobrazení součásti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="95d2f-159">Vyvolání komponentu zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-159">Invoking a view component</span></span>

<span data-ttu-id="95d2f-160">Chcete-li použít komponentu zobrazení, volejte následující uvnitř zobrazení:</span><span class="sxs-lookup"><span data-stu-id="95d2f-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="95d2f-161">Parametry se předá `InvokeAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="95d2f-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="95d2f-162">`PriorityList` Zobrazení součásti vyvinuté v následujícím článku se volá z *Views/Todo/Index.cshtml* zobrazení souboru.</span><span class="sxs-lookup"><span data-stu-id="95d2f-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="95d2f-163">V následujícím příkladu `InvokeAsync` metoda je volána s dva parametry:</span><span class="sxs-lookup"><span data-stu-id="95d2f-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="95d2f-164">Vyvolání komponentu zobrazení jako značka pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="95d2f-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="95d2f-165">Pro technologii ASP.NET Core 1.1 a vyšší, můžete vyvolat součást zobrazení jako [značky pomocná](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="95d2f-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="95d2f-166">Jsou použita Pascal třídy a metody parametry pro značku Pomocníci přeložit na jejich [nižší případ kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="95d2f-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="95d2f-167">Používá značky pomocníka, který má vyvolat součást zobrazení `<vc></vc>` elementu.</span><span class="sxs-lookup"><span data-stu-id="95d2f-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="95d2f-168">Součást zobrazení je určena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="95d2f-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="95d2f-169">Poznámka: Pokud chcete používat komponenty zobrazení jako značka pomocné rutiny, je nutné zaregistrovat sestavení obsahující komponenty zobrazení pomocí `@addTagHelper` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="95d2f-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="95d2f-170">Například pokud příslušné součásti zobrazení v sestavení nazvané "MyWebApp", přidejte následující direktiva k `_ViewImports.cshtml` souboru:</span><span class="sxs-lookup"><span data-stu-id="95d2f-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="95d2f-171">Součást zobrazení můžete zaregistrovat jako značka pomocné rutiny do souboru, která odkazuje na komponentu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="95d2f-172">V tématu [Správa oboru pomocná značky](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Další informace o postupu při registraci značky pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="95d2f-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="95d2f-173">`InvokeAsync` Metodu použitou v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="95d2f-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="95d2f-174">V kódu pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="95d2f-174">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="95d2f-175">V ukázce výše `PriorityList` zobrazení součást se změní na `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="95d2f-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="95d2f-176">Parametry pro zobrazení součásti jsou předána jako atributy malá písmena kebab.</span><span class="sxs-lookup"><span data-stu-id="95d2f-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="95d2f-177">Vyvolání komponentu zobrazení přímo z řadiče</span><span class="sxs-lookup"><span data-stu-id="95d2f-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="95d2f-178">Zobrazení součásti jsou obvykle vyvolány ze zobrazení, ale je přímo z metody kontroleru můžete vyvolat.</span><span class="sxs-lookup"><span data-stu-id="95d2f-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="95d2f-179">Při zobrazení součásti nedefinují koncové body, jako jsou řadiče, můžete snadno implementovat akce kontroleru, který vrátí obsah `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="95d2f-179">While view components do not define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="95d2f-180">V tomto příkladu je přímo z řadiče volá komponentu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="95d2f-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="95d2f-181">Návod: Vytvoření jednoduché zobrazení komponenty</span><span class="sxs-lookup"><span data-stu-id="95d2f-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="95d2f-182">[Stáhněte si](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), vytvoření a testování počáteční kód.</span><span class="sxs-lookup"><span data-stu-id="95d2f-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="95d2f-183">Je jednoduchý projekt se `Todo` řadič, který zobrazí seznam *Todo* položky.</span><span class="sxs-lookup"><span data-stu-id="95d2f-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Seznam ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="95d2f-185">Přidání třídy ViewComponent</span><span class="sxs-lookup"><span data-stu-id="95d2f-185">Add a ViewComponent class</span></span>

<span data-ttu-id="95d2f-186">Vytvoření *ViewComponents* složky a přidejte následující `PriorityListViewComponent` třídy:</span><span class="sxs-lookup"><span data-stu-id="95d2f-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="95d2f-187">Poznámky k kód:</span><span class="sxs-lookup"><span data-stu-id="95d2f-187">Notes on the code:</span></span>

* <span data-ttu-id="95d2f-188">Třídy součásti zobrazení mohou být obsaženy v **žádné** složky v projektu.</span><span class="sxs-lookup"><span data-stu-id="95d2f-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="95d2f-189">Protože třída název PriorityList**ViewComponent** končí příponou **ViewComponent**, modul runtime použije řetězec "PriorityList" při odkazování na komponenty třídy ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="95d2f-190">I objasníme, který podrobněji později.</span><span class="sxs-lookup"><span data-stu-id="95d2f-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="95d2f-191">`[ViewComponent]` Atributu můžete změnit název slouží k odkazování komponentu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="95d2f-192">Například můžeme může mít s názvem třídy `XYZ` a použít `ViewComponent` atribut:</span><span class="sxs-lookup"><span data-stu-id="95d2f-192">For example, we could have named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="95d2f-193">`[ViewComponent]` Výše uvedený atribut informuje výběr zobrazení komponent pro použití názvu `PriorityList` při vyhledávání pro zobrazení související s komponentou a použití řetězce "PriorityList" při odkazování na komponenty třídy ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="95d2f-194">I objasníme, který podrobněji později.</span><span class="sxs-lookup"><span data-stu-id="95d2f-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="95d2f-195">Používá komponentu [vkládání závislostí](../../fundamentals/dependency-injection.md) chcete zpřístupnit data kontextu.</span><span class="sxs-lookup"><span data-stu-id="95d2f-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="95d2f-196">`InvokeAsync`zpřístupňuje metodu, která lze volat z zobrazení ale může trvat libovolný počet argumentů.</span><span class="sxs-lookup"><span data-stu-id="95d2f-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="95d2f-197">`InvokeAsync` Metoda vrací sadu `ToDo` položky, které odpovídají `isDone` a `maxPriority` parametry.</span><span class="sxs-lookup"><span data-stu-id="95d2f-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="95d2f-198">Vytvoření zobrazení syntaxe Razor součást zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-198">Create the view component Razor view</span></span>

* <span data-ttu-id="95d2f-199">Vytvořte *zobrazení/sdílené nebo součásti* složky.</span><span class="sxs-lookup"><span data-stu-id="95d2f-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="95d2f-200">Tato složka **musí** nazván *součásti*.</span><span class="sxs-lookup"><span data-stu-id="95d2f-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="95d2f-201">Vytvořte *zobrazení/sdílené nebo součástí nebo PriorityList* složky.</span><span class="sxs-lookup"><span data-stu-id="95d2f-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="95d2f-202">Tento název složky musí odpovídat názvu třídy součástí zobrazení, nebo název třídy minus přípona (Pokud jsme postupovali podle konvence a použít *ViewComponent* přípony v názvu třídy).</span><span class="sxs-lookup"><span data-stu-id="95d2f-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="95d2f-203">Pokud jste použili `ViewComponent` atribut název třídy potřebovat tak, aby odpovídaly označení atribut.</span><span class="sxs-lookup"><span data-stu-id="95d2f-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="95d2f-204">Vytvoření *Views/Shared/Components/PriorityList/Default.cshtml* Razor zobrazení:[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="95d2f-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="95d2f-205">Zobrazení syntaxe Razor přebírá seznam `TodoItem` a zobrazí je.</span><span class="sxs-lookup"><span data-stu-id="95d2f-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="95d2f-206">Pokud komponentu zobrazení `InvokeAsync` metoda neprojde název zobrazení (jako naše ukázka), *výchozí* slouží pro název zobrazení pomocí konvencí.</span><span class="sxs-lookup"><span data-stu-id="95d2f-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="95d2f-207">Později v tomto kurzu I budete ukazují, jak předat název zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="95d2f-208">Pokud chcete přepsat výchozí stylu k určitému kontroleru, přidat zobrazení do složky specifické řadiče zobrazení (například *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="95d2f-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="95d2f-209">Pokud komponentu zobrazení je specifický pro řadič, můžete ho přidat do složky pro konkrétní řadič (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="95d2f-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="95d2f-210">Přidat `div` obsahující volání součást Seznam s prioritou k dolnímu okraji *Views/Todo/index.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="95d2f-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="95d2f-211">Kód `@await Component.InvokeAsync` ukazuje syntaxi pro volání součásti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="95d2f-212">První argument je název součást, kterou chceme volání nebo volání.</span><span class="sxs-lookup"><span data-stu-id="95d2f-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="95d2f-213">Následující parametry jsou předány součásti.</span><span class="sxs-lookup"><span data-stu-id="95d2f-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="95d2f-214">`InvokeAsync`může trvat libovolný počet argumentů.</span><span class="sxs-lookup"><span data-stu-id="95d2f-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="95d2f-215">Testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="95d2f-215">Test the app.</span></span> <span data-ttu-id="95d2f-216">Následující obrázek znázorňuje seznamu úkolů a položky s prioritou:</span><span class="sxs-lookup"><span data-stu-id="95d2f-216">The following image shows the ToDo list and the priority items:</span></span>

![seznam a prioritu položek todo](view-components/_static/pi.png)

<span data-ttu-id="95d2f-218">Součást zobrazení můžete také volat přímo z řadiče:</span><span class="sxs-lookup"><span data-stu-id="95d2f-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![Priorita položky z IndexVC akce](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="95d2f-220">Zadejte název a zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-220">Specifying a view name</span></span>

<span data-ttu-id="95d2f-221">Komponentu komplexní zobrazení může být nutné zadat jiné než výchozí zobrazení za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="95d2f-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="95d2f-222">Následující kód ukazuje, jak určit "PVC" zobrazení `InvokeAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="95d2f-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="95d2f-223">Aktualizace `InvokeAsync` metoda v `PriorityListViewComponent` třídy.</span><span class="sxs-lookup"><span data-stu-id="95d2f-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="95d2f-224">Kopírování *Views/Shared/Components/PriorityList/Default.cshtml* soubor k zobrazení s názvem *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95d2f-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="95d2f-225">Přidáte záhlaví se indikovat, že zobrazení PVC se používá.</span><span class="sxs-lookup"><span data-stu-id="95d2f-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="95d2f-226">Aktualizace *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="95d2f-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="95d2f-227">Spusťte aplikaci a ověření PVC zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-227">Run the app and verify PVC view.</span></span>

![Součást zobrazení s prioritou](view-components/_static/pvc.png)

<span data-ttu-id="95d2f-229">Pokud není PVC zobrazení vykresleno, ověřte, zda že jsou volání komponentu zobrazení s prioritou 4 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="95d2f-229">If the PVC view is not rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="95d2f-230">Zkontrolujte cestu zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-230">Examine the view path</span></span>

* <span data-ttu-id="95d2f-231">Změňte parametr priority na tři nebo méně, nevrátí zobrazení s prioritou.</span><span class="sxs-lookup"><span data-stu-id="95d2f-231">Change the priority parameter to three or less so the priority view is not returned.</span></span>
* <span data-ttu-id="95d2f-232">Dočasně přejmenujte *Views/Todo/Components/PriorityList/Default.cshtml* k *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95d2f-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="95d2f-233">Testování aplikací, získáte následující chybě:</span><span class="sxs-lookup"><span data-stu-id="95d2f-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="95d2f-234">Kopírování *Views/Todo/Components/PriorityList/1Default.cshtml* k *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95d2f-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="95d2f-235">Přidat některé značky, aby *sdílené* Todo zobrazení součásti zobrazení udávajících zobrazení je z *sdílené* složky.</span><span class="sxs-lookup"><span data-stu-id="95d2f-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="95d2f-236">Testovací **sdílené** součásti zobrazení.</span><span class="sxs-lookup"><span data-stu-id="95d2f-236">Test the **Shared** component view.</span></span>

![Výstup úkolů s sdílené součásti zobrazení](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="95d2f-238">Zamezení magic řetězce</span><span class="sxs-lookup"><span data-stu-id="95d2f-238">Avoiding magic strings</span></span>

<span data-ttu-id="95d2f-239">Pokud chcete zkompilovat čas zabezpečení, můžete název komponenty pevně zobrazení nahradit název třídy.</span><span class="sxs-lookup"><span data-stu-id="95d2f-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="95d2f-240">Vytvořte komponentu zobrazení bez přípony "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="95d2f-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="95d2f-241">Přidat `using` příkaz, který má vaše Razor zobrazení souboru a použít `nameof` operátor:</span><span class="sxs-lookup"><span data-stu-id="95d2f-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="95d2f-242">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="95d2f-242">Additional Resources</span></span>

* [<span data-ttu-id="95d2f-243">Vkládání závislostí do zobrazení</span><span class="sxs-lookup"><span data-stu-id="95d2f-243">Dependency injection into views</span></span>](dependency-injection.md)
