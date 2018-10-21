---
title: Práce s distribuovanou mezipamětí v ASP.NET Core
author: ardalis
description: Zjistěte, jak používat ASP.NET Core distribuované ukládání do mezipaměti ke zlepšení výkonu aplikací a škálovatelnost, obzvláště v prostředí cloudu nebo serveru farmy.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 85da734f3ae7bcf0936888edfb6ac91d4362eef2
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477473"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="9f22b-103">Práce s distribuovanou mezipamětí v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f22b-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="9f22b-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9f22b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9f22b-105">Distribuované mezipaměti může zlepšit výkon a škálovatelnost aplikací ASP.NET Core, zvláště když jsou hostované v cloudu nebo v serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="9f22b-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="9f22b-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f22b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="9f22b-107">Co je to distribuovaná mezipaměť</span><span class="sxs-lookup"><span data-stu-id="9f22b-107">What is a distributed cache</span></span>

<span data-ttu-id="9f22b-108">Distribuované mezipaměti je sdílen více serverů aplikace (viz [mezipaměti Základy](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="9f22b-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="9f22b-109">Informace v mezipaměti nejsou uloženy v paměti jednotlivých webových serverů a data uložená v mezipaměti je k dispozici pro všechny servery aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f22b-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="9f22b-110">To poskytuje několik výhod:</span><span class="sxs-lookup"><span data-stu-id="9f22b-110">This provides several advantages:</span></span>

1. <span data-ttu-id="9f22b-111">Data uložená v mezipaměti je přeměnit na všech webových serverech.</span><span class="sxs-lookup"><span data-stu-id="9f22b-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="9f22b-112">Odlišné výsledky v závislosti na tom, které web server zpracovává jeho žádost se uživatelům nezobrazuje</span><span class="sxs-lookup"><span data-stu-id="9f22b-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="9f22b-113">Data uložená v mezipaměti odolává web server se restartuje a nasazení.</span><span class="sxs-lookup"><span data-stu-id="9f22b-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="9f22b-114">Jednotlivé webové servery můžete odebrat nebo přidány bez dopadu na mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="9f22b-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="9f22b-115">Zdrojového úložiště dat má menší počet požadavků provedených na jeho (než s více mezipaměti v paměti nebo žádnou instanci mezipaměti vůbec).</span><span class="sxs-lookup"><span data-stu-id="9f22b-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="9f22b-116">Pokud používáte SQL Server distribuované mezipaměti, některé z těchto výhod jsou pouze hodnotu true, pokud instance samostatné databáze se používá pro ukládání do mezipaměti než pro zdroj dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f22b-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="9f22b-117">Stejně jako všechny mezipaměti distribuované mezipaměti výrazně reakce můžete zlepšit vaší aplikace, protože obvykle můžete načíst data z mezipaměti mnohem rychleji než relační databáze (nebo webovou službu).</span><span class="sxs-lookup"><span data-stu-id="9f22b-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="9f22b-118">Konfigurace mezipaměti závisí na konkrétní implementaci.</span><span class="sxs-lookup"><span data-stu-id="9f22b-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="9f22b-119">Tento článek popisuje, jak nakonfigurovat i Redis a systému SQL Server distribuované ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9f22b-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="9f22b-120">Bez ohledu na to, kterou implementaci je vybrána, aplikace komunikuje s mezipamětí pomocí společného `IDistributedCache` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9f22b-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="9f22b-121">Rozhraní IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="9f22b-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="9f22b-122">`IDistributedCache` Rozhraní zahrnuje synchronní a asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="9f22b-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="9f22b-123">Rozhraní umožňuje položky, které chcete přidat, načíst a odebrat z implementace distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9f22b-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="9f22b-124">`IDistributedCache` Rozhraní zahrnuje následující metody:</span><span class="sxs-lookup"><span data-stu-id="9f22b-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="9f22b-125">**GET, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="9f22b-125">**Get, GetAsync**</span></span>

<span data-ttu-id="9f22b-126">Vezme s klíčem řetězce a načte položku v mezipaměti jako `byte[]` Pokud nalézt v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9f22b-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="9f22b-127">**Sada SetAsync**</span><span class="sxs-lookup"><span data-stu-id="9f22b-127">**Set, SetAsync**</span></span>

<span data-ttu-id="9f22b-128">Přidá položku (jako `byte[]`) do mezipaměti pomocí řetězce klíče.</span><span class="sxs-lookup"><span data-stu-id="9f22b-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="9f22b-129">**Aktualizace, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="9f22b-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="9f22b-130">Aktualizuje položku v mezipaměti na základě jeho klíče resetuje jeho časový limit klouzavé vypršení platnosti (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="9f22b-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="9f22b-131">**Odebrat, asynchronně odebere**</span><span class="sxs-lookup"><span data-stu-id="9f22b-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="9f22b-132">Odebere položku mezipaměti na základě jeho klíče.</span><span class="sxs-lookup"><span data-stu-id="9f22b-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="9f22b-133">Použít `IDistributedCache` rozhraní:</span><span class="sxs-lookup"><span data-stu-id="9f22b-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="9f22b-134">Přidání požadovaných balíčků NuGet do souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="9f22b-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="9f22b-135">Nakonfigurujte konkrétní implementaci `IDistributedCache` ve vaší `Startup` třídy `ConfigureServices` metoda a přidejte ho do kontejneru existuje.</span><span class="sxs-lookup"><span data-stu-id="9f22b-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="9f22b-136">Z aplikace [Middleware](xref:fundamentals/middleware/index) nebo třídy kontroleru MVC, požádat o instanci `IDistributedCache` z konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="9f22b-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="9f22b-137">Instance budou poskytovat [injektáž závislostí](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="9f22b-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="9f22b-138">Není nutné používat jednotlivý prvek nebo rozsah doba života pro `IDistributedCache` instance (nejméně pro předdefinované implementace).</span><span class="sxs-lookup"><span data-stu-id="9f22b-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="9f22b-139">Můžete také vytvořit instanci, bez ohledu na to může být nutné jednu (namísto použití [injektáž závislostí](../../fundamentals/dependency-injection.md)), ale váš kód může být obtížnější testovat a je v rozporu [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="9f22b-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="9f22b-140">Následující příklad ukazuje, jak použít instanci `IDistributedCache` v jednoduché middleware součásti:</span><span class="sxs-lookup"><span data-stu-id="9f22b-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="9f22b-141">Ve výše uvedeném kódu je hodnota uložená v mezipaměti číst, ale nikdy zapsány.</span><span class="sxs-lookup"><span data-stu-id="9f22b-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="9f22b-142">V této ukázce je hodnota nastavena pouze při spuštění serveru a nemění.</span><span class="sxs-lookup"><span data-stu-id="9f22b-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="9f22b-143">V případě více serverů v spustit nejnovější server přepíše jakékoli předchozí hodnoty, které byly nastavené zásadami ostatní servery.</span><span class="sxs-lookup"><span data-stu-id="9f22b-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="9f22b-144">`Get` a `Set` metody používají `byte[]` typu.</span><span class="sxs-lookup"><span data-stu-id="9f22b-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="9f22b-145">Proto se řetězcová hodnota musí být převeden pomocí `Encoding.UTF8.GetString` (pro `Get`) a `Encoding.UTF8.GetBytes` (pro `Set`).</span><span class="sxs-lookup"><span data-stu-id="9f22b-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="9f22b-146">Následující kód z *Startup.cs* ukazuje, nastaví se hodnota:</span><span class="sxs-lookup"><span data-stu-id="9f22b-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="9f22b-147">Protože `IDistributedCache` je nakonfigurovaný v `ConfigureServices` metoda, je k dispozici na `Configure` metodu jako parametr.</span><span class="sxs-lookup"><span data-stu-id="9f22b-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="9f22b-148">Přidání jako parametr vám umožní nakonfigurované instance má být poskytnuta prostřednictvím DI.</span><span class="sxs-lookup"><span data-stu-id="9f22b-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="9f22b-149">Použití Redis distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9f22b-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="9f22b-150">[Redis](https://redis.io/) je úložiště otevřít zdroj dat v paměti, což se často používá jako distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9f22b-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="9f22b-151">Můžete používat místně, a můžete nakonfigurovat [Azure Redis Cache](https://azure.microsoft.com/services/cache/) pro vaše aplikace ASP.NET Core hostovaných v Azure.</span><span class="sxs-lookup"><span data-stu-id="9f22b-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="9f22b-152">Nakonfiguruje aplikaci ASP.NET Core pomocí implementace mezipaměti `RedisDistributedCache` instance.</span><span class="sxs-lookup"><span data-stu-id="9f22b-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="9f22b-153">Mezipaměti Redis vyžaduje [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="9f22b-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="9f22b-154">Konfigurace Redis implementace v `ConfigureServices` a k němu přístup v kódu vaší aplikace můžete si vyžádat instance `IDistributedCache` (viz výše uvedený kód).</span><span class="sxs-lookup"><span data-stu-id="9f22b-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="9f22b-155">Ve vzorovém kódu `RedisCache` implementace se používá, když je server nakonfigurovaný pro `Staging` prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f22b-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="9f22b-156">Proto `ConfigureStagingServices` konfiguruje metodu `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="9f22b-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="9f22b-157">K instalaci Redis na místním počítači, nainstalujte chocolatey balíček [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) a spusťte `redis-server` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9f22b-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="9f22b-158">SQL Server pomocí distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9f22b-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="9f22b-159">Implementace SqlServerCache umožňuje distribuované mezipaměti pro použití jiné databáze systému SQL Server jako svůj záložní úložiště.</span><span class="sxs-lookup"><span data-stu-id="9f22b-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="9f22b-160">Vytvoření SQL serveru tabulku můžete použít nástroj mezipaměti sql, nástroj vytvoří tabulku s názvem a schéma, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="9f22b-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="9f22b-161">Přidat `SqlConfig.Tools` k `<ItemGroup>` element soubor projektu a spustit `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="9f22b-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="9f22b-162">Otestujte SqlConfig.Tools spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="9f22b-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="9f22b-163">SqlConfig.Tools zobrazí využití, možnosti a nápovědy k příkazům.</span><span class="sxs-lookup"><span data-stu-id="9f22b-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="9f22b-164">Vytvoření tabulky v SQL serveru spuštěním `sql-cache create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="9f22b-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="9f22b-165">Vytvořené tabulky má následující schéma:</span><span class="sxs-lookup"><span data-stu-id="9f22b-165">The created table has the following schema:</span></span>

![Tabulka mezipaměti systému SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="9f22b-167">Stejně jako všechny implementace mezipaměti by měla vaše aplikace získávat a nastavovat hodnoty mezipaměti pomocí instance `IDistributedCache`, nikoli `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="9f22b-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="9f22b-168">Implementuje vzorek `SqlServerCache` v produkčním prostředí (abyste je nakonfigurován v `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="9f22b-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="9f22b-169">`ConnectionString` (A volitelně také `SchemaName` a `TableName`) by měl obvykle být uloženy mimo vaši správy zdrojových kódů (například UserSecrets), mohou obsahovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9f22b-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="9f22b-170">Doporučení</span><span class="sxs-lookup"><span data-stu-id="9f22b-170">Recommendations</span></span>

<span data-ttu-id="9f22b-171">Při rozhodování o tom, která implementace `IDistributedCache` je přímo pro vaši aplikaci, zvolte mezi Redis a systému SQL Server na základě vaší stávající infrastruktury a prostředí, vašim požadavkům na výkon a prostředí pro váš tým.</span><span class="sxs-lookup"><span data-stu-id="9f22b-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="9f22b-172">Pokud váš tým je pohodlnější, práce s využitím Redisu, je skvělou volbou.</span><span class="sxs-lookup"><span data-stu-id="9f22b-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="9f22b-173">Pokud váš tým přednost systému SQL Server, máte jistotu, že v této implementaci stejně.</span><span class="sxs-lookup"><span data-stu-id="9f22b-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="9f22b-174">Všimněte si, že tradiční řešení ukládání do mezipaměti ukládá data v paměti, která umožňuje rychlé načítání dat.</span><span class="sxs-lookup"><span data-stu-id="9f22b-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="9f22b-175">Měli byste ukládání běžně používaných dat v mezipaměti a uložit veškerá data v trvalém úložišti back-end, jako je například SQL Server nebo služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9f22b-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="9f22b-176">Redis Cache je řešení ukládání do mezipaměti, která poskytuje vysokou propustností a nízkou latenci v porovnání s mezipaměti SQL.</span><span class="sxs-lookup"><span data-stu-id="9f22b-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f22b-177">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9f22b-177">Additional resources</span></span>

* [<span data-ttu-id="9f22b-178">Redis Cache v Azure</span><span class="sxs-lookup"><span data-stu-id="9f22b-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="9f22b-179">SQL Database v Azure</span><span class="sxs-lookup"><span data-stu-id="9f22b-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
