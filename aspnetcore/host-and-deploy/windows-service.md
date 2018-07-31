---
title: Hostitele ASP.NET Core ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4aded0b87ca14a5c09844cc378efb1ac0c12a289
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342153"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="a48ae-103">Hostitele ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="a48ae-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="a48ae-104">Podle [Luke Latham](https://github.com/guardrex) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a48ae-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a48ae-105">Aplikace ASP.NET Core je možné hostovat na Windows bez použití služby IIS jako [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="a48ae-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="a48ae-106">Pokud hostovaný jako služba Windows, aplikace může automaticky spuštění po restartování a dojde k chybě, které nevyžaduje zásah člověka.</span><span class="sxs-lookup"><span data-stu-id="a48ae-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="a48ae-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a48ae-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="a48ae-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a48ae-108">Get started</span></span>

<span data-ttu-id="a48ae-109">K nastavení existujícího projektu ASP.NET Core pro spouštění ve službě jsou nezbytné následující minimální změny:</span><span class="sxs-lookup"><span data-stu-id="a48ae-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="a48ae-110">V souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="a48ae-110">In the project file:</span></span>

   1. <span data-ttu-id="a48ae-111">Ověřte existenci identifikátor modulu runtime nebo ho přidat do  **\<PropertyGroup >** , který obsahuje Cílová architektura:</span><span class="sxs-lookup"><span data-stu-id="a48ae-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

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

   1. <span data-ttu-id="a48ae-112">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="a48ae-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="a48ae-113">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a48ae-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="a48ae-114">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="a48ae-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="a48ae-115">Volání [UseContentRoot](xref:fundamentals/host/web-host#content-root) a cesta k aplikaci pro publikování umístění, namísto použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="a48ae-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="a48ae-116">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="a48ae-116">Publish the app.</span></span> <span data-ttu-id="a48ae-117">Použití [dotnet publikovat](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování pro Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="a48ae-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="a48ae-118">Při používání sady Visual Studio, vyberte **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="a48ae-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="a48ae-119">Chcete-li publikovat ukázkovou aplikaci z příkazového řádku, spusťte následující příkaz v okně konzoly ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="a48ae-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="a48ae-120">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku vytvořte službu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="a48ae-121">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="a48ae-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="a48ae-122">**Mezera mezi znaménko rovná se a znak uvozovek na začátek cesty je povinný.**</span><span class="sxs-lookup"><span data-stu-id="a48ae-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="a48ae-123">Služby se publikují do složky projektu, použijte cestu k *publikovat* složku pro vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="a48ae-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="a48ae-124">V následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="a48ae-124">In the following example:</span></span>

   * <span data-ttu-id="a48ae-125">Projekt se nachází v `c:\my_services\AspNetCoreService` složky.</span><span class="sxs-lookup"><span data-stu-id="a48ae-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="a48ae-126">Projekt, se publikují v `Release` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a48ae-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="a48ae-127">Moniker cílového rozhraní (TFM) je `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="a48ae-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="a48ae-128">Identifikátor modulu Runtime (RID) je `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="a48ae-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="a48ae-129">Je název spustitelné aplikace *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="a48ae-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="a48ae-130">Služba má název **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="a48ae-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="a48ae-131">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a48ae-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="a48ae-132">Ujistěte se, že je k dispozici mezi prostor `binPath=` argument a její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="a48ae-133">Publikovat a spustit službu z jiné složky:</span><span class="sxs-lookup"><span data-stu-id="a48ae-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="a48ae-134">Použití [– výstupní &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) možnost `dotnet publish` příkazu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="a48ae-135">Pokud používáte Visual Studio, nakonfigurujte **cílové umístění** v **FolderProfile** publikovat stránku vlastností před výběrem **publikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a48ae-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="a48ae-136">Vytvoření služby s `sc.exe` příkazu cesta k výstupní složce.</span><span class="sxs-lookup"><span data-stu-id="a48ae-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="a48ae-137">Zahrnout název spustitelného souboru služby na cestě k dispozici na `binPath`.</span><span class="sxs-lookup"><span data-stu-id="a48ae-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="a48ae-138">Spusťte službu pomocí `sc start <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="a48ae-139">Spustit službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a48ae-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="a48ae-140">Příkaz trvá několik sekund se spustit službu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="a48ae-141">`sc query <SERVICE_NAME>` Příkaz je možné zkontrolovat stav služby k určení stavu:</span><span class="sxs-lookup"><span data-stu-id="a48ae-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="a48ae-142">Použijte následující příkaz a zkontrolujte stav služby app service vzorku:</span><span class="sxs-lookup"><span data-stu-id="a48ae-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="a48ae-143">Pokud je služba v `RUNNING` stavu a pokud je služba webové aplikace, procházet aplikace, její cesta (ve výchozím nastavení, `http://localhost:5000`, který přesměruje `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="a48ae-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="a48ae-144">Aplikační služba ukázkového procházet aplikace na adrese `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a48ae-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="a48ae-145">Zastavit službu s `sc stop <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="a48ae-146">Následující příkaz zastaví aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="a48ae-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="a48ae-147">Po krátké prodlevě zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="a48ae-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="a48ae-148">Postup kontroly stavu aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="a48ae-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="a48ae-149">Pokud je aplikační služba ukázkového v `STOPPED` stavu, použijte následující příkaz pro odinstalaci aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="a48ae-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="a48ae-150">Poskytují způsob, jak běží mimo službu</span><span class="sxs-lookup"><span data-stu-id="a48ae-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="a48ae-151">Usnadňuje testování a ladění, když se provozují mimo službu, takže je obvyklý přidáte kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="a48ae-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="a48ae-152">Například aplikace může běžet jako aplikace konzoly v jazyce `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="a48ae-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="a48ae-153">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínač byl předtím, než jsou předány argumenty [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="a48ae-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="a48ae-154">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="a48ae-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="a48ae-155">Zpracování, spouštění a zastavování události</span><span class="sxs-lookup"><span data-stu-id="a48ae-155">Handle stopping and starting events</span></span>

<span data-ttu-id="a48ae-156">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="a48ae-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="a48ae-157">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="a48ae-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="a48ae-158">Vytvořit metodu rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , který předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="a48ae-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="a48ae-159">V `Program.Main`, zavolat novou metodu rozšíření `RunAsCustomService`, namísto [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="a48ae-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="a48ae-160">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="a48ae-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="a48ae-161">Pokud vlastní `WebHostService` kód vyžaduje službu z injektáž závislostí (jako je například protokolování), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="a48ae-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="a48ae-162">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="a48ae-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="a48ae-163">Služby, které interakci s žádostí z Internetu nebo podnikové síti a jsou za proxy nebo nástroj pro vyrovnávání zatížení může vyžadovat dodatečnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a48ae-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="a48ae-164">Další informace naleznete v tématu <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="a48ae-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a48ae-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a48ae-165">Additional resources</span></span>

* <span data-ttu-id="a48ae-166">[Konfigurace koncového bodu kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (včetně konfigurace protokolu HTTPS a podporu SNI)</span><span class="sxs-lookup"><span data-stu-id="a48ae-166">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
