---
title: "Značka Pomocník jádro ASP.NET MVC do mezipaměti"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značky mezipaměti"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 1710a5781fb69aaa6101270d6b4fd44f92c7f06c
ms.sourcegitcommit: a33737ea24e1ea9642e461d1bc90d6701f889436
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="9c511-104">Značka Pomocník jádro ASP.NET MVC do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9c511-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="9c511-105">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="9c511-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="9c511-106">Pomocník značky mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah do vnitřní mezipaměti poskytovatele ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c511-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="9c511-107">Nastaví výchozí zobrazovací modul Razor `expires-after` než 20 minut.</span><span class="sxs-lookup"><span data-stu-id="9c511-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="9c511-108">Následující kód Razor ukládá do mezipaměti data a času:</span><span class="sxs-lookup"><span data-stu-id="9c511-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="9c511-109">První požadavek na stránku, který obsahuje `CacheTagHelper` se zobrazí aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="9c511-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="9c511-110">Další požadavky se zobrazí hodnota uložená v mezipaměti, dokud mezipaměti vyprší platnost (výchozí nastavení 20 minut) nebo vyřazování podle přetížení paměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="9c511-111">Můžete nastavit dobu uložení do mezipaměti s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="9c511-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="9c511-112">Mezipaměti atributů značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="9c511-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="9c511-113">povoleno</span><span class="sxs-lookup"><span data-stu-id="9c511-113">enabled</span></span>    


| <span data-ttu-id="9c511-114">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-114">Attribute Type</span></span>    | <span data-ttu-id="9c511-115">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="9c511-116">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9c511-116">boolean</span></span>           | <span data-ttu-id="9c511-117">"true" (výchozí)</span><span class="sxs-lookup"><span data-stu-id="9c511-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="9c511-118">"false"</span><span class="sxs-lookup"><span data-stu-id="9c511-118">"false"</span></span>   |


<span data-ttu-id="9c511-119">Určuje, zda je ukládat do mezipaměti obsah uzavřené do pomocné rutiny značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="9c511-120">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="9c511-120">The default is `true`.</span></span>  <span data-ttu-id="9c511-121">Pokud nastavena na `false` tohoto pomocníka značky mezipaměti bude mít neplatí ukládání do mezipaměti pro vykreslený výstup.</span><span class="sxs-lookup"><span data-stu-id="9c511-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="9c511-122">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="9c511-123">vyprší dne</span><span class="sxs-lookup"><span data-stu-id="9c511-123">expires-on</span></span> 

| <span data-ttu-id="9c511-124">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-124">Attribute Type</span></span>    | <span data-ttu-id="9c511-125">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="9c511-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="9c511-126">DateTimeOffset</span></span>    | <span data-ttu-id="9c511-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="9c511-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="9c511-128">Nastaví datum vypršení platnosti absolutní.</span><span class="sxs-lookup"><span data-stu-id="9c511-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="9c511-129">V následujícím příkladu bude ukládat do mezipaměti obsah pomocná značky mezipaměti až 17:02:00 na 29 leden 2025.</span><span class="sxs-lookup"><span data-stu-id="9c511-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="9c511-130">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="9c511-131">Po vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="9c511-131">expires-after</span></span>

| <span data-ttu-id="9c511-132">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-132">Attribute Type</span></span>    | <span data-ttu-id="9c511-133">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="9c511-134">Časový interval</span><span class="sxs-lookup"><span data-stu-id="9c511-134">TimeSpan</span></span>    | <span data-ttu-id="9c511-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="9c511-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="9c511-136">Nastaví dobu od prvního požadavku pro ukládání do mezipaměti obsah.</span><span class="sxs-lookup"><span data-stu-id="9c511-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="9c511-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-137">Example:</span></span>

```cshtml
<Cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="9c511-138">klouzavé vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="9c511-138">expires-sliding</span></span>

| <span data-ttu-id="9c511-139">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-139">Attribute Type</span></span>    | <span data-ttu-id="9c511-140">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="9c511-141">Časový interval</span><span class="sxs-lookup"><span data-stu-id="9c511-141">TimeSpan</span></span>    | <span data-ttu-id="9c511-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="9c511-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="9c511-143">Nastaví dobu, která by měla být vyřazena položku mezipaměti, pokud není přístup.</span><span class="sxs-lookup"><span data-stu-id="9c511-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="9c511-144">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="9c511-145">měnit podle hlavičky</span><span class="sxs-lookup"><span data-stu-id="9c511-145">vary-by-header</span></span>

| <span data-ttu-id="9c511-146">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-146">Attribute Type</span></span>    | <span data-ttu-id="9c511-147">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-148">String</span><span class="sxs-lookup"><span data-stu-id="9c511-148">String</span></span>            | <span data-ttu-id="9c511-149">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="9c511-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="9c511-150">"User-Agent, kódování obsahu"</span><span class="sxs-lookup"><span data-stu-id="9c511-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="9c511-151">Přijme hodnotu jedné hlavičky nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní, oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="9c511-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="9c511-152">Následující příklad monitoruje hodnota hlavičky `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="9c511-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="9c511-153">V příkladu se uloží obsah do mezipaměti pro každý jiný `User-Agent` webového serveru.</span><span class="sxs-lookup"><span data-stu-id="9c511-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="9c511-154">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="9c511-155">se liší podle dotazu</span><span class="sxs-lookup"><span data-stu-id="9c511-155">vary-by-query</span></span>

| <span data-ttu-id="9c511-156">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-156">Attribute Type</span></span>    | <span data-ttu-id="9c511-157">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-158">String</span><span class="sxs-lookup"><span data-stu-id="9c511-158">String</span></span>            | <span data-ttu-id="9c511-159">"Zkontrolujte"</span><span class="sxs-lookup"><span data-stu-id="9c511-159">"Make"</span></span>                |
|                   | <span data-ttu-id="9c511-160">"Zkontrolujte modelu"</span><span class="sxs-lookup"><span data-stu-id="9c511-160">"Make,Model"</span></span> |

<span data-ttu-id="9c511-161">Přijme jeden záhlaví hodnotu nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní hodnota hlavičky oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="9c511-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="9c511-162">V následujícím příkladu vypadá na hodnoty `Make` a `Model`.</span><span class="sxs-lookup"><span data-stu-id="9c511-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="9c511-163">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="9c511-164">se liší podle trasy</span><span class="sxs-lookup"><span data-stu-id="9c511-164">vary-by-route</span></span>

| <span data-ttu-id="9c511-165">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-165">Attribute Type</span></span>    | <span data-ttu-id="9c511-166">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-167">String</span><span class="sxs-lookup"><span data-stu-id="9c511-167">String</span></span>            | <span data-ttu-id="9c511-168">"Zkontrolujte"</span><span class="sxs-lookup"><span data-stu-id="9c511-168">"Make"</span></span>                |
|                   | <span data-ttu-id="9c511-169">"Zkontrolujte modelu"</span><span class="sxs-lookup"><span data-stu-id="9c511-169">"Make,Model"</span></span> |

<span data-ttu-id="9c511-170">Přijme hodnotu jedné hlavičky nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změna hodnoty parametru data trasy oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="9c511-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="9c511-171">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-171">Example:</span></span>

<span data-ttu-id="9c511-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9c511-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="9c511-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c511-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="9c511-174">se liší podle cookie</span><span class="sxs-lookup"><span data-stu-id="9c511-174">vary-by-cookie</span></span>

| <span data-ttu-id="9c511-175">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-175">Attribute Type</span></span>    | <span data-ttu-id="9c511-176">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-177">String</span><span class="sxs-lookup"><span data-stu-id="9c511-177">String</span></span>            | <span data-ttu-id="9c511-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="9c511-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="9c511-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="9c511-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="9c511-180">Přijme jeden záhlaví hodnotu nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud se změní (s) hodnoty hlavičky oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="9c511-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="9c511-181">V následujícím příkladu vypadá v souboru cookie přidruženého ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="9c511-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="9c511-182">Když je uživatel ověřen žádosti soubor cookie nastavit který aktivuje aktualizace mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="9c511-183">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="9c511-184">se liší podle uživatele</span><span class="sxs-lookup"><span data-stu-id="9c511-184">vary-by-user</span></span>

| <span data-ttu-id="9c511-185">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-185">Attribute Type</span></span>    | <span data-ttu-id="9c511-186">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-187">Boolean</span><span class="sxs-lookup"><span data-stu-id="9c511-187">Boolean</span></span>             | <span data-ttu-id="9c511-188">"true"</span><span class="sxs-lookup"><span data-stu-id="9c511-188">"true"</span></span>                  |
|                     | <span data-ttu-id="9c511-189">"Nepravda" (výchozí)</span><span class="sxs-lookup"><span data-stu-id="9c511-189">"false" (default)</span></span> |

<span data-ttu-id="9c511-190">Určuje, zda má mezipaměti resetovat při změně přihlášeného uživatele (nebo objekt kontextu zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="9c511-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="9c511-191">Aktuální uživatel je také označován jako objekt kontextu požadavku a lze je zobrazit v zobrazení syntaxe Razor pod položkou `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="9c511-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="9c511-192">Následující příklad hledána v aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c511-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="9c511-193">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="9c511-194">Pomocí tohoto atributu udržuje obsah v mezipaměti prostřednictvím cyklus přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="9c511-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="9c511-195">Při použití `vary-by-user="true"`, přihlášení a odhlášení akce zruší platnost mezipaměti pro ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c511-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="9c511-196">Mezipaměti je neplatná, protože byl vygenerován novou hodnotu jedinečný soubor cookie na přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9c511-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="9c511-197">Mezipaměti bude zachována pro anonymní stavu, pokud žádný soubor cookie je k dispozici nebo vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="9c511-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="9c511-198">To znamená, že pokud je přihlášen žádný uživatel, se zachová mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="9c511-199">se liší podle</span><span class="sxs-lookup"><span data-stu-id="9c511-199">vary-by</span></span>

| <span data-ttu-id="9c511-200">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-200">Attribute Type</span></span>    | <span data-ttu-id="9c511-201">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-202">String</span><span class="sxs-lookup"><span data-stu-id="9c511-202">String</span></span>             | <span data-ttu-id="9c511-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="9c511-203">"@Model"</span></span>                 |


<span data-ttu-id="9c511-204">Umožňuje přizpůsobení získá jaké data uložena do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="9c511-205">Při aktualizaci objektu odkazuje atributu řetězec hodnotu změny, obsah pomocná značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="9c511-206">Zřetězení řetězců modelu hodnot často jsou přiřazeny tomuto atributu.</span><span class="sxs-lookup"><span data-stu-id="9c511-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="9c511-207">Efektivní, to znamená, že aktualizace zřetězených hodnot zruší platnost mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="9c511-208">V následujícím příkladu se předpokládá, metoda kontroleru vykreslování zobrazení součtů celočíselnou hodnotu dva parametry trasy, `myParam1` a `myParam2`a vrátí, jako vlastnost jeden model.</span><span class="sxs-lookup"><span data-stu-id="9c511-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="9c511-209">Při změně této součet obsah pomocná značky mezipaměti je vykreslen a uložili do mezipaměti znovu.</span><span class="sxs-lookup"><span data-stu-id="9c511-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="9c511-210">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-210">Example:</span></span>

<span data-ttu-id="9c511-211">Akce:</span><span class="sxs-lookup"><span data-stu-id="9c511-211">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="9c511-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9c511-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="9c511-213">Priorita</span><span class="sxs-lookup"><span data-stu-id="9c511-213">priority</span></span>

| <span data-ttu-id="9c511-214">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="9c511-214">Attribute Type</span></span>    | <span data-ttu-id="9c511-215">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c511-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="9c511-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="9c511-216">CacheItemPriority</span></span>  | <span data-ttu-id="9c511-217">"Vysoká"</span><span class="sxs-lookup"><span data-stu-id="9c511-217">"High"</span></span>                   |
|                    | <span data-ttu-id="9c511-218">"Nízká"</span><span class="sxs-lookup"><span data-stu-id="9c511-218">"Low"</span></span> |
|                    | <span data-ttu-id="9c511-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="9c511-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="9c511-220">"Normální"</span><span class="sxs-lookup"><span data-stu-id="9c511-220">"Normal"</span></span> |

<span data-ttu-id="9c511-221">Obsahuje mezipaměti vyřazení pokyny k poskytovateli předdefinované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="9c511-222">Webový server bude vyřazení `Low` nejprve mezipaměti položky, když je paměť přetížena.</span><span class="sxs-lookup"><span data-stu-id="9c511-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="9c511-223">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9c511-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="9c511-224">`priority` Atribut nezaručí konkrétní úroveň mezipaměti uchování.</span><span class="sxs-lookup"><span data-stu-id="9c511-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="9c511-225">`CacheItemPriority`je pouze návrhu.</span><span class="sxs-lookup"><span data-stu-id="9c511-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="9c511-226">Nastavení tohoto atributu na `NeverRemove` není zaručeno, že budou vždy zachována mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c511-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="9c511-227">V tématu [další prostředky](#additional-resources) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9c511-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="9c511-228">Pomocník značky mezipaměti je závislá na [služby mezipaměti paměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="9c511-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="9c511-229">Pomocník značky mezipaměti přidá službu, pokud nebyl přidán.</span><span class="sxs-lookup"><span data-stu-id="9c511-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c511-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c511-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
