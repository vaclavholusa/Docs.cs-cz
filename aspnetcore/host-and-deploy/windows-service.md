---
title: Jádro ASP.NET hostitele ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 33493e1ff674cd81544a7d14a7fd758c1e68bf9a
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252344"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="befaa-103">Jádro ASP.NET hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="befaa-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="befaa-104">Podle [Luke Latham](https://github.com/guardrex) a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="befaa-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="befaa-105">Aplikace ASP.NET Core může být hostovaný v systému Windows bez použití služby IIS jako [služba systému Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="befaa-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="befaa-106">Pokud je hostováno jako služby systému Windows, aplikace může automaticky spustit po restartování počítače a dojde k chybě bez nutnosti lidského zásahu.</span><span class="sxs-lookup"><span data-stu-id="befaa-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="befaa-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="befaa-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="befaa-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="befaa-108">Get started</span></span>

<span data-ttu-id="befaa-109">Následující minimální změny jsou nezbytné pro nastavení existujícího projektu ASP.NET Core spuštění ve službě:</span><span class="sxs-lookup"><span data-stu-id="befaa-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="befaa-110">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="befaa-110">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="befaa-111">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="befaa-111">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="befaa-112">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="befaa-112">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="befaa-113">Pokud kód volá `UseContentRoot`, cesta k aplikaci je publikována umístění Místo použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="befaa-113">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="befaa-114">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="befaa-114">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="befaa-115">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="befaa-115">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="befaa-116">Publikujte aplikaci do složky.</span><span class="sxs-lookup"><span data-stu-id="befaa-116">Publish the app to a folder.</span></span> <span data-ttu-id="befaa-117">Použití [dotnet publikování](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) který publikuje do složky.</span><span class="sxs-lookup"><span data-stu-id="befaa-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="befaa-118">K publikování ukázkové aplikace z příkazového řádku, spusťte následující příkaz v okně konzoly ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="befaa-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="befaa-119">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření služby (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span><span class="sxs-lookup"><span data-stu-id="befaa-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="befaa-120">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="befaa-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="befaa-121">**Mezera mezi znaménkem rovnosti a uvozovky, která se spouští cesta je požadovaná.**</span><span class="sxs-lookup"><span data-stu-id="befaa-121">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="befaa-122">Pro ukázkovou aplikaci a příkaz, který následuje služba je:</span><span class="sxs-lookup"><span data-stu-id="befaa-122">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="befaa-123">S názvem **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="befaa-123">Named **MyService**.</span></span>
   * <span data-ttu-id="befaa-124">Publikovaná *c:\\svc* složky.</span><span class="sxs-lookup"><span data-stu-id="befaa-124">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="befaa-125">Má aplikaci spustitelný soubor s názvem *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="befaa-125">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="befaa-126">Otevřete příkazové okno s oprávněními správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="befaa-126">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="befaa-127">**Zajistěte, aby místo nachází mezi `binPath=` argument a její hodnotu.**</span><span class="sxs-lookup"><span data-stu-id="befaa-127">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="befaa-128">Spusťte službu s `sc start <SERVICE_NAME>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="befaa-128">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="befaa-129">Spusťte službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="befaa-129">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="befaa-130">Příkaz přijímá několik sekund, spusťte službu.</span><span class="sxs-lookup"><span data-stu-id="befaa-130">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="befaa-131">`sc query <SERVICE_NAME>` Příkaz lze použít ke kontrole stavu služby určit její stav:</span><span class="sxs-lookup"><span data-stu-id="befaa-131">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="befaa-132">Použijte následující příkaz a zkontrolujte stav služby ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="befaa-132">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="befaa-133">Pokud služba je v `RUNNING` stav a pokud služba je webová aplikace, vyhledejte aplikaci v jeho cestu (ve výchozím nastavení, `http://localhost:5000`, který přesměruje na `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="befaa-133">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="befaa-134">Ukázka app service, vyhledat aplikaci v `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="befaa-134">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="befaa-135">Zastavte službu s `sc stop <SERVICE_NAME>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="befaa-135">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="befaa-136">Následující příkaz zastaví službu ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="befaa-136">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="befaa-137">Po chvíli trvat kdykoli zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="befaa-137">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="befaa-138">Zkontrolujte stav služby ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="befaa-138">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="befaa-139">Když služba ukázkové aplikace je v `STOPPED` stavu, odinstalujte službu ukázkové aplikace použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="befaa-139">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="befaa-140">Zadejte způsob, jak spustit mimo službu</span><span class="sxs-lookup"><span data-stu-id="befaa-140">Provide a way to run outside of a service</span></span>

<span data-ttu-id="befaa-141">Je snazší testování a ladění při spuštění mimo službu, takže se obvykle přidávat kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="befaa-141">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="befaa-142">Například můžete spouštět aplikace jako konzolové aplikace s `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="befaa-142">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="befaa-143">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="befaa-143">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="befaa-144">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínače se odebere před argumenty do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="befaa-144">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="befaa-145">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="befaa-145">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="befaa-146">Zpracování ukončení a spuštění události</span><span class="sxs-lookup"><span data-stu-id="befaa-146">Handle stopping and starting events</span></span>

<span data-ttu-id="befaa-147">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="befaa-147">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="befaa-148">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="befaa-148">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="befaa-149">Vytvoření metody rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="befaa-149">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="befaa-150">V `Program.Main`, zavolejte metodu nové rozšíření, `RunAsCustomService`, místo [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="befaa-150">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="befaa-151">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="befaa-151">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="befaa-152">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="befaa-152">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="befaa-153">Pokud vlastní `WebHostService` kódu vyžaduje službu z vkládání závislostí (například protokolovač), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="befaa-153">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="befaa-154">Proxy server a scénáře pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="befaa-154">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="befaa-155">Služby, které interakci s požadavky z Internetu nebo podnikové síti a jsou za proxy server nebo službu Vyrovnávání zatížení může vyžadovat další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="befaa-155">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="befaa-156">Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="befaa-156">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="befaa-157">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="befaa-157">Acknowledgments</span></span>

<span data-ttu-id="befaa-158">Tento článek byl napsán pomocí publikovaných zdrojů:</span><span class="sxs-lookup"><span data-stu-id="befaa-158">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="befaa-159">Hostování ASP.NET Core jako služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="befaa-159">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="befaa-160">Jak hostovat vaše základní technologie ASP.NET ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="befaa-160">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
