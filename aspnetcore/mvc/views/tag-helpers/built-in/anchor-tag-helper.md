---
title: "Pomocník značka ukotvení ASP.NET Core"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značka ukotvení"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="2f32c-103">Pomocník značka ukotvení</span><span class="sxs-lookup"><span data-stu-id="2f32c-103">Anchor Tag Helper</span></span>

<span data-ttu-id="2f32c-104">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="2f32c-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="2f32c-105">Pomocník značka ukotvení vylepšuje HTML anchor (`<a ... ></a>`) značky přidáním nové atributy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="2f32c-106">Vygeneruje odkaz (na `href` značka) je vytvořený pomocí nové atributy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="2f32c-107">Aby adresa URL může obsahovat volitelné protokol, jako je například https.</span><span class="sxs-lookup"><span data-stu-id="2f32c-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="2f32c-108">Níže mluvčího řadiče se používá v ukázky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2f32c-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="2f32c-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="2f32c-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="2f32c-110">Atributy pomocné rutiny značka ukotvení</span><span class="sxs-lookup"><span data-stu-id="2f32c-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="2f32c-111">ASP-controller</span><span class="sxs-lookup"><span data-stu-id="2f32c-111">asp-controller</span></span>

<span data-ttu-id="2f32c-112">`asp-controller`je použito k přidružení řadič, který se použije k vytvoření adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2f32c-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="2f32c-113">Řadičů určených musí existovat v aktuálním projektu.</span><span class="sxs-lookup"><span data-stu-id="2f32c-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="2f32c-114">Následující kód obsahuje seznam všech mluvčí:</span><span class="sxs-lookup"><span data-stu-id="2f32c-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="2f32c-115">Generovaný kód bude:</span><span class="sxs-lookup"><span data-stu-id="2f32c-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="2f32c-116">Pokud `asp-controller` je zadán a `asp-action` není výchozí `asp-action` bude výchozí metodu řadiče aktuálně prováděné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2f32c-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="2f32c-117">Aby se v předchozím příkladu, pokud `asp-action` je vynechána, a tohoto pomocníka značka ukotvení se generují z *HomeController*na `Index` zobrazení (**/Home**), bude vygenerovaný kód:</span><span class="sxs-lookup"><span data-stu-id="2f32c-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="2f32c-118">Akce ASP</span><span class="sxs-lookup"><span data-stu-id="2f32c-118">asp-action</span></span>

<span data-ttu-id="2f32c-119">`asp-action`Název metody akce v kontroleru, který bude zahrnut v vygenerovaného `href`.</span><span class="sxs-lookup"><span data-stu-id="2f32c-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="2f32c-120">Například následující kód nastavit vygenerovaného `href` tak, aby odkazoval na stránku podrobností mluvčího:</span><span class="sxs-lookup"><span data-stu-id="2f32c-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="2f32c-121">Generovaný kód bude:</span><span class="sxs-lookup"><span data-stu-id="2f32c-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="2f32c-122">Pokud žádné `asp-controller` zadán atribut, použije se výchozí kontroleru volání zobrazení provádění aktuálního zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2f32c-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="2f32c-123">Pokud atribut `asp-action` je `Index`, pak žádná akce je připojena k adrese URL, což výchozí `Index` volané metodě.</span><span class="sxs-lookup"><span data-stu-id="2f32c-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="2f32c-124">Akce zadané (nebo uvedena), musí existovat v kontroleru, kterou se odkazuje v `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="2f32c-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="2f32c-125">stránka ASP</span><span class="sxs-lookup"><span data-stu-id="2f32c-125">asp-page</span></span>

<span data-ttu-id="2f32c-126">Použití `asp-page` atribut v značku ukotvení k nastavení jeho adresa URL tak, aby odkazoval na konkrétní stránku.</span><span class="sxs-lookup"><span data-stu-id="2f32c-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="2f32c-127">Název stránky s lomítkem prefixu "/" vytvoří adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2f32c-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="2f32c-128">Adresu URL v následující ukázka odkazuje na stránce "Mluvčího" v aktuálním adresáři.</span><span class="sxs-lookup"><span data-stu-id="2f32c-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="2f32c-129">`asp-page` Atribut v předchozí ukázce kódu vykreslí výstup ve formátu HTML v zobrazení, která je podobná následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="2f32c-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="2f32c-130">`asp-page` Atribut je vzájemně se vylučuje s `asp-route`, `asp-controller`, a `asp-action` atributy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="2f32c-131">Ale `asp-page` lze použít s `asp-route-id` řídit směrování, jak ukazuje následující příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="2f32c-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="2f32c-132">`asp-route-id` Vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="2f32c-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="2f32c-133">Použít `asp-page` atribut v stránky Razor, adresy URL musí být relativní cesta, například `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="2f32c-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="2f32c-134">Relativní cesty v `asp-page` atribut nejsou k dispozici v zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="2f32c-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="2f32c-135">Místo toho použijte syntaxi "/" pro zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="2f32c-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="2f32c-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="2f32c-136">asp-route-{value}</span></span>

<span data-ttu-id="2f32c-137">`asp-route-`je předponu trasy zástupnému znaku.</span><span class="sxs-lookup"><span data-stu-id="2f32c-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="2f32c-138">Libovolná hodnota, která jste uložili po konci dash, bude vyhodnocen jako potenciální parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="2f32c-139">Pokud není nalezen výchozí trasu, připojí se k vygenerovaný odkaz href jako parametr žádosti a hodnota tuto předponu trasy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="2f32c-140">V opačném případě se bude nahrazena v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="2f32c-141">Za předpokladu, že je metoda kontroleru definovali takto:</span><span class="sxs-lookup"><span data-stu-id="2f32c-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="2f32c-142">A jste výchozí šablonu trasy definované v vaše *Startup.cs* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2f32c-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="2f32c-143">**Cshtml** soubor, který obsahuje pomocný značka ukotvení nezbytných k používání **mluvčího** parametr modelu předané z řadiče zobrazení je následující:</span><span class="sxs-lookup"><span data-stu-id="2f32c-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="2f32c-144">Generovaný kód jazyka HTML se bude následujícím způsobem, protože **id** byl nalezen v výchozí trasu.</span><span class="sxs-lookup"><span data-stu-id="2f32c-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="2f32c-145">Pokud předponu trasy není součástí směrování šablony najít, což je případ s následující **cshtml** souboru:</span><span class="sxs-lookup"><span data-stu-id="2f32c-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="2f32c-146">Generovaný kód jazyka HTML se bude následujícím způsobem, protože **speakerid** nebyl nalezen v trasy, která odpovídá:</span><span class="sxs-lookup"><span data-stu-id="2f32c-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="2f32c-147">Pokud má jedna `asp-controller` nebo `asp-action` nejsou zadané, pak stejné zpracování výchozí je následovaný, protože `asp-route` atribut.</span><span class="sxs-lookup"><span data-stu-id="2f32c-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="2f32c-148">ASP trasy</span><span class="sxs-lookup"><span data-stu-id="2f32c-148">asp-route</span></span>

<span data-ttu-id="2f32c-149">`asp-route`poskytuje způsob, jak vytvořit adresu URL, který odkazuje přímo na pojmenovanou trasu.</span><span class="sxs-lookup"><span data-stu-id="2f32c-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="2f32c-150">Pomocí směrování atributů, trasu může mít název jak je znázorněno `SpeakerController` a používá v jeho `Evaluations` metoda.</span><span class="sxs-lookup"><span data-stu-id="2f32c-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="2f32c-151">`Name = "speakerevals"`informuje pomocná značka ukotvení ke generování trasu přímo k dané metody kontroleru pomocí adresy URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="2f32c-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="2f32c-152">Pokud `asp-controller` nebo `asp-action` je zadán kromě `asp-route`, postupu generované nemusí být očekávat.</span><span class="sxs-lookup"><span data-stu-id="2f32c-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="2f32c-153">`asp-route`Nepoužívejte pomocí atributů `asp-controller` nebo `asp-action` aby nedošlo ke konfliktu trasy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="2f32c-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="2f32c-154">asp-all-route-data</span></span>

<span data-ttu-id="2f32c-155">`asp-all-route-data`Umožňuje vytváření slovník párů klíčových hodnot, kde klíč je název parametru a hodnota je hodnota přidružená k tomuto klíči.</span><span class="sxs-lookup"><span data-stu-id="2f32c-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="2f32c-156">Jako v příkladu níže znázorňuje slovníku vložené se vytvoří a data jsou předána do zobrazení razor.</span><span class="sxs-lookup"><span data-stu-id="2f32c-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="2f32c-157">Jako alternativu může být data předány také pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="2f32c-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="2f32c-158">Výše uvedený kód generuje následující adresu URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="2f32c-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="2f32c-159">Pokud po kliknutí na odkaz, metoda kontroleru `EvaluationsCurrent` je volána.</span><span class="sxs-lookup"><span data-stu-id="2f32c-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="2f32c-160">Je volána, protože tento řadič se dvěma řetězcovými parametry, které odpovídají, co byl vytvořen z `asp-all-route-data` slovníku.</span><span class="sxs-lookup"><span data-stu-id="2f32c-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="2f32c-161">Pokud žádné klíče ve slovníku shody směrovat parametry, tyto hodnoty bude nahrazena v postupu podle potřeby a ostatní neodpovídající hodnoty se budou generovat jako parametry žádosti.</span><span class="sxs-lookup"><span data-stu-id="2f32c-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="2f32c-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="2f32c-162">asp-fragment</span></span>

<span data-ttu-id="2f32c-163">`asp-fragment`Definuje fragment adresy URL pro připojení k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="2f32c-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="2f32c-164">Pomocník značka ukotvení přidá znak hash (#).</span><span class="sxs-lookup"><span data-stu-id="2f32c-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="2f32c-165">Pokud jste vytvořit značku:</span><span class="sxs-lookup"><span data-stu-id="2f32c-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="2f32c-166">Vygenerovaná adresa URL bude: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="2f32c-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="2f32c-167">Hodnota hash značky jsou užitečné při sestavování aplikací na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2f32c-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="2f32c-168">Mohou být použity pro snadné označování a hledání v jazyce JavaScript, třeba.</span><span class="sxs-lookup"><span data-stu-id="2f32c-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="2f32c-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="2f32c-169">asp-area</span></span>

<span data-ttu-id="2f32c-170">`asp-area`Nastaví název oblasti, který používá ASP.NET Core nastavit odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="2f32c-171">Níže jsou příklady jak atribut oblasti způsobí přemapování trasy.</span><span class="sxs-lookup"><span data-stu-id="2f32c-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="2f32c-172">Nastavení `asp-area` pro blogy předpony adresáři `Areas/Blogs` pro trasy přidružené kontrolery a zobrazení pro tuto značku ukotvení.</span><span class="sxs-lookup"><span data-stu-id="2f32c-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="2f32c-173">název projektu</span><span class="sxs-lookup"><span data-stu-id="2f32c-173">Project name</span></span>
  * <span data-ttu-id="2f32c-174">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="2f32c-174">wwwroot</span></span>
  * <span data-ttu-id="2f32c-175">Oblasti</span><span class="sxs-lookup"><span data-stu-id="2f32c-175">Areas</span></span>
    * <span data-ttu-id="2f32c-176">Blogy</span><span class="sxs-lookup"><span data-stu-id="2f32c-176">Blogs</span></span>
      * <span data-ttu-id="2f32c-177">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="2f32c-177">Controllers</span></span>
        * <span data-ttu-id="2f32c-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="2f32c-178">HomeController.cs</span></span>
      * <span data-ttu-id="2f32c-179">zobrazení</span><span class="sxs-lookup"><span data-stu-id="2f32c-179">Views</span></span>
        * <span data-ttu-id="2f32c-180">Domů</span><span class="sxs-lookup"><span data-stu-id="2f32c-180">Home</span></span>
          * <span data-ttu-id="2f32c-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2f32c-181">Index.cshtml</span></span>
          * <span data-ttu-id="2f32c-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="2f32c-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="2f32c-183">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="2f32c-183">Controllers</span></span>

<span data-ttu-id="2f32c-184">Určení značku oblasti, který je platný, například ```area="Blogs"``` při odkazování na ```AboutBlog.cshtml``` soubor bude vypadat jako následující využitím pomocné rutiny značka ukotvení.</span><span class="sxs-lookup"><span data-stu-id="2f32c-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="2f32c-185">Generovaný kód HTML bude obsahovat segmentu oblasti a bude takto:</span><span class="sxs-lookup"><span data-stu-id="2f32c-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="2f32c-186">Pro MVC oblasti pro práci ve webové aplikaci musí šablona trasy obsahovat odkaz na oblasti, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="2f32c-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="2f32c-187">Šablony, které je druhý parametr z `routes.MapRoute` volání metody, se zobrazí jako:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="2f32c-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="2f32c-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="2f32c-188">asp-protocol</span></span>

<span data-ttu-id="2f32c-189">`asp-protocol` Je pro zadání protokol (například `https`) v svoji adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2f32c-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="2f32c-190">Příklad značky pomocné rutiny ukotvení s uvedením protokolu bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2f32c-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="2f32c-191">a bude generují kód HTML následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2f32c-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="2f32c-192">Domény v příkladu je localhost, ale pomocný značka ukotvení používá webu veřejné domény při generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2f32c-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f32c-193">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2f32c-193">Additional resources</span></span>

* [<span data-ttu-id="2f32c-194">Oblasti</span><span class="sxs-lookup"><span data-stu-id="2f32c-194">Areas</span></span>](xref:mvc/controllers/areas)
