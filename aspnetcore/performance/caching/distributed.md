---
title: Práce s distribuované mezipaměti ASP.NET Core
author: ardalis
description: Další informace o použití ASP.NET Core distribuované ukládání do mezipaměti ke zlepšení aplikace výkon a škálovatelnost, zejména v prostředí cloudu nebo server farmy.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 5ddc3a6927652f773ab38f93db1e222c5a1900b3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077696"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="09f30-103">Práce s distribuované mezipaměti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09f30-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="09f30-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="09f30-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="09f30-105">Distribuované mezipaměti může zvýšit výkon a škálovatelnost aplikací ASP.NET Core, zejména v případě, že hostovaný v cloudu nebo server prostředí farmy.</span><span class="sxs-lookup"><span data-stu-id="09f30-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="09f30-106">Tento článek vysvětluje, jak pracovat s implementace a abstrakce předdefinované distribuované mezipaměti ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09f30-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="09f30-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="09f30-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="09f30-108">Co je distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-108">What is a distributed cache</span></span>

<span data-ttu-id="09f30-109">Distribuované mezipaměti, je sdílen více serverů aplikace (viz [mezipaměti Základy](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="09f30-109">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="09f30-110">Informace v mezipaměti nejsou uloženy v paměti jednotlivých webových serverů a data uložená v mezipaměti je k dispozici pro všechny servery aplikace.</span><span class="sxs-lookup"><span data-stu-id="09f30-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="09f30-111">To poskytuje několik výhod:</span><span class="sxs-lookup"><span data-stu-id="09f30-111">This provides several advantages:</span></span>

1. <span data-ttu-id="09f30-112">Data uložená v mezipaměti je souvislý ve všech webových serverech.</span><span class="sxs-lookup"><span data-stu-id="09f30-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="09f30-113">Uživatelé neuvidí odlišné výsledky v závislosti na tom, které webový server zpracovává jeho žádost</span><span class="sxs-lookup"><span data-stu-id="09f30-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="09f30-114">Data uložená v mezipaměti odolává webový server se restartuje a nasazení.</span><span class="sxs-lookup"><span data-stu-id="09f30-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="09f30-115">Jednotlivé webové servery můžete odebrat nebo přidány bez mezipaměti, které mají vliv.</span><span class="sxs-lookup"><span data-stu-id="09f30-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="09f30-116">Zdrojového úložiště dat obsahuje méně požadavky na jeho (než s více mezipaměti v paměti nebo ne mezipaměti vůbec).</span><span class="sxs-lookup"><span data-stu-id="09f30-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="09f30-117">Pokud používáte SQL Server distribuované mezipaměti, některé tyto výhody jsou pouze hodnotu true, pokud instance samostatné databáze se používá pro mezipaměť než pro zdroj dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="09f30-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="09f30-118">Stejně jako všechny mezipaměť můžete distribuované mezipaměti výrazné zlepšení aplikace odezvy, vzhledem k tomu obvykle můžete načíst data z mezipaměti mnohem rychlejší než z relační databáze (nebo webové služby).</span><span class="sxs-lookup"><span data-stu-id="09f30-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="09f30-119">Konfigurace mezipaměti je konkrétní implementace.</span><span class="sxs-lookup"><span data-stu-id="09f30-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="09f30-120">Tento článek popisuje, jak nakonfigurovat i Redis a ukládá do mezipaměti distribuované systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="09f30-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="09f30-121">Bez ohledu na to, které implementace je vybrána, aplikace komunikuje s mezipamětí pomocí společného `IDistributedCache` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="09f30-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="09f30-122">Rozhraní IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="09f30-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="09f30-123">`IDistributedCache` Rozhraní zahrnuje synchronní a asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="09f30-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="09f30-124">Rozhraní umožňuje položky přidat, načíst a odebrat z implementace distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="09f30-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="09f30-125">`IDistributedCache` Rozhraní zahrnuje následující metody:</span><span class="sxs-lookup"><span data-stu-id="09f30-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="09f30-126">**GET, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="09f30-126">**Get, GetAsync**</span></span>

<span data-ttu-id="09f30-127">Přebírá řetězec klíč a načte jako položka v mezipaměti `byte[]` Pokud nalezen v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="09f30-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="09f30-128">**Sada, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="09f30-128">**Set, SetAsync**</span></span>

<span data-ttu-id="09f30-129">Přidá položku (jako `byte[]`) do mezipaměti pomocí klíče řetězec.</span><span class="sxs-lookup"><span data-stu-id="09f30-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="09f30-130">**Aktualizace, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="09f30-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="09f30-131">Aktualizuje položky v mezipaměti na základě jeho klíče resetování jeho klouzavé vypršení časového limitu (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="09f30-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="09f30-132">**Odebrat, asynchronně odebere**</span><span class="sxs-lookup"><span data-stu-id="09f30-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="09f30-133">Odebere položku mezipaměti na základě jeho klíče.</span><span class="sxs-lookup"><span data-stu-id="09f30-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="09f30-134">Chcete-li použít `IDistributedCache` rozhraní:</span><span class="sxs-lookup"><span data-stu-id="09f30-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="09f30-135">Do souboru projektu přidáte požadované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="09f30-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="09f30-136">Nakonfigurujte konkrétní implementace `IDistributedCache` ve vaší `Startup` třídy `ConfigureServices` metoda a přidejte ji do kontejneru existuje.</span><span class="sxs-lookup"><span data-stu-id="09f30-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="09f30-137">Z aplikace [Middleware](xref:fundamentals/middleware/index) nebo třídy řadiče MVC, požádejte o instanci `IDistributedCache` z konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="09f30-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="09f30-138">Instance poskytovaný [vkládání závislostí](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="09f30-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="09f30-139">Není nutné používat Singleton nebo Scoped doba života pro `IDistributedCache` instance (alespoň pro předdefinované implementace).</span><span class="sxs-lookup"><span data-stu-id="09f30-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="09f30-140">Můžete také vytvořit instance, bez ohledu na jednu může být nutné (místo použití [vkládání závislostí](../../fundamentals/dependency-injection.md)), ale může být váš kód těžší chcete otestovat a je v rozporu [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="09f30-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="09f30-141">Následující příklad ukazuje, jak použít instanci `IDistributedCache` v komponentě jednoduché middleware:</span><span class="sxs-lookup"><span data-stu-id="09f30-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="09f30-142">Ve výše uvedeném kódu je hodnota uložená v mezipaměti pro čtení, ale nikdy zapsána.</span><span class="sxs-lookup"><span data-stu-id="09f30-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="09f30-143">V této ukázce je hodnota nastavit pouze při spuštění serveru a nezmění.</span><span class="sxs-lookup"><span data-stu-id="09f30-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="09f30-144">Ve scénáři s více serverů přepíše nejnovější serveru spusťte všechny předchozí hodnoty, které byly nastavené zásadami jiných serverů.</span><span class="sxs-lookup"><span data-stu-id="09f30-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="09f30-145">`Get` a `Set` pomocí metody `byte[]` typu.</span><span class="sxs-lookup"><span data-stu-id="09f30-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="09f30-146">Proto řetězcovou hodnotu převést pomocí `Encoding.UTF8.GetString` (pro `Get`) a `Encoding.UTF8.GetBytes` (pro `Set`).</span><span class="sxs-lookup"><span data-stu-id="09f30-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="09f30-147">Následující kód z *Startup.cs* ukazuje, nastaví se hodnota:</span><span class="sxs-lookup"><span data-stu-id="09f30-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="09f30-148">Vzhledem k tomu `IDistributedCache` je nakonfigurovaný v `ConfigureServices` metoda, je k dispozici `Configure` metoda jako parametr.</span><span class="sxs-lookup"><span data-stu-id="09f30-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="09f30-149">Přidání jako parametr vám umožní nakonfigurované instance poskytované prostřednictvím DI.</span><span class="sxs-lookup"><span data-stu-id="09f30-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="09f30-150">Použití Redis distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="09f30-151">[Redis](https://redis.io/) je úložiště dat v paměti s otevřeným zdrojem, který se často používá jako distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="09f30-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="09f30-152">Můžete ji použít místně, a můžete nakonfigurovat [Azure Redis Cache](https://azure.microsoft.com/services/cache/) pro vaše aplikace Azure hostovaná ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09f30-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="09f30-153">Aplikace ASP.NET Core nakonfiguruje mezipaměti pomocí implementace `RedisDistributedCache` instance.</span><span class="sxs-lookup"><span data-stu-id="09f30-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="09f30-154">Konfigurace v implementaci Redis v `ConfigureServices` a k němu přístup v kódu aplikace tím, že požádá o instanci `IDistributedCache` (viz výše uvedeném kódu).</span><span class="sxs-lookup"><span data-stu-id="09f30-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="09f30-155">V ukázkovém kódu `RedisCache` implementace se používá, když je server nakonfigurovaný pro `Staging` prostředí.</span><span class="sxs-lookup"><span data-stu-id="09f30-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="09f30-156">Proto `ConfigureStagingServices` metoda nakonfiguruje `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="09f30-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="09f30-157">Na místním počítači nainstalovat Redis, nainstalujte balíček chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) a spusťte `redis-server` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="09f30-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="09f30-158">Pomocí systému SQL Server distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="09f30-159">Implementace SqlServerCache umožňuje distribuované mezipaměti používat databázi systému SQL Server jako své úložiště zálohování.</span><span class="sxs-lookup"><span data-stu-id="09f30-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="09f30-160">Chcete-li vytvořit systému SQL Server tabulku můžete použít nástroj sql mezipaměti, nástroj vytvoří tabulku s názvem a schéma, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="09f30-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="09f30-161">Přidat `SqlConfig.Tools` k `<ItemGroup>` element souboru projektu a spusťte `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="09f30-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="09f30-162">Otestujte SqlConfig.Tools spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="09f30-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="09f30-163">SqlConfig.Tools zobrazí využití, možnosti a nápovědu k příkazu.</span><span class="sxs-lookup"><span data-stu-id="09f30-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="09f30-164">Umožňuje vytvořit tabulku v systému SQL Server spuštěna `sql-cache create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="09f30-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="09f30-165">Vytvoření tabulky má následující schéma:</span><span class="sxs-lookup"><span data-stu-id="09f30-165">The created table has the following schema:</span></span>

![Tabulka mezipaměti SqlServer](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="09f30-167">Podobně jako všechny implementace mezipaměti, musí aplikace získávat a nastavovat hodnoty mezipaměti pomocí instance `IDistributedCache`, nikoli `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="09f30-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="09f30-168">Ukázka implementuje `SqlServerCache` v provozním prostředí (tak, že je nakonfigurovaný v `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="09f30-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="09f30-169">`ConnectionString` (A volitelně `SchemaName` a `TableName`) by měl obvykle být uloženy mimo zdrojového kódu (například UserSecrets), mohou obsahovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="09f30-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="09f30-170">Doporučení</span><span class="sxs-lookup"><span data-stu-id="09f30-170">Recommendations</span></span>

<span data-ttu-id="09f30-171">Při rozhodování, kdy zavedení `IDistributedCache` je pro aplikace, vyberte mezi Redis a SQL Server na základě vaší stávající infrastruktury a prostředí, vašim požadavkům na výkon a prostředí vašeho týmu.</span><span class="sxs-lookup"><span data-stu-id="09f30-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="09f30-172">Pokud váš tým pohodlnější práce s Redis, je ideální volbou.</span><span class="sxs-lookup"><span data-stu-id="09f30-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="09f30-173">Pokud váš tým upřednostňuje systému SQL Server, může být jisti, že implementace také.</span><span class="sxs-lookup"><span data-stu-id="09f30-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="09f30-174">Všimněte si, že tradiční řešení ukládání do mezipaměti ukládá data v paměti, která umožňuje rychlé načítání dat.</span><span class="sxs-lookup"><span data-stu-id="09f30-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="09f30-175">Měli uložit často používaná data do mezipaměti a uložení celého datového v trvalém úložišti back-end, jako je například SQL Server nebo úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="09f30-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="09f30-176">Mezipaměť redis systému je řešení ukládání do mezipaměti, která umožňuje vysoké prostupnosti a nízké latence porovnání mezipaměti SQL.</span><span class="sxs-lookup"><span data-stu-id="09f30-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09f30-177">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="09f30-177">Additional resources</span></span>

* [<span data-ttu-id="09f30-178">Redis Cache v Azure</span><span class="sxs-lookup"><span data-stu-id="09f30-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="09f30-179">Databáze SQL v Azure</span><span class="sxs-lookup"><span data-stu-id="09f30-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="09f30-180">Mezipaměť v paměti</span><span class="sxs-lookup"><span data-stu-id="09f30-180">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="09f30-181">Detekovat změny s tokeny změn</span><span class="sxs-lookup"><span data-stu-id="09f30-181">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="09f30-182">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-182">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="09f30-183">Middleware pro ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-183">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="09f30-184">Uložení pomocné rutiny značky do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-184">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="09f30-185">Pomocná rutina značek v distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="09f30-185">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
