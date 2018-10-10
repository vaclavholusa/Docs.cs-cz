---
title: Injektáž závislostí v ASP.NET Core
author: guardrex
description: Zjistěte, jak ASP.NET Core implementuje injektáž závislostí a způsobu jeho použití.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 33fae5d87029c8b3afdc321e0247555c1e479d07
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912615"
---
# <a name="dependency-injection-in-aspnet-core"></a>Injektáž závislostí v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), a [Luke Latham](https://github.com/guardrex)

ASP.NET Core podporuje závislost vkládání (DI) software vzor návrhu, což je technika, pro dosažení [řízení IOC (Inversion)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) mezi třídami a jejich závislosti.

Další informace specifické pro vkládání závislostí do kontrolerů MVC najdete v tématu <xref:mvc/controllers/dependency-injection>.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Přehled injektáž závislostí

A *závislost* je libovolný objekt, který vyžaduje jiný objekt. Zkontrolujte následující `MyDependency` třídy s `WriteMessage` metodu, která jiných tříd v aplikaci závisí na:

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

Instance `MyDependency` třídy lze provádět `WriteMessage` metody, které jsou k dispozici na třídu. `MyDependency` Třídy je závislost `IndexModel` třídy:

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

Instance `MyDependency` třídy lze provádět `WriteMessage` metody, které jsou k dispozici na třídu. `MyDependency` Třídy je závislost `HomeController` třídy:

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

Vytvoří třídu a přímo závisí `MyDependency` instance. Závislosti kódu (například v předchozím příkladu) jsou problematické a mělo by se vyhnout z následujících důvodů:

* Chcete-li nahradit `MyDependency` s jinou implementaci třídy musí být změněny.
* Pokud `MyDependency` má závislosti, musíte je nakonfigurovat pomocí třídy. Ve velkých projektech s více třídami v závislosti na `MyDependency`, kód konfigurace stane rozmístěny na aplikaci.
* Tato implementace je obtížné testování částí. Aplikace by měla používat model nebo se zakázaným inzerováním `MyDependency` třídu, která není možné s tímto přístupem.

Injektáž závislostí řeší tyto problémy prostřednictvím:

* Použití rozhraní abstraktní implementace závislostí.
* Registrace závislosti v kontejneru služby. ASP.NET Core nabízí integrovanou službu kontejneru, [IServiceProvider](/dotnet/api/system.iserviceprovider). Služby jsou registrované aplikace `Startup.ConfigureServices` metody.
* *Vkládání* služby do konstruktoru třídy, ve kterém se používá. Rozhraní framework přebírá odpovědnost vytvoření instance závislosti a jejich zničení, pokud už je nepotřebujete.

V [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` rozhraní definuje metody, která poskytuje služby do aplikace:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

Toto rozhraní je implementováno podle konkrétního typu implementujícího typ `MyDependency`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

`MyDependency` Požadavky [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) 've svém konstruktoru. Není zřetězené způsobem pomocí vkládání závislostí. Každá požadovaná závislost zase požaduje svoje vlastní závislosti. Kontejner řeší závislosti v grafu a vrátí službu zcela přeložit. Souhrnný sadu závislosti, které je třeba vyřešit se obvykle označuje jako *strom závislostí*, *graf závislosti*, nebo *graf objektu*.

`IMyDependency` a `ILogger<TCategoryName>` musí být zaregistrovaný v kontejneru služby. `IMyDependency` je zaregistrovaný v `Startup.ConfigureServices`. `ILogger<TCategoryName>` registraci protokolování abstrakce infrastrukturu, takže má [služby poskytované rozhraním](#framework-provided-services) registrován ve výchozím nastavení v rámci rozhraní.

V ukázkové aplikaci `IMyDependency` služba není registrována s konkrétní typ `MyDependency`. Registrace obory doba platnosti služby k době života jeden požadavek. [Služba životnosti](#service-lifetimes) jsou popsány dále v tomto tématu.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> Každý `services.Add{SERVICE_NAME}` – metoda rozšíření přidá (a potenciálně nakonfiguruje) služby. Například `services.AddMvc()` přidá služby Razor Pages a vyžadují MVC. Doporučujeme, aby aplikace postupujte podle Tato konvence. Rozšiřující metody v umístění [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) obor názvů pro zapouzdření skupiny registrací služby.

Pokud konstruktor služby vyžaduje jednoduchého typu, například `string`, primitivní vlastnost může být vloženy pomocí [konfigurace](xref:fundamentals/configuration/index) nebo [možnosti vzor](xref:fundamentals/configuration/options):

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

Instance služby je požadováno prostřednictvím konstruktoru třídy, kde se služba používá a přiřadí do privátní pole. Pole se používá pro přístup ke službě podle potřeby v rámci třídy.

V ukázkové aplikaci `IMyDependency` instance je požadováno a použít k volání služby `WriteMessage` metody:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a>Služby poskytované rozhraním

`Startup.ConfigureServices` Metoda odpovídá za definování služeb aplikace využívá, včetně funkcí platformy, jako je například Entity Framework Core a ASP.NET Core MVC. Na začátku `IServiceCollection` poskytnuté `ConfigureServices` obsahuje následující definice služby (v závislosti na [konfiguraci hostitele](xref:fundamentals/host/index)):

| Typ služby | Doba platnosti |
| ------------ | -------- |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Přechodná |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Přechodná |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Přechodná |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Přechodná |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |

Metody rozšíření kolekce služby je možné zaregistrovat službu (a jeho závislé služby, pokud je to nutné), tato konvence při použití jediného `Add{SERVICE_NAME}` metodu rozšíření k registraci všech služeb vyžadují danou službu. Následující kód je příklad toho, jak přidat další služby do kontejneru pomocí metody rozšíření [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):

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

Další informace najdete v tématu [ServiceCollection třídy](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) v dokumentaci k rozhraní API.

## <a name="service-lifetimes"></a>Životnost služby

Zvolte odpovídající životnost pro každé registrovanou službu. ASP.NET Core services můžete nakonfigurovat následující životní cyklus:

**Přechodná**

Přechodná doba platnosti služby se vytvoří pokaždé, když jste žádali. Tato doba platnosti je nejvhodnější pro zjednodušené, bezstavových služeb.

**Obor**

S vymezeným oborem životnost služby se vytvoří jednou každý požadavek.

> [!WARNING]
> Při použití služby s vymezeným oborem v middleware, vloží službu do `Invoke` nebo `InvokeAsync` metody. Prostřednictvím konstruktoru vkládání není vložit, protože nutí službě a chovají se jako singleton. Další informace naleznete v tématu <xref:fundamentals/middleware/index>.

**singleton**

Deklarace služeb typu singleton životnost se vytvoří při prvním jste žádali (nebo když `ConfigureServices` spuštění a instance je zadán s registrací služby). Každý další požadavek používá stejnou instanci. Pokud aplikace vyžaduje chování typu singleton, se doporučuje povolení kontejneru služby ke správě životnosti služby. Nemusíte implementovat vzor návrhu typu singleton a poskytnout uživatelský kód ke správě životnosti objektu ve třídě.

> [!WARNING]
> Je nebezpečné vyřešit vymezené služby z typu singleton. Může to způsobit služby má nesprávný stav při zpracování následných žádostí.

### <a name="constructor-injection-behavior"></a>Konstruktor chování vkládání

Služby se dají vyřešit dva mechanismy:

* `IServiceProvider`
* [ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; umožňuje vytvoření objektu bez registrace služby v kontejneru pro vkládání závislostí. `ActivatorUtilities` se používá s přístupných abstrakce, jako je například pomocných rutin značek, kontrolery MVC a vazače modelů.

Konstruktory mohou přijímat argumenty, které nejsou součástí injektáž závislostí, ale argumenty musí přiřadit výchozí hodnoty.

Když jsou vyřešeny služby `IServiceProvider` nebo `ActivatorUtilities`, vyžaduje konstruktor vkládání *veřejné* konstruktoru.

Když jsou vyřešeny služby `ActivatorUtilities`, konstruktor vkládání vyžaduje tento pouze jeden použít konstruktor existuje. Přetížení konstruktoru jsou podporované, ale může existovat pouze jedním přetížením, jehož argumenty lze všechny splnit vkládání závislostí.

## <a name="entity-framework-contexts"></a>Kontext Entity Framework

Entity Framework kontexty má přidat do kontejneru služby pomocí vymezených životnost. Tento problém řeší automaticky volání [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) metoda při registraci kontext databáze. Služby, které používají kontext databáze by měl také použít s vymezeným oborem životnost.

## <a name="lifetime-and-registration-options"></a>Doba života a možností registrace

Chcete-li prokázali rozdíl mezi možnostmi životnost a registraci, zvažte následující rozhraní, které představují úlohy jako operaci s jedinečným identifikátorem `OperationId`. V závislosti na konfiguraci životnosti služby operace pro následující rozhraní kontejneru poskytuje stejné nebo jiné instanci služby na požádání třídou:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

Rozhraní jsou implementovány v `Operation` třídy. `Operation` Konstruktor vytvoří identifikátor GUID, pokud jeden není zadán:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

`OperationService` Zaregistrován to záleží na každé z nich `Operation` typy. Když `OperationService` žádá pomocí vkládání závislostí, obdrží buď novou instanci třídy každé služby nebo stávající instance podle doby života ze závislých služeb.

* Pokud přechodné služby se vytvoří při požadavku `OperationsId` z `IOperationTransient` služby se liší od `OperationsId` z `OperationService`. `OperationService` obdrží novou instanci třídy `IOperationTransient` třídy. Vrací novou instanci jinou `OperationsId`.
* Pokud vymezené služby se vytvoří každý požadavek, `OperationsId` z `IOperationScoped` služba je stejné jako u `OperationService` v rámci požadavku. Obě služby napříč požadavky, sdílet jiné `OperationsId` hodnotu.
* Pokud jsou služby typu singleton a instanci typu singleton vytvořit jednou a použít v rámci všech požadavků a všemi službami, `OperationsId` je konstantní napříč všemi požadavky služby.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

V `Startup.ConfigureServices`, každý typ je přidat do kontejneru podle životnosti s názvem:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

`IOperationSingletonInstance` Služba používá konkrétní instanci s známé ID `Guid.Empty`. Je jasné, když tento typ se používá (její identifikátor GUID se všemi nulovými hodnotami).

::: moniker range=">= aspnetcore-2.1"

Ukázková aplikace předvádí správu životnosti objektů v rámci a mezi jednotlivé požadavky. Ukázková aplikace `IndexModel` požadavků každý druh `IOperation` typ a `OperationService`. Stránce se pak zobrazí všechny třídy modelu stránky a služby `OperationId` hodnoty prostřednictvím vlastnosti přiřazení:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Ukázková aplikace předvádí správu životnosti objektů v rámci a mezi jednotlivé požadavky. Obsahuje ukázkovou aplikaci `OperationsController` , že každý žádosti druh `IOperation` typ a `OperationService`. `Index` Akce nastaví služby do `ViewBag` pro zobrazení služby `OperationId` hodnoty:

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

Dva následující výstup ukazuje výsledky dvou požadavků:

**První požadavek:**

Operace kontroleru:

Přechodné: d233e165-f417-469b-a866-1cf1935d2518  
Obor: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

`OperationService` operace:

Přechodné: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Obor: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

**Druhou žádost:**

Operace kontroleru:

Přechodné: b63bd538-0a37-4ff1-90ba-081c5138dda0  
Obor: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

`OperationService` operace:

Přechodné: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Obor: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Jednotlivý prvek: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Podívejte se, které `OperationId` hodnoty se liší v rámci požadavku a mezi požadavky:

* *Přechodné* objekty jsou vždy odlišné. Všimněte si, že přechodná `OperationId` hodnota prvního a druhého požadavky se liší pro obě `OperationService` operací a napříč požadavky. Novou instanci se poskytuje pro každou službu a požadavek.
* *Obor* objekty jsou stejné v rámci požadavku, ale jiné napříč požadavky.
* *Jednotlivý prvek* objekty jsou stejné pro všechny objekty a všechny požadavky bez ohledu na to, jestli se `Operation` instance je k dispozici v `ConfigureServices`.

## <a name="call-services-from-main"></a>Volání služby z hlavní

Vytvoření [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) s [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) vyřešit vymezené služby v rámci oboru aplikace. Tento přístup je užitečný pro přístup k vymezené služby při spuštění počítače a spouštět úlohy inicializace. Následující příklad ukazuje, jak získat kontext pro `MyScopedService` v `Program.Main`:

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

## <a name="scope-validation"></a>Rozsah ověřování

::: moniker range=">= aspnetcore-2.0"

Když aplikace běží ve vývojovém prostředí, poskytovatele služeb výchozí provádí kontroly pro ověření, že:

* Vymezené služby nejsou přímo nebo nepřímo vyřešit z kořenové poskytovatele služeb.
* Vymezené služby nejsou přímo nebo nepřímo vloženy do jednotlivých prvků.

::: moniker-end

Poskytovatel služeb root je vytvořen, když [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána. Doba života poskytovatele služeb kořenový odpovídá životnost aplikace/serveru při zprostředkovatel začíná aplikace a je uvolněna při ukončení aplikace.

Vymezené služby jsou uvolněna pomocí kontejneru, který je vytvořil. Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na jednotlivý prvek, protože to je jenom uvolněn pomocí Kořenový kontejner při vypnutí serveru/aplikace. Ověřování služby Obory zachytí tyto situace při `BuildServiceProvider` je volána.

Další informace naleznete v tématu <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Žádost o služby

Žádost o služby k dispozici v rámci ASP.NET Core z `HttpContext` jsou vystaveny prostřednictvím [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) kolekce.

Žádost o služby představují služby nakonfigurovaný a požadovány v rámci aplikace. Pokud objekty určení závislostí, jsou tyto splněno typy nalezené v `RequestServices`, nikoli `ApplicationServices`.

Aplikace obvykle by neměly používat tyto vlastnosti přímo. Místo toho požadavku typy, třídy vyžadovat prostřednictvím konstruktor třídy a povolit rozhraní framework vložit závislosti. To poskytuje třídy, které usnadňuje testování (najdete v článku [testování a ladění](xref:test/index) témata).

> [!NOTE]
> Raději jako parametry konstruktoru pro přístup k žádosti o závislosti `RequestServices` kolekce.

## <a name="design-services-for-dependency-injection"></a>Služby návrhu pro vkládání závislostí

Osvědčené postupy jsou následující:

* Navrhujte služby pomocí vkládání závislostí získat jejich závislosti.
* Vyhněte se volání metody stavová a statické (postup známý jako [statické plevami](https://deviq.com/static-cling/)).
* Vyhněte se přímé vytváření instancí závislých tříd v rámci služeb. Přímé vytvoření instance páry v odstupu kód pro konkrétní implementaci.

Pomocí následujících [SOLID zásady z objektu orientovaný návrh](https://deviq.com/solid/), třídy aplikace jsou často přirozeně na malé, skvěle a snadno otestované.

Pokud třída zdá se, že máte příliš mnoho vložených závislostí, je obecně znak, třída má příliš mnoho zodpovědnosti a porušuje [jedné zásadě odpovědnost (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Pokus o Refaktorovat třídy některé z jeho zodpovědnosti přesunutím do nové třídy. Mějte na paměti, která tříd modelu stránky Razor Pages a třídy kontroleru MVC byste se zaměřit na aspekty uživatelského rozhraní. Obchodní pravidla a data přístup implementace podrobnosti by měly být neustále ve třídách, které jsou vhodné pro tyto [oddělení obavy](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Vyřazení služby

Kontejner volá `Dispose` pro `IDisposable` typy ji vytvoří. Pokud instance je přidat do kontejneru v uživatelském kódu, není automaticky odstraněn.

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
> V ASP.NET Core 1.0, kontejner volá metodu dispose v *všechny* `IDisposable` objektů, včetně těch, které se nepovedlo vytvořit.

::: moniker-end

## <a name="default-service-container-replacement"></a>Výchozí služba kontejneru nahrazení

Integrovaná služba kontejneru je určen pro sloužit potřebám rozhraní framework a většina uživatelů aplikací. Doporučujeme používat integrované kontejneru, pokud potřebujete konkrétní funkce, která nepodporuje. Některé z funkcí podporovaných v 3. stran kontejnery nebyl nalezen v předdefinované kontejneru:

* Vkládání vlastnosti
* Vkládání podle názvu
* Podřízené kontejnery
* Management vlastní doba života
* `Func<T>` Podpora pro opožděná inicializace

Najdete v článku [injektáž závislostí souboru readme.md](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) seznam některých kontejnerů, které podporují adaptéry.

Následující příklad nahradí kontejneru integrované s [Autofac](https://autofac.org/):

* Instalovat balíčky odpovídajícího kontejneru:

    * [Autofac](https://www.nuget.org/packages/Autofac/)
    * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Konfigurace kontejneru v `Startup.ConfigureServices` a vraťte se `IServiceProvider`:

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

    Použití kontejneru 3. stran `Startup.ConfigureServices` musí vracet `IServiceProvider`.

* Konfigurace Autofac v `DefaultModule`:

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

Za běhu Autofac lze přeložit typy a vložení závislosti. Další informace o používání Autofac pomocí ASP.NET Core, najdete v článku [Autofac dokumentaci](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Bezpečnost vlákna

Deklarace služeb typu singleton musí být bezpečné pro vlákna. Pokud služby typu singleton obsahuje závislost na přechodné služby, přechodné služby také muset být bezpečné, v závislosti, jak se používají v typu singleton.

Metoda factory jedné služby, jako je například druhý argument [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), nebude musí být bezpečné pro vlákna. Typ, jako jsou (`static`) konstruktor, ji má zaručeno, že má být volána po jednom vlákně.

## <a name="recommendations"></a>Doporučení

Při práci s injektáž závislostí, mít na paměti tato doporučení:

* Vyhněte se ukládání dat a konfigurace přímo do kontejneru služby. Například by neměla uživatele nákupního košíku přidat obvykle do kontejneru služby. Konfigurace by měl používat [možnosti vzor](xref:fundamentals/configuration/options). Podobně nepoužívejte "vlastník dat" objekty, které existují pouze pokud chcete povolit přístup na některý objekt. Je lepší požádat o skutečné položky prostřednictvím DI.

* Vyhněte se statické přístup ke službám (příklad staticky – zadáním [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) pro použití jinde).

* Vyhněte se použití *služby lokátoru vzor*. Není třeba vyvolat <xref:System.IServiceProvider.GetService*> při DI místo toho můžete získat instanci služby. Další variantou Lokátor služby, aby se vkládá objekt factory, který řeší závislosti za běhu. Obě tyto postupy kombinace [ovládacího prvku inverzi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategie.

* Vyhněte se statické přístup k `HttpContext` (například [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).

Stejně jako všechny sadu doporučení mohou nastat situace, ve kterém jsou vyžadována doporučení se ignoruje. Výjimky se vyskytují jen vzácně&mdash;většinou zvláštní případy v rámci samotného rozhraní.

DI je *alternativní* na vzorech přístupu statická/globální objekt. Nebudete moci využít výhod DI, jsou-li zkombinovány s přístupem statický objekt.

## <a name="additional-resources"></a>Další zdroje

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [Zápis čistý kód v ASP.NET Core s injektáž závislostí (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Spravované kontejneru návrhu aplikace, Prelude: Kam patří kontejneru?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Princip explicitní závislosti.](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Inverze – kontejnery ovládacích prvků a vzor injektáž závislostí (Martina Fowlera)](https://www.martinfowler.com/articles/injection.html)
* [Je nový spojovací ("vzájemné připevnění" kódu pro konkrétní implementaci)](https://ardalis.com/new-is-glue)
* [Postup při registraci služby s více rozhraními v ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
