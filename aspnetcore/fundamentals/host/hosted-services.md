---
title: Úlohy na pozadí s hostovanými službami v ASP.NET Core
author: guardrex
description: Zjistěte, jak implementovat úlohy na pozadí s hostovanými službami v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 087ff4e1e169e1a1f76e93d4993441e47bafc945
ms.sourcegitcommit: 7097dba14d5b858e82758ee031ac62dbe3611339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39138594"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Úlohy na pozadí s hostovanými službami v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

V ASP.NET Core, je možné implementovat úlohy na pozadí jako *hostovaných služeb*. Hostovanou službu je třída s atributem logiky úloh na pozadí, který implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní. Toto téma obsahuje tři příklady hostované služby:

* Úlohy na pozadí, který běží na časovač.
* Hostovanou službu, která aktivuje vymezené služby. Injektáž závislostí můžete použít službu s vymezeným oborem.
* Úkoly ve frontě na pozadí, které spouští sekvenčně.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace je k dispozici ve dvou verzích:

* Webového hostitele &ndash; The webového hostitele je užitečné pro hostování webových aplikací. Příklad kódu v tomto tématu je od verze webového hostitele vzorku. Další informace najdete v tématu [webového hostitele](xref:fundamentals/host/web-host) tématu.
* Obecný hostitele &ndash; obecný hostitelské Novinky v ASP.NET Core 2.1. Další informace najdete v tématu [obecný hostitele](xref:fundamentals/host/generic-host) tématu.

## <a name="ihostedservice-interface"></a>IHostedService rozhraní

Hostované služby, které implementují [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní. Rozhraní definuje dvě metody pro objekty, které se spravují přes hostitele:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -volá se po spuštění serveru a [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) se aktivuje. `StartAsync` obsahuje logiku pro spuštění úlohy na pozadí.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) – aktivováno, když hostitel provádí řádné vypnutí. `StopAsync` obsahuje logiku pro ukončení úlohy na pozadí a uvolnění nespravovaných prostředků. Pokud se aplikace ukončí neočekávaně (například aplikace proces selže), `StopAsync` nemusí být volána.

Hostovaná služba je aktivována jednou při spuštění aplikace a řádné vypnutí při vypnutí aplikace. Když [IDisposable](/dotnet/api/system.idisposable) je implementováno, prostředky se dá uvolnit při uvolnění kontejneru služby. Pokud dojde k chybě při provádění úlohy na pozadí, `Dispose` by měla být volána i v případě `StopAsync` není volána.

## <a name="timed-background-tasks"></a>Úlohy na pozadí vypršel časový limit

Úlohy na pozadí vypršel časový limit využívá [System.Threading.Timer](/dotnet/api/system.threading.timer) třídy. Časovač spustí úkolu `DoWork` metody. Časovač je zakázáno na `StopAsync` a uvolněno při uvolnění má v kontejneru služby `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Není registrován v `Startup.ConfigureServices` s `AddHostedService` – metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Vymezené služby v rámci úlohy na pozadí

Použití vymezené služby v rámci `IHostedService`, vytvoření oboru. Ve výchozím nastavení je vytvořen žádný obor pro hostovanou službu.

Služba úloh na pozadí s vymezeným oborem obsahuje logiku úlohy na pozadí. V následujícím příkladu [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) se vloží do služby:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Hostovaná služba vytvoří obor vyřešit služba úloh na pozadí s vymezeným oborem volat jeho `DoWork` metody:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Služby jsou registrované ve `Startup.ConfigureServices`. `IHostedService` Zaregistruje implementaci `AddHostedService` – metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Úlohy na pozadí ve frontě

Fronta úloh na pozadí je založená na platformě .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([nezávazně naplánované být integrované pro ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

V `QueueHostedService`, úlohy na pozadí ve frontě jsou odstraněné z fronty a provést, protože [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice), což je základní třída pro implementaci dlouho běžící `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

Služby jsou registrované ve `Startup.ConfigureServices`. `IHostedService` Zaregistruje implementaci `AddHostedService` – metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

V třídě modelu Index stránky `IBackgroundTaskQueue` se vloží do konstruktoru a přiřadí do `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Když **přidat úkol** výběru tlačítka na indexovou stránku, `OnPostAddTask` provedení metody. `QueueBackgroundWorkItem` je volána k zařazení do fronty pracovní položku:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Další zdroje

* [Implementace úloh na pozadí v mikroslužbách s IHostedService a BackgroundService třídy](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
