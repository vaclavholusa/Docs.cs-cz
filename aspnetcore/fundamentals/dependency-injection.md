---
title: "Vkládání závislostí v ASP.NET Core"
author: ardalis
description: "Zjistěte, jak ASP.NET Core implementuje vkládání závislostí a způsobu jeho použití."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 85e25b92b01d84279752deb7865987746c181c72
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Vkládání závislostí v ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)

ASP.NET Core slouží od základů se na podporu a využít vkládání závislostí. Aplikace ASP.NET Core můžete využít předdefinované framework služby tak, že je vloženy do metody ve třídě, spuštění a aplikačních služeb můžete nakonfigurovat pro vkládání také. Výchozí kontejner služeb poskytovaných ASP.NET Core poskytuje minimální funkce nastavit a není určena k nahrazení jiných kontejnerů.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Co je vkládání závislostí?

Vkládání závislostí (DI) je technika k dosažení volné párování mezi objekty a jejich spolupracovníci nebo závislosti. Ne přímo vytváření instancí spolupracovníci, nebo pomocí statické odkazy jsou uvedené objekty třídy musí za účelem provádění akcí k třídě nějakým způsobem. Nejčastěji se třídy deklarovat závislé prostřednictvím jejich konstruktoru, což jim umožní postupujte podle [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/). Tento postup se označuje jako "konstruktor vkládání".

Když třídy jsou navrženy s DI pamatovat, budou se více volně vázány protože nemají přímé, pevně zakódované závislosti na jejich spolupracovníci. To odpovídá [závislostí inverzi Princip](http://deviq.com/dependency-inversion-principle/), která uvádí, že *"by neměl vysokou úroveň moduly závisí na nízkou úroveň moduly; obě by měl záviset na abstrakce."* Místo odkazující na konkrétní implementace, třídy požadavku abstrakce (obvykle `interfaces`) které jsou poskytovány k nim-li třída je vytvořená. Extrahování závislosti do rozhraní a poskytování implementace tato rozhraní jako parametry je také příklad [vzoru návrhu strategie](http://deviq.com/strategy-design-pattern/).

Při systému je navržen pro použití DI, s mnoho tříd, které žádají o jejich závislosti prostřednictvím jejich – konstruktor (nebo vlastnosti), je vhodné mít třídu vyhrazený pro vytváření těchto tříd s jejich přidružené závislosti. Tyto třídy jsou označovány jako *kontejnery*, nebo přesněji řečeno, [inverzi řízení (IoC)](http://deviq.com/inversion-of-control/) kontejnery nebo kontejnerů vkládání závislostí (DI). Kontejner je v podstatě objekt factory, který zodpovídá za poskytování instancí typů, které jsou požadovány z něj. Pokud daný typ deklaruje, že má závislosti a kontejneru byl nakonfigurován pro zadejte typy závislostí, vytvoří závislosti v rámci vytváření požadovaný instance. Tímto způsobem lze zadat grafy složitých závislostí na třídy bez nutnosti jakékoli konstrukce pevně objektu. Kromě vytváření objektů pomocí jejich závislosti, Spravovat kontejnery obvykle životnosti objektů v aplikaci.

ASP.NET Core obsahuje jednoduché předdefinované kontejner (reprezentována `IServiceProvider` rozhraní), která podporuje vkládání konstruktor ve výchozím nastavení a ASP.NET zpřístupní určité služby prostřednictvím DI. SOUBOR ASP. Kontejner pro NET odkazuje na typy spravuje jako *služby*. Ve zbývající části tohoto článku *služby* bude odkazovat na typy, které jsou spravovány nástrojem kontejner IoC ASP.NET Core. Konfigurace služby předdefinované kontejneru v `ConfigureServices` metoda ve vaší aplikaci `Startup` třídy.

> [!NOTE]
> Martin Fowler zápisem rozsáhlé článek na [inverzi kontejnery ovládacích prvků a vzor vkládání závislostí](https://www.martinfowler.com/articles/injection.html). Microsoft Patterns and Practices má také skvělé popis [vkládání závislostí](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Tento článek se zabývá vkládání závislostí, která se vztahuje na všechny aplikace ASP.NET. Vkládání závislostí v rámci řadiče MVC je popsaná v [řadiče a vkládání závislostí](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Chování vkládání – konstruktor

Konstruktor vkládání vyžaduje, aby v konstruktoru *veřejné*. Jinak vyvolá výjimku aplikace `InvalidOperationException`:

> Nepodařilo se najít vhodný konstruktor pro typ 'YourType'. Zkontrolujte typ je konkrétní a služby jsou registrované pro všechny parametry veřejný konstruktor.


Konstruktor vkládání vyžaduje, že pouze jeden použít konstruktor neexistuje. Konstruktor přetížení jsou podporované, ale může existovat jenom jeden přetížení, jejichž argumenty lze všechny splnit vkládání závislostí. Pokud existuje více než jeden, vyvolá výjimku aplikace `InvalidOperationException`:

> Více konstruktory přijímá všechny typy daný argument byly zjištěny v typu 'YourType'. Měla by existovat pouze jedna použít konstruktor.

Konstruktory může přijmout argumenty, které nejsou součástí vkládání závislostí, ale tyto musí podporovat výchozí hodnoty. Příklad:

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

## <a name="using-framework-provided-services"></a>Pomocí služby poskytované framework

`ConfigureServices` Metoda v `Startup` třída je odpovědná za definici služby, aplikace bude používat, včetně funkcí platformy jako Entity Framework Core a ASP.NET Core MVC. Standardně `IServiceCollection` poskytované `ConfigureServices` má následující služby definované (v závislosti na [konfigurace hostitele](xref:fundamentals/hosting)):

| Typ služby | Doba platnosti |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Dočasná |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Dočasná |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Dočasná |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Dočasná |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | singleton |

Dole je příklad toho, jak přidat další služby ke kontejneru s počtem rozšiřující metody jako `AddDbContext`, `AddIdentity`, a `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Funkce a middleware technologii ASP.NET, jako je například MVC, postupujte podle konvence použití jedné přidat*ServiceName* metody rozšíření pro všechny služby, tato funkce vyžaduje registraci.

>[!TIP]
> Můžete požádat o určité zadaný framework služby v rámci `Startup` najdete v části metody prostřednictvím jejich seznamy parametrů - [spuštění aplikace](startup.md) další podrobnosti.

## <a name="registering-services"></a>Registrace služby

Takto můžete zaregistrovat vlastní aplikačních služeb. První obecný typ představuje typ (obvykle rozhraní), který bude vyžádána z kontejneru. Druhý obecný typ představuje konkrétní typ, který mohl vytvořit jeho instanci kontejneru, který se používá ke splnění těchto požadavků.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Každý `services.Add<ServiceName>` metoda rozšíření přidá (a potenciálně nakonfiguruje) služby. Například `services.AddMvc()` přidá služby MVC požaduje. Je doporučeno podle touto konvencí, umístění rozšiřujících metod v `Microsoft.Extensions.DependencyInjection` obor názvů pro zapouzdření skupiny registrací služby.

`AddTransient` Metoda se používá k mapování abstraktní typy na konkrétní služby, které jsou vytvářeny samostatně instance pro každý objekt, který vyžaduje. To se označuje jako služby *životnost*, a další životnost možnosti jsou popsané níže. Je důležité, abyste zvolili příslušné doba platnosti pro jednotlivé služby, které je zaregistrovat. Novou instanci třídy služby poskytovat pro různé třídy, která požaduje? Má být použita jedna instance v rámci dané webové žádosti? Nebo se jedna instance mají použít pro životního cyklu aplikace?

V ukázce pro tento článek je jednoduchý kontroler, který se zobrazí názvy znaků, názvem `CharactersController`. Jeho `Index` metoda zobrazí aktuální seznam znaků, které byly uloženy v aplikaci a inicializuje kolekci pár znaků, pokud žádný neexistuje. Všimněte si, že i když tato aplikace používá Entity Framework Core a `ApplicationDbContext` třídy pro jeho stálost, žádná které je zřejmá v řadiči. Místo toho má byla abstrahované mechanismus přístupu konkrétním data za rozhraní, `ICharacterRepository`, které způsobem [použitému vzoru](http://deviq.com/repository-pattern/). Instance `ICharacterRepository` je požadována prostřednictvím konstruktoru a přiřazená privátní pole, které se pak použije k přístup ke znakům podle potřeby.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` Definuje tyto dvě metody kontroleru musí pracovat s `Character` instance.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Toto rozhraní je implementováno zase podle konkrétního typu, `CharacterRepository`, který se používá za běhu.

> [!NOTE]
> Způsob DI se používá s `CharacterRepository` obecné modelu, můžete provést u všech aplikačních služeb, ne jenom při "úložiště" nebo data přístupové třídy je třída.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Všimněte si, že `CharacterRepository` požadavky `ApplicationDbContext` v jeho konstruktoru. Není pro vkládání závislostí, který se má použít zřetězené způsobem tímto způsobem, se každý požadovaný závislostí zase vyžaduje vlastní závislosti. Kontejner je zodpovědná za řešení všechny závislé součásti v tomto grafu a vrácení službu plně vyřešený.

> [!NOTE]
> Vytváření požadovaný objekt a všechny objekty vyžaduje a všechny objekty těch, které vyžadují, se někdy označuje jako *grafu objektu*. Podobně souhrnný sadu závislosti, které je třeba vyřešit se obvykle označuje jako *strom závislosti* nebo *graf závislostí*.

V takovém případě obě `ICharacterRepository` a naopak `ApplicationDbContext` musí být zaregistrované v kontejneru služby v `ConfigureServices` v `Startup`. `ApplicationDbContext` je nakonfigurován pomocí volání metody rozšíření `AddDbContext<T>`. Následující kód ukazuje registrace `CharacterRepository` typu.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Kontexty Entity Framework musí být přidaní do ke kontejneru služby pomocí `Scoped` životního cyklu. To se stará automaticky Pokud používáte metody helper, jak je uvedeno výše. Úložiště, které bude nutné používat rozhraní Entity Framework by měli používat stejnou dobu životnosti.

>[!WARNING]
> Je řešení hlavní nebezpečí k věnujte pozornost `Scoped` služby z typu singleton. Je pravděpodobné, v případě, že služby budou mít nesprávný stav při zpracování následných žádostí.

Služby, které mají závislosti na jejich měli zaregistrovat v kontejneru. Pokud služby konstruktor vyžaduje primitivní, například `string`, to může vložit pomocí [konfigurace](xref:fundamentals/configuration/index) a [možnosti vzor](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Služba životnosti a možnosti registrace

ASP.NET – služby můžete nakonfigurovat následující životnosti:

**Transient**

Přechodný životního cyklu služeb vytvářejí pokaždé, když, kterou jste požadovali. Tato doba platnosti je nejvhodnější pro prosté, bezstavové služby.

**Obor**

Vymezená životního cyklu služeb se vytvoří jednou na základě požadavku.

singleton

Singleton životního cyklu služeb se vytvoří při prvním jste požadovali (nebo když `ConfigureServices` se spustí, pokud existuje instance zadáte) a potom budou všechny následné žádosti o použít stejnou instanci. Pokud vaše aplikace vyžaduje chování typu singleton, povolení kontejneru služby pro správu životního cyklu služby se doporučuje namísto singleton vzor návrhu implementace a správa životního cyklu vaší objekt ve třídě sami.

Služby lze dokument zaregistrovat u kontejneru několika způsoby. Jsme už viděli, jak zaregistrovat implementace služby daného typu zadáním konkrétní typ, který má použít. Kromě toho objekt lze zadat, který bude použit k vytvoření instance na vyžádání. Třetí přístup je přímo zadat instanci typu, který chcete použít, ve kterém případ kontejneru se nebude pokoušet vytvořit instanci (ani se dispose instance).

K předvedení rozdíl mezi těmito možnostmi životnost a registrace, zvažte jednoduché rozhraní, které představuje jeden nebo více úloh, jako *operace* s jedinečným identifikátorem `OperationId`. V závislosti na tom, jak jsme konfigurovat doba platnosti pro tuto službu poskytne kontejner stejný nebo jiný instancí služby k vyžádání třídě. Chcete-li vymazat, který platnosti je požadováno, vytvoříme jeden typ za možnost pro celou životnost:

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Implementaci těchto rozhraní použití jedné třídy, `Operation`, která přijímá `Guid` v jeho konstruktoru nebo používá nové `Guid` Pokud žádný je k dispozici.

Vedle `ConfigureServices`, každý typ je přidat do kontejneru podle své životnosti s názvem:

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Všimněte si, že `IOperationSingletonInstance` služba používá konkrétní instanci s známé ID `Guid.Empty` tak bude zrušte při tento typ se používá (její identifikátor Guid bude samých nul). Také zaregistrovali `OperationService` to závisí na všech dalších `Operation` typy, takže bude zrušte v rámci žádost o tom, jestli je tato služba získávání stejnou instanci jako správce nebo novou pro každý typ operace. Tato služba nemá je vystavit jeho závislé součásti jako vlastnosti, takže je můžete zobrazit v zobrazení.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

K předvedení životnosti objektů v rámci a mezi samostatnou jednotlivé požadavky na aplikaci, zahrnuje vzorek `OperationsController` , každý požadavky druh `IOperation` typu a také `OperationService`. `Index` Akce poté zobrazí všechny řadiče a služby `OperationId` hodnoty.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Teď dvě samostatné žádosti o provedení této akce kontroleru:

![Operace zobrazení závislostí vkládání ukázkovou webovou aplikaci spuštěné v Microsoft Edge zobrazující hodnoty operaci ID (GUID) pro přechodná, Scoped, typu Singleton a Instance Kontroleru a operace operací služby na první požadavek.](dependency-injection/_static/lifetimes_request1.png)

![Operace zobrazit zobrazuje hodnoty ID operace pro další požadavek.](dependency-injection/_static/lifetimes_request2.png)

Sledovat, které `OperationId` hodnoty se liší v rámci požadavku a mezi požadavky.

* *Přechodný* objekty jsou vždy různých; novou instanci je zadán pro každý řadič a každou službu.

* *Obor* objekty jsou stejné v rámci požadavku, ale jiné napříč různé požadavky

* *Singleton* objekty jsou stejné pro všechny objekty a všechny žádosti o (bez ohledu na to, jestli je součástí instance `ConfigureServices`)

## <a name="scope-validation"></a>Ověření oboru

Když aplikace běží ve vývojovém prostředí na technologii ASP.NET Core 2.0 nebo novější, výchozím zprostředkovatelem služeb provádí kontroly ověřit, jestli:

* Vymezené služby nejsou přímo nebo nepřímo přeložit od kořenové poskytovatele služeb.
* Vymezená služby nejsou přímo nebo nepřímo vložit do jednotlivých prvků.

Kořenového poskytovatele služby se vytvoří při [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) je volána. Doba platnosti poskytovatele služeb kořenové odpovídá na aplikační nebo server životního cyklu, pokud zprostředkovatel začíná aplikace a uvolnění při vypnutí aplikace.

Vymezená služby jsou zapomenuty kontejnerem, který je vytvořil. Pokud vymezené služby se vytvoří v kořenovém kontejneru, životnost služby je efektivně povýšen na singleton, protože jejich pouze likvidace podle Kořenový kontejner při ukončení aplikace nebo serveru. Ověřování služby Obory zachytí těchto situacích při `BuildServiceProvider` je volána.

Další informace najdete v tématu [obor ověření v tomto tématu hostitelský](xref:fundamentals/hosting#scope-validation).

## <a name="request-services"></a>Žádost o služby

Žádosti o služby k dispozici v rámci technologie ASP.NET z `HttpContext` se zveřejňují přes `RequestServices` kolekce.

![Položka HttpContext žádosti o služby Intellisense kontextové dialogové okno s oznámením, že žádost o služby, získá nebo nastaví IServiceProvider, který poskytuje přístup ke kontejneru služby žádosti.](dependency-injection/_static/request-services.png)

Žádost o služby představují služby nakonfigurovat a požadavků v rámci vaší aplikace. Pokud vašich objektů určení závislostí, tyto jsou splněna typy najít v `RequestServices`, nikoli `ApplicationServices`.

Obecně byste neměli používat tyto vlastnosti přímo, upřednostňují místo toho k vyžádání typy tříd, které vyžadujete prostřednictvím konstruktoru třídy a, takže rozhraní vložit tyto závislosti. Dostaneme třídy, které se snadněji testování (viz [testování](../testing/index.md)) a jsou více volně vázány.

> [!NOTE]
> Dáváte přednost požaduje závislosti jako parametry konstruktor přístup `RequestServices` kolekce.

## <a name="designing-services-for-dependency-injection"></a>Navrhování služeb pro vkládání závislostí

Měli byste navrhnout vaše služby, aby získat jejich spolupracovníci pomocí vkládání závislostí. To znamená, že zabraňující použití stavová statickou metodu volání (důsledkem toho kód pach, označuje jako [statické plevami](http://deviq.com/static-cling/)) a přímé instance závislých tříd v rámci vašich služeb. Mohou pomoci pamatovat fráze, [nové je pojidlem](https://ardalis.com/new-is-glue), při výběru, zda se má vytvořit instanci typu nebo o pomocí vkládání závislostí. Pomocí následujících [plnou zásady z objektu zaměřené na konkrétní návrh](http://deviq.com/solid/), tříd se přirozeně jsou obvykle malé, dobře započítané a snadno otestované.

Co když zjistíte, že vaše třídy mívají k způsob příliš velký počet závislostí se vložit? Toto je obecně znaménkem, že se pokouší o provedení příliš mnoho třídě a pravděpodobně porušuje SRP - [jeden zásady odpovědnosti](http://deviq.com/single-responsibility-principle/). V tématu, pokud lze refaktorovat třída některé jeho odpovědnosti přesunutím do nové třídy. Mějte na paměti, že vaše `Controller` třídy musí zaměřit na aspekty uživatelského rozhraní, obchodní pravidla a data přístup implementace podrobnosti by měly být udržovány v příslušné tyto třídy [oddělit obavy](http://deviq.com/separation-of-concerns/).

S ohledem na přístup k datům konkrétně je můžete vložit `DbContext` do řadičů (za předpokladu, že jste přidali EF ke kontejneru služby v `ConfigureServices`). Někteří vývojáři dávají přednost používání rozhraní úložiště do databáze místo vložení `DbContext` přímo. Pomocí rozhraní pro zapouzdření data logiku přístup na jednom místě minimalizovat počet míst, budete muset změnit při změně databáze.

### <a name="disposing-of-services"></a>Uvolnění služeb

Kontejner zavolá `Dispose` pro `IDisposable` typy vytvoří. Ale pokud přidáte instanci ke kontejneru sami, se nebude uvolněno.

Příklad:

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
> Ve verzi 1.0, kontejner s názvem uvolnění na *všechny* `IDisposable` objekty, včetně těch, které ji vytvořit.

## <a name="replacing-the-default-services-container"></a>Nahrazení výchozího kontejneru služby

Kontejner vestavěné služby slouží k obsluhovat požadavky na základní rozhraní a většina příjemce aplikace založené na něm. Vývojáři však můžete nahradit kontejneru předdefinované jejich upřednostňovaný kontejneru. `ConfigureServices` Obvykle vrátí metoda `void`, ale pokud dojde ke změně jeho podpis vrátit `IServiceProvider`, jiný kontejner můžete nakonfigurovat a vrácena. Mnoho kontejnerů IOC nejsou k dispozici pro .NET. V tomto příkladu [Autofac](https://autofac.org/) se používá balíček.

Nejdřív nainstalujte balíčky odpovídajícího kontejneru:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

V dalším kroku nakonfigurujte kontejneru v `ConfigureServices` a vrátit `IServiceProvider`:

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
> Pokud používáte kontejner DI třetích stran, musíte změnit `ConfigureServices` tak, aby vracel `IServiceProvider` místo `void`.

Nakonec, nakonfigurujte Autofac jako normální v `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Za běhu se použije Autofac vyřešit typy a vložit závislosti. [Další informace o používání Autofac a ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Bezpečnost vlákna

Jednotlivý prvek služby musí být bezpečné pro přístup z více vláken. Pokud služba singleton má závislost na přechodný službu, službu přechodný také potřebovat podprocesů, podle toho, jak se používají v singleton.

## <a name="recommendations"></a>Doporučení

Při práci s vkládání závislostí, mějte následující doporučení:

* DI je pro objekty, které mají závislosti na komplexní. Všechny příklady objektů, které mohou být přidány do DI jsou řadiče, služby, adaptéry a úložiště.

* Vyhněte se přímo v DI ukládání dat a konfigurace. Například nákupní košík uživatele obvykle nepřidávat kontejner služeb. Konfigurace by měl používat [možnosti vzor](xref:fundamentals/configuration/options). Vyhněte se podobně "data držitelem" objekty, které existují pouze pro povolení přístupu k některé z jiného objektu. Je lepší požádat o položce skutečné potřeby prostřednictvím DI, pokud je to možné.

* Vyhněte se statickou přístup ke službám.

* Vyhněte se umístění služby v kódu aplikace.

* Vyhněte se statickou přístup k `HttpContext`.

> [!NOTE]
> Jako všech sad doporučení mohou nastat situace, kdy je potřeba jeden je ignorována. Našli jsme výjimky třeba výjimečných – většinou velmi zvláštních případech v rámci samotného.

Pamatujte si, že je vkládání závislostí *alternativní* na static, globální objekt přístupové vzorce. Nebudete moci pochopit výhody DI Pokud kombinujete statické objektu přístup.

## <a name="additional-resources"></a>Další zdroje

* [Spuštění aplikace](xref:fundamentals/startup)
* [Testování](xref:testing/index)
* [Aktivace na základě Factory middlewaru](xref:fundamentals/middleware/extensibility)
* [Psaní kódu vyčištění v ASP.NET Core pomocí vkládání závislostí (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Návrh aplikace spravované kontejneru, Prelude: Kde podporuje, patří kontejneru?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Princip explicitní závislosti.](http://deviq.com/explicit-dependencies-principle/)
* [Inverze – kontejnery ovládacích prvků a vzor vkládání závislostí](https://www.martinfowler.com/articles/injection.html) (Fowler)
