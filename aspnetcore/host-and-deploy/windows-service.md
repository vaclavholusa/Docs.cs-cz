---
title: Jádro ASP.NET hostitele ve službě Windows
author: tdykstra
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="f8686-103">Jádro ASP.NET hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="f8686-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="f8686-104">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f8686-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f8686-105">Doporučeným způsobem, jak hostovat aplikace ASP.NET Core v systému Windows bez použití služby IIS je chcete-li používat [služba systému Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="f8686-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="f8686-106">Pokud je hostováno jako služby systému Windows, aplikace může automaticky spustit po restartování počítače a dojde k chybě bez nutnosti lidského zásahu.</span><span class="sxs-lookup"><span data-stu-id="f8686-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="f8686-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f8686-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f8686-108">Pokyny o tom, jak spuštění ukázkové aplikace, naleznete v části tohoto příkladu *README.md* souboru.</span><span class="sxs-lookup"><span data-stu-id="f8686-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8686-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8686-109">Prerequisites</span></span>

* <span data-ttu-id="f8686-110">Aplikace musíte spustit na modul runtime rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f8686-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="f8686-111">V *.csproj* souboru, zadejte odpovídající hodnoty pro [TargetFramework](/nuget/schema/target-frameworks) a [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="f8686-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="f8686-112">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="f8686-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="f8686-113">Při vytváření projektu v sadě Visual Studio, použijte **aplikace ASP.NET Core (rozhraní .NET Framework)** šablony.</span><span class="sxs-lookup"><span data-stu-id="f8686-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="f8686-114">Pokud aplikace přijímá požadavky od Internetu (nikoli pouze z interní sítě), musí používat [HTTP.sys](xref:fundamentals/servers/httpsys) webový server (dříve označovaný jako [WebListener](xref:fundamentals/servers/weblistener) pro aplikace ASP.NET Core 1.x) namísto [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="f8686-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="f8686-115">IIS se doporučuje pro použití jako reverzní proxy server s Kestrel pro nasazení okraj.</span><span class="sxs-lookup"><span data-stu-id="f8686-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="f8686-116">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="f8686-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="f8686-117">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f8686-117">Get started</span></span>

<span data-ttu-id="f8686-118">Tato část popisuje minimální změny, které jsou nezbytné k nastavení existujícího projektu ASP.NET Core ke spuštění ve službě.</span><span class="sxs-lookup"><span data-stu-id="f8686-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="f8686-119">Nainstalujte balíček NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="f8686-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="f8686-120">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f8686-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="f8686-121">Volání `host.RunAsService` místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="f8686-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="f8686-122">Pokud kód volá `UseContentRoot`, použijte cestu k umístění pro publikování místo `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="f8686-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8686-123">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="f8686-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8686-124">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="f8686-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. <span data-ttu-id="f8686-125">Publikujte aplikaci do složky.</span><span class="sxs-lookup"><span data-stu-id="f8686-125">Publish the app to a folder.</span></span> <span data-ttu-id="f8686-126">Použití [dotnet publikování](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) který publikuje do složky.</span><span class="sxs-lookup"><span data-stu-id="f8686-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="f8686-127">Otestujte vytváření a spouštění služby.</span><span class="sxs-lookup"><span data-stu-id="f8686-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="f8686-128">Otevřete příkazové okno s oprávněním správce k použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření a spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="f8686-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="f8686-129">Pokud je služba Moje_služba, publikovány do `c:\svc`, a s názvem AspNetCoreService, jsou příkazy:</span><span class="sxs-lookup"><span data-stu-id="f8686-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="f8686-130">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="f8686-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Okno konzoly vytvořte a spusťte příklad](windows-service/_static/create-start.png)

   <span data-ttu-id="f8686-132">Když se tyto příkazy dokončit, přejděte do stejnou cestu jako při spuštění jako konzolové aplikace (ve výchozím nastavení, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="f8686-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Spuštěná ve službě](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="f8686-134">Zadejte způsob, jak spustit mimo službu</span><span class="sxs-lookup"><span data-stu-id="f8686-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="f8686-135">Je snazší testování a ladění při spuštění mimo službu, takže se obvykle přidávat kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="f8686-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="f8686-136">Například můžete spouštět aplikace jako konzolové aplikace s `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="f8686-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8686-137">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="f8686-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8686-138">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="f8686-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="f8686-139">Zpracování ukončení a spuštění události</span><span class="sxs-lookup"><span data-stu-id="f8686-139">Handle stopping and starting events</span></span>

<span data-ttu-id="f8686-140">Pro zpracování `OnStarting`, `OnStarted`, a `OnStopping` události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="f8686-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="f8686-141">Vytvořte třídu, která je odvozena z `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="f8686-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="f8686-142">Vytvoření metody rozšíření pro `IWebHost` , předá vlastní `WebHostService` k `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="f8686-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="f8686-143">V `Program.Main`, zavolejte metodu nové rozšíření, `RunAsCustomService`, místo `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="f8686-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8686-144">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="f8686-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8686-145">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="f8686-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
<span data-ttu-id="f8686-146">Pokud vlastní `WebHostService` kódu vyžaduje službu z vkládání závislostí (například protokolovač), získat ze `Services` vlastnost `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="f8686-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="f8686-147">Proxy server a scénáře pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="f8686-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="f8686-148">Služby, které interakci s požadavky z Internetu nebo podnikové síti a jsou za proxy server nebo službu Vyrovnávání zatížení může vyžadovat další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="f8686-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="f8686-149">Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="f8686-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="f8686-150">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="f8686-150">Acknowledgments</span></span>

<span data-ttu-id="f8686-151">Tento článek byl napsán pomocí publikovaných zdrojů:</span><span class="sxs-lookup"><span data-stu-id="f8686-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="f8686-152">Hostování ASP.NET Core jako služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="f8686-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="f8686-153">Jak hostovat vaše základní technologie ASP.NET ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="f8686-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
