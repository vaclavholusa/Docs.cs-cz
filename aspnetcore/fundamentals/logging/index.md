---
title: Protokolování v ASP.NET Core
author: ardalis
description: Další informace o protokolovacího rozhraní v ASP.NET Core. Objevte poskytovatelé vestavěné protokolování a další informace o Oblíbené zprostředkovatele třetí strany.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: dde01129bb7ea29544c4c416dfe9b5522a738d01
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938482"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="9d490-104">Protokolování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d490-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="9d490-105">Podle [Steve Smith](https://ardalis.com/) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9d490-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9d490-106">ASP.NET Core podporuje protokolování rozhraní API, která funguje s různých poskytovatelů protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="9d490-107">Předdefinované zprostředkovatele umožní odeslat protokoly do jednoho nebo více cílů a je možné připojit v rámci protokolování třetích stran.</span><span class="sxs-lookup"><span data-stu-id="9d490-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="9d490-108">Tento článek ukazuje, jak použít rozhraní API pro vestavěné protokolování a poskytovatelé ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="9d490-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d490-109">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9d490-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9d490-110">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9d490-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="9d490-111">Informace o protokolování stdout při hostování za nástrojem službou IIS najdete v tématu <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="9d490-111">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="9d490-112">Informace týkající se stdout protokolování pomocí služby Azure App Service najdete v tématu <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="9d490-112">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="9d490-113">Jak vytvořit protokoly</span><span class="sxs-lookup"><span data-stu-id="9d490-113">How to create logs</span></span>

<span data-ttu-id="9d490-114">Chcete-li vytvořit protokoly, implementovat [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) objektu z [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru:</span><span class="sxs-lookup"><span data-stu-id="9d490-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9d490-115">K tomuto objektu protokolovač zavoláním metody protokolování:</span><span class="sxs-lookup"><span data-stu-id="9d490-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9d490-116">Tento příklad vytvoří protokolů pomocí `TodoController` třídy jako *kategorie*.</span><span class="sxs-lookup"><span data-stu-id="9d490-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="9d490-117">Kategorie jsou vysvětleny [dále v tomto článku](#log-category).</span><span class="sxs-lookup"><span data-stu-id="9d490-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="9d490-118">ASP.NET Core neposkytuje asynchronní metody protokolování, protože protokolování by měl být tak rychle, že není vhodné náklady na použití modifikátoru async.</span><span class="sxs-lookup"><span data-stu-id="9d490-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="9d490-119">Pokud jste v situaci, kde, který není true, zvažte možnost změnit způsob, jakým jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9d490-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="9d490-120">Pokud vaše úložiště dat je pomalá, nejprve zápis zpráv protokolu do rychlého úložiště a pak přesuňte je pomalé úložiště později.</span><span class="sxs-lookup"><span data-stu-id="9d490-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="9d490-121">Protokolujte například, do fronty zpráv, která má číst a trvale uložena do úložiště pomalé jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="9d490-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="9d490-122">Přidání poskytovatele</span><span class="sxs-lookup"><span data-stu-id="9d490-122">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d490-123">Protokolování zprostředkovatele přijímá zprávy, které vytvoříte pomocí `ILogger` objekt zobrazí a uloží je.</span><span class="sxs-lookup"><span data-stu-id="9d490-123">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9d490-124">Například konzola poskytovatel zobrazí zprávy na konzole a zprostředkovatele služby Azure App Service můžete ukládat ve službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="9d490-124">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9d490-125">Chcete-li použít poskytovatele, zavolejte poskytovatele `Add<ProviderName>` metody rozšíření v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d490-125">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="9d490-126">Povolí protokolování pomocí výchozí šablony projektu [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metody:</span><span class="sxs-lookup"><span data-stu-id="9d490-126">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9d490-127">Protokolování zprostředkovatele přijímá zprávy, které vytvoříte pomocí `ILogger` objekt zobrazí a uloží je.</span><span class="sxs-lookup"><span data-stu-id="9d490-127">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9d490-128">Například konzola poskytovatel zobrazí zprávy na konzole a zprostředkovatele služby Azure App Service můžete ukládat ve službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="9d490-128">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9d490-129">Používat poskytovatele, nainstalujte svůj balíček NuGet a volání metody rozšíření zprostředkovatele na instanci [implementaci třídy ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9d490-129">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="9d490-130">ASP.NET Core [injektáž závislostí](xref:fundamentals/dependency-injection) (DI) poskytuje `ILoggerFactory` instance.</span><span class="sxs-lookup"><span data-stu-id="9d490-130">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="9d490-131">`AddConsole` a `AddDebug` metody rozšíření jsou definovány v [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) a [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) balíčky.</span><span class="sxs-lookup"><span data-stu-id="9d490-131">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="9d490-132">Každá metoda rozšíření volá `ILoggerFactory.AddProvider` metodu instance zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="9d490-132">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="9d490-133">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) přidá poskytovatele protokolování v `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="9d490-133">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="9d490-134">Pokud chcete získat výstup protokolu z kódu, který se spustí dříve, přidejte zprostředkovatele protokolování v `Startup` konstruktoru třídy.</span><span class="sxs-lookup"><span data-stu-id="9d490-134">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="9d490-135">Zjistíte informace o jednotlivých [vestavěné protokolování zprostředkovatele](#built-in-logging-providers) a obsahuje odkazy na [zprostředkovatele přihlášení třetí strany](#third-party-logging-providers) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="9d490-135">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="9d490-136">Konfigurace nastavení souboru</span><span class="sxs-lookup"><span data-stu-id="9d490-136">Settings file configuration</span></span>

<span data-ttu-id="9d490-137">Každý z předchozích příkladů v [Přidání poskytovatelů](#how-to-add-providers) načte zprostředkovatele konfigurace protokolování z část `Logging` části souborů s nastavením aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d490-137">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="9d490-138">Následující příklad ukazuje obsah typické *appsettings. Development.JSON* souboru:</span><span class="sxs-lookup"><span data-stu-id="9d490-138">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="9d490-139">`LogLevel` klíče představují názvy protokolů.</span><span class="sxs-lookup"><span data-stu-id="9d490-139">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="9d490-140">`Default` Klíč platí do protokolů, které nejsou výslovně uvedena.</span><span class="sxs-lookup"><span data-stu-id="9d490-140">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="9d490-141">Hodnota představuje [úrovně protokolování](#log-level) použitý pro daný protokol.</span><span class="sxs-lookup"><span data-stu-id="9d490-141">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="9d490-142">Protokol klíče této sady `IncludeScopes` (`Console` v příkladu), zadejte, pokud [protokolu obory](#log-scopes) jsou povolené pro zadaný protokol.</span><span class="sxs-lookup"><span data-stu-id="9d490-142">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="9d490-143">`LogLevel` klíče představují názvy protokolů.</span><span class="sxs-lookup"><span data-stu-id="9d490-143">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="9d490-144">`Default` Klíč platí do protokolů, které nejsou výslovně uvedena.</span><span class="sxs-lookup"><span data-stu-id="9d490-144">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="9d490-145">Hodnota představuje [úrovně protokolování](#log-level) použitý pro daný protokol.</span><span class="sxs-lookup"><span data-stu-id="9d490-145">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="9d490-146">Ukázkový výstup protokolování</span><span class="sxs-lookup"><span data-stu-id="9d490-146">Sample logging output</span></span>

<span data-ttu-id="9d490-147">Ukázkový kód je znázorněno v předchozí části zobrazí protokoly konzoly při spuštění z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9d490-147">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="9d490-148">Tady je příklad výstupu konzoly:</span><span class="sxs-lookup"><span data-stu-id="9d490-148">Here's an example of console output:</span></span>

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

<span data-ttu-id="9d490-149">Tyto protokoly, které byly vytvořeny tak, že přejdete do `http://localhost:5000/api/todo/0`, která aktivuje spuštění obou `ILogger` volání je znázorněno v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="9d490-149">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="9d490-150">Tady je příklad stejného protokolů, jak se objeví v okně ladění, když spustíte ukázkovou aplikaci v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9d490-150">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="9d490-151">Protokoly, které byly vytvořeny `ILogger` volání je znázorněno v předchozí části začínají řetězcem "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="9d490-151">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="9d490-152">Protokoly, které začínají řetězcem "Microsoft". kategorie jsou z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d490-152">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="9d490-153">ASP.NET Core, samotného a kódu aplikace používají stejné protokolování rozhraní API a stejní poskytovatelé protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-153">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="9d490-154">Zbývající část tohoto článku popisuje některé podrobnosti a možností pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-154">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="9d490-155">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="9d490-155">NuGet packages</span></span>

<span data-ttu-id="9d490-156">`ILogger` a `ILoggerFactory` rozhraní jsou v [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a mají výchozí implementace pro ně [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="9d490-156">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="9d490-157">Kategorie protokolu</span><span class="sxs-lookup"><span data-stu-id="9d490-157">Log category</span></span>

<span data-ttu-id="9d490-158">A *kategorie* je součástí každý protokol, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="9d490-158">A *category* is included with each log that you create.</span></span> <span data-ttu-id="9d490-159">Zadejte kategorii při vytváření `ILogger` objektu.</span><span class="sxs-lookup"><span data-stu-id="9d490-159">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="9d490-160">Kategorie může být libovolný řetězec, ale konvence, je použít plně kvalifikovaný název třídy, ze kterého se zapisují protokoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-160">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="9d490-161">Příklad: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="9d490-161">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="9d490-162">Můžete určit kategorii jako řetězec nebo používat metodu rozšíření, který se odvozuje od typu kategorie.</span><span class="sxs-lookup"><span data-stu-id="9d490-162">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="9d490-163">Chcete-li zadat kategorii, kterou jako řetězec, zavolejte `CreateLogger` na `ILoggerFactory` instance, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="9d490-163">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="9d490-164">Ve většině případů, ji bude snazší používat `ILogger<T>`, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="9d490-164">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9d490-165">To je ekvivalentní volání `CreateLogger` názvem plně kvalifikovaný typ `T`.</span><span class="sxs-lookup"><span data-stu-id="9d490-165">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="9d490-166">Úroveň protokolování</span><span class="sxs-lookup"><span data-stu-id="9d490-166">Log level</span></span>

<span data-ttu-id="9d490-167">Při každém zápisu do protokolu, zadejte jeho [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="9d490-167">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="9d490-168">Určuje úroveň protokolu stupeň závažnosti nebo důležitost.</span><span class="sxs-lookup"><span data-stu-id="9d490-168">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="9d490-169">Například můžete například napsat `Information` protokolu, pokud metoda skončí za normálních okolností `Warning` po návratu metody vrátit kód 404 a protokolu `Error` protokolování při zachycení neočekávanou výjimku.</span><span class="sxs-lookup"><span data-stu-id="9d490-169">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="9d490-170">V následujícím příkladu kódu, názvy metod (například `LogWarning`) zadejte úroveň protokolu.</span><span class="sxs-lookup"><span data-stu-id="9d490-170">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="9d490-171">První parametr je [protokolu událost s ID](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="9d490-171">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="9d490-172">Je druhý parametr [zpráv šablonu](#log-message-template) se zástupnými symboly pro argument hodnoty podle zbývající parametry metody.</span><span class="sxs-lookup"><span data-stu-id="9d490-172">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="9d490-173">Parametry metody jsou vysvětlené podrobněji dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="9d490-173">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9d490-174">Metody protokolu, které obsahují úroveň z názvu metody jsou [rozšiřující metody pro ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="9d490-174">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="9d490-175">Na pozadí, tyto metody volat `Log` metodu, která přebírá `LogLevel` parametru.</span><span class="sxs-lookup"><span data-stu-id="9d490-175">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="9d490-176">Můžete volat `Log` metoda přímo spíše než jeden z těchto metod rozšíření, ale syntaxe je poměrně složitý.</span><span class="sxs-lookup"><span data-stu-id="9d490-176">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="9d490-177">Další informace najdete v tématu [ILogger rozhraní](/dotnet/api/microsoft.extensions.logging.ilogger) a [rozšíření protokolovače zdrojový kód](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="9d490-177">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="9d490-178">ASP.NET Core definuje následující [protokolu úrovně](/dotnet/api/microsoft.extensions.logging.loglevel), zde seřazené od nejnižší na nejvyšší závažnost.</span><span class="sxs-lookup"><span data-stu-id="9d490-178">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="9d490-179">Trasování = 0</span><span class="sxs-lookup"><span data-stu-id="9d490-179">Trace = 0</span></span>

  <span data-ttu-id="9d490-180">Informace, které je vhodné pouze pro vývojáře ladění chyby.</span><span class="sxs-lookup"><span data-stu-id="9d490-180">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="9d490-181">Tyto zprávy mohou obsahovat citlivé aplikaci data a by nemělo být povoleno v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9d490-181">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="9d490-182">*Ve výchozím nastavení zakázané.*</span><span class="sxs-lookup"><span data-stu-id="9d490-182">*Disabled by default.*</span></span> <span data-ttu-id="9d490-183">Příklad: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="9d490-183">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="9d490-184">Ladění = 1</span><span class="sxs-lookup"><span data-stu-id="9d490-184">Debug = 1</span></span>

  <span data-ttu-id="9d490-185">Informace, která má krátkodobou užitečnost při vývoji a ladění.</span><span class="sxs-lookup"><span data-stu-id="9d490-185">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="9d490-186">Příklad: `Entering method Configure with flag set to true.` obvykle neumožňuje `Debug` úroveň zaznamená v produkčním prostředí, pokud řešíte, kvůli velkému počtu protokoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-186">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="9d490-187">Informace o = 2</span><span class="sxs-lookup"><span data-stu-id="9d490-187">Information = 2</span></span>

  <span data-ttu-id="9d490-188">Pro sledování obecný tok z aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d490-188">For tracking the general flow of the application.</span></span> <span data-ttu-id="9d490-189">Tyto protokoly mají obvykle dlouhodobé některá z hodnot.</span><span class="sxs-lookup"><span data-stu-id="9d490-189">These logs typically have some long-term value.</span></span> <span data-ttu-id="9d490-190">Příklad: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="9d490-190">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="9d490-191">Upozornění = 3</span><span class="sxs-lookup"><span data-stu-id="9d490-191">Warning = 3</span></span>

  <span data-ttu-id="9d490-192">Neobvyklé nebo neočekávané události v aplikaci flow.</span><span class="sxs-lookup"><span data-stu-id="9d490-192">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="9d490-193">Ty mohou obsahovat chyby nebo jinými podmínkami, který nezpůsobí zastavení aplikace, ale který možná bude nutné se měl prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="9d490-193">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="9d490-194">Zpracované výjimky jsou běžné místo, kde můžete použít `Warning` úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-194">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="9d490-195">Příklad: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="9d490-195">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="9d490-196">Chyba = 4</span><span class="sxs-lookup"><span data-stu-id="9d490-196">Error = 4</span></span>

  <span data-ttu-id="9d490-197">Chyby a výjimky, které nelze zpracovat.</span><span class="sxs-lookup"><span data-stu-id="9d490-197">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="9d490-198">Tyto zprávy označují selhání aktuální aktivitu nebo operace (jako je například aktuální požadavek HTTP), ne chybu celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d490-198">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="9d490-199">Příklad zprávy protokolu: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="9d490-199">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="9d490-200">Kritická = 5</span><span class="sxs-lookup"><span data-stu-id="9d490-200">Critical = 5</span></span>

  <span data-ttu-id="9d490-201">Chyby, které vyžadují okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="9d490-201">For failures that require immediate attention.</span></span> <span data-ttu-id="9d490-202">Příklady: scénářům ztráty dat, volné místo na disku.</span><span class="sxs-lookup"><span data-stu-id="9d490-202">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="9d490-203">Úroveň protokolu můžete určit, kolik výstup protokolu je zapsán do konkrétního úložiště média nebo zobrazení okna.</span><span class="sxs-lookup"><span data-stu-id="9d490-203">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="9d490-204">Například v produkčním prostředí může být vhodné všechny protokoly za `Information` úroveň a nižší přejdete na svazek úložiště dat a všechny protokoly za `Warning` úrovně a přejděte do úložiště dat hodnotu vyšší.</span><span class="sxs-lookup"><span data-stu-id="9d490-204">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="9d490-205">Během vývoje, můžete obvykle odesílat protokoly `Warning` nebo vyšší závažnost do konzoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-205">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="9d490-206">Pokud potřebujete vyřešit, můžete přidat `Debug` úroveň.</span><span class="sxs-lookup"><span data-stu-id="9d490-206">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="9d490-207">[Filtrování protokolu](#log-filtering) části dále v tomto článku vysvětluje, jak řídit které úrovně protokolu zpracovává zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="9d490-207">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="9d490-208">Zapíše rozhraní ASP.NET Core `Debug` úroveň protokoly pro události rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="9d490-208">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="9d490-209">Příklady protokolu dříve v tomto článku vyloučené protokoly níže `Information` úroveň, proto není `Debug` zobrazila úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-209">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="9d490-210">Tady je příklad protokoly konzoly Pokud spustíte ukázkovou aplikaci nakonfigurovat tak, aby zobrazit `Debug` a vyšší protokoly pro zprostředkovatele konzoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-210">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="9d490-211">ID události protokolu</span><span class="sxs-lookup"><span data-stu-id="9d490-211">Log event ID</span></span>

<span data-ttu-id="9d490-212">Při každém zápisu do protokolu, můžete zadat *ID události*.</span><span class="sxs-lookup"><span data-stu-id="9d490-212">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="9d490-213">Ukázková aplikace dělá to pomocí místně definované `LoggingEvents` třídy:</span><span class="sxs-lookup"><span data-stu-id="9d490-213">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="9d490-214">ID události je celočíselná hodnota, která vám umožní přidružit sadu protokolovaných událostí mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="9d490-214">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="9d490-215">Například protokolu pro přidání položky do nákupního košíku může být událost ID 1000 a protokolu pro dokončení nákupu může být událost ID 1001.</span><span class="sxs-lookup"><span data-stu-id="9d490-215">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="9d490-216">Ve výstupu protokolování může uloženy v poli ID události nebo součástí textové zprávy, v závislosti na poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="9d490-216">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="9d490-217">Ladění zprostředkovatele nezobrazí ID událostí, ale konzola poskytovatel je zobrazuje v závorkách za kategorií:</span><span class="sxs-lookup"><span data-stu-id="9d490-217">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="9d490-218">Šablona zprávy protokolu</span><span class="sxs-lookup"><span data-stu-id="9d490-218">Log message template</span></span>

<span data-ttu-id="9d490-219">Pokaždé, když zápisu zprávy do protokolu zadejte šablonu zprávy.</span><span class="sxs-lookup"><span data-stu-id="9d490-219">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="9d490-220">Šablona zprávy může být řetězec, nebo může obsahovat pojmenované zástupné symboly, do které argument hodnoty budou vloženy.</span><span class="sxs-lookup"><span data-stu-id="9d490-220">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="9d490-221">Šablona není řetězec formátu a zástupné symboly by měly být pojmenovány, ne číslované.</span><span class="sxs-lookup"><span data-stu-id="9d490-221">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9d490-222">Pořadí zástupné symboly, nikoli jejich názvy, určuje, jaké parametry se používají k zadání jejich hodnot.</span><span class="sxs-lookup"><span data-stu-id="9d490-222">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="9d490-223">Pokud máte následující kód:</span><span class="sxs-lookup"><span data-stu-id="9d490-223">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="9d490-224">Výslednou zprávu protokolu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9d490-224">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="9d490-225">Rozhraní protokolování zpráv formátování tímto způsobem, aby bylo možné pro protokolování poskytovatele, jak implementovat [sémantického protokolování, označovaného také jako strukturované protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9d490-225">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="9d490-226">Protože samotné argumenty jsou předány do systému protokolování, ne jenom šablony formátovaná zpráva poskytovatelé protokolování můžete ukládat hodnoty parametrů jako pole kromě šablonu zprávy.</span><span class="sxs-lookup"><span data-stu-id="9d490-226">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="9d490-227">Pokud jste směrování protokolu výstup do služby Azure Table Storage a volání metody protokolovacího nástroje vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9d490-227">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="9d490-228">Každá entita Azure Table může mít `ID` a `RequestTime` vlastnosti, které zjednodušuje dotazy na data protokolu.</span><span class="sxs-lookup"><span data-stu-id="9d490-228">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="9d490-229">Můžete najít všechny protokoly v rámci konkrétní `RequestTime` rozsah, aniž by bylo nutné analyzovat časový limit textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="9d490-229">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="9d490-230">Protokolování výjimek</span><span class="sxs-lookup"><span data-stu-id="9d490-230">Logging exceptions</span></span>

<span data-ttu-id="9d490-231">Protokolovací nástroj metody mají přetížení, které umožňují předat výjimku, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9d490-231">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="9d490-232">Různí poskytovatelé zpracovat informace o výjimce různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="9d490-232">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="9d490-233">Tady je příklad výstupu ladění zprostředkovatele z výše uvedeném kódu.</span><span class="sxs-lookup"><span data-stu-id="9d490-233">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="9d490-234">Filtrování protokolu</span><span class="sxs-lookup"><span data-stu-id="9d490-234">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d490-235">Můžete zadat minimální úroveň protokolování pro konkrétního zprostředkovatele a kategorie nebo pro všechny poskytovatele nebo všechny kategorie.</span><span class="sxs-lookup"><span data-stu-id="9d490-235">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="9d490-236">Všechny protokoly nižší než minimální úroveň nejsou předán tohoto poskytovatele, tak nemusíte získat zobrazení nebo uložené.</span><span class="sxs-lookup"><span data-stu-id="9d490-236">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="9d490-237">Pokud budete chtít potlačit všechny protokoly, můžete zadat `LogLevel.None` jako minimální úroveň protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-237">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="9d490-238">Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9d490-238">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9d490-239">**Vytvoření pravidla filtru v konfiguraci**</span><span class="sxs-lookup"><span data-stu-id="9d490-239">**Create filter rules in configuration**</span></span>

<span data-ttu-id="9d490-240">Šablony projektu vytvořit kód, který volá `CreateDefaultBuilder` nastavit protokolování pro poskytovatele konzoly a ladění.</span><span class="sxs-lookup"><span data-stu-id="9d490-240">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="9d490-241">`CreateDefaultBuilder` Metoda také nastaví protokolování hledat konfiguraci `Logging` oddílu, psát kód podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="9d490-241">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="9d490-242">Konfigurační data Určuje minimální úrovně podle poskytovatele a kategorie, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9d490-242">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="9d490-243">Tento dokument JSON vytvoří šest pravidla filtru, jeden pro poskytovatele ladění, čtyři pro zprostředkovatele konzoly a ten, který se vztahuje na všechny poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="9d490-243">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="9d490-244">Uvidíte jak později jedním z těchto pravidel je vybrán pro každého zprostředkovatele při `ILogger` je vytvořen objekt.</span><span class="sxs-lookup"><span data-stu-id="9d490-244">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="9d490-245">**Pravidla filtru v kódu**</span><span class="sxs-lookup"><span data-stu-id="9d490-245">**Filter rules in code**</span></span>

<span data-ttu-id="9d490-246">V kódu, můžete zaregistrovat pravidla filtru, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9d490-246">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="9d490-247">Druhá `AddFilter` Určuje zprostředkovatele, který ladění pomocí názvu typu.</span><span class="sxs-lookup"><span data-stu-id="9d490-247">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="9d490-248">První `AddFilter` platí pro všechny poskytovatele, protože ji neurčuje, typ zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="9d490-248">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="9d490-249">**Jak pravidla filtrování se použijí**</span><span class="sxs-lookup"><span data-stu-id="9d490-249">**How filtering rules are applied**</span></span>

<span data-ttu-id="9d490-250">Nastavte `AddFilter`protokolování aplikace (systém souborů) zapnete.</span><span class="sxs-lookup"><span data-stu-id="9d490-250">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="9d490-251">Stránka portálu diagnostické protokoly Azure</span><span class="sxs-lookup"><span data-stu-id="9d490-251">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="9d490-252">Číslo</span><span class="sxs-lookup"><span data-stu-id="9d490-252">Number</span></span> | <span data-ttu-id="9d490-253">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="9d490-253">Provider</span></span>      | <span data-ttu-id="9d490-254">Přejděte streamování protokolů stránku, abyste zobrazili zprávy aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d490-254">Categories that begin with ...</span></span>          | <span data-ttu-id="9d490-255">Jsou aplikace prostřednictvím přihlášení  rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9d490-255">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="9d490-256">1</span><span class="sxs-lookup"><span data-stu-id="9d490-256">1</span></span>      | <span data-ttu-id="9d490-257">Ladit</span><span class="sxs-lookup"><span data-stu-id="9d490-257">Debug</span></span>         | <span data-ttu-id="9d490-258">Streamování protokolů Azure aplikace na portálu.</span><span class="sxs-lookup"><span data-stu-id="9d490-258">All categories</span></span>                          | <span data-ttu-id="9d490-259">Informace o</span><span class="sxs-lookup"><span data-stu-id="9d490-259">Information</span></span>       |
| <span data-ttu-id="9d490-260">2</span><span class="sxs-lookup"><span data-stu-id="9d490-260">2</span></span>      | <span data-ttu-id="9d490-261">Konzola</span><span class="sxs-lookup"><span data-stu-id="9d490-261">Console</span></span>       | <span data-ttu-id="9d490-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="9d490-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="9d490-263">Upozornění</span><span class="sxs-lookup"><span data-stu-id="9d490-263">Warning</span></span>           |
| <span data-ttu-id="9d490-264">3</span><span class="sxs-lookup"><span data-stu-id="9d490-264">3</span></span>      | <span data-ttu-id="9d490-265">Konzola</span><span class="sxs-lookup"><span data-stu-id="9d490-265">Console</span></span>       | <span data-ttu-id="9d490-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="9d490-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="9d490-267">Ladit</span><span class="sxs-lookup"><span data-stu-id="9d490-267">Debug</span></span>             |
| <span data-ttu-id="9d490-268">4</span><span class="sxs-lookup"><span data-stu-id="9d490-268">4</span></span>      | <span data-ttu-id="9d490-269">Konzola</span><span class="sxs-lookup"><span data-stu-id="9d490-269">Console</span></span>       | <span data-ttu-id="9d490-270">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="9d490-270">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="9d490-271">Chyba</span><span class="sxs-lookup"><span data-stu-id="9d490-271">Error</span></span>             |
| <span data-ttu-id="9d490-272">5</span><span class="sxs-lookup"><span data-stu-id="9d490-272">5</span></span>      | <span data-ttu-id="9d490-273">Konzola</span><span class="sxs-lookup"><span data-stu-id="9d490-273">Console</span></span>       | <span data-ttu-id="9d490-274">Streamování protokolů Azure aplikace na portálu.</span><span class="sxs-lookup"><span data-stu-id="9d490-274">All categories</span></span>                          | <span data-ttu-id="9d490-275">Informace o</span><span class="sxs-lookup"><span data-stu-id="9d490-275">Information</span></span>       |
| <span data-ttu-id="9d490-276">6</span><span class="sxs-lookup"><span data-stu-id="9d490-276">6</span></span>      | <span data-ttu-id="9d490-277">Protokolování trasování programu Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="9d490-277">All providers</span></span> | <span data-ttu-id="9d490-278">Streamování protokolů Azure aplikace na portálu.</span><span class="sxs-lookup"><span data-stu-id="9d490-278">All categories</span></span>                          | <span data-ttu-id="9d490-279">Ladit</span><span class="sxs-lookup"><span data-stu-id="9d490-279">Debug</span></span>             |
| <span data-ttu-id="9d490-280">7</span><span class="sxs-lookup"><span data-stu-id="9d490-280">7</span></span>      | <span data-ttu-id="9d490-281">Protokolování trasování programu Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="9d490-281">All providers</span></span> | <span data-ttu-id="9d490-282">Systém</span><span class="sxs-lookup"><span data-stu-id="9d490-282">System</span></span>                                  | <span data-ttu-id="9d490-283">Ladit</span><span class="sxs-lookup"><span data-stu-id="9d490-283">Debug</span></span>             |
| <span data-ttu-id="9d490-284">8</span><span class="sxs-lookup"><span data-stu-id="9d490-284">8</span></span>      | <span data-ttu-id="9d490-285">Ladit</span><span class="sxs-lookup"><span data-stu-id="9d490-285">Debug</span></span>         | <span data-ttu-id="9d490-286">Microsoft</span><span class="sxs-lookup"><span data-stu-id="9d490-286">Microsoft</span></span>                               | <span data-ttu-id="9d490-287">Trasování</span><span class="sxs-lookup"><span data-stu-id="9d490-287">Trace</span></span>             |

<span data-ttu-id="9d490-288">`ILogger`Application Insights`ILoggerFactory` SDK je schopen shromažďování trasování telemetrických dat z protokolů generovaných prostřednictvím protokolování infrastruktury ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d490-288">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="9d490-289">Další informace najdete v tématu `ILogger`Microsoft/ApplicationInsights-aspnetcore Wiki: protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-289">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="9d490-290">Vysoce výkonné protokolování pomocí LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="9d490-290">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="9d490-291">Následující požadovaný algoritmus se používá pro každého zprostředkovatele při `ILogger` se vytvoří pro danou kategorii:</span><span class="sxs-lookup"><span data-stu-id="9d490-291">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="9d490-292">Vyberte všechna pravidla, které odpovídají zprostředkovateli nebo její alias.</span><span class="sxs-lookup"><span data-stu-id="9d490-292">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="9d490-293">Pokud nejsou nalezeny, vyberte všechna pravidla s poskytovatelem služby prázdný.</span><span class="sxs-lookup"><span data-stu-id="9d490-293">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="9d490-294">Z výsledků v předchozím kroku vyberte pravidla s nejdelší odpovídající předpona kategorie.</span><span class="sxs-lookup"><span data-stu-id="9d490-294">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="9d490-295">Pokud nejsou nalezeny, vyberte všechna pravidla, které nechcete zadat kategorii.</span><span class="sxs-lookup"><span data-stu-id="9d490-295">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="9d490-296">Pokud je vybraných víc pravidel trvat **poslední** jeden.</span><span class="sxs-lookup"><span data-stu-id="9d490-296">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="9d490-297">Pokud jsou vybraná žádná pravidla, použijte `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="9d490-297">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="9d490-298">Předpokládejme například, že máte předchozí seznam pravidel a můžete vytvořit `ILogger` objektu pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="9d490-298">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="9d490-299">Ladění zprostředkovatele platí pravidla 1, 6 a 8.</span><span class="sxs-lookup"><span data-stu-id="9d490-299">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="9d490-300">Pravidlo 8 není nejvíce specifické, tak, aby se byla vybrána.</span><span class="sxs-lookup"><span data-stu-id="9d490-300">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="9d490-301">Pro poskytovatele konzoly platí pravidla 3, 4, 5 a 6.</span><span class="sxs-lookup"><span data-stu-id="9d490-301">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="9d490-302">Pravidlo 3 není nejvíce specifické.</span><span class="sxs-lookup"><span data-stu-id="9d490-302">Rule 3 is most specific.</span></span>

<span data-ttu-id="9d490-303">Při vytváření protokolů pomocí `ILogger` pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" protokoly z `Trace` úrovně a vyšší budou moct ladění zprostředkovatele a protokoly `Debug` úrovně a vyšší přejde k poskytovateli konzoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-303">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="9d490-304">**Aliasy poskytovatele**</span><span class="sxs-lookup"><span data-stu-id="9d490-304">**Provider aliases**</span></span>

<span data-ttu-id="9d490-305">Název typu můžete použít k zadání zprostředkovatele v konfiguraci, ale každý poskytovatel definuje v kratší *alias* , která se snadněji používá.</span><span class="sxs-lookup"><span data-stu-id="9d490-305">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="9d490-306">Pro předdefinované zprostředkovatele použijte následující aliasy:</span><span class="sxs-lookup"><span data-stu-id="9d490-306">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="9d490-307">Konzola</span><span class="sxs-lookup"><span data-stu-id="9d490-307">Console</span></span>
- <span data-ttu-id="9d490-308">Ladit</span><span class="sxs-lookup"><span data-stu-id="9d490-308">Debug</span></span>
- <span data-ttu-id="9d490-309">Protokol událostí</span><span class="sxs-lookup"><span data-stu-id="9d490-309">EventLog</span></span>
- <span data-ttu-id="9d490-310">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="9d490-310">AzureAppServices</span></span>
- <span data-ttu-id="9d490-311">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9d490-311">TraceSource</span></span>
- <span data-ttu-id="9d490-312">EventSource</span><span class="sxs-lookup"><span data-stu-id="9d490-312">EventSource</span></span>

<span data-ttu-id="9d490-313">**Výchozí minimální úroveň**</span><span class="sxs-lookup"><span data-stu-id="9d490-313">**Default minimum level**</span></span>

<span data-ttu-id="9d490-314">Je k dispozici minimální úroveň nastavení, která se projeví pouze v případě, že žádná pravidla z konfigurace nebo kódu platí pro daného poskytovatele a kategorii.</span><span class="sxs-lookup"><span data-stu-id="9d490-314">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="9d490-315">Následující příklad ukazuje, jak nastavit minimální úroveň:</span><span class="sxs-lookup"><span data-stu-id="9d490-315">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="9d490-316">Pokud není explicitně nastavena na minimální úroveň, výchozí hodnota je `Information`, což znamená, že `Trace` a `Debug` protokoly jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="9d490-316">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="9d490-317">**Funkce filtru**</span><span class="sxs-lookup"><span data-stu-id="9d490-317">**Filter functions**</span></span>

<span data-ttu-id="9d490-318">Můžete napsat kód ve funkci filtru použít pravidla filtrování.</span><span class="sxs-lookup"><span data-stu-id="9d490-318">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="9d490-319">Pro všechny poskytovatele a kategorie, které nemají přiřazené konfigurace nebo kódu pravidla je vyvolána funkce filtru.</span><span class="sxs-lookup"><span data-stu-id="9d490-319">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="9d490-320">Kód ve funkci má přístup k typ zprostředkovatele, kategorii a úroveň protokolu se rozhodnout, zda mají být protokolovány zprávu.</span><span class="sxs-lookup"><span data-stu-id="9d490-320">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="9d490-321">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9d490-321">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9d490-322">Někteří poskytovatelé protokolování umožňují zadat při protokoly by měly být zapsána do úložiště média nebo ignorovat podle úroveň protokolu a kategorie.</span><span class="sxs-lookup"><span data-stu-id="9d490-322">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="9d490-323">`AddConsole` a `AddDebug` rozšiřující metody poskytují přetížení, které umožňují předat kritéria filtrování.</span><span class="sxs-lookup"><span data-stu-id="9d490-323">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="9d490-324">Následující ukázkový kód způsobí, že konzola poskytovatel ignorovat protokolech níže `Warning` úroveň, při ladění zprostředkovatele ignoruje protokoly, které vytvoří rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="9d490-324">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="9d490-325">`AddEventLog` Metoda má přetížení přijímající `EventLogSettings` instanci, která může obsahovat funkce filtrování v jeho `Filter` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9d490-325">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="9d490-326">Zprostředkovatel TraceSource neposkytuje, některý z těchto přetížení, protože jeho úroveň protokolování a další parametry jsou založené na `SourceSwitch` a `TraceListener` používá.</span><span class="sxs-lookup"><span data-stu-id="9d490-326">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="9d490-327">Můžete nastavit pravidla filtrování pro všech zprostředkovatelů, které jsou registrovány `ILoggerFactory` instance pomocí `WithFilter` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="9d490-327">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="9d490-328">Následující příklad omezuje framework protokoly (kategorie začíná řetězcem "Microsoft" nebo "Systém") a upozornění přičemž umožníte aplikaci protokolu na úroveň ladění.</span><span class="sxs-lookup"><span data-stu-id="9d490-328">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="9d490-329">Pokud chcete zabránit zápisu pro danou kategorii všechny protokoly pomocí filtrování, můžete zadat `LogLevel.None` jako minimální úroveň protokolování pro dané kategorie.</span><span class="sxs-lookup"><span data-stu-id="9d490-329">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="9d490-330">Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9d490-330">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9d490-331">`WithFilter` – Metoda rozšíření poskytuje [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="9d490-331">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="9d490-332">Metoda vrátí nový `ILoggerFactory` zaregistrovaný poskytovatel instanci, která bude filtrovat zprávy protokolu, který je předán všichni poskytovatelé protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="9d490-332">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="9d490-333">To nemá vliv na jiné `ILoggerFactory` instance, včetně původní `ILoggerFactory` instance.</span><span class="sxs-lookup"><span data-stu-id="9d490-333">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="9d490-334">Protokol obory</span><span class="sxs-lookup"><span data-stu-id="9d490-334">Log scopes</span></span>

<span data-ttu-id="9d490-335">Můžete seskupit sadu logické operace v rámci *oboru* aby bylo možné připojit stejná data pro každý protokol, který je vytvořen jako součást této sady.</span><span class="sxs-lookup"><span data-stu-id="9d490-335">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="9d490-336">Například může být vhodné každý protokol vytvořených jako součást zpracování transakce zahrnují ID transakce.</span><span class="sxs-lookup"><span data-stu-id="9d490-336">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="9d490-337">Obor je `IDisposable` typ, který je vrácen [ILogger.BeginScope&lt;TState&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) metoda a bude trvat, dokud je uvolněn.</span><span class="sxs-lookup"><span data-stu-id="9d490-337">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="9d490-338">Použijete zabalení vaší protokolovací nástroj se volá v oboru `using` blokovat, jak je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="9d490-338">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="9d490-339">Následující kód umožní obory pro zprostředkovatele konzoly:</span><span class="sxs-lookup"><span data-stu-id="9d490-339">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="9d490-340">*Soubor program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d490-340">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="9d490-341">Konfigurace `IncludeScopes` možnost protokolovací nástroj konzoly je nutné povolit protokolování na základě oboru.</span><span class="sxs-lookup"><span data-stu-id="9d490-341">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="9d490-342">`IncludeScopes` můžete nakonfigurovat přes *appsettings* konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="9d490-342">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="9d490-343">Další informace najdete v tématu [konfigurační soubor nastavení](#settings-file-configuration) oddílu.</span><span class="sxs-lookup"><span data-stu-id="9d490-343">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9d490-344">*Soubor program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d490-344">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="9d490-345">Konfigurace `IncludeScopes` možnost protokolovací nástroj konzoly je nutné povolit protokolování na základě oboru.</span><span class="sxs-lookup"><span data-stu-id="9d490-345">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9d490-346">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d490-346">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="9d490-347">Každé zprávě protokolu obsahuje rozsahem informací:</span><span class="sxs-lookup"><span data-stu-id="9d490-347">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="9d490-348">Poskytovatelé vestavěné protokolování</span><span class="sxs-lookup"><span data-stu-id="9d490-348">Built-in logging providers</span></span>

<span data-ttu-id="9d490-349">ASP.NET Core se celá dodává následující zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="9d490-349">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="9d490-350">Console</span><span class="sxs-lookup"><span data-stu-id="9d490-350">Console</span></span>](#console-provider)
* [<span data-ttu-id="9d490-351">Ladění</span><span class="sxs-lookup"><span data-stu-id="9d490-351">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="9d490-352">EventSource</span><span class="sxs-lookup"><span data-stu-id="9d490-352">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="9d490-353">EventLog</span><span class="sxs-lookup"><span data-stu-id="9d490-353">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="9d490-354">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9d490-354">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="9d490-355">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9d490-355">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="9d490-356">Konzola zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9d490-356">Console provider</span></span>

<span data-ttu-id="9d490-357">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) balíček zprostředkovatele odešle výstup protokolu konzoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-357">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="9d490-358">[Přetížení AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) umožňují předáním minimální úroveň protokolování, funkce filtru a logickou hodnotu, která označuje, zda jsou obory.</span><span class="sxs-lookup"><span data-stu-id="9d490-358">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="9d490-359">Další možností je a zajistěte tak předání `IConfiguration` objektu, který můžete určit podporu obory a úrovní protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-359">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="9d490-360">Pokud uvažujete o poskytovateli konzoly pro použití v produkčním prostředí, mějte na paměti, že má významný dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="9d490-360">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="9d490-361">Když vytvoříte nový projekt v sadě Visual Studio `AddConsole` metoda vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9d490-361">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="9d490-362">Tento kód odkazuje `Logging` část *appSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="9d490-362">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="9d490-363">Nastavení zobrazené protokoly framework limit upozornění zároveň umožní aplikaci k přihlášení na úrovni ladění, jak je vysvětleno v [filtrování protokolu](#log-filtering) oddílu.</span><span class="sxs-lookup"><span data-stu-id="9d490-363">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="9d490-364">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9d490-364">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="9d490-365">Ladění zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9d490-365">Debug provider</span></span>

<span data-ttu-id="9d490-366">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) balíček zprostředkovatele zapíše výstup protokolu pomocí [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) třídy (`Debug.WriteLine` volání metody).</span><span class="sxs-lookup"><span data-stu-id="9d490-366">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="9d490-367">V systému Linux, tento zprostředkovatel zapisuje protokoly do */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="9d490-367">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="9d490-368">[Přetížení AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) umožňují předáním minimální úroveň protokolování nebo funkce filtru.</span><span class="sxs-lookup"><span data-stu-id="9d490-368">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="9d490-369">Zprostředkovatel EventSource</span><span class="sxs-lookup"><span data-stu-id="9d490-369">EventSource provider</span></span>

<span data-ttu-id="9d490-370">Pro aplikace, které cílí ASP.NET Core 1.1.0 nebo vyšší, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) balíček zprostředkovatele můžete implementovat trasování událostí.</span><span class="sxs-lookup"><span data-stu-id="9d490-370">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="9d490-371">Na Windows, používá [trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="9d490-371">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="9d490-372">Zprostředkovatel je multiplatformní, ale nejsou žádná událost kolekce a zobrazení nástroje ještě pro Linux nebo macOS.</span><span class="sxs-lookup"><span data-stu-id="9d490-372">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

<span data-ttu-id="9d490-373">Je dobrým způsobem, jak shromažďovat a zobrazovat protokoly použít [nástroje PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="9d490-373">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="9d490-374">Existují jiné nástroje pro prohlížení protokolů trasování událostí pro Windows, ale PerfView poskytuje nejlepší prostředí pro práci s událostmi trasování událostí pro Windows, protože ho vygeneroval technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9d490-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="9d490-375">Ke konfiguraci PerfView pro shromažďování události zapsané podle tohoto zprostředkovatele, přidejte řetězec `*Microsoft-Extensions-Logging` k **dalších poskytovatelů** seznamu.</span><span class="sxs-lookup"><span data-stu-id="9d490-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="9d490-376">(Nenechte si ujít hvězdičku na začátku řetězce.)</span><span class="sxs-lookup"><span data-stu-id="9d490-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Další zprostředkovatelé Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="9d490-378">Poskytovatel protokolu událostí Windows</span><span class="sxs-lookup"><span data-stu-id="9d490-378">Windows EventLog provider</span></span>

<span data-ttu-id="9d490-379">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) balíček zprostředkovatele odesílá výstup protokolu do protokolu událostí Windows.</span><span class="sxs-lookup"><span data-stu-id="9d490-379">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="9d490-380">[Přetížení AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) umožňují předáním `EventLogSettings` nebo minimální úroveň protokolování.</span><span class="sxs-lookup"><span data-stu-id="9d490-380">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="9d490-381">TraceSource poskytovatele</span><span class="sxs-lookup"><span data-stu-id="9d490-381">TraceSource provider</span></span>

<span data-ttu-id="9d490-382">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) používá balíček zprostředkovatele [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) knihovny a poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="9d490-382">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="9d490-383">[Přetížení AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) umožňují předáním přepínač zdroje a naslouchací proces trasování.</span><span class="sxs-lookup"><span data-stu-id="9d490-383">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="9d490-384">K používání tohoto poskytovatele, má aplikace spouštět rozhraní .NET Framework (spíše než .NET Core).</span><span class="sxs-lookup"><span data-stu-id="9d490-384">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="9d490-385">Zprostředkovatel umožňuje směrovat zprávy širokou škálu [naslouchacích procesů](/dotnet/framework/debug-trace-profile/trace-listeners), například [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) použít v ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d490-385">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="9d490-386">Následující příklad nastaví `TraceSource` zprostředkovatele, který zaznamenává `Warning` a vyšší zprávu do okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="9d490-386">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="9d490-387">Zprostředkovatel služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9d490-387">Azure App Service provider</span></span>

<span data-ttu-id="9d490-388">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) balíček zprostředkovatele zapisuje protokoly do textových souborů v systému souborů aplikace služby Azure App Service a na [úložiště objektů blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) v účtu služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9d490-388">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="9d490-389">Zprostředkovatel je k dispozici pouze u aplikací určených pro ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9d490-389">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d490-390">Pokud cílí na .NET Core, nemusíte instalovat balíček zprostředkovatele nebo explicitně volat [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="9d490-390">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="9d490-391">Zprostředkovatel je automaticky dostupný pro aplikace, při nasazení aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9d490-391">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="9d490-392">Pokud se zaměřujete na rozhraní .NET Framework, přidejte do projektu balíček zprostředkovatele a vyvolání `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="9d490-392">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="9d490-393">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) přetížení vám umožní předat v [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) pomocí kterého můžete přepsat výchozí nastavení, jako je například protokolování výstupu šablony, názvu objektu blob a souboru omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="9d490-393">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="9d490-394">(*Výstupu šablony* je zpráv šablonu, která se použije pro všechny protokoly nad ten, který zadáte při volání `ILogger` metoda.)</span><span class="sxs-lookup"><span data-stu-id="9d490-394">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

::: moniker-end

<span data-ttu-id="9d490-395">Když nasadíte aplikaci služby App Service, aplikace respektuje nastavení v [diagnostické protokoly](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) část **služby App Service** stránky na webu Azure portal.</span><span class="sxs-lookup"><span data-stu-id="9d490-395">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="9d490-396">Když tato nastavení jsou aktualizovány, změny se projeví okamžitě bez nutnosti restartování nebo opětovné nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d490-396">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Nastavení protokolování Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="9d490-398">Výchozím umístěním pro soubory protokolů je *D:\\domácí\\LogFiles\\aplikace* složky a výchozí název souboru je *diagnostiky yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="9d490-398">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="9d490-399">Výchozí limit velikosti souboru je 10 MB a výchozí maximální počet souborů, které uchovávají se 2.</span><span class="sxs-lookup"><span data-stu-id="9d490-399">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="9d490-400">Výchozí název objektu blob je *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="9d490-400">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="9d490-401">Další informace o výchozím chování najdete v tématu [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="9d490-401">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="9d490-402">Zprostředkovatel funguje pouze v případě projektu běží v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="9d490-402">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="9d490-403">Nemá žádný vliv, pokud projekt je spuštěn místně&mdash;nelze zapsat do místních souborů nebo místním vývojovým úložištěm objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="9d490-403">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="9d490-404">Zprostředkovatele přihlášení třetí strany</span><span class="sxs-lookup"><span data-stu-id="9d490-404">Third-party logging providers</span></span>

<span data-ttu-id="9d490-405">Rozhraní protokolování třetích stran, které pracují s ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9d490-405">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="9d490-406">[elmah.IO](https://elmah.io/) ([úložiště GitHub se vzorovými](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="9d490-406">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="9d490-407">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([úložiště GitHub se vzorovými](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="9d490-407">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="9d490-408">[JSNLog](http://jsnlog.com/) ([úložiště GitHub se vzorovými](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="9d490-408">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="9d490-409">[Loggr](http://loggr.net/) ([úložiště GitHub se vzorovými](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="9d490-409">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="9d490-410">[NLog](http://nlog-project.org/) ([úložiště GitHub se vzorovými](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="9d490-410">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="9d490-411">[Serilog](https://serilog.net/) ([úložiště GitHub se vzorovými](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="9d490-411">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="9d490-412">Můžete provádět některé rozhraní třetích stran [sémantického protokolování, označovaného také jako strukturované protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9d490-412">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="9d490-413">Použití rozhraní třetích stran je podobný pomocí jedné z předdefinovaných poskytovatelů:</span><span class="sxs-lookup"><span data-stu-id="9d490-413">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="9d490-414">Přidání balíčku NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="9d490-414">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="9d490-415">Volání metody rozšíření na `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="9d490-415">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="9d490-416">Další informace najdete v dokumentaci každého rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9d490-416">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="9d490-417">Streamování protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="9d490-417">Azure log streaming</span></span>

<span data-ttu-id="9d490-418">Streamování protokolů Azure umožňuje zobrazení protokolu aktivit v reálném čase:</span><span class="sxs-lookup"><span data-stu-id="9d490-418">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="9d490-419">Aplikační server</span><span class="sxs-lookup"><span data-stu-id="9d490-419">The application server</span></span>
* <span data-ttu-id="9d490-420">Webový server</span><span class="sxs-lookup"><span data-stu-id="9d490-420">The web server</span></span>
* <span data-ttu-id="9d490-421">Trasování neúspěšných žádostí</span><span class="sxs-lookup"><span data-stu-id="9d490-421">Failed request tracing</span></span>

<span data-ttu-id="9d490-422">Postup konfigurace, streamování protokolů Azure:</span><span class="sxs-lookup"><span data-stu-id="9d490-422">To configure Azure log streaming:</span></span>

* <span data-ttu-id="9d490-423">Přejděte **diagnostické protokoly** stránky na stránce portálu vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="9d490-423">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="9d490-424">Nastavte **protokolování aplikace (systém souborů)** zapnete.</span><span class="sxs-lookup"><span data-stu-id="9d490-424">Set **Application Logging (Filesystem)** to on.</span></span>

![Stránka portálu diagnostické protokoly Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="9d490-426">Přejděte **streamování protokolů** stránku, abyste zobrazili zprávy aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d490-426">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="9d490-427">Jsou aplikace prostřednictvím přihlášení `ILogger` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9d490-427">They're logged by application through the `ILogger` interface.</span></span>

![Streamování protokolů Azure aplikace na portálu.](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="9d490-429">Protokolování trasování programu Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="9d490-429">Azure Application Insights trace logging</span></span>

<span data-ttu-id="9d490-430">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK je schopen shromažďování trasování telemetrických dat z protokolů generovaných prostřednictvím protokolování infrastruktury ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d490-430">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="9d490-431">Další informace najdete v tématu [Microsoft/ApplicationInsights-aspnetcore Wiki: protokolování](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="9d490-431">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d490-432">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9d490-432">Additional resources</span></span>

[<span data-ttu-id="9d490-433">Vysoce výkonné protokolování pomocí LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="9d490-433">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
