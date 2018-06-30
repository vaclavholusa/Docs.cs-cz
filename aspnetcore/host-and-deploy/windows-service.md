---
title: Jádro ASP.NET hostitele ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 718cc83bb29c0cff323853d22c107e00616b1dd1
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126232"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="993b9-103">Jádro ASP.NET hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="993b9-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="993b9-104">Podle [Luke Latham](https://github.com/guardrex) a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="993b9-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="993b9-105">Aplikace ASP.NET Core může být hostovaný v systému Windows bez použití služby IIS jako [služba systému Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="993b9-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="993b9-106">Pokud je hostováno jako služby systému Windows, aplikace může automaticky spustit po restartování počítače a dojde k chybě bez nutnosti lidského zásahu.</span><span class="sxs-lookup"><span data-stu-id="993b9-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="993b9-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="993b9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="993b9-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="993b9-108">Get started</span></span>

<span data-ttu-id="993b9-109">Následující minimální změny jsou nezbytné pro nastavení existujícího projektu ASP.NET Core spuštění ve službě:</span><span class="sxs-lookup"><span data-stu-id="993b9-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="993b9-110">V souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="993b9-110">In the project file:</span></span>

   1. <span data-ttu-id="993b9-111">Ověřte přítomnost identifikátor runtime nebo ho přidat do  **\<PropertyGroup >** cílové rozhraní, která obsahuje:</span><span class="sxs-lookup"><span data-stu-id="993b9-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="993b9-112">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="993b9-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="993b9-113">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="993b9-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="993b9-114">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="993b9-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="993b9-115">Pokud kód volá `UseContentRoot`, cesta k aplikaci je publikována umístění Místo použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="993b9-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="993b9-116">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="993b9-116">Publish the app.</span></span> <span data-ttu-id="993b9-117">Použití [dotnet publikování](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="993b9-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="993b9-118">K publikování ukázkové aplikace z příkazového řádku, spusťte následující příkaz v okně konzoly ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="993b9-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="993b9-119">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="993b9-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="993b9-120">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="993b9-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="993b9-121">**Mezera mezi znaménkem rovnosti a uvozovky na začátku cesta je požadovaná.**</span><span class="sxs-lookup"><span data-stu-id="993b9-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="993b9-122">Pro službu publikovaná ve složce projektu, použijte cestu k *publikování* složku pro vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="993b9-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="993b9-123">Služba je v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="993b9-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="993b9-124">S názvem **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="993b9-124">Named **MyService**.</span></span>
   * <span data-ttu-id="993b9-125">Publikovaná *c:\\my_services\\AspNetCoreService\\bin\\verze\\&lt;TARGET_FRAMEWORK&gt;\\publikování* složky.</span><span class="sxs-lookup"><span data-stu-id="993b9-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="993b9-126">Reprezentována aplikaci spustitelný soubor s názvem *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="993b9-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="993b9-127">Otevřete příkazové okno s oprávněními správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="993b9-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="993b9-128">Zajistěte, aby místo nachází mezi `binPath=` argument a její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="993b9-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="993b9-129">K publikování a spuštění služby v jiné složce:</span><span class="sxs-lookup"><span data-stu-id="993b9-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="993b9-130">Použití [– výstupní &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) možnost `dotnet publish` příkaz.</span><span class="sxs-lookup"><span data-stu-id="993b9-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="993b9-131">Vytvoření služby pomocí `sc.exe` příkaz pomocí cesty ke složce výstup.</span><span class="sxs-lookup"><span data-stu-id="993b9-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="993b9-132">Zahrnout název spustitelného souboru procesu služby k zadané cestě `binPath`.</span><span class="sxs-lookup"><span data-stu-id="993b9-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="993b9-133">Spusťte službu s `sc start <SERVICE_NAME>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="993b9-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="993b9-134">Spusťte službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="993b9-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="993b9-135">Příkaz přijímá několik sekund, spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="993b9-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="993b9-136">`sc query <SERVICE_NAME>` Příkaz lze použít ke kontrole stavu služby určit její stav:</span><span class="sxs-lookup"><span data-stu-id="993b9-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="993b9-137">Použijte následující příkaz a zkontrolujte stav služby ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="993b9-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="993b9-138">Pokud služba je v `RUNNING` stav a pokud služba je webová aplikace, vyhledejte aplikaci v jeho cestu (ve výchozím nastavení, `http://localhost:5000`, který přesměruje na `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="993b9-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="993b9-139">Ukázka app service, vyhledat aplikaci v `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="993b9-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="993b9-140">Zastavte službu s `sc stop <SERVICE_NAME>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="993b9-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="993b9-141">Následující příkaz zastaví službu ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="993b9-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="993b9-142">Po chvíli trvat kdykoli zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="993b9-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="993b9-143">Zkontrolujte stav služby ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="993b9-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="993b9-144">Když služba ukázkové aplikace je v `STOPPED` stavu, odinstalujte službu ukázkové aplikace použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="993b9-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="993b9-145">Zadejte způsob, jak spustit mimo službu</span><span class="sxs-lookup"><span data-stu-id="993b9-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="993b9-146">Je snazší testování a ladění při spuštění mimo službu, takže se obvykle přidávat kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="993b9-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="993b9-147">Například můžete spouštět aplikace jako konzolové aplikace s `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="993b9-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="993b9-148">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínače se odebere před argumenty do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="993b9-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="993b9-149">Zpracování ukončení a spuštění události</span><span class="sxs-lookup"><span data-stu-id="993b9-149">Handle stopping and starting events</span></span>

<span data-ttu-id="993b9-150">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="993b9-150">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="993b9-151">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="993b9-151">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="993b9-152">Vytvoření metody rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="993b9-152">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="993b9-153">V `Program.Main`, zavolejte metodu nové rozšíření, `RunAsCustomService`, místo [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="993b9-153">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="993b9-154">Pokud vlastní `WebHostService` kódu vyžaduje službu z vkládání závislostí (například protokolovač), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="993b9-154">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="993b9-155">Proxy server a scénáře pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="993b9-155">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="993b9-156">Služby, které interakci s požadavky z Internetu nebo podnikové síti a jsou za proxy server nebo službu Vyrovnávání zatížení může vyžadovat další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="993b9-156">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="993b9-157">Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="993b9-157">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="993b9-158">Konfigurace koncového bodu kestrel</span><span class="sxs-lookup"><span data-stu-id="993b9-158">Kestrel endpoint configuration</span></span>

<span data-ttu-id="993b9-159">Informace o konfiguraci koncového bodu Kestrel, včetně konfigurace protokolu HTTPS a SNI podporu, najdete v části [konfigurace koncového bodu Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="993b9-159">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
