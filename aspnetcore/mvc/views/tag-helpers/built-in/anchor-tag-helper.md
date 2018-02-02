---
title: "Pomocník značka ukotvení"
author: pkellner
description: "Zjistit ASP.NET pomocné rutiny značka ukotvení základní atributy a roli, kterou každý atribut hraje v rozšíření chování značky HTML anchor."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="289b4-103">Pomocník značka ukotvení</span><span class="sxs-lookup"><span data-stu-id="289b4-103">Anchor Tag Helper</span></span>

<span data-ttu-id="289b4-104">Podle [Petr Kellner](http://peterkellner.net) a [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="289b4-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="289b4-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="289b4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="289b4-106">[Pomocná značka ukotvení](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) rozšiřuje standardní anchor HTML (`<a ... ></a>`) značky přidáním nové atributy.</span><span class="sxs-lookup"><span data-stu-id="289b4-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="289b4-107">Podle konvence, mají předponu názvy atributů `asp-`.</span><span class="sxs-lookup"><span data-stu-id="289b4-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="289b4-108">Element anchor vykreslené `href` hodnota atributu je určen podle hodnot `asp-` atributy.</span><span class="sxs-lookup"><span data-stu-id="289b4-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="289b4-109">*SpeakerController* se používá v ukázky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="289b4-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="289b4-110">Inventář `asp-` atributy způsobem.</span><span class="sxs-lookup"><span data-stu-id="289b4-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="289b4-111">ASP-controller</span><span class="sxs-lookup"><span data-stu-id="289b4-111">asp-controller</span></span>

<span data-ttu-id="289b4-112">[Asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) atribut přiřadí o řadič ke generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="289b4-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="289b4-113">Následující kód obsahuje seznam všech mluvčí:</span><span class="sxs-lookup"><span data-stu-id="289b4-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="289b4-114">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="289b4-115">Pokud `asp-controller` zadán atribut a `asp-action` není výchozí `asp-action` hodnota je přidružené k aktuálně prováděné zobrazení akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="289b4-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="289b4-116">Pokud `asp-action` je vynechaný předchozí kód a pomocné značka ukotvení se používá v *HomeController*na *Index* zobrazení (*/Home*), je generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="289b4-117">Akce ASP</span><span class="sxs-lookup"><span data-stu-id="289b4-117">asp-action</span></span>

<span data-ttu-id="289b4-118">[Asp akce](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) hodnota atributu představuje název akce kontroleru součástí vygenerovaného `href` atribut.</span><span class="sxs-lookup"><span data-stu-id="289b4-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="289b4-119">Následující kód nastaví vygenerovaného `href` hodnota atributu na stránku hodnocení mluvčího:</span><span class="sxs-lookup"><span data-stu-id="289b4-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="289b4-120">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="289b4-121">Pokud žádné `asp-controller` atribut je určena, bude použita výchozí kontroleru volání zobrazení provádění aktuálního zobrazení.</span><span class="sxs-lookup"><span data-stu-id="289b4-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="289b4-122">Pokud `asp-action` hodnota atributu je `Index`, pak žádná akce je připojena k adrese URL, vedoucí k vyvolání výchozí `Index` akce.</span><span class="sxs-lookup"><span data-stu-id="289b4-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="289b4-123">Akce zadané (nebo uvedena), musí existovat v kontroleru, kterou se odkazuje v `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="289b4-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="289b4-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="289b4-124">asp-route-{value}</span></span>

<span data-ttu-id="289b4-125">[Asp - trasy-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atribut umožňuje předponu trasy zástupný znak.</span><span class="sxs-lookup"><span data-stu-id="289b4-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="289b4-126">Všechny hodnoty zabírá `{value}` zástupný symbol interpretována jako potenciální parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="289b4-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="289b4-127">Pokud není nalezen výchozí trasu, připojí se tato předpona trasy k vygenerovaného `href` atribut jako parametr žádosti a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="289b4-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="289b4-128">Jinak je nahrazena v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="289b4-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="289b4-129">Vezměte v úvahu následující akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="289b4-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="289b4-130">Pomocí výchozí šablony trasy definované v *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="289b4-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="289b4-131">Zobrazení MVC používá model, poskytuje akce, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="289b4-131">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="289b4-132">Výchozí trasu `{id?}` , odpovídal zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="289b4-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="289b4-133">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="289b4-134">Předpokládejme, že předpona trasy není součástí odpovídající směrování šablony, stejně jako u následující zobrazení MVC:</span><span class="sxs-lookup"><span data-stu-id="289b4-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="289b4-135">Následující kód HTML je vygenerovat, protože `speakerid` nebyl nalezen v odpovídající trasy:</span><span class="sxs-lookup"><span data-stu-id="289b4-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="289b4-136">Pokud má jedna `asp-controller` nebo `asp-action` nejsou zadané, pak stejné zpracování výchozí je následovaný, protože `asp-route` atribut.</span><span class="sxs-lookup"><span data-stu-id="289b4-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="289b4-137">ASP trasy</span><span class="sxs-lookup"><span data-stu-id="289b4-137">asp-route</span></span>

<span data-ttu-id="289b4-138">[Asp trasy](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) atribut se používá pro vytvoření adresy URL připojení přímo k pojmenovanou trasu.</span><span class="sxs-lookup"><span data-stu-id="289b4-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="289b4-139">Pomocí [atributy směrování](xref:mvc/controllers/routing#attribute-routing), může mít název trasy, jak je znázorněno `SpeakerController` a používá v jeho `Evaluations` akce:</span><span class="sxs-lookup"><span data-stu-id="289b4-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="289b4-140">V následující kód `asp-route` pojmenovanou trasu odkazuje atribut:</span><span class="sxs-lookup"><span data-stu-id="289b4-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="289b4-141">Pomocník značka ukotvení vytváří trasu přímo k této akci kontroleru pomocí adresy URL */mluvčího/hodnocení*.</span><span class="sxs-lookup"><span data-stu-id="289b4-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="289b4-142">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="289b4-143">Pokud `asp-controller` nebo `asp-action` je zadán kromě `asp-route`, postupu generované nemusí být očekávat.</span><span class="sxs-lookup"><span data-stu-id="289b4-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="289b4-144">Aby nedošlo ke konfliktu trasy `asp-route` by neměl být použit s `asp-controller` a `asp-action` atributy.</span><span class="sxs-lookup"><span data-stu-id="289b4-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="289b4-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="289b4-145">asp-all-route-data</span></span>

<span data-ttu-id="289b4-146">[Asp všechny trasy dat](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atribut podporuje vytváření slovník páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="289b4-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="289b4-147">Klíč je název parametru a hodnota je hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="289b4-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="289b4-148">V následujícím příkladu je slovník inicializovat a předána do zobrazení Razor.</span><span class="sxs-lookup"><span data-stu-id="289b4-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="289b4-149">Alternativně může být předán data pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="289b4-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="289b4-150">Předchozí kód generuje následující HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="289b4-151">`asp-all-route-data` Slovníku se sloučí k vytvoření řetězec dotazu, který splňuje požadavky přetížené `Evaluations` akce:</span><span class="sxs-lookup"><span data-stu-id="289b4-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="289b4-152">Pokud se ve slovníku všechny klíče shodují parametry trasy, tyto hodnoty jsou nahrazena v postupu podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="289b4-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="289b4-153">Neodpovídající hodnoty jsou generovány jako parametry žádosti.</span><span class="sxs-lookup"><span data-stu-id="289b4-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="289b4-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="289b4-154">asp-fragment</span></span>

<span data-ttu-id="289b4-155">[Asp fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) atribut definuje fragment adresy URL pro připojení k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="289b4-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="289b4-156">Pomocník značka ukotvení přidá znak hash (#).</span><span class="sxs-lookup"><span data-stu-id="289b4-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="289b4-157">Vezměte v úvahu následující kód:</span><span class="sxs-lookup"><span data-stu-id="289b4-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="289b4-158">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="289b4-159">Hodnota hash značky jsou užitečné při vytváření aplikace na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="289b4-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="289b4-160">Mohou být použity pro snadné označování a hledání v jazyce JavaScript, třeba.</span><span class="sxs-lookup"><span data-stu-id="289b4-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="289b4-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="289b4-161">asp-area</span></span>

<span data-ttu-id="289b4-162">[Asp oblasti](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) atribut nastaví název oblasti, použije se k nastavení odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="289b4-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="289b4-163">Následující příklad znázorňuje, jak atribut oblasti způsobí přemapování trasy.</span><span class="sxs-lookup"><span data-stu-id="289b4-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="289b4-164">Nastavení `asp-area` na "Blogy" předpony adresáři *oblasti nebo blogy* pro trasy přidružené kontrolery a zobrazení pro tuto značku ukotvení.</span><span class="sxs-lookup"><span data-stu-id="289b4-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="289b4-165">**< název projektu\>**</span><span class="sxs-lookup"><span data-stu-id="289b4-165">**<Project name\>**</span></span>
  * <span data-ttu-id="289b4-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="289b4-166">**wwwroot**</span></span>
  * <span data-ttu-id="289b4-167">**Oblasti**</span><span class="sxs-lookup"><span data-stu-id="289b4-167">**Areas**</span></span>
    * <span data-ttu-id="289b4-168">**Blogy**</span><span class="sxs-lookup"><span data-stu-id="289b4-168">**Blogs**</span></span>
      * <span data-ttu-id="289b4-169">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="289b4-169">**Controllers**</span></span>
        * <span data-ttu-id="289b4-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="289b4-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="289b4-171">**Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="289b4-171">**Views**</span></span>
        * <span data-ttu-id="289b4-172">**Domovské**</span><span class="sxs-lookup"><span data-stu-id="289b4-172">**Home**</span></span>
          * <span data-ttu-id="289b4-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="289b4-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="289b4-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="289b4-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="289b4-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="289b4-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="289b4-176">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="289b4-176">**Controllers**</span></span>

<span data-ttu-id="289b4-177">Zadané předchozí hierarchii adresářů, kód tak, aby odkazovaly *AboutBlog.cshtml* souboru je:</span><span class="sxs-lookup"><span data-stu-id="289b4-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="289b4-178">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="289b4-179">Pro oblasti pro práci v aplikaci MVC musí šablona trasy obsahovat odkaz na oblasti, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="289b4-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="289b4-180">Tato šablona je reprezentována druhý parametr `routes.MapRoute` volání metody *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="289b4-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="289b4-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="289b4-181">asp-protocol</span></span>

<span data-ttu-id="289b4-182">[Asp protokol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) atribut je pro zadání protokol (například `https`) v svoji adresu URL.</span><span class="sxs-lookup"><span data-stu-id="289b4-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="289b4-183">Příklad:</span><span class="sxs-lookup"><span data-stu-id="289b4-183">For example:</span></span>

<span data-ttu-id="289b4-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span><span class="sxs-lookup"><span data-stu-id="289b4-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span></span>

<span data-ttu-id="289b4-185">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="289b4-186">Název hostitele v příkladu je localhost, ale pomocný značka ukotvení používá webu veřejné domény při generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="289b4-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="289b4-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="289b4-187">asp-host</span></span>

<span data-ttu-id="289b4-188">[Asp hostitele](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) atribut je pro zadání názvu hostitele v svoji adresu URL.</span><span class="sxs-lookup"><span data-stu-id="289b4-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="289b4-189">Příklad:</span><span class="sxs-lookup"><span data-stu-id="289b4-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="289b4-190">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="289b4-191">stránka ASP</span><span class="sxs-lookup"><span data-stu-id="289b4-191">asp-page</span></span>

<span data-ttu-id="289b4-192">[Stránka asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) atribut se používá s stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="289b4-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="289b4-193">Použijte je k nastavení značku ukotvení `href` hodnota atributu na konkrétní stránku.</span><span class="sxs-lookup"><span data-stu-id="289b4-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="289b4-194">Prefixu název stránky s lomítkem ("/") vytvoří adresu URL.</span><span class="sxs-lookup"><span data-stu-id="289b4-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="289b4-195">Následující příklad ukazuje na účastníka Razor stránky:</span><span class="sxs-lookup"><span data-stu-id="289b4-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="289b4-196">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="289b4-197">`asp-page` Atribut je vzájemně se vylučuje s `asp-route`, `asp-controller`, a `asp-action` atributy.</span><span class="sxs-lookup"><span data-stu-id="289b4-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="289b4-198">Ale `asp-page` lze použít s `asp-route-{value}` řídit směrování, jak ukazuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="289b4-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="289b4-199">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="289b4-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="289b4-200">asp-page-handler</span></span>

<span data-ttu-id="289b4-201">[Rutina stránky asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) atribut se používá s stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="289b4-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="289b4-202">Je určený pro propojení na konkrétní stránky obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="289b4-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="289b4-203">Vezměte v úvahu obslužná rutina následující stránky:</span><span class="sxs-lookup"><span data-stu-id="289b4-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="289b4-204">Model stránky přidruženému značek odkazy na `OnGetProfile` obslužná rutina stránky.</span><span class="sxs-lookup"><span data-stu-id="289b4-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="289b4-205">Všimněte si, že `On<Verb>` předpona názvu stránky obslužná rutina metoda je vynechán v `asp-page-handler` hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="289b4-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="289b4-206">Pokud to byly asynchronní metodu, `Async` přípona by být příliš vynechán.</span><span class="sxs-lookup"><span data-stu-id="289b4-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="289b4-207">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="289b4-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="289b4-208">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="289b4-208">Additional resources</span></span>

* [<span data-ttu-id="289b4-209">Oblasti</span><span class="sxs-lookup"><span data-stu-id="289b4-209">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="289b4-210">Úvod do stránky Razor</span><span class="sxs-lookup"><span data-stu-id="289b4-210">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
