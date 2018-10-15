---
title: Ukotvení pomocné rutiny značky v ASP.NET Core
author: pkellner
description: Objevte atributy ASP.NET Core ukotvení pomocné rutiny značky a roli, kterou každý atribut hraje v rozšíření chování značky jazyka HTML anchor.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 13508729c1e3b64a8b0e6965da57880738ab85c3
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325546"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="26fc1-103">Ukotvení pomocné rutiny značky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26fc1-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="26fc1-104">Podle [Peter Kellner](http://peterkellner.net) a [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="26fc1-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="26fc1-105">[Ukotvení pomocné rutiny značky](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) zvyšuje standard HTML anchor (`<a ... ></a>`) tak, že přidáte nové atributy značky.</span><span class="sxs-lookup"><span data-stu-id="26fc1-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="26fc1-106">Podle konvence mají předponu názvů atributů `asp-`.</span><span class="sxs-lookup"><span data-stu-id="26fc1-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="26fc1-107">Prvek vykreslené ukotvení `href` hodnota atributu je určen podle hodnot `asp-` atributy.</span><span class="sxs-lookup"><span data-stu-id="26fc1-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="26fc1-108">Přehled pomocných rutin značek, naleznete v tématu <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="26fc1-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="26fc1-109">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="26fc1-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="26fc1-110">*SpeakerController* se používá v ukázky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="26fc1-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="26fc1-111">Inventář `asp-` atributy způsobem.</span><span class="sxs-lookup"><span data-stu-id="26fc1-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="26fc1-112">kontroler ASP</span><span class="sxs-lookup"><span data-stu-id="26fc1-112">asp-controller</span></span>

<span data-ttu-id="26fc1-113">[Asp kontroleru](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) atribut přiřadí sloužit ke generování adresy URL kontroleru.</span><span class="sxs-lookup"><span data-stu-id="26fc1-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="26fc1-114">Následující kód uvádí všechny přednášející:</span><span class="sxs-lookup"><span data-stu-id="26fc1-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="26fc1-115">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="26fc1-116">Pokud `asp-controller` je zadán atribut a `asp-action` není výchozí `asp-action` hodnotu akce kontroleru, který je přidružený k aktuálně prováděné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="26fc1-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="26fc1-117">Pokud `asp-action` je vynecháno z předchozí kód a ukotvení pomocné rutiny značky se používá v *HomeController*společnosti *Index* zobrazení (*/Home*), je generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="26fc1-118">Akce ASP</span><span class="sxs-lookup"><span data-stu-id="26fc1-118">asp-action</span></span>

<span data-ttu-id="26fc1-119">[Asp akce](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) hodnota atributu představuje název akce kontroleru součástí generované `href` atribut.</span><span class="sxs-lookup"><span data-stu-id="26fc1-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="26fc1-120">Následující kód nastaví generované `href` hodnotu atributu na stránce hodnocení mluvčího:</span><span class="sxs-lookup"><span data-stu-id="26fc1-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="26fc1-121">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="26fc1-122">Pokud ne `asp-controller` atribut zadán, je použit výchozí řadič volání zobrazení provádění aktuálního zobrazení.</span><span class="sxs-lookup"><span data-stu-id="26fc1-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="26fc1-123">Pokud `asp-action` hodnota atributu je `Index`, pak žádná akce je připojena k adrese URL, což vede k vyvolání výchozí `Index` akce.</span><span class="sxs-lookup"><span data-stu-id="26fc1-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="26fc1-124">Akce zadané (nebo nastavit na výchozí hodnotu), musí existovat v kontroleru odkazuje `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="26fc1-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="26fc1-125">ASP - route-{value}</span><span class="sxs-lookup"><span data-stu-id="26fc1-125">asp-route-{value}</span></span>

<span data-ttu-id="26fc1-126">[Asp - route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atribut umožňuje předponu trasy zástupný znak.</span><span class="sxs-lookup"><span data-stu-id="26fc1-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="26fc1-127">Všechny hodnoty zabírá `{value}` zástupný symbol je interpretován jako potenciální parametr trasa.</span><span class="sxs-lookup"><span data-stu-id="26fc1-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="26fc1-128">Pokud výchozí trasa není nalezen, tuto předponu trasy, se připojí k generované `href` atribut jako parametr žádosti a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="26fc1-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="26fc1-129">V opačném případě je nahrazena v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="26fc1-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="26fc1-130">Vezměte v úvahu následující akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="26fc1-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="26fc1-131">Pomocí výchozí šablony trasy definované v *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="26fc1-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="26fc1-132">Zobrazení MVC používá model poskytované akci, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="26fc1-132">The MVC view uses the model, provided by the action, as follows:</span></span>

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

<span data-ttu-id="26fc1-133">Výchozí trasa `{id?}` odpovídal zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="26fc1-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="26fc1-134">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="26fc1-135">Předpokládejme, že předponu trasy, které nejsou součástí odpovídající směrování šablony, stejně jako u následující zobrazení MVC:</span><span class="sxs-lookup"><span data-stu-id="26fc1-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

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

<span data-ttu-id="26fc1-136">Následující kód HTML je generována, protože `speakerid` nebyl nalezen v odpovídající trasy:</span><span class="sxs-lookup"><span data-stu-id="26fc1-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="26fc1-137">Pokud `asp-controller` nebo `asp-action` nejsou zadané, pak stejnou výchozí zpracování je a potom je `asp-route` atribut.</span><span class="sxs-lookup"><span data-stu-id="26fc1-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="26fc1-138">ASP trasy</span><span class="sxs-lookup"><span data-stu-id="26fc1-138">asp-route</span></span>

<span data-ttu-id="26fc1-139">[Trasy asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) atribut se používá pro vytvoření adresy URL propojení přímo s pojmenovanou trasu.</span><span class="sxs-lookup"><span data-stu-id="26fc1-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="26fc1-140">Pomocí [směrování atributů](xref:mvc/controllers/routing#attribute-routing), může mít název trasy, jak je znázorněno `SpeakerController` a používat v jeho `Evaluations` akce:</span><span class="sxs-lookup"><span data-stu-id="26fc1-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="26fc1-141">V následujícím kódu `asp-route` atribut odkazuje na pojmenovanou trasu:</span><span class="sxs-lookup"><span data-stu-id="26fc1-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="26fc1-142">Ukotvení pomocné rutiny značky vytváří trasu přímo k této akci kontroleru pomocí adresy URL */mluvčího/hodnocení*.</span><span class="sxs-lookup"><span data-stu-id="26fc1-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="26fc1-143">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="26fc1-144">Pokud `asp-controller` nebo `asp-action` určena kromě `asp-route`, postupu generované nemusí být co očekáváte.</span><span class="sxs-lookup"><span data-stu-id="26fc1-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="26fc1-145">Aby se zabránilo konfliktu trasy `asp-route` by neměl být použit s `asp-controller` a `asp-action` atributy.</span><span class="sxs-lookup"><span data-stu-id="26fc1-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="26fc1-146">ASP všechny trasy dat</span><span class="sxs-lookup"><span data-stu-id="26fc1-146">asp-all-route-data</span></span>

<span data-ttu-id="26fc1-147">[Asp všechny trasy dat](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atribut podporuje vytváření slovník párů klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="26fc1-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="26fc1-148">Klíč je název parametru a hodnota je hodnota tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="26fc1-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="26fc1-149">V následujícím příkladu je slovník inicializován a předána do zobrazení Razor.</span><span class="sxs-lookup"><span data-stu-id="26fc1-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="26fc1-150">Alternativně může předávat data pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="26fc1-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="26fc1-151">Předcházející kód vygeneruje následující kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="26fc1-152">`asp-all-route-data` Slovníku se sloučí k vytvoření dotazu splnění požadavků přetížené `Evaluations` akce:</span><span class="sxs-lookup"><span data-stu-id="26fc1-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="26fc1-153">Pokud se ve slovníku všechny klíče shodují parametry trasy, tyto hodnoty jsou nahrazeny v postupu podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="26fc1-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="26fc1-154">Neodpovídající hodnoty jsou generovány jako parametry požadavku.</span><span class="sxs-lookup"><span data-stu-id="26fc1-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="26fc1-155">ASP fragment</span><span class="sxs-lookup"><span data-stu-id="26fc1-155">asp-fragment</span></span>

<span data-ttu-id="26fc1-156">[Asp fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) atribut definuje fragment adresy URL pro připojení k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="26fc1-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="26fc1-157">Ukotvení pomocné rutiny značky přidá znak hash (#).</span><span class="sxs-lookup"><span data-stu-id="26fc1-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="26fc1-158">Vezměte v úvahu následující kód:</span><span class="sxs-lookup"><span data-stu-id="26fc1-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="26fc1-159">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="26fc1-160">Hodnota hash značky jsou užitečné při vytváření aplikací na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="26fc1-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="26fc1-161">Jejich lze snadno označení a vyhledávání v jazyce JavaScript, třeba.</span><span class="sxs-lookup"><span data-stu-id="26fc1-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="26fc1-162">oblast ASP</span><span class="sxs-lookup"><span data-stu-id="26fc1-162">asp-area</span></span>

<span data-ttu-id="26fc1-163">[Asp oblasti](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) atribut nastaví název oblasti, která slouží k nastavení odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="26fc1-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="26fc1-164">Následující příklad znázorňuje, jak atribut oblasti způsobí přemapování trasy.</span><span class="sxs-lookup"><span data-stu-id="26fc1-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="26fc1-165">Nastavení `asp-area` "Blogy" předpon adresáři *oblasti/blogy* trasy z přidružených kontrolerů a zobrazení pro tuto značku ukotvení.</span><span class="sxs-lookup"><span data-stu-id="26fc1-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="26fc1-166">**{Název projektu}**</span><span class="sxs-lookup"><span data-stu-id="26fc1-166">**{Project name}**</span></span>
  * <span data-ttu-id="26fc1-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="26fc1-167">**wwwroot**</span></span>
  * <span data-ttu-id="26fc1-168">**Oblasti**</span><span class="sxs-lookup"><span data-stu-id="26fc1-168">**Areas**</span></span>
    * <span data-ttu-id="26fc1-169">**Blogy**</span><span class="sxs-lookup"><span data-stu-id="26fc1-169">**Blogs**</span></span>
      * <span data-ttu-id="26fc1-170">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="26fc1-170">**Controllers**</span></span>
        * <span data-ttu-id="26fc1-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="26fc1-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="26fc1-172">**Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="26fc1-172">**Views**</span></span>
        * <span data-ttu-id="26fc1-173">**Domovská stránka**</span><span class="sxs-lookup"><span data-stu-id="26fc1-173">**Home**</span></span>
          * <span data-ttu-id="26fc1-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="26fc1-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="26fc1-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="26fc1-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="26fc1-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="26fc1-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="26fc1-177">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="26fc1-177">**Controllers**</span></span>

<span data-ttu-id="26fc1-178">Zadaný předchozí hierarchii adresářů, kód tak, aby odkazovaly *AboutBlog.cshtml* souboru je:</span><span class="sxs-lookup"><span data-stu-id="26fc1-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="26fc1-179">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="26fc1-180">Pro oblasti pro práci v aplikaci MVC šablona trasy uvést odkaz na oblast, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="26fc1-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="26fc1-181">Tato šablona je reprezentována druhý parametr `routes.MapRoute` volání metody *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="26fc1-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="26fc1-182">protokol ASP</span><span class="sxs-lookup"><span data-stu-id="26fc1-182">asp-protocol</span></span>

<span data-ttu-id="26fc1-183">[Protokolu asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) atribut slouží k určení protokol (například `https`) v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="26fc1-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="26fc1-184">Příklad:</span><span class="sxs-lookup"><span data-stu-id="26fc1-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="26fc1-185">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="26fc1-186">Název hostitele v tomto příkladu je localhost, ale ukotvení pomocné rutiny značky používá veřejnou doménu na webu při generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="26fc1-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="26fc1-187">ASP hostitele</span><span class="sxs-lookup"><span data-stu-id="26fc1-187">asp-host</span></span>

<span data-ttu-id="26fc1-188">[Asp hostitele](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) atribut slouží k určení názvu hostitele v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="26fc1-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="26fc1-189">Příklad:</span><span class="sxs-lookup"><span data-stu-id="26fc1-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="26fc1-190">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="26fc1-191">stránka ASP</span><span class="sxs-lookup"><span data-stu-id="26fc1-191">asp-page</span></span>

<span data-ttu-id="26fc1-192">[Stránka asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) atribut se používá se stránkami Razor.</span><span class="sxs-lookup"><span data-stu-id="26fc1-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="26fc1-193">Můžete nastavit značku ukotvení `href` hodnotu atributu na určitou stránku.</span><span class="sxs-lookup"><span data-stu-id="26fc1-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="26fc1-194">Předpony názvu stránky s lomítkem ("/") se vytvoří adresa URL.</span><span class="sxs-lookup"><span data-stu-id="26fc1-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="26fc1-195">Následující příklad ukazuje na účastník stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="26fc1-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="26fc1-196">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="26fc1-197">`asp-page` Atribut je vzájemně se vylučuje s `asp-route`, `asp-controller`, a `asp-action` atributy.</span><span class="sxs-lookup"><span data-stu-id="26fc1-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="26fc1-198">Ale `asp-page` jde použít s `asp-route-{value}` řídit směrování, jak ukazuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="26fc1-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="26fc1-199">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="26fc1-200">rutina stránky ASP</span><span class="sxs-lookup"><span data-stu-id="26fc1-200">asp-page-handler</span></span>

<span data-ttu-id="26fc1-201">[Rutina stránky asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) atribut se používá se stránkami Razor.</span><span class="sxs-lookup"><span data-stu-id="26fc1-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="26fc1-202">Je určena pro odkazování na určitou stránku obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="26fc1-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="26fc1-203">Vezměte v úvahu následující rutiny stránky:</span><span class="sxs-lookup"><span data-stu-id="26fc1-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="26fc1-204">Model stránky přidruženého k tomuto kódu odkazy na `OnGetProfile` obslužná rutina stránky.</span><span class="sxs-lookup"><span data-stu-id="26fc1-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="26fc1-205">Všimněte si, že `On<Verb>` předpona názvu metody obslužné rutiny stránky je vynecháno v `asp-page-handler` hodnotu atributu.</span><span class="sxs-lookup"><span data-stu-id="26fc1-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="26fc1-206">To šlo o asynchronní metodu `Async` přípona by příliš vynechat.</span><span class="sxs-lookup"><span data-stu-id="26fc1-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="26fc1-207">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="26fc1-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="26fc1-208">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="26fc1-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
