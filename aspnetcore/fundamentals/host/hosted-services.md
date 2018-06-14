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
ms.openlocfilehash: 13ac7e266b657bc186188b2b6f40204cfd936fca
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341818"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="55686-103">Úlohy na pozadí s hostované služby v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55686-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="55686-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="55686-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="55686-105">Ve ASP.NET Core, se dají implementovat úlohy na pozadí jako *hostovaných služeb*.</span><span class="sxs-lookup"><span data-stu-id="55686-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="55686-106">Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="55686-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="55686-107">Toto téma obsahuje tři příklady hostované služby:</span><span class="sxs-lookup"><span data-stu-id="55686-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="55686-108">Úlohy na pozadí, která běží na časovač.</span><span class="sxs-lookup"><span data-stu-id="55686-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="55686-109">Hostovanou službu, která aktivuje vymezené služby.</span><span class="sxs-lookup"><span data-stu-id="55686-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="55686-110">Vymezená služby můžete použít vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="55686-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="55686-111">Úlohy na pozadí zařazených do fronty, které spouští sekvenčně.</span><span class="sxs-lookup"><span data-stu-id="55686-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="55686-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="55686-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="55686-113">Ukázková aplikace je zadáno ve dvou verzích:</span><span class="sxs-lookup"><span data-stu-id="55686-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="55686-114">Webové hostitele &ndash; The webového hostitele je užitečné pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="55686-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="55686-115">Příklad kódu ukazuje v tomto tématu je z verze webového hostitele vzorku.</span><span class="sxs-lookup"><span data-stu-id="55686-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="55686-116">Další informace najdete v tématu [webového hostitele](xref:fundamentals/host/web-host) tématu.</span><span class="sxs-lookup"><span data-stu-id="55686-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="55686-117">Obecné hostitele &ndash; obecné hostitel je v technologii ASP.NET Core 2.1 nového.</span><span class="sxs-lookup"><span data-stu-id="55686-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="55686-118">Další informace najdete v tématu [obecné hostitele](xref:fundamentals/host/generic-host) tématu.</span><span class="sxs-lookup"><span data-stu-id="55686-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="55686-119">IHostedService rozhraní</span><span class="sxs-lookup"><span data-stu-id="55686-119">IHostedService interface</span></span>

<span data-ttu-id="55686-120">Implementace hostované služby [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="55686-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="55686-121">Rozhraní definuje dvě metody pro objekty, které jsou spravovány hostitele:</span><span class="sxs-lookup"><span data-stu-id="55686-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="55686-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -volána po spuštění serveru a [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="55686-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="55686-123">`StartAsync` obsahuje logiku pro spuštění úlohy na pozadí.</span><span class="sxs-lookup"><span data-stu-id="55686-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="55686-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -aktivované, když se hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="55686-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="55686-125">`StopAsync` obsahuje logiku za účelem úlohy na pozadí a uvolnění jakýchkoli nespravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="55686-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="55686-126">Pokud aplikace ukončí neočekávaně (například aplikace proces selže), `StopAsync` nemusí být volána.</span><span class="sxs-lookup"><span data-stu-id="55686-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="55686-127">Hostovaná služba je aktivována jednou při spuštění aplikace a řádně vypnutí při vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="55686-127">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="55686-128">Když [IDisposable](/dotnet/api/system.idisposable) je implementována, prostředky můžete uvolněno při uvolnění kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="55686-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="55686-129">Pokud se chyba při provádění úlohy pozadí, `Dispose` by měla být volána i v případě `StopAsync` není volán.</span><span class="sxs-lookup"><span data-stu-id="55686-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="55686-130">Úlohy na pozadí vypršel</span><span class="sxs-lookup"><span data-stu-id="55686-130">Timed background tasks</span></span>

<span data-ttu-id="55686-131">Úlohy na pozadí vypršel využívá [System.Threading.Timer](/dotnet/api/system.threading.timer) třídy.</span><span class="sxs-lookup"><span data-stu-id="55686-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="55686-132">Časovač se aktivuje úkolu `DoWork` metoda.</span><span class="sxs-lookup"><span data-stu-id="55686-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="55686-133">Časovač je zakázána na `StopAsync` a uvolněno při uvolnění má v kontejneru služby `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="55686-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="55686-134">Služba je zaregistrován v `Startup.ConfigureServices` s `AddHostedService` metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="55686-134">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="55686-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="55686-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="55686-136">Služba je zaregistrován v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="55686-136">The service is registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="55686-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="55686-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="55686-138">Využívání vymezené služby v úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="55686-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="55686-139">Použití oboru služeb v rámci `IHostedService`, vytvoření oboru.</span><span class="sxs-lookup"><span data-stu-id="55686-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="55686-140">Žádný obor se ve výchozím nastavení vytvoří pro hostovanou službu.</span><span class="sxs-lookup"><span data-stu-id="55686-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="55686-141">Vymezená pozadí úloh služby obsahuje logiku úloha běžící na pozadí.</span><span class="sxs-lookup"><span data-stu-id="55686-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="55686-142">V následujícím příkladu [objektu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) je vložit do služby:</span><span class="sxs-lookup"><span data-stu-id="55686-142">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="55686-143">Hostovaná služba vytvoří obor překlad služby úloh vymezená pozadí volat jeho `DoWork` metoda:</span><span class="sxs-lookup"><span data-stu-id="55686-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="55686-144">Služby jsou zaregistrovány v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="55686-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="55686-145">`IHostedService` Implementace není zaregistrována `AddHostedService` metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="55686-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="55686-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="55686-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="55686-147">Služby jsou zaregistrovány v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="55686-147">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="55686-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="55686-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

## <a name="queued-background-tasks"></a><span data-ttu-id="55686-149">Úlohy na pozadí ve frontě</span><span class="sxs-lookup"><span data-stu-id="55686-149">Queued background tasks</span></span>

<span data-ttu-id="55686-150">Fronty úloh na pozadí je založena na rozhraní .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([předběžně naplánované jako integrované pro ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="55686-150">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="55686-151">V `QueueHostedService`, úlohy na pozadí (`workItem`) ve frontě jsou vyjmutou a jsou prováděny:</span><span class="sxs-lookup"><span data-stu-id="55686-151">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="55686-152">Služby jsou zaregistrovány v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="55686-152">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="55686-153">`IHostedService` Implementace není zaregistrována `AddHostedService` metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="55686-153">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="55686-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="55686-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="55686-155">Služby jsou zaregistrovány v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="55686-155">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="55686-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="55686-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="55686-157">Ve třídě Index stránky modelu `IBackgroundTaskQueue` se vložit do konstruktoru a přiřadí do `Queue`:</span><span class="sxs-lookup"><span data-stu-id="55686-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="55686-158">Když **přidat úloha** výběru tlačítka na indexovou stránku, `OnPostAddTask` spuštění metody.</span><span class="sxs-lookup"><span data-stu-id="55686-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="55686-159">`QueueBackgroundWorkItem` je volána zařadit do fronty pracovní položku:</span><span class="sxs-lookup"><span data-stu-id="55686-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="55686-160">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="55686-160">Additional resources</span></span>

* [<span data-ttu-id="55686-161">Implementace úlohy na pozadí v mikroslužeb s IHostedService a BackgroundService – třída</span><span class="sxs-lookup"><span data-stu-id="55686-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="55686-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="55686-162">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
