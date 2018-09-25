---
title: Pomocná rutina značek v ASP.NET Core MVC do mezipaměti
author: pkellner
description: Další informace o použití pomocné rutiny značky mezipaměti.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 11754d2858d8f02c7eb9baac8feda9b50ddb3d79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028152"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="22573-103">Pomocná rutina značek v ASP.NET Core MVC do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="22573-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="22573-104">Podle [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="22573-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="22573-105">Pomocná rutina značek mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti obsah k poskytovateli vnitřní mezipaměti ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22573-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="22573-106">Nastaví výchozí zobrazovací modul Razor `expires-after` až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="22573-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="22573-107">Následující kód Razor ukládá datum a čas:</span><span class="sxs-lookup"><span data-stu-id="22573-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="22573-108">První požadavek na stránku, která obsahuje `CacheTagHelper` zobrazí aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="22573-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="22573-109">Další požadavky se zobrazí hodnota uložená v mezipaměti, dokud mezipaměti vyprší (výchozí nastavení 20 minut) nebo vyřadí se tlaku na paměť.</span><span class="sxs-lookup"><span data-stu-id="22573-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="22573-110">Můžete nastavit dobu uložení do mezipaměti s následujícími atributy:</span><span class="sxs-lookup"><span data-stu-id="22573-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="22573-111">Atributy pomocné rutiny značky do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="22573-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="22573-112">Povoleno</span><span class="sxs-lookup"><span data-stu-id="22573-112">enabled</span></span>    


| <span data-ttu-id="22573-113">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-113">Attribute Type</span></span>    | <span data-ttu-id="22573-114">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="22573-115">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="22573-115">boolean</span></span>           | <span data-ttu-id="22573-116">"true" (výchozí)</span><span class="sxs-lookup"><span data-stu-id="22573-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="22573-117">"false"</span><span class="sxs-lookup"><span data-stu-id="22573-117">"false"</span></span>   |


<span data-ttu-id="22573-118">Určuje, zda se uloží do mezipaměti obsah uzavřená v pomocné rutiny značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="22573-119">Výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="22573-119">The default is `true`.</span></span>  <span data-ttu-id="22573-120">Pokud nastavit `false` této pomocné rutiny značky mezipaměti se neprojeví ukládání do mezipaměti na vykresleného výstupu.</span><span class="sxs-lookup"><span data-stu-id="22573-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="22573-121">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="22573-122">vyprší dne</span><span class="sxs-lookup"><span data-stu-id="22573-122">expires-on</span></span> 

| <span data-ttu-id="22573-123">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-123">Attribute Type</span></span> |           <span data-ttu-id="22573-124">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="22573-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="22573-125">DateTimeOffset</span></span> | <span data-ttu-id="22573-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="22573-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="22573-127">Nastaví datum absolutní vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="22573-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="22573-128">V následujícím příkladu bude ukládat do mezipaměti obsah pomocné rutiny značky mezipaměti až do 17:02:00 na 29. ledna 2025.</span><span class="sxs-lookup"><span data-stu-id="22573-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="22573-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="22573-130">Po vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="22573-130">expires-after</span></span>

| <span data-ttu-id="22573-131">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-131">Attribute Type</span></span> |        <span data-ttu-id="22573-132">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="22573-133">Časový interval</span><span class="sxs-lookup"><span data-stu-id="22573-133">TimeSpan</span></span>    | <span data-ttu-id="22573-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="22573-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="22573-135">Nastaví dobu od prvního požadavku pro ukládání do mezipaměti obsah.</span><span class="sxs-lookup"><span data-stu-id="22573-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="22573-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="22573-137">klouzavé vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="22573-137">expires-sliding</span></span>

| <span data-ttu-id="22573-138">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-138">Attribute Type</span></span> |        <span data-ttu-id="22573-139">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="22573-140">Časový interval</span><span class="sxs-lookup"><span data-stu-id="22573-140">TimeSpan</span></span>    | <span data-ttu-id="22573-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="22573-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="22573-142">Nastaví čas, který se položka mezipaměti by měla být vyřazena, pokud není přístup.</span><span class="sxs-lookup"><span data-stu-id="22573-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="22573-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="22573-144">měnit podle hlavičky</span><span class="sxs-lookup"><span data-stu-id="22573-144">vary-by-header</span></span>

| <span data-ttu-id="22573-145">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-145">Attribute Type</span></span>    | <span data-ttu-id="22573-146">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="22573-147">String</span><span class="sxs-lookup"><span data-stu-id="22573-147">String</span></span>            | <span data-ttu-id="22573-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="22573-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="22573-149">"User-Agent, kódování obsahu"</span><span class="sxs-lookup"><span data-stu-id="22573-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="22573-150">Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti při změnách.</span><span class="sxs-lookup"><span data-stu-id="22573-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="22573-151">Následující příklad monitoruje hodnota hlavičky `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="22573-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="22573-152">V příkladu uloží do mezipaměti obsah pro každé jiné `User-Agent` uvedené na webový server.</span><span class="sxs-lookup"><span data-stu-id="22573-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="22573-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="22573-154">se liší podle dotazu</span><span class="sxs-lookup"><span data-stu-id="22573-154">vary-by-query</span></span>

| <span data-ttu-id="22573-155">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-155">Attribute Type</span></span>    | <span data-ttu-id="22573-156">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="22573-157">String</span><span class="sxs-lookup"><span data-stu-id="22573-157">String</span></span>            | <span data-ttu-id="22573-158">"Vytvořit"</span><span class="sxs-lookup"><span data-stu-id="22573-158">"Make"</span></span>                |
|                   | <span data-ttu-id="22573-159">"Značku, Model"</span><span class="sxs-lookup"><span data-stu-id="22573-159">"Make,Model"</span></span> |

<span data-ttu-id="22573-160">Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, když se změní hodnota hlavičky.</span><span class="sxs-lookup"><span data-stu-id="22573-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="22573-161">Následující příklad zobrazuje hodnoty `Make` a `Model`.</span><span class="sxs-lookup"><span data-stu-id="22573-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="22573-162">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="22573-163">se liší podle postupu</span><span class="sxs-lookup"><span data-stu-id="22573-163">vary-by-route</span></span>

| <span data-ttu-id="22573-164">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-164">Attribute Type</span></span>    | <span data-ttu-id="22573-165">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="22573-166">String</span><span class="sxs-lookup"><span data-stu-id="22573-166">String</span></span>            | <span data-ttu-id="22573-167">"Vytvořit"</span><span class="sxs-lookup"><span data-stu-id="22573-167">"Make"</span></span>                |
|                   | <span data-ttu-id="22573-168">"Značku, Model"</span><span class="sxs-lookup"><span data-stu-id="22573-168">"Make,Model"</span></span> |

<span data-ttu-id="22573-169">Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud změna hodnoty parametru data trasy.</span><span class="sxs-lookup"><span data-stu-id="22573-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="22573-170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-170">Example:</span></span>

<span data-ttu-id="22573-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="22573-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="22573-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="22573-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="22573-173">se liší podle cookie</span><span class="sxs-lookup"><span data-stu-id="22573-173">vary-by-cookie</span></span>

| <span data-ttu-id="22573-174">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-174">Attribute Type</span></span>    | <span data-ttu-id="22573-175">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="22573-176">String</span><span class="sxs-lookup"><span data-stu-id="22573-176">String</span></span>            | <span data-ttu-id="22573-177">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="22573-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="22573-178">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="22573-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="22573-179">Přijímá hodnotu jedné hlavičce nebo čárkou oddělený seznam hodnot hlavičky, které aktivují aktualizace mezipaměti, pokud se změní (s) hodnoty hlavičky.</span><span class="sxs-lookup"><span data-stu-id="22573-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="22573-180">V následujícím příkladu vypadá v souboru cookie přidruženého k ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="22573-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="22573-181">Když je ověření uživatele žádosti soubor cookie nastavení aktivuje aktualizace mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="22573-182">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="22573-183">se liší podle uživatele</span><span class="sxs-lookup"><span data-stu-id="22573-183">vary-by-user</span></span>

| <span data-ttu-id="22573-184">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-184">Attribute Type</span></span>    | <span data-ttu-id="22573-185">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="22573-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="22573-186">Boolean</span></span>             | <span data-ttu-id="22573-187">"true"</span><span class="sxs-lookup"><span data-stu-id="22573-187">"true"</span></span>                  |
|                     | <span data-ttu-id="22573-188">"Nepravda" (výchozí)</span><span class="sxs-lookup"><span data-stu-id="22573-188">"false" (default)</span></span> |

<span data-ttu-id="22573-189">Určuje, zda mezipaměti by měla resetovat při změně přihlášeného uživatele (nebo objekt kontextu zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="22573-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="22573-190">Aktuální uživatel se také označuje jako instanční objekt kontextu požadavku a lze je zobrazit v zobrazení Razor odkazem `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="22573-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="22573-191">Následující příklad zjistí aktuálně přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="22573-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="22573-192">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="22573-193">Pomocí tohoto atributu uchovává obsah v mezipaměti v průběhu cyklu přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="22573-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="22573-194">Při použití `vary-by-user="true"`, přihlášení a odhlášení akce zruší platnost mezipaměti pro ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="22573-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="22573-195">Mezipaměť je neplatná, protože nová hodnota jedinečný soubor cookie byl vygenerován při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="22573-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="22573-196">Mezipaměť je zachován z důvodu anonymní stavu Pokud je k dispozici žádný soubor cookie nebo vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="22573-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="22573-197">To znamená, že pokud není přihlášen žádný uživatel, bude udržovat mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="22573-198">se liší podle</span><span class="sxs-lookup"><span data-stu-id="22573-198">vary-by</span></span>

| <span data-ttu-id="22573-199">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-199">Attribute Type</span></span> | <span data-ttu-id="22573-200">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="22573-201">String</span><span class="sxs-lookup"><span data-stu-id="22573-201">String</span></span>     |    <span data-ttu-id="22573-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="22573-202">"@Model"</span></span>    |

<span data-ttu-id="22573-203">Umožňuje přizpůsobení získá jaká data uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="22573-204">Při aktualizaci objektu, odkazuje atribut řetězec hodnotu změny, obsah pomocné rutiny značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="22573-205">Často zřetězení hodnoty modelu jsou přiřazeny k tomuto atributu.</span><span class="sxs-lookup"><span data-stu-id="22573-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="22573-206">Efektivně, to znamená, že aktualizace zřetězených hodnot zruší platnost mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="22573-207">V následujícím příkladu se předpokládá metoda kontroleru vykreslení zobrazení součtů celočíselnou hodnotu dva parametry trasy `myParam1` a `myParam2`a vrátí ji jako vlastnost jednoho modelu.</span><span class="sxs-lookup"><span data-stu-id="22573-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="22573-208">Při změně tohoto součtu obsah pomocné rutiny značky mezipaměti je vykreslen a uložili do mezipaměti znovu.</span><span class="sxs-lookup"><span data-stu-id="22573-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="22573-209">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-209">Example:</span></span>

<span data-ttu-id="22573-210">Akce:</span><span class="sxs-lookup"><span data-stu-id="22573-210">Action:</span></span>

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

<span data-ttu-id="22573-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="22573-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="22573-212">Priorita</span><span class="sxs-lookup"><span data-stu-id="22573-212">priority</span></span>

| <span data-ttu-id="22573-213">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="22573-213">Attribute Type</span></span>    | <span data-ttu-id="22573-214">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="22573-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="22573-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="22573-215">CacheItemPriority</span></span>  | <span data-ttu-id="22573-216">"Vysoká"</span><span class="sxs-lookup"><span data-stu-id="22573-216">"High"</span></span>                   |
|                    | <span data-ttu-id="22573-217">"Nízká"</span><span class="sxs-lookup"><span data-stu-id="22573-217">"Low"</span></span> |
|                    | <span data-ttu-id="22573-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="22573-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="22573-219">"Normální"</span><span class="sxs-lookup"><span data-stu-id="22573-219">"Normal"</span></span> |

<span data-ttu-id="22573-220">Obsahuje pokyny k vyřazení mezipaměti integrovanou mezipaměť na poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="22573-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="22573-221">Webový server vyřazení `Low` nejprve mezipaměti položky, když je přetížena paměť.</span><span class="sxs-lookup"><span data-stu-id="22573-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="22573-222">Příklad:</span><span class="sxs-lookup"><span data-stu-id="22573-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="22573-223">`priority` Atribut nezaručuje konkrétní úroveň mezipaměti dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="22573-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="22573-224">`CacheItemPriority` je jenom návrh.</span><span class="sxs-lookup"><span data-stu-id="22573-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="22573-225">Nastavení tohoto atributu na `NeverRemove` nezaručuje, že budou vždy uchovávat mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="22573-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="22573-226">Zobrazit [další prostředky](#additional-resources) Další informace.</span><span class="sxs-lookup"><span data-stu-id="22573-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="22573-227">Pomocná rutina značek mezipaměti je závislá na [služby mezipaměti paměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="22573-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="22573-228">Pomocná rutina značek mezipaměti přidá službu, pokud nebyla přidána.</span><span class="sxs-lookup"><span data-stu-id="22573-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22573-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="22573-229">Additional resources</span></span>

* [<span data-ttu-id="22573-230">Mezipaměť v paměti</span><span class="sxs-lookup"><span data-stu-id="22573-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="22573-231">Úvod do systému Identity</span><span class="sxs-lookup"><span data-stu-id="22573-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
