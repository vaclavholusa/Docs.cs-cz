---
title: Injektáž závislostí v ASP.NET Core
author: guardrex
description: Zjistěte, jak ASP.NET Core implementuje injektáž závislostí a způsobu jeho použití.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 57b50512d0dfbe1b1b7d7782f6eb7e328dd99149
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211075"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="ce539-103">Injektáž závislostí v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce539-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="ce539-104">Podle [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ce539-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ce539-105">ASP.NET Core podporuje závislost vkládání (DI) software vzor návrhu, což je technika, pro dosažení [řízení IOC (Inversion)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) mezi třídami a jejich závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce539-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="ce539-106">Další informace specifické pro vkládání závislostí do kontrolerů MVC najdete v tématu <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ce539-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="ce539-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce539-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="ce539-108">Přehled injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="ce539-108">Overview of dependency injection</span></span>

<span data-ttu-id="ce539-109">A *závislost* je libovolný objekt, který vyžaduje jiný objekt.</span><span class="sxs-lookup"><span data-stu-id="ce539-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="ce539-110">Zkontrolujte následující `MyDependency` třídy s `WriteMessage` metodu, která jiných tříd v aplikaci závisí na:</span><span class="sxs-lookup"><span data-stu-id="ce539-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ce539-111">Instance `MyDependency` třídy lze provádět `WriteMessage` metody, které jsou k dispozici na třídu.</span><span class="sxs-lookup"><span data-stu-id="ce539-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="ce539-112">`MyDependency` Třídy je závislost `IndexModel` třídy:</span><span class="sxs-lookup"><span data-stu-id="ce539-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ce539-113">Instance `MyDependency` třídy lze provádět `WriteMessage` metody, které jsou k dispozici na třídu.</span><span class="sxs-lookup"><span data-stu-id="ce539-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="ce539-114">`MyDependency` Třídy je závislost `HomeController` třídy:</span><span class="sxs-lookup"><span data-stu-id="ce539-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="ce539-115">Vytvoří třídu a přímo závisí `MyDependency` instance.</span><span class="sxs-lookup"><span data-stu-id="ce539-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="ce539-116">Závislosti kódu (například v předchozím příkladu) jsou problematické a mělo by se vyhnout z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="ce539-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="ce539-117">Chcete-li nahradit `MyDependency` s jinou implementaci třídy musí být změněny.</span><span class="sxs-lookup"><span data-stu-id="ce539-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="ce539-118">Pokud `MyDependency` má závislosti, musíte je nakonfigurovat pomocí třídy.</span><span class="sxs-lookup"><span data-stu-id="ce539-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="ce539-119">Ve velkých projektech s více třídami v závislosti na `MyDependency`, kód konfigurace stane rozmístěny na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce539-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="ce539-120">Tato implementace je obtížné testování částí.</span><span class="sxs-lookup"><span data-stu-id="ce539-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="ce539-121">Aplikace by měla používat model nebo se zakázaným inzerováním `MyDependency` třídu, která není možné s tímto přístupem.</span><span class="sxs-lookup"><span data-stu-id="ce539-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="ce539-122">Injektáž závislostí řeší tyto problémy prostřednictvím:</span><span class="sxs-lookup"><span data-stu-id="ce539-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="ce539-123">Použití rozhraní abstraktní implementace závislostí.</span><span class="sxs-lookup"><span data-stu-id="ce539-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="ce539-124">Registrace závislosti v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="ce539-125">ASP.NET Core nabízí integrovanou službu kontejneru, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="ce539-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="ce539-126">Služby jsou registrované aplikace `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="ce539-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="ce539-127">*Vkládání* služby do konstruktoru třídy, ve kterém se používá.</span><span class="sxs-lookup"><span data-stu-id="ce539-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="ce539-128">Rozhraní framework přebírá odpovědnost vytvoření instance závislosti a jejich zničení, pokud už je nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="ce539-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="ce539-129">V [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` rozhraní definuje metody, která poskytuje služby do aplikace:</span><span class="sxs-lookup"><span data-stu-id="ce539-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce539-130">Toto rozhraní je implementováno podle konkrétního typu implementujícího typ `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="ce539-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce539-131">`MyDependency` Požadavky [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) 've svém konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="ce539-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="ce539-132">Není zřetězené způsobem pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="ce539-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="ce539-133">Každá požadovaná závislost zase požaduje svoje vlastní závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce539-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="ce539-134">Kontejner řeší závislosti v grafu a vrátí službu zcela přeložit.</span><span class="sxs-lookup"><span data-stu-id="ce539-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="ce539-135">Souhrnný sadu závislosti, které je třeba vyřešit se obvykle označuje jako *strom závislostí*, *graf závislosti*, nebo *graf objektu*.</span><span class="sxs-lookup"><span data-stu-id="ce539-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="ce539-136">`IMyDependency` a `ILogger<TCategoryName>` musí být zaregistrovaný v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="ce539-137">`IMyDependency` je zaregistrovaný v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce539-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ce539-138">`ILogger<TCategoryName>` registraci protokolování abstrakce infrastrukturu, takže má [služby poskytované rozhraním](#framework-provided-services) registrován ve výchozím nastavení v rámci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ce539-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="ce539-139">V ukázkové aplikaci `IMyDependency` služba není registrována s konkrétní typ `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="ce539-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="ce539-140">Registrace obory doba platnosti služby k době života jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="ce539-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="ce539-141">[Služba životnosti](#service-lifetimes) jsou popsány dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ce539-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ce539-142">Každý `services.Add{SERVICE_NAME}` – metoda rozšíření přidá (a potenciálně nakonfiguruje) služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-142">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="ce539-143">Například `services.AddMvc()` přidá služby Razor Pages a vyžadují MVC.</span><span class="sxs-lookup"><span data-stu-id="ce539-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="ce539-144">Doporučujeme, aby aplikace postupujte podle Tato konvence.</span><span class="sxs-lookup"><span data-stu-id="ce539-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="ce539-145">Rozšiřující metody v umístění [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) obor názvů pro zapouzdření skupiny registrací služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="ce539-146">Pokud konstruktor služby vyžaduje jednoduchého typu, například `string`, primitivní vlastnost může být vloženy pomocí [konfigurace](xref:fundamentals/configuration/index) nebo [možnosti vzor](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="ce539-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="ce539-147">Instance služby je požadováno prostřednictvím konstruktoru třídy, kde se služba používá a přiřadí do privátní pole.</span><span class="sxs-lookup"><span data-stu-id="ce539-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="ce539-148">Pole se používá pro přístup ke službě podle potřeby v rámci třídy.</span><span class="sxs-lookup"><span data-stu-id="ce539-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="ce539-149">V ukázkové aplikaci `IMyDependency` instance je požadováno a použít k volání služby `WriteMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="ce539-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="ce539-150">Služby poskytované rozhraním</span><span class="sxs-lookup"><span data-stu-id="ce539-150">Framework-provided services</span></span>

<span data-ttu-id="ce539-151">`Startup.ConfigureServices` Metoda odpovídá za definování služeb aplikace využívá, včetně funkcí platformy, jako je například Entity Framework Core a ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ce539-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="ce539-152">Na začátku `IServiceCollection` poskytnuté `ConfigureServices` obsahuje následující definice služby (v závislosti na [konfiguraci hostitele](xref:fundamentals/host/index)):</span><span class="sxs-lookup"><span data-stu-id="ce539-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="ce539-153">Typ služby</span><span class="sxs-lookup"><span data-stu-id="ce539-153">Service Type</span></span> | <span data-ttu-id="ce539-154">Doba platnosti</span><span class="sxs-lookup"><span data-stu-id="ce539-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="ce539-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="ce539-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="ce539-156">Přechodná</span><span class="sxs-lookup"><span data-stu-id="ce539-156">Transient</span></span> |
| [<span data-ttu-id="ce539-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="ce539-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="ce539-158">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-158">Singleton</span></span> |
| [<span data-ttu-id="ce539-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="ce539-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="ce539-160">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-160">Singleton</span></span> |
| [<span data-ttu-id="ce539-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="ce539-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="ce539-162">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-162">Singleton</span></span> |
| [<span data-ttu-id="ce539-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="ce539-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="ce539-164">Přechodná</span><span class="sxs-lookup"><span data-stu-id="ce539-164">Transient</span></span> |
| [<span data-ttu-id="ce539-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="ce539-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="ce539-166">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-166">Singleton</span></span> |
| [<span data-ttu-id="ce539-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="ce539-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="ce539-168">Přechodná</span><span class="sxs-lookup"><span data-stu-id="ce539-168">Transient</span></span> |
| [<span data-ttu-id="ce539-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="ce539-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="ce539-170">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-170">Singleton</span></span> |
| [<span data-ttu-id="ce539-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="ce539-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="ce539-172">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-172">Singleton</span></span> |
| [<span data-ttu-id="ce539-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="ce539-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="ce539-174">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-174">Singleton</span></span> |
| [<span data-ttu-id="ce539-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="ce539-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="ce539-176">Přechodná</span><span class="sxs-lookup"><span data-stu-id="ce539-176">Transient</span></span> |
| [<span data-ttu-id="ce539-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="ce539-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="ce539-178">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-178">Singleton</span></span> |
| [<span data-ttu-id="ce539-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="ce539-179">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="ce539-180">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-180">Singleton</span></span> |
| [<span data-ttu-id="ce539-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="ce539-181">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="ce539-182">singleton</span><span class="sxs-lookup"><span data-stu-id="ce539-182">Singleton</span></span> |

<span data-ttu-id="ce539-183">Metody rozšíření kolekce služby je možné zaregistrovat službu (a jeho závislé služby, pokud je to nutné), tato konvence při použití jediného `Add{SERVICE_NAME}` metodu rozšíření k registraci všech služeb vyžadují danou službu.</span><span class="sxs-lookup"><span data-stu-id="ce539-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="ce539-184">Následující kód je příklad toho, jak přidat další služby do kontejneru pomocí metody rozšíření [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span><span class="sxs-lookup"><span data-stu-id="ce539-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="ce539-185">Další informace najdete v tématu [ServiceCollection třídy](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) v dokumentaci k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ce539-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="ce539-186">Životnost služby</span><span class="sxs-lookup"><span data-stu-id="ce539-186">Service lifetimes</span></span>

<span data-ttu-id="ce539-187">Zvolte odpovídající životnost pro každé registrovanou službu.</span><span class="sxs-lookup"><span data-stu-id="ce539-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="ce539-188">ASP.NET Core services můžete nakonfigurovat následující životní cyklus:</span><span class="sxs-lookup"><span data-stu-id="ce539-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="ce539-189">**Přechodná**</span><span class="sxs-lookup"><span data-stu-id="ce539-189">**Transient**</span></span>

<span data-ttu-id="ce539-190">Přechodná doba platnosti služby se vytvoří pokaždé, když jste žádali.</span><span class="sxs-lookup"><span data-stu-id="ce539-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="ce539-191">Tato doba platnosti je nejvhodnější pro zjednodušené, bezstavových služeb.</span><span class="sxs-lookup"><span data-stu-id="ce539-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="ce539-192">**Obor**</span><span class="sxs-lookup"><span data-stu-id="ce539-192">**Scoped**</span></span>

<span data-ttu-id="ce539-193">S vymezeným oborem životnost služby se vytvoří jednou každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="ce539-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="ce539-194">Při použití služby s vymezeným oborem v middleware, vloží službu do `Invoke` nebo `InvokeAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="ce539-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="ce539-195">Prostřednictvím konstruktoru vkládání není vložit, protože nutí službě a chovají se jako singleton.</span><span class="sxs-lookup"><span data-stu-id="ce539-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="ce539-196">Další informace naleznete v tématu <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="ce539-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="ce539-197">**singleton**</span><span class="sxs-lookup"><span data-stu-id="ce539-197">**Singleton**</span></span>

<span data-ttu-id="ce539-198">Deklarace služeb typu singleton životnost se vytvoří při prvním jste žádali (nebo když `ConfigureServices` spuštění a instance je zadán s registrací služby).</span><span class="sxs-lookup"><span data-stu-id="ce539-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="ce539-199">Každý další požadavek používá stejnou instanci.</span><span class="sxs-lookup"><span data-stu-id="ce539-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="ce539-200">Pokud aplikace vyžaduje chování typu singleton, se doporučuje povolení kontejneru služby ke správě životnosti služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="ce539-201">Nemusíte implementovat vzor návrhu typu singleton a poskytnout uživatelský kód ke správě životnosti objektu ve třídě.</span><span class="sxs-lookup"><span data-stu-id="ce539-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="ce539-202">Je nebezpečné vyřešit vymezené služby z typu singleton.</span><span class="sxs-lookup"><span data-stu-id="ce539-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="ce539-203">Může to způsobit služby má nesprávný stav při zpracování následných žádostí.</span><span class="sxs-lookup"><span data-stu-id="ce539-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="ce539-204">Konstruktor chování vkládání</span><span class="sxs-lookup"><span data-stu-id="ce539-204">Constructor injection behavior</span></span>

<span data-ttu-id="ce539-205">Služby se dají vyřešit dva mechanismy:</span><span class="sxs-lookup"><span data-stu-id="ce539-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="ce539-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; umožňuje vytvoření objektu bez registrace služby v kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="ce539-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="ce539-207">`ActivatorUtilities` se používá s přístupných abstrakce, jako je například pomocných rutin značek, kontrolery MVC a vazače modelů.</span><span class="sxs-lookup"><span data-stu-id="ce539-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="ce539-208">Konstruktory mohou přijímat argumenty, které nejsou součástí injektáž závislostí, ale argumenty musí přiřadit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ce539-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="ce539-209">Když jsou vyřešeny služby `IServiceProvider` nebo `ActivatorUtilities`, vyžaduje konstruktor vkládání *veřejné* konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="ce539-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="ce539-210">Když jsou vyřešeny služby `ActivatorUtilities`, konstruktor vkládání vyžaduje tento pouze jeden použít konstruktor existuje.</span><span class="sxs-lookup"><span data-stu-id="ce539-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="ce539-211">Přetížení konstruktoru jsou podporované, ale může existovat pouze jedním přetížením, jehož argumenty lze všechny splnit vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="ce539-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="ce539-212">Kontext Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ce539-212">Entity Framework contexts</span></span>

<span data-ttu-id="ce539-213">Entity Framework kontexty má přidat do kontejneru služby pomocí vymezených životnost.</span><span class="sxs-lookup"><span data-stu-id="ce539-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="ce539-214">Tento problém řeší automaticky volání [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) metoda při registraci kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="ce539-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="ce539-215">Služby, které používají kontext databáze by měl také použít s vymezeným oborem životnost.</span><span class="sxs-lookup"><span data-stu-id="ce539-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="ce539-216">Doba života a možností registrace</span><span class="sxs-lookup"><span data-stu-id="ce539-216">Lifetime and registration options</span></span>

<span data-ttu-id="ce539-217">Chcete-li prokázali rozdíl mezi možnostmi životnost a registraci, zvažte následující rozhraní, které představují úlohy jako operaci s jedinečným identifikátorem `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="ce539-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="ce539-218">V závislosti na konfiguraci životnosti služby operace pro následující rozhraní kontejneru poskytuje stejné nebo jiné instanci služby na požádání třídou:</span><span class="sxs-lookup"><span data-stu-id="ce539-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce539-219">Rozhraní jsou implementovány v `Operation` třídy.</span><span class="sxs-lookup"><span data-stu-id="ce539-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="ce539-220">`Operation` Konstruktor vytvoří identifikátor GUID, pokud jeden není zadán:</span><span class="sxs-lookup"><span data-stu-id="ce539-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce539-221">`OperationService` Zaregistrován to záleží na každé z nich `Operation` typy.</span><span class="sxs-lookup"><span data-stu-id="ce539-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="ce539-222">Když `OperationService` žádá pomocí vkládání závislostí, obdrží buď novou instanci třídy každé služby nebo stávající instance podle doby života ze závislých služeb.</span><span class="sxs-lookup"><span data-stu-id="ce539-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="ce539-223">Pokud přechodné služby se vytvoří při požadavku `OperationsId` z `IOperationTransient` služby se liší od `OperationsId` z `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="ce539-223">If transient services are created when requested, the `OperationsId` of the `IOperationTransient` service is different than the `OperationsId` of the `OperationService`.</span></span> <span data-ttu-id="ce539-224">`OperationService` obdrží novou instanci třídy `IOperationTransient` třídy.</span><span class="sxs-lookup"><span data-stu-id="ce539-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="ce539-225">Vrací novou instanci jinou `OperationsId`.</span><span class="sxs-lookup"><span data-stu-id="ce539-225">The new instance yields a different `OperationsId`.</span></span>
* <span data-ttu-id="ce539-226">Pokud vymezené služby se vytvoří každý požadavek, `OperationsId` z `IOperationScoped` služba je stejné jako u `OperationService` v rámci požadavku.</span><span class="sxs-lookup"><span data-stu-id="ce539-226">If scoped services are created per request, the `OperationsId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="ce539-227">Obě služby napříč požadavky, sdílet jiné `OperationsId` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ce539-227">Across requests, both services share a different `OperationsId` value.</span></span>
* <span data-ttu-id="ce539-228">Pokud jsou služby typu singleton a instanci typu singleton vytvořit jednou a použít v rámci všech požadavků a všemi službami, `OperationsId` je konstantní napříč všemi požadavky služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationsId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce539-229">V `Startup.ConfigureServices`, každý typ je přidat do kontejneru podle životnosti s názvem:</span><span class="sxs-lookup"><span data-stu-id="ce539-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="ce539-230">`IOperationSingletonInstance` Služba používá konkrétní instanci s známé ID `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="ce539-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="ce539-231">Je jasné, když tento typ se používá (její identifikátor GUID se všemi nulovými hodnotami).</span><span class="sxs-lookup"><span data-stu-id="ce539-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ce539-232">Ukázková aplikace předvádí správu životnosti objektů v rámci a mezi jednotlivé požadavky.</span><span class="sxs-lookup"><span data-stu-id="ce539-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="ce539-233">Ukázková aplikace `IndexModel` požadavků každý druh `IOperation` typ a `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="ce539-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="ce539-234">Stránce se pak zobrazí všechny třídy modelu stránky a služby `OperationId` hodnoty prostřednictvím vlastnosti přiřazení:</span><span class="sxs-lookup"><span data-stu-id="ce539-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ce539-235">Ukázková aplikace předvádí správu životnosti objektů v rámci a mezi jednotlivé požadavky.</span><span class="sxs-lookup"><span data-stu-id="ce539-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="ce539-236">Obsahuje ukázkovou aplikaci `OperationsController` , že každý žádosti druh `IOperation` typ a `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="ce539-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="ce539-237">`Index` Akce nastaví služby do `ViewBag` pro zobrazení služby `OperationId` hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ce539-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce539-238">Dva následující výstup ukazuje výsledky dvou požadavků:</span><span class="sxs-lookup"><span data-stu-id="ce539-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="ce539-239">**První požadavek:**</span><span class="sxs-lookup"><span data-stu-id="ce539-239">**First request:**</span></span>

<span data-ttu-id="ce539-240">Operace kontroleru:</span><span class="sxs-lookup"><span data-stu-id="ce539-240">Controller operations:</span></span>

<span data-ttu-id="ce539-241">Přechodné: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="ce539-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="ce539-242">Obor: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="ce539-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="ce539-243">Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="ce539-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="ce539-244">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="ce539-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="ce539-245">`OperationService` operace:</span><span class="sxs-lookup"><span data-stu-id="ce539-245">`OperationService` operations:</span></span>

<span data-ttu-id="ce539-246">Přechodné: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="ce539-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="ce539-247">Obor: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="ce539-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="ce539-248">Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="ce539-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="ce539-249">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="ce539-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="ce539-250">**Druhou žádost:**</span><span class="sxs-lookup"><span data-stu-id="ce539-250">**Second request:**</span></span>

<span data-ttu-id="ce539-251">Operace kontroleru:</span><span class="sxs-lookup"><span data-stu-id="ce539-251">Controller operations:</span></span>

<span data-ttu-id="ce539-252">Přechodné: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="ce539-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="ce539-253">Obor: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="ce539-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="ce539-254">Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="ce539-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="ce539-255">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="ce539-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="ce539-256">`OperationService` operace:</span><span class="sxs-lookup"><span data-stu-id="ce539-256">`OperationService` operations:</span></span>

<span data-ttu-id="ce539-257">Přechodné: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="ce539-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="ce539-258">Obor: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="ce539-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="ce539-259">Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="ce539-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="ce539-260">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="ce539-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="ce539-261">Podívejte se, které `OperationId` hodnoty se liší v rámci požadavku a mezi požadavky:</span><span class="sxs-lookup"><span data-stu-id="ce539-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="ce539-262">*Přechodné* objekty jsou vždy odlišné.</span><span class="sxs-lookup"><span data-stu-id="ce539-262">*Transient* objects are always different.</span></span> <span data-ttu-id="ce539-263">Všimněte si, že přechodná `OperationId` hodnota prvního a druhého požadavky se liší pro obě `OperationService` operací a napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="ce539-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="ce539-264">Novou instanci se poskytuje pro každou službu a požadavek.</span><span class="sxs-lookup"><span data-stu-id="ce539-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="ce539-265">*Obor* objekty jsou stejné v rámci požadavku, ale jiné napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="ce539-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="ce539-266">*Jednotlivý prvek* objekty jsou stejné pro všechny objekty a všechny požadavky bez ohledu na to, jestli se `Operation` instance je k dispozici v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce539-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="ce539-267">Volání služby z hlavní</span><span class="sxs-lookup"><span data-stu-id="ce539-267">Call services from main</span></span>

<span data-ttu-id="ce539-268">Vytvoření [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) s [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) vyřešit vymezené služby v rámci oboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce539-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="ce539-269">Tento přístup je užitečný pro přístup k vymezené služby při spuštění počítače a spouštět úlohy inicializace.</span><span class="sxs-lookup"><span data-stu-id="ce539-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="ce539-270">Následující příklad ukazuje, jak získat kontext pro `MyScopedService` v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="ce539-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="ce539-271">Rozsah ověřování</span><span class="sxs-lookup"><span data-stu-id="ce539-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce539-272">Když aplikace běží ve vývojovém prostředí, poskytovatele služeb výchozí provádí kontroly pro ověření, že:</span><span class="sxs-lookup"><span data-stu-id="ce539-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="ce539-273">Vymezené služby nejsou přímo nebo nepřímo vyřešit z kořenové poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="ce539-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="ce539-274">Vymezené služby nejsou přímo nebo nepřímo vloženy do jednotlivých prvků.</span><span class="sxs-lookup"><span data-stu-id="ce539-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="ce539-275">Poskytovatel služeb root je vytvořen, když [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána.</span><span class="sxs-lookup"><span data-stu-id="ce539-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="ce539-276">Doba života poskytovatele služeb kořenový odpovídá životnost aplikace/serveru při zprostředkovatel začíná aplikace a je uvolněna při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce539-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="ce539-277">Vymezené služby jsou uvolněna pomocí kontejneru, který je vytvořil.</span><span class="sxs-lookup"><span data-stu-id="ce539-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="ce539-278">Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na jednotlivý prvek, protože to je jenom uvolněn pomocí Kořenový kontejner při vypnutí serveru/aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce539-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="ce539-279">Ověřování služby Obory zachytí tyto situace při `BuildServiceProvider` je volána.</span><span class="sxs-lookup"><span data-stu-id="ce539-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="ce539-280">Další informace naleznete v tématu <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="ce539-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="ce539-281">Žádost o služby</span><span class="sxs-lookup"><span data-stu-id="ce539-281">Request Services</span></span>

<span data-ttu-id="ce539-282">Žádost o služby k dispozici v rámci ASP.NET Core z `HttpContext` jsou vystaveny prostřednictvím [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) kolekce.</span><span class="sxs-lookup"><span data-stu-id="ce539-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="ce539-283">Žádost o služby představují služby nakonfigurovaný a požadovány v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce539-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="ce539-284">Pokud objekty určení závislostí, jsou tyto splněno typy nalezené v `RequestServices`, nikoli `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="ce539-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="ce539-285">Aplikace obvykle by neměly používat tyto vlastnosti přímo.</span><span class="sxs-lookup"><span data-stu-id="ce539-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="ce539-286">Místo toho požadavku typy, třídy vyžadovat prostřednictvím konstruktor třídy a povolit rozhraní framework vložit závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce539-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="ce539-287">To poskytuje třídy, které usnadňuje testování (najdete v článku [testování a ladění](xref:test/index) témata).</span><span class="sxs-lookup"><span data-stu-id="ce539-287">This yields classes that are easier to test (see the [Test and debug](xref:test/index) topics).</span></span>

> [!NOTE]
> <span data-ttu-id="ce539-288">Raději jako parametry konstruktoru pro přístup k žádosti o závislosti `RequestServices` kolekce.</span><span class="sxs-lookup"><span data-stu-id="ce539-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="ce539-289">Služby návrhu pro vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="ce539-289">Design services for dependency injection</span></span>

<span data-ttu-id="ce539-290">Osvědčené postupy jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ce539-290">Best practices are to:</span></span>

* <span data-ttu-id="ce539-291">Navrhujte služby pomocí vkládání závislostí získat jejich závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce539-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="ce539-292">Vyhněte se volání metody stavová a statické (postup známý jako [statické plevami](https://deviq.com/static-cling/)).</span><span class="sxs-lookup"><span data-stu-id="ce539-292">Avoid stateful, static method calls (a practice known as [static cling](https://deviq.com/static-cling/)).</span></span>
* <span data-ttu-id="ce539-293">Vyhněte se přímé vytváření instancí závislých tříd v rámci služeb.</span><span class="sxs-lookup"><span data-stu-id="ce539-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="ce539-294">Přímé vytvoření instance páry v odstupu kód pro konkrétní implementaci.</span><span class="sxs-lookup"><span data-stu-id="ce539-294">Direct instantiation couples the code to a particular implementation.</span></span>

<span data-ttu-id="ce539-295">Pomocí následujících [SOLID zásady z objektu orientovaný návrh](https://deviq.com/solid/), třídy aplikace jsou často přirozeně na malé, skvěle a snadno otestované.</span><span class="sxs-lookup"><span data-stu-id="ce539-295">By following the [SOLID Principles of Object Oriented Design](https://deviq.com/solid/), app classes naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="ce539-296">Pokud třída zdá se, že máte příliš mnoho vložených závislostí, je obecně znak, třída má příliš mnoho zodpovědnosti a porušuje [jedné zásadě odpovědnost (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="ce539-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="ce539-297">Pokus o Refaktorovat třídy některé z jeho zodpovědnosti přesunutím do nové třídy.</span><span class="sxs-lookup"><span data-stu-id="ce539-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="ce539-298">Mějte na paměti, která tříd modelu stránky Razor Pages a třídy kontroleru MVC byste se zaměřit na aspekty uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ce539-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="ce539-299">Obchodní pravidla a data přístup implementace podrobnosti by měly být neustále ve třídách, které jsou vhodné pro tyto [oddělení obavy](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="ce539-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="ce539-300">Vyřazení služby</span><span class="sxs-lookup"><span data-stu-id="ce539-300">Disposal of services</span></span>

<span data-ttu-id="ce539-301">Kontejner volá `Dispose` pro `IDisposable` typy ji vytvoří.</span><span class="sxs-lookup"><span data-stu-id="ce539-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="ce539-302">Pokud instance je přidat do kontejneru v uživatelském kódu, není automaticky odstraněn.</span><span class="sxs-lookup"><span data-stu-id="ce539-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="ce539-303">V ASP.NET Core 1.0, kontejner volá metodu dispose v *všechny* `IDisposable` objektů, včetně těch, které se nepovedlo vytvořit.</span><span class="sxs-lookup"><span data-stu-id="ce539-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="ce539-304">Výchozí služba kontejneru nahrazení</span><span class="sxs-lookup"><span data-stu-id="ce539-304">Default service container replacement</span></span>

<span data-ttu-id="ce539-305">Integrovaná služba kontejneru je určen pro sloužit potřebám rozhraní framework a většina uživatelů aplikací.</span><span class="sxs-lookup"><span data-stu-id="ce539-305">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="ce539-306">Doporučujeme používat integrované kontejneru, pokud potřebujete konkrétní funkce, která nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="ce539-306">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="ce539-307">Některé z funkcí podporovaných v 3. stran kontejnery nebyl nalezen v předdefinované kontejneru:</span><span class="sxs-lookup"><span data-stu-id="ce539-307">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="ce539-308">Vkládání vlastnosti</span><span class="sxs-lookup"><span data-stu-id="ce539-308">Property injection</span></span>
* <span data-ttu-id="ce539-309">Vkládání podle názvu</span><span class="sxs-lookup"><span data-stu-id="ce539-309">Injection based on name</span></span>
* <span data-ttu-id="ce539-310">Podřízené kontejnery</span><span class="sxs-lookup"><span data-stu-id="ce539-310">Child containers</span></span>
* <span data-ttu-id="ce539-311">Management vlastní doba života</span><span class="sxs-lookup"><span data-stu-id="ce539-311">Custom lifetime management</span></span>
* <span data-ttu-id="ce539-312">`Func<T>` Podpora pro opožděná inicializace</span><span class="sxs-lookup"><span data-stu-id="ce539-312">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="ce539-313">Najdete v článku [injektáž závislostí souboru readme.md](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) seznam některých kontejnerů, které podporují adaptéry.</span><span class="sxs-lookup"><span data-stu-id="ce539-313">See the [Dependency Injection readme.md file](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="ce539-314">Následující příklad nahradí kontejneru integrované s [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="ce539-314">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="ce539-315">Instalovat balíčky odpovídajícího kontejneru:</span><span class="sxs-lookup"><span data-stu-id="ce539-315">Install the appropriate container package(s):</span></span>

    * [<span data-ttu-id="ce539-316">Autofac</span><span class="sxs-lookup"><span data-stu-id="ce539-316">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
    * [<span data-ttu-id="ce539-317">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="ce539-317">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="ce539-318">Konfigurace kontejneru v `Startup.ConfigureServices` a vraťte se `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="ce539-318">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="ce539-319">Použití kontejneru 3. stran `Startup.ConfigureServices` musí vracet `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="ce539-319">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="ce539-320">Konfigurace Autofac v `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="ce539-320">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="ce539-321">Za běhu Autofac lze přeložit typy a vložení závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce539-321">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="ce539-322">Další informace o používání Autofac pomocí ASP.NET Core, najdete v článku [Autofac dokumentaci](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="ce539-322">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="ce539-323">Bezpečnost vlákna</span><span class="sxs-lookup"><span data-stu-id="ce539-323">Thread safety</span></span>

<span data-ttu-id="ce539-324">Deklarace služeb typu singleton musí být bezpečné pro vlákna.</span><span class="sxs-lookup"><span data-stu-id="ce539-324">Singleton services need to be thread safe.</span></span> <span data-ttu-id="ce539-325">Pokud služby typu singleton obsahuje závislost na přechodné služby, přechodné služby také muset být bezpečné, v závislosti, jak se používají v typu singleton.</span><span class="sxs-lookup"><span data-stu-id="ce539-325">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="ce539-326">Metoda factory jedné služby, jako je například druhý argument [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), nebude musí být bezpečné pro vlákna.</span><span class="sxs-lookup"><span data-stu-id="ce539-326">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="ce539-327">Typ, jako jsou (`static`) konstruktor, ji má zaručeno, že má být volána po jednom vlákně.</span><span class="sxs-lookup"><span data-stu-id="ce539-327">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="ce539-328">Doporučení</span><span class="sxs-lookup"><span data-stu-id="ce539-328">Recommendations</span></span>

<span data-ttu-id="ce539-329">Při práci s injektáž závislostí, mít na paměti tato doporučení:</span><span class="sxs-lookup"><span data-stu-id="ce539-329">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="ce539-330">Vyhněte se ukládání dat a konfigurace přímo do kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-330">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="ce539-331">Například by neměla uživatele nákupního košíku přidat obvykle do kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="ce539-331">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="ce539-332">Konfigurace by měl používat [možnosti vzor](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="ce539-332">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="ce539-333">Podobně nepoužívejte "vlastník dat" objekty, které existují pouze pokud chcete povolit přístup na některý objekt.</span><span class="sxs-lookup"><span data-stu-id="ce539-333">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="ce539-334">Je lepší požádat o skutečné položky pomocí vkládání závislostí, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="ce539-334">It's better to request the actual item via dependency injection, if possible.</span></span>

* <span data-ttu-id="ce539-335">Vyhněte se statické přístup ke službám (příklad staticky – zadáním [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) pro použití jinde).</span><span class="sxs-lookup"><span data-stu-id="ce539-335">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="ce539-336">Vyhněte se použití modelu služby Lokátor (například [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span><span class="sxs-lookup"><span data-stu-id="ce539-336">Avoid using the service locator pattern (for example, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span></span>

* <span data-ttu-id="ce539-337">Vyhněte se statické přístup k `HttpContext` (například [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="ce539-337">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="ce539-338">Stejně jako všechny sadu doporučení mohou nastat situace, ve kterém jsou vyžadována doporučení se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="ce539-338">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="ce539-339">Výjimky se vyskytují jen vzácně&mdash;většinou zvláštní případy v rámci samotného rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ce539-339">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="ce539-340">Injektáž závislostí je *alternativní* na vzorech přístupu statická/globální objekt.</span><span class="sxs-lookup"><span data-stu-id="ce539-340">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="ce539-341">Nebudete moci využít výhod injektáž závislostí, jsou-li zkombinovány s přístupem statický objekt.</span><span class="sxs-lookup"><span data-stu-id="ce539-341">You may not be able to realize the benefits of dependency injection if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce539-342">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ce539-342">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="ce539-343">Zápis čistý kód v ASP.NET Core s injektáž závislostí (MSDN)</span><span class="sxs-lookup"><span data-stu-id="ce539-343">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="ce539-344">Spravované kontejneru návrhu aplikace, Prelude: Kam patří kontejneru?</span><span class="sxs-lookup"><span data-stu-id="ce539-344">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="ce539-345">Princip explicitní závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce539-345">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="ce539-346">Inverze – kontejnery ovládacích prvků a vzor injektáž závislostí (Martina Fowlera)</span><span class="sxs-lookup"><span data-stu-id="ce539-346">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="ce539-347">Je nový spojovací ("vzájemné připevnění" kódu pro konkrétní implementaci)</span><span class="sxs-lookup"><span data-stu-id="ce539-347">New is Glue ("gluing" code to a particular implementation)</span></span>](https://ardalis.com/new-is-glue)
