---
title: "Značka Pomocník jádro ASP.NET MVC do mezipaměti"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značky mezipaměti"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="8bd9f-103">Značka Pomocník jádro ASP.NET MVC do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="8bd9f-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="8bd9f-104">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="8bd9f-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="8bd9f-105">Pomocník značky mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah do vnitřní mezipaměti poskytovatele ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="8bd9f-106">Nastaví výchozí zobrazovací modul Razor `expires-after` než 20 minut.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="8bd9f-107">Následující kód Razor ukládá do mezipaměti data a času:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="8bd9f-108">První požadavek na stránku, který obsahuje `CacheTagHelper` se zobrazí aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="8bd9f-109">Další požadavky se zobrazí hodnota uložená v mezipaměti, dokud mezipaměti vyprší platnost (výchozí nastavení 20 minut) nebo vyřazování podle přetížení paměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="8bd9f-110">Můžete nastavit dobu uložení do mezipaměti s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="8bd9f-111">Mezipaměti atributů značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="8bd9f-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="8bd9f-112">povoleno</span><span class="sxs-lookup"><span data-stu-id="8bd9f-112">enabled</span></span>    


| <span data-ttu-id="8bd9f-113">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-113">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-114">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="8bd9f-115">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="8bd9f-115">boolean</span></span>           | <span data-ttu-id="8bd9f-116">"true" (výchozí)</span><span class="sxs-lookup"><span data-stu-id="8bd9f-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="8bd9f-117">"false"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-117">"false"</span></span>   |


<span data-ttu-id="8bd9f-118">Určuje, zda je ukládat do mezipaměti obsah uzavřené do pomocné rutiny značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="8bd9f-119">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-119">The default is `true`.</span></span>  <span data-ttu-id="8bd9f-120">Pokud nastavena na `false` tohoto pomocníka značky mezipaměti bude mít neplatí ukládání do mezipaměti pro vykreslený výstup.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="8bd9f-121">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="8bd9f-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="8bd9f-122">expires-on</span></span> 

| <span data-ttu-id="8bd9f-123">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-123">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-124">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="8bd9f-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="8bd9f-125">DateTimeOffset</span></span>    | <span data-ttu-id="8bd9f-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="8bd9f-127">Nastaví datum vypršení platnosti absolutní.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="8bd9f-128">V následujícím příkladu bude ukládat do mezipaměti obsah pomocná značky mezipaměti až 17:02:00 na 29 leden 2025.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="8bd9f-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="8bd9f-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="8bd9f-130">expires-after</span></span>

| <span data-ttu-id="8bd9f-131">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-131">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-132">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="8bd9f-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8bd9f-133">TimeSpan</span></span>    | <span data-ttu-id="8bd9f-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="8bd9f-135">Nastaví dobu od prvního požadavku pro ukládání do mezipaměti obsah.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="8bd9f-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="8bd9f-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="8bd9f-137">expires-sliding</span></span>

| <span data-ttu-id="8bd9f-138">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-138">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-139">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="8bd9f-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8bd9f-140">TimeSpan</span></span>    | <span data-ttu-id="8bd9f-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="8bd9f-142">Nastaví dobu, která by měla být vyřazena položku mezipaměti, pokud není přístup.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="8bd9f-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="8bd9f-144">měnit podle hlavičky</span><span class="sxs-lookup"><span data-stu-id="8bd9f-144">vary-by-header</span></span>

| <span data-ttu-id="8bd9f-145">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-145">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-146">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-147">String</span><span class="sxs-lookup"><span data-stu-id="8bd9f-147">String</span></span>            | <span data-ttu-id="8bd9f-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="8bd9f-149">"User-Agent, kódování obsahu"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="8bd9f-150">Přijme hodnotu jedné hlavičky nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní, oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="8bd9f-151">Následující příklad monitoruje hodnota hlavičky `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="8bd9f-152">V příkladu se uloží obsah do mezipaměti pro každý jiný `User-Agent` webového serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="8bd9f-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="8bd9f-154">se liší podle dotazu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-154">vary-by-query</span></span>

| <span data-ttu-id="8bd9f-155">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-155">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-156">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-157">String</span><span class="sxs-lookup"><span data-stu-id="8bd9f-157">String</span></span>            | <span data-ttu-id="8bd9f-158">"Zkontrolujte"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-158">"Make"</span></span>                |
|                   | <span data-ttu-id="8bd9f-159">"Zkontrolujte modelu"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-159">"Make,Model"</span></span> |

<span data-ttu-id="8bd9f-160">Přijme jeden záhlaví hodnotu nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní hodnota hlavičky oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="8bd9f-161">V následujícím příkladu vypadá na hodnoty `Make` a `Model`.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="8bd9f-162">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="8bd9f-163">se liší podle trasy</span><span class="sxs-lookup"><span data-stu-id="8bd9f-163">vary-by-route</span></span>

| <span data-ttu-id="8bd9f-164">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-164">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-165">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-166">String</span><span class="sxs-lookup"><span data-stu-id="8bd9f-166">String</span></span>            | <span data-ttu-id="8bd9f-167">"Zkontrolujte"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-167">"Make"</span></span>                |
|                   | <span data-ttu-id="8bd9f-168">"Zkontrolujte modelu"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-168">"Make,Model"</span></span> |

<span data-ttu-id="8bd9f-169">Přijme hodnotu jedné hlavičky nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změna hodnoty parametru data trasy oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="8bd9f-170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-170">Example:</span></span>

<span data-ttu-id="8bd9f-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="8bd9f-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="8bd9f-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8bd9f-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="8bd9f-173">se liší podle cookie</span><span class="sxs-lookup"><span data-stu-id="8bd9f-173">vary-by-cookie</span></span>

| <span data-ttu-id="8bd9f-174">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-174">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-175">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-176">String</span><span class="sxs-lookup"><span data-stu-id="8bd9f-176">String</span></span>            | <span data-ttu-id="8bd9f-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="8bd9f-178">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="8bd9f-179">Přijme jeden záhlaví hodnotu nebo seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud se změní (s) hodnoty hlavičky oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="8bd9f-180">V následujícím příkladu vypadá v souboru cookie přidruženého ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="8bd9f-181">Když je uživatel ověřen žádosti soubor cookie nastavit který aktivuje aktualizace mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="8bd9f-182">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="8bd9f-183">se liší podle uživatele</span><span class="sxs-lookup"><span data-stu-id="8bd9f-183">vary-by-user</span></span>

| <span data-ttu-id="8bd9f-184">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-184">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-185">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="8bd9f-186">Boolean</span></span>             | <span data-ttu-id="8bd9f-187">"true"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-187">"true"</span></span>                  |
|                     | <span data-ttu-id="8bd9f-188">"Nepravda" (výchozí)</span><span class="sxs-lookup"><span data-stu-id="8bd9f-188">"false" (default)</span></span> |

<span data-ttu-id="8bd9f-189">Určuje, zda má mezipaměti resetovat při změně přihlášeného uživatele (nebo objekt kontextu zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="8bd9f-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="8bd9f-190">Aktuální uživatel je také označován jako objekt kontextu požadavku a lze je zobrazit v zobrazení syntaxe Razor pod položkou `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="8bd9f-191">Následující příklad hledána v aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="8bd9f-192">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="8bd9f-193">Pomocí tohoto atributu udržuje obsah v mezipaměti prostřednictvím cyklus přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="8bd9f-194">Při použití `vary-by-user="true"`, přihlášení a odhlášení akce zruší platnost mezipaměti pro ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="8bd9f-195">Mezipaměti je neplatná, protože byl vygenerován novou hodnotu jedinečný soubor cookie na přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="8bd9f-196">Mezipaměti bude zachována pro anonymní stavu, pokud žádný soubor cookie je k dispozici nebo vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="8bd9f-197">To znamená, že pokud je přihlášen žádný uživatel, se zachová mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="8bd9f-198">se liší podle</span><span class="sxs-lookup"><span data-stu-id="8bd9f-198">vary-by</span></span>

| <span data-ttu-id="8bd9f-199">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-199">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-200">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-201">String</span><span class="sxs-lookup"><span data-stu-id="8bd9f-201">String</span></span>             | <span data-ttu-id="8bd9f-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-202">"@Model"</span></span>                 |


<span data-ttu-id="8bd9f-203">Umožňuje přizpůsobení získá jaké data uložena do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="8bd9f-204">Při aktualizaci objektu odkazuje atributu řetězec hodnotu změny, obsah pomocná značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="8bd9f-205">Zřetězení řetězců modelu hodnot často jsou přiřazeny tomuto atributu.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="8bd9f-206">Efektivní, to znamená, že aktualizace zřetězených hodnot zruší platnost mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="8bd9f-207">V následujícím příkladu se předpokládá, metoda kontroleru vykreslování zobrazení součtů celočíselnou hodnotu dva parametry trasy, `myParam1` a `myParam2`a vrátí, jako vlastnost jeden model.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="8bd9f-208">Při změně této součet obsah pomocná značky mezipaměti je vykreslen a uložili do mezipaměti znovu.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="8bd9f-209">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-209">Example:</span></span>

<span data-ttu-id="8bd9f-210">Akce:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-210">Action:</span></span>

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

<span data-ttu-id="8bd9f-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8bd9f-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="8bd9f-212">Priorita</span><span class="sxs-lookup"><span data-stu-id="8bd9f-212">priority</span></span>

| <span data-ttu-id="8bd9f-213">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="8bd9f-213">Attribute Type</span></span>    | <span data-ttu-id="8bd9f-214">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="8bd9f-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8bd9f-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="8bd9f-215">CacheItemPriority</span></span>  | <span data-ttu-id="8bd9f-216">"Vysoká"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-216">"High"</span></span>                   |
|                    | <span data-ttu-id="8bd9f-217">"Nízká"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-217">"Low"</span></span> |
|                    | <span data-ttu-id="8bd9f-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="8bd9f-219">"Normální"</span><span class="sxs-lookup"><span data-stu-id="8bd9f-219">"Normal"</span></span> |

<span data-ttu-id="8bd9f-220">Obsahuje mezipaměti vyřazení pokyny k poskytovateli předdefinované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="8bd9f-221">Webový server bude vyřazení `Low` nejprve mezipaměti položky, když je paměť přetížena.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="8bd9f-222">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8bd9f-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="8bd9f-223">`priority` Atribut není zaručit konkrétní úroveň mezipaměti uchování.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="8bd9f-224">`CacheItemPriority`je pouze návrhu.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="8bd9f-225">Nastavení tohoto atributu na `NeverRemove` nezaručuje, že budou vždy zachována mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="8bd9f-226">V tématu [další prostředky](#additional-resources) Další informace.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="8bd9f-227">Pomocník značky mezipaměti je závislá na [služby mezipaměti paměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="8bd9f-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="8bd9f-228">Pomocník značky mezipaměti přidá službu, pokud nebyl přidán.</span><span class="sxs-lookup"><span data-stu-id="8bd9f-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8bd9f-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8bd9f-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
