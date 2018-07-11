---
title: Hostitele ASP.NET Core ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: e9e10b0bc99b2c54bf342121b1a454be5dac66c6
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938194"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ffeca-103">Hostitele ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="ffeca-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ffeca-104">Podle [Luke Latham](https://github.com/guardrex) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ffeca-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ffeca-105">Aplikace ASP.NET Core je možné hostovat na Windows bez použití služby IIS jako [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="ffeca-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="ffeca-106">Pokud hostovaný jako služba Windows, aplikace může automaticky spuštění po restartování a dojde k chybě, které nevyžaduje zásah člověka.</span><span class="sxs-lookup"><span data-stu-id="ffeca-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="ffeca-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ffeca-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="ffeca-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="ffeca-108">Get started</span></span>

<span data-ttu-id="ffeca-109">K nastavení existujícího projektu ASP.NET Core pro spouštění ve službě jsou nezbytné následující minimální změny:</span><span class="sxs-lookup"><span data-stu-id="ffeca-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="ffeca-110">V souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="ffeca-110">In the project file:</span></span>

   1. <span data-ttu-id="ffeca-111">Ověřte existenci identifikátor modulu runtime nebo ho přidat do  **\<PropertyGroup >** , který obsahuje Cílová architektura:</span><span class="sxs-lookup"><span data-stu-id="ffeca-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="ffeca-112">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="ffeca-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="ffeca-113">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="ffeca-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="ffeca-114">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="ffeca-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="ffeca-115">Volání [UseContentRoot](xref:fundamentals/host/web-host#content-root) a cesta k aplikaci pro publikování umístění, namísto použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="ffeca-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="ffeca-116">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffeca-116">Publish the app.</span></span> <span data-ttu-id="ffeca-117">Použití [dotnet publikovat](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování pro Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="ffeca-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="ffeca-118">Chcete-li publikovat ukázkovou aplikaci z příkazového řádku, spusťte následující příkaz v okně konzoly ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="ffeca-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="ffeca-119">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku vytvořte službu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="ffeca-120">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="ffeca-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="ffeca-121">**Mezera mezi znaménko rovná se a znak uvozovek na začátek cesty je povinný.**</span><span class="sxs-lookup"><span data-stu-id="ffeca-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="ffeca-122">Služby se publikují do složky projektu, použijte cestu k *publikovat* složku pro vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="ffeca-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="ffeca-123">Služba je v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ffeca-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="ffeca-124">S názvem **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="ffeca-124">Named **MyService**.</span></span>
   * <span data-ttu-id="ffeca-125">Publikování *c:\\my_services\\AspNetCoreService\\bin\\vydání\\&lt;TARGET_FRAMEWORK&gt;\\publikovat* složky.</span><span class="sxs-lookup"><span data-stu-id="ffeca-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="ffeca-126">Reprezentována spustitelné aplikaci s názvem *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="ffeca-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="ffeca-127">Otevřete příkazové okno s oprávněními správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ffeca-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\<TARGET_FRAMEWORK>\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="ffeca-128">Ujistěte se, že je k dispozici mezi prostor `binPath=` argument a její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="ffeca-129">Publikovat a spustit službu z jiné složky:</span><span class="sxs-lookup"><span data-stu-id="ffeca-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="ffeca-130">Použití [– výstupní &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) možnost `dotnet publish` příkazu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="ffeca-131">Vytvoření služby s `sc.exe` příkazu cesta k výstupní složce.</span><span class="sxs-lookup"><span data-stu-id="ffeca-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="ffeca-132">Zahrnout název spustitelného souboru služby na cestě k dispozici na `binPath`.</span><span class="sxs-lookup"><span data-stu-id="ffeca-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="ffeca-133">Spusťte službu pomocí `sc start <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="ffeca-134">Spustit službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ffeca-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="ffeca-135">Příkaz trvá několik sekund se spustit službu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="ffeca-136">`sc query <SERVICE_NAME>` Příkaz je možné zkontrolovat stav služby k určení stavu:</span><span class="sxs-lookup"><span data-stu-id="ffeca-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="ffeca-137">Použijte následující příkaz a zkontrolujte stav služby app service vzorku:</span><span class="sxs-lookup"><span data-stu-id="ffeca-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="ffeca-138">Pokud je služba v `RUNNING` stavu a pokud je služba webové aplikace, procházet aplikace, její cesta (ve výchozím nastavení, `http://localhost:5000`, který přesměruje `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="ffeca-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="ffeca-139">Aplikační služba ukázkového procházet aplikace na adrese `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ffeca-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="ffeca-140">Zastavit službu s `sc stop <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="ffeca-141">Následující příkaz zastaví aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="ffeca-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="ffeca-142">Po krátké prodlevě zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="ffeca-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="ffeca-143">Postup kontroly stavu aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="ffeca-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="ffeca-144">Pokud je aplikační služba ukázkového v `STOPPED` stavu, použijte následující příkaz pro odinstalaci aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="ffeca-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="ffeca-145">Poskytují způsob, jak běží mimo službu</span><span class="sxs-lookup"><span data-stu-id="ffeca-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="ffeca-146">Usnadňuje testování a ladění, když se provozují mimo službu, takže je obvyklý přidáte kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="ffeca-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="ffeca-147">Například aplikace může běžet jako aplikace konzoly v jazyce `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="ffeca-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="ffeca-148">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínač byl předtím, než jsou předány argumenty [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="ffeca-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="ffeca-149">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="ffeca-149">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="ffeca-150">Zpracování, spouštění a zastavování události</span><span class="sxs-lookup"><span data-stu-id="ffeca-150">Handle stopping and starting events</span></span>

<span data-ttu-id="ffeca-151">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="ffeca-151">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="ffeca-152">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="ffeca-152">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="ffeca-153">Vytvořit metodu rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , který předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="ffeca-153">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ffeca-154">V `Program.Main`, zavolat novou metodu rozšíření `RunAsCustomService`, namísto [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="ffeca-154">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="ffeca-155">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="ffeca-155">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="ffeca-156">Pokud vlastní `WebHostService` kód vyžaduje službu z injektáž závislostí (jako je například protokolování), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ffeca-156">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ffeca-157">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="ffeca-157">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ffeca-158">Služby, které interakci s žádostí z Internetu nebo podnikové síti a jsou za proxy nebo nástroj pro vyrovnávání zatížení může vyžadovat dodatečnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ffeca-158">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ffeca-159">Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ffeca-159">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="ffeca-160">Konfigurace koncového bodu kestrel</span><span class="sxs-lookup"><span data-stu-id="ffeca-160">Kestrel endpoint configuration</span></span>

<span data-ttu-id="ffeca-161">Informace týkající se konfigurace koncového bodu Kestrel, včetně konfigurace protokolu HTTPS a podpory SNI, naleznete v tématu [konfigurace koncového bodu Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="ffeca-161">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
