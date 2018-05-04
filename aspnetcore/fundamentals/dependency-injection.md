---
title: Vkládání závislostí v ASP.NET Core
author: ardalis
description: Zjistěte, jak ASP.NET Core implementuje vkládání závislostí a způsobu jeho použití.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a105f835dddfcd0e9f32059e644f60dc1fdbbe1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="bf248-103">Vkládání závislostí v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf248-103">Dependency injection in ASP.NET Core</span></span>

<a name="fundamentals-dependency-injection"></a>

<span data-ttu-id="bf248-104">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="bf248-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="bf248-105">ASP.NET Core slouží od základů se na podporu a využít vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="bf248-105">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="bf248-106">Aplikace ASP.NET Core můžete využít předdefinované framework služby tak, že je vloženy do metody ve třídě, spuštění a aplikačních služeb můžete nakonfigurovat pro vkládání také.</span><span class="sxs-lookup"><span data-stu-id="bf248-106">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="bf248-107">Výchozí kontejner služeb poskytovaných ASP.NET Core poskytuje minimální funkce nastavit a není určena k nahrazení jiných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="bf248-107">The default services container provided by ASP.NET Core provides a minimal feature set and isn't intended to replace other containers.</span></span>

<span data-ttu-id="bf248-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf248-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="bf248-109">Co je vkládání závislostí?</span><span class="sxs-lookup"><span data-stu-id="bf248-109">What is dependency injection?</span></span>

<span data-ttu-id="bf248-110">Vkládání závislostí (DI) je technika k dosažení volné párování mezi objekty a jejich spolupracovníci nebo závislosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-110">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="bf248-111">Ne přímo vytváření instancí spolupracovníci, nebo pomocí statické odkazy jsou uvedené objekty třídy musí za účelem provádění akcí k třídě nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="bf248-111">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="bf248-112">Nejčastěji se třídy deklarovat závislé prostřednictvím jejich konstruktoru, což jim umožní postupujte podle [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="bf248-112">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="bf248-113">Tento postup se označuje jako "konstruktor vkládání".</span><span class="sxs-lookup"><span data-stu-id="bf248-113">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="bf248-114">Když třídy jsou navrženy s DI pamatovat, budou se více volně vázány protože nemají přímé, pevně zakódované závislosti na jejich spolupracovníci.</span><span class="sxs-lookup"><span data-stu-id="bf248-114">When classes are designed with DI in mind, they're more loosely coupled because they don't have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="bf248-115">To odpovídá [závislostí inverzi Princip](http://deviq.com/dependency-inversion-principle/), která uvádí, že *"by neměl vysokou úroveň moduly závisí na nízkou úroveň moduly; obě by měl záviset na abstrakce."*</span><span class="sxs-lookup"><span data-stu-id="bf248-115">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules shouldn't depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="bf248-116">Místo odkazující na konkrétní implementace, třídy požadavku abstrakce (obvykle `interfaces`) které jsou poskytovány k nim-li třída je vytvořená.</span><span class="sxs-lookup"><span data-stu-id="bf248-116">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="bf248-117">Extrahování závislosti do rozhraní a poskytování implementace tato rozhraní jako parametry je také příklad [vzoru návrhu strategie](http://deviq.com/strategy-design-pattern/).</span><span class="sxs-lookup"><span data-stu-id="bf248-117">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="bf248-118">Při systému je navržen pro použití DI, s mnoho tříd, které žádají o jejich závislosti prostřednictvím jejich – konstruktor (nebo vlastnosti), je vhodné mít třídu vyhrazený pro vytváření těchto tříd s jejich přidružené závislosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-118">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="bf248-119">Tyto třídy jsou označovány jako *kontejnery*, nebo přesněji řečeno, [inverzi řízení (IoC)](http://deviq.com/inversion-of-control/) kontejnery nebo kontejnerů vkládání závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="bf248-119">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="bf248-120">Kontejner je v podstatě objekt factory, který zodpovídá za poskytování instancí typů, které jsou požadovány z něj.</span><span class="sxs-lookup"><span data-stu-id="bf248-120">A container is essentially a factory that's responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="bf248-121">Pokud daný typ deklaruje, že má závislosti a kontejneru byl nakonfigurován pro zadejte typy závislostí, vytvoří závislosti v rámci vytváření požadovaný instance.</span><span class="sxs-lookup"><span data-stu-id="bf248-121">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="bf248-122">Tímto způsobem lze zadat grafy složitých závislostí na třídy bez nutnosti jakékoli konstrukce pevně objektu.</span><span class="sxs-lookup"><span data-stu-id="bf248-122">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="bf248-123">Kromě vytváření objektů pomocí jejich závislosti, Spravovat kontejnery obvykle životnosti objektů v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bf248-123">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="bf248-124">ASP.NET Core obsahuje jednoduché předdefinované kontejner (reprezentována `IServiceProvider` rozhraní), která podporuje vkládání konstruktor ve výchozím nastavení a ASP.NET zpřístupní určité služby prostřednictvím DI.</span><span class="sxs-lookup"><span data-stu-id="bf248-124">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="bf248-125">SOUBOR ASP. Kontejner pro NET odkazuje na typy spravuje jako *služby*.</span><span class="sxs-lookup"><span data-stu-id="bf248-125">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="bf248-126">Ve zbývající části tohoto článku *služby* bude odkazovat na typy, které jsou spravovány nástrojem kontejner IoC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bf248-126">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="bf248-127">Konfigurace služby předdefinované kontejneru v `ConfigureServices` metoda ve vaší aplikaci `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="bf248-127">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="bf248-128">Martin Fowler zápisem rozsáhlé článek na [inverzi kontejnery ovládacích prvků a vzor vkládání závislostí](https://www.martinfowler.com/articles/injection.html).</span><span class="sxs-lookup"><span data-stu-id="bf248-128">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="bf248-129">Microsoft Patterns and Practices má také skvělé popis [vkládání závislostí](https://msdn.microsoft.com/library/hh323705.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf248-129">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="bf248-130">Tento článek se zabývá vkládání závislostí, která se vztahuje na všechny aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf248-130">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="bf248-131">Vkládání závislostí v rámci řadiče MVC je popsaná v [řadiče a vkládání závislostí](../mvc/controllers/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bf248-131">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="bf248-132">Chování vkládání – konstruktor</span><span class="sxs-lookup"><span data-stu-id="bf248-132">Constructor injection behavior</span></span>

<span data-ttu-id="bf248-133">Konstruktor vkládání vyžaduje, aby v konstruktoru *veřejné*.</span><span class="sxs-lookup"><span data-stu-id="bf248-133">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="bf248-134">Jinak vyvolá výjimku aplikace `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="bf248-134">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="bf248-135">Nepodařilo se najít vhodný konstruktor pro typ 'YourType'.</span><span class="sxs-lookup"><span data-stu-id="bf248-135">A suitable constructor for type 'YourType' couldn't be located.</span></span> <span data-ttu-id="bf248-136">Zkontrolujte typ je konkrétní a služby jsou registrované pro všechny parametry veřejný konstruktor.</span><span class="sxs-lookup"><span data-stu-id="bf248-136">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>

<span data-ttu-id="bf248-137">Konstruktor vkládání vyžaduje, že pouze jeden použít konstruktor neexistuje.</span><span class="sxs-lookup"><span data-stu-id="bf248-137">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="bf248-138">Konstruktor přetížení jsou podporované, ale může existovat jenom jeden přetížení, jejichž argumenty lze všechny splnit vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="bf248-138">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="bf248-139">Pokud existuje více než jeden, vyvolá výjimku aplikace `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="bf248-139">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="bf248-140">Více konstruktory přijímá všechny typy daný argument byly zjištěny v typu 'YourType'.</span><span class="sxs-lookup"><span data-stu-id="bf248-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="bf248-141">Měla by existovat pouze jedna použít konstruktor.</span><span class="sxs-lookup"><span data-stu-id="bf248-141">There should only be one applicable constructor.</span></span>

<span data-ttu-id="bf248-142">Konstruktory může přijmout argumenty, které nejsou součástí vkládání závislostí, ale tyto musí podporovat výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bf248-142">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="bf248-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf248-143">For example:</span></span>

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a><span data-ttu-id="bf248-144">Pomocí služby poskytované framework</span><span class="sxs-lookup"><span data-stu-id="bf248-144">Using framework-provided services</span></span>

<span data-ttu-id="bf248-145">`ConfigureServices` Metoda v `Startup` třída je odpovědná za definici služby, aplikace bude používat, včetně funkcí platformy jako Entity Framework Core a ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="bf248-145">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="bf248-146">Standardně `IServiceCollection` poskytované `ConfigureServices` má následující služby definované (v závislosti na [konfigurace hostitele](xref:fundamentals/hosting)):</span><span class="sxs-lookup"><span data-stu-id="bf248-146">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/hosting)):</span></span>

| <span data-ttu-id="bf248-147">Typ služby</span><span class="sxs-lookup"><span data-stu-id="bf248-147">Service Type</span></span> | <span data-ttu-id="bf248-148">Doba platnosti</span><span class="sxs-lookup"><span data-stu-id="bf248-148">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="bf248-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="bf248-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="bf248-150">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-150">Singleton</span></span> |
| [<span data-ttu-id="bf248-151">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="bf248-151">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="bf248-152">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-152">Singleton</span></span> |
| [<span data-ttu-id="bf248-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="bf248-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="bf248-154">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-154">Singleton</span></span> |
| [<span data-ttu-id="bf248-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="bf248-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="bf248-156">Dočasná</span><span class="sxs-lookup"><span data-stu-id="bf248-156">Transient</span></span> |
| [<span data-ttu-id="bf248-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="bf248-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="bf248-158">Dočasná</span><span class="sxs-lookup"><span data-stu-id="bf248-158">Transient</span></span> |
| [<span data-ttu-id="bf248-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="bf248-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="bf248-160">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-160">Singleton</span></span> |
| [<span data-ttu-id="bf248-161">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="bf248-161">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="bf248-162">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-162">Singleton</span></span> |
| [<span data-ttu-id="bf248-163">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="bf248-163">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="bf248-164">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-164">Singleton</span></span> |
| [<span data-ttu-id="bf248-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="bf248-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="bf248-166">Dočasná</span><span class="sxs-lookup"><span data-stu-id="bf248-166">Transient</span></span> |
| [<span data-ttu-id="bf248-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="bf248-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="bf248-168">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-168">Singleton</span></span> |
| [<span data-ttu-id="bf248-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="bf248-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="bf248-170">Dočasná</span><span class="sxs-lookup"><span data-stu-id="bf248-170">Transient</span></span> |
| [<span data-ttu-id="bf248-171">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="bf248-171">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="bf248-172">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-172">Singleton</span></span> |
| [<span data-ttu-id="bf248-173">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="bf248-173">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="bf248-174">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-174">Singleton</span></span> |
| [<span data-ttu-id="bf248-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="bf248-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="bf248-176">singleton</span><span class="sxs-lookup"><span data-stu-id="bf248-176">Singleton</span></span> |

<span data-ttu-id="bf248-177">Dole je příklad toho, jak přidat další služby ke kontejneru s počtem rozšiřující metody jako `AddDbContext`, `AddIdentity`, a `AddMvc`.</span><span class="sxs-lookup"><span data-stu-id="bf248-177">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="bf248-178">Funkce a middleware technologii ASP.NET, jako je například MVC, postupujte podle konvence použití jedné přidat*ServiceName* metody rozšíření pro všechny služby, tato funkce vyžaduje registraci.</span><span class="sxs-lookup"><span data-stu-id="bf248-178">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

> [!TIP]
> <span data-ttu-id="bf248-179">Můžete požádat o určité zadaný framework služby v rámci `Startup` najdete v části metody prostřednictvím jejich seznamy parametrů - [spuštění aplikace](startup.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-179">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-services"></a><span data-ttu-id="bf248-180">Registrace služby</span><span class="sxs-lookup"><span data-stu-id="bf248-180">Registering services</span></span>

<span data-ttu-id="bf248-181">Takto můžete zaregistrovat vlastní aplikačních služeb.</span><span class="sxs-lookup"><span data-stu-id="bf248-181">You can register your own application services as follows.</span></span> <span data-ttu-id="bf248-182">První obecný typ představuje typ (obvykle rozhraní), který bude vyžádána z kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf248-182">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="bf248-183">Druhý obecný typ představuje konkrétní typ, který mohl vytvořit jeho instanci kontejneru, který se používá ke splnění těchto požadavků.</span><span class="sxs-lookup"><span data-stu-id="bf248-183">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="bf248-184">Každý `services.Add<ServiceName>` metoda rozšíření přidá (a potenciálně nakonfiguruje) služby.</span><span class="sxs-lookup"><span data-stu-id="bf248-184">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="bf248-185">Například `services.AddMvc()` přidá služby MVC požaduje.</span><span class="sxs-lookup"><span data-stu-id="bf248-185">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="bf248-186">Je doporučeno podle touto konvencí, umístění rozšiřujících metod v `Microsoft.Extensions.DependencyInjection` obor názvů pro zapouzdření skupiny registrací služby.</span><span class="sxs-lookup"><span data-stu-id="bf248-186">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="bf248-187">`AddTransient` Metoda se používá k mapování abstraktní typy na konkrétní služby, které jsou vytvářeny samostatně instance pro každý objekt, který vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="bf248-187">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="bf248-188">To se označuje jako služby *životnost*, a další životnost možnosti jsou popsané níže.</span><span class="sxs-lookup"><span data-stu-id="bf248-188">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="bf248-189">Je důležité, abyste zvolili příslušné doba platnosti pro jednotlivé služby, které je zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="bf248-189">It's important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="bf248-190">Novou instanci třídy služby poskytovat pro různé třídy, která požaduje?</span><span class="sxs-lookup"><span data-stu-id="bf248-190">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="bf248-191">Má být použita jedna instance v rámci dané webové žádosti?</span><span class="sxs-lookup"><span data-stu-id="bf248-191">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="bf248-192">Nebo se jedna instance mají použít pro životního cyklu aplikace?</span><span class="sxs-lookup"><span data-stu-id="bf248-192">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="bf248-193">V ukázce pro tento článek je jednoduchý kontroler, který se zobrazí názvy znaků, názvem `CharactersController`.</span><span class="sxs-lookup"><span data-stu-id="bf248-193">In the sample for this article, there's a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="bf248-194">Jeho `Index` metoda zobrazí aktuální seznam znaků, které byly uloženy v aplikaci a inicializuje kolekci pár znaků, pokud žádný neexistuje.</span><span class="sxs-lookup"><span data-stu-id="bf248-194">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="bf248-195">Všimněte si, že i když tato aplikace používá Entity Framework Core a `ApplicationDbContext` třídy pro jeho stálost, žádná které je zřejmá v řadiči.</span><span class="sxs-lookup"><span data-stu-id="bf248-195">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that's apparent in the controller.</span></span> <span data-ttu-id="bf248-196">Místo toho má byla abstrahované mechanismus přístupu konkrétním data za rozhraní, `ICharacterRepository`, které způsobem [použitému vzoru](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="bf248-196">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="bf248-197">Instance `ICharacterRepository` je požadována prostřednictvím konstruktoru a přiřazená privátní pole, které se pak použije k přístup ke znakům podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="bf248-197">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="bf248-198">`ICharacterRepository` Definuje tyto dvě metody kontroleru musí pracovat s `Character` instance.</span><span class="sxs-lookup"><span data-stu-id="bf248-198">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="bf248-199">Toto rozhraní je implementováno zase podle konkrétního typu, `CharacterRepository`, který se používá za běhu.</span><span class="sxs-lookup"><span data-stu-id="bf248-199">This interface is in turn implemented by a concrete type, `CharacterRepository`, that's used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="bf248-200">Způsob DI se používá s `CharacterRepository` obecné modelu, můžete provést u všech aplikačních služeb, ne jenom při "úložiště" nebo data přístupové třídy je třída.</span><span class="sxs-lookup"><span data-stu-id="bf248-200">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="bf248-201">Všimněte si, že `CharacterRepository` požadavky `ApplicationDbContext` v jeho konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bf248-201">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="bf248-202">Není pro vkládání závislostí, který se má použít zřetězené způsobem tímto způsobem, se každý požadovaný závislostí zase vyžaduje vlastní závislosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-202">It's not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="bf248-203">Kontejner je zodpovědná za řešení všechny závislé součásti v tomto grafu a vrácení službu plně vyřešený.</span><span class="sxs-lookup"><span data-stu-id="bf248-203">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="bf248-204">Vytváření požadovaný objekt a všechny objekty vyžaduje a všechny objekty těch, které vyžadují, se někdy označuje jako *grafu objektu*.</span><span class="sxs-lookup"><span data-stu-id="bf248-204">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="bf248-205">Podobně souhrnný sadu závislosti, které je třeba vyřešit se obvykle označuje jako *strom závislosti* nebo *graf závislostí*.</span><span class="sxs-lookup"><span data-stu-id="bf248-205">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="bf248-206">V takovém případě obě `ICharacterRepository` a naopak `ApplicationDbContext` musí být zaregistrované v kontejneru služby v `ConfigureServices` v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bf248-206">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="bf248-207">`ApplicationDbContext` je nakonfigurován pomocí volání metody rozšíření `AddDbContext<T>`.</span><span class="sxs-lookup"><span data-stu-id="bf248-207">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="bf248-208">Následující kód ukazuje registrace `CharacterRepository` typu.</span><span class="sxs-lookup"><span data-stu-id="bf248-208">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="bf248-209">Kontexty Entity Framework musí být přidaní do ke kontejneru služby pomocí `Scoped` životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="bf248-209">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="bf248-210">To se stará automaticky Pokud používáte metody helper, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="bf248-210">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="bf248-211">Úložiště, které bude nutné používat rozhraní Entity Framework by měli používat stejnou dobu životnosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-211">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

> [!WARNING]
> <span data-ttu-id="bf248-212">Je řešení hlavní nebezpečí k věnujte pozornost `Scoped` služby z typu singleton.</span><span class="sxs-lookup"><span data-stu-id="bf248-212">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="bf248-213">Je pravděpodobné, v případě, že služby budou mít nesprávný stav při zpracování následných žádostí.</span><span class="sxs-lookup"><span data-stu-id="bf248-213">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="bf248-214">Služby, které mají závislosti na jejich měli zaregistrovat v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf248-214">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="bf248-215">Pokud služby konstruktor vyžaduje primitivní, například `string`, to může vložit pomocí [konfigurace](xref:fundamentals/configuration/index) a [možnosti vzor](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="bf248-215">If a service's constructor requires a primitive, such as a `string`, this can be injected by using [configuration](xref:fundamentals/configuration/index) and the [options pattern](xref:fundamentals/configuration/options).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="bf248-216">Služba životnosti a možnosti registrace</span><span class="sxs-lookup"><span data-stu-id="bf248-216">Service lifetimes and registration options</span></span>

<span data-ttu-id="bf248-217">ASP.NET – služby můžete nakonfigurovat následující životnosti:</span><span class="sxs-lookup"><span data-stu-id="bf248-217">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="bf248-218">**Dočasná**</span><span class="sxs-lookup"><span data-stu-id="bf248-218">**Transient**</span></span>

<span data-ttu-id="bf248-219">Přechodný životního cyklu služeb vytvářejí pokaždé, když, kterou jste požadovali.</span><span class="sxs-lookup"><span data-stu-id="bf248-219">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="bf248-220">Tato doba platnosti je nejvhodnější pro prosté, bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="bf248-220">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="bf248-221">**Obor**</span><span class="sxs-lookup"><span data-stu-id="bf248-221">**Scoped**</span></span>

<span data-ttu-id="bf248-222">Vymezená životního cyklu služeb se vytvoří jednou na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="bf248-222">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="bf248-223">Pokud používáte vymezené služby v middleware, Vložit službu do `Invoke` nebo `InvokeAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="bf248-223">If you're using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="bf248-224">Prostřednictvím vkládání konstruktor není vložit, protože vynutí služba se bude chovat, jako je typu singleton.</span><span class="sxs-lookup"><span data-stu-id="bf248-224">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span>

<span data-ttu-id="bf248-225">**singleton**</span><span class="sxs-lookup"><span data-stu-id="bf248-225">**Singleton**</span></span>

<span data-ttu-id="bf248-226">Singleton životního cyklu služeb se vytvoří při prvním jste požadovali (nebo když `ConfigureServices` se spustí, pokud existuje instance zadáte) a potom budou všechny následné žádosti o použít stejnou instanci.</span><span class="sxs-lookup"><span data-stu-id="bf248-226">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="bf248-227">Pokud vaše aplikace vyžaduje chování typu singleton, povolení kontejneru služby pro správu životního cyklu služby se doporučuje namísto singleton vzor návrhu implementace a správa životního cyklu vaší objekt ve třídě sami.</span><span class="sxs-lookup"><span data-stu-id="bf248-227">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="bf248-228">Služby lze dokument zaregistrovat u kontejneru několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="bf248-228">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="bf248-229">Jsme už viděli, jak zaregistrovat implementace služby daného typu zadáním konkrétní typ, který má použít.</span><span class="sxs-lookup"><span data-stu-id="bf248-229">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="bf248-230">Kromě toho objekt lze zadat, který bude použit k vytvoření instance na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="bf248-230">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="bf248-231">Třetí přístup je přímo zadat instanci typu, který chcete použít, ve kterém případ kontejneru se nebude pokoušet vytvořit instanci (ani se dispose instance).</span><span class="sxs-lookup"><span data-stu-id="bf248-231">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="bf248-232">K předvedení rozdíl mezi těmito možnostmi životnost a registrace, zvažte jednoduché rozhraní, které představuje jeden nebo více úloh, jako *operace* s jedinečným identifikátorem `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="bf248-232">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="bf248-233">V závislosti na tom, jak jsme konfigurovat doba platnosti pro tuto službu poskytne kontejner stejný nebo jiný instancí služby k vyžádání třídě.</span><span class="sxs-lookup"><span data-stu-id="bf248-233">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="bf248-234">Chcete-li vymazat, který platnosti je požadováno, vytvoříme jeden typ za možnost pro celou životnost:</span><span class="sxs-lookup"><span data-stu-id="bf248-234">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="bf248-235">Implementaci těchto rozhraní použití jedné třídy, `Operation`, která přijímá `Guid` v jeho konstruktoru nebo používá nové `Guid` Pokud žádný je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bf248-235">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="bf248-236">Vedle `ConfigureServices`, každý typ je přidat do kontejneru podle své životnosti s názvem:</span><span class="sxs-lookup"><span data-stu-id="bf248-236">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="bf248-237">Všimněte si, že `IOperationSingletonInstance` služba používá konkrétní instanci s známé ID `Guid.Empty` tak bude zrušte při tento typ se používá (její identifikátor Guid bude samých nul).</span><span class="sxs-lookup"><span data-stu-id="bf248-237">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="bf248-238">Také zaregistrovali `OperationService` to závisí na všech dalších `Operation` typy, takže bude zrušte v rámci žádost o tom, jestli je tato služba získávání stejnou instanci jako správce nebo novou pro každý typ operace.</span><span class="sxs-lookup"><span data-stu-id="bf248-238">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="bf248-239">Tato služba nemá je vystavit jeho závislé součásti jako vlastnosti, takže je můžete zobrazit v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bf248-239">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="bf248-240">K předvedení životnosti objektů v rámci a mezi samostatnou jednotlivé požadavky na aplikaci, zahrnuje vzorek `OperationsController` , každý požadavky druh `IOperation` typu a také `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="bf248-240">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="bf248-241">`Index` Akce poté zobrazí všechny řadiče a služby `OperationId` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bf248-241">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="bf248-242">Teď dvě samostatné žádosti o provedení této akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="bf248-242">Now two separate requests are made to this controller action:</span></span>

![Operace zobrazení závislostí vkládání ukázkovou webovou aplikaci spuštěné v Microsoft Edge zobrazující hodnoty operaci ID (GUID) pro přechodná, Scoped, typu Singleton a Instance Kontroleru a operace operací služby na první požadavek.](dependency-injection/_static/lifetimes_request1.png)

![Operace zobrazit zobrazuje hodnoty ID operace pro další požadavek.](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="bf248-245">Sledovat, které `OperationId` hodnoty se liší v rámci požadavku a mezi požadavky.</span><span class="sxs-lookup"><span data-stu-id="bf248-245">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="bf248-246">*Přechodný* objekty jsou vždy různých; novou instanci je zadán pro každý řadič a každou službu.</span><span class="sxs-lookup"><span data-stu-id="bf248-246">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="bf248-247">*Obor* objekty jsou stejné v rámci požadavku, ale jiné napříč různé požadavky</span><span class="sxs-lookup"><span data-stu-id="bf248-247">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="bf248-248">*Singleton* objekty jsou stejné pro všechny objekty a všechny žádosti o (bez ohledu na to, jestli je součástí instance `ConfigureServices`)</span><span class="sxs-lookup"><span data-stu-id="bf248-248">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="resolve-a-scoped-service-within-the-application-scope"></a><span data-ttu-id="bf248-249">Překlad vymezené služby v rámci oboru aplikace</span><span class="sxs-lookup"><span data-stu-id="bf248-249">Resolve a scoped service within the application scope</span></span>

<span data-ttu-id="bf248-250">Vytvoření [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) s [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) překlad vymezené služby v rámci oboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf248-250">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="bf248-251">Tento přístup je užitečný pro přístup k oboru služby při spuštění ke spuštění úlohy inicializace.</span><span class="sxs-lookup"><span data-stu-id="bf248-251">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="bf248-252">Následující příklad ukazuje, jak získat kontext pro `MyScopedService` v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="bf248-252">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

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

## <a name="scope-validation"></a><span data-ttu-id="bf248-253">Ověření oboru</span><span class="sxs-lookup"><span data-stu-id="bf248-253">Scope validation</span></span>

<span data-ttu-id="bf248-254">Když aplikace běží ve vývojovém prostředí na technologii ASP.NET Core 2.0 nebo novější, výchozím zprostředkovatelem služeb provádí kontroly ověřit, jestli:</span><span class="sxs-lookup"><span data-stu-id="bf248-254">When the app is running in the Development environment on ASP.NET Core 2.0 or later, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="bf248-255">Vymezené služby nejsou přímo nebo nepřímo přeložit od kořenové poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="bf248-255">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="bf248-256">Vymezená služby nejsou přímo nebo nepřímo vložit do jednotlivých prvků.</span><span class="sxs-lookup"><span data-stu-id="bf248-256">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="bf248-257">Kořenového poskytovatele služby se vytvoří při [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána.</span><span class="sxs-lookup"><span data-stu-id="bf248-257">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="bf248-258">Doba platnosti poskytovatele služeb kořenové odpovídá na aplikační nebo server životního cyklu, pokud zprostředkovatel začíná aplikace a uvolnění při vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf248-258">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="bf248-259">Vymezená služby jsou zapomenuty kontejnerem, který je vytvořil.</span><span class="sxs-lookup"><span data-stu-id="bf248-259">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="bf248-260">Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na singleton, protože jejich pouze likvidace podle Kořenový kontejner při ukončení aplikace nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="bf248-260">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="bf248-261">Ověřování služby Obory zachytí těchto situacích při `BuildServiceProvider` je volána.</span><span class="sxs-lookup"><span data-stu-id="bf248-261">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="bf248-262">Další informace najdete v tématu [obor ověření v tomto tématu hostitelský](xref:fundamentals/hosting#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="bf248-262">For more information, see [Scope validation in the Hosting topic](xref:fundamentals/hosting#scope-validation).</span></span>

## <a name="request-services"></a><span data-ttu-id="bf248-263">Žádost o služby</span><span class="sxs-lookup"><span data-stu-id="bf248-263">Request Services</span></span>

<span data-ttu-id="bf248-264">Žádosti o služby k dispozici v rámci technologie ASP.NET z `HttpContext` se zveřejňují přes `RequestServices` kolekce.</span><span class="sxs-lookup"><span data-stu-id="bf248-264">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![Položka HttpContext žádosti o služby Intellisense kontextové dialogové okno s oznámením, že žádost o služby, získá nebo nastaví IServiceProvider, který poskytuje přístup ke kontejneru služby žádosti.](dependency-injection/_static/request-services.png)

<span data-ttu-id="bf248-266">Žádost o služby představují služby nakonfigurovat a požadavků v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf248-266">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="bf248-267">Pokud vašich objektů určení závislostí, tyto jsou splněna typy najít v `RequestServices`, nikoli `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="bf248-267">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="bf248-268">Obecně byste neměli používat tyto vlastnosti přímo, upřednostňují místo toho k vyžádání typy tříd, které vyžadujete prostřednictvím konstruktoru třídy a, takže rozhraní vložit tyto závislosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-268">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="bf248-269">Dostaneme třídy, které se snadněji testování (najdete v části [Test a ladění](../testing/index.md)) a jsou více volně vázány.</span><span class="sxs-lookup"><span data-stu-id="bf248-269">This yields classes that are easier to test (see [Test and debug](../testing/index.md)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="bf248-270">Dáváte přednost požaduje závislosti jako parametry konstruktor přístup `RequestServices` kolekce.</span><span class="sxs-lookup"><span data-stu-id="bf248-270">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-services-for-dependency-injection"></a><span data-ttu-id="bf248-271">Navrhování služeb pro vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="bf248-271">Designing services for dependency injection</span></span>

<span data-ttu-id="bf248-272">Měli byste navrhnout vaše služby, aby získat jejich spolupracovníci pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="bf248-272">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="bf248-273">To znamená, že zabraňující použití stavová statickou metodu volání (důsledkem toho kód pach, označuje jako [statické plevami](http://deviq.com/static-cling/)) a přímé instance závislých tříd v rámci vašich služeb.</span><span class="sxs-lookup"><span data-stu-id="bf248-273">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="bf248-274">Mohou pomoci pamatovat fráze, [nové je pojidlem](https://ardalis.com/new-is-glue), při výběru, zda se má vytvořit instanci typu nebo o pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="bf248-274">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="bf248-275">Pomocí následujících [plnou zásady z objektu zaměřené na konkrétní návrh](http://deviq.com/solid/), tříd se přirozeně jsou obvykle malé, dobře započítané a snadno otestované.</span><span class="sxs-lookup"><span data-stu-id="bf248-275">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="bf248-276">Co když zjistíte, že vaše třídy mívají k způsob příliš velký počet závislostí se vložit?</span><span class="sxs-lookup"><span data-stu-id="bf248-276">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="bf248-277">Toto je obecně znaménkem, že se pokouší o provedení příliš mnoho třídě a pravděpodobně porušuje SRP - [jeden zásady odpovědnosti](http://deviq.com/single-responsibility-principle/).</span><span class="sxs-lookup"><span data-stu-id="bf248-277">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="bf248-278">V tématu, pokud lze refaktorovat třída některé jeho odpovědnosti přesunutím do nové třídy.</span><span class="sxs-lookup"><span data-stu-id="bf248-278">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="bf248-279">Mějte na paměti, že vaše `Controller` třídy musí zaměřit na aspekty uživatelského rozhraní, obchodní pravidla a data přístup implementace podrobnosti by měly být udržovány v příslušné tyto třídy [oddělit obavy](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="bf248-279">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="bf248-280">S ohledem na přístup k datům konkrétně je můžete vložit `DbContext` do řadičů (za předpokladu, že jste přidali EF ke kontejneru služby v `ConfigureServices`).</span><span class="sxs-lookup"><span data-stu-id="bf248-280">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="bf248-281">Někteří vývojáři dávají přednost používání rozhraní úložiště do databáze místo vložení `DbContext` přímo.</span><span class="sxs-lookup"><span data-stu-id="bf248-281">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="bf248-282">Pomocí rozhraní pro zapouzdření data logiku přístup na jednom místě minimalizovat počet míst, budete muset změnit při změně databáze.</span><span class="sxs-lookup"><span data-stu-id="bf248-282">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="bf248-283">Uvolnění služeb</span><span class="sxs-lookup"><span data-stu-id="bf248-283">Disposing of services</span></span>

<span data-ttu-id="bf248-284">Kontejner zavolá `Dispose` pro `IDisposable` typy vytvoří.</span><span class="sxs-lookup"><span data-stu-id="bf248-284">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="bf248-285">Ale pokud přidáte instanci ke kontejneru sami, se nebude uvolněno.</span><span class="sxs-lookup"><span data-stu-id="bf248-285">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="bf248-286">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf248-286">Example:</span></span>

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> <span data-ttu-id="bf248-287">Ve verzi 1.0, kontejner s názvem uvolnění na *všechny* `IDisposable` objekty, včetně těch, které ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="bf248-287">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="bf248-288">Nahrazení výchozího kontejneru služby</span><span class="sxs-lookup"><span data-stu-id="bf248-288">Replacing the default services container</span></span>

<span data-ttu-id="bf248-289">Kontejner vestavěné služby slouží k obsluhovat požadavky na základní rozhraní a většina příjemce aplikace založené na něm.</span><span class="sxs-lookup"><span data-stu-id="bf248-289">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="bf248-290">Vývojáři však můžete nahradit kontejneru předdefinované jejich upřednostňovaný kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf248-290">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="bf248-291">`ConfigureServices` Obvykle vrátí metoda `void`, ale pokud dojde ke změně jeho podpis vrátit `IServiceProvider`, jiný kontejner můžete nakonfigurovat a vrácena.</span><span class="sxs-lookup"><span data-stu-id="bf248-291">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="bf248-292">Mnoho kontejnerů IOC nejsou k dispozici pro .NET.</span><span class="sxs-lookup"><span data-stu-id="bf248-292">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="bf248-293">V tomto příkladu [Autofac](https://autofac.org/) se používá balíček.</span><span class="sxs-lookup"><span data-stu-id="bf248-293">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="bf248-294">Nejdřív nainstalujte balíčky odpovídajícího kontejneru:</span><span class="sxs-lookup"><span data-stu-id="bf248-294">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="bf248-295">V dalším kroku nakonfigurujte kontejneru v `ConfigureServices` a vrátit `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="bf248-295">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

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

> [!NOTE]
> <span data-ttu-id="bf248-296">Pokud používáte kontejner DI třetích stran, musíte změnit `ConfigureServices` tak, aby vracel `IServiceProvider` místo `void`.</span><span class="sxs-lookup"><span data-stu-id="bf248-296">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="bf248-297">Nakonec, nakonfigurujte Autofac jako normální v `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="bf248-297">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="bf248-298">Za běhu se použije Autofac vyřešit typy a vložit závislosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-298">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="bf248-299">[Další informace o používání Autofac a ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="bf248-299">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="bf248-300">Bezpečnost vlákna</span><span class="sxs-lookup"><span data-stu-id="bf248-300">Thread safety</span></span>

<span data-ttu-id="bf248-301">Jednotlivý prvek služby musí být bezpečné pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="bf248-301">Singleton services need to be thread safe.</span></span> <span data-ttu-id="bf248-302">Pokud služba singleton má závislost na přechodný službu, službu přechodný také potřebovat podprocesů, podle toho, jak se používají v singleton.</span><span class="sxs-lookup"><span data-stu-id="bf248-302">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="bf248-303">Doporučení</span><span class="sxs-lookup"><span data-stu-id="bf248-303">Recommendations</span></span>

<span data-ttu-id="bf248-304">Při práci s vkládání závislostí, mějte následující doporučení:</span><span class="sxs-lookup"><span data-stu-id="bf248-304">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="bf248-305">DI je pro objekty, které mají závislosti na komplexní.</span><span class="sxs-lookup"><span data-stu-id="bf248-305">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="bf248-306">Všechny příklady objektů, které mohou být přidány do DI jsou řadiče, služby, adaptéry a úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf248-306">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="bf248-307">Vyhněte se přímo v DI ukládání dat a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bf248-307">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="bf248-308">Například nákupní košík uživatele obvykle nepřidávat kontejner služeb.</span><span class="sxs-lookup"><span data-stu-id="bf248-308">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="bf248-309">Konfigurace by měl používat [možnosti vzor](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="bf248-309">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="bf248-310">Vyhněte se podobně "data držitelem" objekty, které existují pouze pro povolení přístupu k některé z jiného objektu.</span><span class="sxs-lookup"><span data-stu-id="bf248-310">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="bf248-311">Je lepší požádat o položce skutečné potřeby prostřednictvím DI, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="bf248-311">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="bf248-312">Vyhněte se statickou přístup ke službám.</span><span class="sxs-lookup"><span data-stu-id="bf248-312">Avoid static access to services.</span></span>

* <span data-ttu-id="bf248-313">Vyhněte se umístění služby v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf248-313">Avoid service location in your application code.</span></span>

* <span data-ttu-id="bf248-314">Vyhněte se statickou přístup k `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="bf248-314">Avoid static access to `HttpContext`.</span></span>

> [!NOTE]
> <span data-ttu-id="bf248-315">Jako všech sad doporučení mohou nastat situace, kdy je potřeba jeden je ignorována.</span><span class="sxs-lookup"><span data-stu-id="bf248-315">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="bf248-316">Našli jsme výjimky třeba výjimečných – většinou velmi zvláštních případech v rámci samotného.</span><span class="sxs-lookup"><span data-stu-id="bf248-316">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="bf248-317">Pamatujte si, že je vkládání závislostí *alternativní* na static, globální objekt přístupové vzorce.</span><span class="sxs-lookup"><span data-stu-id="bf248-317">Remember, dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="bf248-318">Nebudete moci pochopit výhody DI Pokud kombinujete statické objektu přístup.</span><span class="sxs-lookup"><span data-stu-id="bf248-318">You won't be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf248-319">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bf248-319">Additional resources</span></span>

* [<span data-ttu-id="bf248-320">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="bf248-320">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="bf248-321">Testování a ladění</span><span class="sxs-lookup"><span data-stu-id="bf248-321">Test and debug</span></span>](xref:testing/index)
* [<span data-ttu-id="bf248-322">Aktivace na základě Factory middlewaru</span><span class="sxs-lookup"><span data-stu-id="bf248-322">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="bf248-323">Psaní kódu vyčištění v ASP.NET Core pomocí vkládání závislostí (MSDN)</span><span class="sxs-lookup"><span data-stu-id="bf248-323">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="bf248-324">Návrh aplikace spravované kontejneru, Prelude: Kde podporuje, patří kontejneru?</span><span class="sxs-lookup"><span data-stu-id="bf248-324">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="bf248-325">Princip explicitní závislosti.</span><span class="sxs-lookup"><span data-stu-id="bf248-325">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)
* <span data-ttu-id="bf248-326">[Inverze – kontejnery ovládacích prvků a vzor vkládání závislostí](https://www.martinfowler.com/articles/injection.html) (Fowler)</span><span class="sxs-lookup"><span data-stu-id="bf248-326">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
