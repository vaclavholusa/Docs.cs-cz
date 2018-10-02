---
title: Úlohy na pozadí s hostovanými službami v ASP.NET Core
author: guardrex
description: Zjistěte, jak implementovat úlohy na pozadí s hostovanými službami v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 9c38b1e1d429498bcd59f780e3d3fe1a50eae32d
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860924"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Úlohy na pozadí s hostovanými službami v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

V ASP.NET Core, je možné implementovat úlohy na pozadí jako *hostovaných služeb*. Hostovaná služba je třída s logikou úlohy na pozadí a implementuje rozhraní <xref:Microsoft.Extensions.Hosting.IHostedService>. Toto téma obsahuje tři příklady hostované služby:

* Úlohy na pozadí, který běží na časovač.
* Hostovanou službu, která aktivuje vymezené služby. Injektáž závislostí můžete použít službu s vymezeným oborem.
* Úkoly ve frontě na pozadí, které spouští sekvenčně.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace je k dispozici ve dvou verzích:

* Webového hostitele &ndash; The webového hostitele je užitečné pro hostování webových aplikací. Příklad kódu v tomto tématu je od verze webového hostitele vzorku. Další informace najdete v tématu [webového hostitele](xref:fundamentals/host/web-host) tématu.
* Obecný hostitele &ndash; obecný hostitelské Novinky v ASP.NET Core 2.1. Další informace najdete v tématu [obecný hostitele](xref:fundamentals/host/generic-host) tématu.

## <a name="package"></a>Balíček

Odkaz [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) nebo přidat odkaz na balíček [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) balíčku.

## <a name="ihostedservice-interface"></a>IHostedService rozhraní

Hostované služby, které implementují <xref:Microsoft.Extensions.Hosting.IHostedService> rozhraní. Rozhraní definuje dvě metody pro objekty, které se spravují přes hostitele:

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*)  -  `StartAsync` obsahuje logiku pro spuštění úlohy na pozadí. Při použití [webového hostitele](xref:fundamentals/host/web-host), `StartAsync` se volá, když server spuštěn a [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) se aktivuje. Při použití [obecný hostitele](xref:fundamentals/host/generic-host), `StartAsync` je volána před provedením `ApplicationStarted` se aktivuje.

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) – aktivováno, když hostitel provádí řádné vypnutí. `StopAsync` obsahuje logiku pro ukončení úlohy na pozadí a uvolnění nespravovaných prostředků. Pokud se aplikace ukončí neočekávaně (například aplikace proces selže), `StopAsync` nemusí být volána.

Hostovaná služba je aktivována jednou při spuštění aplikace a řádné vypnutí při vypnutí aplikace. Když <xref:System.IDisposable> je implementováno, prostředky se dá uvolnit při uvolnění kontejneru služby. Pokud dojde k chybě při provádění úlohy na pozadí, `Dispose` by měla být volána i v případě `StopAsync` není volána.

## <a name="timed-background-tasks"></a>Úlohy na pozadí vypršel časový limit

Úlohy na pozadí vypršel časový limit využívá [System.Threading.Timer](xref:System.Threading.Timer) třídy. Časovač spustí úkolu `DoWork` metody. Časovač je zakázáno na `StopAsync` a uvolněno při uvolnění má v kontejneru služby `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Není registrován v `Startup.ConfigureServices` s `AddHostedService` – metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Vymezené služby v rámci úlohy na pozadí

Použití vymezené služby v rámci `IHostedService`, vytvoření oboru. Ve výchozím nastavení je vytvořen žádný obor pro hostovanou službu.

Služba úloh na pozadí s vymezeným oborem obsahuje logiku úlohy na pozadí. V následujícím příkladu <xref:Microsoft.Extensions.Logging.ILogger> se vloží do služby:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Hostovaná služba vytvoří obor vyřešit služba úloh na pozadí s vymezeným oborem volat jeho `DoWork` metody:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Služby jsou registrované ve `Startup.ConfigureServices`. `IHostedService` Zaregistruje implementaci `AddHostedService` – metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Úlohy na pozadí ve frontě

Fronta úloh na pozadí je založená na platformě .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([nezávazně naplánované být integrované pro ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

V `QueueHostedService`, úlohy na pozadí ve frontě jsou odstraněné z fronty a provést, protože <xref:Microsoft.Extensions.Hosting.BackgroundService>, což je základní třída pro implementaci dlouho běžící `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Služby jsou registrované ve `Startup.ConfigureServices`. `IHostedService` Zaregistruje implementaci `AddHostedService` – metoda rozšíření:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

V třídě modelu Index stránky `IBackgroundTaskQueue` se vloží do konstruktoru a přiřadí do `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Když **přidat úkol** výběru tlačítka na indexovou stránku, `OnPostAddTask` provedení metody. `QueueBackgroundWorkItem` je volána k zařazení do fronty pracovní položku:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Další zdroje

* [Implementace úloh na pozadí v mikroslužbách s IHostedService a BackgroundService třídy](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](xref:System.Threading.Timer)
