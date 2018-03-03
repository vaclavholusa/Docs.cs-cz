---
title: "Rozložení"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 140ee5a9b43ee952e46795e433ceb1553a6da906
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="layout"></a><span data-ttu-id="333cd-102">Rozložení</span><span class="sxs-lookup"><span data-stu-id="333cd-102">Layout</span></span>

<span data-ttu-id="333cd-103">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="333cd-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="333cd-104">Zobrazení často sdílet visual a programovací elementy.</span><span class="sxs-lookup"><span data-stu-id="333cd-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="333cd-105">V tomto článku se naučíte používat běžné rozložení, sdílet direktivy a spustit běžné kód před vykreslování zobrazení v aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="333cd-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="333cd-106">Co je rozložení</span><span class="sxs-lookup"><span data-stu-id="333cd-106">What is a Layout</span></span>

<span data-ttu-id="333cd-107">Většina webových aplikací mít běžné rozložení, které poskytuje uživateli konzistentního prostředí jako stisku stránky.</span><span class="sxs-lookup"><span data-stu-id="333cd-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="333cd-108">Rozložení obvykle obsahuje společné elementy uživatelského rozhraní, například záhlaví aplikace, navigace nebo nabídky elementy a zápatí stránky.</span><span class="sxs-lookup"><span data-stu-id="333cd-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Příklad rozložení stránky](layout/_static/page-layout.png)

<span data-ttu-id="333cd-110">Počet stránek v aplikaci také často používají společné struktury HTML, například skriptů a šablon.</span><span class="sxs-lookup"><span data-stu-id="333cd-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="333cd-111">Všechny tyto sdílené prvky může být definována v *rozložení* souboru, který může odkazovat potom všechna zobrazení používaných v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="333cd-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="333cd-112">Rozložení snížit duplicitní kódu v zobrazeních, pomoci jim postupujte podle kroků [nemáte opakujte sami (suchého) Princip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="333cd-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="333cd-113">Podle konvence je výchozí rozložení pro aplikaci ASP.NET s názvem `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="333cd-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="333cd-114">Šablona projektu Visual Studio jádro ASP.NET MVC zahrnuje tento soubor rozložení `Views/Shared` složky:</span><span class="sxs-lookup"><span data-stu-id="333cd-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![zobrazení složky v Průzkumníku řešení](layout/_static/web-project-views.png)

<span data-ttu-id="333cd-116">Toto rozložení definuje šablonu nejvyšší úrovně pro zobrazení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="333cd-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="333cd-117">Aplikace nevyžadují rozložení a aplikace můžete definovat více než jedno rozložení s různá zobrazení určení různých rozložení.</span><span class="sxs-lookup"><span data-stu-id="333cd-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="333cd-118">Příklad `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="333cd-118">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="333cd-119">Určení rozložení</span><span class="sxs-lookup"><span data-stu-id="333cd-119">Specifying a Layout</span></span>

<span data-ttu-id="333cd-120">Zobrazení syntaxe Razor je k dispozici `Layout` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="333cd-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="333cd-121">Jednotlivá zobrazení zadejte rozložení nastavením této vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="333cd-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="333cd-122">Rozložení zadaný můžete použít úplnou cestu (Příklad: `/Views/Shared/_Layout.cshtml`) nebo částečný název (Příklad: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="333cd-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="333cd-123">Pokud je zadaný částečný název, bude zobrazovací modul Razor hledat soubor rozložení pomocí jeho procesu zjišťování standardní.</span><span class="sxs-lookup"><span data-stu-id="333cd-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="333cd-124">Řadič spojená je prohledána složka nejprve následuje `Shared` složky.</span><span class="sxs-lookup"><span data-stu-id="333cd-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="333cd-125">Tento proces zjišťování je stejný jako ten, který používá ke zjišťování [částečná zobrazení](partial.md).</span><span class="sxs-lookup"><span data-stu-id="333cd-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="333cd-126">Ve výchozím nastavení, musí volat každé rozložení `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="333cd-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="333cd-127">Kdekoli volání `RenderBody` je umístěna, bude vykreslen obsah zobrazení.</span><span class="sxs-lookup"><span data-stu-id="333cd-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="333cd-128">Oddíly</span><span class="sxs-lookup"><span data-stu-id="333cd-128">Sections</span></span>

<span data-ttu-id="333cd-129">Rozložení můžete volitelně odkazovat na jeden nebo více *části*, voláním `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="333cd-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="333cd-130">Oddíly umožňují uspořádat, kde má být umístěn některé prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="333cd-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="333cd-131">Každé volání `RenderSection` můžete určit, zda je daný oddíl požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="333cd-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="333cd-132">Pokud není nalezen požadovaný oddíl, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="333cd-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="333cd-133">Jednotlivá zobrazení zadejte obsah, mají být zpracovány v oddílu pomocí `@section` syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="333cd-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="333cd-134">Pokud zobrazení určující sekci, musí být vykreslen (nebo dojde k chybě).</span><span class="sxs-lookup"><span data-stu-id="333cd-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="333cd-135">Příklad `@section` definice v zobrazení:</span><span class="sxs-lookup"><span data-stu-id="333cd-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="333cd-136">Ve výše uvedeném kódu, ověřování skriptů jsou přidány do `scripts` část zobrazení, která zahrnuje formuláře.</span><span class="sxs-lookup"><span data-stu-id="333cd-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="333cd-137">Ostatní zobrazení ve stejné aplikaci, nemusí vyžadovat další skripty a proto nebude muset definovat skripty oddíl.</span><span class="sxs-lookup"><span data-stu-id="333cd-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="333cd-138">Oddíly, které jsou definované v zobrazení jsou k dispozici pouze v jeho okamžitou rozložení stránky.</span><span class="sxs-lookup"><span data-stu-id="333cd-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="333cd-139">Se nelze odkazovat z částečné., zobrazení součásti nebo dalšími částmi systému zobrazení.</span><span class="sxs-lookup"><span data-stu-id="333cd-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="333cd-140">Ignoruje části</span><span class="sxs-lookup"><span data-stu-id="333cd-140">Ignoring sections</span></span>

<span data-ttu-id="333cd-141">Ve výchozím nastavení text a všechny části obsahu stránce musí všechny pro vykreslení ke stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="333cd-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="333cd-142">Zobrazovací modul Razor vynucuje tato sledování, zda subjektem a každá část byl vykreslen.</span><span class="sxs-lookup"><span data-stu-id="333cd-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="333cd-143">Chcete-li určit, aby modul zobrazení ignorovat textu nebo částech, zavolejte `IgnoreBody` a `IgnoreSection` metody.</span><span class="sxs-lookup"><span data-stu-id="333cd-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="333cd-144">Text a každé části na stránce Razor musí být buď vykresluje nebo ignorovat.</span><span class="sxs-lookup"><span data-stu-id="333cd-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="333cd-145">Import sdílených direktivy</span><span class="sxs-lookup"><span data-stu-id="333cd-145">Importing Shared Directives</span></span>

<span data-ttu-id="333cd-146">Zobrazení pomocí direktivy Razor můžete provést celou řadu věcí, jako je například import obory názvů nebo provádění [vkládání závislostí](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="333cd-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="333cd-147">Direktivy sdíleny mnoho zobrazení je možné zadat společného `_ViewImports.cshtml` souboru.</span><span class="sxs-lookup"><span data-stu-id="333cd-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="333cd-148">`_ViewImports` Soubor podporuje následující direktivy:</span><span class="sxs-lookup"><span data-stu-id="333cd-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="333cd-149">Soubor nepodporuje jiných součástí Razor, například funkce a definice části.</span><span class="sxs-lookup"><span data-stu-id="333cd-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="333cd-150">Ukázka `_ViewImports.cshtml` souboru:</span><span class="sxs-lookup"><span data-stu-id="333cd-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="333cd-151">`_ViewImports.cshtml` Souboru u aplikace ASP.NET MVC základní obvykle umístěny v `Views` složky.</span><span class="sxs-lookup"><span data-stu-id="333cd-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="333cd-152">A `_ViewImports.cshtml` soubor může být umístěn v rámci libovolné složky, ve kterém případu, uplatní se jenom u zobrazení v této složce a jejích podsložkách.</span><span class="sxs-lookup"><span data-stu-id="333cd-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="333cd-153">`_ViewImports` zpracování souborů začínající na kořenové úrovni a potom pro každou složku plán pro umístění zobrazení sebe, tak na kořenové úrovni zadané nastavení může být přepsána na úrovni složek.</span><span class="sxs-lookup"><span data-stu-id="333cd-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="333cd-154">Například, pokud kořenové úrovni `_ViewImports.cshtml` soubor Určuje `@model` a `@addTagHelper`a jiné `_ViewImports.cshtml` soubor ve složce přidružené řadiče zobrazení určuje jinou `@model` a přidá další `@addTagHelper`, zobrazení bude mít přístup k Pomocníci obě značky a bude používat k tomu `@model`.</span><span class="sxs-lookup"><span data-stu-id="333cd-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="333cd-155">Pokud více `_ViewImports.cshtml` soubory spouštějí pro zobrazení, kombinaci chování direktivy součástí `ViewImports.cshtml` soubory bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="333cd-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="333cd-156">`@addTagHelper`, `@removeTagHelper`: všechny spuštění, v pořadí</span><span class="sxs-lookup"><span data-stu-id="333cd-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="333cd-157">`@tagHelperPrefix`: byl nejblíže k zobrazení přepíše všechny ostatní</span><span class="sxs-lookup"><span data-stu-id="333cd-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="333cd-158">`@model`: byl nejblíže k zobrazení přepíše všechny ostatní</span><span class="sxs-lookup"><span data-stu-id="333cd-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="333cd-159">`@inherits`: byl nejblíže k zobrazení přepíše všechny ostatní</span><span class="sxs-lookup"><span data-stu-id="333cd-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="333cd-160">`@using`: všechny jsou zahrnuty; duplicity se ignorují.</span><span class="sxs-lookup"><span data-stu-id="333cd-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="333cd-161">`@inject`: pro každou vlastnost nejbližší jeden k zobrazení přepíše všechny ostatní se stejným názvem vlastnosti</span><span class="sxs-lookup"><span data-stu-id="333cd-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="333cd-162">Spuštění kódu před každou zobrazení</span><span class="sxs-lookup"><span data-stu-id="333cd-162">Running Code Before Each View</span></span>

<span data-ttu-id="333cd-163">Pokud máte kód budete muset spustit před každé zobrazení, mají být umístěny do `_ViewStart.cshtml` souboru.</span><span class="sxs-lookup"><span data-stu-id="333cd-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="333cd-164">Podle konvence `_ViewStart.cshtml` soubor je umístěný ve `Views` složky.</span><span class="sxs-lookup"><span data-stu-id="333cd-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="333cd-165">Příkazy uvedené v `_ViewStart.cshtml` spouštějí před každé úplné zobrazení (není rozložení a není částečná zobrazení).</span><span class="sxs-lookup"><span data-stu-id="333cd-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="333cd-166">Jako [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` je hierarchická.</span><span class="sxs-lookup"><span data-stu-id="333cd-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="333cd-167">Pokud `_ViewStart.cshtml` soubor je definované ve složce přidružené řadiče zobrazení, se spustí po definovanému v kořenovém `Views` složku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="333cd-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="333cd-168">Ukázka `_ViewStart.cshtml` souboru:</span><span class="sxs-lookup"><span data-stu-id="333cd-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="333cd-169">Výše uvedeného souboru Určuje, že bude používat všechna zobrazení `_Layout.cshtml` rozložení.</span><span class="sxs-lookup"><span data-stu-id="333cd-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="333cd-170">Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` jsou obvykle umístěny v `/Views/Shared` složky.</span><span class="sxs-lookup"><span data-stu-id="333cd-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="333cd-171">Verze těchto souborů úrovni aplikace musí být umístěny přímo v `/Views` složky.</span><span class="sxs-lookup"><span data-stu-id="333cd-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
