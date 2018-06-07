---
title: Úlohy na pozadí s hostované služby v ASP.NET Core
author: guardrex
description: Zjistěte, jak implementovat úlohy na pozadí s hostované služby v ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 122d09981ae314a65e4a0771cb7b9b3b02bcaa8e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819029"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Úlohy na pozadí s hostované služby v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Ve ASP.NET Core, se dají implementovat úlohy na pozadí jako *hostovaných služeb*. Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní. Toto téma obsahuje tři příklady hostované služby:

* Úlohy na pozadí, která běží na časovač.
* Hostovanou službu, která aktivuje vymezené služby. Vymezená služby můžete použít vkládání závislostí.
* Úlohy na pozadí zařazených do fronty, které spouští sekvenčně.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

Ukázková aplikace je zadáno ve dvou verzích:

* Webové hostitele &ndash; The webového hostitele je užitečné pro hostování webových aplikací. Příklad kódu ukazuje v tomto tématu je z verze webového hostitele vzorku. Další informace najdete v tématu [webového hostitele](xref:fundamentals/host/web-host) tématu.
* Obecné hostitele &ndash; obecné hostitel je v technologii ASP.NET Core 2.1 nového. Další informace najdete v tématu [obecné hostitele](xref:fundamentals/host/generic-host) tématu.

::: moniker-end

## <a name="ihostedservice-interface"></a>IHostedService rozhraní

Implementace hostované služby [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní. Rozhraní definuje dvě metody pro objekty, které jsou spravovány hostitele:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -volána po spuštění serveru a [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) se aktivuje. `StartAsync` obsahuje logiku pro spuštění úlohy na pozadí.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -aktivované, když se hostitel provádí řádné vypnutí. `StopAsync` obsahuje logiku za účelem úlohy na pozadí a uvolnění jakýchkoli nespravovaných prostředků. Pokud aplikace ukončí neočekávaně (například aplikace proces selže), `StopAsync` nemusí být volána.

Hostovaná služba je aktivována jednou při spuštění aplikace a řádně vypnutí při vypnutí aplikace. Když [IDisposable](/dotnet/api/system.idisposable) je implementována, prostředky můžete uvolněno při uvolnění kontejneru služby. Pokud se chyba při provádění úlohy pozadí, `Dispose` by měla být volána i v případě `StopAsync` není volán.

## <a name="timed-background-tasks"></a>Úlohy na pozadí vypršel

Úlohy na pozadí vypršel využívá [System.Threading.Timer](/dotnet/api/system.threading.timer) třídy. Časovač se aktivuje úkolu `DoWork` metoda. Časovač je zakázána na `StopAsync` a uvolněno při uvolnění má v kontejneru služby `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

Služba je zaregistrován v `Startup.ConfigureServices` s `AddHostedService` metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Služba je zaregistrován v `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Využívání vymezené služby v úlohy na pozadí

Použití oboru služeb v rámci `IHostedService`, vytvoření oboru. Žádný obor se ve výchozím nastavení vytvoří pro hostovanou službu.

Vymezená pozadí úloh služby obsahuje logiku úloha běžící na pozadí. V následujícím příkladu [objektu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) je vložit do služby:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Hostovaná služba vytvoří obor překlad služby úloh vymezená pozadí volat jeho `DoWork` metoda:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

Služby jsou zaregistrovány v `Startup.ConfigureServices`. `IHostedService` Implementace není zaregistrována `AddHostedService` metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Služby jsou zaregistrovány v `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

## <a name="queued-background-tasks"></a>Úlohy na pozadí ve frontě

Fronty úloh na pozadí je založena na rozhraní .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([předběžně naplánované jako integrované pro ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

V `QueueHostedService`, úlohy na pozadí (`workItem`) ve frontě jsou vyjmutou a jsou prováděny:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

Služby jsou zaregistrovány v `Startup.ConfigureServices`. `IHostedService` Implementace není zaregistrována `AddHostedService` metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Služby jsou zaregistrovány v `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

Ve třídě Index stránky modelu `IBackgroundTaskQueue` se vložit do konstruktoru a přiřadí do `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Když **přidat úloha** výběru tlačítka na indexovou stránku, `OnPostAddTask` spuštění metody. `QueueBackgroundWorkItem` je volána zařadit do fronty pracovní položku:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Další zdroje

* [Implementace úlohy na pozadí v mikroslužeb s IHostedService a BackgroundService – třída](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
