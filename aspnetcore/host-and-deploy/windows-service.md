---
title: Hostitele ASP.NET Core ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: bce09a500160f0bf13926786d277f8b1e88c1bf8
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894254"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="700b7-103">Hostitele ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="700b7-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="700b7-104">Podle [Luke Latham](https://github.com/guardrex) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="700b7-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="700b7-105">Aplikace ASP.NET Core je možné hostovat na Windows bez použití služby IIS jako [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="700b7-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="700b7-106">Pokud hostovaný jako služba Windows, aplikace může automaticky spuštění po restartování a dojde k chybě, které nevyžaduje zásah člověka.</span><span class="sxs-lookup"><span data-stu-id="700b7-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="700b7-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="700b7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="700b7-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="700b7-108">Get started</span></span>

<span data-ttu-id="700b7-109">K nastavení existujícího projektu ASP.NET Core pro spouštění ve službě jsou nezbytné následující minimální změny:</span><span class="sxs-lookup"><span data-stu-id="700b7-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="700b7-110">V souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="700b7-110">In the project file:</span></span>

   1. <span data-ttu-id="700b7-111">Ověřte existenci identifikátor modulu runtime nebo ho přidat do  **\<PropertyGroup >** , který obsahuje Cílová architektura:</span><span class="sxs-lookup"><span data-stu-id="700b7-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="700b7-112">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="700b7-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="700b7-113">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="700b7-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="700b7-114">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="700b7-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="700b7-115">Volání [UseContentRoot](xref:fundamentals/host/web-host#content-root) a cesta k aplikaci pro publikování umístění, namísto použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="700b7-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="700b7-116">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="700b7-116">Publish the app.</span></span> <span data-ttu-id="700b7-117">Použití [dotnet publikovat](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování pro Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="700b7-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="700b7-118">Chcete-li publikovat ukázkovou aplikaci z příkazového řádku, spusťte následující příkaz v okně konzoly ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="700b7-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="700b7-119">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku vytvořte službu.</span><span class="sxs-lookup"><span data-stu-id="700b7-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="700b7-120">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="700b7-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="700b7-121">**Mezera mezi znaménko rovná se a znak uvozovek na začátek cesty je povinný.**</span><span class="sxs-lookup"><span data-stu-id="700b7-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="700b7-122">Služby se publikují do složky projektu, použijte cestu k *publikovat* složku pro vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="700b7-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="700b7-123">Služba je v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="700b7-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="700b7-124">S názvem **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="700b7-124">Named **MyService**.</span></span>
   * <span data-ttu-id="700b7-125">Publikování *c:\\my_services\\AspNetCoreService\\bin\\vydání\\&lt;TARGET_FRAMEWORK&gt;\\publikovat* složky.</span><span class="sxs-lookup"><span data-stu-id="700b7-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="700b7-126">Reprezentována spustitelné aplikaci s názvem *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="700b7-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="700b7-127">Otevřete příkazové okno s oprávněními správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="700b7-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="700b7-128">Ujistěte se, že je k dispozici mezi prostor `binPath=` argument a její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="700b7-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="700b7-129">Publikovat a spustit službu z jiné složky:</span><span class="sxs-lookup"><span data-stu-id="700b7-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="700b7-130">Použití [– výstupní &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) možnost `dotnet publish` příkazu.</span><span class="sxs-lookup"><span data-stu-id="700b7-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="700b7-131">Vytvoření služby s `sc.exe` příkazu cesta k výstupní složce.</span><span class="sxs-lookup"><span data-stu-id="700b7-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="700b7-132">Zahrnout název spustitelného souboru služby na cestě k dispozici na `binPath`.</span><span class="sxs-lookup"><span data-stu-id="700b7-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="700b7-133">Spusťte službu pomocí `sc start <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="700b7-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="700b7-134">Spustit službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="700b7-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="700b7-135">Příkaz trvá několik sekund se spustit službu.</span><span class="sxs-lookup"><span data-stu-id="700b7-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="700b7-136">`sc query <SERVICE_NAME>` Příkaz je možné zkontrolovat stav služby k určení stavu:</span><span class="sxs-lookup"><span data-stu-id="700b7-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="700b7-137">Použijte následující příkaz a zkontrolujte stav služby app service vzorku:</span><span class="sxs-lookup"><span data-stu-id="700b7-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="700b7-138">Pokud je služba v `RUNNING` stavu a pokud je služba webové aplikace, procházet aplikace, její cesta (ve výchozím nastavení, `http://localhost:5000`, který přesměruje `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="700b7-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="700b7-139">Aplikační služba ukázkového procházet aplikace na adrese `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="700b7-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="700b7-140">Zastavit službu s `sc stop <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="700b7-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="700b7-141">Následující příkaz zastaví aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="700b7-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="700b7-142">Po krátké prodlevě zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="700b7-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="700b7-143">Postup kontroly stavu aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="700b7-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="700b7-144">Pokud je aplikační služba ukázkového v `STOPPED` stavu, použijte následující příkaz pro odinstalaci aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="700b7-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="700b7-145">Poskytují způsob, jak běží mimo službu</span><span class="sxs-lookup"><span data-stu-id="700b7-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="700b7-146">Usnadňuje testování a ladění, když se provozují mimo službu, takže je obvyklý přidáte kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="700b7-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="700b7-147">Například aplikace může běžet jako aplikace konzoly v jazyce `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="700b7-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="700b7-148">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínač byl předtím, než jsou předány argumenty [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="700b7-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="700b7-149">Zpracování, spouštění a zastavování události</span><span class="sxs-lookup"><span data-stu-id="700b7-149">Handle stopping and starting events</span></span>

<span data-ttu-id="700b7-150">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="700b7-150">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="700b7-151">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="700b7-151">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="700b7-152">Vytvořit metodu rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , který předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="700b7-152">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="700b7-153">V `Program.Main`, zavolat novou metodu rozšíření `RunAsCustomService`, namísto [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="700b7-153">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="700b7-154">Pokud vlastní `WebHostService` kód vyžaduje službu z injektáž závislostí (jako je například protokolování), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="700b7-154">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="700b7-155">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="700b7-155">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="700b7-156">Služby, které interakci s žádostí z Internetu nebo podnikové síti a jsou za proxy nebo nástroj pro vyrovnávání zatížení může vyžadovat dodatečnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="700b7-156">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="700b7-157">Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="700b7-157">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="700b7-158">Konfigurace koncového bodu kestrel</span><span class="sxs-lookup"><span data-stu-id="700b7-158">Kestrel endpoint configuration</span></span>

<span data-ttu-id="700b7-159">Informace týkající se konfigurace koncového bodu Kestrel, včetně konfigurace protokolu HTTPS a podpory SNI, naleznete v tématu [konfigurace koncového bodu Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="700b7-159">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
