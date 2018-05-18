---
title: Úlohy na pozadí s hostované služby v ASP.NET Core
author: guardrex
description: Zjistěte, jak implementovat úlohy na pozadí s hostované služby v ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: ace7fc8622864099b7c0e36e4a914de340d4d4e9
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="1f0ec-103">Úlohy na pozadí s hostované služby v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f0ec-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="1f0ec-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1f0ec-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1f0ec-105">Ve ASP.NET Core, se dají implementovat úlohy na pozadí jako *hostovaných služeb*.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="1f0ec-106">Hostovaná služba je třída s pozadí úloh logiky, která implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="1f0ec-107">Toto téma obsahuje tři příklady hostované služby:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="1f0ec-108">Úlohy na pozadí, která běží na časovač.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="1f0ec-109">Hostovanou službu, která aktivuje vymezené služby.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="1f0ec-110">Vymezená služby můžete použít vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="1f0ec-111">Úlohy na pozadí zařazených do fronty, které spouští sekvenčně.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="1f0ec-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1f0ec-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="1f0ec-113">IHostedService rozhraní</span><span class="sxs-lookup"><span data-stu-id="1f0ec-113">IHostedService interface</span></span>

<span data-ttu-id="1f0ec-114">Implementace hostované služby [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="1f0ec-115">Rozhraní definuje dvě metody pro objekty, které jsou spravovány hostitele:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="1f0ec-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -volána po spuštění serveru a [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="1f0ec-117">`StartAsync` obsahuje logiku pro spuštění úlohy na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="1f0ec-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -aktivované, když se hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="1f0ec-119">`StopAsync` obsahuje logiku za účelem úlohy na pozadí a uvolnění jakýchkoli nespravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="1f0ec-120">Pokud aplikace ukončí neočekávaně (například aplikace proces selže), `StopAsync` nemusí být volána.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="1f0ec-121">Hostovaná služba je typu singleton, která se aktivuje jednou při spuštění aplikace a řádně vypnutí při vypnutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="1f0ec-122">Když [IDisposable](/dotnet/api/system.idisposable) je implementována, prostředky můžete uvolněno při uvolnění kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="1f0ec-123">Pokud se chyba při provádění úlohy pozadí, `Dispose` by měla být volána i v případě `StopAsync` není volán.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="1f0ec-124">Úlohy na pozadí vypršel</span><span class="sxs-lookup"><span data-stu-id="1f0ec-124">Timed background tasks</span></span>

<span data-ttu-id="1f0ec-125">Úlohy na pozadí vypršel využívá [System.Threading.Timer](/dotnet/api/system.threading.timer) třídy.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="1f0ec-126">Časovač se aktivuje úkolu `DoWork` metoda.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="1f0ec-127">Časovač je zakázána na `StopAsync` a uvolněno při uvolnění má v kontejneru služby `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="1f0ec-128">Služba je zaregistrován v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="1f0ec-129">Využívání vymezené služby v úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="1f0ec-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="1f0ec-130">Použití oboru služeb v rámci `IHostedService`, vytvoření oboru.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="1f0ec-131">Žádný obor se ve výchozím nastavení vytvoří pro hostovanou službu.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="1f0ec-132">Vymezená pozadí úloh služby obsahuje logiku úloha běžící na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="1f0ec-133">V následujícím příkladu [objektu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) je vložit do služby:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="1f0ec-134">Hostovaná služba vytvoří obor překlad služby úloh vymezená pozadí volat jeho `DoWork` metoda:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="1f0ec-135">Služby jsou zaregistrovány v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="1f0ec-136">Úlohy na pozadí ve frontě</span><span class="sxs-lookup"><span data-stu-id="1f0ec-136">Queued background tasks</span></span>

<span data-ttu-id="1f0ec-137">Fronty úloh na pozadí je založena na rozhraní .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([předběžně naplánované jako integrované pro ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="1f0ec-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="1f0ec-138">V `QueueHostedService`, úlohy na pozadí (`workItem`) ve frontě jsou vyjmutou a jsou prováděny:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="1f0ec-139">Služby jsou zaregistrovány v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="1f0ec-140">Ve třídě Index stránky modelu `IBackgroundTaskQueue` se vložit do konstruktoru a přiřadí do `Queue`:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="1f0ec-141">Když **přidat úloha** výběru tlačítka na indexovou stránku, `OnPostAddTask` spuštění metody.</span><span class="sxs-lookup"><span data-stu-id="1f0ec-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="1f0ec-142">`QueueBackgroundWorkItem` je volána zařadit do fronty pracovní položku:</span><span class="sxs-lookup"><span data-stu-id="1f0ec-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="1f0ec-143">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1f0ec-143">Additional resources</span></span>

* [<span data-ttu-id="1f0ec-144">Implementace úlohy na pozadí v mikroslužeb s IHostedService a BackgroundService – třída</span><span class="sxs-lookup"><span data-stu-id="1f0ec-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="1f0ec-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="1f0ec-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
