---
title: Protokolování v ASP.NET Core
author: ardalis
description: Další informace o rozhraní protokolování v ASP.NET Core. Zjistit předdefinované protokolování zprostředkovatele a další informace o oblíbených poskytovatelů třetích stran.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 8ba604ae8748455c95932f9d8843c1f7a5da2a06
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272760"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="d3b92-104">Protokolování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b92-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="d3b92-105">Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d3b92-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d3b92-106">Jádro ASP.NET podporuje protokolování rozhraní API, která funguje s různými zprostředkovatelů protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="d3b92-107">Předdefinované zprostředkovatele umožňují odeslat protokoly na jeden nebo více míst, a můžete zařadit rozhraní protokolování třetích stran.</span><span class="sxs-lookup"><span data-stu-id="d3b92-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="d3b92-108">Tento článek ukazuje, jak používat rozhraní API pro integrované protokolování a poskytovatelé ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d3b92-110">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3b92-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d3b92-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3b92-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="d3b92-113">Postup vytvoření protokoly</span><span class="sxs-lookup"><span data-stu-id="d3b92-113">How to create logs</span></span>

<span data-ttu-id="d3b92-114">Pokud chcete vytvořit protokoly, implementovat [objektu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) objektu z [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru:</span><span class="sxs-lookup"><span data-stu-id="d3b92-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="d3b92-115">Pak volejte metody protokolování pro tento objekt protokolovacího nástroje:</span><span class="sxs-lookup"><span data-stu-id="d3b92-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="d3b92-116">Tento příklad vytvoří protokoly s `TodoController` třídy jako *kategorie*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="d3b92-117">Kategorie jsou vysvětleny [dále v tomto článku](#log-category).</span><span class="sxs-lookup"><span data-stu-id="d3b92-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="d3b92-118">ASP.NET Core neposkytuje asynchronní metody protokolovacího nástroje, protože by měla být tak rychlé, že není vhodné nákladů na použití asynchronních protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="d3b92-119">Pokud jste v situaci, kde to není pravda, zvažte změnu způsob přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d3b92-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="d3b92-120">Pokud data store je pomalé, zápis zpráv protokolu rychlé úložiště nejprve, pak přesuňte je pomalé úložiště později.</span><span class="sxs-lookup"><span data-stu-id="d3b92-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="d3b92-121">Například Přihlaste se do fronty zpráv, který má číst a uložit trvale na pomalé úložiště jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="d3b92-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="d3b92-122">Postup přidání zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="d3b92-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-123">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d3b92-124">Zprostředkovatel protokolování přijímá zprávy, které vytvoříte pomocí `ILogger` objektu a zobrazuje nebo je uloží.</span><span class="sxs-lookup"><span data-stu-id="d3b92-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="d3b92-125">Například konzola poskytovatel zprávy zobrazí v konzole a zprostředkovatele služby Azure App Service je uložit do úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d3b92-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="d3b92-126">Chcete-li použít poskytovatele, volejte poskytovatele `Add<ProviderName>` metoda rozšíření v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d3b92-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="d3b92-127">Výchozí šablona projektu umožňuje protokolování pomocí [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metoda:</span><span class="sxs-lookup"><span data-stu-id="d3b92-127">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d3b92-129">Zprostředkovatel protokolování přijímá zprávy, které vytvoříte pomocí `ILogger` objektu a zobrazuje nebo je uloží.</span><span class="sxs-lookup"><span data-stu-id="d3b92-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="d3b92-130">Například konzola poskytovatel zprávy zobrazí v konzole a zprostředkovatele služby Azure App Service je uložit do úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d3b92-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="d3b92-131">Pro použití poskytovatele, nainstalujte jeho balíček NuGet a volání metody rozšíření poskytovatele na instanci [implementaci třídy ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d3b92-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="d3b92-132">ASP.NET Core [vkládání závislostí](xref:fundamentals/dependency-injection) (DI) poskytuje `ILoggerFactory` instance.</span><span class="sxs-lookup"><span data-stu-id="d3b92-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="d3b92-133">`AddConsole` a `AddDebug` rozšiřující metody jsou definovány v [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) a [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) balíčky.</span><span class="sxs-lookup"><span data-stu-id="d3b92-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="d3b92-134">Každá metoda rozšíření volá `ILoggerFactory.AddProvider` metodu předáním v instanci poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="d3b92-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="d3b92-135">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) Přidá zprostředkovatele protokolování v `Startup.Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="d3b92-135">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="d3b92-136">Pokud chcete získat výstup protokolu z kódu, který spouští dříve, přidejte zprostředkovatele protokolování v `Startup` konstruktoru třídy.</span><span class="sxs-lookup"><span data-stu-id="d3b92-136">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

---

<span data-ttu-id="d3b92-137">Najdete zde informace o jednotlivých [integrované protokolování zprostředkovatele](#built-in-logging-providers) a obsahuje odkazy na [poskytovatelů třetích stran protokolování](#third-party-logging-providers) dále v článku.</span><span class="sxs-lookup"><span data-stu-id="d3b92-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="d3b92-138">Ukázkový výstup protokolování</span><span class="sxs-lookup"><span data-stu-id="d3b92-138">Sample logging output</span></span>

<span data-ttu-id="d3b92-139">Ukázkový kód uvedené v předchozí části se zobrazí protokoly v konzole při spuštění z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d3b92-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="d3b92-140">Tady je příklad výstupu konzoly:</span><span class="sxs-lookup"><span data-stu-id="d3b92-140">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

<span data-ttu-id="d3b92-141">Tyto protokoly byly vytvořeny tak, že přejdete do `http://localhost:5000/api/todo/0`, která aktivuje spuštění obou `ILogger` volání uvedené v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="d3b92-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="d3b92-142">Tady je příklad stejné protokolů, jak jsou zobrazeny v okně ladění, když spustíte ukázkovou aplikaci v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d3b92-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="d3b92-143">Protokoly, které byly vytvořeny `ILogger` volání uvedené v předchozí části začínat řetězcem "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="d3b92-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="d3b92-144">Protokoly, které začínají kategorie "Microsoft" jsou z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3b92-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="d3b92-145">ASP.NET Core sám a kódu aplikace používají stejné rozhraní API protokolování a poskytovateli stejné protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="d3b92-146">Zbývající část tohoto článku popisuje některé podrobnosti a možnosti pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="d3b92-147">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="d3b92-147">NuGet packages</span></span>

<span data-ttu-id="d3b92-148">`ILogger` a `ILoggerFactory` rozhraní jsou v [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a výchozí implementace pro ně jsou v [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="d3b92-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="d3b92-149">Kategorie protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-149">Log category</span></span>

<span data-ttu-id="d3b92-150">A *kategorie* je zahrnut v každém protokolu, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="d3b92-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="d3b92-151">Zadejte kategorii, při vytváření `ILogger` objektu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="d3b92-152">Kategorie může být libovolný řetězec, ale konvenci, je použít plně kvalifikovaný název třídy, ze kterého se zapisují protokoly.</span><span class="sxs-lookup"><span data-stu-id="d3b92-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="d3b92-153">Například: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="d3b92-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="d3b92-154">Můžete zadat kategorii jako řetězec nebo použijte metodu rozšíření, která je odvozena kategorii z typu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="d3b92-155">Chcete-li zadat kategorii jako řetězec, volejte `CreateLogger` na `ILoggerFactory` instance, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d3b92-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="d3b92-156">Ve většině případů, je jednodušší použít `ILogger<T>`, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="d3b92-157">Jde o ekvivalent volání `CreateLogger` s názvem plně kvalifikovaný typ `T`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="d3b92-158">Úroveň protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-158">Log level</span></span>

<span data-ttu-id="d3b92-159">Pokaždé, když napíšete protokolu, zadejte jeho [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="d3b92-159">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="d3b92-160">Úroveň protokolu označuje stupeň závažnosti nebo důležitost.</span><span class="sxs-lookup"><span data-stu-id="d3b92-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="d3b92-161">Můžete například napsat `Information` protokolu při ukončení metody za normálních okolností `Warning` po návratu metody, návratový kód 404 a protokolu `Error` protokolu při catch neočekávanou výjimku.</span><span class="sxs-lookup"><span data-stu-id="d3b92-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="d3b92-162">V následujícím příkladu kódu, názvy metod (například `LogWarning`) zadejte úroveň protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="d3b92-163">První parametr je [protokolu událost s ID](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="d3b92-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="d3b92-164">Druhý parametr je [Šablona zprávy](#log-message-template) se zástupnými symboly pro argument hodnoty poskytované zbývající parametry metody.</span><span class="sxs-lookup"><span data-stu-id="d3b92-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="d3b92-165">Parametry metody jsou podrobně popsány dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d3b92-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="d3b92-166">Metody protokolu, které obsahují úroveň v název metody jsou [rozšiřující metody pro objektu ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="d3b92-166">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="d3b92-167">Na pozadí volat tyto metody `Log` metody, která přijímá `LogLevel` parametr.</span><span class="sxs-lookup"><span data-stu-id="d3b92-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="d3b92-168">Můžete volat `Log` metoda přímo spíše než jeden z těchto metod rozšíření, ale syntaxe je poměrně složitá.</span><span class="sxs-lookup"><span data-stu-id="d3b92-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="d3b92-169">Další informace najdete v tématu [objektu ILogger rozhraní](/dotnet/api/microsoft.extensions.logging.ilogger) a [rozšíření protokolovače zdrojový kód](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="d3b92-169">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="d3b92-170">ASP.NET Core definuje následující [protokolu úrovně](/dotnet/api/microsoft.extensions.logging.loglevel), seřazené zde z minimálně na nejvyšší závažnosti.</span><span class="sxs-lookup"><span data-stu-id="d3b92-170">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="d3b92-171">Trasování = 0</span><span class="sxs-lookup"><span data-stu-id="d3b92-171">Trace = 0</span></span>

  <span data-ttu-id="d3b92-172">Pro informace, které je vhodné pouze pro vývojáře, ladění problém.</span><span class="sxs-lookup"><span data-stu-id="d3b92-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="d3b92-173">Tyto zprávy mohou obsahovat citlivé aplikaci data a nemělo by být povolené v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3b92-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="d3b92-174">*Zakázané ve výchozím nastavení.*</span><span class="sxs-lookup"><span data-stu-id="d3b92-174">*Disabled by default.*</span></span> <span data-ttu-id="d3b92-175">Příklad: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="d3b92-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="d3b92-176">Ladění = 1</span><span class="sxs-lookup"><span data-stu-id="d3b92-176">Debug = 1</span></span>

  <span data-ttu-id="d3b92-177">Informace, která má krátkodobou užitečnost při vývoji a ladění.</span><span class="sxs-lookup"><span data-stu-id="d3b92-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="d3b92-178">Příklad: `Entering method Configure with flag set to true.` obvykle nebude povolit `Debug` úroveň protokolů v produkčním prostředí, pokud řešíte, kvůli velkému počtu protokoly.</span><span class="sxs-lookup"><span data-stu-id="d3b92-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="d3b92-179">Informace o = 2</span><span class="sxs-lookup"><span data-stu-id="d3b92-179">Information = 2</span></span>

  <span data-ttu-id="d3b92-180">Pro sledování obecný tok aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b92-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="d3b92-181">Tyto protokoly obvykle mají některé dlouhodobé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d3b92-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="d3b92-182">Příklad: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="d3b92-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="d3b92-183">Upozornění = 3</span><span class="sxs-lookup"><span data-stu-id="d3b92-183">Warning = 3</span></span>

  <span data-ttu-id="d3b92-184">Nestandardní nebo neočekávané události v toku aplikací.</span><span class="sxs-lookup"><span data-stu-id="d3b92-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="d3b92-185">Ty mohou obsahovat chyby nebo jinými podmínkami, které nemáte způsobit zastavení aplikace, ale které může potřebovat nutné prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="d3b92-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="d3b92-186">Zpracovávaný výjimky jsou obvyklé místo pro použití `Warning` úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="d3b92-187">Příklad: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="d3b92-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="d3b92-188">Chyba = 4</span><span class="sxs-lookup"><span data-stu-id="d3b92-188">Error = 4</span></span>

  <span data-ttu-id="d3b92-189">Chyby a výjimky, které nelze zpracovat.</span><span class="sxs-lookup"><span data-stu-id="d3b92-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="d3b92-190">Tyto zprávy označují selhání v aktuální aktivita nebo operace (například aktuální požadavek HTTP), není chybu celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3b92-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="d3b92-191">Příklad zprávy protokolu: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="d3b92-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="d3b92-192">Kritické = 5</span><span class="sxs-lookup"><span data-stu-id="d3b92-192">Critical = 5</span></span>

  <span data-ttu-id="d3b92-193">K selhání, které vyžadují okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="d3b92-193">For failures that require immediate attention.</span></span> <span data-ttu-id="d3b92-194">Příklady: ztrátě dat., nedostatek místa na disku.</span><span class="sxs-lookup"><span data-stu-id="d3b92-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="d3b92-195">Úroveň protokolu můžete určit, kolik protokolu výstup zapsán do média konkrétní úložiště a které zobrazí okno.</span><span class="sxs-lookup"><span data-stu-id="d3b92-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="d3b92-196">Například v produkčním prostředí můžete všechny protokoly `Information` úroveň a nižší přejít na úložiště dat svazek a všechny protokoly `Warning` úrovně a vyšší přejděte k úložišti dat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="d3b92-197">Během vývoje, může být obvykle odeslat protokoly `Warning` nebo vyšší závažnosti ke konzole.</span><span class="sxs-lookup"><span data-stu-id="d3b92-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="d3b92-198">Když potřebujete vyřešit, můžete přidat `Debug` úroveň.</span><span class="sxs-lookup"><span data-stu-id="d3b92-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="d3b92-199">[Filtrování protokolu](#log-filtering) později v tomto článku vysvětluje, jak řídit které protokolu úrovně zpracovává zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="d3b92-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="d3b92-200">Zapíše rozhraní ASP.NET Core `Debug` protokoly pro framework události na úrovni.</span><span class="sxs-lookup"><span data-stu-id="d3b92-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="d3b92-201">Příklady protokolu dříve v tomto článku vyloučené protokoly níže `Information` úroveň, takže žádná `Debug` úrovně protokoly byly vidět.</span><span class="sxs-lookup"><span data-stu-id="d3b92-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="d3b92-202">Tady je příklad protokoly konzoly, pokud spustíte ukázkovou aplikaci nakonfigurovat tak, aby zobrazit `Debug` a vyšší protokoly pro zprostředkovatele konzoly.</span><span class="sxs-lookup"><span data-stu-id="d3b92-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="d3b92-203">ID události protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-203">Log event ID</span></span>

<span data-ttu-id="d3b92-204">Pokaždé, když napíšete protokolu, můžete zadat *ID události*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="d3b92-205">Ukázková aplikace k tomu pomocí místně definované `LoggingEvents` třídy:</span><span class="sxs-lookup"><span data-stu-id="d3b92-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="d3b92-206">ID události je celočíselná hodnota, která můžete přidružit jednu na druhou sadu protokolované události.</span><span class="sxs-lookup"><span data-stu-id="d3b92-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="d3b92-207">Například protokolu pro přidání položky do nákupního košíku může být událost ID 1000 a protokolu pro dokončení nákupu může být událost ID 1001.</span><span class="sxs-lookup"><span data-stu-id="d3b92-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="d3b92-208">Ve výstupu protokolování může uložené v poli ID události nebo součástí textové zprávy, v závislosti na zprostředkovateli.</span><span class="sxs-lookup"><span data-stu-id="d3b92-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="d3b92-209">Ladění zprostředkovatele nezobrazí ID událostí, ale konzola poskytovatel je zobrazuje v závorkách za kategorií:</span><span class="sxs-lookup"><span data-stu-id="d3b92-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="d3b92-210">Šablona zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-210">Log message template</span></span>

<span data-ttu-id="d3b92-211">Pokaždé, když napíšete zprávu protokolu poskytnete Šablona zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b92-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="d3b92-212">Šablona zprávy může být řetězec, nebo může obsahovat pojmenované zástupné symboly, do které argument hodnoty budou vloženy.</span><span class="sxs-lookup"><span data-stu-id="d3b92-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="d3b92-213">Šablony není řetězec formátu, a zástupné symboly by měla mít název, není číslované.</span><span class="sxs-lookup"><span data-stu-id="d3b92-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="d3b92-214">Pořadí zástupných symbolů, není jejich názvy, určuje, které parametry slouží k zadání jejich hodnot.</span><span class="sxs-lookup"><span data-stu-id="d3b92-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="d3b92-215">Pokud máte následující kód:</span><span class="sxs-lookup"><span data-stu-id="d3b92-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="d3b92-216">Výsledný zprávy protokolu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d3b92-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="d3b92-217">Rozhraní protokolování zpráv formátování tímto způsobem, aby bylo možné pro protokolování zprostředkovatele implementovat [sémantické protokolování, také známé jako strukturovaný protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="d3b92-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="d3b92-218">Protože jsou argumenty sami předat protokolování systému, ne jenom šablony formátovaná zpráva zprostředkovatelé protokolování může ukládat hodnoty parametru jako pole kromě Šablona zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b92-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="d3b92-219">Pokud jste odkazovat protokolu výstup Azure Table Storage a volání metody protokolovacího nástroje vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d3b92-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="d3b92-220">Každá entita Azure Table může mít `ID` a `RequestTime` vlastnosti, které zjednodušuje dotazy na data protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="d3b92-221">Můžete najít všechny protokoly v rámci konkrétní `RequestTime` rozsah bez nutnosti analyzovat časový limit textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="d3b92-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="d3b92-222">Protokolování výjimky</span><span class="sxs-lookup"><span data-stu-id="d3b92-222">Logging exceptions</span></span>

<span data-ttu-id="d3b92-223">Metody protokolovacího nástroje, mají přetížení, které umožňují předat výjimku, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d3b92-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="d3b92-224">Různé zprostředkovatele zpracovat informace o výjimce různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="d3b92-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="d3b92-225">Tady je příklad výstupu ladění zprostředkovatele z výše uvedeném kódu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="d3b92-226">Filtrování protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-227">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d3b92-228">Můžete zadat úroveň minimální protokolu pro konkrétního zprostředkovatele a kategorie nebo pro všechny poskytovatele nebo všechny kategorie.</span><span class="sxs-lookup"><span data-stu-id="d3b92-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="d3b92-229">Žádné protokoly nižší než minimální úroveň nejsou předaný tohoto zprostředkovatele, takže nemusíte získat nezobrazí nebo uložené.</span><span class="sxs-lookup"><span data-stu-id="d3b92-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="d3b92-230">Pokud chcete potlačit všechny protokoly, můžete zadat `LogLevel.None` jako úroveň minimální protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="d3b92-231">Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="d3b92-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="d3b92-232">**Vytvoření pravidla filtru v konfiguraci**</span><span class="sxs-lookup"><span data-stu-id="d3b92-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="d3b92-233">Šablony projektů vytvořit kód, který volá `CreateDefaultBuilder` nastavení protokolování pro zprostředkovatele konzoly a ladění.</span><span class="sxs-lookup"><span data-stu-id="d3b92-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="d3b92-234">`CreateDefaultBuilder` Metoda také nastaví protokolování tak, aby hledat konfiguraci v `Logging` části pomocí kódu takto:</span><span class="sxs-lookup"><span data-stu-id="d3b92-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="d3b92-235">Konfigurační data Určuje minimální protokolu úrovně zprostředkovatele a kategorie, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d3b92-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="d3b92-236">Tento formát JSON vytvoří šesti pravidla filtru, jeden pro zprostředkovatele ladění, čtyři pro zprostředkovatele konzoly a ten, který se vztahuje na všechny poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="d3b92-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="d3b92-237">Zobrazí se později jak právě jeden z těchto pravidel je zvolen pro každého zprostředkovatele při `ILogger` je vytvořen objekt.</span><span class="sxs-lookup"><span data-stu-id="d3b92-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="d3b92-238">**Pravidla filtru v kódu**</span><span class="sxs-lookup"><span data-stu-id="d3b92-238">**Filter rules in code**</span></span>

<span data-ttu-id="d3b92-239">V kódu, můžete zaregistrovat pravidla filtru, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d3b92-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="d3b92-240">Druhý `AddFilter` Určuje zprostředkovatele, který ladění pomocí jeho názvu typu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="d3b92-241">První `AddFilter` platí pro všechny poskytovatele, protože neurčuje typ poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="d3b92-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="d3b92-242">**Jak pravidla filtrování se použijí.**</span><span class="sxs-lookup"><span data-stu-id="d3b92-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="d3b92-243">Konfigurační data a `AddFilter` uvedeném v předchozích ukázkách kódu vytvořit pravidla uvedené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="d3b92-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="d3b92-244">Prvních šest pocházet z příklad konfigurace a poslední dva pocházet z příkladu kódu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="d3b92-245">Číslo</span><span class="sxs-lookup"><span data-stu-id="d3b92-245">Number</span></span> | <span data-ttu-id="d3b92-246">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="d3b92-246">Provider</span></span>      | <span data-ttu-id="d3b92-247">Kategorie, které začínají...</span><span class="sxs-lookup"><span data-stu-id="d3b92-247">Categories that begin with ...</span></span>          | <span data-ttu-id="d3b92-248">Úroveň minimální protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="d3b92-249">1</span><span class="sxs-lookup"><span data-stu-id="d3b92-249">1</span></span>      | <span data-ttu-id="d3b92-250">Ladit</span><span class="sxs-lookup"><span data-stu-id="d3b92-250">Debug</span></span>         | <span data-ttu-id="d3b92-251">Všechny kategorie</span><span class="sxs-lookup"><span data-stu-id="d3b92-251">All categories</span></span>                          | <span data-ttu-id="d3b92-252">Informace o</span><span class="sxs-lookup"><span data-stu-id="d3b92-252">Information</span></span>       |
| <span data-ttu-id="d3b92-253">2</span><span class="sxs-lookup"><span data-stu-id="d3b92-253">2</span></span>      | <span data-ttu-id="d3b92-254">Konzola</span><span class="sxs-lookup"><span data-stu-id="d3b92-254">Console</span></span>       | <span data-ttu-id="d3b92-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="d3b92-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="d3b92-256">Upozornění</span><span class="sxs-lookup"><span data-stu-id="d3b92-256">Warning</span></span>           |
| <span data-ttu-id="d3b92-257">3</span><span class="sxs-lookup"><span data-stu-id="d3b92-257">3</span></span>      | <span data-ttu-id="d3b92-258">Konzola</span><span class="sxs-lookup"><span data-stu-id="d3b92-258">Console</span></span>       | <span data-ttu-id="d3b92-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="d3b92-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="d3b92-260">Ladit</span><span class="sxs-lookup"><span data-stu-id="d3b92-260">Debug</span></span>             |
| <span data-ttu-id="d3b92-261">4</span><span class="sxs-lookup"><span data-stu-id="d3b92-261">4</span></span>      | <span data-ttu-id="d3b92-262">Konzola</span><span class="sxs-lookup"><span data-stu-id="d3b92-262">Console</span></span>       | <span data-ttu-id="d3b92-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="d3b92-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="d3b92-264">Chyba</span><span class="sxs-lookup"><span data-stu-id="d3b92-264">Error</span></span>             |
| <span data-ttu-id="d3b92-265">5</span><span class="sxs-lookup"><span data-stu-id="d3b92-265">5</span></span>      | <span data-ttu-id="d3b92-266">Konzola</span><span class="sxs-lookup"><span data-stu-id="d3b92-266">Console</span></span>       | <span data-ttu-id="d3b92-267">Všechny kategorie</span><span class="sxs-lookup"><span data-stu-id="d3b92-267">All categories</span></span>                          | <span data-ttu-id="d3b92-268">Informace o</span><span class="sxs-lookup"><span data-stu-id="d3b92-268">Information</span></span>       |
| <span data-ttu-id="d3b92-269">6</span><span class="sxs-lookup"><span data-stu-id="d3b92-269">6</span></span>      | <span data-ttu-id="d3b92-270">Všichni poskytovatelé</span><span class="sxs-lookup"><span data-stu-id="d3b92-270">All providers</span></span> | <span data-ttu-id="d3b92-271">Všechny kategorie</span><span class="sxs-lookup"><span data-stu-id="d3b92-271">All categories</span></span>                          | <span data-ttu-id="d3b92-272">Ladit</span><span class="sxs-lookup"><span data-stu-id="d3b92-272">Debug</span></span>             |
| <span data-ttu-id="d3b92-273">7</span><span class="sxs-lookup"><span data-stu-id="d3b92-273">7</span></span>      | <span data-ttu-id="d3b92-274">Všichni poskytovatelé</span><span class="sxs-lookup"><span data-stu-id="d3b92-274">All providers</span></span> | <span data-ttu-id="d3b92-275">Systém</span><span class="sxs-lookup"><span data-stu-id="d3b92-275">System</span></span>                                  | <span data-ttu-id="d3b92-276">Ladit</span><span class="sxs-lookup"><span data-stu-id="d3b92-276">Debug</span></span>             |
| <span data-ttu-id="d3b92-277">8</span><span class="sxs-lookup"><span data-stu-id="d3b92-277">8</span></span>      | <span data-ttu-id="d3b92-278">Ladit</span><span class="sxs-lookup"><span data-stu-id="d3b92-278">Debug</span></span>         | <span data-ttu-id="d3b92-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="d3b92-279">Microsoft</span></span>                               | <span data-ttu-id="d3b92-280">trasování</span><span class="sxs-lookup"><span data-stu-id="d3b92-280">Trace</span></span>             |

<span data-ttu-id="d3b92-281">Při vytváření `ILogger` objekt zápis protokolů, `ILoggerFactory` objekt vybere jedno pravidlo na zprostředkovatele pro použití tohoto protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="d3b92-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="d3b92-282">Všechny zprávy, které jsou napsané v tomto `ILogger` objekt jsou filtrovány podle vybraná pravidla.</span><span class="sxs-lookup"><span data-stu-id="d3b92-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="d3b92-283">Většina konkrétní pravidlo možné pro každou kategorii dvojice a zprostředkovatele se vybere z dostupná pravidla.</span><span class="sxs-lookup"><span data-stu-id="d3b92-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="d3b92-284">Následující algoritmus se používá pro každého zprostředkovatele při `ILogger` se vytvoří pro danou kategorii:</span><span class="sxs-lookup"><span data-stu-id="d3b92-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="d3b92-285">Vyberte všechna pravidla, které odpovídají zprostředkovatel nebo jeho alias.</span><span class="sxs-lookup"><span data-stu-id="d3b92-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="d3b92-286">Pokud nejsou nalezeny, vyberte všechna pravidla s poskytovatele prázdný.</span><span class="sxs-lookup"><span data-stu-id="d3b92-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="d3b92-287">Od výsledku v předchozím kroku vyberte pravidla s nejdéle odpovídající kategorii předponu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="d3b92-288">Pokud nejsou nalezeny, vyberte všechna pravidla, které nechcete zadat kategorii.</span><span class="sxs-lookup"><span data-stu-id="d3b92-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="d3b92-289">Pokud je vybraných víc pravidel trvat **poslední** jeden.</span><span class="sxs-lookup"><span data-stu-id="d3b92-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="d3b92-290">Pokud je vybrána žádná pravidla, použijte `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-290">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="d3b92-291">Předpokládejme například, máte předchozí seznam pravidel a vytvoříte `ILogger` objektu pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="d3b92-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="d3b92-292">Ladění zprostředkovatele platí pravidla 1, 6 a 8.</span><span class="sxs-lookup"><span data-stu-id="d3b92-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="d3b92-293">Pravidlo 8 je nejvíce konkrétní tak, aby se jeden vybrané.</span><span class="sxs-lookup"><span data-stu-id="d3b92-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="d3b92-294">Pro zprostředkovatele konzoly platí pravidla 3, 4, 5 a 6.</span><span class="sxs-lookup"><span data-stu-id="d3b92-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="d3b92-295">Pravidlo 3 je nejvíce.</span><span class="sxs-lookup"><span data-stu-id="d3b92-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="d3b92-296">Při vytváření protokoly s `ILogger` pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", protokoly z `Trace` úrovni a vyšší přejde na ladění zprostředkovatele a protokoly `Debug` úrovni a vyšší přejde k poskytovateli konzoly.</span><span class="sxs-lookup"><span data-stu-id="d3b92-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="d3b92-297">**Aliasy zprostředkovatele**</span><span class="sxs-lookup"><span data-stu-id="d3b92-297">**Provider aliases**</span></span>

<span data-ttu-id="d3b92-298">Název typu můžete zadat poskytovatele v konfiguraci, ale každý poskytovatel definuje kratší *alias* , je jednodušší použít.</span><span class="sxs-lookup"><span data-stu-id="d3b92-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="d3b92-299">Pro předdefinované zprostředkovatele použijte následující aliasy:</span><span class="sxs-lookup"><span data-stu-id="d3b92-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="d3b92-300">Konzola</span><span class="sxs-lookup"><span data-stu-id="d3b92-300">Console</span></span>
- <span data-ttu-id="d3b92-301">Ladit</span><span class="sxs-lookup"><span data-stu-id="d3b92-301">Debug</span></span>
- <span data-ttu-id="d3b92-302">Protokol událostí</span><span class="sxs-lookup"><span data-stu-id="d3b92-302">EventLog</span></span>
- <span data-ttu-id="d3b92-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="d3b92-303">AzureAppServices</span></span>
- <span data-ttu-id="d3b92-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d3b92-304">TraceSource</span></span>
- <span data-ttu-id="d3b92-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="d3b92-305">EventSource</span></span>

<span data-ttu-id="d3b92-306">**Výchozí minimální úroveň**</span><span class="sxs-lookup"><span data-stu-id="d3b92-306">**Default minimum level**</span></span>

<span data-ttu-id="d3b92-307">Není k dispozici minimální nastavení úrovně, který se uplatní pouze v případě, že žádná pravidla z konfigurace nebo kód platí pro daného zprostředkovatele a kategorie.</span><span class="sxs-lookup"><span data-stu-id="d3b92-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="d3b92-308">Následující příklad ukazuje, jak nastavit minimální úroveň:</span><span class="sxs-lookup"><span data-stu-id="d3b92-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="d3b92-309">Pokud není explicitně nastavit minimální úroveň, výchozí hodnota je `Information`, to znamená, že `Trace` a `Debug` protokoly jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="d3b92-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="d3b92-310">**Funkce filtru**</span><span class="sxs-lookup"><span data-stu-id="d3b92-310">**Filter functions**</span></span>

<span data-ttu-id="d3b92-311">Můžete napsat kód ve funkci filtru pro použití pravidel filtrování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="d3b92-312">Pro všechny zprostředkovatele a kategorie, které nemají přiřazenou konfigurace nebo kód pravidla je volána funkce filtru.</span><span class="sxs-lookup"><span data-stu-id="d3b92-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="d3b92-313">Kód ve funkci má přístup k typ zprostředkovatele, kategorie a úroveň protokolu se rozhodnout, zda mají být protokolovány zprávu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="d3b92-314">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d3b92-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d3b92-316">Někteří poskytovatelé protokolování umožňují určit, kdy by měla být protokoly na médium úložiště nebo ignorovat na základě úroveň protokolu a kategorie.</span><span class="sxs-lookup"><span data-stu-id="d3b92-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="d3b92-317">`AddConsole` a `AddDebug` metody rozšíření poskytují přetížení, které umožňují předat kritéria filtrování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="d3b92-318">Následující vzorový kód způsobí, že konzola poskytovatel ignorovat protokoly níže `Warning` úroveň, při ladění zprostředkovatele ignoruje protokoly, které vytvoří rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d3b92-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="d3b92-319">`AddEventLog` Metoda má přetížení, které přijímá `EventLogSettings` instanci, která může obsahovat filtrování funkce v jeho `Filter` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d3b92-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="d3b92-320">Zprostředkovatel TraceSource neposkytuje, žádný z těchto přetížení vzhledem k tomu, že jsou na základě jeho úroveň protokolování a dalších parametrů `SourceSwitch` a `TraceListener` používá.</span><span class="sxs-lookup"><span data-stu-id="d3b92-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="d3b92-321">Můžete nastavit pravidla filtrování pro všechny poskytovatele, které jsou registrovány `ILoggerFactory` instance pomocí `WithFilter` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d3b92-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="d3b92-322">Následující příklad omezuje protokoly framework (kategorie začíná "Microsoft" nebo "Systém") a upozornění při upozornění v protokolu aplikace na úrovni ladění.</span><span class="sxs-lookup"><span data-stu-id="d3b92-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="d3b92-323">Pokud chcete zabránit zápisu pro danou kategorii všechny protokoly pomocí filtrování, můžete zadat `LogLevel.None` jako úroveň minimální protokolu této kategorie.</span><span class="sxs-lookup"><span data-stu-id="d3b92-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="d3b92-324">Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="d3b92-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="d3b92-325">`WithFilter` Rozšíření metoda je poskytována [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3b92-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="d3b92-326">Metoda vrátí novou `ILoggerFactory` instance, která bude filtrovat zprávy protokolu předaný všechny protokoly poskytovatele registrované s ním.</span><span class="sxs-lookup"><span data-stu-id="d3b92-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="d3b92-327">Nemá vliv, jakékoliv `ILoggerFactory` instance, včetně původní `ILoggerFactory` instance.</span><span class="sxs-lookup"><span data-stu-id="d3b92-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="d3b92-328">Obory protokolu</span><span class="sxs-lookup"><span data-stu-id="d3b92-328">Log scopes</span></span>

<span data-ttu-id="d3b92-329">Můžete seskupit sadu logické operace v rámci *oboru* Chcete-li přiřadit každý protokol, který je vytvořen jako součást této sady stejná data.</span><span class="sxs-lookup"><span data-stu-id="d3b92-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="d3b92-330">Například můžete každý protokol vytvořené jako součást zpracování transakci zahrnují ID transakce.</span><span class="sxs-lookup"><span data-stu-id="d3b92-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="d3b92-331">Obor je `IDisposable` typ, který je vrácený [ILogger.BeginScope&lt;TState&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) metoda a bude trvat, dokud je zrušen.</span><span class="sxs-lookup"><span data-stu-id="d3b92-331">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="d3b92-332">Použít obor podle zabalení vaše protokoly volání metod `using` blokovat, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="d3b92-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="d3b92-333">Následující kód umožňuje obory pro zprostředkovatele konzoly:</span><span class="sxs-lookup"><span data-stu-id="d3b92-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-334">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d3b92-335">V *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d3b92-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="d3b92-336">Konfigurace `IncludeScopes` možnost protokolovacího nástroje Konzola je potřeba povolit protokolování obor.</span><span class="sxs-lookup"><span data-stu-id="d3b92-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="d3b92-337">Konfigurace `IncludeScopes` pomocí *appsettings* konfigurační soubory bude k dispozici ve verzi ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d3b92-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d3b92-339">V *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d3b92-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="d3b92-340">Každé zprávě protokolu obsahuje informace o oboru:</span><span class="sxs-lookup"><span data-stu-id="d3b92-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="d3b92-341">Zprostředkovatelé integrované protokolování</span><span class="sxs-lookup"><span data-stu-id="d3b92-341">Built-in logging providers</span></span>

<span data-ttu-id="d3b92-342">ASP.NET Core dodává tyto zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="d3b92-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="d3b92-343">Console</span><span class="sxs-lookup"><span data-stu-id="d3b92-343">Console</span></span>](#console-provider)
* [<span data-ttu-id="d3b92-344">Ladění</span><span class="sxs-lookup"><span data-stu-id="d3b92-344">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="d3b92-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="d3b92-345">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="d3b92-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="d3b92-346">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="d3b92-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d3b92-347">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="d3b92-348">Aplikační služba Azure</span><span class="sxs-lookup"><span data-stu-id="d3b92-348">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="d3b92-349">Konzola poskytovatel</span><span class="sxs-lookup"><span data-stu-id="d3b92-349">Console provider</span></span>

<span data-ttu-id="d3b92-350">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) balíček zprostředkovatele odesílá výstup protokolu ke konzole.</span><span class="sxs-lookup"><span data-stu-id="d3b92-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-351">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="d3b92-353">[Přetížení AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) umožňují předáte v úroveň minimální protokolu, funkce filtru a logická hodnota, která označuje, zda obory.</span><span class="sxs-lookup"><span data-stu-id="d3b92-353">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="d3b92-354">Další možností je předat `IConfiguration` objektu, který můžete určit podporu obory a úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="d3b92-355">Pokud uvažujete o poskytovateli konzoly pro použití v produkčním prostředí, mějte na paměti, že má významný dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="d3b92-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="d3b92-356">Když vytvoříte nový projekt v sadě Visual Studio `AddConsole` metoda vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d3b92-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="d3b92-357">Tento kód odkazuje `Logging` části *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b92-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="d3b92-358">Nastavení při povolení aplikace zobrazí protokoly framework limit upozornění k protokolování na úrovni ladění, jak je popsáno v [filtrování protokolu](#log-filtering) části.</span><span class="sxs-lookup"><span data-stu-id="d3b92-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="d3b92-359">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d3b92-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

### <a name="debug-provider"></a><span data-ttu-id="d3b92-360">Ladění zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="d3b92-360">Debug provider</span></span>

<span data-ttu-id="d3b92-361">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) balíček zprostředkovatele zapíše výstup protokolu pomocí [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) – třída (`Debug.WriteLine` volání metod).</span><span class="sxs-lookup"><span data-stu-id="d3b92-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="d3b92-362">V systému Linux, tohoto zprostředkovatele zapisuje protokoly do */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-363">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="d3b92-365">[Přetížení AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) umožňují předáte v úroveň minimální protokolu nebo funkce filtru.</span><span class="sxs-lookup"><span data-stu-id="d3b92-365">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

### <a name="eventsource-provider"></a><span data-ttu-id="d3b92-366">EventSource zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="d3b92-366">EventSource provider</span></span>

<span data-ttu-id="d3b92-367">Pro aplikace, které cílí ASP.NET Core 1.1.0 nebo vyšší, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) balíček zprostředkovatele můžete implementovat trasování událostí.</span><span class="sxs-lookup"><span data-stu-id="d3b92-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="d3b92-368">V systému Windows, používá [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="d3b92-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="d3b92-369">Zprostředkovatel je platformy, ale nejsou žádná událost shromažďování a zobrazení nástroje pro Linux nebo systému macOS ještě.</span><span class="sxs-lookup"><span data-stu-id="d3b92-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-370">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="d3b92-372">Dobrým způsobem, jak shromáždit a zobrazit protokoly se má používat [nástroje PerfView nástroj](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="d3b92-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="d3b92-373">Existují další nástroje pro prohlížení protokolů trasování událostí pro Windows, ale nástroje PerfView přináší nejlepší výsledky pro práci s události ETW vygenerované pomocí technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d3b92-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="d3b92-374">Konfigurace nástroje PerfView pro shromažďování události zapsané podle tohoto zprostředkovatele, přidejte řetězec `*Microsoft-Extensions-Logging` k **další poskytovatele** seznamu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="d3b92-375">(Nezapomeňte si projít hvězdičky na začátku řetězce.)</span><span class="sxs-lookup"><span data-stu-id="d3b92-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Další poskytovatele nástroje Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="d3b92-377">Poskytovatel protokolu událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="d3b92-377">Windows EventLog provider</span></span>

<span data-ttu-id="d3b92-378">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) balíček zprostředkovatele odesílá výstup protokolu do protokolu událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d3b92-378">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-379">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="d3b92-381">[Přetížení AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) umožňují předáte v `EventLogSettings` nebo úroveň minimální protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-381">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

### <a name="tracesource-provider"></a><span data-ttu-id="d3b92-382">TraceSource zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="d3b92-382">TraceSource provider</span></span>

<span data-ttu-id="d3b92-383">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) používá balíček zprostředkovatele [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) knihovny a zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="d3b92-383">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-384">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-384">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-385">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-385">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="d3b92-386">[Přetížení AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) umožňují předáte v přepínač zdroje a naslouchací proces trasování.</span><span class="sxs-lookup"><span data-stu-id="d3b92-386">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="d3b92-387">Pro tohoto zprostředkovatele použijte aplikace má ke spuštění na rozhraní .NET Framework (nikoli .NET Core).</span><span class="sxs-lookup"><span data-stu-id="d3b92-387">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="d3b92-388">Umožňuje zprostředkovatele směrování zpráv do různých [naslouchací procesy](/dotnet/framework/debug-trace-profile/trace-listeners), například [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) použít v ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3b92-388">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="d3b92-389">Následující příklad konfiguruje `TraceSource` zprostředkovatele, který protokoluje `Warning` a vyšší zprávy v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="d3b92-389">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="d3b92-390">Zprostředkovatel služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d3b92-390">Azure App Service provider</span></span>

<span data-ttu-id="d3b92-391">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) balíček zprostředkovatele zapisuje protokoly do textových souborů v systému souborů aplikace služby Azure App Service a na [úložiště objektů blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) v účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d3b92-391">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="d3b92-392">Zprostředkovatel je dostupné pouze pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d3b92-392">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d3b92-393">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d3b92-394">Cílení na .NET Core, nemusíte instalovat balíček zprostředkovatele nebo explicitně volejte [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="d3b92-394">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="d3b92-395">Zprostředkovatel je automaticky dostupný v aplikaci při nasazení aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="d3b92-395">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="d3b92-396">Pokud cílení na rozhraní .NET Framework, do projektu přidejte balíček zprostředkovatele a vyvolání `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="d3b92-396">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d3b92-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d3b92-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="d3b92-398">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) přetížení vám umožní předat v [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) se kterým můžete přepsat výchozí nastavení, jako je například protokolování výstupu šablony, název objektu blob a souborů omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="d3b92-398">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="d3b92-399">(*Výstup šablony* je šablona zprávy, který se použije pro všechny protokoly nad ten, který je zadat při volání `ILogger` metoda.)</span><span class="sxs-lookup"><span data-stu-id="d3b92-399">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="d3b92-400">Když nasazujete do aplikace služby App Service, aplikace respektuje nastavení v [diagnostické protokoly](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) části **služby App Service** stránce portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3b92-400">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="d3b92-401">Když tato nastavení jsou aktualizovány, změny se projeví okamžitě bez nutnosti restartování nebo opětovné nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b92-401">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Nastavení protokolování Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="d3b92-403">Výchozí umístění pro soubory protokolu je v *D:\\domácí\\LogFiles\\aplikace* složku a výchozí název souboru je *diagnostiky yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-403">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="d3b92-404">Výchozí limit velikosti souboru je 10 MB a výchozí maximální počet souborů, které uchovávají se 2.</span><span class="sxs-lookup"><span data-stu-id="d3b92-404">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="d3b92-405">Výchozí název objektu blob je *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-405">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="d3b92-406">Další informace o výchozí chování najdete v tématu [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="d3b92-406">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="d3b92-407">Zprostředkovatel funguje pouze při spuštění projektu v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="d3b92-407">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="d3b92-408">Nemá žádný vliv, pokud projekt běží místně&mdash;není zapsat do místních souborů nebo vývoj pro místní úložiště pro objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="d3b92-408">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="d3b92-409">Zprostředkovatelé třetí strany protokolování</span><span class="sxs-lookup"><span data-stu-id="d3b92-409">Third-party logging providers</span></span>

<span data-ttu-id="d3b92-410">Rozhraní protokolování třetích stran, které pracují s ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d3b92-410">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="d3b92-411">[elmah.IO](https://elmah.io/) ([úložiště GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="d3b92-411">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="d3b92-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([úložiště GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="d3b92-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="d3b92-413">[JSNLog](http://jsnlog.com/) ([úložiště GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="d3b92-413">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="d3b92-414">[Loggr](http://loggr.net/) ([úložiště GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="d3b92-414">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="d3b92-415">[NLog](http://nlog-project.org/) ([úložiště GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="d3b92-415">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="d3b92-416">[Serilog](https://serilog.net/) ([úložiště GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="d3b92-416">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="d3b92-417">Můžete provést některé architektury třetích stran [sémantické protokolování, také známé jako strukturovaný protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="d3b92-417">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="d3b92-418">Použití rozhraní třetích stran je podobná pomocí jedné z předdefinované zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="d3b92-418">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="d3b92-419">Přidejte balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b92-419">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="d3b92-420">Volání metody rozšíření na `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-420">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="d3b92-421">Další informace najdete v dokumentaci k každý framework.</span><span class="sxs-lookup"><span data-stu-id="d3b92-421">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="d3b92-422">Streamování protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="d3b92-422">Azure log streaming</span></span>

<span data-ttu-id="d3b92-423">Vysílání datového proudu protokolů Azure umožňuje zobrazit aktivitu protokolu v reálném čase z:</span><span class="sxs-lookup"><span data-stu-id="d3b92-423">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="d3b92-424">Aplikační server</span><span class="sxs-lookup"><span data-stu-id="d3b92-424">The application server</span></span>
* <span data-ttu-id="d3b92-425">Webový server</span><span class="sxs-lookup"><span data-stu-id="d3b92-425">The web server</span></span>
* <span data-ttu-id="d3b92-426">Trasování neúspěšných žádostí</span><span class="sxs-lookup"><span data-stu-id="d3b92-426">Failed request tracing</span></span>

<span data-ttu-id="d3b92-427">Postup konfigurace streamování protokolů Azure:</span><span class="sxs-lookup"><span data-stu-id="d3b92-427">To configure Azure log streaming:</span></span>

* <span data-ttu-id="d3b92-428">Přejděte na **protokolů diagnostiky** stránky ze stránky portálu vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="d3b92-428">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="d3b92-429">Nastavit **protokolování aplikací (systém souborů)** na on.</span><span class="sxs-lookup"><span data-stu-id="d3b92-429">Set **Application Logging (Filesystem)** to on.</span></span>

![Stránka Azure portálu diagnostických protokolů](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="d3b92-431">Přejděte na **vysílání datového proudu protokolu** stránky zobrazení zpráv aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b92-431">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="d3b92-432">Jste přihlášení pomocí aplikace prostřednictvím `ILogger` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d3b92-432">They're logged by application through the `ILogger` interface.</span></span>

![Vysílání datového proudu protokolu aplikace portálu Azure](index/_static/azure-log-streaming.png)

## <a name="additional-resources"></a><span data-ttu-id="d3b92-434">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d3b92-434">Additional resources</span></span>

[<span data-ttu-id="d3b92-435">Vysoce výkonné protokolování s LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="d3b92-435">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
