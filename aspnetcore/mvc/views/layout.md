---
title: Rozložení v ASP.NET Core
author: ardalis
description: Zjistěte, jak používat společné rozložení, sdílet direktivy a spustit běžné kód před vykreslení zobrazení v aplikaci ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753339"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="2bb34-103">Rozložení v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2bb34-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="2bb34-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2bb34-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2bb34-105">Zobrazení často sdílení visual a programové prvků.</span><span class="sxs-lookup"><span data-stu-id="2bb34-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="2bb34-106">V tomto článku se dozvíte, jak používat společné rozložení, sdílet direktivy a spustit společný kód před vykreslení zobrazení v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bb34-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET Core app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="2bb34-107">Co je rozložení</span><span class="sxs-lookup"><span data-stu-id="2bb34-107">What is a Layout</span></span>

<span data-ttu-id="2bb34-108">Většina webových aplikací mají společné rozložení, který poskytuje uživatelům konzistentní prostředí při práci ze stránky na stránku.</span><span class="sxs-lookup"><span data-stu-id="2bb34-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="2bb34-109">Rozložení obvykle zahrnuje společné prvky uživatelského rozhraní, jako je například aplikace záhlaví, navigaci nebo prvky nabídky a zápatí.</span><span class="sxs-lookup"><span data-stu-id="2bb34-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Příklad rozložení stránky](layout/_static/page-layout.png)

<span data-ttu-id="2bb34-111">Společné struktury HTML, jako jsou skripty a šablony stylů také často používá mnoho stránek v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="2bb34-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="2bb34-112">Všechny tyto sdílené prvky mohou být definovány v *rozložení* soubor, který může odkazovat ve všech zobrazeních použít v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2bb34-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="2bb34-113">Rozložení snížit duplicitního kódu v zobrazeních, usnadňuje postupujte [není opakujte sami (zkušební) Princip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="2bb34-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="2bb34-114">Podle konvence je výchozí rozložení aplikace ASP.NET Core s názvem `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="2bb34-114">By convention, the default layout for an ASP.NET Core app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="2bb34-115">Šablony projektů Visual Studio ASP.NET Core MVC zahrnuje tento soubor rozložení `Views/Shared` složky:</span><span class="sxs-lookup"><span data-stu-id="2bb34-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![zobrazení složky v Průzkumníku řešení](layout/_static/web-project-views.png)

<span data-ttu-id="2bb34-117">Toto rozložení definuje šablonu nejvyšší úrovně pro zobrazení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2bb34-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="2bb34-118">Aplikace nevyžadují rozložení a aplikací můžete definovat více než jedno rozložení s různá zobrazení zadání různá rozložení.</span><span class="sxs-lookup"><span data-stu-id="2bb34-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="2bb34-119">Příklad `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="2bb34-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="2bb34-120">Určení rozložení</span><span class="sxs-lookup"><span data-stu-id="2bb34-120">Specifying a Layout</span></span>

<span data-ttu-id="2bb34-121">Zobrazení Razor je k dispozici `Layout` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2bb34-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="2bb34-122">Jednotlivá zobrazení zadat rozložení tak, že nastavíte tuto vlastnost:</span><span class="sxs-lookup"><span data-stu-id="2bb34-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="2bb34-123">Zadané rozložení můžete použít úplnou cestu (Příklad: `/Views/Shared/_Layout.cshtml`) nebo částečný název (Příklad: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="2bb34-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="2bb34-124">Když částečný název zadán, bude hledat rozložení souboru pomocí jeho procesu zjišťování standardní zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="2bb34-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="2bb34-125">Složky spojené kontroleru je nejprve prohledán, za nímž následuje `Shared` složky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="2bb34-126">Tento proces zjišťování je stejný jako ten, který používá ke zjišťování [částečná zobrazení](partial.md).</span><span class="sxs-lookup"><span data-stu-id="2bb34-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="2bb34-127">Ve výchozím nastavení, musí volat každou rozložení `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="2bb34-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="2bb34-128">Všude, kde volání `RenderBody` je umístěn, bude vykreslen obsah zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2bb34-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="2bb34-129">Oddíly</span><span class="sxs-lookup"><span data-stu-id="2bb34-129">Sections</span></span>

<span data-ttu-id="2bb34-130">Rozložení můžete volitelně odkazovat na jeden nebo více *oddíly*, voláním `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="2bb34-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="2bb34-131">Oddíly umožňují organizovat umístění některých prvků stránky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="2bb34-132">Každé volání `RenderSection` můžete určit, zda je tento oddíl požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="2bb34-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="2bb34-133">Pokud není nalezen požadovaný oddíl, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2bb34-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="2bb34-134">Jednotlivá zobrazení zadejte obsah, který mohl být vykreslen v rámci oddílu pomocí `@section` syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="2bb34-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="2bb34-135">Pokud zobrazení určující sekci, je nutné vykreslit (nebo dojde k chybě).</span><span class="sxs-lookup"><span data-stu-id="2bb34-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="2bb34-136">Příklad `@section` definice v zobrazení:</span><span class="sxs-lookup"><span data-stu-id="2bb34-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="2bb34-137">Ve výše uvedeném kódu, skripty pro ověření jsou přidány do `scripts` části zobrazení, která obsahuje formulář.</span><span class="sxs-lookup"><span data-stu-id="2bb34-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="2bb34-138">Ostatní zobrazení ve stejné aplikaci nemusí potřebovat další skripty a tak nebude potřeba nadefinovat oddíl skripty.</span><span class="sxs-lookup"><span data-stu-id="2bb34-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="2bb34-139">Oddíly definované v zobrazení jsou k dispozici pouze v její okamžitý rozložení stránky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="2bb34-140">Nelze se odkazovat z částečných zobrazení, zobrazení komponenty nebo jiných částí systému zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2bb34-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="2bb34-141">Oddíly se ignoruje</span><span class="sxs-lookup"><span data-stu-id="2bb34-141">Ignoring sections</span></span>

<span data-ttu-id="2bb34-142">Ve výchozím nastavení text a všechny části na stránce obsahu musí všechny zobrazovat na stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="2bb34-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="2bb34-143">Zobrazovací modul Razor vynucuje Díky sledování, zda byl vykreslen text a každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="2bb34-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="2bb34-144">Chcete-li dát pokyn zobrazovací modul, aby ignoroval text nebo oddíly, zavolejte `IgnoreBody` a `IgnoreSection` metody.</span><span class="sxs-lookup"><span data-stu-id="2bb34-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="2bb34-145">Text a každý oddíl na stránce Razor musí být buď vykreslen nebo ignorovat.</span><span class="sxs-lookup"><span data-stu-id="2bb34-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="2bb34-146">Import sdílených direktivy</span><span class="sxs-lookup"><span data-stu-id="2bb34-146">Importing Shared Directives</span></span>

<span data-ttu-id="2bb34-147">Zobrazení pomocí direktivy Razor můžete provést řadu věcí, například obory názvů pro import nebo provádění [injektáž závislostí](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="2bb34-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="2bb34-148">Direktivy, které sdílí mnoho zobrazení je možné zadat běžný `_ViewImports.cshtml` souboru.</span><span class="sxs-lookup"><span data-stu-id="2bb34-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="2bb34-149">`_ViewImports` Soubor podporuje následující direktivy:</span><span class="sxs-lookup"><span data-stu-id="2bb34-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="2bb34-150">Tento soubor nepodporuje další funkce Razor, jako je například funkce a definice části.</span><span class="sxs-lookup"><span data-stu-id="2bb34-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="2bb34-151">Ukázka `_ViewImports.cshtml` souboru:</span><span class="sxs-lookup"><span data-stu-id="2bb34-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="2bb34-152">`_ViewImports.cshtml` Soubor pro aplikaci ASP.NET Core MVC je obvykle umístěn ve `Views` složky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="2bb34-153">A `_ViewImports.cshtml` souboru je možné použít v jakékoli složce, ve kterém případu, použijí se jenom u zobrazení v této složce a jejích podsložkách.</span><span class="sxs-lookup"><span data-stu-id="2bb34-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="2bb34-154">`_ViewImports` zpracování souborů začínají na kořenové úrovni, a potom pro každou složku k umístění zobrazení, tak nastavení zadané na kořenové úrovni lze přepsat na úrovni složek.</span><span class="sxs-lookup"><span data-stu-id="2bb34-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="2bb34-155">Například, pokud kořenové úrovni `_ViewImports.cshtml` soubor Určuje `@model` a `@addTagHelper`a další `_ViewImports.cshtml` určuje jiný soubor ve složce přidružené kontroleru zobrazení `@model` a přidá další `@addTagHelper`, zobrazení bude mít přístup k oběma pomocných rutin značek a bude používat ten `@model`.</span><span class="sxs-lookup"><span data-stu-id="2bb34-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="2bb34-156">Pokud je položek víc `_ViewImports.cshtml` soubory jsou spuštěny pro zobrazení, kombinované chování direktiv součástí `ViewImports.cshtml` soubory budou následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2bb34-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="2bb34-157">`@addTagHelper`, `@removeTagHelper`: všech spuštění, v pořadí</span><span class="sxs-lookup"><span data-stu-id="2bb34-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="2bb34-158">`@tagHelperPrefix`: na ten nejbližší do zobrazení přepíše všechny ostatní</span><span class="sxs-lookup"><span data-stu-id="2bb34-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="2bb34-159">`@model`: na ten nejbližší do zobrazení přepíše všechny ostatní</span><span class="sxs-lookup"><span data-stu-id="2bb34-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="2bb34-160">`@inherits`: na ten nejbližší do zobrazení přepíše všechny ostatní</span><span class="sxs-lookup"><span data-stu-id="2bb34-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="2bb34-161">`@using`: všechny jsou zahrnuty; duplicity se ignorují.</span><span class="sxs-lookup"><span data-stu-id="2bb34-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="2bb34-162">`@inject`: pro každou vlastnost na ten nejbližší do zobrazení přepíše všechny ostatní se stejným názvem vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2bb34-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="2bb34-163">Spuštění kódu před každou zobrazení</span><span class="sxs-lookup"><span data-stu-id="2bb34-163">Running Code Before Each View</span></span>

<span data-ttu-id="2bb34-164">Pokud máte kód, je potřeba spustit před každé zobrazení, musí být umístěné ve `_ViewStart.cshtml` souboru.</span><span class="sxs-lookup"><span data-stu-id="2bb34-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="2bb34-165">Podle konvence `_ViewStart.cshtml` soubor se nachází v `Views` složky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="2bb34-166">Příkazy uvedené v `_ViewStart.cshtml` jsou spouštěny před každou úplné zobrazení (nikoli rozložení a není částečná zobrazení).</span><span class="sxs-lookup"><span data-stu-id="2bb34-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="2bb34-167">Stejně jako [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` jsou hierarchická.</span><span class="sxs-lookup"><span data-stu-id="2bb34-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="2bb34-168">Pokud `_ViewStart.cshtml` soubor je definován ve složce přidružené kontroleru zobrazení, se spustí po definovanému v kořenovém adresáři `Views` složku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="2bb34-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="2bb34-169">Ukázka `_ViewStart.cshtml` souboru:</span><span class="sxs-lookup"><span data-stu-id="2bb34-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="2bb34-170">Výše uvedeného souboru Určuje, zda budou používat všechna zobrazení `_Layout.cshtml` rozložení.</span><span class="sxs-lookup"><span data-stu-id="2bb34-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb34-171">Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` jsou obvykle umístěny v `/Views/Shared` složky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="2bb34-172">Verze těchto souborů úrovni aplikace by měl být umístěn v přímo `/Views` složky.</span><span class="sxs-lookup"><span data-stu-id="2bb34-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
