---
title: "Pomocník značka ukotvení | Microsoft Docs"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značka ukotvení"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: e3754c4313f01bc746ccb8efe11611ae213e3955
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="9bd5e-104">Pomocník značka ukotvení</span><span class="sxs-lookup"><span data-stu-id="9bd5e-104">Anchor Tag Helper</span></span>

<span data-ttu-id="9bd5e-105">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="9bd5e-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="9bd5e-106">Pomocník značka ukotvení vylepšuje HTML anchor (`<a ... ></a>`) značky přidáním nové atributy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="9bd5e-107">Vygeneruje odkaz (na `href` značka) je vytvořený pomocí nové atributy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="9bd5e-108">Aby adresa URL může obsahovat volitelné protokol, jako je například https.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="9bd5e-109">Níže mluvčího řadiče se používá v ukázky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-109">The speaker controller below is used in samples in this document.</span></span>

<br/><span data-ttu-id="9bd5e-110">
**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="9bd5e-110">
**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="9bd5e-111">Atributy pomocné rutiny značka ukotvení</span><span class="sxs-lookup"><span data-stu-id="9bd5e-111">Anchor Tag Helper Attributes</span></span>

- - -

### <a name="asp-controller"></a><span data-ttu-id="9bd5e-112">ASP-controller</span><span class="sxs-lookup"><span data-stu-id="9bd5e-112">asp-controller</span></span>

<span data-ttu-id="9bd5e-113">`asp-controller`je použito k přidružení řadič, který se použije k vytvoření adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="9bd5e-114">Řadičů určených musí existovat v aktuálním projektu.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="9bd5e-115">Následující kód obsahuje seznam všech mluvčí:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="9bd5e-116">Generovaný kód bude:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="9bd5e-117">Pokud `asp-controller` je zadán a `asp-action` není, výchozím `asp-action` bude výchozí metodu řadiče aktuálně prováděné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="9bd5e-118">Aby se v předchozím příkladu, pokud `asp-action` je vynechána, a tohoto pomocníka značka ukotvení se generují z *HomeController*na `Index` zobrazení (**/Home**), bude vygenerovaný kód:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>


```html
<a href="/Home">All Speakers</a>
```

- - -
  
### <a name="asp-action"></a><span data-ttu-id="9bd5e-119">Akce ASP</span><span class="sxs-lookup"><span data-stu-id="9bd5e-119">asp-action</span></span>

<span data-ttu-id="9bd5e-120">`asp-action`Název metody akce v kontroleru, který bude zahrnut v vygenerovaného `href`.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="9bd5e-121">Například následující kód nastavit vygenerovaného `href` tak, aby odkazoval na stránku podrobností mluvčího:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="9bd5e-122">Generovaný kód bude:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="9bd5e-123">Pokud žádné `asp-controller` zadán atribut, použije se výchozí kontroleru volání zobrazení provádění aktuálního zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="9bd5e-124">Pokud atribut `asp-action` je `Index`, pak žádná akce je připojena k adrese URL, což výchozí `Index` volané metodě.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="9bd5e-125">Akce zadané (nebo uvedena), musí existovat v kontroleru, kterou se odkazuje v `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

- - -
  
<a name="route"></a>
### <a name="asp-route-value"></a><span data-ttu-id="9bd5e-126">ASP - trasy-{value}</span><span class="sxs-lookup"><span data-stu-id="9bd5e-126">asp-route-{value}</span></span>

<span data-ttu-id="9bd5e-127">`asp-route-`je předponu trasy zástupnému znaku.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-127">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="9bd5e-128">Libovolná hodnota, která jste uložili po konci dash, bude vyhodnocen jako potenciální parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-128">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="9bd5e-129">Pokud není nalezen výchozí trasu, připojí se k vygenerovaný odkaz href jako parametr žádosti a hodnota tuto předponu trasy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-129">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="9bd5e-130">V opačném případě se bude nahrazena v šabloně trasy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-130">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="9bd5e-131">Za předpokladu, že je metoda kontroleru definovali takto:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-131">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="9bd5e-132">A jste výchozí šablonu trasy definované v vaše *Startup.cs* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-132">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="9bd5e-133">**Cshtml** soubor, který obsahuje pomocný značka ukotvení nezbytných k používání **mluvčího** parametr modelu předané z řadiče zobrazení je následující:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-133">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="9bd5e-134">Generovaný kód jazyka HTML se bude následujícím způsobem, protože **id** byl nalezen v výchozí trasu.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-134">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="9bd5e-135">Pokud předponu trasy není součástí směrování šablony najít, což je případ s následující **cshtml** souboru:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-135">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="9bd5e-136">Generovaný kód jazyka HTML se bude následujícím způsobem, protože **speakerid** nebyl nalezen v trasy, která odpovídá:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-136">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>


```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="9bd5e-137">Pokud má jedna `asp-controller` nebo `asp-action` nejsou zadané, pak stejné zpracování výchozí je následovaný, protože `asp-route` atribut.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-137">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

- - -

### <a name="asp-route"></a><span data-ttu-id="9bd5e-138">ASP trasy</span><span class="sxs-lookup"><span data-stu-id="9bd5e-138">asp-route</span></span>

<span data-ttu-id="9bd5e-139">`asp-route`poskytuje způsob, jak vytvořit adresu URL, který odkazuje přímo na pojmenovanou trasu.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-139">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="9bd5e-140">Pomocí směrování atributů, trasu může mít název jak je znázorněno `SpeakerController` a používá v jeho `Evaluations` metoda.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-140">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="9bd5e-141">`Name = "speakerevals"`informuje pomocná značka ukotvení ke generování trasu přímo k dané metody kontroleru pomocí adresy URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-141">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="9bd5e-142">Pokud `asp-controller` nebo `asp-action` je zadán kromě `asp-route`, postupu generované nemusí být očekávat.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-142">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="9bd5e-143">`asp-route`Nepoužívejte pomocí atributů `asp-controller` nebo `asp-action` aby nedošlo ke konfliktu trasy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-143">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

- - -

### <a name="asp-all-route-data"></a><span data-ttu-id="9bd5e-144">ASP všechny trasy dat</span><span class="sxs-lookup"><span data-stu-id="9bd5e-144">asp-all-route-data</span></span>

<span data-ttu-id="9bd5e-145">`asp-all-route-data`Umožňuje vytváření slovník párů klíčových hodnot, kde klíč je název parametru a hodnota je hodnota přidružená k tomuto klíči.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-145">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="9bd5e-146">Jako v příkladu níže znázorňuje slovníku vložené se vytvoří a data jsou předána do zobrazení razor.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-146">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="9bd5e-147">Jako alternativu může být data předány také pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-147">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="9bd5e-148">Výše uvedený kód generuje následující adresu URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="9bd5e-148">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="9bd5e-149">Pokud po kliknutí na odkaz, metoda kontroleru `EvaluationsCurrent` je volána.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-149">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="9bd5e-150">Je volána, protože tento řadič se dvěma řetězcovými parametry, které odpovídají, co byl vytvořen z `asp-all-route-data` slovníku.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-150">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="9bd5e-151">Pokud žádné klíče ve slovníku shody směrovat parametry, tyto hodnoty bude nahrazena v postupu podle potřeby a ostatní neodpovídající hodnoty se budou generovat jako parametry žádosti.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-151">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

- - -

### <a name="asp-fragment"></a><span data-ttu-id="9bd5e-152">ASP fragment</span><span class="sxs-lookup"><span data-stu-id="9bd5e-152">asp-fragment</span></span>

<span data-ttu-id="9bd5e-153">`asp-fragment`Definuje fragment adresy URL pro připojení k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-153">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="9bd5e-154">Pomocník značka ukotvení přidá znak hash (#).</span><span class="sxs-lookup"><span data-stu-id="9bd5e-154">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="9bd5e-155">Pokud jste vytvořit značku:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-155">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="9bd5e-156">Vygenerovaná adresa URL bude: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="9bd5e-156">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="9bd5e-157">Hodnota hash značky jsou užitečné při sestavování aplikací na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-157">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="9bd5e-158">Mohou být použity pro snadné označování a hledání v jazyce JavaScript, třeba.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-158">They can be used for easy marking and searching in JavaScript, for example.</span></span>

- - -

### <a name="asp-area"></a><span data-ttu-id="9bd5e-159">oblasti ASP</span><span class="sxs-lookup"><span data-stu-id="9bd5e-159">asp-area</span></span>

<span data-ttu-id="9bd5e-160">`asp-area`Nastaví název oblasti, který používá ASP.NET Core nastavit odpovídající trasy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-160">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="9bd5e-161">Níže jsou příklady jak atribut oblasti způsobí přemapování trasy.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-161">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="9bd5e-162">Nastavení `asp-area` pro blogy předpony adresáři `Areas/Blogs` pro trasy přidružené kontrolery a zobrazení pro tuto značku ukotvení.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-162">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="9bd5e-163">název projektu</span><span class="sxs-lookup"><span data-stu-id="9bd5e-163">Project name</span></span>

  * <span data-ttu-id="9bd5e-164">*Wwwroot*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-164">*wwwroot*</span></span>

  * <span data-ttu-id="9bd5e-165">*Oblasti*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-165">*Areas*</span></span>

    * <span data-ttu-id="9bd5e-166">*Blogy*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-166">*Blogs*</span></span>

      * <span data-ttu-id="9bd5e-167">*Řadiče*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-167">*Controllers*</span></span>

        * <span data-ttu-id="9bd5e-168">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-168">*HomeController.cs*</span></span>

      * <span data-ttu-id="9bd5e-169">*Zobrazení*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-169">*Views*</span></span>

        * <span data-ttu-id="9bd5e-170">*Domovské*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-170">*Home*</span></span>

          * <span data-ttu-id="9bd5e-171">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-171">*Index.cshtml*</span></span>
          
          * <span data-ttu-id="9bd5e-172">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-172">*AboutBlog.cshtml*</span></span>
          
  * <span data-ttu-id="9bd5e-173">*Řadiče*</span><span class="sxs-lookup"><span data-stu-id="9bd5e-173">*Controllers*</span></span>
  

        
<span data-ttu-id="9bd5e-174">Určení značku oblasti, který je platný, například ```area="Blogs"``` při odkazování na ```AboutBlog.cshtml``` soubor bude vypadat jako následující využitím pomocné rutiny značka ukotvení.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-174">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="9bd5e-175">Generovaný kód HTML bude obsahovat segmentu oblasti a bude takto:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-175">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="9bd5e-176">Pro MVC oblasti pro práci ve webové aplikaci musí šablona trasy obsahovat odkaz na oblasti, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-176">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="9bd5e-177">Šablony, které je druhý parametr z `routes.MapRoute` volání metody, se zobrazí jako:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="9bd5e-177">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

- - -

### <a name="asp-protocol"></a><span data-ttu-id="9bd5e-178">protokol ASP</span><span class="sxs-lookup"><span data-stu-id="9bd5e-178">asp-protocol</span></span>

<span data-ttu-id="9bd5e-179">`asp-protocol` Je pro zadání protokol (například `https`) v svoji adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-179">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="9bd5e-180">Příklad značky pomocné rutiny ukotvení s uvedením protokolu bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-180">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="9bd5e-181">a bude generují kód HTML následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9bd5e-181">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="9bd5e-182">Domény v příkladu je localhost, ale pomocný značka ukotvení používá webu veřejné domény při generování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9bd5e-182">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

- - -

## <a name="additional-resources"></a><span data-ttu-id="9bd5e-183">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9bd5e-183">Additional resources</span></span>

* [<span data-ttu-id="9bd5e-184">Oblasti</span><span class="sxs-lookup"><span data-stu-id="9bd5e-184">Areas</span></span>](xref:mvc/controllers/areas)
