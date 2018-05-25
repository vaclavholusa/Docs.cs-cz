---
title: Ukládat do mezipaměti v paměti v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak data v paměti jádra ASP.NET do mezipaměti.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 4835e2331afca7a648abac6bc35d255ec6356067
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/24/2018
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="d9cba-103">Ukládat do mezipaměti v paměti v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9cba-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="d9cba-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Jan Luo](https://github.com/JunTaoLuo), a [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d9cba-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d9cba-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9cba-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="d9cba-106">Základní informace o ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d9cba-106">Caching basics</span></span>

<span data-ttu-id="d9cba-107">Ukládání do mezipaměti může výrazně zlepšit výkon a škálovatelnost aplikace, protože se sníží pracovní vyžadovaných ke generování obsahu.</span><span class="sxs-lookup"><span data-stu-id="d9cba-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="d9cba-108">Ukládání do mezipaměti funguje nejlépe s daty, která se zřídka mění.</span><span class="sxs-lookup"><span data-stu-id="d9cba-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="d9cba-109">Ukládání do mezipaměti zhotoví kopii dat, která lze vrátit mnohem rychlejší než z původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="d9cba-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="d9cba-110">Měli byste zápisu a testování aplikace nikdy závislý na data uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="d9cba-111">Jádro ASP.NET podporuje několik různé mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="d9cba-112">Nejjednodušší cache je založená na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), která představuje mezipaměť uložené v paměti webového serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cba-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="d9cba-113">Aplikace, které se spouštějí na serverové farmě několika serverů by měla zajistěte, aby byly relací trvalé při použití mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="d9cba-114">Trvalé relace Ujistěte se, že následné žádosti z klienta všechny přejít na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cba-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="d9cba-115">Například použití webů Azure aplikace [směrování žádostí na aplikace](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) směrovat všechny následné požadavky na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cba-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="d9cba-116">Vyžadovat non trvalé relací ve webové farmě [distribuované mezipaměti](distributed.md) se chcete vyhnout potížím konzistence mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="d9cba-117">Pro některé aplikace může podporovat distribuované mezipaměti vyšší škálování než mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="d9cba-118">Pomocí distribuované mezipaměti snižování zátěže mezipaměti paměti externího procesu.</span><span class="sxs-lookup"><span data-stu-id="d9cba-118">Using a distributed cache offloads the cache memory to an external process.</span></span> 

<span data-ttu-id="d9cba-119">`IMemoryCache` Mezipaměti bude vyřazení položky mezipaměti přetížena paměť, pokud [mezipaměti s prioritou](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) je nastaven na `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="d9cba-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="d9cba-120">Můžete nastavit `CacheItemPriority` upravit prioritu, se kterým mezipaměti vyloučí položky paměť přetížena.</span><span class="sxs-lookup"><span data-stu-id="d9cba-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="d9cba-121">Mezipaměť v paměti můžete ukládat jakýkoli objekt; rozhraní distribuované mezipaměti je omezený na `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d9cba-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="d9cba-122">Pomocí IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="d9cba-122">Using IMemoryCache</span></span>

<span data-ttu-id="d9cba-123">Ukládání do mezipaměti v paměti je *služby* , je na něj odkazovat z vaší aplikace pomocí [vkládání závislostí](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="d9cba-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="d9cba-124">Volání `AddMemoryCache` v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d9cba-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=8)] 

<span data-ttu-id="d9cba-125">Požadavku `IMemoryCache` instance v konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="d9cba-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-999)] 

<span data-ttu-id="d9cba-126">`IMemoryCache` vyžaduje balíček NuGet "Microsoft.Extensions.Caching.Memory".</span><span class="sxs-lookup"><span data-stu-id="d9cba-126">`IMemoryCache` requires NuGet package "Microsoft.Extensions.Caching.Memory".</span></span>

<span data-ttu-id="d9cba-127">Následující kód používá [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) ke kontrole, pokud čas je v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-127">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="d9cba-128">Pokud není v mezipaměti na dobu, nový záznam je vytvořen a přidán do mezipaměti s [nastavit](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="d9cba-128">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="d9cba-129">Zobrazí se aktuální čas a čas v mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="d9cba-129">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="d9cba-130">Uložená v mezipaměti `DateTime` hodnota zůstává v mezipaměti, přestože jsou požadavky v rámci časového limitu (a žádné vyřazení z důvodu přetížení paměti).</span><span class="sxs-lookup"><span data-stu-id="d9cba-130">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="d9cba-131">Následující obrázek ukazuje aktuální čas a starší čas načtení z mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="d9cba-131">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Zobrazení index se zobrazí dva různé časy.](memory/_static/time.png)

<span data-ttu-id="d9cba-133">Následující kód používá [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) a [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) data do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-133">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="d9cba-134">Následující kód volání [získat](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) se načíst uložené v mezipaměti Doba:</span><span class="sxs-lookup"><span data-stu-id="d9cba-134">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="d9cba-135">V tématu [IMemoryCache metody](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) a [CacheExtensions metody](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) popis metod mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-135">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="d9cba-136">Pomocí MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="d9cba-136">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="d9cba-137">Následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d9cba-137">The following sample:</span></span>

- <span data-ttu-id="d9cba-138">Nastaví dobu absolutní vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-138">Sets the absolute expiration time.</span></span> <span data-ttu-id="d9cba-139">Toto je maximální dobu, kterou můžete uložit do mezipaměti na položku a zabrání vzniku příliš zastaralá při nepřetržitě prodloužení klouzavé vypršení platnosti položky.</span><span class="sxs-lookup"><span data-stu-id="d9cba-139">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="d9cba-140">Nastaví klouzavou dobu vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-140">Sets a sliding expiration time.</span></span> <span data-ttu-id="d9cba-141">Hodiny klouzavé vypršení platnosti se obnoví požadavků, které přístup k této položky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-141">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="d9cba-142">Nastaví prioritu mezipaměti `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="d9cba-142">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span> 
- <span data-ttu-id="d9cba-143">Nastaví [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , bude volána po vyřazování položku z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-143">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="d9cba-144">Zpětné volání je spuštěn v jiném podprocesu z kód, který odebere položku z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-144">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a><span data-ttu-id="d9cba-145">Závislosti mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d9cba-145">Cache dependencies</span></span>

<span data-ttu-id="d9cba-146">Následující příklad ukazuje, jak vypršení platnosti položky mezipaměti, pokud vyprší závislé položky.</span><span class="sxs-lookup"><span data-stu-id="d9cba-146">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="d9cba-147">A `CancellationChangeToken` se přidá do položky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-147">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="d9cba-148">Když `Cancel` se volá na `CancellationTokenSource`, jsou vyřazování obě položky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-148">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="d9cba-149">Použití `CancellationTokenSource` umožňuje více záznamů mezipaměti určených k vyloučení jako skupina.</span><span class="sxs-lookup"><span data-stu-id="d9cba-149">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="d9cba-150">S `using` vzor ve výše uvedeném kódu položky mezipaměti vytvořit uvnitř `using` bloku zdědí nastavení vypršení platnosti a aktivační události.</span><span class="sxs-lookup"><span data-stu-id="d9cba-150">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="d9cba-151">Další poznámky</span><span class="sxs-lookup"><span data-stu-id="d9cba-151">Additional notes</span></span>

- <span data-ttu-id="d9cba-152">Při použití zpětné volání znovu naplnit položku mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="d9cba-152">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="d9cba-153">Více požadavků můžete najít hodnota uložená v mezipaměti klíče prázdný protože zpětné volání nebylo dokončeno.</span><span class="sxs-lookup"><span data-stu-id="d9cba-153">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span> 
  - <span data-ttu-id="d9cba-154">Výsledkem může být několik vláken opětovného vyplnění položka v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d9cba-154">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="d9cba-155">Když jedna položka v mezipaměti se používá k vytvoření druhého, podřízená zkopíruje vypršení platnosti tokenů a nastavení na základě času vypršení platnosti nadřazené položce.</span><span class="sxs-lookup"><span data-stu-id="d9cba-155">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="d9cba-156">Podřízená není ručního odebrání vypršela platnost, nebo aktualizace nadřazené položky.</span><span class="sxs-lookup"><span data-stu-id="d9cba-156">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9cba-157">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d9cba-157">Additional resources</span></span>

* [<span data-ttu-id="d9cba-158">Práce s distribuovanou mezipamětí</span><span class="sxs-lookup"><span data-stu-id="d9cba-158">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="d9cba-159">Detekovat změny s tokeny změn</span><span class="sxs-lookup"><span data-stu-id="d9cba-159">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="d9cba-160">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d9cba-160">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="d9cba-161">Middleware pro ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d9cba-161">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="d9cba-162">Uložení pomocné rutiny značky do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d9cba-162">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="d9cba-163">Pomocná rutina značek v distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="d9cba-163">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
